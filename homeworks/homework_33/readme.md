# Настройка NAT для IPv4

### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_33/images/topo.png)

### Таблица адресации
| Устройство | Интерфейс | IP-адрес | Маска подсети |
--- | --- | --- | --- |
| R1 | Fa0/0 | 209.165.200.230 | 255.255.255.248 |
|  | Fa0/1 | 192.168.1.1 | 255.255.255.0 |
| R2 | Fa0/0 | 209.165.200.225 | 255.255.255.248 |
|  | Lo1 | 209.165.200.1 | 255.255.255.224 |
| S1 | vlan 1 | 192.168.1.11 | 255.255.255.0 |
| S2 | vlan 1 | 192.168.1.12 | 255.255.255.0 |
| PC-A | NIC | 192.168.1.2 | 255.255.255.0 |
| PC-B | NIC | 192.168.1.3 | 255.255.255.0 |

1. Создание сети и настройка основных параметров устройства
  * Произведем базовую настройку маршрутизаторов, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_33/images/R1.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_33/images/R2.png)
  * Настроим базовые параметры каждого коммутатора, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_33/images/S1.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_33/images/S2.png)

1. Настройка и проверка NAT для IPv4

2. Настройка и проверка PAT для IPv4

3. Настройка и проверка статического NAT для IPv4
