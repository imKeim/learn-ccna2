
---

> **ВАЖНО**
> 
> Форма для ответов на вопросы будет доступна только при развертывании лабораторной работы 

---

## Топология

![topology](./assets/topology.png)

## Таблица адресации

| **Устройство** | **Интерфейс** | **IPv6-адрес**           |
|------------|-----------|----------------------|
| R1         | G0/0/0    | 2001:db8:acad:2።1/64 |
| R1         | G0/0/0    | fe80::1              |
| R1         | G0/0/1    | 2001:db8:acad:1።1/64 |
| R1         | G0/0/1    | fe80::1              |
| R2         | G0/0/0    | 2001:db8:acad:2።2/64 |
| R2         | G0/0      | fe80::2              |
| R2         | G0/0/1    | 2001:db8:acad:3።1/64 |
| R2         | G0/0/1    | fe80::1              |
| PC-A       | NIC       | DHCP                 |
| PC-B       | NIC       | DHCP                 |

## Задачи

Часть 1. Создать сеть и настроить основные параметры устройства.

Часть 2. Проверить назначить адреса SLAAC от R1.

Часть 3. Настроить и проверить сервер DHCPv6 без сохранения состояния на R1.

Часть 4. Настроить и проверить состояние DHCPv6 сервера на R1.

Часть 5. Настроить и проверить DHCPv6 Relay на R2.

## Общие сведения и сценарий

Динамическое назначение глобальных индивидуальных IPv6-адресов можно настроить тремя способами:

- автоматическая конфигурация адреса без сохранения состояния (Stateless Address Autoconfiguration, SLAAC);
- DHCPv6 без отслеживания состояния;
- адресация DHCPv6 с учетом состояний.

При использовании SLACC для назначения IPv6-адресов хостам сервер DHCPv6 не используется. Поскольку DHCPv6 сервер не используется при реализации SLACC, хосты не могут получать дополнительную важную сетевую информацию, включая адрес сервера доменных имен (DNS), а также имя домена.

При использовании Stateless DHCPv6 для назначения  IPv6-адресов хосту сервер DHCPv6 используется для назначения дополнительной важной информации о сети, однако адрес назначается с помощью SLACC.

При использовании DHCPv6 с отслеживанием состояния, сервер DHCP назначает всю информацию, включая IPv6-адрес узла.

Определение способа получения динамической IPv6-адресации зависит от установленных значений флагов, содержащихся в объявлениях роутера (сообщениях RA).

В предложенном сценарии размеры компании увеличились, и сетевые администраторы больше не имеют возможности назначать IP-адреса для устройств вручную. Ваша задача – настроить роутер R2 для назначения адресов IPv6 в двух разных подсетях, подключенных к R1.

**Примечание.** Роутеры, используемые в практических лабораторных работах CCNA, – это Cisco 4221 с Cisco IOS XE Release 16.9.4 (образ universalk9), а коммутаторы – Cisco Catalyst 2960 с Cisco IOS версии 15.2(2) (образ lanbasek9). Можно использовать другие устройства и версии Cisco IOS. В зависимости от модели устройства и версии Cisco IOS доступные команды и результаты их выполнения могут отличаться от тех, которые показаны в лабораторных работах. Правильные идентификаторы интерфейса смотрите в сводной таблице в конце этой работы.

**Примечание.** Убедитесь, что у всех устройств была удалена начальная конфигурация. Если вы не уверены в этом, обратитесь к инструктору.

## Необходимые ресурсы

-   2 роутера (Cisco 4221 с универсальным образом Cisco IOS XE версии 16.9.4 или аналогичным).
-   2 коммутатора (Cisco 2960 с образом lanbasek9 Cisco IOS версии 15.2 (2) или аналогичным) – **опционально**.
-   2 ПК (ОС Windows с программой эмуляции терминалов, такой как Tera Term).
-   Консольные кабели для настройки устройств Cisco IOS через консольные порты.
-   Кабели Ethernet, расположенные в соответствии с топологией.

## Инструкции

### Часть 1. Создание сети и настройка основных параметров устройства

В первой части лабораторной работы вам предстоит создать топологию сети и настроить базовые параметры для узлов ПК и коммутаторов.

**Шаг 1. Создайте сеть согласно топологии**

Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.

**Шаг 2. Настройте базовые параметры каждого коммутатора (необязательно)**

