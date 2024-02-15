  * Настроим статический NAT для R20.

 **R15**
```
ip nat inside source static 1.1.20.20 100.100.20.20
```
Добавим статический маршрут:
```
ip route 100.100.20.20 255.255.255.255 Null0
```
Теперь объявим этот префик в BGP
```
router bgp 1001
address-family ipv4
  network 100.100.20.20 mask 255.255.255.255
```

Выполним пинг с R20 до оператора Ламас и Триада:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/R20_nat.png)

Теперь посмотрим таблицу nat translations на роутере R15:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/R15_nat.png)

  * Настроим статический NAT(PAT) для офиса Чокурдах.

**R28**
```
ip access-list standard PAT_LAN
 permit 192.168.50.0 0.0.0.7
!
ip nat inside source list PAT_LAN interface Loopback0 overload
!
interface Loopback0
 ip nat inside
!
interface Ethernet0/2.50
 ip nat inside
!
interface Ethernet0/0
 ip nat outside
!
interface Ethernet0/1
 ip nat outside
```
Запустим пинг на VPC до офиса Москва до R15:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/ping.png)

На R28 посмотрим NAT-трансляцию:
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/R28_pat.png)

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
Проверим состояние на SW4 и SW5:  
**SW4**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/SW4_dhcp.png)

**SW5**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/SW5_dhcp.png)
Видим, что адреса выделились из наших пулов для каждого VPC.

Проверим состояние на VPC1 и VPC7:  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/vpc.png)

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
