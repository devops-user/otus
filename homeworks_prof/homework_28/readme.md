# BGP фильтрация

  * Настроим оператора Киторн, чтобы в сторону Москвы анонсировался только default'й-маршрут:  
**R22**
```
router bgp 101
 !
 address-family ipv4
  neighbor 85.75.123.38 soft-reconfiguration inbound
 exit-address-family
 !
 address-family ipv6
  neighbor 2002:5555::13 soft-reconfiguration inbound
 exit-address-family
```
  * Создадим prefix-list и route-map для IPv4/IPv6:
```
!
ip prefix-list pl_default seq 5 permit 0.0.0.0/0
ip prefix-list pl_default seq 10 deny 0.0.0.0/0 le 32
!
!
ipv6 prefix-list pl_default_ipv6 seq 5 permit ::/0
ipv6 prefix-list pl_default_ipv6 seq 10 deny ::/0 le 128
!
route-map rm_default deny 10
 match ip address pl_default
!
route-map rm_default_ipv6 deny 10
 match ipv6 address prefix-list pl_default_ipv6
!
```
  * Повесим наши route-map в сторону eBGP-соседа Москва:
```
router bgp 101
 !
 address-family ipv4
  neighbor 85.75.123.38 route-map rm_default out
 exit-address-family
 !
 address-family ipv6
  neighbor 2002:5555::13 route-map rm_default_ipv6 out
 exit-address-family
```
  * Пооверим, что мы анонсируем по eBGP IPv4/IPv6 в сторону Москвы, как можно увидеть, только default'й-маршрут:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_28/images/R22.png)
