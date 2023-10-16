# Архитектура сети

### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_11/images/topo.png)

### Таблица адресации Триада
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| R23 | e0/0 | 85.75.123.1/30 |
| R23 | e0/1 | 10.123.52.0/31 |
| R23 | e0/2 | 10.123.52.2/31 |
| R23 | lo0 | 1.1.23.23/32 |
| R24 | e0/0 | 85.75.123.5/30 |
| R24 | e0/1 | 10.123.52.4/31 |
| R24 | e0/2 | 10.123.52.3/31 |
| R24 | e0/3 | 85.75.123.25/30 |
| R24 | lo0 | 1.1.24.24/32 |
| R25 | e0/0 | 10.123.52.1/31 |
| R25 | e0/1 | 85.75.123.9/30 |
| R25 | e0/2 | 10.123.52.7/31 |
| R25 | e0/3 | 85.75.123.13/30 |
| R25 | lo0 | 1.1.25.25/32 |
| R26 | e0/0 | 10.123.52.5/31 |
| R26 | e0/1 | 85.75.123.17/30 |
| R26 | e0/2 | 10.123.52.6/31 |
| R26 | e0/3 | 85.75.123.21/30 |
| R26 | lo0 | 1.1.26.26/32 |

**R23**
```
configure terminal
hostname R23
interface eth0/0
ip address 85.75.123.1 255.255.255.252
no shutdown
exit
interface eth0/1
ip address 10.123.52.0 255.255.255.254
no shutdown
exit
interface eth0/2
ip address 10.123.52.2 255.255.255.254
no shutdown
exit
interface lo0
ip address 1.1.23.23 255.255.255.255
no shutdown
exit
```

**R24**
```
configure terminal
hostname R24
interface eth0/0
ip address 85.75.123.5 255.255.255.252
no shutdown
exit
interface eth0/1
ip address 10.123.52.4 255.255.255.254
no shutdown
exit
interface eth0/2
ip address 10.123.52.3 255.255.255.254
no shutdown
exit
interface eth0/3
ip address 85.75.123.25 255.255.255.252
no shutdown
exit
interface lo0
ip address 1.1.24.24 255.255.255.255
no shutdown
exit
```

**R25**
```
configure terminal
hostname R25
interface eth0/0
ip address 10.123.52.1 255.255.255.254
no shutdown
exit
interface eth0/1
ip address 85.75.123.9 255.255.255.252
no shutdown
exit
interface eth0/2
ip address 10.123.52.7 255.255.255.254
no shutdown
exit
interface eth0/3
ip address 85.75.123.13 255.255.255.252
no shutdown
exit
interface lo0
ip address 1.1.25.25 255.255.255.255
no shutdown
exit
```

**R26**
```
configure terminal
hostname R26
interface eth0/0
ip address 10.123.52.5 255.255.255.254
no shutdown
exit
interface eth0/1
ip address 85.75.123.17 255.255.255.252
no shutdown
exit
interface eth0/2
ip address 10.123.52.6 255.255.255.254
no shutdown
exit
interface eth0/3
ip address 85.75.123.21 255.255.255.252
no shutdown
exit
interface lo0
ip address 1.1.26.26 255.255.255.255
no shutdown
exit

```

### Таблица адресации Лабытнанги
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| R27 | e0/0 | 85.75.123.10/30 |
| R27 | lo0 | 1.1.27.27/32 |

**R27**
```
configure terminal
hostname R27
interface eth0/0
ip address 85.75.123.10 255.255.255.252
no shutdown
exit
interface lo0
ip address 1.1.27.27 255.255.255.255
no shutdown
exit
```

### Таблица адресации Чокурдах
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| R28 | e0/0 | 85.75.123.18/30 |
| R28 | e0/1 | 85.75.123.14/30 |
| R28 | e0/2.14 | 10.123.14.1/28 |
| R28 | e0/2.50 | 192.168.50.1/29 |
| R28 | lo0 | 1.1.28.28/32 |
| SW29 | vlan14 -> *Management* | 10.123.14.2/28 |
| VPC30 | NIC | 192.168.50.2/29 |
| VPC31 | NIC | 192.168.50.3/29 |

