---
alias: [ ]
sr-due: 2026-04-12
sr-interval: 697
sr-ease: 270
---
tag: #N/S/Synthesizing #N/T/Tool/App #N/T/Public 
2021-10-20 14:13, [Source](https://habr.com/ru/post/435770/),  [source2](https://code.visualstudio.com/docs/getstarted/tips-and-tricks)
Related: [[]],  
Docs:  

## Описание
  https://code.visualstudio.com/ - кросплатформенный редактор кода с поддержкой функций [[IDE]]

[сравнение visual studio vs visual studio code ](https://alekseev74.ru/lessons/show/visual-studio/code-vs-ide)

###### **Комбинации клавиш и палитра команд**
Самые важные горячие клавиши, в [шпаргалке](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)  
![[keyboard-shortcuts-windows.pdf]]
Или на странице «горячих» клавиш, вызвать которую можно, быстро и последовательно нажав `Ctrl+K` и `Ctrl+S`
-  палитра команд Одна из самых важных горячих клавиш 
`Ctrl + P`    набрав `?`  появистя быстрая подсказка

- **Открыть текущий документ в дзен моде** Distraction Free View из Sublime Text, где всё (кроме кода) удаляется,  
  `Ctrl+K Z`  (Esc Esc to exit)  

- `F1` или `Ctrl + Shift + P` или `Ctrl + P` и `>`, откроется палитра команд с списком  всех доступных действий  

- `Ctrl + T` или `Ctrl + P` и `#`, откроется палитра команд с списком символов (обьектов) в проекте  

- `Ctrl+Shift+O` или `Ctrl + P` и `@`, откроется палитра команд с списком символов (обьектов) в проекте  в текущем файле.  

- Много курсорное редактирование    
`Alt +  click`  
	Можно копировать и вставлять содержимое, выбранное этими курсорами, и они будут вставлены точно в том порядке, в котором они были скопированы.
- Многострочное редактирования  
Windows: `Ctrl + Alt + Arrow Keys`  

- Выделить | редактировать блок  
`Shift+Alt + (drag mouse)`  
  
-  открыть / закрыть встроенный терминал   
	Ctrl+` Show integrated terminal`  
	
-  Переименовать все вхождения   
	Выделяем часть текста или переменную и нажимаем:  
	 `F2` заменить все вхождения в текущем проекте   
	`CTRL + F2` заменить все вхождения, только в текущем файле  

- Открыть недавние папки и файлы 
	`Ctrl+R`

-  Изменить язык подсветки 
	`Ctrl+K M`

- Поправить форматирования кода (Выровнять отступы для стрелочек)
 	`Shift + Alt + F`

### Problems and solvings

![[% git#Git в коде % Visual studio code , говорит, что файл изменен, даже если !изменений нет]]

##### solve Host Key Verification Failed - GitLab with Visual Studio Code on
Resolved by deleting any/all Known_hosts files in ё `C:\Users\vladlen\.ssh` and then executing `ssh git@gitlab.сщь` in Terminal and answering "yes" (which re-adds git@gitlab.com to known_hosts after re-creating a new known_hosts file).



 ## Плагины 
 Открыть окно плагинов `Ctrl+Shift+X`
https://github.com/viatsko/awesome-vscode#bash
 
#### **Gitlens**  
Наиболее полезная функция - легко просматривать кто и когда сделал комит какдой строчки кода.

#### [VS Code Live Share](https://visualstudio.microsoft.com/services/live-share/) - 
дать возможность совместного редактирования файла, вы видите курос друг друга и вносимые изменения.

#### **Rainbow Indent**  
Отступ со стилем. Это расширение окрашивает отступ перед текстом, чередуя четыре разных цвета на каждом шаге.

#### **Polacode**
It makes indentation more readable by colorizing the indentation on each block of code.

#### **shellman**
is a VS Code snippet extension for shell scripts. It provides a free ebook (PDF, epub, mobi) that contains sscripting basics and snippets.
You can find the list of commands on [its Github repo](https://github.com/yousefvand/shellman/blob/master/COMMANDS.md). Shellman provides not only Bash scripting snippets such as `while`, `if`, and `fn` but also `git`, `date`, and other commands as well.

#### Bash Debug
This is [the most popular Bash debugger](https://marketplace.visualstudio.com/items?itemName=rogalmic.bash-debug). It provides `launch.json`, pause support, `watch-debug`, and conditional breakpoints.

#### Bash IDE

## Шпаргалки
