# Основы проектирования сети
### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_03/images/topology.JPG)

### Таблица адресации
| Устройство | Интерфейс | IP-адрес/Маска подсети |
--- | --- | --- |
| DC1-SPINE-1 | Eth1 | 192.168.3.1/24 |
|  | Eth2 | 192.168.4.1/24 |
|  | Eth3 | N/A |
|  | Loopback1 | N/A |
|  | Loopback99 | N/A |
| DC1-SPINE-2 | Eth1 | 192.168.3.1/24 |
|  | Eth2 | 192.168.4.1/24 |
|  | Eth3 | N/A |
|  | Loopback1 | N/A |
|  | Loopback99 | N/A |


### Таблица VLAN
| VLAN | Имя | Назначенный интерфейс |
--- | --- | --- |
| 3 | Management | S1: Ge0/2 |
| 4 | Operations | S2: Ge0/2 |
| 7 | ParkingLot | S1: Ge1/1-3 |
| 7 | ParkingLot | S2: Ge0/0, Ge1/1-3 |
| 8 | Native | N/A |
