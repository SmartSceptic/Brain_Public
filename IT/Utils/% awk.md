---
alias: [ ]
sr-due: 2029-01-03
sr-interval: 1717
sr-ease: 313
---
tag: #N/S/Stub #N/T/Linux #N/T/Tool/Util #N/T/Public 
2023-01-29 14:25, [Source](),  
Related: [[]],  
Docs:  

## Описание
AWK используется для обработки и анализа текстовых файлов. Он может использоваться для извлечения данных, фильтрации, форматирования и преобразования текстовых данных в различных форматах. AWK часто используется в Unix-системах для автоматизации задач обработки данных.
## Шаблон 
```bash

```
## Примеры: 
##### 
```bash

```
### Шпаргалки:

##### Способы установить нестандартный (непробельный) разделитель столбцов
Пример установки символа ":" как разделителя столбцов и вывода первого столбца на примере строки `1:2:3`
```bash
awk -F: '{print $1}' <<< "1:2:3"
```
или
```bash
awk -v FS=: '{print $1}' <<< "1:2:3"
```
или
```bash
awk '{print $1}' FS=: <<< "1:2:3"
```
или 
```bash
awk 'BEGIN{FS=":"} {print $1}' <<< "1:2:3"
```
Разделитель может быть регулярным выражением и содержать НЕСКОЛЬКО разделителей
```bash
awk -F"/|=" -vOFS='\t' '{print $3, $5, $NF}' file
```
или  вывести в скобочках:
```bash
grep "orig_to=<user@domain.com>" /var/log/maillog | awk -F"[<>]" '{print $2}' |wc
```
или в круглых
```bash
awk -F"[()]" '{print $2}' filename
```
Для печати в квадратных скобках, использовать
```bash
awk -F'[][]'
```
`awk -F'[[]]'` -  не работатет
#####  Удалиьт первую колонку 
[src](https://stackoverflow.com/questions/32812916/how-to-delete-the-first-column-which-is-in-fact-row-names-from-a-data-file-in) 
Приравняв ее содержимое кнулю
```bash
awk '{$1=""}1' input | awk '{$1=$1}1' > output
```
#####  Показать количество строк в файле
```bash
awk 'END { print NR }' programming.txt 
10
```

##### Вывести сумарный размер избраных файлов | Summarize filesize in linux
[src](https://unix.stackexchange.com/questions/72661/show-sum-of-file-sizes-in-directory-listing]https://unix.stackexchange.com/questions/72661/show-sum-of-file-sizes-in-directory-listing)
```bash
dir () { ls -FaGl "${@}" | awk '{ total += $4; print }; END { print total }'; }
```

#### Convert between byte count and “human-readable” string
[src](https://stackoverflow.com/questions/37015073/convert-between-byte-count-and-human-readable-string)
##### Перевести различные форматы значений размера файлов, в байты
human readeble to bytes   [src](![[Pasted image 20230504203747.png]])
```bash
awk 'function pp(p){printf "%u\n",$0*1024^p} /[0-9]$/{print $0}/K$/{pp(1)}/M$/{pp(2)}/G$/{pp(3)}/T$/{pp(4)}/[^0-9KMGT]$/{print 0}'
```
**объяснение**
```bash
function pp(p) { printf "%u\n", $0 * 1024^p }
```
Define a function named pp that takes a single parameter p and prints the $0 multiplied by 1024 raised to the p-th power. The %u will print the unsigned decimal integer of that number.
```bash
/[0-9]$/ { print $0 }
```
Match lines that end with a digit (the $ matches the end of the line), then run the code inside the { and }. Print the entire line ($0)
```bash
/K$/ { pp(1) }
```
Match lines that end with the capital letter K, call the function pp() and pass 1 to it (p == 1). NOTE: When $0 (e.g. "1.43K") is used in a math equation only the beginning numbers (i.e. "1.43") will be used below. Example with $0 = "1.43K"
```bash
$0 * 1024^p == 1.43K * 1024^1 == 1.43K * 1024 = 1.43 * 1024 = 1464.32

/M$/ { pp(2) }
```
Match lines that end with the capital letter M, call the function pp() and pass 2 to it (p == 2). Example with $0 == "120.3M"
```bash
$0 * 1024^p == 120.3M * 1024^2 == 120.3M * 1024^2 == 120.3M * 1024*1024 = 120.3 * 1048576 = 126143692.8
```
etc... for G and T
```bash
/[^0-9KMGT]$/ { print 0 }
```
Lines that do not end with a digit or the capital letters K, M, G, or T print "0".
**Example**:
```bash
$ cat dehumanise  
937  
1.43K  
120.3M  
5G  
933G  
12.2T  
bad  
<>
```
**Results**:
```bash
$ awk 'function pp(p){printf "%u\n",$0*1024^p} /[0-9]$/{print $0}/K$/{pp(1)}/M$/{pp(2)}/G$/{pp(3)}/T$/{pp(4)}/[^0-9KMGT]$/{print 0}' dehumanise  
937  
1464  
126143692  
5368709120  
1001801121792  
13414041858867  
0  
0
```
##### Преобразовать обратно в human readable
[src]([https://stackoverflow.com/questions/37015073/convert-between-byte-count-and-human-readable-string](https://stackoverflow.com/questions/37015073/convert-between-byte-count-and-human-readable-string))
```bash
# To:
echo "163564736" | numfmt --to=iec
# From:
echo "156M" | numfmt --from=iec
```

## Используемые или важные ключи.
### Файлы 
