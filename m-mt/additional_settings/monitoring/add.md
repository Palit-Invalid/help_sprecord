---
title: Добавление устройства в систему мониторинга
description: Как добавить устройство SpRecord в систему Zabbix
published: true
date: 2022-06-16T14:30:56.519Z
tags: sprecord zabbix мониторинг
editor: markdown
dateCreated: 2021-12-29T05:28:36.428Z
---

# Добавление устройства
1. Откройте раздел ```Configuration``` - ```Hosts``` в Zabbix.
2. Нажмите кнопку ```Create host``` в правом верхнем углу.
3. Заполните поле ```Host name```, указав уникальное (в системе Zabbix) имя устройства.
4. В поле ```Groups``` выберите группу ```Linux servers```.
5. В поле ```Interfaces``` нажмите ```Add``` и добавьте интерфейс типа ```Agent```:

![add_interface.png](/zabbix/add_interface.png)

6. Укажите IP-адрес устройства в соответствующем поле.
7. При необходимости заполните описание устройства в поле ```Description```.
8. Нажмите кнопку ```Add``` в нижней части окна.

![create_host.png](/zabbix/create_host.png)