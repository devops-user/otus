# Конфигурация безопасности коммутатора

### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/topology.PNG)

### Таблица адресации
| Устройство | Интерфейс/vlan | IP-адрес/Маска подсети |
--- | --- | --- |
| R1 | Fa0/0 | 192.168.10.1/24 |
|  | Loopback 0 | 10.10.1.1/24 |
| S1 | vlan 10 | 192.168.10.201/24 |
| S2 | vlan 10 | 192.168.10.202/24 |
| PC-A | NIC | DHCP | 255.255.255.0 |
| PC-B | NIC | DHCP | 255.255.255.0 |

# Настройка основного сетевого устройства
1. Настроим и проверим текущую конфигурацию маршрутизатор R1 с помощью команды **show ip interface brief**, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/R1.png)
2. Настроим и проверим основные параметры коммутаторов, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S1.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S2.png)

# Настройка VLAN на коммутаторах
1. Настроим VLAN, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S1_vlan.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S2_vlan.png)

# Настройки безопасности коммутатора
1. Релизация магистральных соединений 802.1Q
  * Настроим все магистральные порты Gi0/1 на обоих коммутаторах для использования VLAN 333 в качестве native VLAN, убедимся, что режим транкинга успешно настроен на всех коммутаторах с помощью команды **show interface trunk**, отключим согласование DTP Gi0/1 и проверим с помощью команды **show interfaces Gi0/1 switchport | include Negotiation**, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S1_trunk.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S2_trunk.png)
2. Настройка портов доступа
  * На S1 настроим Gi0/0 и Gi0/2 в качестве портов доступа и свяжим их с VLAN 10, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S1_acc.png) 
  * На S2 настроим порт доступа Gi0/2 и свяжим его с VLAN 10, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S2_acc.png)
3. Безопасность неиспользуемых портов коммутатора
  * На S1 и S2 переместим неиспользуемые порты из VLAN 1 в VLAN 999 и отключим неиспользуемые порты, убедимся, что неиспользуемые порты отключены и связаны с VLAN 999, введя команду **show interfaces status**, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S1_sh.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S2_sh.png)
4. Документирование и реализация функций безопасности порта
  * На S1, введем команду **show port-security interface Gi0/2**  для отображения настроек по умолчанию безопасности порта для интерфейса Gi0/2 и запишем ответы ниже в таблицу, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S1_ps.png)

**Конфигурация безопасности порта по умолчанию**  
| Функция | Настройка по умолчанию |
 --- | --- |
| Защита портов | *Disabled* |
| Максимальное количество записей MAC-адресов | *1* |
| Режим проверки на нарушение безопасности | *Shutdown* |
| Aging Time | *0 mins* |
| Aging Type | *Absolute* |
| Secure Static Address Aging | *Disabled* |
| Sticky MAC Address | *0* |

  * На S1 включим защиту порта на Gi0/2 со следующими настройками, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S1_ps2.png)
  * Проверим функции безопасности порта Gi0/2 на S1, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S1_ps3.png)
  * Включим безопасность порта Gi0/2 на S2 со следующими параметрами безопасности, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S2_ps.png)
  * Проверим функции безопасности порта Gi0/2 на S2, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S2_ps2.png)

5. Реализуем безопасность DHCP snooping
  * На S2 включим DHCP snooping и настроим DHCP snooping во VLAN 10, настроим магистральный порт на S2 как доверенный порт, ограничим ненадежный порт Gi0/2 на S2 пятью DHCP-пакетами в секунду и проверим DHCP Snooping на S2, как показано на рисунке ниже:  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S2_dhcp.png)
  * Проверим привязку отслеживания DHCP с помощью команды **show ip dhcp snooping binding**, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S2_dhcp2.png)
6. Реализeем PortFast и BPDU Guard
  * Настроим PortFast на всех портах доступа, включим защиту BPDU на портах доступа VLAN 10 S1 и S2, убедимся, что защита BPDU и PortFast включены на соответствующих портах, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S1_bpdu.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S2_bpdu.png)
7. Проверим наличие сквозного ⁪подключения между всеми устройствами
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/PC-A.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/PC-B.png)

## Вопросы для повторения
1.	С точки зрения безопасности порта на S2, почему нет значения таймера для оставшегося возраста в минутах, когда было сконфигурировано динамическое обучение - sticky?
  * *Нет значения таймера, потому что при использовании функции sticky-learning, mac-adddress будет выучен коммутатором самостоятельно и занесен в конфиг.*
2.	Что касается безопасности порта на S2, если вы загружаете скрипт текущей конфигурации на S2, почему порту Gi0/2 на PC-B никогда не получит IP-адрес через DHCP?
  * *Странный вопрос, PC-B получит IP-адрес через DHCP, проверил.*
3.	Что касается безопасности порта, в чем разница между типом абсолютного устаревания и типом устаревание по неактивности?
  * *Absolute - устанавливает время старения mac-address равное настроеному значению на порту (в нашем случае 60 минут);*
  * *Inactivity - устанавливает время старения mac-address, только если данный клиент неактивен.*
