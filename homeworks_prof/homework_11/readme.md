# Архитектура сети

### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_11/images/topo.png)

### Таблица адресации Триада
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| R23 | e0/0 | 85.75.123.1/30 |
|  | e0/0 | 2002:5555::11/127 |
|  | e0/1 | 10.123.52.0/31 |
|  | e0/1 | 2001:DB8:1212:1212::0/127 |
|  | e0/1 | FE80::23 |
|  | e0/2 | 10.123.52.2/31 |
|  | e0/2 | 2001:DB8:1212:1212::2/127 |
|  | e0/2 | FE80::23 |
|  | lo0 | 1.1.23.23/32 |
|  | lo0 | 2002:101::23:23/128 |
| R24 | e0/0 | 85.75.123.5/30 |
|  | e0/0 | 2002:5555::6/127 |
|  | e0/1 | 10.123.52.4/31 |
|  | e0/1 | 2001:DB8:1212:1212::4/127 |
|  | e0/1 | FE80::24 |
|  | e0/2 | 10.123.52.3/31 |
|  | e0/2 | 2001:DB8:1212:1212::3/127 |
|  | e0/2 | FE80::24 |
|  | e0/3 | 85.75.123.25/30 |
|  | e0/3 | 2002:5555::14/127 |
|  | lo0 | 1.1.24.24/32 |
|  | lo0 | 2002:101::24:24/128 |
| R25 | e0/0 | 10.123.52.1/31 |
|  | e0/0 | 2001:DB8:1212:1212::1/127 |
|  | e0/0 | FE80::25 |
|  | e0/1 | 85.75.123.9/30 |
|  | e0/1 | 2002:5555::/127 |
|  | e0/2 | 10.123.52.7/31 |
|  | e0/2 | 2001:DB8:1212:1212::7/127 |
|  | e0/2 | FE80::25 |
|  | e0/3 | 85.75.123.13/30 |
|  | e0/3 | 2002:5555::2/127 |
|  | lo0 | 1.1.25.25/32 |
|  | lo0 | 2002:101::25:25/128 |
| R26 | e0/0 | 10.123.52.5/31 |
|  | e0/0 | 2001:DB8:1212:1212::5/127 |
|  | e0/0 | FE80::26 |
|  | e0/1 | 85.75.123.17/30 |
|  | e0/1 | 2002:5555::4/127 |
|  | e0/2 | 10.123.52.6/31 |
|  | e0/2 | 2001:DB8:1212:1212::6/127 |
|  | e0/2 | FE80::26 |
|  | e0/3 | 85.75.123.21/30 |
|  | e0/3 | 2002:5555::16/127 |
|  | lo0 | 1.1.26.26/32 |
|  | lo0 | 2002:101::26:26/128 |

**R23**
```
configure terminal
hostname R23
interface Loopback0
 ip address 1.1.23.23 255.255.255.255
 ipv6 address 2002:101::23:23/128
!         
interface Ethernet0/0
 ip address 85.75.123.1 255.255.255.252
 ipv6 address 2002:5555::11/127
!
interface Ethernet0/1
 ip address 10.123.52.0 255.255.255.254
 ipv6 address FE80::23 link-local
 ipv6 address 2001:DB8:1212:1212::/127
!
interface Ethernet0/2
 ip address 10.123.52.2 255.255.255.254
 ipv6 address FE80::23 link-local
 ipv6 address 2001:DB8:1212:1212::2/127
```

**R24**
```
configure terminal
hostname R24
interface Loopback0
 ip address 1.1.24.24 255.255.255.255
 ipv6 address 2002:101::24:24/128
!
interface Ethernet0/0
 ip address 85.75.123.5 255.255.255.252
 ipv6 address 2002:5555::6/127
!
interface Ethernet0/1
 ip address 10.123.52.4 255.255.255.254
 ipv6 address FE80::24 link-local
 ipv6 address 2001:DB8:1212:1212::4/127
!
interface Ethernet0/2
 ip address 10.123.52.3 255.255.255.254
 ipv6 address FE80::24 link-local
 ipv6 address 2001:DB8:1212:1212::3/127
!
interface Ethernet0/3
 ip address 85.75.123.25 255.255.255.252
 ipv6 address 2002:5555::14/127
```

