# Архитектура сети

### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/final_project/images/topo_1.png)

### Таблица адресации оператора BUDWEISER-TELECOM
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| BUDWEISER-R1 | Fa0/0 | 10.177.123.5/30 |
|  | Fa1/0 | 172.25.123.0/31 |
|  | Lo0 | 1.1.1.1/32 |
| BUDWEISER-R2 | Fa0/0 | 10.177.123.17/30 |
|  | Fa1/0 | 172.25.123.1/31 |
|  | Fa2/0 | 10.199.24.1/25 |
|  | Lo0 | 10.123.52.4/31 |

### Таблица адресации оператора SPATEN-TELECOM
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| SPATEN-R1 | Fa0/0 | 172.16.23.0/31 |
|  | Fa2/0 | 10.177.123.1/30 |
|  | Lo0 | 8.8.8.8/32 |
| SPATEN-R2 | Fa0/0 | 172.16.23.1/31 |
|  | Fa1/0 | 10.177.123.13/30 |
|  | Fa2/0 | 10.177.123.9/30 |
|  | Lo0 | 9.9.9.9/32 |

### Таблица адресации главный офис компании KITCHENWOOD
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| KWOO-HQ_HUB-R1 | Fa0/0 | 10.177.123.6/30 |
|  | Fa2/0 | 10.177.123.2/30 |
|  | Fa2/0.199 | 10.199.199.1/24 |
|  | Fa2/0.200 | 10.199.200.1/24 |

### Таблица адресации компании KITCHENWOOD - филиал Екатеринбург
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| EKT-R1 | E0/0 | 10.177.123.18/30 |
|  | E0/1 | 10.177.123.14/30 |
|  | E0/3.30 | 10.66.30.1/25 |
|  | E0/3.40 | 10.66.40.1/25 |
|  | E0/3.99 | 10.199.66.1/25 |

Задача 2-го этапа модернизации заключается в организации резервных VPN-каналов связи до офиса и переход на динамическую маршрутизацию - BGP. 
На примере офисов Екатеринбург и Санкт-Петербург будут приведены примеры, в офисе Екатеринбург будут организованы два VPN-канала от двух разных операторов связи с возможностью резервирования, 
в офисе Санкт-Петербург будет организован один канал связи от оператора и резервный канал через Интернет, где уже поверх будет настроен GRE/IPSEC до гавного офиса и поднято iBGP.

  * Настроим BGP в филиале Екатеринбург:  
**EKT-R1**
```
router bgp 5665
 template peer-policy sp1
  route-map rm-sp1-out out
  weight 1500
  soft-reconfiguration inbound
  send-community both
 exit-peer-policy
 !
 template peer-policy sp2
  route-map rm-sp2-out out
  weight 1000
  soft-reconfiguration inbound
  send-community both
 exit-peer-policy
 !
 bgp log-neighbor-changes
 neighbor 10.177.123.13 remote-as 8997
 neighbor 10.177.123.13 description SPATEN-TELECOM
 neighbor 10.177.123.17 remote-as 25080
 neighbor 10.177.123.17 description BUDWEISER-TELECOM
 !
 address-family ipv4
  network 10.66.30.0 mask 255.255.255.128
  network 10.66.40.0 mask 255.255.255.128
  network 10.199.66.0 mask 255.255.255.128
  neighbor 10.177.123.13 activate
  neighbor 10.177.123.13 inherit peer-policy sp2
  neighbor 10.177.123.13 weight 1000
  neighbor 10.177.123.17 activate
  neighbor 10.177.123.17 inherit peer-policy sp1
  neighbor 10.177.123.17 weight 1500
  neighbor 10.177.123.17 allowas-in
 exit-address-family
ip bgp-community new-format
```

Создадим prefix-list, с подсетями, которые мы будем анонсировать из филиала в главный офис:
```
ip prefix-list pl-sp-out seq 10 permit 10.66.30.0/25
ip prefix-list pl-sp-out seq 20 permit 10.66.40.0/25
ip prefix-list pl-sp-out seq 30 permit 10.199.66.0/25
```

Создадим два route-map, по задумке, чтобы главный офис понимал, какой канал основной, а какой резервный, будем использовать два разных community, **5665:666** - основной канал, **5665:777** - резервный канал:
```
route-map rm-sp2-out permit 10
 match ip address prefix-list pl-sp-out
 set community 5665:777
!
route-map rm-sp1-out permit 10
 match ip address prefix-list pl-sp-out
 set community 5665:666
```
Посмотрим состояние BGP-сессии и что мы анонсируем двум нашим операторам связи, как видим ниже, BGP-сессии поднялись и наши префиксы анонсируются обоим операторам связи:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/final_project/images/ekt_bgp.png)

  * Теперь посмотрим состояние BGP-сессии и что мы получаем от операторов в главном офисе, как видим ниже, BGP-сессии поднялись и мы получаем наши префиксы из филиала Екаткеринбург от обоих операторов связи:

