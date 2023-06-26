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
  * На S1 настроим Gi0/0 и Gi0/2 в качестве портов доступа и свяжим их с VLAN 10
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S1_acc.png) 
  * На S2 настроим порт доступа Gi0/2 и свяжим его с VLAN 10
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_22/images/S2_acc.png)
3. Безопасность неиспользуемых портов коммутатора

4. Документирование и реализация функций безопасности порта

**Конфигурация безопасности порта по умолчанию**  
| Функция | Настройка по умолчанию |
 --- | --- |
| Защита портов | ** |
| Максимальное количество записей MAC-адресов | ** |
| Режим проверки на нарушение безопасности | ** |
| Aging Time | ** |
| Aging Type | ** |
| Secure Static Address Aging | ** |
| Sticky MAC Address | ** |

5. Реализуем безопасность DHCP snooping
6. Реализeем PortFast и BPDU Guard
