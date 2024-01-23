# BGP фильтрация

 1. * Настроим оператора Киторн, чтобы в сторону Москвы анонсировался default'й-маршрут:  
**R22**
```
router bgp 101
 !
 address-family ipv4
  neighbor 85.75.123.38 default-originate
 exit-address-family
 !
 address-family ipv6
  neighbor 2002:5555::13 default-originate
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
  * Проверим, что мы анонсируем по eBGP IPv4/IPv6 в сторону Москвы, как можно увидеть, только default'й-маршрут:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_28/images/R22.png)

2.  * Настроим оператора Ламас, чтобы в сторону Москвы анонсировался default'й-маршрут:  
**R21**
```
router bgp 101
 !
 address-family ipv4
  neighbor neighbor 85.75.123.34 default-originate
 exit-address-family
 !
 address-family ipv6
  neighbor neighbor 2002:5555::15 default-originate
 exit-address-family
```
  * С помощью as-path отфильтруем префиксы и оставим только тот, что принадлежит СПб:
```
!
ip as-path access-list 50 permit _2042$
!
```
  * Создадим prefix-list и route-map для IPv4/IPv6, так же в route-map'ы добавим соответствие нашему as-path:
```
!
ip prefix-list pl_default seq 10 permit 0.0.0.0/0
ip prefix-list pl_default seq 15 permit 1.1.18.18/32
!
!
ipv6 prefix-list pl_default_ipv6 seq 10 permit ::/0
ipv6 prefix-list pl_default_ipv6 seq 15 permit 2002:101::18:18/128
!
route-map rm_default permit 10
 match ip address pl_default
 match as-path 50
!
route-map rm_default_ipv6 permit 10
 match as-path 50
 match ipv6 address prefix-list pl_default_ipv6
!
```
  * Повесим наши route-map в сторону eBGP-соседа Москва:
```
router bgp 101
 !
 address-family ipv4
  neighbor 85.75.123.34 route-map rm_default out
 exit-address-family
 !
 address-family ipv6
  neighbor 2002:5555::15 route-map rm_default_ipv6 out
 exit-address-family
```
 * Проверим, что мы анонсируем по eBGP IPv4/IPv6 в сторону Москвы, как можно увидеть, только default'й-маршрут и префикс от СПб:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_28/images/R21.png)

2.  * Настроим filter-list в офисе Москва, чтобы избежать транзитного трафика и повесим его в сторону оператора Киторн:  
**R14**
```
!
ip as-path access-list 99 permit ^$
!
router bgp 1001
 !
 address-family ipv4
  neighbor 85.75.123.37 filter-list 99 out
 exit-address-family
 !
 address-family ipv6
  neighbor 2002:5555::12 filter-list 99 out
 exit-address-family
!
```
* Проверим, что мы получаем от eBGP-соседа(Киторн) IPv4/IPv6:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_28/images/R14.png)

2.  * Настроим filter-list в офисе Москва, чтобы избежать транзитного трафика и повесим его в сторону оператора Ламас:  
**R15**
```
!
ip as-path access-list 99 permit ^$
!
router bgp 1001
 !
 address-family ipv4
  neighbor 85.75.123.33 filter-list 99 out
 exit-address-family
 !
 address-family ipv6
  neighbor 2002:5555::14 filter-list 99 out
 exit-address-family
!
```
* Проверим, что мы получаем от eBGP-соседа(Ламас) IPv4/IPv6:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_28/images/R15.png)

 1. * Настроим filter-list в офисе СПб так, чтобы избежать транзитного трафика и повесим его в сторону оператора Триада, и так же отфильтруем не нужные префиксы IPv4/IPv6 создав prefix-list и route-map:  
**R18**
```
!
ip as-path access-list 78 permit ^$
!
router bgp 2042
!
 address-family ipv4
  neighbor 85.75.123.21 filter-list 78 out
  neighbor 85.75.123.21 route-map rm_msc in
  neighbor 85.75.123.25 filter-list 78 out
  neighbor 85.75.123.25 route-map rm_msc in
 exit-address-family
 !
 address-family ipv6
  neighbor 2002:5555::14 route-map rm_msc_ipv6 in
  neighbor 2002:5555::14 filter-list 78 out
  neighbor 2002:5555::16 route-map rm_msc_ipv6 in
  neighbor 2002:5555::16 filter-list 78 out
 exit-address-family
!
ip prefix-list pl_msc seq 5 permit 1.1.14.14/32
ip prefix-list pl_msc seq 10 permit 1.1.15.15/32
ip prefix-list pl_msc seq 15 deny 0.0.0.0/0 le 32
!
ipv6 prefix-list pl_msc_ipv6 seq 5 permit 2002:101::14:14/128
ipv6 prefix-list pl_msc_ipv6 seq 10 permit 2002:101::15:15/128
ipv6 prefix-list pl_msc_ipv6 seq 15 deny ::/0 le 128
!
route-map rm_msc_ipv6 permit 10
 match ipv6 address prefix-list pl_msc_ipv6
!
route-map rm_msc permit 10
 match ip address prefix-list pl_msc
!
```
* Проверим, что мы получаем от eBGP-соседей(Триада) IPv4/IPv6:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_28/images/R18.png)
