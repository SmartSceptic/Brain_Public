---
alias: [ ]
sr-due: 2026-09-27
sr-interval: 819
sr-ease: 289
---
tag: #N/S/Stub #N/T/Tool #N/T/Linux #N/T/Tool/Util #N/T/Public 
2021-08-20 01:12, [Source](),

Related: [[]],

Docs: 

## Описание
Этот скрипт считывает вывод iptables и генерирует красивую блок-схему. Работает со всеми таблицами и цепочками.

## Шаблон 
Клонируйте репозиторий, убедитесь, что awk установлен, установите blockdiag
`iptables -v -L > iptables.txt`
`awk -f iptables-vis.awk < iptables.txt > iptables.dia`
`blockdiag iptables.dia -T svg -o iptables.svg`


## Примеры: 
###### 
```bash

```
### Шпаргалки:
###### Отобразить только выбранные цепочки (поддерживает регулярное выражение):
```bash
awk -f iptables-vis.awk -v 'chain_selector=INPUT|OUTPUT|mychain' < iptables.txt > iptables.dia
```

##### Отобразить пустые цепочки:
```bash
awk -f iptables-vis.awk -v 'include_empty_chains=1 < iptables.txt > iptables.dia
```
## Используемые или важные ключи.
### Файлы