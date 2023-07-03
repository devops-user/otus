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

2. Настройка и проверка NAT для IPv4
  * Настроим NAT на R1, используя пул из трех IP-адресов ***209.165.200.226 – 209.165.200.228***, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_33/images/R1_nat.png)
  * Выполним диагностику и проверим конфигурацию, как показано на рисунке:


3. Настройка и проверка PAT для IPv4
  * Удалим команду преобразования на R1 - **no ip nat inside source list 1 pool PUBLIC_ACCESS** и добавим команду PAT на R1 - **ip nat inside source list 1 pool PUBLIC_ACCESS overload**, как показано на рисунке:
  
  * Протестируем и проверим конфигурацию, как показано на рисунке:
  
  * На R1 удалим команды преобразования nat pool - **no ip nat inside source list 1 pool PUBLIC_ACCESS overload** и **no ip nat pool PUBLIC_ACCESS** затем добавим команду PAT overload, указав внешний интерфейс - **ip nat inside source list 1 interface Fa0/0 overload**, как показано на рисунке:

  * Протестируем и проверим конфигурацию, как показано на рисунке:

4. Настройка и проверка статического NAT для IPv4
  * На R1 очистим текущие трансляции и статистику и выполним команду NAT - **ip nat inside source static 192.168.1.2 209.165.200.229**, необходимую для статического сопоставления внутреннего адреса с внешним адресом, как показано на рисунке:
  
  * Протестируем и проверим конфигурацию, как показано на рисунке:

