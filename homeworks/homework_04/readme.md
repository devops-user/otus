# Создание и настройка сети

1. Создание и подключиние сети в соответствии с топологией. Как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_04/images/base_net.png)

2. Настроим IP-адреса на каждом устройстве согласно приведенной ниже таблице.  

| Устройство | Интерфейс | IP-адрес/маска |
--- | --- | ---
| S1 | vlan 1 | 192.168.1.11/24 |
| S2 | vlan 1 | 192.168.1.12/24 |
| PC-A | NIC | 192.168.1.1/24 |
| PC-B | NIC | 192.168.1.2/24 |

  * **PC-A**  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_04/images/PC_A.png)

  * **PC-B**  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_04/images/PC_B.png)

  * **S1**  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_04/images/S1.png)

  * **S2**  
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_04/images/S2.png)

3. Настроим базовые параметры каждого коммутатора
