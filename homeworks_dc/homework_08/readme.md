# Построение Underlay сети(eBGP)
### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_03/images/topology.JPG)

### Таблица адресации
| Устройство | Интерфейс | IPv4 | IPv6 |
--- | --- | --- | --- |
| **DC1-SPINE-1** | Eth1 | 10.2.1.0/31 | 2002:5555::/127 |
|  | Eth2 | 10.2.1.2/31 | 2002:5555::2/127 |
|  | Eth3 | 10.2.1.4/31 | 2002:5555::4/127 |
|  | Loopback0 | 10.0.1.0/32 | 2002:101::/128 |
|  | Loopback1 | 10.1.1.0/32 |  |
| **DC1-SPINE-2** | Eth1 | 10.2.2.0/31 | 2002:5555:2222::/127 |
|  | Eth2 | 10.2.2.2/31 | 2002:5555:2222::2/127 |
|  | Eth3 | 10.2.2.4/31 | 2002:5555:2222::4/127 |
|  | Loopback0 | 10.0.2.0/32 | 2002:202::/128 |
|  | Loopback1 | 10.1.2.0/32 |  |
| **DC1-LEAF-1A** | Eth1 | 10.2.1.1/31 | 2002:5555::1/127 |
|  | Eth3 | 10.2.2.1/31 | 2002:5555:2222::1/127 |
|  | Loopback0 | 10.0.0.1/32 | 2002:3301::/128 |
|  | Loopback1 | 10.1.0.1/32 |  |
| **DC1-LEAF-1B** | Eth1 | 10.2.1.3/31 | 2002:5555::3/127 |
|  | Eth3 | 10.2.2.3/31 | 2002:5555:2222::3/127 |
|  | Loopback0 | 10.0.0.2/32 | 2002:3302::/128 |
|  | Loopback1 | 10.1.0.2/32 |  |
| **DC1-LEAF-1C** | Eth1 | 10.2.1.5/31 | 2002:5555::5/127 |
|  | Eth3 | 10.2.2.5/31 | 2002:5555:2222::5/127 |
|  | Loopback0 | 10.0.0.3/32 | 2002:3303::/128 |
|  | Loopback1 | 10.1.0.3/32 |  |

Настройка протокола маршрутизации eBGP для Underlay сети:

**DC1-SPINE-1**
```
!
router bgp 65000
   router-id 10.0.1.0
   timers bgp 5 15
   bgp listen range 10.2.1.0/24 peer-group LEAFS peer-filter ASN_LEAFS
   neighbor LEAFS peer group
   neighbor LEAFS bfd
   neighbor 2002:5555::1 remote-as 65101
   neighbor 2002:5555::1 bfd
   neighbor 2002:5555::3 remote-as 65102
   neighbor 2002:5555::3 bfd
   neighbor 2002:5555::5 remote-as 65103
   neighbor 2002:5555::5 bfd
   !
   address-family ipv4
      network 10.0.1.0/32
   !
   address-family ipv6
      neighbor 2002:5555::1 activate
      neighbor 2002:5555::3 activate
      neighbor 2002:5555::5 activate
      network 2002:101::/128
!
peer-filter ASN_LEAFS
   10 match as-range 65101-65103 result accept
!
```

**DC1-SPINE-2**
```
!
router bgp 65000
   router-id 10.0.2.0
   timers bgp 5 15
   bgp listen range 10.2.2.0/24 peer-group LEAFS peer-filter ASN_LEAFS
   neighbor LEAFS peer group
   neighbor LEAFS bfd
   neighbor 2002:5555:2222::1 remote-as 65101
   neighbor 2002:5555:2222::1 bfd
   neighbor 2002:5555:2222::3 remote-as 65102
   neighbor 2002:5555:2222::3 bfd
   neighbor 2002:5555:2222::5 remote-as 65103
   neighbor 2002:5555:2222::5 bfd
   !
   address-family ipv4
      network 10.0.2.0/32
   !
   address-family ipv6
      neighbor 2002:5555:2222::1 activate
      neighbor 2002:5555:2222::3 activate
      neighbor 2002:5555:2222::5 activate
      network 2002:202::/128
!
peer-filter ASN_LEAFS
   10 match as-range 65101-65103 result accept
!
```

**DC1-LEAF-1A**
```
!
router bgp 65101
   router-id 10.0.0.1
   timers bgp 5 15
   maximum-paths 2
   neighbor 10.2.1.0 remote-as 65000
   neighbor 10.2.1.0 bfd
   neighbor 10.2.2.0 remote-as 65000
   neighbor 10.2.2.0 bfd
   neighbor 2002:5555:: remote-as 65000
   neighbor 2002:5555:: bfd
   neighbor 2002:5555:2222:: remote-as 65000
   neighbor 2002:5555:2222:: bfd
   redistribute connected
   !
   address-family ipv6
      neighbor 2002:5555:: activate
      neighbor 2002:5555:2222:: activate
!
```

**DC1-LEAF-1B**
```
!
router bgp 65102
   router-id 10.0.0.2
   timers bgp 5 15
   maximum-paths 2
   neighbor 10.2.1.2 remote-as 65000
   neighbor 10.2.1.2 bfd
   neighbor 10.2.2.2 remote-as 65000
   neighbor 10.2.2.2 bfd
   neighbor 2002:5555::2 remote-as 65000
   neighbor 2002:5555::2 bfd
   neighbor 2002:5555:2222:: bfd
   neighbor 2002:5555:2222::2 remote-as 65000
   neighbor 2002:5555:2222::2 bfd
   redistribute connected
   !
   address-family ipv6
      neighbor 2002:5555::2 activate
      neighbor 2002:5555:2222::2 activate
!
```

**DC1-LEAF-1C**
```
!
router bgp 65103
   router-id 10.0.0.3
   timers bgp 5 15
   maximum-paths 2
   neighbor 10.2.1.4 remote-as 65000
   neighbor 10.2.1.4 bfd
   neighbor 10.2.2.4 remote-as 65000
   neighbor 10.2.2.4 bfd
   neighbor 2002:5555::4 remote-as 65000
   neighbor 2002:5555::4 bfd
   neighbor 2002:5555:2222::4 remote-as 65000
   neighbor 2002:5555:2222::4 bfd
   redistribute connected
   !
   address-family ipv6
      neighbor 2002:5555::4 activate
      neighbor 2002:5555:2222::4 activate
!
```

Проверка IPv4-связанности между устройствами:
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_08/images/spine_1.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_08/images/spine_2.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_08/images/leaf_1.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_08/images/leaf_2.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_08/images/leaf_3.png)

Проверка IPv6-связанности между устройствами:
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_08/images/spine_1v6.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_08/images/spine_2v6.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_08/images/leaf_1v6.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_08/images/leaf_2v6.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_08/images/leaf_3v6.png)

Проверка bfd IPv4/IPv6 для bgp:
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_08/images/spine_1_bfd.PNG)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_08/images/spine_2_bfd.PNG)