1.  Присвойте коммутатору имя устройства.
2.  Отключите поиск DNS, чтобы предотвратить попытки роутера преобразовывать введенные команды так, будто они являются именами узлов.
3.  Назначьте **class** в качестве зашифрованного пароля привилегированного режима EXEC.
4.  Назначьте **cisco** в качестве пароля консоли и включите вход в систему по паролю.
5.  Назначьте **cisco** в качестве пароля VTY и включите вход в систему по паролю.
6.  Зашифруйте открытые пароли.
7.  Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.
8.  Отключите все неиспользуемые порты.
9.  Сохраните текущую конфигурацию в файл загрузочной конфигурации.

**Шаг 3. Произведите базовую настройку роутеров**

1.  Назначьте роутеру имя устройства.
2.  Отключите поиск DNS, чтобы предотвратить его попытки преобразовывать введенные команды так, будто они являются именами узлов.
3.  Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
4.  Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
5.  Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.
6.  Зашифруйте открытые пароли.
7.  Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.
8.  Активация IPv6-маршрутизации
9.  Сохраните текущую конфигурацию в файл загрузочной конфигурации.

**Шаг 4. Настройка интерфейсов и маршрутизации для обоих роутеров**

1.  Настройте интерфейсы G0/0/0 и G0/1 на R1 и R2 с адресами IPv6, указанными в таблице выше.
2.  Настройте маршрут по умолчанию на каждом роутере, который указывает на IP-адрес G0/0/0 на другом устройстве.
3.  Убедитесь, что маршрутизация работает с помощью пинга адреса G0/0/1 R2 из R1
4.  Сохраните текущую конфигурацию в файл загрузочной конфигурации.

### Часть 2. Проверка назначения адреса SLAAC от R1

В этой части вы убедитесь, что узел PC-A получает IPv6-адрес с помощью метода SLAAC.

Включите PC-A и удостоверьтесь, что сетевой адаптер настроен для автоматической настройки IPv6.

Через несколько минут результаты команды **ipconfig** должны показать, что PC-A присвоил себе адрес из сети 2001:db8:1::/64.

```
C:\Users\Student> ipconfig 
Настройка IP для Windows

Ethernet adapter Ethernet 2: 

   Connection-specific DNS Suffix . : 
   IPv6 Address. . . . . . . . . . . : 2001:db8:acad: 1:5 c43:ee7c:2959:da68
   Temporary IPv6 Address. . . . . . : 2001:db8:acad: 1:3 c64:e4f 9:46 e 1:1 f23
   Link-local IPv6-адрес. . . . .: fe80። 5c43:ee7c:2959:да 68% 6
   IPv4-адрес. . . . . . . . . . . : 169.254.218.104
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Default Gateway . . . . . . . . .: fe80።1%6
```

Откуда взялась часть адреса с идентификатором хоста?

**Введите ваш ответ здесь**

### Часть 3. Настройка и проверка сервера DHCPv6 на R1

В этой части выполняется настройка и проверка состояния DHCP-сервера на R1. Цель состоит в том, чтобы предоставить PC-A информацию о DNS-сервере и домене.

**Шаг 1. Более подробно изучите конфигурацию PC-A**

1.  Выполните команду **ipconfig /all** на PC-A и посмотрите на результат.

```
C:\Users\Student> ipconfig /all
Windows IP Configuration

   Host Name . . . . . . . . . . . . : НАСТОЛЬНАЯ 3FR7RKA
   Primary Dns Suffix . . . . . . . : 
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No

Ethernet adapter Ethernet0:

   Connection-specific DNS Suffix . : 
   Description . . . . . . . . . . . : Intel(R) 852574L Gigabit Network Connection 
   Physical Address. . . . . . . . . : 00-50-56-83-63-6D
   IPv6 Address. . . . . . . . . . . : 2001:db8:acad:1:5c43:ee7c:2959:da68(Preferred)
   Temporary IPv6 Address. . . . . . : 2001:db8:acad:1:3c64:e4f9:46e1:1f23(Preferred)
   Link-local IPv6-адрес. . . . . : fe80::5c43:ee7c:2959:da68%6(Preferred)
   IPv4 Address. . . . . . . . . . . : 169.254.218.104(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Шлюз по умолчанию . . . . . . . . .: fe80።1%6
   DHCPv6 IAID . . . . . . . . . . . : 50334761
   DHCPv6 Client DUID.  . . . . . . . : 00-01-00-01-24-F5-CE-A2-00-50-56-B3-63-6D
   DNS-серверы . . . . . . . . . . . : fec0:0:0:ffff::1%1
                                       fec0:0:0:ffff::2%1
                                       fec0:0:0:ffff::3%1
   NetBIOS over Tcpip. . . . . . . . : Enabled
```