**R28**
```
configure terminal
hostname R28
interface eth0/0
ip address 85.75.123.18 255.255.255.252
no shutdown
exit
interface eth0/1
ip address 85.75.123.14 255.255.255.252
no shutdown
exit
interface eth0/2
no shutdown
exit
interface eth0/2.14
description Management
encapsulation dot1Q 14
ip address 10.123.14.1 255.255.255.240
exit
interface eth0/2.50
description LAN
encapsulation dot1Q 50
ip address 192.168.50.1 255.255.255.248
exit
interface lo0
ip address 1.1.28.28 255.255.255.255
no shutdown
exit
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
no shutdown
exit
interface eth0/1
switchport mode access
switchport access vlan 50
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
| R21 | e0/1 | 85.75.123.30/30 |
| R21 | e0/2 | 85.75.123.6/30 |
| R21 | lo0 | 1.1.21.21/32 |

**SW21**
```
configure terminal
hostname R21
interface eth0/0
ip address 85.75.123.33 255.255.255.252
no shutdown
exit
interface eth0/1
ip address 85.75.123.30 255.255.255.252
no shutdown
exit
interface eth0/2
ip address 85.75.123.6 255.255.255.252
no shutdown
exit
interface lo0
ip address 1.1.21.21 255.255.255.255
no shutdown
exit
```

### Таблица адресации Киторн
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| R22 | e0/0 | 85.75.123.37/30 |
| R22 | e0/1 | 85.75.123.29/30 |
| R22 | e0/2 | 85.75.123.2/30 |
| R22 | lo0 | 1.1.22.22/32 |

**R22**
```
configure terminal
hostname R22
interface eth0/0
ip address 85.75.123.37 255.255.255.252
no shutdown
exit
interface eth0/1
ip address 85.75.123.29 255.255.255.252
no shutdown
exit
interface eth0/2
ip address 85.75.123.2 255.255.255.252
no shutdown
exit
interface lo0
ip address 1.1.22.22 255.255.255.255
no shutdown
exit
```

### Таблица адресации С.-Петербург
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| R16 | e0/0 | 10.123.42.12/31 |
| R16 | e0/1 | 10.123.42.3/31 |
| R16 | e0/2 | 10.123.42.10/31 |
| R16 | e0/3 | 10.123.42.4/31 |
| R16 | lo0 | 1.1.16.16/32 |
| R17 | e0/0 | 10.123.42.6/31 |
| R17 | e0/1 | 10.123.42.1/31 |
| R17 | e0/2 | 10.123.42.8/31 |
| R17 | lo0 | 1.1.17.17/32 |
| R18 | e0/0 | 10.123.42.2/31 |
| R18 | e0/1 | 10.123.42.0/31 |
| R18 | e0/2 | 85.75.123.26/30 |
| R18 | e0/3 | 85.75.123.22/30 |
| R28 | lo0 | 1.1.18.18/32 |
| R32 | e0/0 | 10.123.42.5/31 |
| R32 | lo0 | 1.1.32.32/32 |
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
interface eth0/0
ip address 10.123.42.12 255.255.255.254
no shutdown
exit
interface eth0/1
ip address 10.123.42.3 255.255.255.254
no shutdown
exit
interface eth0/2
ip address 10.123.42.10 255.255.255.254
no shutdown
exit
interface eth0/3
ip address 10.123.42.4 255.255.255.254
no shutdown
exit
interface lo0
ip address 1.1.16.16 255.255.255.255
no shutdown
exit
```

**R17**
```
configure terminal
hostname R17
interface eth0/0
ip address 10.123.42.6 255.255.255.254
no shutdown
exit
interface eth0/1
ip address 10.123.42.1 255.255.255.254
no shutdown
exit
interface eth0/2
ip address 10.123.42.8 255.255.255.254
no shutdown
exit
interface lo0
ip address 1.1.17.17 255.255.255.255
no shutdown
exit
```

**R18**
```
configure terminal
hostname R18
interface eth0/0
ip address 10.123.42.2 255.255.255.254
no shutdown
exit
interface eth0/1
ip address 10.123.42.0 255.255.255.254
no shutdown
exit
interface eth0/2
ip address 85.75.123.26 255.255.255.252
no shutdown
exit
interface eth0/3
ip address 85.75.123.22 255.255.255.252
no shutdown
exit
interface lo0
ip address 1.1.18.18 255.255.255.255
no shutdown
exit
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
ip address 192.168.242.3 255.255.255.248
vrrp 1 ip 192.168.242.1
vrrp 1 priority 100
```
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_11/images/sw10_vrrp.png)

