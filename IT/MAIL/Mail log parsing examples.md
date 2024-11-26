---
aliases:
  - Примеры парсинга почтовых логов postfix
  - Примеры парсинга почтовой mailadmin базы
---

-  Почта старше гоада архивируется, старше пяти-семи лет удаляется
[[Migrate mails to microsoft]]
  2   

[[Работа с очередью сообщений в Postfix]]
[[%mailq]]
#### [[% Postfix]] log analyze

##### Вывести статусы сообщений:
Принято | отправлено | отброшено, etc..
```bash
tail -f /var/log/maillog |grep "status="
```
##### Выводить новые ошибки из мейлога:
```bash
tail -f /var/log/maillog |egrep  -i '(warning|error|fatal|panic)'
```
##### Ящики получателей 
```bash
grep "=sent " /var/log/maillog | awk '{print $7}'  | sed 's/to=<//g;s/>,//g' | sort | uniq -c | sort -nk 1 |grep -v gmail
```
##### Домены получателей
```bash
grep "=sent " /var/log/maillog-20200412 | awk '{print $7}'  | sed 's/to=<//g;s/>,//g' |awk -F@ '{print $2}'| sort | uniq -c | sort -nk 1
```

##### Вывести статусы сообщений из мейллога:
```bash
CUR_DATE="Aug 31" && \
echo -e "\n User unknown in virtual mailbox top errors " && \
cat /var/log/maillog |grep "$CUR_DATE" |grep " User unknown in virtual mailbox" |awk '{print $13 }' |sort -h |uniq -c |sort -h |tail &&\
echo -e ' \nRelay access denied  top errors' &&\
cat /var/log/maillog |grep "$CUR_DATE" |grep "Relay access denied" |awk '{print $13 }' |sort -h |uniq -c |sort -h &&\
echo -e ' \n Domain not found  top errors ' &&\
cat /var/log/maillog |grep "$CUR_DATE" |grep " Domain not found " |awk '{print $7, $8, $19 }' |sort -h |uniq -c |sort -h &&\
echo -e ' \n Domain not found;  top errors' &&\
cat /var/log/maillog |grep "$CUR_DATE" |grep " Domain not found;" |awk '{print $10, $13 }' |sort -h |uniq -c |sort -h &&\
echo -e ' \n ANOTHER top deferred errors' &&\
cat /var/log/maillog | grep "$CUR_DATE" |grep  status=deferred |awk '{print $5, from, $7}' FS=: |sort -h |uniq -c |sort -h |tail -n6
```

```bash
zcat  /var/log/maillog-2022{09,10,11,12}* | pflogsumm --detail 0 -u 999999 -h 30 --no_no_msg_size -q > /tmp/report
```

```bash
top 100 @domain.com Recipients by message count

zgrep -e "orig_to=<[A-Za-z0-9._%+-]*@domain.com" /var/log/maillog-2022{09,10,11,12}* > /tmp/orig-to-report

cat  /tmp/orig-to-report | awk '{print $8}' | sed 's/orig_to=<//g' |sed 's/>,//g' |  sort -h | uniq -c | sort -h   > /tmp/orig-to-report-sort

```

##### Несколько запросов для вытягивания из мейллогов, адресов, на которые не доставляется почта.
```bash
zcat /var/log/maillog-20230* | grep "Domain not found"  |awk '{print $13}' |sort -h |uniq -c |sort -h

zcat /var/log/maillog-20230* | grep "The email account that you tried to reach does not exist" |awk '{print $7 }' | sort -h |uniq -c | sort -h

zcat /var/log/maillog-20230* | grep " user does not exist " |awk '{print $7 }' | sort -h |uniq -c | sort -h

zcat /var/log/maillog-20230* | grep " The email account that you tried to reach is over quota and inactive" |awk '{print $7 }' | sort -h |uniq -c | sort -h
```


##### Other
```bash
 rm -f /tmp/sep-to-* ; for log in /var/log/maillog-2022{09,10,11}* ; do cd /tmp/splied/ && rm -f /tmp/splied/* &&  zcat $log | split -l 5000;  for log_part in /tmp/splied/* ; do  zgrep -e 'orig_to=<[A-Za-z0-9._%+-]*@domain.com' $log_part > /tmp/sep-to-com; cat /tmp/sep-to-com | awk '{print $6}' | sort | uniq  > /tmp/sep-to-com-ids; zgrep -f /tmp/sep-to-com-ids $log_part | grep -e "from=<" | tee -a /tmp/sep-to-result ; done ;

cat /tmp/sep-to-result | awk '{print $7}'  | sed 's/from=<//g' |sed 's/>,//g' |sort |uniq -c |sort  -h  | grep -v netsuite | tail -n 100
```

##### Списсок папок (дат, адресов )
```bash
for domain in jirasupport test_for_empty test_2_for_cash test_for_old_vendor_info; do for folder in cur new .Sent; do  ll -d -tr  /srv/vmail/domain.com/$domain/$folder/ | tail -n1  ; done;  done

```

##### Вывести cписок уникальных SMTP логинов и количество их повторений, отсортированный по убыванию.
```bash
zgrep "sasl_username="  /var/log/maillog-202409* | awk '{print $9}' | sed 's/sasl_username=//' |sort -h |uniq -c |sort -nk1 -r
```
#### [[! %Dovecot]] log analyze
##### Посмотртеть отформатированыый список количества подключений к серверу (довекоту) за дату
```bash
cat /var/log/dovecot/info.log | grep -P '^2030-11-19\s*' | grep 'Info: Login' | awk '{print $6}' | sed 's/user=<//g;s/>,//g' | sort | uniq -c | sort -rn | less
```
если лог в старом формате
```bash
 zcat /var/log/dovecot.log-2030111*  | grep 'Login: ' | awk '{print $8}' | sed 's/user=<//g;s/>,//g' | sort | uniq -c | sort -rn
```
##### Посмотртеть отформатированыый список количества ОТБРОШЕННЫХ попыток подключений к серверу (довекоту) за дату
```bash
 tac /var/log/dovecot/info.log | grep -P '^2019-05-*' | grep 'Info: Aborted login' | awk '{print $14}' | sed 's/user=<//g;s/>,//g' | sort | uniq -c | sort -rn | less
```
##### Список адресов с которых конектился пользователь.
```bash
grep 'pop3-login: Info: Login: [user=<viktoriia.kr@domain.com](mailto:user=%3cviktoriia.kr@domain.com)>' dovecot/dovecot.log | awk '{print $8}' | sort | uniq -c | sort | less
```
##### Вывести список айпи адресов, с которых заходил пользователь за заданное время
```bash
tail -n 100000 /var/log/dovecot/dovecot.log  |grep "Login: user=<user@domain.com>" | grep "2018-03-" | awk '{print $8}'  |sort | uniq –c | less
```


## [[% PostfixAdmin]]
##### Алиасы и мейлбоксы
```bash
#алиасы
mysql postfix -e "SELECT address, goto FROM alias WHERE goto NOT LIKE CONCAT('%', address, '%') INTO OUTFILE '/var/lib/mysql-files/export-aliases-to-microsof.csv'  FIELDS TERMINATED BY ';' ENCLOSED BY ' ';"
#мейлбоксы
mysql postfix -e "SELECT address, goto FROM alias WHERE goto LIKE CONCAT('%', address, '%') INTO OUTFILE '/var/lib/mysql-files/export-mailboxes-to-microsof.csv'  FIELDS TERMINATED BY ';' ENCLOSED BY ' ';"
```
##### РАзмер ящиков 
```bash
mysql postfix -e "SELECT local_part FROM mailbox INTO OUTFILE '/var/lib/mysql-files/export-mailboxes-to-microsof.csv'  FIELDS TERMINATED BY ';' ENCLOSED BY ' ';"

while read LINE; do (echo -n "mailbox $LINE has: " >> /tmp/microsoft_migration_size.txt; du -sh /srv/vmail/domain/$LINE >> /tmp/microsoft_migration_size.txt); done < /var/lib/mysql-files/export-mailboxes-to-microsof.csv
```

#### Логины
##### Количество логинов за пол года
```bash
while read -r mailbox; do  zgrep "Info: Login: user=<$mailbox" /var/log/dovecot/info.log-20230* | awk '{print $6}' | sed 's/user=<//g;s/>,//g' | sort | uniq -c >> /tmp/mailboxes_login_since_2023.txt ; done < /tmp/mailboxes.txt
```
##### Число логинов, тип дата  и адрес последнего логина.
```bash
 log_file=/tmp/mailboxes__last_logins_since_08_2023.txt && while read -r mailbox; do echo -en "\n$mailbox ">> $log_file && zgrep -h "Info: Login: user=<$mailbox" /var/log/dovecot/info.log-2023080* | awk '{print " ", $3, $8, $1";"$2 }' |tee  >(tail -n 1 >> $log_file)    >(wc -l |{ read -r count; echo -n "$count";} >> $log_file ); sleep 0.3; > /dev/null ;done < /tmp/mailboxes.txt
```

