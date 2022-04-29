# PROJECT

## Список playbook:

1. Start
2. OpenVPN
3. Bind9
4. PostgreSQL
5. WEB
6. Certbot
7. WWW
8. Mail
9. pgAmin
10. Elastic
11. Logs
12. Zabbix
13. Grafana

## Start

*Все сервера*
<br/> Первоначальная настройка всех хостов:
+ Установка некоторых нужных в дальнейшем пакетов и обновление установленных.
+ Настройка шары - в дальнейшем будет использоваться для копирования данных. Задачи в cron для взаимной синхронизации.
+ Создание шаблонов скриптов для работы через cron - для поднятия сервисов с задержкой после перезагрузки в правильном порядке.

> Присутствовали дополнительные настроки, которые были вырезаны, такие как настрока zsh, т.к. не относятся к проекту. 

## OpenVPN

*Сервер 1*
<br/> Настраивается и поднимается OpenVPN сервер.

*Сервер 2 и 3*
<br/> Настраивается 2 клиента, подключаются к серверу с автозагрузкой.

> Предполагается что инфраструктура выдачи ключей имеется и готовы пары ключей для всех серверов, а также ca и dh. Использовалась easy-rsa.

## Bind9

*Сервер 2*
<br/> Устанавливается Bind9 на первый сервер, настраиваются зоны:
+ Defaul зона.
+ Зоны local для сети OpenVPN - для взаимодействия всех сервисов. Настроены cname db, web, zabbix и т.д. для упрощенного взаимодействия.
+ Публичная зона домена.

*Все сервера*
<br/> Все сервера настраиваются на резолв через наш DNS.

## PostgreSQL

*Сервер 3*
<br/> Устанавливается PostgreSQL. Обеспечивает работу:
+ WordPress.
+ Rouncube.
+ Zabbix - база для сервиса, а также средства мониторинга PostgreSQL.
+ pgAdmin - для базы [Demo](https://postgrespro.com/education/demodb) и ее копий.

> Изначально по ролям из дирректории old создавались пользователи, базы, заливались схемы и данные.
>> В дальнейшем восстанавливался весь дамп кластера базы, т.к. накопились данные в основном от WordPress и Zabbix.
>>> При таком исполнении переносятся все базы, данные, схемы, а также пользователи со всеми правами. Остается только выдать права на сетевое подключение для пользователей.

## WEB

*Сервер 2*
<br/> Установка nginx, Apache2, PHP и потребующиея в дальнейшем библиотеки. Первоначальная настройка:
+ nginx - проксирует запросы на Apache2.
+ Apache2 - показывает стандвртный индекс nginx.

> Веб сервер подготовлен для получения SSL сертификатов.

## Certbot

*Сервер 2*
<br/> Установка Certbot и получение сертификатов через webroot.

<br/> Настройка nginx, один конфиг три Server:
+ Домен второго уровня проксирует запросы на Apache2.
> Дополнительно проксируются запросы к сервисам которые появятся позже: Grafana и Kibana. 
+ Домен третьего уровя forcustomer проксирует запросы на Apache2. 
+ Все запросы перенаправляются на https.

## WWW

*Сервер 2.*
<br/> Использется шара - копируется код для обработки Apache2:
+ Wordress - основа домена [второго уровня](https://admin11.tk/). За основу взята актуальная на момент создания версия 5.9.3.
> Для возможности работы на PostgreSQL использовался проект [pg4wp](https://github.com/kevinoid/postgresql-for-wordpress).
>> Также установлена тема Basic от [WP Puzzle](https://admin11.tk/wp-admin/themes.php?theme=basic) со слегка измененными стилями CSS и плагин [Contact Form 7](https://contactform7.com/).
+ Страница картинка для домена [третьего уровня](https://forcustomer.admin11.tk/).
> C форма обратной связи, за основу взят [этот](https://github.com/itchief/feedback-form) проект с GitHub, также внесены небольшие изменения в PHP код и CSS стили, форма упакована в страницу картинку.
+ Настроенный Roundcube версии 1.5.2 для [почтового сервера](https://admin11.tk/app/mail/).

<br/> Настройка Apache2. Один конфиг, два VirtualHost:
+ Домен вервого уровня имеет root в дирректорию Wordpress, а также алиасы в директории Zabbix, mail и pgAdmin4
+ Домен третьего уровня имеет root в корень дирректории forcustomer.

## Mail

*Сервер 2.*
<br/> !!! Первоначально на сервере установлен Postfix !!!
+ Настройка Postgix.
+ Установка и настройка Dovecot.
> Используется SSL сертификат домена второго уровня.

## pgAdmin4

*Сервер 2.*
<br/> Установка pgAdmin4.
> !!! В дальнейшем требуется с сервера запустить скрипт от разработчика для настройки !!!

## Elastic

*Сервер 3.*
<br/> Добавлен репозиторий Elastic, точнее зеркало Yandex.
<br/> Установка Elasticsearch:
+ Elasticsearch настроен на работу по сети VPN.
+ Настроен доступ по паролю и созданы пользователи со стандартными ролями

<br/> Установлен Kibana:
+ Kibana работает по сети VPN, nginx уже проксирует запросы к Kibana на *Сервер 3*.
+ Настроен парольный доступ к кластеру.

 




