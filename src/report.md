## Part 1. Инструмент ipcalc  
### 1.1. Сети и маски  
#### 1) Адрес сети 192.167.38.54/13  
* Для вывода адреса сети пользуюсь утилитой ipcalc, которую предварительно скачала командой `sudo apt install ipcalc`.  
Далее ввожу команду `ipcalc 192.167.38.54/13`  
<img title="title" alt="OS installation screenshot" src="./screens/1.1.1.png"><br>  

#### 2) Перевод маски 255.255.255.0 в префиксную и двоичную запись, /15 в обычную и двоичную, 11111111.11111111.11111111.11110000 в обычную и префиксную  
* Использую команду `ipcalc 255.255.255.0`  
<img title="title" alt="OS installation screenshot" src="./screens/1.1.2.png"><br>   
Префиксная запись `/24`. Двоичная запись - `11111111.11111111.11111111.00000000`

* Использую команду `ipcalc /15`  
<img title="title" alt="OS installation screenshot" src="./screens/1.1.3.png"><br>  
Обычная запись - `255.254.0.0`. Двоичная запись - `11111111.11111110.00000000.00000000`  

* Считаю  количество ненулевых бит в 11111111.11111111.11111111.11110000, получил - 28. Использую команду `ipcalc 192.167.38.54/28`  
<img title="title" alt="OS installation screenshot" src="./screens/1.1.4.png"><br>  
Обычная запись - `255.255.255.240`. Префиксная запись - `/28`.  

#### 3) Минимальный и максимальный хост в сети 12.167.38.4 при масках: /8, 11111111.11111111.00000000.00000000, 255.255.254.0, /4  
* Использую команду `ipcalc 12.167.38.4/8`  
<img title="title" alt="OS installation screenshot" src="./screens/1.1.5.png"><br>    
Минимальный хост - `12.0.0.1`. Максимальный хост - `12.255.255.254`.  

* Использую команду `ipcalc 12.167.38.4/16`  
<img title="title" alt="OS installation screenshot" src="./screens/1.1.6.png"><br>  
Минимальный хост - `12.167.0.1`. Максимальный хост - `12.167.255.254`.  

* Использую команду `ipcalc 12.167.38.4/23`  
<img title="title" alt="OS installation screenshot" src="./screens/1.1.7.png"><br>   
Минимальный хост - `12.167.38.1`. Максимальный хост - `12.167.39.254`. 

* Использую команду `ipcalc 12.167.38.4/4`  
<img title="title" alt="OS installation screenshot" src="./screens/1.1.8.png"><br>    
Минимальный хост - `0.0.0.1`. Максимальный хост - `15.255.255.254`.  

### 1.2. Localhost  
#### Определить и записать в отчёт, можно ли обратиться к приложению, работающему на localhost, со следующими IP: 194.34.23.100, 127.0.0.2, 127.1.0.1, 128.0.0.1  
* Пишу в гугле "localhost" и в первой ссылке на википедию вижу, что для localhost зарезервирован диапазон ip адресов от 127.0.0.1 до 127.255.255.254 включительно. Из этого делаю вывод, что могу обратиться к приложениям, работающим со следующими ip: `127.0.0.2`, `127.1.0.1`.

### 1.3. Диапазоны и сегменты сетей  
#### 1) Какие из перечисленных IP можно использовать в качестве публичного, а какие только в качестве частных: 10.0.0.45, 134.43.0.2, 192.168.4.2, 172.20.250.4, 172.0.2.1, 192.172.0.1, 172.68.0.2, 172.16.255.255, 10.10.10.10, 192.169.168.1  
* Вывожу информацию обо всех ip адресах с помощью команды `ipcalc <ip адрес>`  
<img title="title" alt="OS installation screenshot" src="./screens/1.3.1.png"><br>
<img title="title" alt="OS installation screenshot" src="./screens/1.3.2.png"><br>
<img title="title" alt="OS installation screenshot" src="./screens/1.3.3.png"><br>
<img title="title" alt="OS installation screenshot" src="./screens/1.3.4.png"><br>

