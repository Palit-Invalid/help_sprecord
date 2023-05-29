---
title: Мониторинг устройств SpRecord с помощью Zabbix
description: Данный раздел описывает добавление устройств SpRecord в систему мониторинга Zabbix на основе шаблонов
published: true
date: 2023-04-14T10:14:55.032Z
tags: zabbix, template
editor: markdown
dateCreated: 2023-03-30T11:06:29.010Z
---

# Общая информация
Мониторинг систем SpRecord на основе шаблона (template) позволяет наблюдать за следующими основными параметрами устройств:
- доступность устройства по сети;
- время отклика (RTT);
- модель и серийный номер устройства;
- версия микропрограммы (прошивки);
- количество записей на устройстве;
- дата и время последней записи;
- объем свободного места;
- состояние каналов устройства (подключена линия или нет);
- работа веб-интерфейса.

В шаблоне предусмотрен набор триггеров, при срабатывании которых Zabbix будет фиксировать проблему и отправлять уведомление администратору:
- устройство недоступно по сети, сетевая задержка слишком велика или фиксируются потери пакетов;
- отсутствие отклика веб-интерфейса устройства;
- заканчивается свободное место на накопителе;
- отсутствие новых записей в течение определённого времени;
- отключение линии на канале (статус "Нет линии");
- изменилась версия микропрограммы (прошивки) устройства.
# Получение шаблона для системы мониторинга
Шаблоны мониторинга устройств SpRecord можно скачать по следующим ссылкам:
Шаблон |	Линейка устройств
| :---: | :--- |
| [Ссылка](https://sprecord.ru/files/downloads/mon/zbx_5.4_template_MT-MIC.zip) | SpRecord M/МТ/MIC |
| [Ссылка](https://sprecord.ru/files/downloads/mon/zbx_5.4_template_PBX.zip)	| SpRecord MiniPBX	|	 
| [Ссылка](https://sprecord.ru/files/downloads/mon/zbx_5.4_template_resident.zip)	| SpRecord SIP Resident	|
| [Ссылка](https://sprecord.ru/files/downloads/mon/zbx_5.4_template_micpro.zip)	| SpRecord MicPro |
Перед тем, как импортировать шаблон, необходимо извлечь его из архива.
# Импортирование шаблона в Zabbix
1. В системе Zabbix перейдите в раздел ```Configuration``` - ```Templates``` и нажмите кнопку ```Import``` в верхнем правом углу.
2. В открывшемся окне выберите файл шаблона и ещё раз нажмите ```Import```.
3. Откроется ещё одно окно с подробным описанием содержимого шаблона, там также нажмите ```Import```.
4. Если всё прошло успешно, в списке шаблонов должен появиться шаблон под соответствующим названием.
# Добавление устройства на основе шаблона
Разберём добавление устройства в систему мониторинга на примере SpRecord MT4.
1. Откройте раздел ```Configuration``` - ```Hosts``` и нажмите кнопку ```Create host``` в верхнем правом углу.
2. Задайте имя устройства в поле ```Host name```, выберите хотя бы одну группу в поле ```Groups``` и добавьте интерфейс типа ```Agent```, где укажите IP-адрес устройства.
![add_host_mt.png](/zabbix/add_host_mt.png)
3. Перейдите на вкладку ```Templates```, где в поле ```Link new templates``` добавьте ранее импортированный шаблон ```SpRecord MT```.
![add_host_mt_template.png](/zabbix/add_host_mt_template.png)
4. Нажмите кнопку ```Add``` для завершения процесса.