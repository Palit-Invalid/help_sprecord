---
title: Определение IP-адреса
description: 
published: true
date: 2022-06-16T14:08:00.734Z
tags: 
editor: markdown
dateCreated: 2022-06-16T14:08:00.734Z
---

Устройство рассчитано на автоматическое получение IP-адреса по протоколу DHCP. Для первоначального подключения необходимо наличие DHCP-сервера (обычно он встроен в роутер), который выдаст устройству IP-адрес.

Для того чтобы войти в устройство для настройки параметров, необходимо знать его IP-адрес. Адрес можно посмотреть:
1) в журнале DHCP-сервера
2) попробовать найти его при помощи сетевого IP-сканера.

## Просмотр журнала DHCP-сервера
Зайдите в панель администрирования вашего роутера и найдите в нем список IP-адресов, которые назначены DHCP-сервером.

Отличительная особенность устройства – в поле HOSTNAME содержится слово «SPRECORD». Например, так выглядит устройство в списке выданных адресов DHCP-сервера:
![ip_on_router.png](/m-mt/ip_on_router.png)
Видно, что устройство получило IP-адрес 192.168.хх.172.

## Сканер IP-адресов
Если нет возможности посмотреть выданные сервером адреса, можно просканировать сеть при помощи специальной утилиты – IP-сканера. Например, можно воспользоваться программой «Angry IP Scanner». Скачать программу можно в разделе «Скачать» сайта sprecord.ru. Для работы программы требуется установленная «Java» (скачать можно здесь http://java.com/download).

Запустите программу сканера и нажмите кнопку «Старт».
![ip_scanner.png](/m-mt/ip_scanner.png)

Программа отобразит список адресов, среди которых нужно найти интересующий нас адрес устройства. Найдите IP-адрес в строке, в которой в столбце Hostname содержится слово «SPRECORD».
![ip_scanner2.png](/m-mt/ip_scanner2.png)

Если по каким-то причинам сканер не показывает ни одного адреса со словом «SPRECORD», то попробуйте уточнить параметры сканирования. Зайдите в настройки (меню Инструменты – Предпочтения), на вкладке «Порты» укажите порт 5900, а на вкладке «Отобразить» установите переключатель в положение «Только активные хосты»:

![setup_scan.png](/m-mt/setup_scan.png)

![setup_scan2.png](/m-mt/setup_scan2.png)

Сохранив настройки, нажмите «Старт» для сканирования. Программа отобразит список адресов, среди которых нужно найти интересующий нас адрес устройства . Отличить его можно по значению «5900» в графе «Ports».

![ip_scanner3.png](/m-mt/ip_scanner3.png)

