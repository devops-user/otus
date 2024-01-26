# VPN. GRE. DmVPN

  * Настроим GRE между офисами Москва и С.-Петербург:

**R14**
```
interface Tunnel0
 description GRE_to_SPB_R18
 ip address 172.16.0.14 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source 1.1.14.14
 tunnel destination 1.1.18.18
end
```

**R18**
```
interface Tunnel0
 description GRE_to_MSK_R14
 ip address 172.16.0.18 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source 1.1.18.18
 tunnel destination 1.1.14.14
end
```
Проверим IP-связность:  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_36/images/gre.png)

  * Настроим DmVPN между офисами Москва и Чокурдах, Лабытнанги:

**R15**
```
interface Tunnel100
 description DmVPV_R27_R28
 ip address 10.254.0.15 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip nhrp map multicast dynamic
 ip nhrp network-id 254
 ip tcp adjust-mss 1360
 tunnel source Loopback0
 tunnel mode gre multipoint
end
```

**R27**
```
interface Tunnel100
 description DmVPN_R15
 ip address 10.254.0.27 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip nhrp map multicast 1.1.15.15
 ip nhrp map multicast 10.254.0.15
 ip nhrp map 10.254.0.15 1.1.15.15
 ip nhrp network-id 254
 ip nhrp nhs 10.254.0.15
 ip tcp adjust-mss 1360
 tunnel source Loopback0
 tunnel mode gre multipoint
end
```

**R28**
```
interface Tunnel100
 description DmVPN_R15
 ip address 10.254.0.28 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip nhrp map multicast 1.1.15.15
 ip nhrp map multicast 10.254.0.15
 ip nhrp map 10.254.0.15 1.1.15.15
 ip nhrp network-id 254
 ip nhrp nhs 10.254.0.15
 ip tcp adjust-mss 1360
 tunnel source Loopback0
 tunnel mode gre multipoint
end
```
Проверим IP-связность и состояние на R15:  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_36/images/R15.png)

Проверим IP-связность и состояние на R27:  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_36/images/R27.png)

Проверим IP-связность и состояние на R28:  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_36/images/R28.png)