**R25**
```
configure terminal
hostname R25
interface Loopback0
 ip address 1.1.25.25 255.255.255.255
 ipv6 address 2002:101::25:25/128
!         
interface Ethernet0/0
 ip address 10.123.52.1 255.255.255.254
 ipv6 address FE80::25 link-local
 ipv6 address 2001:DB8:1212:1212::1/127
!
interface Ethernet0/1
 ip address 85.75.123.9 255.255.255.252
 ipv6 address 2002:5555::/127
!
interface Ethernet0/2
 ip address 10.123.52.7 255.255.255.254
 ipv6 address FE80::25 link-local
 ipv6 address 2001:DB8:1212:1212::7/127
!
interface Ethernet0/3
 ip address 85.75.123.13 255.255.255.252
 ipv6 address 2002:5555::2/127
```

**R26**
```
configure terminal
hostname R26
interface Loopback0
 ip address 1.1.26.26 255.255.255.255
 ipv6 address 2002:101::26:26/128
!         
interface Ethernet0/0
 ip address 10.123.52.5 255.255.255.254
 ipv6 address FE80::26 link-local
 ipv6 address 2001:DB8:1212:1212::5/127
!
interface Ethernet0/1
 ip address 85.75.123.17 255.255.255.252
 ipv6 address 2002:5555::4/127
!
interface Ethernet0/2
 ip address 10.123.52.6 255.255.255.254
 ipv6 address FE80::26 link-local
 ipv6 address 2001:DB8:1212:1212::6/127
!
interface Ethernet0/3
 ip address 85.75.123.21 255.255.255.252
 ipv6 address 2002:5555::16/127
```

### Таблица адресации Лабытнанги
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| R27 | e0/0 | 85.75.123.10/30 |
|  | e0/0 | 2002:5555::1/127 |
|  | lo0 | 1.1.27.27/32 |
|  | lo0 | 2002:101::27:27/128 |

**R27**
```
configure terminal
hostname R27
interface Loopback0
 ip address 1.1.27.27 255.255.255.255
 ipv6 address 2002:101::27:27/128
!         
interface Ethernet0/0
 ip address 85.75.123.10 255.255.255.252
 ipv6 address 2002:5555::1/127
```

### Таблица адресации Чокурдах
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| R28 | e0/0 | 85.75.123.18/30 |
|  | e0/0 | 2002:5555::5/127 |
|  | e0/1 | 85.75.123.14/30 |
|  | e0/1 | 2002:5555::3/127 |
|  | e0/2.14 | 10.123.14.1/28 |
|  | e0/2.50 | 192.168.50.1/29 |
|  | lo0 | 1.1.28.28/32 |
|  | lo0 | 2002:101::28:28/128 |
| SW29 | vlan14 -> *Management* | 10.123.14.2/28 |
| VPC30 | NIC | 192.168.50.2/29 |
| VPC31 | NIC | 192.168.50.3/29 |

**R28**
```
configure terminal
hostname R28
interface Loopback0
 ip address 1.1.28.28 255.255.255.255
 ipv6 address 2002:101::28:28/128
!
interface Ethernet0/0
 ip address 85.75.123.18 255.255.255.252
 ipv6 address 2002:5555::5/127
!
interface Ethernet0/1
 ip address 85.75.123.14 255.255.255.252
 ipv6 address 2002:5555::3/127
!
interface Ethernet0/2
 no ip address
 no shutdown
!
interface Ethernet0/2.14
 description Management
 encapsulation dot1Q 14
 ip address 10.123.14.1 255.255.255.240
!
interface Ethernet0/2.50
 description LAN
 encapsulation dot1Q 50
 ip address 192.168.50.1 255.255.255.248
```

**SW29**
```
configure terminal
hostname SW29
vlan 14
name MNGT
exit
vlan 50
name LAN
exit
hostname SW29
interface eth0/0
switchport mode access
switchport access vlan 50
spanning-tree portfast
no shutdown
exit
interface eth0/1
switchport mode access
switchport access vlan 50
spanning-tree portfast
no shutdown
exit
interface eth0/2
switchport trunk encapsulation dot1Q
switchport mode trunk
switchport trunk all vlan 14,50
no shutdown
exit
interface vlan14
ip address 10.123.14.2 255.255.255.240
no shutdown
```


