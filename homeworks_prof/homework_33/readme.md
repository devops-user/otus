  * Настроим NTP сервер на R12 и R13.

**R12**
```
ntp source Loopback0
ntp master 4
ntp update-calendar
```

**R13**
```
ntp source Loopback0
ntp master 5
ntp update-calendar
```

Проверим состояние и статус:  
**R14**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/R14.png)

**R15**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/R15.png)

**R19**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/R19.png)

**R20**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/R20.png)

**SW4**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/SW4.png)

**SW5**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/SW5.png)

**SW3**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/SW3.png)

**SW2**  
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_33/images/SW2.png)
