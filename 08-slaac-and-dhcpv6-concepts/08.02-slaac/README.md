<!-- 8.2.1 -->
## Обзор SLAAC

Не каждая сеть имеет доступ к серверу DHCPv6. Но каждое устройство в сети IPv6 нуждается в GUA. Метод SLAAC позволяет хостам создавать свой собственный уникальный глобальный одноадресный IPv6 без использования служб DHCPv6 сервера.

**SLAAC** — это служба без схранения состояния. Это означает, что нет сервера, который хранит информацию о сетевых адресах, чтобы узнать, какие IPv6-адреса используются и какие из них доступны.

SLAAC использует ICMPv6-сообщения запроса роутера и его объявления, чтобы предоставить данные об адресации и другую информацию о конфигурации, обычно предоставляемую DHCP-сервером. Хост настраивает свой IPv6-адрес на основе информации, отправляемой в RА. Сообщения RA отправляются роутером IPv6 каждые 200 секунд.

Хост также может отправить сообщение **Router Solicitation** (RS) с запросом о том, чтобы роутер с поддержкой IPv6 передал хосту RA.

SLAAC может быть развернут только как SLAAC, или SLAAC с DHCPv6.

<!-- 8.2.2 -->
## Включение SLAAC

Обратитесь к следующей топологии, чтобы увидеть, как SLAAC включен для предоставления динамического распределения GUA без изменения состояния.

![](./assets/8.2.2.svg)


<!--
Показывает топологию с роутером, подключенным к коммутатору, соединенному с хост-компьютером
-->

Предположим, что R1 GigabitEthernet 0/0/1 настроен с указанными IPv6 GUA и локальными адресами канала. Нажмите каждую кнопку, чтобы объяснить, как R1 включен для SLAAC.

**Проверка адресов IPv6**

Выходные данные команды **show ipv6 interface** отображают текущие настройки интерфейса G0/0/1.

Как было подчеркнуто, R1 были назначены следующие адреса IPv6:

* **локальный адрес канала IPv6** — fe80::1;
* **GUA и подсеть адрес канала IPv6** — 2001:db8:acad:1::1 и 2001:db8:acad:1::/64;
* **группа всех узлов IPv6** — ff02::1.

```
R1# show ipv6 interface G0/0/1
GigabitEthernet0/0/1 is up, line protocol is up
  IPv6 is enabled, link-local address is FE80::1
  No Virtual link-local address(es):
  Description: Link to LAN
  Global unicast address(es):
    2001:DB8:ACAD:1::1, subnet is  2001:DB8:ACAD:1::/64
  Joined group address(es):
    FF02::1
    FF02::1:FF00:1
(дальше выходные данные опущены)
R1#
```

**Активация IPv6-маршрутизации**

Несмотря на то, что интерфейс роутера имеет конфигурацию IPv6, он все еще не включен для отправки RAs, содержащих сведения о конфигурации адресов, на узлы, использующие SLAAC.

Чтобы включить отправку сообщений RA, он должен присоединиться к группе IPv6 all-routers с помощью команды **ipv6 unicast-routing** в режиме глобальной конфигурации, как показано в выходных данных.

```
R1(config)# ipv6 unicast-routing
R1(config)# exit
R1# 
```

**Убедитесь, что SLAAC включен**

Сообщение RS отправляется на IPv6-адрес многоадресной рассылки FF02::2, который поддерживают все роутеры. Вы можете использовать команду **show ipv6 interface**, чтобы проверить, включен ли роутер, как показано на выходных данных.

Устройство Cisco с поддержкой IPv6 отправляет сообщения RA на адрес многоадресной рассылки всех узлов IPv6 ff02::1 каждые 200 секунд.

```
R1# show ipv6 interface G0/0/1 | section Joined
  Joined group address(es):
    FF02::1
    FF02::2
    FF02::1:FF00:1
R1#
```

<!-- 8.2.3 -->
## Использование SLAAC по умолчанию

Метод SLAAC только включен по умолчанию при настройке команды **ipv6 unicast-routing**. Все включенные интерфейсы Ethernet с настроенным GUA IPv6 начнут отправлять сообщения RA с флагом A равным 1, а флаги O и M — 0, как показано на рисунке.

 Флаг **A = 1** предлагает клиенту создать свой собственный IPv6 GUA, используя префикс, объявленный в RA. Клиент может создать свой собственный идентификатор интерфейса, используя метод Extended Unique Identifier (EUI-64) или случайно сгенерированный.

Флаги **O = 0** и **M = 0** указывают клиенту использовать информацию исключительно в сообщении RA. Сюда входит информация о префиксе, длине префикса, DNS-сервере, MTU и информация о шлюзе по умолчанию. Далее клиент не получает никакой информации от сервера DHCPv6.

![](./assets/8.2.3.svg)


<!--
Показано, что при SLAAC только у роутера установлен флаг A в 1 
-->

В примере PC1 включен для автоматического получения информации об IPv6-адресации. Из-за настроек флагов **A**, **O** и **M** PC1 выполняет только SLAAC, используя информацию, содержащуюся в сообщении RA, отправленном R1.

**Адрес шлюза по умолчанию** — это адрес источника IPv6 сообщения RA, который является LLA для R1. Он может быть получен только динамически из сообщения RA. Сервер DHCPv6 не предоставляет эту информацию.

