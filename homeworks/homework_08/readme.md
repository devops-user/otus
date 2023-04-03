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
2. Активируем IPv6-маршрутизацию на R1.
  * В командной строке на PC-B введем команду **ipconfig**, чтобы получить данные IPv6-адреса, назначенного интерфейсу ПК, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/raw/main/homeworks/homework_08/images/PC_B_1.png)
  * Назначен ли индивидуальный IPv6-адрес сетевой интерфейсной карте (NIC) на PC-B - *нет, индивидуальный IPv6-адрес не назначен.*
  * Активируем IPv6-маршрутизацию на R1 с помощью команды **ipv6 unicast-routing**, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/raw/main/homeworks/homework_08/images/router_unicast_r.png)
  * Еще раз введем команду **ipconfig** на PC-B, проверив данные IPv6-адреса, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/raw/main/homeworks/homework_08/images/PC_B_2.png)
  * Почему PC-B получил глобальный префикс маршрутизации и идентификатор подсети - *Сетевые устройства отправляют сообщения router solicitation(RS) для обнаружения соседей IPv6 на многоадресный адрес по умолчанию, который всегда прослушивают все маршрутизаторы. Любой маршрутизатор в сегменте, который его получает, ответит сообщением router advertisement(RA) для обнаружения соседей IPv6. В этом случае устройство использует протокол обнаружения соседей (NDP) IPv6, чтобы определить подсеть /64 для сети. Затем он использует свой собственный MAC-адрес и стандартный алгоритм EUI-64 для создания уникального адреса.*
3. Назначим IPv6-адреса интерфейсу управления (SVI) на S1.
  * Назначим IPv6-адрес для S1, и также назначим этому интерфейсу локальный адрес канала, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/raw/main/homeworks/homework_08/images/S1_ip.png)
  * Проверим правильность назначения IPv6-адресов интерфейсу управления с помощью команды **show ipv6 interface vlan1**, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/raw/main/homeworks/homework_08/images/S1_sh_br_vl.png)
4. Назначим компьютерам статические IPv6-адреса.
  * Откроем окно Свойства Ethernet для каждого ПК и назначим адресацию IPv6 каждому ПК, как показано на рисунке ниже:
![](https://github.com/devops-user/otus/raw/main/homeworks/homework_08/images/PC_A_ip.png)
![](https://github.com/devops-user/otus/blob/main/homeworks/homework_08/images/PC_B_ip.png)
# Проверка сквозного подключения
