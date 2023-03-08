# Настройка базовой конфигурации коммутатора.

## 1. Создание сети и проверка настроек коммутатора по умолчанию

1. Для базовой конфигурации устрайства необходимо использовать консольный кабель, что мы и сделаем. Соединим порт RS-232 компьютера  с консольным портом  коммутатора. Как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/PC_SW_console.png)
2. С помощь программы эмуляции терминала на компьютере, установим соедине с коммутатором через консольный кабель. Настройки программы эмуляции терминала компьютера показаны на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/Terminal_cfg.png)
3. При первом подключении к коммутатору мы попадаем в Пользовательский режим, о чем свидетельствует символ "**>**". Как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/mode_1.png)
4. Для проверки начальной конфигурации коммутатора, нам надо перейти в Привилегированный режим набрав слово "**enable**", символ "**>**" изменится на символ "**#**". Как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/mode_2.png)

5. Проверим начальную конфигурацию коммутатора (используется модель Cisco 2960-24TT) с помощью команды "**show running-config**".
   * На коммутаторе - 24 FastEthernet порта;
   * На коммутаторе - 2 GigabitEthernet порта;
   * На коммутаторе - от 0 до 15 vty-линий (всего 16 vty-линий).

6. Изучим характеристики SVI для VLAN 1 с помощью комманды "**show interface vlan 1**". Как показано на рисунке ниже.
   * На интерфейс vlan 1 не назначен ip-адрес;
   * MAC-адрес интерфейса vlan 1 - **0001.c970.34e3**;
   * Данный интерфейс vlan 1 - выключен и находится в сосотоянии "**administratively down**".
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/vlan_1.png)

7. Изучим IP-свойства интерфейса SVI сети VLAN 1 с помощью комманды "**show ip interface vlan 1**". Как показано на рисунке ниже.
   * Видим, что интерфейс vlan 1 находится в состоянии "**Down**" и не имеет ip-адреса.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/vlan_1_1.png)

8. Соединим Ethernet-кабелем FastEthernet порт на компьюетере и FastEthernet0/6 порт коммутатора. Как показано на рисунке ниже.
   * IP-свойства интерфейса сети VLAN 1, не изменились.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/FastEthernet_SW_PC.png)

9. Изучим сведения о версии ОС Cisco IOS на коммутаторе с помощью комманды "**show version**". Как показано на рисунке ниже.
   * Базовый MAC-адрес - *00:01:C9:70:34:E3*;
   * Коммутатор работает под ОС IOS версии - *15.0(2)SE4*;
   * Образ системы называется - *C2960-LANBASEK9-M*.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/SW_version.png)

10. Изучим свойства по умолчанию интерфейса FastEthernet0/6 с помощью команды "**show interface Fastethernet0/6**". Как показано на рисунке ниже.
   * Данный интерфейс включен и находится в состоянии "**Up**";
   * Для того чтобы включить интерфейс, необходимо в Привилегированном режиме ввести команду "**configure terminal**" и попасть в режим глобальной конфигурации, затем надо зайти на нужный нам интерфейс командой "**intrerface FastEthernet0/6**" и выполнить команду "**no shutdown**". Как показано на рисунке.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/FastEthernet_6_no_shut.png)
   * MAC-адрес интерфейса - *0009.7cbe.6206*;
   * Настройки скорости и дуплекса - *Full-duplex, 100Mb/s*.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/FastEthernet_6.png)

11. Изучим флеш-память с помощью команды "**show flash**". Как показано на рисунке ниже.
   * Имя присвоеное образу Cisco IOS - *2960-lanbasek9-mz.150-2.SE4*
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/show_flash.png)

## 2. Настройка базовых параметров коммутатора

1. Настройм базовые параметры коммутатора. Как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/base_cfg.png)

2. Назначим IP-адреса интерфейсу SVI vlan 1 на коммутаторе и FastEthernet порту компьютера. Как показано на рисунке ниже. Согласно нашей таблицы IP-адресации:

Устройство | Интерфейс | IP-адрес / префикс |
 --- | --- | ---
 S1 | vlan 1 | 192.168.1.2/24 |
 PC | NIC/FastEthernet | 192.168.1.10/24 |
 
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/vlan_1_ip.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/PC_ip.png)
 
3. Доступ через консоль на коммутатор ограничим паролем с помощью команд "**password cisco**" и "**login**". Чтобы консольные сообщения не прерывали выполнение команд, используем параметр "**logging synchronous**".
Как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/console.png)

   * Если попытаться заново подключиться через консоль на наш коммутатор, то он запросит пароль на вход и при переходе в Привилегированный режим также необходимо ввести пароль. Как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/console_2.png)

4. Настроим каналы виртуального соединения для удаленного управления (vty). Настроим 5 виртуальных соединений для этого используем в Привилегированный режим команду "**line vty 0 4**", дальее зададим пароль на доступ командами "**password cisco**" и "**login**" и разрешим подключение по протоколу "**telnet**" командой "**transport input telnet**". Как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/vty.png)
   * Команда **login** - нужна для того, чтобы включить проверку пароля при входе.

## 3. Проверка сетевых подключений

1. Отобразим конфигурацию коммутатора с помощью команды "**show running-config**" и увидим параметры которые мы настроили выше. Как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/check_cfg.png)

2. Проверим параметры VLAN 1 с помощью команды - "**show interface vlan 1**". Как показано на рисунке ниже.
   * Сосотояние vlan 1 - *administratively down*
   * Сосотояние канального протокола vlan 1  - *down*
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/vlan_1_status.png)

3. Протестируем сквозное соединение, отправив эхо-запрос на ip-адрес нашего интерфейс vlan 1 на коммутаторе. Эхо-запрос не будет проходить, т.к. интерфейс vlan 1 - выключен и находится в состоянии - *down*. Как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/ping_1.png)

Для того, чтобы включить наш интерфейс vlan 1, надо выполнить команду - "**no shutdown**" после этого состояние интерфейса vlan 1 перейдет - *Up*. Как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/vlan_1_no_shut.png)

5. Проверим удаленное управление коммутатором S1 по протоколу "**telnet**" с нашего компьютера. Как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/telnet_1.png)

Как видно подключение прошло успешно, необходимо ввести пароль на доступ, после перехода в Привилегированный режим так же необходимо ввести пароль, сохраним нашу конфигурацию с помощью команды "**copy running-config startup-config**". Как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_02/images/telnet_2.png)