Меня интересует строка "Hosts/Net" и конкретно надпись "Private Internet", наличие которой говорит о том, что ip адрес принадлежит частной сети.  
Вывод:  
* Адреса `10.0.0.45`, `192.168.4.2`, `172.20.250.4`, `172.16.255.255`, `10.10.10.10` можно использовать только в качестве частных.  
* Адреса `134.43.0.2`, `172.0.2.1`, `192.172.0.1`, `172.68.0.2`, `192.169.168.1` можно использовать в качестве публичных.  

#### 2) Какие из перечисленных IP адресов шлюза возможны у сети 10.10.0.0/18: 10.0.0.1, 10.10.0.2, 10.10.10.10, 10.10.100.1, 10.10.1.255  
* Использую команду `ipcalc -b 10.10.0.0/18`  
<img title="title" alt="OS installation screenshot" src="./screens/1.3.5.png"><br>   
Возможны адреса `10.10.0.2`, `10.10.10.10` и `10.10.1.255`  

## Part 2. Статическая маршрутизация между двумя машинами  

#### Поднять две виртуальные машины (далее -- ws1 и ws2)

#### С помощью команды ip a посмотреть существующие сетевые интерфейсы  
* ws1  
<img title="title" alt="OS installation screenshot" src="./screens/2.0.0.png"><br>  

#### Описать сетевой интерфейс, соответствующий внутренней сети, на обеих машинах и задать следующие адреса и маски: ws1 - 192.168.100.10, маска /16, ws2 - 172.24.116.8, маска /12  

* На обеих машинах выполняю команду `sudo nano /etc/netplan/00-installer-config.yaml` и вношу изменения.  
<img title="title" alt="OS installation screenshot" src="./screens/2.0.1.png"><br> 
<img title="title" alt="OS installation screenshot" src="./screens/2.0.2.png"><br>     

#### Выполнить команду netplan apply для перезапуска сервиса сети  

<img title="title" alt="OS installation screenshot" src="./screens/2.0.3.png"><br> 

### 2.1. Добавление статического маршрута вручную  
#### Добавить статический маршрут от одной машины до другой и обратно при помощи команды вида ip r add 

#### Добавить статический маршрут от одной машины до другой и обратно при помощи команды вида ip r add  
* Команда для ws1: `sudo ip r add 172.24.116.8 dev enp0s3`     
* Команда для ws2: `sudo ip r add 192.168.100.10 dev enp0s3`

<img title="title" alt="OS installation screenshot" src="./screens/2.1.1.png"><br>

#### Пропинговать соединение между машинами  
  
<img title="title" alt="OS installation screenshot" src="./screens/2.1.2.png"><br>

### 2.2. Добавление статического маршрута с сохранением  
#### Перезапустить машины  
* Использую команду `sudo reboot`  

#### Добавить статический маршрут от одной машины до другой с помощью файла etc/netplan/00-installer-config.yaml  
<img title="title" alt="OS installation screenshot" src="./screens/2.2.1.png"><br>

#### Пропинговать соединение между машинами  

<img title="title" alt="OS installation screenshot" src="./screens/2.2.2.png"><br>  

## Part 3. Утилита iperf3  
### 3.1. Скорость соединения  
#### Перевести и записать в отчёт: 8 Mbps в MB/s, 100 MB/s в Kbps, 1 Gbps в Mbps  
* 8 Mbps = 1 MB/s  
* 100 MB/s = 819200 Kbps  
* 1 Gbps = 1024 Mbps  

### 3.2. Утилита iperf3  
#### Измерить скорость соединения между ws1 и ws2  
* ws1 выступает в роли сервера. Запуск iperf3 сервер, команда: `iperf3 -s -f m`   
* ws2 выступает в роли клиента. Запуск iperf3 клиент, команда: `iperf3 -c 192.168.100.10`
<img title="title" alt="OS installation screenshot" src="./screens/3.1.png"><br> 

