# Построение Underlay сети(eBGP)
### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_03/images/topology.JPG)

Настройка протокола маршрутизации BGP для Underlay сети:

**DC1-SPINE-1**
```
!
router bgp 65000
   router-id 10.0.1.0
   timers bgp 5 15
   bgp listen range 10.2.1.0/24 peer-group LEAFS peer-filter ASN_LEAFS
   neighbor LEAFS peer group
   neighbor LEAFS bfd
   network 10.0.1.0/32
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
   network 10.0.2.0/32
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
   network 10.0.0.1/32
   redistribute connected
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
   network 10.0.0.2/32
   redistribute connected
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
   network 10.0.0.3/32
   redistribute connected
!
```

Проверка IP-связанности между устройствами:
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_08/images/spine_1.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_08/images/spine_2.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_08/images/leaf_1.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_08/images/leaf_2.png)

![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_08/images/leaf_3.png)