![](https://github.com/devops-user/otus/blob/main/homeworks_prof/final_project/images/kwood_bgp_1.png)

Чтобы нам понимать в главном офисе какие префиксы пришли через основной канал связи , а какие через резервный канал связи, создадим community-list ISP1 и ISP2 и route-map, как мы говорили выше, будем использовать два разных community, **5665:666** - основной канал, **5665:777** - резервный канал:
```
ip community-list standard ISP2 permit 5665:777
ip community-list standard ISP1 permit 5665:666
!
route-map PEER_1 permit 10
 match community ISP1
 set local-preference 1500
!
route-map PEER_2 permit 10
 match community ISP2
 set local-preference 1000
```

Как выглядит сама секция настроек BGP в главном офисе:  
```
router bgp 5665
 bgp log-neighbor-changes
 neighbor 10.177.123.1 remote-as 8997
 neighbor 10.177.123.1 description SPATEN-TELECOM
 neighbor 10.177.123.5 remote-as 25080
 neighbor 10.177.123.5 description BUDWEISER-TELECOM
 neighbor 10.254.0.78 remote-as 5665
 !
 address-family ipv4
  network 10.199.199.0 mask 255.255.255.0
  network 10.199.200.0 mask 255.255.255.0
  neighbor 10.177.123.1 activate
  neighbor 10.177.123.1 soft-reconfiguration inbound
  neighbor 10.177.123.1 route-map PEER_2 in
  neighbor 10.177.123.5 activate
  neighbor 10.177.123.5 soft-reconfiguration inbound
  neighbor 10.177.123.5 route-map PEER_1 in
  neighbor 10.254.0.78 activate
  neighbor 10.254.0.78 allowas-in
  neighbor 10.254.0.78 soft-reconfiguration inbound
  neighbor 10.254.0.78 route-map PEER_2 in
 exit-address-family
```
Посмотрим более детально один из префиксов филиала Екатеринбург, как видим через одного оператора приходит с community - 5665:666, а через другого с community - 5665:777, так же согласной нашей логике, мы определяем разные local-preference, чтобы работать в один момент через одного оператора связи, можно заметить, что префиксы из филиала Екатеринбург приходят от разных операторов, но лучший путь (*best*) выбирается через оператора *BUDWEISER-TELECOM - 10.177.123.5*, в случае падения одного из каналов, префиксы будут доступны через другого операторв связи:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/final_project/images/kwood_bgp_2.png)

  * Перейдем к настройкам филиала Санкт-Петербург:

**SPB-R1**
```
router bgp 5665
 template peer-policy sp1
  route-map rm-sp1-out out
  weight 1500
  soft-reconfiguration inbound
  send-community both
 exit-peer-policy
 !
 template peer-policy sp2
  route-map rm-sp2-out out
  weight 1000
  soft-reconfiguration inbound
  send-community both
 exit-peer-policy
 !
 bgp log-neighbor-changes
 neighbor 10.177.123.21 remote-as 25080
 neighbor 10.177.123.21 description BUDWEISER-TELECOM
 neighbor 10.254.0.1 remote-as 5665
 !
 address-family ipv4
  network 10.78.30.0 mask 255.255.255.128
  network 10.78.40.0 mask 255.255.255.128
  network 10.199.78.0 mask 255.255.255.128
  neighbor 10.177.123.21 activate
  neighbor 10.177.123.21 inherit peer-policy sp1
  neighbor 10.177.123.21 weight 1500
  neighbor 10.177.123.21 filter-list 99 out
  neighbor 10.254.0.1 activate
  neighbor 10.254.0.1 inherit peer-policy sp2
  neighbor 10.254.0.1 weight 1000
  neighbor 10.254.0.1 allowas-in
  neighbor 10.254.0.1 filter-list 99 out
 exit-address-family
ip bgp-community new-format
```

Здесь так же есть prefix-list с анонсируемыми подсетями филиала и два route-map с назначение наших community:
```
ip prefix-list pl-sp-out seq 10 permit 10.78.30.0/25
ip prefix-list pl-sp-out seq 20 permit 10.78.40.0/25
ip prefix-list pl-sp-out seq 30 permit 10.199.78.0/25
!
route-map rm-sp2-out permit 10
 match ip address prefix-list pl-sp-out
 set community 5665:777
!
route-map rm-sp1-out permit 10
 match ip address prefix-list pl-sp-out
 set community 5665:666
```
