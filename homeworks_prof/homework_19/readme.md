# Настройка IS-IS

### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_19/images/isis.png)

**R23**
Настроим секцию для IS-IS и включим IS-IS IPv4/IPv6 на интерфейсах:
```
configure terminal
interface Loopback0
 ip router isis 
 ipv6 router isis 
!
interface Ethernet0/1
 ip router isis 
 ipv6 router isis 
!
interface Ethernet0/2
 ip router isis 
 ipv6 router isis 
!
router isis
 net 49.2222.1000.1023.0023.00
 passive-interface Loopback0
!
```

**R24**
Настроим секцию для IS-IS и включим IS-IS IPv4/IPv6 на интерфейсах:
```
configure terminal
interface Loopback0
 ip router isis 
 ipv6 router isis 
!
interface Ethernet0/1
 ip router isis 
 ipv6 router isis 
!
interface Ethernet0/2
 ip router isis 
 ipv6 router isis 
!
router isis
 net 49.0024.1000.1024.0024.00
 passive-interface Loopback0
!
```

**R25**
Настроим секцию для IS-IS и включим IS-IS IPv4/IPv6 на интерфейсах:
```
configure terminal
interface Loopback0
 ip router isis 
 ipv6 router isis 
!
interface Ethernet0/0
 ip router isis 
 ipv6 router isis 
!
interface Ethernet0/2
 ip router isis 
 ipv6 router isis 
!
router isis
 net 49.2222.1000.1025.0025.00
 passive-interface Loopback0
!
```

**R26**
Настроим секцию для IS-IS и включим IS-IS IPv4/IPv6 на интерфейсах:
```
configure terminal
interface Loopback0
 ip router isis 
 ipv6 router isis 
!
interface Ethernet0/0
 ip router isis 
 ipv6 router isis 
!
interface Ethernet0/2
 ip router isis 
 ipv6 router isis 
!
router isis
 net 49.0026.1000.1026.0026.00
 passive-interface Loopback0
!
```