### Таблица адресации Ламас
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| R21 | e0/0 | 85.75.123.33/30 |
|  | e0/0 | 2002:5555::14/127 |
|  | e0/1 | 85.75.123.30/30 |
|  | e0/1 | 2002:5555::9/127 |
|  | e0/2 | 85.75.123.6/30 |
|  | e0/2 | 2002:5555::7/127 |
|  | lo0 | 1.1.21.21/32 |
|  | lo0 | 2002:101::21:21/128 |

**R21**
```
configure terminal
hostname R21
interface Loopback0
 ip address 1.1.21.21 255.255.255.255
 ipv6 address 2002:101::21:21/128
!
interface Ethernet0/0
 ip address 85.75.123.33 255.255.255.252
 ipv6 address 2002:5555::14/127
!
interface Ethernet0/1
 ip address 85.75.123.30 255.255.255.252
 ipv6 address 2002:5555::9/127
!
interface Ethernet0/2
 ip address 85.75.123.6 255.255.255.252
 ipv6 address 2002:5555::7/127
```

### Таблица адресации Киторн
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| R22 | e0/0 | 85.75.123.37/30 |
|  | e0/0 | 2002:5555::12/127 |
|  | e0/1 | 85.75.123.29/30 |
|  | e0/1 | 2002:5555::8/127 |
|  | e0/2 | 85.75.123.2/30 |
|  | e0/2 | 2002:5555::10/127 |
|  | lo0 | 1.1.22.22/32 |
|  | lo0 | 2002:101::22:22/128 |

**R22**
```
configure terminal
hostname R22
interface Loopback0
 ip address 1.1.22.22 255.255.255.255
 ipv6 address 2002:101::22:22/128
!
interface Ethernet0/0
 ip address 85.75.123.37 255.255.255.252
 ipv6 address 2002:5555::12/127
!
interface Ethernet0/1
 ip address 85.75.123.29 255.255.255.252
 ipv6 address 2002:5555::8/127
!
interface Ethernet0/2
 ip address 85.75.123.2 255.255.255.252
 ipv6 address 2002:5555::10/127
```

### Таблица адресации С.-Петербург
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| R16 | e0/0 | 10.123.42.12/31 |
|  | e0/1 | 10.123.42.3/31 |
|  | e0/1 | 2001:DB8:2042:2042::1/127 |
|  | e0/1 | FE80::16 |
|  | e0/2 | 10.123.42.10/31 |
|  | e0/3 | 10.123.42.4/31 |
|  | e0/3 | 2001:DB8:2042:2042::5/127 |
|  | e0/3 | FE80::16 |
|  | lo0 | 1.1.16.16/32 |
|  | lo0 | 2002:101::16:16/128 |
| R17 | e0/0 | 10.123.42.6/31 |
|  | e0/1 | 10.123.42.1/31 |
|  | e0/1 | 2001:DB8:2042:2042::3/127 |
|  | e0/1 | FE80::17 |
|  | e0/2 | 10.123.42.8/31 |
|  | lo0 | 1.1.17.17/32 |
|  | lo0 | 2002:101::17:17/128 |
| R18 | e0/0 | 10.123.42.2/31 |
|  | e0/0 | 2001:DB8:2042:2042::/127 |
|  | e0/0 | FE80::18 |
|  | e0/1 | 10.123.42.0/31 |
|  | e0/1 | 2001:DB8:2042:2042::2/127 |
|  | e0/1 | FE80::18 |
|  | e0/2 | 85.75.123.26/30 |
|  | e0/2 | 2002:5555::15/127 |
|  | e0/3 | 85.75.123.22/30 |
|  | e0/3 | 2002:5555::17/127 |
|  | lo0 | 1.1.18.18/32 |
|  | lo0 | 2002:101::18:18/128 |
| R32 | e0/0 | 10.123.42.5/31 |
|  | e0/0 | 2001:DB8:2042:2042::4/127 |
|  | e0/0 | FE80::32 |
|  | lo0 | 1.1.32.32/32 |
|  | lo0 | 2002:101::32:32/128 |
| SW9 | e0/3 | 10.123.42.7/31 |
| SW9 | e1/0 | 10.123.42.11/31 |
| SW9 | vlan242 | 192.168.242.2/29 |
| SW9 | vrrp 1 | 192.168.242.1/29 |
| SW9 | lo0 | 1.1.9.9/32 |
| SW10 | e0/3 | 10.123.142.13/31 |
| SW10 | e1/0 | 10.123.42.9/31 |
| SW10 | vlan242 | 192.168.242.3/29 |
| SW10 | vrrp 1 | 192.168.242.1/29 |
| SW10 | lo0 | 1.1.10.10/32 |
| VPC8 | NIC | 192.168.42.4/29 |
| VPC | NIC | 192.168.42.5/29 |


