---
alias: [ ]
sr-due: 2026-10-16
sr-interval: 838
sr-ease: 294
---
tag: #N/S/Rare  #N/T/Linux #N/T/Tool/App  #N/T/Public 
2022-07-06 13:53, [Source](),  
Related: [[]],  
Docs:  
[[=Установка и настройка PostfixAdmin]]

## Описание
_о чем заметка в 2-3 предложения_  

#### Альтернативы postfixamdin 

-   [http://lostapp.ru/soft/vimbadmin](http://lostapp.ru/soft/vimbadmin)
-   [https://www.ratuus.org/](https://www.ratuus.org/)
-   [https://demo.modoboa.org/accounts/login/?next=/](https://demo.modoboa.org/accounts/login/?next=/)      +написано на питоне комплексное решение[https://www.iredmail.org/admin_panel.html](https://www.iredmail.org/admin_panel.html)   -бесплатная версия ограничена, платная 400$ в год  +есть демка [https://www.iredmail.org/admin_demo.html](https://www.iredmail.org/admin_demo.html) +дашборды +

## Шаблон 
##### Увеличить число строк в логе
в /config.inc.php
```bash
$CONF['page_size'] = '999';
```
## Примеры: 
######  
```bash

```
### Шпаргалки:

#### Maladmin Mysql
######  Создать почтовый ящик через mysql 
postfix create mailbox
```bash
# Получаем список доменов из почтовой базы
mysql -B --disable-column-names postfix -e "select domain from domain" | grep -v ALL

#Генерируем md5 хеш из пароля для ящика
doveadm  pw -s md5 -p 'password'
# Создаем ящик c сгенерированным ранее паролем
INSERT INTO mailbox (username, password, maildir, quota, local_part, domain, active) VALUES ('non-existent-mailboxes@domain.com', 'MD5-HASH-HERE', 'domain.comm/non-existent-mailboxes/', '0', 'non-existent-mailboxes', 'domain.com', '1');
# Прописываем, куда доставлять или передаем почту:
insert into alias (address,goto,domain,active) values ('non-existent-mailboxes@domain.com', 'non-existent-mailboxes@domain.com, aother-forward-mailbox@domain.com', 'domain.com', '1');

#Проверка
mysql postfix -e "select * from mailbox where username like 'vladlen%' \G"
mysql postfix -e "select * from alias where address like 'vladlen%' \G"
```
##### Созать почтовыя ящик в базе, для каждого домена (с перетиранием существующих)
```bash
#Добавляем ящики пользователя в каждый домен
mysql postfix -e "REPLACE INTO mailbox (username, password, maildir, quota, local_part, domain, active) SELECT CONCAT ('vladlen.ch@',domain), '\$1\$c9809462\$GAV.TRCqxOAs4nSXesJJm1', concat(domain,'/vladlen.ch/'), 0, 'vladlen.ch', domain, 1 FROM domain;"
# Добавляем соответствующий алиасы для ящика пользователя
 mysql postfix -e "REPLACE INTO alias (address,goto,domain,active) SELECT CONCAT('vladlen.ch@',domain), CONCAT('vladlen.ch@',domain, ', vladlen.ch@main-domain.com'), domain, 1 FROM domain;"

#Создаем обязательные, согласно RFC 2142 алиасы, на примере dmarc-reports
mysql postfix -e "REPLACE INTO alias (address,goto,domain,active) SELECT CONCAT('dmarc-reports@',domain), CONCAT('root@',domain), domain, 1 FROM domain;"

#очищаем возможные лишние дубликаты.
mysql postfix -e "delete from mailbox where username like '%@ALL'\G" 
mysql postfix -e "delete from alias where address like '%@ALL'\G"

```

#####  Добавить админа в постфиксадмин бд
Есть 3 способа: 
1 через уже существующиего админа в веб интерфейсе
2. скриптом
```bash
/var/www/postfixadmin/scripts/postfixadmin-cli admin add vladlen.ch@domain.com --superadmin 1 --active 1 --password Pass --password2 Pass
```
Или сгенерировать новый пароль и завести нового админа
```bash

echo -n 'pass' | md5sum
doveadm pw -s MD5-CRYPT -p password
 
mysql postfix -e ''INSERT INTO admin (`username`, `password`, `superadmin`, `active`) VALUES ('root@domain.com', 'MD5_hash_pass_here', '1', '1');"
```
Заменить пароль для существующего  админа
```bash
UPDATE `admin` SET `password` = MD5('password') WHERE `admin`.`username` = 'root@localhost'; 
```

##### Посмотреть адреса получателя в баже
```bash
select * from alias where address = '[abuse@odomain.com](mailto:abuse@domain.com)';
```

##### Добавить  в  список  рассылки  еще  один  адресс  
```bash
update alias set goto = abuse@domain.com, admin@domain.com'  where address = 'abuse@domain.com';
```

#### Обновить пароль для почтового ящика
```bash
select * from mailbox where username = 'igor';
```

![[Linux Users and Security#создание Пароля пользователям]]

![[% doveadm pw#Получить md5 хеш пароля]]
```bash
update mailbox set password = '$1$vQ2NMUtv5ftjon/wej4J1' where username = 'user@domain.com';
```

Проверить доступ можно телнетом  на порт и введя пароль
```bash
a login vladlen.ch@vladlen.ch qwerty123
a select INBOX
a search body "user"
```



## Используемые или важные ключи.
### Файлы  
