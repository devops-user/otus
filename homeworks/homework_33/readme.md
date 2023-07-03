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
  * Выполним диагностику и проверим конфигурацию, запустим эхо-запрос до интерфейса Lo1 (209.165.200.1) с PC-A, PC-B, S2, S1 и на R1 посмотрим таблицу NAT, выполнив команду - ***show ip nat translations***, как можно увидеть по сообщению в консоле, что пула из 3-х IP-адресов не хватило и пинг с S1 не прошел, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_33/images/R1_nat2.png)

3. Настройка и проверка PAT для IPv4
  * Удалим команду преобразования на R1 - ***no ip nat inside source list 1 pool PUBLIC_ACCESS*** и добавим команду PAT на R1 - ***ip nat inside source list 1 pool PUBLIC_ACCESS overload***, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_33/images/R1_no_nat.png)  
  * Протестируем и проверим конфигурацию, запустим эхо-запрос до интерфейса Lo1 (209.165.200.1) с PC-A, PC-B и на R1 посмотрим таблицу NAT, выполнив команду - ***show ip nat translations***, можно заметить, что внутренний глобальный адрес одинаков для обоих сеансови время использования IP-адреса сократилось до 1 минуты, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_33/images/R1_pat.png)
  * *Как маршрутизатор отслеживает, куда идут ответы?* - *Маршрутизатор отслеживает ответы по номеру порта*

  * На R1 удалим команды преобразования nat pool - ***no ip nat inside source list 1 pool PUBLIC_ACCESS overload*** и ***no ip nat pool PUBLIC_ACCESS*** затем добавим команду PAT overload, указав внешний интерфейс - ***ip nat inside source list 1 interface Fa0/0 overload***, как показано на рисунке:  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_33/images/R1_pat2.png)
  * Протестируем и проверим конфигурацию, запустим эхо-запрос до интерфейса Lo1 (209.165.200.1) с PC-A, PC-B и на R1 посмотрим таблицу NAT, выполнив команду - ***show ip nat translations***, как можно увидеть, что теперь все внутренние глобальные адреса сопоставляются с IP-адресом интерфейса Fa0/0 (*209.165.200.230*), как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_33/images/R1_pat3.png)

4. Настройка и проверка статического NAT для IPv4
  * На R1 очистим текущие трансляции и статистику и выполним команду NAT - ***ip nat inside source static 192.168.1.2 209.165.200.229***, необходимую для статического сопоставления внутреннего адреса с внешним адресом, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_33/images/R1_snat.png)  
  * Протестируем и проверим конфигурацию, запустим ping  с R2 на 209.165.200.229, и увидим на R1 статическое сопоставление и преобразование на уровне порта для входящих ping, как показано на рисунке:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_33/images/R1_snat2.png) 
