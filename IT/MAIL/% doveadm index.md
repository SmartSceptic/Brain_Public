---
alias: [ ]
sr-due: 2023-04-08
sr-interval: 62
sr-ease: 299
---
tag: #N/S/Synthesizing  #N/T/Tool #N/T/Linux #N/T/Tool/Util
2021-02-15 22:17
Source: ,
Related: [[]],
Docs: https://wiki.dovecot.org/Tools/Doveadm/Index


## Описание
_Добавить неиндексированные сообщения из почтового ящика в файл индекса / кеша. Если включен полнотекстовый поиск, добавьте также неиндексированные сообщения в базу данных [[FTS]]._

Кэширование добавляет только те поля, которые были ранее добавлены к решениям о кешировании почтового ящика, поэтому оно не будет делать ничего полезного для почтовых ящиков, к которым клиент пользователя еще не получил доступ. Вы можете использовать [[% doveadm dump]] чтобы показать текущие решения по кэшированию конкретного почтового ящика.

## Шаблон
```bash
doveadm [ -Dv ] index [ -S socket_path ] [ -q ] [ -n max_recent ] почтовый ящик
doveadm [ -Dv ] index [ -S socket_path ] -A [ -q ] [ -n max_recent ] почтовый ящик
doveadm [ -Dv ] index [ -S socket_path ] -F file [ -q ] [ -n max_recent ] почтовый ящик
doveadm [ -Dv ] index [ -S socket_path ] -u  user [ -q ] [ -n max_recent ] почтовый ящик
```
## Примеры: 
```bash
doveadm index -u bob INBOX
```
### Шпаргалки:
##### Repush data to solr (update index)
Используется вместе с rescan ![[% doveadm fts#Fts Rescan]]
- Предварительно, Может понадобится ресинк
```bash
**doveadm force-resync -u vladlen.ch INBOX**
```
- For one user INBOX only
```bash
doveadm fts rescan -u user@domain && doveadm index -u user@domain -q INBOX
```
- For one user IINBOX and ALL SUBFOLDER
```bash
doveadm fts rescan -u user@domain && doveadm index -u user@domain -q '*'
```
- For all users INBOX and ALL SUBFOLDER
```bash
doveadm fts rescan -A && doveadm index -A -q '*'
```
- For all users INBOX and ALL SUBFOLDER with force-resync
```bash
doveadm force-resync && doveadm fts rescan -A && doveadm index -A -q '*'
```
- verbose
```bash
doveadm -Dv index -u user@domain -q INBOX
```
- Запустить паралельно
```bash
find ./ -maxdepth 1 -printf "%f@domain.com\\n" | grep -v "./" | parallel -j 40 doveadm -v index -q \\'\*\\' -u {}
```

 
## Используемые или важные ключи.

-d
### Файлы



