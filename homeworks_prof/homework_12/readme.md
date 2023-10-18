# Маршрутизация на основе политик (PBR)

### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_11/images/topo.png)

#### 1. Офис Чокурдах
  * Настроим два маршрута по-умолчанию на маршрутизаторе офиса Чокурдах в сторону провайдера Триада, но маршрут в сторону R26 будет менее приоритетным:
```
ip route 0.0.0.0 0.0.0.0 85.75.123.13 name to_R25
ip route 0.0.0.0 0.0.0.0 85.75.123.17 254 name to_R26
```

  * Настроим отслеживание линков на маршрутизаторе офиса Чокурдах, ниже на рисунке видим статистику и статус ответа, что всё *ОК*:
```
ip sla 25
 icmp-echo 85.75.123.13 source-ip 85.75.123.14
 frequency 10
ip sla schedule 25 life forever start-time now
ip sla 26
 icmp-echo 85.75.123.17 source-ip 85.75.123.18
 frequency 10
ip sla schedule 26 life forever start-time now
```
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_12/images/sla_ok.png)

  * Создадим track для отслеживания статуса линков и привяжем их к нашим маршрутам:
```
track 25 ip sla 25 reachability
 delay down 60 up 60
track 26 ip sla 26 reachability
 delay down 60 up 60
```
```
ip route 0.0.0.0 0.0.0.0 85.75.123.13 name to_R25 track 25
ip route 0.0.0.0 0.0.0.0 85.75.123.17 254 name to_R26 track 26
```

  * Погасим интерфейс на маршрутизаторе R25 провайдера Триада, ниже на рисунке увидим статистику и статус ответа:

![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_12/images/sla_nok.png)  

  * Можно увидеть как сработал наш track и маршрут по-умолчанию изменился

![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_12/images/track.png)

  * С помощью policy и route-map пустим трафик от VPC31 через 85.75.123.17, для этого создадим access-list и route-map и повесим нашу route-map на интерфейс:
```
ip access-list extended acl_vpc31
 permit ip host 192.168.50.3 any
!
route-map rm_vpc31_nh permit 10
 match ip address acl_vpc31
 set ip next-hop 85.75.123.17
!
interface Ethernet0/2.50
 description LAN
 encapsulation dot1Q 50
 ip address 192.168.50.1 255.255.255.248
 ip policy route-map rm_vpc31_nh
end
```
Для проверки на R25 и R26 прописал статический маршрут до loopback-адреса (1.1.27.27) офиса Лабытнанги и запустил trace с VPC31 и VPC30:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_12/images/trace.png)

#### 1. Офис Лабытнанги
  * Настрим маршрут по-умолчанию на маршрутизаторе офиса Лабытнанги:
```
ip route 0.0.0.0 0.0.0.0 85.75.123.9 name to_R25
```
