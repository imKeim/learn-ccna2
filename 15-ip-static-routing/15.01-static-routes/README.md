<!-- 15.1.1 -->
## Типы статических маршрутов

Статические маршруты часто используются в сети. Это верно даже при наличии настроенного протокола динамической маршрутизации. Например, организация может настроить статический маршрут по умолчанию для поставщика услуг и объявить его другим корпоративным роутерам с использованием протокола динамической маршрутизации.

Статические маршруты можно настроить для IPv4 и IPv6. Оба этих протокола поддерживают:

* стандартный статический маршрут;
* статический маршрут по умолчанию;
* плавающий статический маршрут;
* суммарный статический маршрут.

Статические маршруты настраиваются с помощью команд глобальной конфигурации **ip route** и **ipv6 route**.

<!-- 15.1.2 -->
## Параметры следующего перехода

При настройке статического маршрута следующий переход может быть идентифицирован по IP-адресу, интерфейсу выхода или использовать оба варианта. В зависимости от того, как указан адрес назначения, создается один из трех типов маршрута.

* **Маршрут следующего перехода** – казывается только IP-адрес следующего перехода.
* **Напрямую подключенный статический маршрут** – указывается только интерфейс выхода маршрутизатора.
* **Полностью заданный статический маршрут** – определены IP-адрес и интерфейс выхода следующего перехода.

<!-- 15.1.3 -->
## Команда статического маршрута IPv4

Настройка статических маршрутов IPv4 выполняется с помощью следующей команды:

```
Router(config)# ip route network-address subnet-mask { ip-address | exit-intf [ip-address]} [distance]
```

**Примечание.** Необходимо настроить параметры **_ip-address_**, **_exit-intf_** или **_ip-address_** и **_exit-intf_**.

В таблице описаны **ip route** параметры команд.

| **Параметр** | **Описание** |
| --- | --- |
| `network-address` | Определяет сетевой адрес назначения IPv4 удаленной сети, чтобы добавить в таблицу маршрутизации. |
| `subnet-mask` | <ul><li>Определяет маску подсети удаленной сети. </li><li>Маска подсети может быть изменена при объединении группы сетей и создает суммарный статический маршрут. </li></ul> |
| `ip-address` | <ul><li>Определяет IPv4-адрес роутера следующего перехода. </li><li>Обычно используется с широковещательными сетями (например, Ethernet). </li><li>Может создать рекурсивный статический маршрут, где роутер выполняет дополнительный поиск, чтобы найти интерфейс выхода. </li></ul> |
| `exit-intf` | <ul><li>Определяет интерфейс выхода для пересылки пакетов. </li><li>Статический маршрут с прямым подключением. </li><li>Обычно используется при подключении к сети в конфигурации «точка-точка». </li></ul> |
| `exit-intf ip-address` | Создает полностью определенный статический маршрут, поскольку он указывает выход и IPv4-адрес следующего перехода. |
| `distance` | <ul><li>Опциональная команда, которая может использоваться для назначения административного расстояния значение от 1 до 255. </li><li>Обычно используется для настройки плавающего статического маршрута, административное расстояние, которое выше, чем динамически изученный маршрут. </li></ul> |

<!-- 15.1.4 -->
## Команда статического маршрута IPv6

Настройка статических маршрутов IPv6 выполняется с помощью следующей команды:

```
Router(config)# ipv6 route ipv6-prefix/prefix-length {ipv6-address | exit-intf [ipv6-address]} [distance]
```

Большинство параметров идентичны IPv4-версии этой команды.

В таблице показаны различные параметры **ipv6 route** команд и их описания.

| **Параметр** | **Описание** |
| --- | --- |
| `ipv6-prefix` | Определяет сетевой адрес назначения IPv6 удаленной сети чтобы добавить в таблицу маршрутизации. |
| `/prefix-length` | Определяет длину префикса удаленной сети. |
| `ipv6-address` | <ul><li>Определяет IPv6-адрес роутера следующего перехода. </li><li>Обычно используется с широковещательными сетями (например, Ethernet)</li><li>Может создать рекурсивный статический маршрут, где роутер выполняет дополнительный поиск, чтобы найти интерфейс выхода. </li></ul> |
| `exit-intf` | <ul><li>Определяет интерфейс выхода для пересылки пакетов. </li><li>Статический маршрут с прямым подключением. </li><li>Обычно используется при подключении к сети в конфигурации «точка-точка». </li></ul> |
| `exit-intf ipv6-address` | Создает полностью определенный статический маршрут, поскольку он указывает выходной интерфейс и IPv6-адрес следующего перехода. |
| `distance` | <ul><li>Опциональная команда, которая может использоваться для назначения административного расстояния в значении от 1 до 255. </li><li>Обычно используется для настройки плавающего статического маршрута, административное расстояние, которое выше, чем динамически изученный маршрут. </li></ul> |

**Примечание.** Для того, чтобы роутер мог осуществлять пересылку пакетов для IPv6, необходимо настроить команду глобальной конфигурации **ipv6 unicast-routing**.

<!-- 15.1.5 -->
## Топология двойного стека

На рисунке приведена топология двойного стека. В настоящее время статические маршруты не настроены для IPv4 или IPv6.

![](./assets/15.1.5.svg)