## Part 4. Сетевой экран  
### 4.1. Утилита iptables  
#### Создать файл /etc/firewall.sh, имитирующий фаерволл, на ws1 и ws2:
    #!/bin/sh  

    # Deleting all the rules in the "filter" table (default).  
    iptables -F  
    iptables –X  

#### Нужно добавить в файл подряд следующие правила:  

#### 1) на ws1 применить стратегию когда в начале пишется запрещающее правило, а в конце пишется разрешающее правило (это касается пунктов 4 и 5)  

#### 2) на ws2 применить стратегию когда в начале пишется разрешающее правило, а в конце пишется запрещающее правило (это касается пунктов 4 и 5)  

#### 3) открыть на машинах доступ для порта 22 (ssh) и порта 80 (http)  

#### 4) запретить echo reply (машина не должна "пинговаться”, т.е. должна быть блокировка на OUTPUT)  

#### 5) разрешить echo reply (машина должна "пинговаться")  

* добавляю правила :  Запрещаю echo-reply, открываю доступ для портов 22 и 80, разрешаю echo-reply.
<img title="title" alt="OS installation screenshot" src="./screens/4.1.png"><br>   


#### Запустить файлы на обеих машинах командами `chmod +x /etc/firewall.sh` и `/etc/firewall.sh`  
* запускаю файл 

### 4.2. Утилита nmap  
#### Командой ping найти машину, которая не "пингуется", после чего утилитой nmap показать, что хост машины запущен 

<img title="title" alt="OS installation screenshot" src="./screens/4.2.png"><br> 

* Разница между стратегиями заключается в том, что в первом файле первым подходящим правилом для пакета является запрет, а во втором - разрешение. Применяется только первое подходящее правило, остальные игнорируются.  

## Part 5. Статическая маршрутизация сети  
#### Поднять пять виртуальных машин (3 рабочие станции (ws11, ws21, ws22) и 2 роутера (r1, r2)) 

### 5.1. Настройка адресов машин  
#### Настроить конфигурации машин в etc/netplan/00-installer-config.yaml согласно сети на рисунке.

<img title="title" alt="OS installation screenshot" src="./screens/5.1.1.png"><br>  

#### Перезапустить сервис сети. Если ошибок нет, то командой ip -4 a проверить, что адрес машины задан верно. Также пропинговать ws22 с ws21. Аналогично пропинговать r1 с ws11.  
 
<img title="title" alt="OS installation screenshot" src="./screens/5.1.2.png"><br> 
 
<img title="title" alt="OS installation screenshot" src="./screens/5.1.3.png"><br> 
 
<img title="title" alt="OS installation screenshot" src="./screens/5.1.4.png"><br> 

### 5.2. Включение переадресации IP-адресов.  
#### Для включения переадресации IP, выполните команду на роутерах:  
`sysctl -w net.ipv4.ip_forward=1`  
* ввод   
<img title="title" alt="OS installation screenshot" src="./screens/5.2.1.png"><br>  

#### Откройте файл /etc/sysctl.conf и добавьте в него следующую строку:  
`net.ipv4.ip_forward = 1`  
* изменяю файл для r1 и r2  
<img title="title" alt="OS installation screenshot" src="./screens/5.2.2.png"><br>

### 5.3. Установка маршрута по-умолчанию  
#### Настроить маршрут по-умолчанию (шлюз) для рабочих станций. Для этого добавить default перед IP роутера в файле конфигураций  

* добавляю шлюз для машин, также потребовалось добавить шлюзы для роутеров, чтобы пинг доходил в соседнюю сеть
<img title="title" alt="OS installation screenshot" src="./screens/5.3.1.png"><br>  

#### Вызвать ip r и показать, что добавился маршрут в таблицу маршрутизации   
<img title="title" alt="OS installation screenshot" src="./screens/5.3.2.png"><br>

