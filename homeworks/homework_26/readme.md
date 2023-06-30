# Настройка протокола OSPFv2 для одной области

### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_26/images/topology.png)

### Таблица адресации
| Устройство | Интерфейс | IP-адрес | Маска подсети |
--- | --- | --- | --- |
| R1 | Fa0/0 | 10.53.0.1 | 255.255.255.248 |
|  | Lo1 | 172.16.1.1 | 255.255.255.0 |
| R2 | Fa0/0 | 10.53.0.2 | 255.255.255.0 |
|  | Lo1 | 192.168.1.1 | 255.255.255.0 |

1. Создание сети и настройка основных параметров устройства
  * Произведем базовую настройку маршрутизаторов, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_26/images/R1.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_26/images/R2.png)
  * Настроим базовые параметры каждого коммутатора, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_26/images/S1.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_26/images/S2.png)

2. Настройка и проверка базовой работы протокола OSPFv2 для одной области
  * Выполним настройки интерфейсов и базового OSPFv2 на каждом маршрутизаторе, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_26/images/R1_ip.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_26/images/R2_ip.png)
  * Убедимся, что OSPFv2 работает между маршрутизаторами, выолнив команду, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_26/images/R1_sh.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_26/images/R2_sh.png)
  * На R1 выполним команду **show ip route ospf**, чтобы убедиться, что сеть R2 Loopback1 присутствует в таблице маршрутизации, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_26/images/R1_sh2.png)
  * - *Какой маршрутизатор является DR? Какой маршрутизатор является BDR? Каковы критерии отбора?*
    - *Маршрутизатор R2 является DR, маршрутизатор R1 является BDR, критерии выбора DR (Designated Router) назначается маршрутизатор с наивысшим приоритетом, если приоритеты равны, то выбирается маршрутизатор с наивысшим router-id, в нашем случае у R2 router-id выше.*
  * Запустим Ping до адреса интерфейса R2 Loopback 1 из R1, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_26/images/R1_ping.png)

3. Оптимизация и проверка конфигурации OSPFv2 для одной области
  * Реализуем различных оптимизаций на каждом маршрутизаторе, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_26/images/R1_ospf.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_26/images/R2_ospf.png)
  * Убедимся, что оптимизация OSPFv2 реализовалась, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_26/images/R1_ospf2.png)
  * На R1 выполним команду **show ip route ospf**, чтобы убедиться, что сеть R2 Loopback1 присутствует в таблице маршрутизации, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_26/images/R1_ospf3.png)
  * Выполним команду **show ip route ospf** на маршрутизаторе R2 и убедимя, что единственная информация о маршруте OSPF должна быть распространяемый по умолчанию маршрут R1, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_26/images/R2_ospf2.png)
  * Запустим Ping до адреса интерфейса R1 Loopback 1 из R2, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_26/images/R1_ping2.png)
