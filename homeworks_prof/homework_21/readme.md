# Настройка EIGRP

### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_21/images/eigrp.png)

   * *R16-17 анонсируют только суммарные префиксы - не совсем понятно, в нашем случае с eigrp named mode auto-summary (суммирование маршрутов не сделать), поэтому на R16 и R17 в сторону R18 настроил summary-address 0.0.0.0 0.0.0.0 на интерфейсах, что указано ниже в конфигурации.*

**R16**  
Настроим секцию для EIGRP и включим EIGRP IPv6 на интерфейсах:
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
  af-interface Ethernet0/0
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/2
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/3
   summary-address 0.0.0.0 0.0.0.0
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address 0.0.0.0 0.0.0.0
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 1.1.16.16 0.0.0.0
  network 10.123.42.2 0.0.0.1
  network 10.123.42.4 0.0.0.1
  eigrp router-id 1.1.16.16
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface Ethernet0/1
   summary-address ::/0
  exit-af-interface
  !
  af-interface Ethernet0/3
   summary-address ::/0
  exit-af-interface
  !
  topology base
  exit-af-topology
  eigrp router-id 1.1.16.16
 exit-address-family
!
```
Посмотрим соседей и полученые маршруты IPv4/IPv6, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_21/images/R16.png)

**R17**  
Настроим секцию для EIGRP и включим EIGRP IPv6 на интерфейсах:
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
  af-interface Ethernet0/0
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
  af-interface Ethernet0/1
   summary-address 0.0.0.0 0.0.0.0
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 1.1.17.17 0.0.0.0
  network 10.123.42.0 0.0.0.1
  eigrp router-id 1.1.17.17
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface Ethernet0/1
   summary-address ::/0
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
  exit-af-topology
  eigrp router-id 1.1.18.18
 exit-address-family
!
```
Посмотрим соседей и полученые маршруты IPv4/IPv6, как показано на рисунке:
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
