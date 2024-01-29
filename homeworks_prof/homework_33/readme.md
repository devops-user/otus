  * Настроим DHCP-сервер для IPv4 в офисе Москва на SW4 и SW5.

**SW4**
```
ip dhcp excluded-address 192.168.155.1 192.168.155.4
!
ip dhcp pool LAN_155
 network 192.168.155.0 255.255.255.240
 default-router 192.168.155.1 
 dns-server 192.168.155.1 
```

**SW5**
```
ip dhcp excluded-address 192.168.55.1 192.168.55.4
!
ip dhcp pool LAN_55
 network 192.168.55.0 255.255.255.240
 default-router 192.168.55.1 
 dns-server 192.168.55.1 
```


  * Настроим NTP сервер на R12 и R13.

**R12**
```
ntp source Loopback0
ntp master 4
ntp update-calendar
```

**R13**
```
ntp source Loopback0
ntp master 5
ntp update-calendar
```

Проверим состояние и статус:  
**R14**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/R14.png)

**R15**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/R15.png)

**R19**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/R19.png)

**R20**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/R20.png)

**SW4**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/SW4.png)

**SW5**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/SW5.png)

**SW3**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/SW3.png)

**SW2**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/SW2.png)