#### Пропинговать с ws11 роутер r2 и показать на r2, что пинг доходит. Для этого использовать команду:  
`tcpdump -tn -i eth1`
Изменяю eth1 на название моего адаптера enp0s9 и пингую:    
<img title="title" alt="OS installation screenshot" src="./screens/5.3.3.png"><br>

### 5.4. Добавление статических маршрутов  
#### Добавить в роутеры r1 и r2 статические маршруты в файле конфигураций. Пример для r1 маршрута в сетку 10.20.0.0/26:  
    # Добавить в конец описания сетевого интерфейса eth1:  
    - to: 10.20.0.0  
      via: 10.100.0.12  
<img title="title" alt="OS installation screenshot" src="./screens/5.4.1.png"><br>

#### Вызвать ip r и показать таблицы с маршрутами на обоих роутерах.  
<img title="title" alt="OS installation screenshot" src="./screens/5.4.2.png"><br>

#### Запустить команды на ws11:  
`ip r list 10.10.0.0/[маска сети]` и `ip r list 0.0.0.0/0`  
* команда `ip r list 10.10.0.0/[маска сети]`  
<img title="title" alt="OS installation screenshot" src="./screens/5.4.4.png"><br> 

* Маршрут по умолчанию имеет более низкий приоритет и срабатывает, когда не найден подходящий маршрут в таблице маршрутизации. Для сети 10.10.0.0 мы создали правило, соответственно используется созданный маршрут. Также можно устанавливать метрику, чтобы менять приоритеты маршрутов.

### 5.5. Построение списка маршрутизаторов  
#### Запустить на r1 команду дампа:  
`tcpdump -tnv -i eth0`  

#### При помощи утилиты traceroute построить список маршрутизаторов на пути от ws11 до ws21  
Устанавливаю утилиту traceroute: `sudo apt install traceroute`.
* вызов и вывод traceroute на ws11
<img title="title" alt="OS installation screenshot" src="./screens/5.5.1.png"><br> 

* Принцип построения пути при помощи traceroute:  
Для определения промежуточных маршрутизаторов traceroute отправляет серию пакетов данных целевому узлу, при этом каждый раз увеличивая на 1 значение поля TTL («время жизни»). Это поле обычно указывает максимальное количество маршрутизаторов, которое может быть пройдено пакетом. Первый пакет отправляется с TTL, равным 1, и поэтому первый же маршрутизатор возвращает обратно сообщение ICMP, указывающее на невозможность доставки данных. Traceroute фиксирует адрес маршрутизатора, а также время между отправкой пакета и получением ответа (эти сведения выводятся на монитор компьютера). Затем traceroute повторяет отправку пакета, но уже с TTL, равным 2, что позволяет первому маршрутизатору пропустить пакет дальше.  
Процесс повторяется до тех пор, пока при определённом значении TTL пакет не достигнет целевого узла. При получении ответа от этого узла процесс трассировки считается завершённым.

### 5.6. Использование протокола ICMP при маршрутизации  
#### Запустить на r1 перехват сетевого трафика, проходящего через eth0 с помощью команды:  
`tcpdump -n -i eth0 icmp`  

#### Пропинговать с ws11 несуществующий IP (например, 10.30.0.111) с помощью команды:

<img title="title" alt="OS installation screenshot" src="./screens/5.6.png"><br> 

## Part 6. Динамическая настройка IP с помощью DHCP  
#### Для r2 настроить в файле /etc/dhcp/dhcpd.conf конфигурацию службы DHCP: 

* для начала скачиваю isc-dhcp-server: `sudo apt install isc-dhcp-server`
#### 1) указать адрес маршрутизатора по-умолчанию, DNS-сервер и адрес внутренней сети.  
* вношу изменения в файл /etc/dhcp/dhcpd.conf
<img title="title" alt="OS installation screenshot" src="./screens/6.1.png"><br>

#### 2) в файле resolv.conf прописать nameserver 8.8.8.8.  
* вношу изменения в файл /etc/resolv.conf 
<img title="title" alt="OS installation screenshot" src="./screens/6.7.png"><br>  

