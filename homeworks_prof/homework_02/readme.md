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
| 7 | ParkingLot | S1: - |
| 7 | ParkingLot | S2: Ge0/0 |
| 8 | Native | N/A |

# Создание сети и настройка основных параметров устройства
1. Создадим сеть согласно топологии.
  * Подключим устройства, как показано в топологии.
2. Настроим базовые параметры для маршрутизатора, как показано ниже.

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


4. Настроим узлы ПК, как показано на рисунке ниже.  

