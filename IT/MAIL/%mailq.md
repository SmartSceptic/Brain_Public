---
alias: [ ]
sr-due: 2027-04-25
sr-interval: 945
sr-ease: 290
---
tag: #N/S/Well #N/T/Linux #N/T/Tool/Util #N/T/Public 
2021-04-05 01:05
Source: , 
Related: [[% Postfix]], [[%postqueue]]
Docs: https://www.opennet.ru/man.shtml?topic=mailq&category=1&russian=4

## Описание
Mailq распечатывает сводку почтовых сообщений, поставленных в очередь для будущей доставки.
[[Работа с очередью сообщений в Postfix]]

## Шаблон
```bash
mailq [-Ac] [-q...] [-v]  
```
## Примеры: 
##### Посмотреть очередь
```bash
mailq 
```
### Шпаргалки:
##### Посмотреть количество писем в очереди
Команда mailq в конце выдает общее количество сообщений в очереди, например:
```bash
-- 23 Kbytes in 18 Requests.
```
 в данном примере в очереди находится 18 сообщений общим объемом 23 Кбайт.
##### Попытаться доставить всю почту в очереди
```bash
mailq -q
```
Это реализуется путем выполнения команды  [[%postqueue]]
![[%postqueue#Принудительно отправить одно сообщение из очереди по id]]

#### АНАЛИЗ НЕДОСТАВЛЕННЫХ ПИСЕМ
##### Сортировка получателей в очереди
```bash
mailq | grep -v '^ *(' | awk 'NR % 3 == 0 {print}' |sort -h|uniq -c |sort -h
```
##### Сортировка ДОМЕНОВ получателей в очереди
```bash
mailq | grep -v '^ *(' | awk -F "@" 'NR % 3 == 0 {print $2}' |sort -h|uniq -c |sort -h
```
##### Сортировка отправителей в очереди
```bash
mailq | egrep "[A-Z0-9]{10}"  |awk '{print $7}' |sort -h|uniq -c |sort -h
```
или
```bash
mailq|grep ^[A-F0-9] |cut -c 42-80| sort | uniq -c| sort -n
```
##### Cортировка ДОМЕНОВ ??? в очереди
```bash
mailq | egrep "[A-Z0-9]{10}"  |awk '{print $7}' | awk -F "@" '{print $2}'|sort -h|uniq -c |sort -h
```
##### Сортировка почты по домену отправителя
```
mailq | egrep '\w{2,3}.*\d*:\d*.*@.*\..*' --color|awk '{print $7}'|cut -d@ -f2|sort|uniq -c
```

с помощью [[%postcat]]
##### 
```bash
for MAILQID in $(mailq |grep sender@domain.com |awk '{print $1}'); do postcat -hq $MAILQID |grep "^To: "; done
```
##### Формируем список айдишников, писем с ошибками на отправку
```bash
mailq |grep MAILER-DAEMON |awk '{print $1}' > /tmp/mailer-demon-mails
```
##### Какие ТЕМЫ в очереди на отпавку
```bash
while read LINE; do postcat -hq $LINE |grep "Subject"; done < /tmp/mailer-demon-mails | uniq -c
```
##### Какие ПОЛУЧАТЕЛИ в очереди
```bash
while read LINE; do postcat -hq $LINE |grep "To: "; done < /tmp/mailer-demon-mails |sort |uniq -c |sort -h
```
##### Какие ДОМЕНЫ в очереди на отрправку
```bash
while read LINE; do postcat -hq $LINE |grep "To: " | awk -F "@" '{print $2}'; done < /tmp/mailer-demon-mails |sort |uniq -c |sort -h
```
##### Кому недоставлена почта
```bash
while read LINE; do postcat -bq $LINE |grep ">: user unknown"; done < /tmp/mailer-demon-mails |sort -h |uniq -c |sort -h
```
##### Sort by from address :
```bash
mailq | awk '/^[0-9,A-F]/ {print $7}' | sort | uniq -c | sort -n
```


#### Удаление писем из очереди
##### To remove all mails sent by [user@adminlogs.info](mailto:user@adminlogs.info) from the queue :
```bash
mailq| grep '^[A-Z0-9]'|grep [user@adminlogs.info|cut](mailto:user@adminlogs.info|cut) -f1 -d' ' |tr -d \*|postsuper -d -
```
##### To remove all mails being sent using the From address “[user@adminlogs.info](mailto:user@adminlogs.info)” :
```bash
mailq | awk '[/^[0-9,A-F].*user@adminlogs.info](mailto:/%5e[0-9,A-F].*user@adminlogs.info) / {print $1}' | cut -d '!' -f 1 | postsuper -d -
```

с помощью [[%postsuper]]
##### Удалить все письма из очереди с сообщениями об ошибке от mailer daemon
(убирая возможные звездочки из mailq id)  
```bash
mailq |grep MAILER-DAEMON |awk '{print $1}' | sed 's/*//' | postsuper -d -
```
##### Удалить все письма из очереди на определенный домен или пользователя
(убирая возможные звездочки из mailq id)  
```bash
mailq | grep -v "Queue ID" | while read line; do if [ "1" != `echo $line | wc -m` ]; then echo -n " "$line; else echo""; fi ; done | grep "Connection frequency limited" | grep '@news.ubisoft.com' | awk '{print $1}' | sed 's/*//' | postsuper -d -
```

С помощью [[%postqueue]]
##### Удаление всех писем в очереди для конкретного домена
```bash
postqueue -p | tail -n +2 | awk 'BEGIN { RS = "" } [/@example\.com/](mailto:/@example\.com/) { print $1 }' | tr -d '*!' | postsuper -d -
```
##### Удалить все письма в очереди От: определенного адреса
```bash
postqueue -p | tail -n +2 | awk 'BEGIN { RS = "" } /username@example\.com/ { print $1 }' | tr -d '*!' | postsuper -d -
```

##### To remove all mails sent by the domain [adminlogs.info](http://www.adminlogs.info/) from the queue : 
```bash
mailq| grep '^[A-Z0-9]'|grep @adminlogs.info|cut -f1 -d' ' |tr -d \*|postsuper -d -
```

## Используемые или важные ключи.
Также, очередь можно посмотреть командами:
```bash 
find /var/spool/postfix/deferred -type f | wc -l
find /var/spool/postfix/active -type f | wc -l
find /var/spool/postfix/incoming -type f | wc -l
find /var/spool/postfix/defer -type f | wc -l
```
 данные каталоги являются местом, где временно хранятся письма очереди.
 
### Файлы