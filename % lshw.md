---
aliases: 
sr-due: 2025-04-24
sr-interval: 298
sr-ease: 284
---
tag: #N/S/Stub #N/T/Linux #N/T/Tool/Util #N/T/Public 
2023-10-16 15:59, [Source](),  
Related: [[% lsblk]]
Docs:  

## Описание
Инструмент Linux, который используется для _создания подробной информации о конфигурации оборудования системы_.

## Шаблон 
```bash

```
## Примеры: 
##### 
```bash

```
### Шпаргалки:
#####  disk model (temp check)
отобразить все диски и контроллеры хранилищ в системе
```bash
lshw -class disk -class storage
```
##### Вывод информацию о дисках включая [[RAID]]
```bash
lshw -class disk
lshw -short -C disk
# или в json формате
lshw -class disk -json
```
От имени пользователя root, чтобы отображать только сведения о жестких дисках (включая информацию о RAID).
## Используемые или важные ключи.
### Файлы  