# Настройка iBGP

  * Настроим iBGP в офисе Москва:  

**R14**
```
router bgp 1001
 neighbor 1.1.15.15 remote-as 1001
 neighbor 1.1.15.15 update-source Loopback0
 neighbor 2002:101::15:15 remote-as 1001
 neighbor 2002:101::15:15 update-source Loopback0
 !
 address-family ipv4
  neighbor 1.1.15.15 activate
  neighbor 1.1.15.15 next-hop-self
  neighbor 1.1.15.15 soft-reconfiguration inbound
  neighbor 85.75.123.37 route-map rm_low_preference in
 exit-address-family
 !        
 address-family ipv6
  neighbor 2002:101::15:15 activate
  neighbor 2002:101::15:15 next-hop-self
  neighbor 2002:101::15:15 soft-reconfiguration inbound
  neighbor 2002:5555::12 route-map rm_low_preference in
 exit-address-family
```
  * Так же создадим route-map с низким приоритеом local-preference и повесим его в сторону оператора Киторн:
```
route-map rm_low_preference permit 5
 set local-preference 10
```
Посмотрим соседей IPv4/IPv6, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_27/images/R14.png)

Посмотрим, что выход из офиса осуществялется через оператора Ламас, для этого посмотрим на префикс - 1.1.18.18 (2002:101::18:18/128), который принадлежит маршрутизатору в офисе СПб, как видим, он прилетает нам от нашего iBGP-соседа и считается как лучший:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_27/images/R14_show.png)

**R15**
```
router bgp 1001
 neighbor 1.1.14.14 remote-as 1001
 neighbor 1.1.14.14 update-source Loopback0
 neighbor 2002:101::14:14 remote-as 1001
 neighbor 2002:101::14:14 update-source Loopback0
!
 address-family ipv4
  neighbor 1.1.14.14 activate
  neighbor 1.1.14.14 next-hop-self
  neighbor 1.1.14.14 soft-reconfiguration inbound
exit-address-family
!
address-family ipv6
  neighbor 2002:101::14:14 activate
  neighbor 2002:101::14:14 next-hop-self
  neighbor 2002:101::14:14 soft-reconfiguration inbound
 exit-address-family
```
Посмотрим соседей IPv4/IPv6, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_27/images/R15.png)

  * Настроим iBGP у оператора Триада:

**R23**
```
router bgp 520
 neighbor 1.1.24.24 remote-as 520
 neighbor 1.1.24.24 update-source Loopback0
 neighbor 1.1.25.25 remote-as 520
 neighbor 1.1.25.25 update-source Loopback0
 neighbor 1.1.26.26 remote-as 520
 neighbor 1.1.26.26 update-source Loopback0
 neighbor 2002:101::24:24 remote-as 520
 neighbor 2002:101::24:24 update-source Loopback0
 neighbor 2002:101::25:25 remote-as 520
 neighbor 2002:101::25:25 update-source Loopback0
 neighbor 2002:101::26:26 remote-as 520
 neighbor 2002:101::26:26 update-source Loopback0
 !
 address-family ipv4
  neighbor 1.1.24.24 activate
  neighbor 1.1.24.24 route-reflector-client
  neighbor 1.1.24.24 next-hop-self
  neighbor 1.1.24.24 soft-reconfiguration inbound
  neighbor 1.1.25.25 activate
  neighbor 1.1.25.25 route-reflector-client
  neighbor 1.1.25.25 next-hop-self
  neighbor 1.1.25.25 soft-reconfiguration inbound
  neighbor 1.1.26.26 activate
  neighbor 1.1.26.26 route-reflector-client
  neighbor 1.1.26.26 next-hop-self
  neighbor 1.1.26.26 soft-reconfiguration inbound
  no neighbor 2002:101::24:24 activate
  no neighbor 2002:101::25:25 activate
  no neighbor 2002:101::26:26 activate
 exit-address-family
 !
 address-family ipv6
  neighbor 2002:101::24:24 activate
  neighbor 2002:101::24:24 route-reflector-client
  neighbor 2002:101::24:24 soft-reconfiguration inbound
  neighbor 2002:101::25:25 activate
  neighbor 2002:101::25:25 route-reflector-client
  neighbor 2002:101::25:25 soft-reconfiguration inbound
  neighbor 2002:101::26:26 activate
  neighbor 2002:101::26:26 route-reflector-client
  neighbor 2002:101::26:26 soft-reconfiguration inbound
 exit-address-family
```
Посмотрим соседей IPv4/IPv6, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_27/images/R23.png)

**R24**
```
router bgp 520
 neighbor 1.1.23.23 remote-as 520
 neighbor 1.1.23.23 update-source Loopback0
 neighbor 2002:101::23:23 remote-as 520
 neighbor 2002:101::23:23 update-source Loopback0
 !
 address-family ipv4
  neighbor 1.1.23.23 activate
  neighbor 1.1.23.23 next-hop-self
  neighbor 1.1.23.23 soft-reconfiguration inbound
  no neighbor 2002:101::23:23 activate
 exit-address-family
 !
 address-family ipv6
  neighbor 2002:101::23:23 activate
  neighbor 2002:101::23:23 soft-reconfiguration inbound
 exit-address-family
```
Посмотрим соседей IPv4/IPv6, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_27/images/R24.png)

**R25**
```
router bgp 520
 neighbor 1.1.23.23 remote-as 520
 neighbor 1.1.23.23 update-source Loopback0
 neighbor 2002:101::23:23 remote-as 520
 neighbor 2002:101::23:23 update-source Loopback0
 !
 address-family ipv4
  neighbor 1.1.23.23 activate
  neighbor 1.1.23.23 next-hop-self
  neighbor 1.1.23.23 soft-reconfiguration inbound
  no neighbor 2002:101::23:23 activate
 exit-address-family
 !
 address-family ipv6
  neighbor 2002:101::23:23 activate
  neighbor 2002:101::23:23 soft-reconfiguration inbound
 exit-address-family
```
Посмотрим соседей IPv4/IPv6, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_27/images/R25.png)

**R26**
```
router bgp 520
 neighbor 1.1.23.23 remote-as 520
 neighbor 1.1.23.23 update-source Loopback0
 neighbor 2002:101::23:23 remote-as 520
 neighbor 2002:101::23:23 update-source Loopback0
 !
 address-family ipv4
  neighbor 1.1.23.23 activate
  neighbor 1.1.23.23 next-hop-self
  neighbor 1.1.23.23 soft-reconfiguration inbound
  no neighbor 2002:101::23:23 activate
 exit-address-family
 !
 address-family ipv6
  neighbor 2002:101::23:23 activate
  neighbor 2002:101::23:23 soft-reconfiguration inbound
 exit-address-family
```
Посмотрим соседей IPv4/IPv6, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_27/images/R26.png)

  * Настроим BGP в офисе СПб так, чтобы трафик до любого офиса распределялся по двум линкам одновременно:

**R18**
```
router bgp 2042
!
 address-family ipv4
  maximum-paths 2
 exit-address-family
!
 address-family ipv6
  maximum-paths 2
 exit-address-family
```
Сделаем трассировку до адреса оператора Киторн, видим, что есть несколько путей, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_27/images/R18_trace.png)
