---
title: Проверка доступности устройства SpRecord по сети
description: Добавление проверки ICMP Ping в Zabbix
published: true
date: 2021-12-29T05:54:43.958Z
tags: sprecord zabbix мониторинг
editor: markdown
dateCreated: 2021-12-29T05:54:40.691Z
---

# Добавление проверки доступности устройства по сети
1. В разделе ```Configuration``` - ```Hosts``` найдите ваше устройство и щёлкните по его имени, чтобы открыть страницу конфигурирования.
2. Перейдите на вкладку ```Templates```.
3. В поле ```Link new templates``` нажмите ```Select``` и выбрите шаблон ```ICMP Ping```.
4. Нажмите ```Update``` для внесения изменений в конфигурацию хоста.

![add_template_ping.png](/zabbix/add_template_ping.png)

5. Для проверки откройте раздел ```Monitoring``` - ```Hosts```, найдите ваше устройство, щёлкните по его имени и выберите в выпадающем меню ```Latest data```:

![latest_data.png](/zabbix/latest_data.png)

6. В списке источников данных найдите источник с именем ```ICMP response time``` и нажмите кнопку ```Graph``` справа от него. Должен отобразиться график времени отклика устройства.

![icmp_response_graph.png](/zabbix/icmp_response_graph.png)