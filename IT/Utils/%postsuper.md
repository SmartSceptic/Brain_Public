---
alias: [ ]
sr-due: 2026-10-01
sr-interval: 829
sr-ease: 291
---
tag: #N/S/Medium #N/T/Linux #N/T/Tool/Util #N/T/Public
2021-04-05 01:42
Source: ,
Related: [[% Postfix]], [[%mailq]]
Docs: https://www.opennet.ru/man.shtml?topic=postsuper&category=1&russian=0

## Описание
Команда postsuper предназначена для обслуживания очереди Postfix. Эта команда выполняется только суперпользователем.
## Шаблон
```bash
postsuper [-psv] [-d queue_id] [-r queue_id] [directory ...]  
```
## Примеры: 
######   Удалить определенное письмо из очереди:
```bash
postsuper -d <идентификатор письма>
```
* идентификатор письма можно увидеть командой mailq.
### Шпаргалки:
##### Удалить письма из очереди
```bash
postsuper -d ALL
```
   Удалить определенное письмо из очереди:
```bash
postsuper -d XXXXXXXXXX
```
##### To remove all mails in the deferred queue :  
```bash
  postsuper -d ALL deferred
```
##### Перезапустить очередь
```bash
postsuper -r ALL
```
Если не помогло, поочередно:
```bash
postfix stop
postsuper -r ALL
postfix start
```

##### Постановка письма на «удержание» (перевод в режим hold – postfix не будет пытаться отправить письмо получателю в таком режиме)  
```
# postsuper -h <ID-сообщения>
```

```
# postsuper -h ALL
```
-  все сообщения перевести в режим hold  
```
# postsuper -h deferred
```
- все письма с очереди deferred перевести в режим hold  
##### Снятие письма с режима «удержание»  
```
# postsuper -H <ID-сообщения>
```

```
# postsuper -H ALL
``` 
– все сообщения перевести с режима hold в режим deferred


## Используемые или важные ключи.
### Файлы