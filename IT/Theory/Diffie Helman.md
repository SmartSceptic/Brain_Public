---
aliases:
  - Создание hi (Diffie Hellman) сертификата
---
tag: #N/S/Stub #N/T/Conspect #N/T/Article  #N/T/Public 
2024-09-27 21:00, [Source](https://maxrival.com/sozdaniie-dh-diffie-hellman-siertifikata/),  
Authors: [[]]   
Related: [[]] 

Если вы в данный момент занимаетесь настройкой [[!& SSL  TLS|SSL]] сертификата для вашего домена, то скорее всего уже слышали о том, что безопасность сайта можно улучшить не только SSL сертификатом, но и специальным ключом, использующий алгоритм Диффи Хельмана. Данный ключ может использоваться не только в работе сайта, но и в любом другом случае, где применяются технологии шифрования. Одним из ярких примеров, где применяются DH ключи является программа OpenVPN.

Ниже мы рассмотрим процесс создания DH ключа и подключения ключа к популярным веб, почтовым и VPN серверам.
(!) Все операции производятся на операционной системе Linux Centos 7.

Для генерации ключа используется пакет [[% OpenSSL]], который присутствует во всех linux системах.

![[% OpenSSL#Генерация ключа Diffie Helman]]

## Использование
 Ниже приведены выдержки из конфигурационных файлов с указанием строк для подключение DH ключа к веб, почтовым и VPN серверам.

Для подключения DH ключа необходимо два параметра:    
 (1) включить безопасные шифры(чиперы)  
 (2) подключить ключ, который сгенерирован на предудущем этапе.

### [[% Nginx]]
Настройка производится в конфигурационном файле `/etc/nginx/sites-enabled/example.com`
(!) путь и имя конфигурационного файла может отличаться, так как Nginx позволяет производить include дополнительных конфигурационных файлов в основной.
```bash
#Добавляем данные о ключе
ssl_dhparam path/file;  
# данные о чиперах
ssl_prefer_server_ciphers on;  
ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';  
```
и перезагружаем сервис
```bash
systemctl restart nginx  
```
(!) Стоит обратить внимание на параметр ssl_prefer_server_ciphers on; данный парамер непосредственно включает чтение списка чиперов, а после уже идет список чиперов.

### Apache
Настройка производится в конфигурационном файле `/etc/httpd/conf/httpd.conf`

(!) путь и имя конфигурационного файла может отличаться, так как Apache позволяет производить include дополнительных конфигурационных файлов в основной конфигурационный файл.

(!)Стоит также обратить внимание еще на тот момент, что расположение файла может отличаться в зависимости от операционной системы. Так например в операционных системах Debian/Ubuntu данный файл будет располагаться по следующему пути /etc/apache/httpd.conf, либо /etc/apache2/httpd.conf
```bash
# Добавляем данные о ключе
SSLDHParametersFile /path/dh.file  
# данные о чиперах
SSLHonorCipherOrder on  
SSLCipherSuite ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA  
```
### Lighttpd 
Настройка производится в конфигурационном файле /etc/lighttpd/lighttpd.conf
```bash
# Добавляем данные о ключе
ssl.dh-file="path/file"  
# данные о чиперах
ssl.cipher-list = "ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA "  
```
и перезагружаем сервис
```bash
systemctl restart lighttpd  
```
### Sendmail
Настройка производится в конфигурационном файле /etc/mail/sendmail.mc в секции LOCAL_CONFIG
```bash
# Добавляем данные о ключе
O DHParameters=path/file  
# данные о чиперах
O CipherList=ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA  
```
и перезагружаем сервис
```bash
systemctl restart sendmail  
```

### [[% Postfix]]
Настройка производится в конфигурационном файле /etc/postfix/main.cf
```bash
# Добавляем данные о ключе
smtpd_tls_dh1024_param_file = path/file  
# данные о чиперах
smtpd_tls_exclude_ciphers = aNULL, eNULL, EXPORT, DES, RC4, MD5, PSK, aECDH, EDH-DSS-DES-CBC3-SHA, EDH-RSA-DES-CDC3-SHA, KRB5-DE5, CBC3-SHA  
```
и перезагружаем сервис
```bash
systemctl restart postfix  
```
### OpenVPN
Настройка производится в конфигурационном файле /etc/openvpn/server.conf
Добавляем данные о ключе
```bash
dh "path/file"  
```
и перезагружаем сервис
```bash
systemctl restart openvpn@server  
```
(!) Включение дополнительных параметров для работы DH ключа не требуется.
### Дополнительные заметки:
(!) Для работы с DH ключом необходимо обязательно включить SSL режим работы.
(!) Списки чиперов, указанные в примерах, не являются окончательными и можгут меняться, в зависимости от текущих уязвимостей протоколов и ваших потребностей.