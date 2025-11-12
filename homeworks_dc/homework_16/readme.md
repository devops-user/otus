# Переход от 3-х уровневой топологи в ЦОД к топологии CLOS.

На данном этапе есть сеть (ЦОД остался в наследство) со стандартной 3-х уровневой топологией Core/Distribution/Access, но т.к. это в первую очередь ЦОД, то принято решение переходить на все эти современные штуки в виде EVPN/VxLAN.

На схеме видим стандартную топологию: 
* уровень доступа (L2) - это четыри стека коммутаторов, в каждом стеке по два коммутатора Huawei CE6681-48S6CQ;
* уровень распределения (L2/L3) - это два коммутатора Huawei CE8865-4C;
* уровень ядра (L3) - это два маршрутизатора Huawei NetEngine 8000 M4.
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_16/images/Three_Layer_1.jpg)

И даже с этим как-то можно жить, но к этому добавляются стыки с операторами связи, стыки с удаленными офисами. На уровне ядра/распределения имеем разные ACL (access-lists) для ограничения доступа/связности между хостами. Так же нет целевой схемы включения удаленных офисов.
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_16/images/Three_Layer_2.jpg)

В сторону уровня доступа настроен VRRP, что так же не отвечает требованиям современного ЦОД
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_16/images/vrrp1.png)
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_16/images/vrrp2.png)

В итоге хотим прийдти к топологии CLOS и получить сеть такого вида:
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_16/images/Leaf-Spine.jpg)

Работы разделим на несколько этапов:
1) Монтаж и запуск NGFW Huawei USG 6615F, настройка правил по требованиям ИБ - уходим от access-lists, переносим всё на Firewall.
2) Перенос физических стыков для удаленных офисов и Интернет-провайдеров - переносим физические линки с будущих Spines.
3) Отключение стеков - расключаем стеки, в дальнейшем каждый из стеков будут отдельными Leafs, два Leafs будут в одной автономной системе (по аналогии два коммутатора в одном стеке).
4) Настраиваем EVPN/VxLAN - смотрим, что все туннели поднялись между будущими Leaf's
5) Переносим все sub/vlanIf-интерфейсы на Leaf's.

На выходе получаем чистую фабрику без лишних стыков на уровне Spine, весь L3 у нас будет жить на уровне Leaf.

В качестве Firewall's используем два Huawei USG 6615F, в качестве Primary/Secondary, которые так же будут "шарить" нагрузку между собой.
Настроим их, между собой Firewalls будут общаться по проприетарному протоколу HRP (High-availability Redundancy Protocol), конфигурация одинаковая на обоих Firewalls:
```
#
hrp enable
hrp preempt delay 600
hrp auto-sync config static-route
hrp auto-sync config policy-based-route
hrp mirror session enable
hrp interface <YOU_INTERFACE> remote <IP_ADDRESS>
hrp authentication-key <YOU_KEY>
hrp escape enable
#
```

Посмотрим статусы на USG 6615F и увидим, что у одного роль - **Primary** и в имени хоста появилась надпись - **HRP_M**, второй имеет роль - **Secondary** и в имени хоста появилась надпись - **HRP_S**:
```
HRP_M<HUAWEI-FW1>display hrp state 
2025-11-12 16:33:34.692 +03:00
 Local role: Load Sharing (Primary), Peer role: Load Sharing (Secondary)
 Local priority: 0:2, Peer priority: 0:2
 Connection state: succeeded
 Stable time: 50 days, 23 hours, 27 minutes
 Last state change information: 2025-09-22 17:07:02 The HRP core state changed due to "Peer priority changed". (old_state=active, new_state=load sharing (primary), local_fault_indicator=0, local_robustness_indicator=2, peer_fault_indicator=0, peer_robustness_indicator=2)
```
```
HRP_S<HUAWEI-FW2>display hrp state
2025-11-12 16:35:15.160 +03:00
 Local role: Load Sharing (Secondary), Peer role: Load Sharing (Primary)
 Local priority: 0:2, Peer priority: 0:2
 Connection state: succeeded
 Stable time: 50 days, 23 hours, 29 minutes
 Last state change information: 2025-09-22 17:07:02 The HRP core state changed due to "Local priority changed". (old_state=standby, new_state=load sharing (secondary), local_fault_indicator=0, local_robustness_indicator=2, peer_fault_indicator=0, peer_robustness_indicator=2)
```

Firewalls будут общаться с нашими BorderLeafs по протоколу eBGP:
```
#
bgp 64532
 router-id 11.11.11.11
 private-4-byte-as enable
 peer <IP_ADDRESS_PEER> as-number 64530
 peer <IP_ADDRESS_PEER> connect-interface <YOU_INTERFACE>
 peer <IP_ADDRESS_PEER> bfd min-tx-interval 150 min-rx-interval 150
 peer <IP_ADDRESS_PEER> bfd enable
 peer <IP_ADDRESS_PEER> as-number 64530
 peer <IP_ADDRESS_PEER> connect-interface <YOU_INTERFACE>
 peer <IP_ADDRESS_PEER> bfd min-tx-interval 150 min-rx-interval 150
 peer <IP_ADDRESS_PEER> bfd enable
#
 #
 ipv4-family unicast
  maximum load-balancing ebgp 2
  peer <IP_ADDRESS_PEER> enable
  peer <IP_ADDRESS_PEER> enable
#
```

Посмотрим поднялась-ли bgp-сессия:
```
HRP_M<HUAWEI-FW1>display bgp peer   
2025-11-12 16:49:10.192 +03:00
 BGP local router ID        : 11.11.11.11
 Local AS number            : 64532
 Total number of peers      : 2
 Peers in established state : 2

  Peer                             V          AS  MsgRcvd  MsgSent  OutQ  Up/Down       State  PrefRcv
  <IP_ADDRESS_PEER>                    4       64530    36659    36154     0 0522h45m Established       13
  <IP_ADDRESS_PEER>                    4       64530    62697    61772     0 0894h08m Established       14
```
