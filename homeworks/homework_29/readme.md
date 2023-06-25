# Настройка и проверка расширенных списков контроля доступа.

### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/topo.png)

### Таблица адресации
| Устройство | Интерфейс/vlan | IP-адрес/Маска подсети | Шлюз по умолчанию |
--- | --- | --- | --- |
| R1 | Ge0/0/1 | - | - |
|  | Ge0/0/1.20 | 10.20.0.1/24 | - |
|  | Ge0/0/1.30 | 10.30.0.1/24 | - |
|  | Ge0/0/1.40 | 10.40.0.1/24 | - |
|  | Ge0/0/1.1000 | - | - |
|  | Loopback 0 | 172.16.1.1/24 | - |
| R2 | Ge0/0/1 | 10.20.0.4 | - |
| S1 | vlan 20 | 10.20.0.2/24 | 10.20.0.1 |
| S2 | vlan 20 | 10.20.0.3/24 | 10.20.0.1 |
| PC-A | NIC | 10.30.0.10/24 | 10.30.0.1 |
| PC-B | NIC | 10.40.0.10/24 | 10.40.0.1 |

### Таблица VLAN
| VLAN | Имя | Назначенный интерфейс |
--- | ---| --- |
| 20 | Management | S2: Fa0/5 |
| 30 | Operations | S1: Fa0/6 |
| 40 | Sales | S1: Fa0/18 |
| 999 | ParkingLot | S1: Fa0/2-4, Fa0/7-24, Ge0/1-2 |
|  | ParkingLot | S2: Fa0/2-4, Fa0/6-17, Fa0/19-24, Ge0/1-2 |
| 1000 | Собственная | - |

## Создание сети и настройка основных параметров устройства
1. Произведем базовую настройку маршрутизаторов, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/R1.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/R2.png)
3. Настроим базовые параметры каждого коммутатора, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/S1.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/S2.png)

## Назначение сети VLAN соответствующим интерфейсам коммутатора
1. Создадим сети VLAN на коммутаторах
  * Создадим необходимые VLAN, настроим интерфейс управления и шлюз по умолчанию на каждом коммутаторе, назначим все неиспользуемые порты коммутаторов VLAN ParkingLot, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/S1_vlan.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/S2_vlan.png)

2. Назначим сети VLAN соответствующим интерфейсам коммутатора
  * Назначим используемые порты соответствующей VLAN и выполните команду **show vlan brief** для проверки, как показано на рисунке:  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/S1_vlan2.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/S2_vlan2.png)

## Настройка транков (магистральные каналы)
1. Вручную настроим магистральный интерфейс Fa0/1
  * Изменим режим порта, установим для native vlan значение 1000, укажим, что VLAN 10, 20, 30 и 1000 разрешены в транке и выполним команду **show interfaces trunk** для проверки портов магистрали, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/S1_trunk.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/S2_trunk.png)
2. Вручную настроим магистральный интерфейс Fa0/5 на коммутаторе S1
  * Настроим интерфейс S1 Fa0/5 с теми же параметрами транка, что и Fa0/1,	выполним команду **show interfaces trunk** для проверки настроек транка, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/S1_trunk2.png)

## Настройка маршрутизации
1. Настроим маршрутизацию между сетями VLAN на R1
  * Настроим подинтерфейсы для каждой VLAN, настроим интерфейс Loopback 1 на R1, с помощью команды **show ip interface brief** проверим конфигурацию подынтерфейса, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/R1_vlan.png)
2. Настроим интерфейс R2 g0/0/1, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/R2_ip.png)

## Настройка удаленного доступа
1. Настроим все сетевые устройства для базовой поддержки SSH
  * Создадим локального пользователя, в качестве доменного имени используем **ccna-lab.com**, сгенерируем криптоключи и настроим первые пять линий VTY на каждом устройстве, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/R1_ssh.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/R2_ssh.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/S1_ssh.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/S2_ssh.png)
2. Включим защищенные веб-службы с проверкой подлинности на R1
  * Не поддерживается в CPT

## Проверка подключения
1. Настроим узлы ПК, как показано на рисунке:  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/PC-A_ip.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/PC-B_ip.png)
2. Выполним следующие тесты, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/PC-A_test.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/PC-B_test.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_29/images/PC-B_test2.png)

## Настройка и проверка списков контроля доступа (ACL)
