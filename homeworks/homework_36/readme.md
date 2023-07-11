# Настройка протоколов CDP, LLDP и NTP

### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_36/images/topo.png)

### Таблица адресации
| Устройство | Интерфейс/vlan | IP-адрес/Маска подсети | Шлюз по умолчанию |
--- | --- | --- | --- |
| R1 | Ge0/0/1 | - | - |
|  | Loopback 0 | 172.16.1.1/24 | - |
| R2 | Ge0/0/1 | 10.22.0.1/24 | - |
| S1 | SVI VLAN 1  | 10.22.0.2/24 | 10.22.0.1 |
| S2 | SVI VLAN 1  | 10.22.0.3/24 | 10.22.0.1 |

# Создание сети и настройка основных параметров устройства
1. Настроим базовые параметры для маршрутизатора, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_36/images/R1.png)
2. Настроим базовые параметры каждого коммутатора, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_36/images/S1.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_36/images/S2.png)

# Обнаружение сетевых ресурсов с помощью протокола CDP
  * На R1 используем соответствующую команду **show cdp interface**, чтобы определить, сколько интерфейсов включено CDP, как показано на рисунке:  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_36/images/R1_cdp.png)  
*Сколько интерфейсов участвует в объявлениях CDP? Какие из них активны? - Участвуют все интерфейсы в объявлениях CDP, активен только один интерфейс - FastEthernet0/0*
  * На R1 используйте соответствующую команду **show cdp entry S1**, чтобы определить версию IOS, используемую на S1, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_36/images/R1_cdp2.png)
*Какая версия IOS используется на S1? - Experimental Version 15.1(20130726:213425)*
  * На S1 используйте соответствующую команду **show cdp traffic**, чтобы определить, сколько пакетов CDP было выданных, как показано на рисунке:  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_36/images/S1_cdp.png)  
*Сколько пакетов имеет выход CDP с момента последнего сброса счетчика? - 67 пакетов*
  * Отключим CDP глобально на всех устройствах, с помощью команжы - **no cdp run**
# Обнаружение сетевых ресурсов с помощью протокола LLDP
  * Выполним команду - **lldp run**, чтобы включить LLDP на всех устройствах в топологии, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_36/images/S1_lldp.png)
  * На S1 выполним соответствующую команду - **show lldp entry S2**, чтобы предоставить подробную информацию о S2, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_36/images/S1_lldp2.png)
*Что такое chassis ID  для коммутатора S2? - Это его mac-address*
# Настройка NTP
1. Выведим на экран текущее время с помощью команды **show clock detail** и запишем сведения о текущем времени в таблицу, как показано на рисунке:  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_36/images/R1_clock.png)

| Дата | Время | Часовой пояс | Источник времени |
--- | --- | --- | --- |
| Jul 10 2023 | 18:40:41.483 | UTC | hardware calendar |

2. Установим время с помощью команды **clock set** на маршрутизаторе R1, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_36/images/R1_clock2.png)
3. Настроим R1 в качестве хозяина NTP с уровнем слоя 4, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_36/images/R1_ntp.png)
4. Настроим клиент NTP
  * Выполните соответствующую команду на S1 и S2, чтобы просмотреть настроенное время. Запишите текущее время,  в таблицe, как показано на рисунке:  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_36/images/SW_clock.png)

| Дата | Время | Часовой пояс |
--- | --- | --- |
| Jul 10 2023 | 18:59:51.181 | UTC |
| Jul 10 2023 | 19:00:02.422 | UTC |

5. Проверим настройку NTP
  * Как можем увидеть, синхронизация с маршрутизатором есть, который выступает в качестве NTP-сервера:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_36/images/R1_ntp2.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_36/images/S1_ntp.png)
____________________________________________________________________________________________
*Для каких интерфейсов в пределах сети не следует использовать протоколы обнаружения сетевых ресурсов? - Для интерфейсов, которые смотрят в сторону клиентов, т.к. зная модель оборудования и версию софта, можно использовать эксплоит для атаки на оборудование.*
