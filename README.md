# Project

СПИСОК PLAYBOOK:
================
0. test.yml
1. start.yml
2. openvpn.yml
3. bind9.yml
4. postgreslq.yml
5. web.yml
6. certbot.yml
7. www.yml
8. mail.yml
9. pgadmin.yml
10. elastic.yml
11. logs.yml
12. zabbix.yml
13. grafana.yml

0 - test
Не обращайте внимание, просто потестить маленькие роли)

1 - Start
Первоначальная настройка хостов: установка некоторых нужных в дальнейшем пакетов и обновление установленных, настройка шары и скриптов для работы через cron. Присутствовали дополнительные настроки, которые были вылезаны, такие как настрока zsh, т.к. не относятся к проекту. 

2 - OpenVPN
Настраивается сервер и 2 клиента. Предполагается что инфраструктура выдачи ключей имеется и готовы пары ключей для всех серверов, а также ca и dh. Использовался easy-rsa.

3 - Bind9
Устанавливается Bind9 на первый сервер и все сервера настраиваются на резолв через него. В Bind настрона defaul зона, а также зоны local и зона домена.

4 - PostgreSQL
Устанавливается PostgreSQL на сервер базы данных. Изначально по ролям из дирректории old создавались пользователи, базы, заливались схемы и данные для сервисов: WordPress, Zabbix, Rouncube, база Demo для pgAdmin и учетная запись для мониторинга состояния базы. В дальнейшем восстанавливался весь дамп кластера базы, т.к. накопились данные в основном от WordPress и Zabbix. При таком исполнении переносятся все базы, данные, схемы, а также пользователи со всеми правами. Остается только выдать права на сетевое подключение для пользователей.

5 - WEB
Просто ставится nginx, Apache2, PHP и потребующиея в дальнейшем библиотеки. Первоначальная настройка: nginx всегда работает как прокси, Apache2 занимается обработкой HTML и PHP


