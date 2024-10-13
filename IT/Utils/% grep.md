---
alias: [zgrep, egrep, global regular expression print ]
sr-due: 2026-01-04
sr-interval: 448
sr-ease: 298
---
tag: #N/S/Synthesizing #N/T/Linux #N/T/Tool/Util #N/T/Public
2021-11-30 19:58, [Source](),  
Related: [[% rigrep]] [[% Pgrep]] [[regexp]]  
Docs:
## Описание
Команда grep используется для поиска заданного текста в файле или потоке данных. Она позволяет искать совпадения по регулярному выражению ([[regexp]]) и выводить строки, содержащие найденный текст.
## Шаблон 
```bash
/usr/xpg4/bin/grep [ -E | -F ] [ -c | -l | -q ] [ -bhinsvwx ]  [ -e список_образцов ... ] -f файл_образцов ...   [ имя_файла ... ]
```
## Примеры: 
##### Очистка файла от коментариев, и пустых строк
```bash
egrep -v '(^#|^\s*$|^\s*\t*#)' !$ or filename 
```
which also excludes comment lines starting with TAB or SPACE and nothing else before the comment (frequent in config files with multi-line comments behind a setting).
##### Греп нескольких фраз, и ошибок
```bash
 tail -f /var/log/maillog |egrep  -i '(warning|error|fatal|panic|ssl)'
```
### Шпаргалки:
https://najomi.org/_nix/grep
##### Подсветки
Выбор цвета осуществляется с помощью переменной окружения **GREP_COLOR**: export GREP_COLOR=36 дает голубой цвет, а export GREP_COLOR=32 - зеленый лайм.
```bash
 grep --color=always abc a_file.txt
```
#### grep  with multiple patterns | Греп множественных паттернов
Here are all other possibilities:  
1.  Use single quotes in the pattern: grep 'pattern*' file1 file2
##### Search all text files 
```bash
grep 'word*' *.txt  
# или
grep 'word1\|word2\|word3' /path/to/file  
### Search all python files for 'wordA' or 'wordB' ###  
grep 'wordA*'\''wordB' *.py  
```
2.  Use extended regular expressions: egrep 'pattern1|pattern2' \*.py
```bash
grep -E 'word1|word2' *.doc  
```
или
##### Negative match
Use multiple patterns with grep -v. So you can print all lines in a file except those containing the multiple patterns you specify.
```bash
egrep -v 'Googlebot|msnbot-media|YandexBot|bingbot' *.log
```
If you wanted to do all in one command, you could go w/ sed instead
![[% sed#Удалить несколько патернов из файла]]

3.  Use this syntax on older Unix shells: grep -e pattern1 -e pattern2 \*.pl
#####  Grep нескольких патернов в нескольких директориях
```bash
grep -ri -e "Patter1 " -e " Pattern2 " /srv/vmail/{subdir1,subdir2,}/
```

#####  Рекурсивный поиск шаблона в группе файлов
```bash
grep -R /way/to/*any/folder/ foo
grep foo `ls -R /way/to/any*/folder/text.file`
grep foo `find /way/to/*any/folder/ -name "text.file"`
for file in /way/to/some/folder/text*.file; do grep foo $file; done
```
#####  Рекурсивно найти по папкам данное слово и вывести номер строки и путь до файла:
```bash
$ grep -nri 'foobar' *
```

![[% xargs#Объединение xargs с grep для поиска строки в списке файлов, предоставленном этой командой.]]

##### Убрать из файла file.txt все строки, содержащиеся в файле not.wanted
Из одного файла удалить все строки другого файла
```bash
grep -v -f not.wanted file.txt 
#Или
grep -F -v -f file_what_delete.txt file_where_delete.txt > result.txt
```
##### Найти вхождение числа 6 в потоке ввода и вывести его вместе с 2 строками до и одной строкой после
```bash
$ for i in {1..10} ; do echo $i; done | grep 6 -B2 -A1
```

##### Find All Email Addresses in a File using Grep
```bash
grep -E -o "\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,6}\b" filename.txt
```
##### Исключить бинарные файлы из грепа, exclude
```bash
grep -InH your-word *
```

#### Grep IP
[src](http://website-lab.ru/article/regexp/search-ip-address/)
##### Найти Все IP Адреса с Помощью Grep
Пропарсим файл и найдем в нем все IP адреса из диапазона от `0.0.0.0`до `999.999.999.999`с помощью grep:
```bash
grep -E -o "([0-9]{1,3}[\.]){3}[0-9]{1,3}" file.txt
```

##### Поиск Правильных IPv4 Адресов
Регулярное выражение для **поиска и проверки правильных IPv4 адресов**:
```bash
grep -E -o  "(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)"  file.txt
```

##### Поиск IPv6 Адресов
Регулярное выражение для проверки IPv6 адреса:
```
grep -E -o  "((^|:)([0-9a-fA-F]{0,4})){1,8}"  file.txt
```

## Используемые или важные ключи.
`--exclude-dir={node_modules,dir1,dir2,dir3}` Исключить папки из рекурсивного поиска.
-I -- process a binary file as if it did not contain matching data;  
-n -- prefix each line of output with the 1-based line number within its input file  
-H -- print the file name for each match
 Просмотр N строк после совпавшей с помощью аргумента -A.  
 Просмотр N строк перед совпавшей с помощью аргумента -B.  
 Просмотр N строк до и после совпавшей с помощью аргумента -C.
**--color** раскрасить вхождения   --color=auto, --color=always, --color=never