# Переход от 3-х уровневой топологи в ЦОД к топологии CLOS.

На данном этапе есть сеть (ЦОД остался в наследство) со стандартной 3-х уровневой топологией Core/Distribution/Access, но т.к. это в первую очередь ЦОД, то принято решение переходить на все эти современные штуки в виде EVPN/VxLAN.

На схеме видим стандартную топологию: 
* уровень доступа (L2) - это четыри стека коммутаторов, в каждом стеке по два коммутатора Huawei CE6681-48S6CQ;
* уровень распределения (L2/L3) - это два коммутатора Huawei CE8865-4C;
* уровень ядра (L3) - это два маршрутизатора Huawei NetEngine 8000 M4.
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_16/images/Three_Layer_1.jpg)

И даже с этим как-то можно жить, но к этому добавляются стыки с операторами связи, стыки с удаленными офисами. На уровне ядра/распределения имеем разные ACL (access-lists) для ограничения доступа/связности между хостами. Так же нет целевой схемы включения удаленных офисов.
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_16/images/Three_Layer_2.jpg)

В сторону уровня доступа настроен VRRP, что так же не отвечает требованиям современного ЦОД
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_16/images/vrrp1.png)
![](https://github.com/devops-user/otus/blob/main/homeworks_dc/homework_16/images/vrrp2.png)

В итоге хотим прийдти в топологии CLOS
