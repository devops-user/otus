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
  * Так же создадим route-map с низким приоритеом и увеличим длинну as-path в bgp и повесим его в сторону оператора Киторн:
```
route-map rm_low_preference permit 5
 set local-preference 10
 set as-path prepend 1001 1001
```
Посмотрим соседей IPv4/IPv6, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_27/images/R14.png)

Посмотрим, что выход из офиса осуществялется через оператора Ламас, для этого посмотрим на префикс - 1.1.18.18 (2002:101::18:18/128), который принадлежит маршрутизатору в офисе СПб, как видим, он прилетает нам от нашего iBGP-соседа и считается как лучший:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_27/images/R14_show.png)

Псомотри, что наш as-path стал длиннее:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_27/images/R14_prepend.png)


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
 bgp router-id 1.1.23.23
 bgp log-neighbor-changes
 neighbor TRIADA_peergroup peer-group
 neighbor TRIADA_peergroup remote-as 520
 neighbor TRIADA_peergroup update-source Loopback0
 neighbor TRIADA_peergroup_IPv6 peer-group
 neighbor TRIADA_peergroup_IPv6 remote-as 520
 neighbor TRIADA_peergroup_IPv6 update-source Loopback0
 neighbor 1.1.24.24 peer-group TRIADA_peergroup
 neighbor 1.1.25.25 peer-group TRIADA_peergroup
 neighbor 1.1.26.26 peer-group TRIADA_peergroup
 neighbor 2002:101::24:24 peer-group TRIADA_peergroup_IPv6
 neighbor 2002:101::25:25 peer-group TRIADA_peergroup_IPv6
 neighbor 2002:101::26:26 peer-group TRIADA_peergroup_IPv6
 !
 address-family ipv4
  network 1.1.23.23 mask 255.255.255.255
  neighbor TRIADA_peergroup route-reflector-client
  neighbor TRIADA_peergroup next-hop-self
  neighbor 1.1.24.24 activate
  neighbor 1.1.25.25 activate
  neighbor 1.1.26.26 activate
  no neighbor 2002:101::24:24 activate
  no neighbor 2002:101::25:25 activate
  no neighbor 2002:101::26:26 activate
 exit-address-family
 !
 address-family ipv6
  network 2002:101::23:23/128
  neighbor TRIADA_peergroup_IPv6 route-reflector-client
  neighbor TRIADA_peergroup_IPv6 next-hop-self
  neighbor 2002:101::24:24 activate
  neighbor 2002:101::25:25 activate
  neighbor 2002:101::26:26 activate
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