**R32**
```
configure terminal
hostname R32
interface eth0/0
ip address 10.123.42.5 255.255.255.254
no shutdown
exit
interface lo0
ip address 1.1.32.32 255.255.255.255
no shutdown
exit
```
  * Проверка работы с VPCs  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_11/images/vpc8.png)
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_11/images/vpc.png)


### Таблица адресации Москва
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| R12 | e0/0 | 10.101.101.0/31 |
| R12 | e0/1 | 10.101.101.2/31 |
| R12 | e0/2 | 10.101.101.4/31 |
| R12 | e0/3 | 10.101.101.6/31 |
| R12 | lo0 | 1.1.12.12/32 |
| R13 | e0/0 | 10.101.101.8/31 |
| R13 | e0/1 | 10.101.101.10/31 |
| R13 | e0/2 | 10.101.101.12/31 |
| R13 | e0/3 | 10.101.101.14/31 |
| R13 | lo0 | 1.1.13.13/32 |
| R14 | e0/0 | 10.101.101.5/31 |
| R14 | e0/1 | 10.101.101.15/31 |
| R14 | e0/2 | 85.75.123.38/30 |
| R14 | e0/3 | 10.101.101.16/31 |
| R14 | lo0 | 1.1.14.14/32 |
| R15 | e0/0 | 10.101.101.13/31 |
| R15 | e0/1 | 10.101.101.7/31 |
| R15 | e0/2 | 85.75.123.34/30 |
| R15 | e0/3 | 10.101.101.18/31 |
| R15 | lo0 | 1.1.15.15/32 |
| R19 | e0/0 | 10.101.101.17/31 |
| R19 | lo0 | 1.1.19.19/32 |
| R20 | e0/0 | 10.101.101.19/31 |
| R20 | lo0 | 1.1.20.20/32 |
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
interface eth0/0
ip address 10.101.101.0 255.255.255.254
no shutdown
exit
interface eth0/1
ip address 10.101.101.2 255.255.255.254
no shutdown
exit
interface eth0/2
ip address 10.101.101.4 255.255.255.254
no shutdown
exit
interface eth0/3
ip address 10.101.101.6 255.255.255.254
no shutdown
exit
interface lo0
ip address 1.1.12.12 255.255.255.255
no shutdown
exit
```

**R13**
```
configure terminal
hostname R13
interface eth0/0
ip address 10.101.101.8 255.255.255.254
no shutdown
exit
interface eth0/1
ip address 10.101.101.10 255.255.255.254
no shutdown
exit
interface eth0/2
ip address 10.101.101.12 255.255.255.254
no shutdown
exit
interface eth0/3
ip address 10.101.101.14 255.255.255.254
no shutdown
exit
interface lo0
ip address 1.1.13.13 255.255.255.255
no shutdown
exit
```

**R14**
```
configure terminal
hostname R14
interface eth0/0
ip address 10.101.101.5 255.255.255.254
no shutdown
exit
interface eth0/1
ip address 10.101.101.15 255.255.255.254
no shutdown
exit
interface eth0/2
ip address 85.75.123.38 255.255.255.252
no shutdown
exit
interface eth0/3
ip address 10.101.101.16 255.255.255.254
no shutdown
exit
interface lo0
ip address 1.1.14.14 255.255.255.255
no shutdown
exit
```

**R15**
```
configure terminal
hostname R15
interface eth0/0
ip address 10.101.101.13 255.255.255.254
no shutdown
exit
interface eth0/1
ip address 10.101.101.7 255.255.255.254
no shutdown
exit
interface eth0/2
ip address 85.75.123.34 255.255.255.252
no shutdown
exit
interface eth0/3
ip address 10.101.101.18 255.255.255.254
no shutdown
exit
interface lo0
ip address 1.1.15.15 255.255.255.255
no shutdown
exit
```

**R19**
```
configure terminal
hostname R19
interface eth0/0
ip address 10.101.101.17 255.255.255.254
no shutdown
exit
interface lo0
ip address 1.1.19.19 255.255.255.255
no shutdown
exit
```

**R20**
```
configure terminal
hostname R20
interface eth0/0
ip address 10.101.101.19 255.255.255.254
no shutdown
exit
interface lo0
ip address 1.1.20.20 255.255.255.255
no shutdown
exit
```