```
C:\PC1> ipconfig
Windows IP Configuration
Ethernet adapter Ethernet0:
   Connection-specific DNS Suffix  . : 
   IPv6 Address. . . . . . . . . . . : 2001:db8:acad:1:1de9:c69:73ee:ca8c
   Link-local IPv6 Address . . . . . : fe80::fb:1d54:839f:f595%21
   IPv4 Address. . . . . . . . . . . : 169.254.202.140
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Default Gateway . . . . . . . . . : fe80::1%6
C:\PC1>
```

<!-- 8.2.4 -->
## Сообщения RS ICMPv6

Роутер отправляет RA-сообщения каждые 200 секунд. Тем не менее, он также отправит сообщение RA, если он получит сообщение RS от хоста.

Когда клиент настроен на получение информации об адресации автоматически, он отправляет сообщение RS на адрес многоадресной рассылки всех роутеров IPv6 ff02: :2.

На рисунке показан ПК, отправляющий сообщение RS на адрес многоадресной рассылки всех роутеров и получающий RA в ответ.

![](./assets/8.2.4.svg)


1.  PC1 только что загрузился и еще не получил сообщение RA. Таким образом, он отправляет сообщение RS на адрес многоадресной рассылки IPv6 всех роутеров ff02: :2 с запросом RA.
2.  R1 входит в группу всех роутеров IPv6 и получает сообщение RS. Он генерирует RA, содержащий префикс локальной сети и длину префикса (например, 2001:db8:acad:1::/64). Затем он отправляет его на адрес многоадресной рассылки всех узлов IPv6 ff02::1. PC1 использует эту информацию для создания уникального GUA IPv6.


<!-- 8.2.5 -->
## Процесс для создания идентификатора интерфейса

Используя SLAAC, хост обычно получает информацию о 64-битной подсети IPv6 от роутера RA. Однако он должен генерировать оставшийся 64-битный идентификатор интерфейса (ID) одним из двух методов.

* **Генерация случайным образом** — 64-битный ID может быть случайным числом, сгенерированным операционной системой клиента. Этот метод теперь используется хостами Windows 10.
* **EUI-64** — хост создает идентификатор интерфейса, используя свой 48-битный MAC-адрес и вставляет шестнадцатеричное значение fffe в середине адреса. Некоторые операционные системы по умолчанию используют случайно сгенерированный идентификатор интерфейса вместо метода EUI-64, из-за проблем конфиденциальности. Это связано с тем, что MAC-адрес узла Ethernet используется EUI-64 для создания идентификатора интерфейса.

**Примечание.** Windows, Linux и Mac OS позволяют пользователю изменять генерирование идентификатора интерфейса либо случайным образом, либо использовать EUI-64.

Например, в следующих выходных данных команды **ipconfig** хост Windows 10 PC1 использовал информацию о подсети IPv6, содержащуюся в R1 RA, и случайным образом сгенерировал 64-битный идентификатор интерфейса, выделенный в выходных данных.

```
C:\PC1> ipconfig
Windows IP Configuration
Ethernet adapter Ethernet0:
   Connection-specific DNS Suffix  . : 
   IPv6 Address. . . . . . . . . . . : 2001:db8:acad:1:1de9:c69:73ee:ca8c
   Link-local IPv6 Address . . . . . : fe80::fb:1d54:839f:f595%21
   IPv4 Address. . . . . . . . . . . : 169.254.202.140
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Default Gateway . . . . . . . . . : fe80::1%6
C:\PC1>
```

<!-- 8.2.6 -->
## Обнаружение дублирующихся адресов (DAD)

Этот процесс позволяет хосту создавать IPv6-адрес. Однако нет гарантии, что адрес уникален в сети.

Поскольку SLAAC — это процесс без отслеживания состояния, компьютер PC1 должен проверить, что новый созданный IPv6-адрес является уникальным, прежде чем его использовать. **Процесс обнаружения дубликатов адресов** (DAD) используется хостом для обеспечения уникальности GUA IPv6.

DAD реализован с использованием ICMPv6. Чтобы выполнить его, хост отправляет сообщение ICMPv6 Neighbor Solicitation (NS) со специально созданным адресом многоадресной рассылки, который называется адресом многоадресной рассылки запрашиваемого узла. Этот адрес дублирует последние 24 бита IPv6-адреса узла.

Если другие устройства не отвечают сообщением с объявлением соседей, значит, скорее всего, что адрес является уникальным и может быть использован PC1. Если сообщение запроса поиска соседей получено PC1, значит, адрес не уникален и операционная система должна установить новый идентификатор интерфейса для использования.

Целевая группа Internet Engineering Task Force (IETF) рекомендует использовать DAD на всех одноадресных IPv6 независимо от того, создан ли он только с помощью SLAAC, получен с использованием DHCPv6 с сохранением состояния или настроен вручную. DAD не является обязательным, так как 64-битный идентификатор интерфейса предоставляет возможности 18 квинтиллионов и вероятность дублирования является удаленным. Однако большинство операционных систем выполняют DAD на всех одноадресных IPv6 независимо от способа настройки адреса.

<!-- 8.2.7 -->
<!-- quiz -->
