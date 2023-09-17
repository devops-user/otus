### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_02/images/topology.png)

### Таблица адресации
| Устройство | Интерфейс | IP-адрес/Маска подсети | Шлюз по умолчанию |
--- | --- | --- | --- |
| R1 | Ge0/0/1.3 | 192.168.3.1/24 | - |
|  | Ge0/0/1.4 | 192.168.4.1/24 | - |
|  | Ge0/0/1.8 | N/A | - |
| S1 | vlan 3 | 192.168.3.11/24 | 192.168.3.1 |
| S2 | vlan 3 | 192.168.3.12/24 | 192.168.3.1 |
| PC-A | NIC | 192.168.3.3/24 | 192.168.3.1 |
| PC-B | NIC | 192.168.4.3/24 | 192.168.4.1 |

### Таблица VLAN
| VLAN | Имя | Назначенный интерфейс |
--- | --- | --- |
| 3 | Management | S1: Fa0/6 |
| 4 | Operations | S2: Fa0/18 |
| 7 | ParkingLot | S1: Fa0/2-4, Fa0/7-24, Ge0/1-2 |
| 7 | ParkingLot | S2: Fa0/2-17, Fa0/19-24, Ge0/1-2 |
| 8 | Native | N/A |

# Создание сети и настройка основных параметров устройства
1. Создадим сеть согласно топологии.
  * Подключим устройства, как показано в топологии.
2. Настроим базовые параметры для маршрутизатора, как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/R1.png)
3. Настроим базовые параметры каждого коммутатора, как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/S1.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/S2.png)
4. Настроим узлы ПК, как показано на рисунке ниже.  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/PC-A.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/PC-B.png)
