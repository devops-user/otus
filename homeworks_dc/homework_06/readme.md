# Построение Underlay сети(ISIS)
### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_03/images/topology.JPG)

Настройка протокола маршрутизации ISIS для Underlay сети:

**DC1-SPINE-1**
```
!
router isis HOMELAB
   net 49.0001.0100.0000.1000.00
   is-hostname DC1-SPINE-1
   !
   address-family ipv4 unicast
      bfd all-interfaces
   !
   address-family ipv6 unicast
      bfd all-interfaces
!
interface Ethernet1
   no switchport
   ip address 10.2.1.0/31
   ipv6 enable
   ipv6 address 2002:5555::/127
   isis enable HOMELAB
   isis bfd
   isis circuit-type level-1
   isis network point-to-point
!
interface Ethernet2
   no switchport
   ip address 10.2.1.2/31
   ipv6 enable
   ipv6 address 2002:5555::2/127
   isis enable HOMELAB
   isis bfd
   isis circuit-type level-1
   isis network point-to-point
!
interface Ethernet3
   no switchport
   ip address 10.2.1.4/31
   ipv6 enable
   ipv6 address 2002:5555::4/127
   isis enable HOMELAB
   isis bfd
   isis circuit-type level-1
   isis network point-to-point
!
interface Loopback0
   ip address 10.0.1.0/32
   ipv6 enable
   ipv6 address 2002:101::/128
   isis enable HOMELAB
   isis passive
!
```

**DC1-SPINE-2**
```
!
router isis HOMELAB
   net 49.0001.0100.0000.2000.00
   is-hostname DC1-SPINE-2
   !
   address-family ipv4 unicast
      bfd all-interfaces
   !
   address-family ipv6 unicast
      bfd all-interfaces
!
interface Ethernet1
   no switchport
   ip address 10.2.2.0/31
   ipv6 enable
   ipv6 address 2002:5555:2222::/127
   isis enable HOMELAB
   isis bfd
   isis circuit-type level-1
   isis network point-to-point
!
interface Ethernet2
   no switchport
   ip address 10.2.2.2/31
   ipv6 enable
   ipv6 address 2002:5555:2222::2/127
   isis enable HOMELAB
   isis bfd
   isis circuit-type level-1
   isis network point-to-point
!
interface Ethernet3
   no switchport
   ip address 10.2.2.4/31
   ipv6 enable
   ipv6 address 2002:5555:2222::4/127
   isis enable HOMELAB
   isis bfd
   isis circuit-type level-1
   isis network point-to-point
!
interface Loopback0
   ip address 10.0.2.0/32
   ipv6 enable
   ipv6 address 2002:202::/128
   isis enable HOMELAB
   isis passive
!
```

**DC1-LEAF-1A**
```
!
router isis HOMELAB
   net 49.0001.0100.0000.0001.00
   is-hostname DC1-LEAF-1A
   !
   address-family ipv4 unicast
      bfd all-interfaces
   !
   address-family ipv6 unicast
      bfd all-interfaces
!
interface Ethernet1
   no switchport
   ip address 10.2.1.1/31
   ipv6 enable
   ipv6 address 2002:5555::1/127
   isis enable HOMELAB
   isis bfd
   isis circuit-type level-1
   isis network point-to-point
!
interface Ethernet3
   no switchport
   ip address 10.2.2.1/31
   ipv6 enable
   ipv6 address 2002:5555:2222::1/127
   isis enable HOMELAB
   isis bfd
   isis circuit-type level-1
   isis network point-to-point
!
interface Loopback0
   ip address 10.0.0.1/32
   ipv6 enable
   ipv6 address 2002:3301::/128
   isis enable HOMELAB
   isis passive
!
```

**DC1-LEAF-1B**
```
!
router isis HOMELAB
   net 49.0001.0100.0000.0002.00
   is-hostname DC1-LEAF-1B
   !
   address-family ipv4 unicast
      bfd all-interfaces
   !
   address-family ipv6 unicast
      bfd all-interfaces
!
interface Ethernet1
   no switchport
   ip address 10.2.1.3/31
   ipv6 enable
   ipv6 address 2002:5555::3/127
   isis enable HOMELAB
   isis bfd
   isis circuit-type level-1
   isis network point-to-point
!
interface Ethernet3
   no switchport
   ip address 10.2.2.3/31
   ipv6 enable
   ipv6 address 2002:5555:2222::3/127
   isis enable HOMELAB
   isis bfd
   isis circuit-type level-1
   isis network point-to-point
!
interface Loopback0
   ip address 10.0.0.2/32
   ipv6 enable
   ipv6 address 2002:3302::/128
   isis enable HOMELAB
   isis passive
!
```

**DC1-LEAF-1C**
```
!
router isis HOMELAB
   net 49.0001.0100.0000.0003.00
   is-hostname DC1-LEAF-1C
   !
   address-family ipv4 unicast
      bfd all-interfaces
   !
   address-family ipv6 unicast
      bfd all-interfaces
!
interface Ethernet1
   no switchport
   ip address 10.2.1.5/31
   ipv6 enable
   ipv6 address 2002:5555::5/127
   isis enable HOMELAB
   isis bfd
   isis circuit-type level-1
   isis network point-to-point
!
interface Ethernet3
   no switchport
   ip address 10.2.2.5/31
   ipv6 enable
   ipv6 address 2002:5555:2222::5/127
   isis enable HOMELAB
   isis bfd
   isis circuit-type level-1
   isis network point-to-point
!
interface Loopback0
   ip address 10.0.0.3/32
   ipv6 enable
   ipv6 address 2002:3303::/128
   isis enable HOMELAB
   isis passive
!
```

Проверка IP-связанности между устройствами:
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_06/images/spine_1.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_06/images/spine_2.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_06/images/leaf_1.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_06/images/leaf_2.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_06/images/leaf_3.png)

Проверка IPv6-связанности между устройствами:
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_06/images/spine_1v6.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_06/images/spine_2v6.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_06/images/leaf_1v6.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_06/images/leaf_2v6.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_06/images/leaf_3v6.png)
