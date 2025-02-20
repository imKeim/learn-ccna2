
---

> **ВАЖНО**
> 
> Форма для ответов на вопросы будет доступна только при развертывании лабораторной работы 

---

## Таблица адресации

| **Устройство**                                     | **Интерфейс**       | **IP-адрес**     | **Маска подсети**   | **Шлюз по умолчанию** |
|------------------------------------------------|------------------|--------------|-----------------|-------------------|
| **Corporate Office (офис компании)**           |                  |              |                 |                   |
| Corporate Router (корпоративный роутер) | G1/0             | 192.168.1.1  | 255.255.255.0   | —                 |
| Corporate Router (корпоративный роутер) | G2/0             | 192.168.4.1  | 255.255.255.252 | —                 |
| Corporate Router (корпоративный роутер) | G3/0             | 192.168.3.1  | 255.255.255.252 | —                 |
| Corporate Server                               | Fa0              | 192.168.1.10 | 255.255.255.0   | 192.168.1.1       |
| Corporate WLC                                  | G2               | 192.168.1.50 | 255.255.255.0   | 192.168.1.1       |
| LAP-C1 to C6                                   | G0               | DHCP         | —               | —                 |
| Laptop                                         | Fa0              | DHCP         | —               | —                 |
| **Branch (филиал)**                            |                  |              |                 |                   |
| Branch Router                                  | G0/0             | 192.168.2.1  | 255.255.255.0   | —                 |
| Branch Router                                  | G2/0             | 192.168.4.2  | 255.255.255.252 | —                 |
| Branch Server                                  | Fa0              | 192.168.2.10 | 255.255.255.0   | 192.168.2.1       |
| Branch WLC                                     | G2               | 192.168.2.50 | 255.255.255.0   | 192.168.2.1       |
| LAP-B1 to B6                                   | G0               | DHCP         | —               | 192.168.2.1       |
| **Центральный офис (CO)**                      |                  |              |                 |                   |
| CO Server                                      | —                | 192.168.5.2  | 255.255.255.252 | 192.168.5.1       |
| Cellular                                       | —                | 172.16.1.1   | 255.255.255.0   | —                 |
| CO Router                                      | G0/0             | 192.168.5.1  | 255.255.255.252 | —                 |
| CO Router                                      | G1/0             | 192.168.6.1  | 255.255.255.252 | —                 |
| CO Router                                      | G3/0             | 192.168.3.2  | 255.255.255.252 | —                 |
| Cell Towers 0-5                                | Coaxial 0-2, 4-5 | —            | —               | —                 |

## Цели

Часть 1. Изучение беспроводной сети.

Часть 2. Добавление подключения Wi-Fi в зал заседаний.

Часть 3. Добавление беспроводного подключения к кофейне в «мертвой» зоне сотовой связи.

Часть 4. Добавление беспроводного подключения к домашнему офису.

## Общие сведения и сценарий

XYZ Corporation расширяет свои сетевые возможности, чтобы обеспечить улучшенную связь в своих местных офисах, а также возможность подключения для тех, кто хочет работать удаленно. В этом режиме симуляции физического оборудования (PTPM) вам было предложено помочь с этим планом, просмотрев текущие возможности сети и добавив функциональные по мере необходимости.

**Примечание.** Это задание не оценивается. Тесты можно использовать для проверки физических подключений и конфигураций.

## Инструкции

### Часть 1. Изучение беспроводной сети

В части 1 вы изучите беспроводную сеть и проверите подключение.

**Примечание.** В этом задании включено **беспроводное** и **сотовое** представление сигналов. Однако их можно отключить, нажав на **View Wireless Signals** (Ctrl+Shift+W) на верхней синей панели инструментов.

**Шаг 1. Изучите топологию**

1.  В **физическом** режиме вы заметите, что **Home City** содержит пять различных местоположений: **Corporate Office**, **Branch Office**, **Central Office (CO)**, **Home Office** и **Coffee Shop**.

    - Ответьте на вопрос №1

2.  Перейдите в **Corporate Office**. Обратите внимание, что шесть облегченных беспроводных точек доступа (LWAP) подключены к стойке.

    - Ответьте на вопрос №2

3.  Перейдите в **Wiring Closet** в **CO**.

    - Ответьте на вопрос №3

4.  Перейдите в **Branch Office**. Обратите внимание, что пять LWAP подключены с помощью медного кабеля **Branch Switch** в стойке. Затем коммутатор подключается к **Branch WLC** (к контроллеру беспроводной локальной сети).

**Шаг 2. Проверьте подключение**

1.  Чтобы проверить подключение, перейдите в раздел **Corporate Office** \> **Corporate Office Wiring Closet**.

