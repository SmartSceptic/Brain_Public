---
alias: [ ]
sr-due: 2026-12-24
sr-interval: 873
sr-ease: 290
---
tag: #N/S/Stub #N/T/Conspect #N/T/Article #N/T/Public 
2021-08-17 21:58, [Source](https://raymii.org/s/tutorials/Postfix_Log_message_from_to_and_subject_headers.html),

Authors: [[]] 

Related: [[% Postfix]]

---

Есть маленькая хитрость для Postfix, которая позволяет логировать `subject`, `from`и `to` всех сообщений которые проходят через постфикс.
Это удобно, для дебага.

Сначала создайте файл `/etc/postfix/header_checks`и вставьте в него следующее:

```
/^subject:/      WARN
/^to:/           WARN
/^from:/         WARN
/^Subject:/      WARN
/^To:/           WARN
/^From:/         WARN
```

Теперь добавьте в конец файла `/etc/postfix/main.cf` следующее:

```
header_checks = regexp:/etc/postfix/header_checks
```

И перезапустите постфикс:

```
service postfix restart
```

В логах появятся подобные сообщения.

```
Dec  4 08:23:05 localhost postfix/cleanup[2278]: 90CA714: warning: header
Subject: This is a testmail which gets logged from localhost[127.0.0.1];
from=<root@localhost> to=<root@localhost> proto=ESMTP helo=<localhost>
```