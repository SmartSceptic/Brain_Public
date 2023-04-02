---
alias: [Linux network, Сетевые утилиты ]
sr-due: 2023-03-18
sr-interval: 36
sr-ease: 239
---
tag: #N/S/Stub  #N/T/Public #T/T/To/Brain 
2022-10-19 22:43  


В этой статье мы рассмотрим лучшие сетевые утилиты linux, это самые основные команды, которые можно использовать для администрирования сети в Linux, тестирование сети linux, проверить сеть на работоспособность и обнаружить неполадки сети. Более подробную информацию по каждой из них вы можете найти в официальной документацию.


-   [Возможность подключения](https://losst.ru/luchshie-setevye-utility-linux#i)
-   [ARP](https://losst.ru/luchshie-setevye-utility-linux#ARP)
-   [Маршрутизация](https://losst.ru/luchshie-setevye-utility-linux#i-2)
-   [Другое](https://losst.ru/luchshie-setevye-utility-linux#i-3)
-   [Важные файлы](https://losst.ru/luchshie-setevye-utility-linux#i-4)
-   [Анализ сети](https://losst.ru/luchshie-setevye-utility-linux#i-5)
-   [Свитчинг](https://losst.ru/luchshie-setevye-utility-linux#i-6)
-   [VLAN](https://losst.ru/luchshie-setevye-utility-linux#VLAN)
-   [UDP / TCP](https://losst.ru/luchshie-setevye-utility-linux#UDP_TCP)
-   [NAT/Фаервол](https://losst.ru/luchshie-setevye-utility-linux#NAT)

#### ВОЗМОЖНОСТЬ ПОДКЛЮЧЕНИЯ

- ping <хост> — отправляет один эхо-запрос по протоколу [[ICMP]] на удалённый хост. Пакеты будут отправляется непрерывно, пока вы не нажмете `Ctrl+C`. Когда пакет будет отправлен, хост должен отправить ответное ICMP сообщение, это и будет означать, что другой хост работает.
- telnet хост <порт> — проверить доступность определенного порта на хосте. По умолчанию telnet использует порт 23, но также можно использовать и другие. Например, 7 — эхо порт, 25 — SMTP, почтовый сервер, 79 — Finger, предоставляет информацию о других пользователях сети. Нажмите Ctrl+] чтобы завершить работу telnet.

#### ARP
[[% arp]]

#### МАРШРУТИЗАЦИЯ
Эти утилиты позволяют выполнять администрирование сети linux, а также настраивать маршрутизацию между узлами.

Таблица маршрутизации хранится в ядре и используется для маршрутизации IP пакетов за пределами локальной сети.
[[% netstat]]
![[% netstat#Вывести таблицу маршрутизации]]
![[% netstat#отображает таблицу маршрутизации ipv4]]

![[% route#Описание]]
- [[% routed]] 
- [[% gated]] 
- [[% traceroute]]



sysctl net.inet.ip.forwarding=1 — разрешает прохождение пакетов через этот компьютер.

route flush — удалить все маршруты

route add -net 0.0.0.0 192.168.10.2 — добавить маршрут по умолчанию

routed -Pripv2 –Pno_rdisc –d [-s|-q] — запустить демон routed по протоколу RIP2 без поддержки ICMP автообнаружения и подробном (s) или минимальном (q) режиме вывода информации.

route add 224.0.0.0/4 127.0.0.1 — добавить маршрут, используемый RIP2

rtquery –n — попросить RIP демона отправить запрос на другой хост (ручное обновление таблицы маршрутизации.

ДРУГОЕ

Иногда нужно не только управление сетью linux, но и работа с другими протоколами, такими как DNS или FTP.

nslookup — отправить запрос DNS серверу, на преобразование доменного имени в IP. Например, nslookup facebook.com вернет ip адрес сервера facebook.com.

ftp хост — передать файлы на хост. Часто нужно использовать также логин и пароль.

rlogin -l — подключиться к виртуальному терминалу с помощью telnet. Не рекомендуется использовать эту конструкцию, лучше используйте защищенное соединение по ssh.

ВАЖНЫЕ ФАЙЛЫ

/etc/hosts — локальные имена для ip адресов

/etc/networks — имена сетей относительно ip адресов

/etc/protocols — имена протоколов для номеров протоколов

/etc/services — tcp/udp сервисы для номеров портов

АНАЛИЗ СЕТИ

ifconfig интерфейс адрес [up] — запустить интерфейс

ifconfig интерфейс [down|delete] — остановить интерфейс

ethereal & — позволяет запустить ethereal в фоновом режиме

tcpdump –i -vvv — инструмент для записи и анализа пакетов
[[% netstat#отобразить настройки сети и статистику]]

udpmt –p порт –s байт целевой_хост — генерирует UDP трафик

udptarget –p порт — получать UDP трафик

tcpmt –p порт –s байт целевой_хост — генерировать TPC трафик

tcptarget –p порт — получать TCP трафик

ifconfig — посмотреть состояние сетевых интерфейсов

netmask [up] — позволят посмотреть подсети

СВИТЧИНГ

ifconfig sl0 srcIP dstIP — настроить серийный интерфейс( сначала выполните slattach –l /dev/ttyd0 а затем sysctl net.inet.ip.forwarding=1

telnet 192.168.0.254 — подключится к свитчу из этой подсети

sh ru или show running-configuration — отобразить текущую конфигурацию

configure terminal — перейти в режим настройки

exit — выйти из режима настройки

VLAN

vlan n — создать VLAN с ID n

no vlan N — удалить VLAN с ID N

untagged Y — добавить порт Y к VLAN N

ifconfig vlan0 create — создать интерфейс vlan0

ifconfig vlan0 vlan ID vlandev em0 — соединить vlan0 с em0

ifconfig vlan0 up — активировать виртуальный интерфейс

UDP / TCP

Это программы для работы с сетью linux, которые позволяют передавать пакеты между машинами.

socklab udp — запустить socklab по протоколу udp.

sock — создать udp соект

sendto ид_сокета хост — порт передача пакетов данных

recvfrom ид_сокета размер_байт — получить данные из сокета

socklab tcp — запустить socklab по протоколу tcp

passive — создать сокет в пассивном режиме.

accept — разрешает входящие соединения

connect хост порт — эквивалент socklab

clase — закрывает соединение

read размер_байт — прочитать несколько байт из сокета

write — записать несколько байт в сокет

NAT/ФАЕРВОЛ

rm /etc/resolv.conf — отключает разрешение доменных имен, что гарантирует правильную фильтрацию по правилам фаервола.

ipnat –f имя_файла -записывает правила фаервола в файл

ipnat –l — получить список активных правил

ipnat –C –F — Очистить таблицу правил

map em0 192.168.1.0/24 -> 195.221.227.57/32 em0 — привязать IP адреса к интерфейсу

map em0 192.168.1.0/24 -> 195.221.227.57/32 portmap tcp/udp 20000:50000 — привязка IP адреса вместе с портом.

ipf –f имя_файла — записать правила в файл

ipf –F –a — очистить таблицу правил

ipfstat –I — информация о фильтрации, отфильтрованных пакетах и правилах.

ВЫВОДЫ

В этой статье мы рассмотрели самые полезные сетевые утилиты linux, с помощью них вы можете выполнять тестирование сети linux, проверить сеть на работоспособность и обнаружить неполадки сети. Более подробную информацию по каждой из них вы можете найти в официальной документац

- [[% lsof]]

---------------------
[https://proft.me/2011/08/9/primery-ispolzovaniya-netstat-ss-netcat/](https://proft.me/2011/08/9/primery-ispolzovaniya-netstat-ss-netcat/)

netstat ip ifconfig    [https://www.cyberciti.biz/faq/linux-list-network-interfaces-names-command/](https://www.cyberciti.biz/faq/linux-list-network-interfaces-names-command/)

Ручная настройка сети в Linux [https://parallel.uran.ru/book/export/html/442](https://parallel.uran.ru/book/export/html/442)


--------------------------------------


Теория:
[[Частные подсети]]

## Утилиты Linux
[Evry Linux Networking tool I know](https://wizardzines.com/networking-tools-poster/)
![[networking-tools-poster.pdf]]

##### [[DNS]]

##### Управление сетью.
[[% nmcli]]
[[% iptables]]

##### передача файлов  
[[% scp]]  
[[% rsync]]  

##### дебаг сети   
[[% dig]]  
[[% TELNET]]  
[[% mtr]]  
[[% nmap]]  
[[% iperf]]  
[[% netcat]]  

##### VPN
[[% openvpn]] : [[% pritunl vpn web interface]]
##### Прочие  
[[% iprange]]  
[[% iptables-vis]]  

[[% UFW]]  

[[% OpenSSL]]  
[[% ifconfig]]  


## Документация:
[[OSI]]

[[network interfaces]] : [[Создание и настройка виртуальных сетевых интерфейсов в Linux]]  
[[Примеры конфигурации сетевого интерфейса]] 
