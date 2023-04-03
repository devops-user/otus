# Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора
1. Настройка топологии и начальная конфигурация маршрутизатора и коммутатора. Как показано на рисунке ниже.
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_08/images/topology.png)
  * Конфигурация основных параметров маршрутизатора. Как показано на рисунке ниже.
![](https://github.com/devops-user/otus/raw/main/homeworks/homework_08/images/base_cfg_router.png)
  * конфигурация основных параметров коммутатора. Как показано на рисунке ниже.
![](https://github.com/devops-user/otus/raw/main/homeworks/homework_02/images/base_cfg.png)

# Ручная настройка IPv6-адресов
Таблица адресации:
| Устройство | Интерфейс | IPv6-адрес/Длинна префикса | Шлюз по умолчанию |
|--- | --- | --- | --- |
| R1 | Ge0/0 | 2001:DB8:ACAD:A::1/64 | - |
| R1 | Ge0/1 | 2001:DB8:ACAD:1::1 /64 | - |
| S1 | vlan 1 | 2001:DB8:ACAD:1::b/64 | - |
| PC-A | NIC | 2001:DB8:ACAD:1::3/64 | FE80::1 |
| PC-B | NIC | 2001:DB8:ACAD:A::3/64 | FE80::1 |

1. Назначим IPv6-адреса интерфейсам Ethernet на R1.
  * Назначим глобальные индивидуальные IPv6-адреса обоим интерфейсам маршрутизатора, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/raw/main/homeworks/homework_08/images/router_ipv6.png)
  * Введем команду **show ipv6 interface brief**, чтобы проверить, назначен ли каждому интерфейсу корректный индивидуальный IPv6-адрес, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/raw/main/homeworks/homework_08/images/router_sh_br.png)
  * Чтобы обеспечить соответствие локальных адресов канала индивидуальному адресу, вручную введем локальные адреса канала на каждом интерфейсе Ethernet на R1, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/raw/main/homeworks/homework_08/images/router_link_local.png)
  * Используем команду **show ipv6 interface brief**, чтобы убедиться, что локальный адрес связи изменен на fe80::1, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/raw/main/homeworks/homework_08/images/router_sh_br_2.png)
3. Активируем IPv6-маршрутизацию на R1.
4. Назначим IPv6-адреса интерфейсу управления (SVI) на S1.
5. Назначим компьютерам статические IPv6-адреса.

# Проверка сквозного подключения
