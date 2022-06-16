---
title: Модули сценариев
description: 
published: true
date: 2022-06-16T08:42:30.118Z
tags: 
editor: markdown
dateCreated: 2021-04-06T15:35:33.372Z
---

# Описание модулей
Иконка | Название | Описание
--- | --- | ---
![design_enter.png](/minipbx/icons/design_enter.png){.align-left} | Начало выполнения сценария | Старт алгоритма входящих звонков сценария
![paste.png](/minipbx/icons/paste.png){.align-left} | Буфер обмена | Копирование группы элементов для дальнейшей установки в нужном месте
![play_128p.png](/minipbx/icons/play_128p.png){.align-left} | Проиграть аудиоролик | Проигрывание заранее подготовленного сообщения, варианты сообщении: записать самостоятельно или перевести текст в звуковой файл (только при наличии интернета)
![dial.png](/minipbx/icons/dial.png){.align-left} | Вызов абонента или вызов группы абонентов | Добавьте сотрудников для ответа на звонок, варианты: один сотрудник, группа сотрудников, исключительный номер - перевод вызова на внешний номер мобильный или городской
![queue.png](/minipbx/icons/queue.png){.align-left} | Очередь | постановка абонентов в очередь с возможностью проигрывания голосового сообщения при ожидании, донабора номера для выполнения действия
![voice-mail-icon.png](/minipbx/icons/voice-mail-icon.png){.align-left} | Голосовая почта | Автоответчик, позволяет сохранять сообщения для дальнейшего прослушивания
![ivr_256p.png](/minipbx/icons/ivr_256p.png =x75){.align-left} | IVR меню | Голосовое приветствие с возможностью донабора короткого номера для выполнения сценария, позволяет набрать прямой внутреннии номер и перевести вызов сразу на нужного абонента
![ivr_back.png](/minipbx/icons/ivr_back.png =x75){.align-left} | Возврат в IVR меню | Позволяет вернуть абонента в начало IVR меню
![label.png](/minipbx/icons/label.png =x60){.align-left} | Метка | Точка в сценарии, на которую можно в любой момент вернуться
![goto_label.png](/minipbx/icons/goto_label.png){.align-left} | Переход на метку | Переход на созданную метку
![script.png](/minipbx/icons/script.png){.align-left} | Выполнить сценарий | Позволяет передать управление другому, ранее созданному сценарию
![clock.png](/minipbx/icons/clock.png){.align-left} | Выбор времени | Выбор времени для выполнения сценария
![calendar.png](/minipbx/icons/calendar.png){.align-left} | Выбор дней недели | Выбор дней недели для выполнения сценария
![phonebook.png](/minipbx/icons/phonebook.png){.align-left} | Выбор списка абонентов | Адресная книга для сотрудников, при внесении контактов в адресную книгу входящий звонок с указанного номера переадресуется по указанному сценарию
![trunk_b.png](/minipbx/icons/trunk_b.png){.align-left} | Фильтр внешних линий | Позволяет разделить линии на несколько групп для выполнения сценария для каждой линии отдельно
![filter_num.png](/minipbx/icons/filter_num.png){.align-left} | Фильтр номера звонящего абонента | Позволяет разделить звонки от разных абонентов
![filter_num.png](/minipbx/icons/filter_num.png){.align-left} | Фильтр номера вызываемого абонента | Позволяет разделить звонки к разным абонентам
![email.png](/minipbx/icons/email.png){.align-left} | [Отправить E-mail](./modules/send_email) | Отправить уведомление по E-mail
![sms.png](/minipbx/icons/sms.png){.align-left} | [Отправить SMS](./modules/send_sms) | Отправка уведомление о звонке через модем
![telegram.png](/minipbx/icons/telegram.png){.align-left} | [Отправить уведомление в Telegram](./modules/send_telegram) | Отправка оповещения о звонке через Telegram
![pause.png](/minipbx/icons/pause.png){.align-left} | Пауза | Приостановить выполнение сценария на выбранное время
![fill_result.png](/minipbx/icons/fill_result.png){.align-left} | Отметка в базе данных | Оставить сообщение какому-либо пользователю
![number_change.png](/minipbx/icons/number_change.png =x60){.align-left} | Модификация номера звонящего абонента | Позволяет изменить номер звонящего
![hangup_32p.png](/minipbx/icons/hangup_32p.png =x60){.align-left} | Завершить соединение | Положить трубку
![process_128p.png](/minipbx/icons/process_128p.png =x60){.align-left} | Выполнить | Позволяет запустить рассылку СМС или автообзвон
![input_number_36p.png](/minipbx/icons/input_number_36p.png =x60){.align-left} | Ввод данных | Запросить ввод данных от пользователя
![play_x_32p.png](/minipbx/icons/play_x_32p.png =x60){.align-left} | Озвучить значение переменной | Озвучить ранее введённые данные
![fill_table.png](/minipbx/icons/fill_table.png =x60){.align-left} | Занести значения переменных в таблицу | Занесение значений одной или нескольких переменных в таблицу
![http_64p.png](/minipbx/icons/http_64p.png =x60){.align-left} | http-запрос | Выполнить HTTP-запрос
![var_action_64p.png](/minipbx/icons/var_action_64p.png =x60){.align-left} | Действие над переменной | Выполнить какое-либо действие над переменной. Доступные действия: присвоить, прибавить, вычесть, объединить строки
![var_cmp_64p.png](/minipbx/icons/var_cmp_64p.png =x60){.align-left} | Сравнение переменной | Сравнить переменную и в зависимости от результата передать управлению в ту или иную ветку сценария
![bitrix.png](/minipbx/icons/bitrix.png =x60){.align-left} | Битрикс24 | Отправить данные в Битрикс24