2.  Обратите внимание, что основной DNS-суффикс отсутствует. Также обратите внимание, что предоставленные адреса DNS-сервера являются адресами локального сайта anycast, а не одноадресные адреса, как ожидалось.

**Шаг 2. Настройте R1 для предоставления DHCPv6 без состояния для PC-A**

1.  Создайте пул DHCP IPv6 на R1 с именем R1-STATELESS. В составе этого пула назначьте адрес DNS-сервера как 2001:db8:acad: :1, а имя домена — как stateless.com.

```
R1(config)# ipv6 dhcp pool R1-STATELESS
R1(config-dhcp)# dns-server 2001:db8:acad::254
R1(config-dhcp)# domain-name STATELESS.com
```

2.  Настройте интерфейс G0/0/1 на R1, чтобы предоставить флаг конфигурации OTHER для локальной сети R1 и укажите только что созданный пул DHCP в качестве ресурса DHCP для этого интерфейса.

```
R1(config)# interface g0/0/1
R1(config-if)# ipv6 nd other-config-flag 
R1(config-if)# ipv6 dhcp server R1-STATELESS
```

3.  Сохраните текущую конфигурацию в файл загрузочной конфигурации.
4.  Перезапустите PC-A.
5.  Проверьте вывод **ipconfig /all** и обратите внимание на изменения.

```
C:\Users\Student> ipconfig /all
Windows IP Configuration 

   Host Name . . . . . . . . . . . . : DESKTOP-3FR7RKA
   Primary Dns Suffix . . . . . . . : 
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No
   DNS Suffix Search List. . . . . . : STATELESS.com

Ethernet adapter Ethernet0:

   Connection-specific DNS Suffix . : STATELESS.com
   Описание . . . . . . . . . . . : Intel(R) 82574L Gigabit Network Connection
   Physical Address. . . . . . . . . : 00-50-56-83-63-6D
   DHCP Enabled. . . . . . . . . . . : Yes
   Autoconfiguration Enabled . . . . : Yes
   IPv6 Address. . . . . . . . . . . : 2001:db8:acad:1:5c43:ee7c:2959:da68(Preferred)
   Temporary IPv6 Address. . . . . . : 2001:db8:acad:1:3c64:e4f9:46e1:1f23(Preferred)
   Link-local IPv6-адрес. . . . . : fe80::5c43:ee7c:2959:da68%6(Preferred)
   IPv4 Address. . . . . . . . . . . : 169.254.218.104(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Default Gateway . . . . . . . . .: fe80።1%6
   DHCPv6 IAID . . . . . . . . . . . : 50334761
   DHCPv6 Client DUID. . . . . . . . : 00-01-00-01-24-F5-CE-A2-00-50-56-B3-63-6D
   DNS Servers . . . . . . . . . . . : 2001:db8:acad። 254
   NetBIOS over Tcpip. . . . . . . . : Enabled
   Список поиска DNS-суффиксов подключения: 
                                       STATELESS.com
```

6.  Протестируйте подключение с помощью пинга IP-адреса интерфейса G0/1 R2.

### Часть 4. Настройка сервера DHCPv6 с сохранением состояния на R1

В этой части настраивается R1 для ответа на запросы DHCPv6 от локальной сети на R2.

1.  Создайте пул DHCPv6 на R1 для сети 2001:db8:acad:3:aaa::/80. Это предоставит адреса локальной сети, подключенной к интерфейсу G0/0/1 на R2. В составе пула задайте DNS-сервер 2001:db8:acad: :254 и задайте доменное имя STATEFUL.com.

```
R1(config)# ipv6 dhcp pool R2-STATEFUL
R1(config-dhcp)# address prefix 2001:db8:acad:3:aaa::/80
R1(config-dhcp)# dns-server 2001:db8:acad::254
R1(config-dhcp)# domain-name STATEFUL.com
```

2.  Назначьте только что созданный пул DHCPv6 интерфейсу g0/0/0 на R1.

```
R1(config)# interface g0/0/0
R1(config-if)# ipv6 dhcp server R2-STATEFUL
```

### Часть 5. Настройка и проверка ретрансляции DHCPv6 на R2.

В пятой части необходимо настроить и проверить ретрансляцию DHCPv6 на R2, позволяя PC-B получать адрес IPv6.

