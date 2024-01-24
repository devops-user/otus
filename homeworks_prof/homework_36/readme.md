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

  * Настроим DMVPN между офисами Москва и Чокурдах, Лабытнанги:
