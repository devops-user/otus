# Маршрутизация на основе политик (PBR)

### Топология
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_11/images/topo.png)

  * Настроим два маршрута по-умолчанию на маршрутизаторе офиса Чокурдах в сторону провайдера Триада:
```
ip route 0.0.0.0 0.0.0.0 85.75.123.13 name to_R25
ip route 0.0.0.0 0.0.0.0 85.75.123.17 name to_R26
```

  * Настроим отслеживание линков на маршрутизаторе офиса Чокурдах, ниже на рисунке видим статистику и статус ответа, что всё *ОК*:
```
ip sla 25
 icmp-echo 85.75.123.13 source-ip 85.75.123.14
 frequency 10
ip sla schedule 25 life forever start-time now
ip sla 26
 icmp-echo 85.75.123.17 source-ip 85.75.123.18
 frequency 10
ip sla schedule 26 life forever start-time now
```
![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_12/images/sla_1.png)


  * Погасим интерфейс на маршрутизаторе R26 провайдера Триада, ниже на рисунке увидим статистику и статус ответа:

![](https://github.com/devops-user/otus/blob/main/homeworks_prof/homework_12/images/sla.png)


  * Настрим маршрут по-умолчанию на маршрутизаторе офиса Лабытнанги:
```
ip route 0.0.0.0 0.0.0.0 85.75.123.9 name to_R25
```
