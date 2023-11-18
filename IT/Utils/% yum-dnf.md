---
alias: [% yum-utils ]
sr-due: 2023-04-09
sr-interval: 63
sr-ease: 290
---
tag: #N/S/EverGreen #N/T/Linux #N/T/Tool/Util #N/T/Public  
2022-05-22 18:06, [Source](https://habr.com/ru/articles/301292/),  
Related: [[% rpm]],  [[% apt]]
Docs:  

#T/T/To/Do/Note/Refactor 
https://www.tecmint.com/configure-software-repositories-in-fedora/

## Описание
Шпаргалка по работе с пакетным менеджером Yum (Yellowdog Updater, Modified), который используется в популярных Linux дистрибутивах: RedHat, CentOS, Scientific Linux (и других). В целях экономии места вывод команд не представлен.

```bash
yum-utils
```

## Оглавление
[[% yum-dnf#Команды yum]]
[[% yum-dnf#Используемые или важные ключи.]]
[[% yum-dnf#Cледующие команды доступны после установки пакета yum-utils]]
[[% yum-dnf#Конфигурационные файлы Yum и их расположение]]
[[% yum-dnf#Некоторые полезные плагины]]
[[% yum-dnf#Работа Yum через прокси сервер]]

#### Команды yum
##### Отображение команд и опций  
```
yum help
```
##### список названий пакетов из репозиторий  
```
yum list
```
###### список всех доступных пакетов  
```
yum list available
```
###### список всех установленных пакетов  
```
yum list installed
```
###### установлен ли указанный пакет  
```
yum list installed httpd
```
###### список установленных и доступных пакетов  
```
yum list all
```
###### список пакетов, относящихся к ядру  

```
#yum list kernel
```
##### отображение информации о пакете  
```
yum info httpd
```
##### список зависимостей и необходимых пакетов  
```
yum deplist httpd
```
##### найти пакет, который содержит файл  
```
yum provides "*bin/top"
```
##### поиск пакета по имени и описанию  
```
yum search httpd
```
##### получить информацию о доступных обновлениях безопасности  
```
yum updateinfo list security
```
##### вывести список групп  
```
yum grouplist
```
###### вывести описание и содержимое группы  
```
yum groupinfo "Basic Web Server"
```
###### установка группы пакетов «Basic Web Server»  
```
#yum groupinstall "Basic Web Server"
```
###### удаление группы  
```
#yum groupremove "Basic Web Server"
```
##### Проверка на доступные обновления  
```
yum check-update
```
##### список подключенных репозиториев  
```
yum repolist
```
##### информация об определенном репозитории  
```
yum repoinfo epel
```
##### информация о пакетах в указанном репозитории  
```
yum repo-pkgs epel list
```
##### установить все пакеты из репозитория  
```
yum repo-pkgs reponame install
```
##### удалить пакеты установленные из репозитория  
```
yum repo-pkgs reponame remove
```
##### создать кэш  
```
yum makecache
```
##### проверить локальную базу rpm (поддерживаются параметры dependencies, duplicates, obsoletes, provides)  
```
yum check
yum check dependencies
```
##### просмотр yum истории (вывод списка транзакций)  
```
yum history list
```
###### просмотр информации определенной транзакции (установленные пакеты, установленные зависимости)  
```
yum history info 9
```
###### отмена транзакции  
```
#yum history undo 9
```
###### повторить  
```
#yum history redo 9
```
##### дополнительно можно просмотреть лог  
```
cat /var/log/yum.log
```
##### удалить пакеты сохраненные в кэше  
```
yum clean packages
```
##### Удалить кеш всех пакетов и метаданные  
```
yum clean all
```
##### установить пакет  
```
yum install httpd
```
##### удаление пакета  
```
yum remove httpd
```
##### обновить пакет  
```
yum update httpd
```
##### обновить все пакеты  
```
yum update
```
##### обновить до определенной версии  
```
yum update-to
```
##### установить из локальной директории (поиск/установка зависимостей будут произведены из подключенных репозиториев)  
```
yum localinstall httpd.rpm
```
или  
```
#yum install httpd.rpm
```
##### установить с http  
```
yum localinstall http://server/repo/httpd.rpm
```
##### откатиться к предыдущей версии пакета  
```
yum downgrade
```
##### переустановка пакета (восстановление удаленных файлов)  
```
yum reinstall httpd
```
##### удаление ненужных более пакетов  
```
yum autoremove
```
##### создание локальных репозиториев (createrepo ставится отдельно)  
```
createrepo
```
##### установка обновлений по расписанию (yum-cron устанавливается отдельно)  
```
yum-cron
```


#### Cледующие команды доступны после установки пакета yum-utils
##### найти из какого репозитория установлен пакет  
```
find-repos-of-install httpd
```
##### найти процессы, пакеты которых обновлены и требуют рестарта  
```
needs-restarting
```
##### запрос к репозиторию, узнать зависимости пакета, не устанавливая его 
```
repoquery  --requires --resolve httpd
```
##### синхронизировать yum репозиторий updates в локальную директорию repo1  
```
reposync -p repo1 --repoid=updates
```
##### проверить локальный репозиторий на целостность  
```
verifytree URL
```
##### завершить транзакции  
```
yum-complete-transaction
```
##### установить необходимые зависимости для сборки RPM пакета  
```
yum-builddep
```
##### управление конфигурационными опциями и репозиториями yum  
```
yum-config-manager
```
##### запрос к локальной базе yum, отображение информации о пакете  
(использованная команда, контрольная сумма, URL с которого был установлен и другое)  

```
yumdb info httpd
```
##### скачать rpm пакеты из репозитория  
```
yumdownloader
``` 
##### скачать src.rpm пакет из репозитория  
(должен быть подключен соответствующий репозиторий, например в '/etc/yum.repos.d/CentOS-Sources.repo' в CentOS)  
```
yumdownloader --source php
```






## Шаблон 
```bash

```
## Примеры: 
##### Узнать какой репозиторий предоставляет пакет:
```
yum whatprovides '*bin/grep'
repoquery -i {packagename}
rpm -qi {packagename}
```
This will give you the actual repo name vs the unhelpful "installed" that yum returns. repoquery is provided by yum-utils.
##### PHP 7.4 on CentOS 8
[https://www.tecmint.com/install-php-on-centos-8/](https://www.tecmint.com/install-php-on-centos-8/
```bash
 yum install [http://rpms.remirepo.net/enterprise/remi-release-7.rpm](http://rpms.remirepo.net/enterprise/remi-release-7.rpm)  
 yum-config-manager --enable remi-php73  
 yum-config-manager --disable remi-php71  
 yum repolist enabled |grep php  
 * remi-php73: mirror.23media.com

yum install php-pecl-mongo
yum repolist 
yum install php-pecl-mongo
yum install --enablerepo=remi* php-pecl-mongo
yum install --enablerepo=remi-php72 php-pecl-mongo
yum install --disablerepo=epel --enablerepo=remi-php72 php-pecl-mongo
yum whatprovides /*php-pecl-mongo
yum search php-pecl-mongo
yum install php72-php-pecl-mongodb.x86_64
```
### Шпаргалки:
##### Проверить наличие обновлений или ошибок в настройке yum
```bash
/bin/yum check-update
```
##### Отключить репозиторий
#T/T/To/Do/Note/Refactor   https://www.if-not-true-then-false.com/2010/yum-remove-repo-repository-yum-disable-repo-repository/
```bash
yum repolist
yum-config-manager --disable <repo_id>
```
######  
```bash
yum list available --showduplicates libcurl
```




#### Используемые или важные ключи.
##### ответить «yes» при запросе,  
```
-y
#yum update -y
```
##### ответить «no» при запросе  
```
--assumeno
```
##### использовать Yum без плагинов  
```
--noplugins
```
##### или отключить определенный плагин  
```
--disableplugin=fastestmirror
```
##### включить плагины, которые установлены, но отключены  
```
yum --enableplugin=ps
```
##### включить отключенный репозиторий  
```
yum update -y --enablerepo=epel
```
##### отключить репозиторий  
```
yum update -y --disablerepo=epel
```
##### скачать пакеты, но не устанавливать  
(на Centos 7 x86_64 будут скачаны в '/var/cache/yum/x86_64/7/base/packages/')  
```
yum install httpd --downloadonly
```

#### Конфигурационные файлы Yum и их расположение
##### Основной конфигурационный файл  
```
/etc/yum.conf
```
##### директория, с конфигурациями (например, yum плагины)  
```
/etc/yum/
```
##### директория, содержащая информацию о репозиториях  
```
/etc/yum.repos.d/
```

##### Некоторые опции yum.conf:
###### Директория, где yum хранит кэш и файлы базы (по умолчанию '/var/cache/yum')  
```
cachedir=/var/cache/yum/$basearch/$releasever
```
###### Определяет должен или нет Yum хранить кэш заголовков и пакетов после успешной установки. Значения: 0 или 1. (по умолчанию 1)  
```
keepcache=1
```
###### уровень вывода отладочных сообщений. Значения: 1-10 (по умолчанию 2)  
```
debuglevel=2
```
###### лог файл (по умолчанию '/var/log/yum.log')  
```
logfile=/var/log/yum.log
```
###### обновлять устаревшие пакеты  
```
obsoletes=1
```
###### проверка подписи пакетов. Значения: 0 или 1 (по умолчанию 1)  
```
gpgcheck=1
```
###### включение плагинов. Значения: 0 или 1 (по умолчанию 1)  
```
plugins=1
```


#### Некоторые полезные плагины
##### Добавляет опцию командной строки для просмотра ченжлога перед/после обновлениями  
```
yum-plugin-changelog
```
##### выбирает более быстрые репозитории из списка зеркал  
```
yum-plugin-fastestmirror
```
##### добавляет команды keys, keys-info, keys-data, keys-remove, которые позволяют работать с ключами.  
```
yum-plugin-keys
```
##### блокировать указанные пакеты от обновления, команда yum versionlock  
```
yum-plugin-versionlock
```
##### добавление команд yum verify-all, verify-multilib, verify-rpm для проверки контрольных сумм пакетов  
```
yum-plugin-verify
```

#### Работа Yum через прокси сервер
##### Для всех пользователей:  
добавить в секцию [main] в /etc/yum.conf  
```
proxy="http://server:3128"
```
##### при необходимости указать пароль, добавить  
```
proxy_proxy_username=user
proxy_password=pass
```
##### указать прокси для отдельного пользователя  
```
export http_proxy="http://server:3128"
```