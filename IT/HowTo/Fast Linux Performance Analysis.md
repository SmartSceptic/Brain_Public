---
aliases:
  - Linux Performance Analysis in 60000 milliseconds
  - Fast Linux troubleshoot
  - Debug Linux
  - Линукс Дебаг
  - анализ производительности за 60 0000 милисекунд
  - Анализ производительности Линукс за 60 секунд
---
tag: #N/S/Medium   #N/T/Conspect #N/T/Article  #N/T/Public 
2024-03-20 23:39, [Linux Performance Analysis in 60000 milliseconds](https://netflixtechblog.com/linux-performance-analysis-in-60-000-milliseconds-accc10403c55),   
Authors: [[]]   
Related: [[]] 

 https://www.brendangregg.com/linuxperf.html #T/T/To/Do/Note/Refactor 

https://youtu.be/9A3QtGMuqvw #T/T/To/Do/Note/Refactor  добавить ссылки на команды.

Есть большое количество различных специализированных и корпоративных систем сбора статистики, проверок производительности и уведомления о проблемах, вроде [[% zabbix]], [[% grafana]], [[% prometheus]] и многих других, которые при правильной настройке сами могут указать на место проблемы. но иногда даже в крупных корпоративных срдах, не говоря уж про маленькие частные решения, нам нужно войти на сервер и вручную запустить некоторые стандартные инструменты измерения производительности Linux.

==Итак: Возникает серьезная проблема с производительностью, и вы подозреваете, что она вызвана сервером, вы на него заходите: что вы проверяете в первую минуту?==  

Ниже приведены первые 60 секунд оптимизированного исследования производительности [[& LINUX|линукс сервера]]  в командной строке с использованием стандартных инструментов Linux,
за это время вы можете получить общее представление об использовании системных ресурсов и запущенных процессах, выполнив следующие десять команд. 
```bash
uptime  
dmesg | tail  
vmstat 1  
mpstat -P ALL 1  
pidstat 1  
iostat -xz 1  
free -m  
sar -n DEV 1  
sar -n TCP,ETCP 1  
top
```
> [!info]
> Некоторые из этих команд требуют установки пакета sysstat 

[[Метрики]]  предоставляемые этими командами, помогут произвести [[Метод USE|USE анализ]]: в первую очередь ищите [[Метрики ошибок]] и [[Метрики насыщенности]], поскольку их легко интерпретировать, а затем - [[Метрики использования]] ресурсов (ЦП, памяти, дисков и т. д.).

Когда вы базово проверили и "оправдали" ресурс, это сужает дальнейшие потенциальные  дальнейшие цели для изучения.

```bash
# Создание папки с уникальным временным штампом
REPORT_DIR="отчет_$(date '+%Y-%m-%d_%H-%M-%S')"
mkdir "$REPORT_DIR"

# Выполнение команд и запись вывода в файлы внутри созданной папки
uptime | tee "$REPORT_DIR/uptime_report.txt"
dmesg | tail | tee "$REPORT_DIR/dmesg_report.txt"
vmstat 1 | tee "$REPORT_DIR/vmstat_report.txt"
mpstat -P ALL 1 | tee "$REPORT_DIR/mpstat_report.txt"
pidstat 1 | tee "$REPORT_DIR/pidstat_report.txt"
iostat -xz 1 | tee "$REPORT_DIR/iostat_report.txt"
free -m | tee "$REPORT_DIR/free_report.txt"
sar -n DEV 1 | tee "$REPORT_DIR/sar_dev_report.txt"
sar -n TCP,ETCP 1 | tee "$REPORT_DIR/sar_tcp_report.txt"
```

![[% tmux#Паралельный запуск команд для Fast Linux Performance Analysis]]

```bash
 REPORT_DIR="/tmp/report_$(date '+%Y-%m-%d_%H-%M-%S')" && mkdir "$REPORT_DIR" && tmux new-session -d -s my_session && tmux split-window -h && tmux select-pane -t 0 && tmux send-keys "timeout 60 uptime | tee $REPORT_DIR/uptime_report.txt" Enter && tmux split-window -v && tmux send-keys "timeout 60 vmstat 1 | tee $REPORT_DIR/vmstat_report.txt" Enter && tmux split-window -v && tmux send-keys "timeout 60 free -m | tee $REPORT_DIR/free_report.txt" Enter && tmux select-pane -t 1 && tmux send-keys "timeout 60 dmesg | tail | tee $REPORT_DIR/dmesg_report.txt" Enter && tmux split-window -v && tmux send-keys "timeout 60 mpstat -P ALL 1 | tee $REPORT_DIR/mpstat_report.txt" Enter && tmux split-window -v && tmux send-keys "timeout 60 sar -n DEV 1 | tee $REPORT_DIR/sar_dev_report.txt" Enter && tmux attach-session -t my_session

```

[[Сбор базовых данных о линукс хосте]]
[[File-based storage#Смотрим нагрузку на диски]]

[[% last]]  #T/T/To/Do/Note/Refactor 
https://netflixtechblog.com/netflix-at-velocity-2015-linux-performance-tools-51964ddb81cf #T/T/To/Do/Note 
##### Пример быстро смотреть нагрузки от [[@Руслан Точилин|@Ruslan Tochilin]]
```bash
echo "LA: $(uptime | awk '{print $10,$11,$12}') | CPU: $(cat /proc/cpuinfo | grep "model name" | wc -l) | LOAD: $(top -bn1 | grep "Cpu(s)" | sed "s/.*, *\([0-9.]*\)%* id.*/\1/" | awk '{print 100 - $1"%"}') | MEM: $(free -m | awk 'NR==2{printf "%.2fGB/%.2fGB", $3/1024, $2/1024}') | $(iostat -x -d 1 1 | awk '$1 ~ /^(nvme|sd)/ {disks[$1]=$NF} END {for (disk in disks) printf "%s: %.2f%% ", disk, disks[disk]; print ""}')"
```
 Пример вывода
```bash
LA: 4.80, 4.34, 4.36 | CPU: 8 | LOAD: 48.4% | MEM: 36.09GB/62.64GB | nvme0n1: 7.76% nvme1n1: 11.54%
```


## Подробный разбор команд
### [[% uptime]]
![[% uptime#Описание]]
_________________
Три числа дают нам некоторое представление о том, как нагрузка меняется с течением времени. Например, если вас попросили проверить проблемный сервер, а значение 1 минуты намного ниже значения 15 минут, возможно, вы вошли в систему слишком поздно и пропустили проблему.
### [[% dmesg]]
![[% dmesg#dmesg tail]]

### [[% vmstat]]
![[% vmstat#vmstat 1]]

[[% mpstat]]
![[% mpstat#Вывести разбивку времени процессора с разбивкой по ядрам]]


[[% pidstat]]
![[% pidstat#Выводить список процессов занимающих процессор раз в секунду]]

### [[% iostat]]
![[% iostat#Вывод расширенной статистик нагрузки на диски с обновлением раз в секунду, и скрыть устройства для которых небыло активности.]]

### [[% free]]
![[% free#Вывести раскладку использования памяти в MiB (мебибайтах)]]

### [[% sar]]
![[% sar#Вывести статистику пропускной способности интерфейсов раз в секунду]]

![[% sar#Вывести обобщенную информация о некоторых ключевых показателях TCP]]

## Последующий анализ

Есть еще много команд и методологий, которые вы можете применить для более глубокого изучения. См. учебник Брендана [по инструментам производительности Linux](https://medium.com/@Netflix_Techblog/netflix-at-velocity-2015-linux-performance-tools-51964ddb81cf) #T/T/To/Do/Note  от Velocity 2015, в котором задействовано более 40 команд, охватывающих наблюдаемость, тестирование производительности, настройку, статическую настройку производительности, профилирование и трассировку.

________


