### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_12/images/topology.png)

### Таблица адресации
| Устройство | Интерфейс | IP-адрес/Маска подсети | Шлюз по умолчанию |
--- | --- | --- | --- |
| R1 | G0/0/1 | 192.168.1.1/24 | - |
| S1 | vlan 1 | 192.168.1.11/24 | 192.168.1.1 |
| PC-A | NIC | 192.168.1.3/24 | 192.168.1.1 |
_________________________________________________________________________________
# Настройка основных параметров устройств
1. Настроим маршрутизатор, как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_12/images/R1_cfg.png)
2. Настроим компьютер PC-A, как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_12/images/PC-A_cfg.png)
3. Проверка подключения к сети.
  * Отправим с PC-A команду **ping** на маршрутизатор R1, как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_12/images/ping.png)

# Настройка маршрутизатора для доступа по протоколу SSH
1. Настроим аутентификацию устройства, зададим домен для устройства и создадим ключ шифрования с указанием его длины, так же включим для протокола ssh поддержку 2-й версии, как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_12/images/R1_key.png)
2. Создадим имя пользователя в локальной базе учетных записей, как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_12/images/R1_login.png)
3. Активируем протокол SSH на линиях VTY, как показано на рисунке ниже.
  * Активируем протоколы **Telnet** и **SSH** на входящих линиях VTY и изменим способ входа в систему таким образом, чтобы использовалась проверка пользователей по локальной базе учетных записей.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_12/images/R1_vty.png)
4. Сохраним текущую конфигурацию в файл загрузочной конфигурации с помощью команды - **copy running-config startup-config**
5. Установим соединение с маршрутизатором по протоколу SSH, как показано на рисунке ниже.
  * С PC-A установим SSH-подключение к R1:
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_12/images/PC-A_ssh.png)

# Настройка коммутатора для доступа по протоколу SSH
1. Настроим основные параметры коммутатора, как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_12/images/S1_cfg.png)
2. Настроим коммутатор для соединения по протоколу SSH, как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_12/images/S1_ssh.png)
3. Установим соединение с коммутатором по протоколу SSH, как показано на рисунке ниже.
  * С PC-A установим подключение по протоколу SSH к интерфейсу SVI коммутатора S1.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_12/images/S1_connect.png)
  * Удалось ли вам установить SSH-соединение с коммутатором? - *Да, соединение установлено успешно.*

# Настройка протокола SSH с использованием интерфейса командной строки (CLI) коммутатора
1. Посмотриv доступные параметры для клиента SSH в Cisco IOS, как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_12/images/S1_ssh_view.png)
2. Установим с коммутатора S1 соединение по протоколу SSH с маршрутизатором R1, как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_12/images/S1_ssh_R1.png)
  * Какие версии протокола SSH поддерживаются при использовании интерфейса командной строки? - *Внрсии 1 и 2*
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_12/images/S1_ssh_version.png)
__________________________________________________________________________________________________
### Как предоставить доступ к сетевому устройству нескольким пользователям, у каждого из которых есть собственное имя пользователя?
  * Для каждого пользователя можно создать отдельную пару login/password, на примере как показано на рисунке ниже. Так же можно использовать централизованные системы контроля доступа, такие как - RADIUS и TACACS.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_12/images/users.png)
