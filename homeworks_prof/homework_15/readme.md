# Настройка OSPF

### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_15/images/ospf.png)

**R14**  
Настроим секцию OSPF для IPv4/IPv6 и включим OSPF на интерфейсах:
```
configure terminal
interface Loopback0
 ip ospf 1 area 0
 ipv6 ospf 1 area 0
!
interface Ethernet0/0
 ip ospf network point-to-point
 ip ospf 1 area 0
 ipv6 ospf 1 area 0
 ipv6 ospf network point-to-point
!
interface Ethernet0/1
 ip ospf network point-to-point
 ip ospf 1 area 0
 ipv6 ospf 1 area 0
 ipv6 ospf network point-to-point
!
interface Ethernet0/3
 ip ospf network point-to-point
 ip ospf 1 area 101
 ipv6 ospf 1 area 101
 ipv6 ospf network point-to-point
!
router ospfv3 1
 router-id 1.1.14.14
 area 101 stub no-summary
!
 address-family ipv6 unicast
  passive-interface Loopback0
  default-information originate always
 exit-address-family
!
router ospf 1
 router-id 1.1.14.14
 area 101 stub no-summary
 passive-interface Loopback0
 default-information originate always
```

**R15**  
Настроим секцию OSPF для IPv4/IPv6 и включим OSPF на интерфейсах:
```
configure terminal
interface Loopback0
 ip ospf 1 area 0
 ipv6 ospf 1 area 0
!
interface Ethernet0/0
 ip ospf network point-to-point
 ip ospf 1 area 0
 ipv6 ospf 1 area 0
 ipv6 ospf network point-to-point
!
interface Ethernet0/1
 ip ospf network point-to-point
 ip ospf 1 area 0
 ipv6 ospf 1 area 0
 ipv6 ospf network point-to-point
!
interface Ethernet0/3
 ip ospf network point-to-point
 ip ospf 1 area 102
 ipv6 ospf 1 area 102
 ipv6 ospf network point-to-point
!
router ospfv3 1
 router-id 1.1.15.15
 !
 address-family ipv6 unicast
  passive-interface Loopback0
  default-information originate always
  area 102 filter-list prefix deny_area_101 in
 exit-address-family
!
router ospf 1
 router-id 1.1.15.15
 area 102 filter-list prefix deny_area_101 in
 passive-interface Loopback0
 default-information originate always
```
Настроим prefix-list'ы для IPv4/IPv6 для фильтрации префиксов из зоны 101:
```
ip prefix-list deny_area_101 seq 5 deny 10.101.101.16/31
ip prefix-list deny_area_101 seq 10 deny 1.1.19.19/32
ip prefix-list deny_area_101 seq 15 permit 0.0.0.0/0 le 32
!
ipv6 prefix-list deny_area_101 seq 5 deny 2001:DB8:1001:1001::4/127
ipv6 prefix-list deny_area_101 seq 10 deny 2002:101::19:19/128
ipv6 prefix-list deny_area_101 seq 15 permit ::/0 le 128
```

**R12**  
Настроим секцию OSPF для IPv4/IPv6 и включим OSPF на интерфейсах:
```
configure terminal
interface Loopback0
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
!
interface Ethernet0/0
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/1
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/2
 ip ospf network point-to-point
 ip ospf 1 area 0
 ipv6 ospf 1 area 0
 ipv6 ospf network point-to-point
!
interface Ethernet0/3
 ip ospf network point-to-point
 ip ospf 1 area 0
 ipv6 ospf 1 area 0
 ipv6 ospf network point-to-point
!
router ospfv3 1
 router-id 1.1.12.12
 !
 address-family ipv6 unicast
  passive-interface Loopback0
 exit-address-family
!
router ospf 1
 router-id 1.1.12.12
 passive-interface Loopback0
```

**R13**  
Настроим секцию OSPF для IPv4/IPv6 и включим OSPF на интерфейсах:
```
configure terminal
interface Loopback0
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
!
interface Ethernet0/0
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/1
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/2
 ip ospf network point-to-point
 ip ospf 1 area 0
 ipv6 ospf 1 area 0
 ipv6 ospf network point-to-point
!
interface Ethernet0/3
 ip ospf network point-to-point
 ip ospf 1 area 0
 ipv6 ospf 1 area 0
 ipv6 ospf network point-to-point
!
router ospfv3 1
 router-id 1.1.13.13
 !
 address-family ipv6 unicast
  passive-interface Loopback0
 exit-address-family
!
router ospf 1
 router-id 1.1.13.13
 passive-interface Loopback0
```

**R19**  
Настроим секцию OSPF для IPv4/IPv6 и включим OSPF на интерфейсах:
```
configure terminal
interface Loopback0
 ip ospf 1 area 101
 ipv6 ospf 1 area 101
!
interface Ethernet0/0
 ip ospf network point-to-point
 ip ospf 1 area 101
 ipv6 ospf 1 area 101
 ipv6 ospf network point-to-point
!
router ospfv3 1
 router-id 1.1.19.19
 area 101 stub no-summary
 !
 address-family ipv6 unicast
  passive-interface Loopback0
 exit-address-family
!
router ospf 1
 router-id 1.1.19.19
 area 101 stub no-summary
 passive-interface Loopback0
```

**R20**  
Настроим секцию OSPF для IPv4/IPv6 и включим OSPF на интерфейсах:
```
configure terminal
interface Loopback0
 ip ospf 1 area 102
 ipv6 ospf 1 area 102
!
interface Ethernet0/0
 ip ospf network point-to-point
 ip ospf 1 area 102
 ipv6 ospf 1 area 102
 ipv6 ospf network point-to-point
!
router ospfv3 1
 router-id 1.1.20.20
 !
 address-family ipv6 unicast
  passive-interface Loopback0
  default-information originate
 exit-address-family
!
router ospf 1
 router-id 1.1.20.20
 passive-interface Loopback0
```

**SW4**  
Настроим секцию OSPF для IPv4/IPv6 и включим OSPF на интерфейсах:
```
configure terminal
interface Loopback0
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
!
interface Ethernet1/0
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet1/1
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Vlan55
 ip ospf 1 area 10
!
interface Vlan101
 ip ospf 1 area 10
!
router ospfv3 1
 router-id 1.1.4.4
 !
 address-family ipv6 unicast
  passive-interface Loopback0
 exit-address-family
!
router ospf 1
 router-id 1.1.4.4
 passive-interface Loopback0
```

**SW5**  
Настроим секцию OSPF для IPv4/IPv6 и включим OSPF на интерфейсах:
```
configure terminal
interface Loopback0
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
!
interface Ethernet1/0
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet1/1
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Vlan55
 ip ospf 1 area 10
!
interface Vlan101
 ip ospf 1 area 10
!
router ospfv3 1
 router-id 1.1.5.5
 !
 address-family ipv6 unicast
  passive-interface Loopback0
 exit-address-family
!
router ospf 1
 router-id 1.1.5.5
 passive-interface Loopback0
```
