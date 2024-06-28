---
alias: [ ]
sr-due: 2025-04-02
sr-interval: 282
sr-ease: 270
---
tag: #N/S/Synthesizing #N/T/Linux #N/T/Tool/Util #N/T/Public
2021-08-10 13:34, [Source](),

Related: [[% awk]],

Docs: [aidalinux](https://aidalinux.ru/w/Sed)

## Описание


## Шаблон 
```bash

```
## Примеры:
###### 
```bash

```
### Шпаргалки:
##### Заменить несколько  паттернов в нескольких файлах.
```bash
sed -i -e 's/pass/passs2/g' -e  's/localhost/192.168.128.14/g' /etc/postfix/mysql/* /etc/dovecot/dovecot-sql.conf
```
##### Сед для каждого файла в списке файлов.
```bash
cat filelist | xargs -I{} sed -i '...' {}
```
[[% xargs]]
##### Строку №5 переместить на №15
```bash
sed -e '5{h;d;};15G' -i '' <файл>
5 — сопоставление адреса строки 5
{ — начало блочной команды
h — сохраняет строку в буфер
d — удаляет её из вывода
} — конец блочной команды
15 — сопоставление строки 15
G — добавляет строку из буфера в конец текущей строки
```

##### заменить строку, если найдена, или добавить в конец файла, если не найдена (правка конфигов)
```bash
sed -n -e '/^FOOBAR=/!p' -e '$aFOOBAR=newvalue' infile
```
При таком подходе вы просто печатаете все, кроме линии, которую хотите отбросить, а затем в конце добавляете замену.

``` bash
sed '/^FOOBAR=/{h;s/=.*/=newvalue/};${x;/^$/{s//FOOBAR=newvalue/;H};x}' infile
```
если строка соответствует, просто скопируйте ее в `h`старое пространство, затем `s`замените значение.  
В конце `$`строки `x`измените удерживающее пространство и пространство образца, затем проверьте, является ли последнее пустым. Если он не пустой, значит, замена уже сделана, так что ничего не поделать. Если он пуст, это означает, что совпадение не найдено, поэтому замените пространство шаблона на требуемую _переменную = значение, а_ затем добавьте к текущей строке в буфере удержания. Наконец, `x`снова изменим:

###### Добавить в начало и конец файла
```bash
sed -i -e '1i Header' -e '$a Trailor' sa

-i:
Edits the file in place
-e script:
Add the script to the commands to be executed
'1i Header':
Match 1st line ('1') and insert ('i') 'Header'
'$a Trailor':
Match last line ('$') and append ('a') 'Trailor'
```

#### Вывести строку по номеру
```bash
sed -n -e Xp -e Yp FILENAME
```
или
```bash
sed -n -e 120p -e 145p -e 1050p /var/log/syslog
```
sed : команда, которая по умолчанию выводит все без исключения строки  
 -n : подавление вывода  
 -e CMD : исполняемая команда  
 Xp: вывод строки под номером X  
 Yp: вывод строки под номером Y  
 FILENAME : имя обрабатываемого файла
##### Печатать диапазон строк
```bash
sed -n '46,49 p' file.txt 
sed -n '16224,16482p;16483q' filename
```

####  Добавить симовлы в начало каждой строки: (закомментировать весь файл)
```bash
sed 's/^/#/' file.txt
```
##### Показать количество строк в файле 
```bash
$ sed -n '$=' programming.txt 
10
```
##### Удалить несколько патернов из файла
```bash
sed '/test/{/error\|critical\|warning/d}' somefile
```
#### 
```bash

```
   
## Используемые или важные ключи.
p - Print out the pattern space (to the standard output). This command is usually only used in conjunction with the -n command-line option.

n - If auto-print is not disabled, print the pattern space, then, regardless, replace the pattern space with the next line of input. If there is no more input then sed exits without processing any more commands.

q - Exit sed without processing any more commands or input. Note that the current pattern space is printed if auto-print is not disabled with the -n option.
### Файлы

