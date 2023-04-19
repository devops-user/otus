### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/topology.png)

### Таблица адресации
| Устройство | Интерфейс | IP-адрес/Маска подсети | Шлюз по умолчанию |
--- | --- | --- | --- |
| R1 | Ge0/0/1.10 | 192.168.10.1/24 | - |
|  | Ge0/0/1.20 | 192.168.20.1/24 | - |
|  | Ge0/0/1.30 | 192.168.30.1/24 | - |
|  | Ge0/0/1.1000 | - | - |
| S1 | vlan 10 | 192.168.10.11/24 | 192.168.10.1 |
| S2 | vlan 10 | 192.168.10.12/24 | 192.168.10.1 |
| PC-A | NIC | 192.168.20.3/24 | 192.168.20.1 |
| PC-B | NIC | 192.168.30.3/24 | 192.168.30.1 |

### Таблица VLAN
| VLAN | Имя | Назначенный интерфейс |
--- | --- | --- |
| 10 | Управление | S1:VLAN 10 |
| 10 | Управление | S2:VLAN 10 |
| 20 | Sales | S1:Fa0/6 |
| 30 | Operations | S2:Fa0/18 |
| 999 | Parking_Lot | S1: Fa0/2-4, Fa0/7-24, Ge0/1-2 |
| 999 | Parking_Lot | S2: Fa0/2-17, Fa0/19-24, Ge0/1-2 |

# Создание сети и настройка основных параметров устройства
1. Создадим сеть согласно топологии.
  * Подключиv устройства, как показано в топологии.
2. Настроим базовые параметры для маршрутизатора, как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/R1.png)
3. Настроим базовые параметры каждого коммутатора, как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/S1.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/S2.png)
4. Настроим узлы ПК, как показано на рисунке ниже.  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/PC-A.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/PC-B.png)

# Создание сетей VLAN и назначение портов коммутатора
1. Создадим сети VLAN на коммутаторах.
  * Создадим и назовем необходимые VLAN на каждом коммутаторе из таблицы выше, как показвно на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/vlan_1.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/vlan_1_1.png)
* Настроим интерфейс управления и шлюз по умолчанию на каждом коммутаторе, используя информацию об IP-адресе в таблице адресации, как показвно на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/vlan_10.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/vlan_10_1.png)

* Назначим все неиспользуемые порты коммутатора VLAN Parking_Lot, настроим их для статического режима доступа и административно деактивируйте их, как показвно на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/S1_999.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/S2_999.png)
2. Назначим сети VLAN соответствующим интерфейсам коммутатора, как показвно на рисунке ниже.
  * Назначим используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настроим их для режима статического доступа, как показвно на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/S1_20.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/S2_30.png)
  * Убедимся, что VLAN назначены на правильные интерфейсы с помощью команды **show vlan**, как показвно на рисунке ниже.  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/S1_if.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/S2_if.png)

# Конфигурация магистрального канала стандарта 802.1Q между коммутаторами
1. Вручную настроим магистральный интерфейс F0/1 на коммутаторах S1 и S2, как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/S1_f1.png)
  * Проверим транки, native VLAN и разрешенные VLAN через транк на обоих коммутаторах, как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/S1_tr.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/S2_tr.png)

2. Вручную настроим магистральный интерфейс F0/5 на коммутаторе S1, как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/S1_f5.png)
  * Проверка транкинга.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/S1_tr_5.png)
  * Что произойдет, если G0/0/1 на R1 будет отключен? - *Как можно увидеть на рисунке выше **Operational Mode** находится в состоянии - down*.

# Настройка маршрутизации между сетями VLAN
1. Настроим маршрутизатор, как показано на рисунке ниже.
  * Настроим подинтерфейсы для каждой VLAN, как указано в таблице IP-адресации.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/R1_sub.png)
  * Убедимся, что вспомогательные интерфейсы работают, как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/R1_sh_tr.png)

# Проверка работы маршрутизации между VLAN
1. Выполним следующие тесты с PC-A, как показано на рисунке ниже.
  * Отправим эхо-запрос с PC-A на шлюз по умолчанию.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/PC-A_ping_1.png)
  * Отправим эхо-запрос с PC-A на PC-B.  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/PC-A_ping_2.png)
  * Отправим команду ping с компьютера PC-A на коммутатор S2.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/PC-A_ping_3.png)

2. Пройдем следующий тест с PC-B, как показано на рисунке ниже.
  * В окне командной строки на PC-B выполним команду **tracert** на адрес PC-A.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_14/images/PC-B_tracert.png)
  * Какие промежуточные IP-адреса отображаются в результатах? - *Отображается IP-адрес подинтерфейса на маршрутизаторе, который является шлюзом по-умолчанию для данного PC-B.*