**Шаг 1. Включите PC-B и проверьте адрес SLAAC, который он генерирует**

```
C:\Users\Student> ipconfig /all
Windows IP Configuration

   Host Name . . . . . . . . . . . . : DESKTOP-3FR7RKA
   Primary Dns Suffix . . . . . . . : 
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No

Ethernet adapter Ethernet0:

   Connection-specific DNS Suffix . : 
   Description . . . . . . . . . . . : Intel(R) 82574L Gigabit Network Connection
   Physical Address. . . . . . . . . : 00-50-56-B3-7B-06
   DHCP Enabled. . . . . . . . . . . : Yes
   Autoconfiguration Enabled . . . . : Yes
   IPv6 Address. . . . . . . . . . . : 2001:db8:acad:3:a0f3:3d39:f9fb:a020(Preferred) 
   Temporary IPv6 Address. . . . . . : 2001:db8:acad:3:d4f3:7b16:eeee:b2b5(Preferred) 
   Link-local IPv6 address. . . . . : fe80::a0f3:3d39:f9fb:a020%6(Preferred) 
   IPv4 Address. . . . . . . . . . . : 169.254.160.32(Preferred) 
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Default Gateway . . . . . . . . .: fe80።1%6
   DHCPv6 IAID . . . . . . . . . . . : 50334761
   DHCPv6 Client DUID. . . . . . . . : 00-01-00-01-24-F2-08-38-00-50-56-B3-7B-06
   DNS Servers . . . . . . . . . . . : fec0:0:0:ffff::1%1
                                       fec0:0:0:ffff::2%1
                                       fec0:0:0:ffff::3%1
   NetBIOS over Tcpip. . . . . . . . : Enabled
```

Обратите внимание на вывод, в котором используется префикс 2001:db8:acad:3::

**Шаг 2. Настройте R2 в качестве агента DHCP-ретрансляции для локальной сети на G0/0/1**

1.  Настройте команду **ipv6 dhcp relay** на интерфейсе R2 G0/0/1, указав адрес назначения интерфейса G0/0/0 на R1. Также настройте команду **managed-config-flag**.

```
R2(config)# interface g0/0/1
R2(config-if)# ipv6 nd managed-config-flag
R2(config-if)# ipv6 dhcp relay destination 2001:db8:acad:2::1 g0/0/0
```

2.  Сохраните конфигурацию.

**Шаг 3. Попытайтесь получить IPv6-адрес из DHCPv6 на PC-B**

1.  Перезапустите PC-B.
2.  Откройте командную строку на PC-B, выполните команду **ipconfig /all** и проверьте выходные данные, чтобы увидеть результаты операции ретрансляции DHCPv6.

```
C:\Users\Student> ipconfig /all
Windows IP Configuration

   Host Name . . . . . . . . . . . . : DESKTOP-3FR7RKA
   Primary Dns Suffix . . . . . . . : 
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No
   DNS Suffix Search List. . . . . . : STATEFUL.com

Ethernet adapter Ethernet0:

   Connection-specific DNS Suffix . : STATEFUL.com
   Description . . . . . . . . . . . : Intel(R) 852574L Gigabit Network Connection
   Physical Address. . . . . . . . . : 00-50-56-B3-7B-06
   DHCP Enabled. . . . . . . . . . . : Yes
   Autoconfiguration Enabled . . . . : Yes
   IPv6 Address. . . . . . . . . . . : 2001:db8:acad3:aaaa:7104:8b7d:5402(Preferred)
   Lease Obtained. . . . . . . . . . : Sunday, October 6, 2019 3:27:13 PM
   Lease Expires . . . . . . . . . . Tuesday, October 8, 2019 3:27:13 PM
   Link-local IPv6-адрес. . . . . : fe80::a0f3:3d39:f9fb:a020%6(Preferred)
   IPv4 Address. . . . . . . . . . . : 169.254.160.32(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Default Gateway . . . . . . . . .: fe80። 2% 6
   DHCPv6 IAID . . . . . . . . . . . : 50334761
   DHCPv6 Client DUID. . . . . . . . : 00-01-00-01-24-F2-08-38-00-50-56-B3-7B-06
   DNS Servers . . . . . . . . . . . : 2001:db8:acad። 254
   NetBIOS over Tcpip. . . . . . . . : Включен
   Список поиска DNS-суффиксов подключения:
                                       STATEFUL.com
```

3.  Проверьте подключение с помощью пинга IP-адреса интерфейса R0 G0/0/1.
