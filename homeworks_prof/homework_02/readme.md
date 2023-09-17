### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_02/images/topology.png)

### Таблица адресации
| Устройство | Интерфейс | IP-адрес/Маска подсети | Шлюз по умолчанию |
--- | --- | --- | --- |
| R1 | Fa0/0.3 | 192.168.3.1/24 | - |
|  | Fa0/0.4 | 192.168.4.1/24 | - |
|  | Fa0/0.8 | N/A | - |
| S1 | vlan 3 | 192.168.3.11/24 | 192.168.3.1 |
| S2 | vlan 3 | 192.168.3.12/24 | 192.168.3.1 |
| PC-A | NIC | 192.168.3.3/24 | 192.168.3.1 |
| PC-B | NIC | 192.168.4.3/24 | 192.168.4.1 |

### Таблица VLAN
| VLAN | Имя | Назначенный интерфейс |
--- | --- | --- |
| 3 | Management | S1: Ge0/2 |
| 4 | Operations | S2: Ge0/2 |
| 7 | ParkingLot | S1: Ge1/1-3 |
| 7 | ParkingLot | S2: Ge0/0, Ge1/1-3 |
| 8 | Native | N/A |

# Создание сети и настройка основных параметров устройства
1. Создадим сеть согласно топологии.
  * Подключим устройства, как показано на топологии выше.
2. Настроим базовые параметры для маршрутизатора, как показано ниже.
  * R1
```
enable
configure terminal
hostname R1
no ip domain-lookup
enable secret class
line console 0
password cisco
logging synchronous
login
exit
line vty 0 4
password cisco
login
exit
service password-encryption
banner motd #
Only for authorization users. #
end
clock set 20:00:01 15 Sep 2023
copy running-config startup-config
```

3. Настроим базовые параметры каждого коммутатора, как показано ниже.
  * S1
```
enable
configure terminal
hostname S1
no ip domain-lookup
enable secret class
line console 0
password cisco
logging synchronous
login
exit
line vty 0 4
password cisco
login
exit
service password-encryption
banner motd #
Only for authorization users. #
end
clock set 20:00:01 15 Sep 2023
copy running-config startup-config
```
  * S2
```
enable
configure terminal
hostname S2
no ip domain-lookup
enable secret class
line console 0
password cisco
logging synchronous
login
exit
line vty 0 4
password cisco
login
exit
service password-encryption
banner motd #
Only for authorization users. #
end
clock set 20:00:01 15 Sep 2023
copy running-config startup-config
```
4. Настроим узлы ПК, как показано на рисунке ниже.  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_02/images/PC-A.PNG)  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_02/images/PC-B.PNG)

# Создание сетей VLAN и назначение портов коммутатора
1. Создадим сети VLAN на коммутаторах.
* Создадим и назовем необходимые VLAN на каждом коммутаторе из таблицы выше, как показвно ниже.
```
enable
configure terminal
vlan 3
name Management
vlan 4
name Operations
vlan 7
name ParkingLot
vlan 8
name Native
exit
```

* Настроим интерфейс управления и шлюз по умолчанию на каждом коммутаторе, используя информацию об IP-адресе в таблице адресации, как показвно ниже.

* S1
```
enable
configure terminal
interface vlan3
ip address 192.168.3.11 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.3.1
end
```

  * S2
```
enable
configure terminal
interface vlan3
ip address 192.168.4.11 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.4.1
end
```
* Назначим все неиспользуемые порты коммутатора VLAN ParkingLot, настроим их для статического режима доступа и административно деактивируйте их, как показвно ниже.
* S1
```
enable
configure terminal
interface range Gi1/1-3
switchport mode access 
switchport access vlan 7
shutdown
```
* S2
```
enable
configure terminal
interface range Gi0/0, Gi1/1-3
switchport mode access 
switchport access vlan 7
shutdown
```

2. Назначим сети VLAN соответствующим интерфейсам коммутатора, как показвно ниже.
  * Назначим используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настроим их для режима статического доступа, как показвно на рисунке ниже.
* S1
```
enable
configure terminal
interface Gi0/2
description Link to PC-A
switchport mode access 
switchport access vlan 3
no shutdown
```
* S2
```
enable
configure terminal
interface Gi0/2
description Link to PC-B
switchport mode access 
switchport access vlan 4
no shutdown
```

  * Убедимся, что VLAN назначены на правильные интерфейсы с помощью команды **show vlan**, как показвно на рисунке ниже.  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_02/images/S1_vlan.png)
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_02/images/S2_vlan.png)
