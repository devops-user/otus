# Построение Underlay сети(OSPF)
### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_03/images/topology.JPG)

Настройка протокола маршрутизации OSPF для Underlay сети:

**DC1-SPINE-1**
```
!
router ospf 1
   router-id 10.0.1.0
   passive-interface Loopback0
   max-lsa 12000
   timers spf delay initial 0 0 0
!
interface Loopback0
   ip address 10.0.1.0/32
   ip ospf area 0.0.0.0
!
interface Ethernet1
   no switchport
   ip address 10.2.1.0/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet2
   no switchport
   ip address 10.2.1.2/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet3
   no switchport
   ip address 10.2.1.4/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
```

**DC1-SPINE-2**
```
!
router ospf 1
   router-id 10.0.2.0
   passive-interface Loopback0
   max-lsa 12000
   timers spf delay initial 0 0 0
!
interface Loopback0
   ip address 10.0.2.0/32
   ip ospf area 0.0.0.0
!
interface Ethernet1
   no switchport
   ip address 10.2.2.0/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet2
   no switchport
   ip address 10.2.2.2/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet3
   no switchport
   ip address 10.2.2.4/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
```

**DC1-LEAF-1A**
```
!
router ospf 1
   router-id 10.0.0.1
   passive-interface Loopback0
   max-lsa 12000
   timers spf delay initial 0 0 0
!
interface Loopback0
   ip address 10.0.0.1/32
   ip ospf area 0.0.0.0
!
interface Ethernet1
   no switchport
   ip address 10.2.1.1/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet3
   no switchport
   ip address 10.2.2.1/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
```

**DC1-LEAF-1B**
```
!
router ospf 1
   router-id 10.0.0.2
   passive-interface Loopback0
   max-lsa 12000
   timers spf delay initial 0 0 0
!
interface Loopback0
   ip address 10.0.0.2/32
   ip ospf area 0.0.0.0
!
interface Ethernet1
   no switchport
   ip address 10.2.1.3/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet3
   no switchport
   ip address 10.2.2.3/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
```

**DC1-LEAF-1C**
```
!
router ospf 1
   router-id 10.0.0.3
   passive-interface Loopback0
   max-lsa 12000
   timers spf delay initial 0 0 0
!
interface Loopback0
   ip address 10.0.0.3/32
   ip ospf area 0.0.0.0
!
interface Ethernet1
   no switchport
   ip address 10.2.1.5/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet3
   no switchport
   ip address 10.2.2.5/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
```

Проверка IP-связанности между устройствами:
