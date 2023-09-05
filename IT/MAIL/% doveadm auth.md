---
alias: [ ]
sr-due: 2023-04-06
sr-interval: 60
sr-ease: 294
---
tag: #N/S/Stub #N/T/Tool #N/T/Linux #N/T/Tool/Util #N/T/Public 
2021-03-05 19:45
Source: ,
Related: [[]],
Docs:**https://wiki.dovecot.org/Tools/Doveadm/Auth**

## Описание
_Test authentication for a user._
dd if=/var/lib/dovecot/ssl-parameters.dat bs=1 skip=88 | openssl dhparam -inform der > /etc/dovecot/dh.pem
## Шаблон
```bash
doveadm [-Dv] auth [-a auth_socket_path] [-x auth_info] user [password]
```
## Примеры: 
```bash
doveadm auth test -x service=imap -x rip=192.0.2.143 john
```
### Шпаргалки:
- **Test SASL with Dovecot** - Проверить креды для довекота. (этот-же сокет испольщуется для аутентификации постфикса)
```bash
doveadm -Dv auth test -a /var/spool/postfix/private/auth test@vladlen.ch qwerty123
```

## Используемые или важные ключи.
	 -D Включает подробные сообщения и отладочные сообщения.
	 -v Включает подробный вывод, включая счетчик прогресса.
### Файлы