**R16**
```
configure terminal
hostname R16
interface Loopback0
 ip address 1.1.16.16 255.255.255.255
 ipv6 address 2002:101::16:16/128
!         
interface Ethernet0/0
 ip address 10.123.42.12 255.255.255.254
!
interface Ethernet0/1
 ip address 10.123.42.3 255.255.255.254
 ipv6 address FE80::16 link-local
 ipv6 address 2001:DB8:2042:2042::1/127
!
interface Ethernet0/2
 ip address 10.123.42.10 255.255.255.254
!
interface Ethernet0/3
 ip address 10.123.42.4 255.255.255.254
 ipv6 address FE80::16 link-local
 ipv6 address 2001:DB8:2042:2042::5/127
```

**R17**
```
configure terminal
hostname R17
interface Loopback0
 ip address 1.1.17.17 255.255.255.255
 ipv6 address 2002:101::17:17/128
!         
interface Ethernet0/0
 ip address 10.123.42.6 255.255.255.254
!
interface Ethernet0/1
 ip address 10.123.42.1 255.255.255.254
 ipv6 address FE80::17 link-local
 ipv6 address 2001:DB8:2042:2042::3/127
!
interface Ethernet0/2
 ip address 10.123.42.8 255.255.255.254
```

**R18**
```
configure terminal
hostname R18
interface Loopback0
 ip address 1.1.18.18 255.255.255.255
 ipv6 address 2002:101::18:18/128
!         
interface Ethernet0/0
 ip address 10.123.42.2 255.255.255.254
 ipv6 address FE80::18 link-local
 ipv6 address 2001:DB8:2042:2042::/127
!
interface Ethernet0/1
 ip address 10.123.42.0 255.255.255.254
 ipv6 address FE80::18 link-local
 ipv6 address 2001:DB8:2042:2042::2/127
!
interface Ethernet0/2
 ip address 85.75.123.26 255.255.255.252
 ipv6 address 2002:5555::15/127
!
interface Ethernet0/3
 ip address 85.75.123.22 255.255.255.252
 ipv6 address 2002:5555::17/127
```

**SW9**
```
configure terminal
hostname SW9
vlan 242
name LAN
exit
interface eth0/0
switchport trunk encapsulation dot1q
switchport mode trunk
channel-group 1 mode active
no shutdown
exit
interface eth0/1
switchport trunk encapsulation dot1q
switchport mode trunk
channel-group 1 mode active
no shutdown
exit
interface Port-channel1
switchport trunk encapsulation dot1q
switchport mode trunk
exit
interface eth0/2
switchport mode access
switchport access vlan 242
spanning-tree portfast
no shutdown
exit
interface eth0/3
no switchport
ip address 10.123.42.7 255.255.255.254
no shutdown
exit
interface eth1/0
no switchport
ip address 10.123.42.11 255.255.255.254
no shutdown
exit
interface lo0
ip address 1.1.9.9 255.255.255.255
no shutdown
exit
interface Vlan242
no shutdown
ip address 192.168.242.2 255.255.255.248
vrrp 1 ip 192.168.242.1
vrrp 1 priority 120
vrrp 1 preempt
```
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_11/images/sw9_vrrp.png)

