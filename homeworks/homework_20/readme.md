# I. Реализация DHCPv4 

### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/topo.png)

### Таблица адресации
| Устройство | Интерфейс | IP-адрес/Маска подсети | Шлюз по умолчанию |
--- | --- | --- | --- |
| R1 | Ge0/0/0 | 10.0.0.1/30 | - |
|  | Ge0/0/1 | - | - |
|  | Ge0/0/1.100 | $${\color{red}192.168.1.1/26}$$ |  |
|  | Ge0/0/1.200 | $${\color{red}192.168.1.65/27}$$ |  |
|  | Ge0/0/1.1000 | - | - |
| R2 | Ge0/0/0 | 10.0.0.2/30 | - |
|  | Ge0/0/1 | $${\color{red}192.168.1.97/28}$$ | - |
| S1 | vlan 200 | $${\color{red}192.168.1.66/27}$$ | $${\color{red}192.168.1.65}$$ |
| S2 | vlan 1 | $${\color{red}192.168.1.98/28}$$ | $${\color{red}192.168.1.97}$$ |
| PC-A | NIC | DHCP | DHCP |
| PC-B | NIC | DHCP | DHCP |


### Таблица VLAN
| VLAN | Имя | Назначенный интерфейс |
--- | ---| --- |
| 1 | Нет | S2: Fa0/18 |
| 100 | Клиенты | S1: Fa0/6 |
| 200 | Управление | S1: VLAN 200 |
| 999 | Parking_Lot | S1: Fa0/1-4, Fa0/7-24, Ge0/1-2 |
| 1000 | Собственная | - |

# Создание сети и настройка основных параметров устройства
1. Создадим схему адресации и запишем в таблицу выше
  * Подсеть сети 192.168.1.0/24 в соответствии со следующими требованиями:
    * Одна подсеть «Подсеть A», поддерживающая 58 хостов - первый IP-адрес *192.168.1.1/26*
    * Одна подсеть «Подсеть B», поддерживающая 28 хостов - первый IP-адрес *192.168.1.65/27*, второй IP-адрес *192.168.1.66/27*
    * Одна подсеть «Подсеть C», поддерживающая 12 узлов - первый IP-адрес *192.168.1.97/28*
2. Создадим сеть согласно топологии, как на рисунке выше
3. Произведем базовую настройку маршрутизаторов, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/R1.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/R2.png)
4. Настроим маршрутизацию между сетями VLAN на маршрутизаторе R1
  * Активируем интерфейс G0/0/1 и настроим подинтерфейсы для каждой VLAN, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/R1_sub.png) 
5. Настроим G0/0/1 на R2, затем G0/0/0 и статическую маршрутизацию для обоих маршрутизаторов
  * Настроим G0/0/1 на R2 с первым IP-адресом подсети C, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/R2_sub.png)
  * Настроим интерфейс G0/0/0 для каждого маршрутизатора и маршрут по умолчанию на каждом маршрутизаторе, указываемом на IP-адрес G0/0/0 на другом маршрутизаторе, как показано на рисунке ниже: 
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/R1_r2.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/R2_r1.png)
  * Убедимся, что статическая маршрутизация работает с помощью пинга до адреса G0/0/1 R2 от R1, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/R2_ping.png)
6. Настроим базовые параметры каждого коммутатора, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/SW1.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/SW2.png)
7. Создадим сети VLAN на коммутаторе S1
  * Создадим необходимые VLAN и настроим и активируем интерфейс управления на S1, как показано на рисунке ниже:  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/SW1_IP.png)
  * Настроим и активируем интерфейс управления на S2, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/SW2_IP.png)
  * Назначим все неиспользуемые порты S1 VLAN Parking_Lot и административно деактивируем их, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/SW1_sh.png)
  * На S2 административно деактивируем все неиспользуемые порты, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/SW2_sh.png)
