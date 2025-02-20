<!-- 7.4.3 -->

## Принципы DHCPv4

Сервер DHCPv4 динамически назначает или арендует клиенту выбранный сервером из пула адресов IPv4-адрес на ограниченный период времени или до тех пор, пока не пропадет необходимость в нем. Процесс аренды DHCPv4 начинается с отправки клиентом сообщения с запросом служб DHCP-сервера.

Если сервер DHCPv4 получает сообщение, он будет укажет в ответе IPv4-адрес и возможные другие сведения о конфигурации сети. Периодически клиент должен связываться с DHCP-сервером для продления срока аренды. Благодаря подобному механизму «переехавшие» или отключившиеся клиенты не занимают адреса, в которых они больше не нуждаются.

Когда клиент загружается (или иным образом хочет присоединиться к сети), он начинает четырехэтапный процесс для получения аренды: DHCPDISCOVER, потом DHCPOFFER, затем DHCPREQUEST и, наконец, DHCPACK. Процесс продления аренды с сервером DHCPv4 клиент начинает о истечения срока аренды. Он двухэтапный: DHCPREQUEST, затем DHCPACK.

## Настройка сервера DHCPv4 в Cisco IOS

Роутер Cisco под управлением ОС Cisco IOS можно настроить в качестве DHCPv4-сервера. Для настройки сервера DHCPv4 Cisco IOS нужно выполнить следующие действия: исключить IPv4-адреса, определить имя пула DHCPv4 и настроить пул DHCPv4. После этого следует проверить конфигурацию с помощью команд **show running-config | section dhcp**, **show ip dhcp binding** и **show ip dhcp server statistics**.

Служба DHCPv4 включена по умолчанию. Для того чтобы отключить ее, нужно в режиме глобальной конфигурации ввести команду **no service dhcp**.

В сложной иерархической сети корпоративные серверы обычно располагаются в серверной ферме. Они могут предоставлять службы DHCP, DNS, TFTP и FTP. Клиенты сети и серверы, как правило, находятся в разных подсетях. Для определения местоположения серверов и получения услуг клиенты часто используют сообщения широковещательной рассылки. Если DHCP-сервер находится в отдельной сети, необходимо настроить такую пересылку пакетов с помощью команды конфигурации **ip helper-address _address interface_**.

Администратор сети может использовать команду **show ip interface** для проверки конфигурации. По умолчанию команда **ip helper-address** переадресовывает следующие восемь служб UDP:

* Port 37: Время;
* Port 49: TACACS;
* Port 53: DNS;
* Port 67: DHCP/BOOTP server;
* Port 68: DHCP/BOOTP client;
* Port 69: TFTP;
* Port 137: NetBIOS name service;
* Port 138: NetBIOS datagram service.

## Настройка клиента DHCPv4

В простейшей конфигурации для соединения с кабельным или DSL-модемом используется Ethernet-интерфейс. Для настройки Ethernet-интерфейса в качестве DHCP-клиента используют команду режима настройки интерфейса **ip address dhcp interface**. Роутеры беспроводной связи настраиваются для автоматического получения IPv4-адресов от интернет-провайдера. Тип интернет-соединения установлен на автоматическую настройку – DHCP. Этот параметр используется, когда роутер подключен к кабельному или DSL-модему и выступает в качестве DHCPv4-клиента, запрашивая IPv4-адрес у интернет-провайдера.

<!-- 7.4.4 -->
<!-- quiz -->