**SW10**
```
configure terminal
hostname SW10
vlan 242
name LAN
exit
interface eth0/0
switchport trunk encapsulation dot1q
switchport mode trunk
channel-group 1 mode active
no shutdown
exit
interface eth0/1
switchport trunk encapsulation dot1q
switchport mode trunk
channel-group 1 mode active
no shutdown
exit
interface Port-channel1
switchport trunk encapsulation dot1q
switchport mode trunk
exit
interface eth0/2
switchport mode access
switchport access vlan 242
spanning-tree portfast
no shutdown
exit
interface eth0/3
no switchport
ip address 10.123.42.13 255.255.255.254
no shutdown
exit
interface eth1/0
no switchport
ip address 10.123.42.9 255.255.255.254
no shutdown
exit
interface lo0
ip address 1.1.10.10 255.255.255.255
no shutdown
exit
interface Vlan242
no shutdown
ip address 192.168.242.3 255.255.255.248
vrrp 1 ip 192.168.242.1
vrrp 1 priority 100
```
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_11/images/sw10_vrrp.png)

**R32**
```
configure terminal
hostname R32
interface Loopback0
 ip address 1.1.32.32 255.255.255.255
 ipv6 address 2002:101::32:32/128
!
interface Ethernet0/0
 ip address 10.123.42.5 255.255.255.254
 ipv6 address FE80::32 link-local
 ipv6 address 2001:DB8:2042:2042::4/127
```
  * Проверка работы с VPCs  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_11/images/vpc8.png)
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_11/images/vpc.png)


### Таблица адресации Москва
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| R12 | e0/0 | 10.101.101.0/31 |
|  | e0/1 | 10.101.101.2/31 |
|  | e0/2 | 10.101.101.4/31 |
|  | e0/2 | 2001:DB8:1001:1001::1/127 |
|  | e0/2 | FE80::12 |
|  | e0/3 | 10.101.101.6/31 |
|  | e0/3 | 2001:DB8:1001:1001::8/127 |
|  | e0/3 | FE80::12 |
|  | lo0 | 1.1.12.12/32 |
|  | lo0 | 2002:101::12:12/128 |
| R13 | e0/0 | 10.101.101.8/31 |
|  | e0/1 | 10.101.101.10/31 |
|  | e0/2 | 10.101.101.12/31 |
|  | e0/2 | 2001:DB8:1001:1001::6/127 |
|  | e0/2 | FE80::13 |
|  | e0/3 | 10.101.101.14/31 |
|  | e0/3 | 2001:DB8:1001:1001::3/127 |
|  | e0/3 | FE80::13 |
|  | lo0 | 1.1.13.13/32 |
|  | lo0 | 2002:101::13:13/128 |
| R14 | e0/0 | 10.101.101.5/31 |
|  | e0/0 | 2001:DB8:1001:1001::0/127 |
|  | e0/0 | FE80::14 |
|  | e0/1 | 10.101.101.15/31 |
|  | e0/1 | 2001:DB8:1001:1001::2/127 |
|  | e0/1 | FE80::14 |
|  | e0/2 | 85.75.123.38/30 |
|  | e0/2 | 2002:5555::13/127 |
|  | e0/3 | 10.101.101.16/31 |
|  | e0/3 | 2001:DB8:1001:1001::4/127 |
|  | e0/3 | FE80::14 |
|  | lo0 | 1.1.14.14/32 |
|  | lo0 | 2002:101::14:14/128 |
| R15 | e0/0 | 10.101.101.13/31 |
|  | e0/0 | 2001:DB8:1001:1001::7/127 |
|  | e0/0 | FE80::15 |
|  | e0/1 | 10.101.101.7/31 |
|  | e0/1 | 2001:DB8:1001:1001::9/127 |
|  | e0/1 | FE80::15 |
|  | e0/2 | 85.75.123.34/30 |
|  | e0/2 | 2002:5555::15/127 |
|  | e0/3 | 10.101.101.18/31 |
|  | e0/3 | 2001:DB8:1001:1001::11/127 |
|  | e0/3 | FE80::15 |
|  | lo0 | 1.1.15.15/32 |
|  | lo0 | 2002:101::15:15/128 |
| R19 | e0/0 | 10.101.101.17/31 |
|  | e0/0 | 2001:DB8:1001:1001::5/127 |
|  | e0/0 | FE80::19 |
|  | lo0 | 1.1.19.19/32 |
|  | lo0 | 2002:101::19:19/128 |
| R20 | e0/0 | 10.101.101.19/31 |
|  | e0/0 | 2001:DB8:1001:1001::10/127 |
|  | e0/0 | FE80::20 |
|  | lo0 | 1.1.20.20/32 |
|  | lo0 | 2002:101::20:20/128 |
| SW2 | vlan101 -> *Management* | 10.123.101.2/29 |
| SW2 | vlan55 | 192.168.55.2/29 |
| SW3 | vlan101 -> *Management* | 10.123.101.3/29 |
| SW3 | vlan55 | 192.168.55.3/29 |
| SW4 | lo0 | 1.1.4.4/32 |
| SW4 | vlan101 | 10.123.101.4/29 |
| SW4 | e1/0 | 10.101.101.1/31 |
| SW4 | e1/1 | 10.101.101.11/31 |
| SW5 | lo0 | 1.1.5.5/32 |
| SW5 | vlan101 | 10.123.101.5/29 |
| SW5 | e1/0 | 10.101.101.9/31 |
| SW5 | e1/1 | 10.101.101.3/31 |
| VPC1 | NIC | 192.168.55.4/29 |
| VPC7 | NIC | 192.168.55.5/29 |