8. Назначим сети VLAN соответствующим интерфейсам коммутаторов, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/SW1_fa6.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/SW2_fa5.png)
  * Убедимся, что VLAN назначены на правильные интерфейсы, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/SW1_vl.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/SW2_vl.png)
  * Почему интерфейс F0/5 указан в VLAN 1? - *т.к. мы не используем VLAN'ы и субинтерфейсы на роутере.*
9. Вручную настроим интерфейс S1 F0/5 в качестве транка 802.1Q, как показано на рисунке ниже:
  * Изменим режим порта коммутатора и проверим состояние транка
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/SW1_tr.png)
  * Какой IP-адрес был бы у ПК, если бы он был подключен к сети с помощью DHCP? - т.к. в вопросе нет конкретики, то можно предположить, что *IP-адрес из подсети 192.168.1.0/26, кроме первого IP-адреса, т.к. он используется на марш-ре в качестве шлюза по-умолчанию для ПК.* 

# Настройка и проверка двух серверов DHCPv4 на R1
1. Настроим R1 с пулами DHCPv4 для двух поддерживаемых подсетей и сохраним конфигурацию, как показано на рисунке ниже:
  * *время аренды нельзя настроить в CPT.*
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/R1_ip_dhcp.png)
2. Проверим конфигурацию сервера DHCPv4, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/R1_sh_ip_1.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/R1_sh_ip_2.png)
  * show ip dhcp server statistics - не поддерживается данная команда в CPT
3. Попытаемся получить IP-адрес от DHCP на PC-A
  * IP-адрес успешно получен  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/PC-A_ip.png)

# Настройка и проверка DHCP-ретрансляции на R2
1. Настроим R2 в качестве агента DHCP-ретрансляции для локальной сети на G0/0/1, как показано на рисунке:
  * Настроим команду **ip helper-address** на G0/0/1, указав IP-адрес G0/0/0 R1 и сохраним конфигурацию
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/R2_helper.png)
2. Попытаемся получить IP-адрес от DHCP на PC-B, как показано на рисунке:
  * Выполним команду ipconfig для просмотра информации об IP-адресе и поверим подключение с помощью пинга
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/PC-B_ping.png)
  * Выполним **show ip dhcp binding** для R1 для проверки назначений адресов в DHCP
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/R1_sh_ip_3.png)
______________________________________________________________________________________________________

# II. Настройка DHCPv6

### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/topov6.JPG)

### Таблица адресации
| Устройство | Интерфейс | IPv6-адрес |
--- | --- | --- |
| R1 | Ge0/0/0 | 2001:db8:acad:2::1/64 |
|  | Ge0/0/0 | fe80::1 |
|  | Ge0/0/1 | 2001:db8:acad:1::1/64 |
|  | Ge0/0/1 | fe80::1 |
| R2 | Ge0/0/0 | 2001:db8:acad:2::2/64 |
|  | Ge0/0/0 | fe80::2 |
|  | Ge0/0/1 | 2001:db8:acad:3::1/64 |
|  | Ge0/0/1 | fe80::1 |
| PC-A | NIC | DHCP |
| PC-B | NIC | DHCP |

# Создание сети и настройка основных параметров устройства
1. Произведем базовую настройку маршрутизаторов, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_20/images/R1v6.png)
2. Настроим интерфейсы и маршрутизацию для обоих маршрутизаторов, как показано на рисунке:

# Проверка назначения адреса SLAAC от R1

# Настройка и проверка сервера DHCPv6 на R1
1. Более подробно изучим конфигурацию PC-A, как показано на рисунке:

2. Настроим R1 для предоставления DHCPv6 без состояния для PC-A, как показано на рисунке:

# Настройка сервера DHCPv6 с сохранением состояния на R1

# Настройка и проверка ретрансляции DHCPv6 на R2
1. Включим PC-B и проверим адрес SLAAC, который он генерирует, как показано на рисунке:

2. Настроим R2 в качестве агента DHCP-ретрансляции для локальной сети на G0/0/1, как показано на рисунке:

3. Попытаемся получить адрес IPv6 из DHCPv6 на PC-B