<!-- 15.1.6 -->
## IPv4 Начальные таблицы маршрутизации

**Таблица IPv4-маршрутизации R1**

```
R1# show ip route | begin Gateway
Gateway of last resort is not set
      172.16.0.0/16 is variably subnetted, 4 subnets, 2 masks
C 172.16.2.0/24 is directly connected, Serial0/1/0
L 172.16.2.1/32 is directly connected, Serial0/1/0
C 172.16.3.0/24 is directly connected, GigabitEthernet0/0/0
L 172.16.3.1/32 is directly connected, GigabitEthernet0/0/0
R1#
```

**Таблица IPv4-маршрутизации R2**

```
R2# show ip route | begin Gateway
Gateway of last resort is not set
      172.16.0.0/16 is variably subnetted, 4 subnets, 2 masks
C 172.16.1.0/24 is directly connected, GigabitEthernet0/0/0
L 172.16.1.1/32 is directly connected, GigabitEthernet0/0/0
C 172.16.2.0/24 is directly connected, Serial0/1/0
L 172.16.2.2/32 is directly connected, Serial0/1/0
      192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.1.0/24 is directly connected, Serial0/1/1
L 192.168.1.2/32 is directly connected, Serial0/1/1
R2#
```

**Таблица IPv4-маршрутизации R3**

```
R3# show ip route | begin Gateway
Gateway of last resort is not set
      192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.1.0/24 is directly connected, Serial0/1/1
L 192.168.1.1/32 is directly connected, Serial0/1/1
      192.168.2.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.2.0/24 is directly connected, GigabitEthernet0/0/0
L 192.168.2.1/32 is directly connected, GigabitEthernet0/0/0
R3#
```

**R1 может пинговать R2**

Ни один роутер не имеет информации о сетях за пределами подключенных к нему интерфейсов. Это означает, что каждое устройство может достигать только непосредственно подключенных сетей, как показано в следующих ping-тестах.

**ping** от R1 к последовательному интерфейсу 0/1/0 R2 должен быть успешным, поскольку это сеть напрямую подключена.

```
R1# ping  172.16.2.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.2.2, timeout is 2 seconds:
!!!!!
```

**R1 не удается выполнить пинг до локальной сети R3**

Это связано с тем, что **ping** в таблице маршрутизации R1 не существует записи для LAN роутера R3.

```
R1# ping 192.168.2.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.2.1, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)
```

<!-- 15.1.7 -->
## Начальные таблицы IPv6-маршрутизации

**Таблица IPv6-маршрутизации R1**

```
R1# show ipv6 route | begin C
C 2001:DB8:ACAD:2::/64 [0/0]
     via Serial0/1/0, directly connected
L 2001:DB8:ACAD:2::1/128 [0/0]
     via Serial0/1/0, receive
C 2001:DB8:ACAD:3::/64 [0/0]
     via GigabitEthernet0/0/0, directly connected
L 2001:DB8:ACAD:3::1/128 [0/0]
     via GigabitEthernet0/0/0, receive
L FF00::/8 [0/0]
     via Null0, receive
R1#
```

**Таблица IPv6-маршрутизации R2**

```
R2# show ipv6 route | begin C 
C 2001:DB8:ACAD:1::/64 [0/0]
     via GigabitEthernet0/0/0, directly connected
L 2001:DB8:ACAD:1::1/128 [0/0]
     via GigabitEthernet0/0/0, receive
C 2001:DB8:ACAD:2::/64 [0/0]
     via Serial0/1/0, directly connected
L 2001:DB8:ACAD:2::2/128 [0/0]
     via Serial0/1/0, receive
C 2001:DB8:CAFE:1::/64 [0/0] 
     via Serial0/1/1, directly connected
L 2001:DB8:CAFE:1::2/128 [0/0]
     via Serial0/1/1, receive
L FF00::/8 [0/0]
     via Null0, receive
R2#
```

**Таблица IPv6-маршрутизации R3**

```
R3# show ipv6 route | begin C 
C 2001:DB8:CAFE:1::/64 [0/0]
     via Serial0/1/1, directly connected
L 2001:DB8:CAFE:1::1/128 [0/0]
     via Serial0/1/1, receive
C 2001:DB8:CAFE:2::/64 [0/0]
     via GigabitEthernet0/0/0, directly connected
Л 2001:DB8:КАФЕ:2: :1/128 [0/0]
     via GigabitEthernet0/0/0, receive
L FF00::/8 [0/0]
     via Null0, receive
R3#
```

**R1 может пинговать R2**

Ни один роутер не имеет информации о сетях за пределами подключенных к нему интерфейсов.

**ping** от R1 к последовательному интерфейсу 0/1/0 на R2 должен быть успешным.

```
R1# ping 2001:db8:acad:2::2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:DB8:ACAD:2::2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/2/3 ms
```

**R1 не удается выполнить ping до локальной сети R3**

Тем не менее, **ping** к локальной сети R3 не удалось. Это связано с тем, что в таблице маршрутизации R1 не существует записи для данной сети.

```
R1# ping 2001:DB8:cafe:2::1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:DB8:CAFE:2::1, timeout is 2 seconds:
% No valid route for destination
Success rate is 0 percent (0/1)
```

<!-- 15.1.8 -->
<!-- quiz -->