**R12**
```
configure terminal
hostname R12
interface Loopback0
 ip address 1.1.12.12 255.255.255.255
 ipv6 address 2002:101::12:12/128
!
interface Ethernet0/0
 ip address 10.101.101.0 255.255.255.254
!
interface Ethernet0/1
 ip address 10.101.101.2 255.255.255.254
!
interface Ethernet0/2
 ip address 10.101.101.4 255.255.255.254
 ipv6 address FE80::12 link-local
 ipv6 address 2001:DB8:1001:1001::1/127
!
interface Ethernet0/3
 ip address 10.101.101.6 255.255.255.254
 ipv6 address FE80::12 link-local
 ipv6 address 2001:DB8:1001:1001::8/127
```

**R13**
```
configure terminal
hostname R13
interface Loopback0
 ip address 1.1.13.13 255.255.255.255
 ipv6 address 2002:101::13:13/128
!
interface Ethernet0/0
 ip address 10.101.101.8 255.255.255.254
!
interface Ethernet0/1
 ip address 10.101.101.10 255.255.255.254
!
interface Ethernet0/2
 ip address 10.101.101.12 255.255.255.254
 ipv6 address FE80::13 link-local
 ipv6 address 2001:DB8:1001:1001::6/127
!
interface Ethernet0/3
 ip address 10.101.101.14 255.255.255.254
 ipv6 address FE80::13 link-local
 ipv6 address 2001:DB8:1001:1001::3/127
```

**R14**
```
configure terminal
hostname R14
interface Loopback0
 ip address 1.1.14.14 255.255.255.255
 ipv6 address 2002:101::14:14/128
!
interface Ethernet0/0
 ip address 10.101.101.5 255.255.255.254
 ipv6 address FE80::14 link-local
 ipv6 address 2001:DB8:1001:1001::/127
!
interface Ethernet0/1
 ip address 10.101.101.15 255.255.255.254
 ipv6 address FE80::14 link-local
 ipv6 address 2001:DB8:1001:1001::2/127
!
interface Ethernet0/2
 ip address 85.75.123.38 255.255.255.252
 ipv6 address 2002:5555::13/127
!
interface Ethernet0/3
 ip address 10.101.101.16 255.255.255.254
 ipv6 address FE80::14 link-local
 ipv6 address 2001:DB8:1001:1001::4/127
```

**R15**
```
configure terminal
hostname R15
interface Loopback0
 ip address 1.1.15.15 255.255.255.255
 ipv6 address 2002:101::15:15/128
!
interface Ethernet0/0
 ip address 10.101.101.13 255.255.255.254
 ipv6 address FE80::15 link-local
 ipv6 address 2001:DB8:1001:1001::7/127
!
interface Ethernet0/1
 ip address 10.101.101.7 255.255.255.254
 ipv6 address FE80::15 link-local
 ipv6 address 2001:DB8:1001:1001::9/127
!
interface Ethernet0/2
 ip address 85.75.123.34 255.255.255.252
 ipv6 address 2002:5555::15/127
!
interface Ethernet0/3
 ip address 10.101.101.18 255.255.255.254
 ipv6 address FE80::15 link-local
 ipv6 address 2001:DB8:1001:1001::11/127
```