2.  Нажмите на ноутбук и выберите **Desktop** \> **Command Prompt**.

    1.  Ping 192.168.2.10 (**Branch Office Server**).

    2.  Ping 192.168.5.2 (магистральное соединение **Central Office**).

    **Примечание.** Packet Tracer может потребовать некоторое время, чтобы сойтись. Вы можете получать сообщения **Request time out**. Тем не менее, оба запроса должны в конечном итоге быть успешными. В нижней части интерфейса Packet Tracer несколько раз нажмите **Fast Forward Time**, чтобы ускорить время сходимости.

3.  Перейдите в **Home City**. Нажмите на **Smartphone** рядом с вышкой сотовой связи над зданием **Central Office**.

4.  Нажмитне на вкладку **Desktop**, а затем – **Command Prompt**.

    1.  Ping 172.16.1.1 (сотовая соединение до **Central Office**).

    2.  Ping 192.168.1.10 (**Corporate Office Server**).

    Вы можете получать сообщения **Request time out**. Тем не менее, запросы должны в конечном итоге быть успешными.

    - Ответьте на вопрос №4

### Часть 2. Добавление подключения Wi-Fi в зал заседаний

В **Branch Office** создается новый зал заседаний. В настоящее время зал заседаний находится в «мертвой» зоне Wi-Fi. В части 2 задача состоит в том, чтобы обеспечить подключение устройств в этой комнате.

**Шаг 1. Установите новое устройство LAP-PT, чтобы обеспечить покрытие нового зала заседаний**

1.  Перейдите в **Branch Office**. Ноутбук внутри нового зала заседаний не имеет доступа к сигналу Wi-Fi.

2.  Нажмите на точку доступа с **Inventory Shelf** и перетащите ее в **Boardroom**.

3.  Нажмите на новую точку доступа, чтобы открыть ее. В меню **Modules** выберите и перетащите модуль **Access_Point_Power_Adapter** и подключите его к порту питания рядом с кнопкой **Reset**.

4.  Откройте вкладку **Config**. В разделе **Global Settings** назначте имя устройству **LAP-B6**.

5.  В разделе **Interface Dot11Radio0** установите параметр **Coverage Range** в **75** метров. Packet Tracer не оценивает этот параметр.

6.  Закройте окно **LAP-B6**. Если у вас включен **View Wireless Signals**, вы заметите, что теперь в зале заседаний есть покрытие.

7.  На панели **Bottom Toolbar**, выберите **Connections** \> **Copper Straight-Through**.

8.  Подключите один конец кабеля к интерфейсу **GigabitEtherent0** на новой точке доступа. Нажмите на стойку оборудования и подключите другой конец кабеля к интерфейсу **Rack** \> **Branch Switch** \>**Fa0/22**.

**Шаг 2. Проверьте связь**

1.  В зале заседаний нажмите на ноутбук, а затем – на вкладку **Desktop** \> **IP Configuration**. Теперь ноутбук должен иметь полную конфигурацию IPv4 в разделе **IP Configuration**. Тем не менее процессы DHCP могут занять несколько минут. При необходимости переключитесь между DHCP и Static для повторной отправки запроса DHCP. Вам также может потребоваться нажать на **Fast Forward Time** несколько раз, чтобы ускорить сходимость.

2.  Когда предоставляется IP-адресация, можно проверить подключение. Закройте окно **IP Configuration** и выберете **Command Prompt**.

    1. Ping 192.168.2.1 (шлюз по умолчанию – **default gateway**).

    2. Ping 192.168.1.10 (**Corporate Server**).

### Часть 3. Добавление беспроводного подключения к кофейне в сотовой «мертвой» зоне

В **Home City** открылась новая кофейня **Coffee Shop**, но в настоящее время в этой области нет сотовой связи. Ваша задача — обеспечить сотовую связь клиентам и сотрудникам кофейни.

**Шаг 1. Подключите новую вышку сотовой связи**

1.  Перейдите в **Home City**.

2.  Обратите внимание, что **Cell Tower** рядом с **Coffee Shop** не подключена **Central Office**.

3.  На нижней панели инструментов выберите **Connections \> Coaxial cable**.

4.  Подключите один конец к интерфейсу **Coaxial0** на неподключенной **Cell Tower**. Затем нажмите на **Central Office \> Central Office Wiring Closet \> Table \> Central Office Server \> Coaxial0/3**.

**Шаг 2. Свяжите ноутбук и смартфон**

1.  Сотрудник хочет работать в кофейне удаленно. Перейдите в **Coffee Shop** и найдите смартфон и ноутбук на столе.

2.  Нажмите на **Smartphone** \> **Config** \> **3G/4G Cell1**, чтобы убедиться, что смартфон получил IP-адрес. Это может занять несколько секунд. При необходимости нажмите **DHCP Refresh**.

3.  Выберите **Settings** и убедитесь, что смартфон получил шлюз по умолчанию и адрес DNS-сервера.

4.  В разделе **Cellular Tethering** включите **Bluetooth**.

5.  В разделе **Interface** нажмите на **Bluetooth** и переведите **Port Status** в состояние **On**. Убедитесь, что параметр **Discoverable** включен.

