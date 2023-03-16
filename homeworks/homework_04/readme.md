# Создание и настройка сети

## 1. Создание и подключиние сети в соответствии с топологией. Как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_04/images/base_net.png)

1. Настроим IP-адреса на каждом устройстве согласно приведенной ниже таблице.  

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

2. Настроим базовые параметры каждого коммутатора. Как показано на рисунке ниже.
  * Настроим имена устройствам S1 и S2
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_04/images/S1_name.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_04/images/S2_name.png)

  * Назначим **cisco** в качестеве пароля доступа через консоль и VTY и **class** в качестве пароля доступа к привилегированному режиму EXEC. Данную процедуру надо выполнить на обоих коммутаторах. Как показано на рисунке ниже.
 ![](https://github.com/devops-user/otus/blob/main/homeworks/homework_04/images/S_base_cfg.png)
 
 # Изучим таблицы МАС-адресов коммутатора
 
 ## 1. Запишем МАС-адреса сетевых устройств.