**R19**
```
configure terminal
hostname R19
interface Loopback0
 ip address 1.1.19.19 255.255.255.255
 ipv6 address 2002:101::19:19/128
!
interface Ethernet0/0
 ip address 10.101.101.17 255.255.255.254
 ipv6 address FE80::19 link-local
 ipv6 address 2001:DB8:1001:1001::5/127
```

**R20**
```
configure terminal
hostname R20
interface Loopback0
 ip address 1.1.20.20 255.255.255.255
 ipv6 address 2002:101::20:20/128
!
interface Ethernet0/0
 ip address 10.101.101.19 255.255.255.254
 ipv6 address FE80::20 link-local
 ipv6 address 2001:DB8:1001:1001::10/127
```

**SW4**
```
configure terminal
hostname SW4
vlan 101
name MNGT
exit
vlan 55
name LAN
exit
interface eth0/2
switchport trunk encapsulation dot1q
switchport mode trunk
channel-group 1 mode active
no shutdown
exit
interface eth0/3
switchport trunk encapsulation dot1q
switchport mode trunk
channel-group 1 mode active
no shutdown
exit
interface Port-channel1
switchport trunk encapsulation dot1q
switchport mode trunk
exit
interface eth0/0
switchport trunk encapsulation dot1q
switchport mode trunk
no shutdown
exit
interface eth0/1
switchport trunk encapsulation dot1q
switchport mode trunk
shutdown
exit
interface eth1/1
no switchport
ip address 10.101.101.11 255.255.255.254
no shutdown
exit
interface eth1/0
no switchport
ip address 10.101.101.1 255.255.255.254
no shutdown
exit
interface lo0
ip address 1.1.4.4 255.255.255.255
no shutdown
exit
interface Vlan101
no shutdown
ip address 10.123.101.4 255.255.255.248
vrrp 101 ip 10.123.101.1
vrrp 101 priority 120
vrrp 101 preempt
exit
interface Vlan55
no shutdown
ip address 192.168.55.3 255.255.255.248
vrrp 55 ip 192.168.55.1
vrrp 55 priority 100
exit
```
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_11/images/sw4_vrrp.png)

**SW5**
```
configure terminal
hostname SW5
vlan 101
name MNGT
exit
vlan 55
name LAN
exit
interface eth0/2
switchport trunk encapsulation dot1q
switchport mode trunk
channel-group 1 mode active
no shutdown
exit
interface eth0/3
switchport trunk encapsulation dot1q
switchport mode trunk
channel-group 1 mode active
no shutdown
exit
interface Port-channel1
switchport trunk encapsulation dot1q
switchport mode trunk
exit
interface eth0/0
switchport trunk encapsulation dot1q
switchport mode trunk
no shutdown
exit
interface eth0/1
switchport trunk encapsulation dot1q
switchport mode trunk
shutdown
exit
interface eth1/1
no switchport
ip address 10.101.101.3 255.255.255.254
no shutdown
exit
interface eth1/0
no switchport
ip address 10.101.101.9 255.255.255.254
no shutdown
exit
interface lo0
ip address 1.1.5.5 255.255.255.255
no shutdown
exit
interface Vlan101
no shutdown
ip address 10.123.101.5 255.255.255.248
vrrp 101 ip 10.123.101.1
vrrp 101 priority 100
interface Vlan55
no shutdown
ip address 192.168.55.2 255.255.255.248
vrrp 55 ip 192.168.55.1
vrrp 55 priority 120
vrrp 55 preempt
exit
```
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_11/images/sw5_vrrp.png)

**SW2**
```
configure terminal
hostname SW2
vlan 101
name MNGT
exit
vlan 55
name LAN
exit
hostname SW2
interface eth0/0
switchport trunk encapsulation dot1Q
switchport mode trunk
switchport trunk all vlan 55,101
no shutdown
exit
interface eth0/1
switchport trunk encapsulation dot1Q
switchport mode trunk
switchport trunk all vlan 55,101
no shutdown
exit
interface eth0/2
switchport mode access
switchport access vlan 55
spanning-tree portfast
no shutdown
exit
interface vlan101
ip address 10.123.101.2 255.255.255.248
no shutdown
exit
```

