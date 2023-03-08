## Настройка базовой конфигурации коммутатора.

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