#### Перезагрузить службу DHCP командой systemctl restart isc-dhcp-server. Машину ws21 перезагрузить при помощи reboot и через ip a показать, что она получила адрес. Также пропинговать ws22 с ws21.  
* перезагружаю службу DHCP при помощи reboot
<img title="title" alt="OS installation screenshot" src="./screens/6.3.png"><br>
<img title="title" alt="OS installation screenshot" src="./screens/6.66.png"><br>

#### Указать MAC адрес у ws11, для этого в etc/netplan/00-installer-config.yaml надо добавить строки: macaddress: 10:10:10:10:10:BA, dhcp4: true  
* вношу изменения в /etc/netplan/00-installer-config.yaml 
<img title="title" alt="OS installation screenshot" src="./screens/6.5.png"><br>    

#### Для r1 настроить аналогично r2, но сделать выдачу адресов с жесткой привязкой к MAC-адресу (ws11). Провести аналогичные тесты  
* Снова скачиваю isc-dhcp-server и вношу изменения в файл /etc/dhcp/dhcpd.conf  
<img title="title" alt="OS installation screenshot" src="./screens/6.7.(1).png"><br> 

* затем редактирую файл /etc/resolv.conf 
<img title="title" alt="OS installation screenshot" src="./screens/6.7.png"><br>

* перезагружаю ws21 и вызываю `ip a`  
<img title="title" alt="OS installation screenshot" src="./screens/6.8.png"><br>   

<img title="title" alt="OS installation screenshot" src="./screens/6.9.png"><br>   

* вызываю команду `sudo dhclient enp0s8`, потом `sudo dhclient enp0s8`  
<img title="title" alt="OS installation screenshot" src="./screens/6.10.png"><br>  

## Part 7. NAT  
#### В файле /etc/apache2/ports.conf на ws22 и r1 изменить строку Listen 80 на Listen 0.0.0.0:80, то есть сделать сервер Apache2 общедоступным  

<img title="title" alt="OS installation screenshot" src="./screens/7.1.png"><br>

<img title="title" alt="OS installation screenshot" src="./screens/7.2.png"><br>
 
#### Запустить веб-сервер Apache командой `service apache2 start` на ws22 и r1

<img title="title" alt="OS installation screenshot" src="./screens/7.3.png"><br>

#### Добавить в фаервол, созданный по аналогии с фаерволом из Части 4, на r2 следующие правила:  
1) Удаление правил в таблице filter - iptables -F
2) Удаление правил в таблице "NAT" - iptables -F -t nat
3) Отбрасывать все маршрутизируемые пакеты - iptables --policy FORWARD DROP
<img title="title" alt="OS installation screenshot" src="./screens/7.4.png"><br>

#### Проверить соединение между ws22 и r1 командой ping  
<img title="title" alt="OS installation screenshot" src="./screens/7.5.png"><br>

#### Добавить в файл ещё одно правило:  
4) Разрешить маршрутизацию всех пакетов протокола ICMP  
<img title="title" alt="OS installation screenshot" src="./screens/7.6.png"><br>

#### Проверить соединение между ws22 и r1 командой ping  
<img title="title" alt="OS installation screenshot" src="./screens/7.7.png"><br>  

#### Добавить в файл ещё два правила:  
5) Включить SNAT, а именно маскирование всех локальных ip из локальной сети, находящейся за r2 (по обозначениям из Части 5 - сеть 10.20.0.0)
6) Включить DNAT на 8080 порт машины r2 и добавить к веб-серверу Apache, запущенному на ws22, доступ извне сети  
<img title="title" alt="OS installation screenshot" src="./screens/7.11.png"><br>

#### Проверить соединение по TCP для SNAT, для этого с ws22 подключиться к серверу Apache на r1 командой:  
`telnet [адрес] [порт]`  
<img title="title" alt="OS installation screenshot" src="./screens/7.10.png"><br>
<img title="title" alt="OS installation screenshot" src="./screens/7.9.png"><br>