6.  В **Coffee Shop** нажимте на **Laptop \> Desktop tab \> Bluetooth** и установите **Port Status** в состояние **On**.

7.  Выберите **Discover**, чтобы отобразить **Smartphone1** в разделе «Обнаруживаемые устройства».

8.  Выберите **Smartphone1**, нажмите кнопку **Pair**, а затем ответьте **Yes** во всплывающем сообщении **Bluetooth Pairing**.

9.  Нажмите на ноутбук еще раз, повторно выберите **Smartphone1** и нажмите кнопку **Tether**. Для работы сопряжения **Bluetooth** может потребоваться переместить смартфон и ноутбук ближе друг к другу.

10. Через несколько секунд вы увидите действительные сведения об адресации в разделе **IP Configuration**. Если нет, повторите предыдущие шаги.

11. Чтобы проверить подключение, закройте окно **Bluetooth Configuration** и нажмите кнопку **Command Prompt**. Запустите эхо-запрос от **Cellular Gateway** (172.16.1.1) до **Corporate Office Server** (192.168.1.10). Если первый эхо-запрос до **Corporate Office Server** не завершен успешно, попробуйте выполнить другой.

### Часть 4. Добавление беспроводного подключения к домашнему офису

Удаленный работник для **XYZ Corporation** только что переехал. В его новым доме еще нт настройки сети. Ваша задача состоит в том, чтобы настроить сеть для обеспечения беспроводного доступа по всему дому и подключения к **Corporate Office**.

**Шаг 1. Выберите устройства и подключать их к кабелю**

1.  Перейдите в раздел **Home City**, а затем **Home Office**.

    На полке за стулом находится беспроводной роутер с внешними антеннами. Также имеется кабельный модем непосредственно справа от роутера. На столе перед диваном есть ноутбук.

2.  Выберите **Connections** \> **Copper Straight-Through**.

3.  Подключите один конец кабеля к **Port 1** на **Cable Modem**, а другой – к порту **Internet** на **Wireless Router**.

4.  Перейдите к виду **Home City**.

5.  На **Bottom Toolbar** нажмите на **Connections** \> **Coaxial**.

6.  Нажмите на **Home Office** \> **Cable Modem0** \> **Port 0**, а затем выберите порт **Central Office** \> **Central Office Wiring Closet** \> **Rack** \> **CMTS** \> **Coaxial7**.

**Шаг 2. Настройка беспроводного роутера**

1.  Перейдите в **Home Office**, нажмите на **Wireless Router** \> **GUI tab**.

2.  Вкладка **Setup** уже выбрана. Убедитесь, что на панели **Internet Connection Type** выбран режим **Automatic Configuration - DHCP**.

3.  В разделе **Network Setup** убедитесь, что настроены следующие параметры:

    IP-address: **192.168.0.1**;

    Subnet mask: **255.255.255.0**;

    DHCP: **Enabled**;

    Starting IP Address: **192.168.0.100**;

    Maximum number of users: **50**.

4.  Вернитесь наверх и перейдите на вкладку **Status**. В разделе **Internet Connection**, беспроводной роутер должен иметь DHCP-адресацию из **Central Office**. В противном случае нажмите кнопку **IP Address Renew**, чтобы повторно отправить сообщения DHCP.

5.  Выберите вкладку **Wireless**.

6.  Настройте сеть 2,4 ГГц с **homesweethome** в качестве **SSID**. Прокрутите страницу вниз и нажмите кнопку **Save Settings**.

7.  Прокрутите вверх и выберите вложенную вкладку **Wireless Security**.

8.  В **Security Mode** 2,4 ГГц выберите **WPA2-Personal**, а затем настройте **MySecureNet** в качестве **парольной фразы**. Прокрутите страницу вниз и нажмите кнопку **Save Settings**.

**Шаг 3. Проверьте подключение**

1.  На столе перед диваном нажмите на ноутбук, а затем – вкладку **Configuration**. После выберите **Wireless0** в разделе **Interface**.

2.  Введите SSID **homesweethome**.

3.  Выберите **WPA2-PSK** для метода **Authentication**, а затем настройте **MySecureNet** в качестве **парольной фразы** PSK.

4.  В разделе **IP Configuration** ноутбук должен получать DHCP-адресацию. Для повторной отправки запросов DHCP может потребоваться несколько раз переключаться между DHCP и Static.

5.  На вкладке **Desktop** нажмите **Command Prompt**. Проверьте связь с различными адресами по всей сети. Например, следующие эхо-запросы должны быть успешными:

    ping 192.168.6.1 (**Central Office router G1/0**);

    ping 192.168.1.10 (**Corporate Office Server**);

    ping 192.168.2.10 (**Branch Office Server**).

### Вопросы для повторения

1.  Ответьте на вопрос №5

2.  Ответьте на вопрос №6

3.  Ответьте на вопрос №7

<!-- [Скачать файл Packet Tracer для локального запуска](./assets/13.5.2-lab.pka) -->
