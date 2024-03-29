# Настройка EIGRP

### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_21/images/eigrp.png)

**R16**  
Настроим секцию для EIGRP и включим EIGRP IPv6 на интерфейсах, так же включим - auto-summary  для IPv4 и настроим - summary-address для IPv6, чтобы маршрутизатор отдавал суммарные маршруты:
```
configure terminal
interface Loopback0 
 ipv6 eigrp 2042 
!
interface Ethernet0/1
 ipv6 eigrp 2042 
!
interface Ethernet0/3
 ipv6 eigrp 2042 
!
router eigrp SPB
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface Loopback0
   passive-interface
  exit-af-interface
  !
  topology base
   auto-summary
   distribute-list prefix eigrp_default out Ethernet0/3
  exit-af-topology
  network 1.1.16.16 0.0.0.0
  network 10.123.42.2 0.0.0.1
  network 10.123.42.4 0.0.0.1
  network 10.123.42.10 0.0.0.1
  network 10.123.42.12 0.0.0.1
  eigrp router-id 1.1.16.16
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface Ethernet0/1
   summary-address 2002:101::/64
   summary-address 2001:DB8:2042:2042::/64
  exit-af-interface
  !
  topology base
   distribute-list prefix-list eigrp_default out Ethernet0/3
  exit-af-topology
  eigrp router-id 1.1.16.16
 exit-address-family
!
```
Настроим два prefix-list'a для IPv4/IPv6:
```
ip prefix-list eigrp_default seq 10 permit 0.0.0.0/0
ipv6 prefix-list eigrp_default seq 10 permit ::/0
```

Посмотрим соседей и полученые маршруты IPv4/IPv6, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_21/images/R16.png)

**R17**  
Настроим секцию для EIGRP и включим EIGRP IPv6 на интерфейсах, так же включим - auto-summary для IPv4 и настроим - summary-address для IPv6, чтобы маршрутизатор отдавал суммарные маршруты:
```
configure terminal
interface Loopback0
 ipv6 eigrp 2042
!
interface Ethernet0/1
 ipv6 eigrp 2042
!
router eigrp SPB
 !
 address-family ipv4 unicast autonomous-system 2042
  !       
  af-interface Loopback0
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/3
   passive-interface
  exit-af-interface
  !
  topology base
   auto-summary
  exit-af-topology
  network 1.1.17.17 0.0.0.0
  network 10.123.42.0 0.0.0.1
  network 10.123.42.6 0.0.0.1
  network 10.123.42.8 0.0.0.1
  eigrp router-id 1.1.17.17
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface Ethernet0/1
   summary-address 2002:101::/64
   summary-address 2001:DB8:2042:2042::/64
  exit-af-interface
  !
  topology base
  exit-af-topology
  eigrp router-id 1.1.17.17
 exit-address-family
!
```
Посмотрим соседей и полученые маршруты IPv4/IPv6, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_21/images/R17.png)


**R18**  
Настроим секцию для EIGRP и включим EIGRP IPv6 на интерфейсах:
```
configure terminal
interface Loopback0
 ipv6 eigrp 2042
!
interface Ethernet0/0
 ipv6 eigrp 2042
!
interface Ethernet0/1
 ipv6 eigrp 2042
!
router eigrp SPB
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface Loopback0
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/2
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/3
   passive-interface
  exit-af-interface
  !
  topology base
   redistribute static
  exit-af-topology
  network 1.1.18.18 0.0.0.0
  network 10.123.42.0 0.0.0.1
  network 10.123.42.2 0.0.0.1
  eigrp router-id 1.1.18.18
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  topology base
   redistribute static
  exit-af-topology
  eigrp router-id 1.1.18.18
 exit-address-family
!
```
Настроим статические маршруты в сторону оператора Триада:
```
ip route 0.0.0.0 0.0.0.0 85.75.123.25 254
ip route 0.0.0.0 0.0.0.0 85.75.123.21 254
!
ipv6 route ::/0 2002:5555::14 254
ipv6 route ::/0 2002:5555::16 254
```

Посмотрим соседей и полученые маршруты IPv4/IPv6, как видим наш маршрутизатор получает суммарный маршрут- *1.0.0.0/8* от двух соседей через интерфейсы - Ethernet0/0 и Ethernet0/1, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_21/images/R18.png)

**R32**  
Настроим секцию для EIGRP и включим EIGRP IPv6 на интерфейсах:
```
configure terminal
interface Loopback0
 ipv6 eigrp 2042
!
interface Ethernet0/0
 ipv6 eigrp 2042
!
router eigrp SPB
 !
 address-family ipv4 unicast autonomous-system 2042
  !       
  af-interface Loopback0
   passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 1.1.32.32 0.0.0.0
  network 10.123.42.4 0.0.0.1
  eigrp router-id 1.1.32.32
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  topology base
  exit-af-topology
  eigrp router-id 1.1.32.32
 exit-address-family
!
```
Посмотрим соседей и полученые маршруты IPv4/IPv6, как видим наш маршрутизатор получает только default'ый маршрут, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_21/images/R32.png)

**SW9**  
Настроим секцию для EIGRP:
```
configure terminal
interface Loopback0
 ipv6 eigrp 2042
!
interface Ethernet0/3
ipv6 eigrp 2042
!
interface Ethernet1/0
ipv6 eigrp 2042
!
router eigrp 2042
 network 1.1.9.9 0.0.0.0
 network 10.123.42.6 0.0.0.1
 network 10.123.42.10 0.0.0.1
 network 192.168.242.0 0.0.0.7
 passive-interface Loopback0
 eigrp router-id 1.1.9.9
!
ipv6 router eigrp 2042
 eigrp router-id 1.1.9.9
!
```
Посмотрим соседей и полученые маршруты IPv4, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_21/images/SW9.png)

**SW10**  
Настроим секцию для EIGRP:
```
configure terminal
interface Loopback0
ipv6 eigrp 2042
!
interface Ethernet0/3
ipv6 eigrp 2042
!
interface Ethernet1/0
ipv6 eigrp 2042
!
router eigrp 2042
 network 1.1.10.10 0.0.0.0
 network 10.123.42.8 0.0.0.1
 network 10.123.42.12 0.0.0.1
 network 192.168.242.0 0.0.0.7
 passive-interface Loopback0
 eigrp router-id 1.1.10.10
!
ipv6 router eigrp 2042
 eigrp router-id 1.1.10.10
!
```
Посмотрим соседей и полученые маршруты IPv4, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_21/images/SW10.png)