**SW3**
```
configure terminal
hostname SW3
vlan 101
name MNGT
exit
vlan 55
name LAN
exit
hostname SW2
interface eth0/0
switchport trunk encapsulation dot1Q
switchport mode trunk
switchport trunk all vlan 55,101
no shutdown
exit
interface eth0/1
switchport trunk encapsulation dot1Q
switchport mode trunk
switchport trunk all vlan 55,101
no shutdown
exit
interface eth0/2
switchport mode access
switchport access vlan 55
spanning-tree portfast
no shutdown
exit
interface vlan101
ip address 10.123.101.3 255.255.255.248
no shutdown
exit
```

  * На коммутаторах SW2 и SW3 включим протокол STP и зададим глобально режим - **spanning-tree mode rapid-pvst**. На портах eth0/1 обоих комутаторов изменим стоимость портов - **spanning-tree cost 101**  для того, что порты eth0/1 заблокировались по STP.  

**SW2**
```
SW2#show spanning-tree                       

VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    32769
             Address     aabb.cc00.2000
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.2000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/3               Desg FWD 100       128.4    Shr 
Et1/0               Desg FWD 100       128.5    Shr 
Et1/1               Desg FWD 100       128.6    Shr 
Et1/2               Desg FWD 100       128.7    Shr 
Et1/3               Desg FWD 100       128.8    Shr 


          
VLAN0055
  Spanning tree enabled protocol rstp
  Root ID    Priority    32823
             Address     aabb.cc00.2000
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32823  (priority 32768 sys-id-ext 55)
             Address     aabb.cc00.2000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Desg FWD 100       128.1    Shr 
Et0/1               Back BLK 101       128.2    Shr 
Et0/2               Desg FWD 100       128.3    Shr Edge 


          
VLAN0101
  Spanning tree enabled protocol rstp
  Root ID    Priority    32869
             Address     aabb.cc00.2000
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32869  (priority 32768 sys-id-ext 101)
             Address     aabb.cc00.2000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Desg FWD 100       128.1    Shr 
Et0/1               Back BLK 101       128.2    Shr 

SW2#show spanning-tree interface Et0/1

Vlan                Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
VLAN0055            Back BLK 101       128.2    Shr 
VLAN0101            Back BLK 101       128.2    Shr 
SW2#
```

**SW3**
```
SW3#show spanning-tree 

VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    32769
             Address     aabb.cc00.3000
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.3000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/3               Desg FWD 100       128.4    Shr 
Et1/0               Desg FWD 100       128.5    Shr 
Et1/1               Desg FWD 100       128.6    Shr 
Et1/2               Desg FWD 100       128.7    Shr 
Et1/3               Desg FWD 100       128.8    Shr 


          
VLAN0055
  Spanning tree enabled protocol rstp
  Root ID    Priority    32823
             Address     aabb.cc00.2000
             Cost        100
             Port        1 (Ethernet0/0)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32823  (priority 32768 sys-id-ext 55)
             Address     aabb.cc00.3000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Root FWD 100       128.1    Shr 
Et0/1               Altn BLK 101       128.2    Shr 
Et0/2               Desg FWD 100       128.3    Shr Edge 


          
VLAN0101
  Spanning tree enabled protocol rstp
  Root ID    Priority    32869
             Address     aabb.cc00.2000
             Cost        100
             Port        1 (Ethernet0/0)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32869  (priority 32768 sys-id-ext 101)
             Address     aabb.cc00.3000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Root FWD 100       128.1    Shr 
Et0/1               Altn BLK 101       128.2    Shr 

SW3#show spanning-tree interface Et0/1

Vlan                Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
VLAN0055            Altn BLK 101       128.2    Shr 
VLAN0101            Altn BLK 101       128.2    Shr 
SW3#
```

  * Проверка работы с VPCs  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_11/images/vpc1.png)
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_11/images/vpc7.png)
