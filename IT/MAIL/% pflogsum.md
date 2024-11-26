---
alias: [ ]
sr-due: 2026-05-17
sr-interval: 560
sr-ease: 273
---
tag: #N/S/Rare #N/T/Linux #N/T/Tool/Util #N/T/Public 
2021-02-16 18:13
Source: ,
Related: [[Mail  monitoring]],
Docs: https://github.com/KTamas/pflogsumm/blob/master/pflogsumm.pl
https://linux.die.net/man/1/pflogsumm

## Pflogsumm
Удобная утилита (скрипт на **Perl**\-е) для подсчёта статистики работы почтового сервера._
## Установка 
```bash
yum  install postfix-perl-scripts
```
## Шаблон
```bash
./pflogsumm.pl /var/log/maillog | tee -a eximstat.log
```
## Примеры: 
```bash

pflogsumm.pl: "no_bounce_detail" is depreciated, use "bounce_detail=0" instead
pflogsumm.pl: "no_deferral_detail" is depreciated, use "deferral_detail=0" instead
pflogsumm.pl: "no_reject_detail" is depreciated, use "reject_detail=0" instead
pflogsumm.pl: "no_smtpd_warnings" is depreciated, use "smtpd_warning_detail=0" instead

```
### Шпаргалки:
###### Запуск с указанием полных путей
```bash
/usr/bin/perl /var/log/pflogsumm.pl -u 25 -h 8 -e  /var/log/maillog  |less
```
######  Ограничить количесво хостов/юзеров до топ 25
```bash
 pflogsumm --detail 25 /var/log/maillog |less
```

###### Вывести ограниченное количество хостов и пользователей с полными ошибками.
```bash
 pflogsumm --detail 30 -u 25 -h 8 --problems_first --verbose_msg_detail /var/log/maillog |less 
 ```
## Используемые или важные ключи.
```
--detail <cnt>

			   Sets all --*-detail, -h and -u to <cnt>.  Is
	   over-ridden by individual settings.  --detail 0
	   suppresses *all* detail.
-e             extended (extreme? excessive?) detail
	   Emit detailed reports.  At present, this includes
	   only a per-message report, sorted by sender domain,
	   then user-in-domain, then by queue i.d.
			   WARNING: the data built to generate this report can
			   quickly consume very large amounts of memory if a
	   lot of log entries are processed!
-h <cnt>       top <cnt> to display in host/domain reports.
	   0 = none.
	   See also: "-u" and "--*-detail" options for further
		 report-limiting options.
-u <cnt>       top <cnt> to display in user reports. 0 == none.
	   See also: "-h" and "--*-detail" options for further
	 	report-limiting options.
--verbose-msg-detail
			   For the message deferral, bounce and reject summaries:
			   display the full "reason", rather than a truncated one.
			   Note: this can result in quite long lines in the report.
```
### Файлы