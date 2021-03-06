---
title: Интеграция с Битрикс24
description: Настройка связки SpRecord MiniPBX и CRM Битрикс24
published: true
date: 2022-06-16T09:40:24.322Z
tags: minipbx, bitrix, битрикс, интеграция, crm
editor: markdown
dateCreated: 2022-05-19T10:15:37.848Z
---

# Общая информация
IP-АТС SpRecord MiniPBX имеет возможность интеграции с CRM Битрикс24. В результате интеграции появляются следующие преимущества:
- пользователям CRM Битрикс24 отображается карточка звонка при входящем звонке на их телефон;
- звонки автоматически регистрируются в CRM Битрикс24 (создаётся лид);
- в CRM Битрикс24 загружается запись разговора;
- появляется возможность совершать исходящие звонки из CRM Битрикс24;
- появляется возможность автоматически направлять звонок на ответственного менеджера.

Для осуществления настройки необходимо установить приложение "SpRecord MiniPBX" из Маркета Битрикс24 и при необходимости внести изменения в голосовое меню IP-АТС.

# Установка приложения в CRM Битрикс24
1. В CRM Битрикс24 откройте раздел "Маркет", найдите приложение "SpRecord MiniPBX" и установите его.
2. В открывшемся приложении необходимо ввести ```Код активации MiniPBX``` и ```API Key```. Код активации можно найти в разделе ```Информация``` веб-интерфейса IP-АТС, а в качестве ```API Key``` введите тестовое значение ```123456```.
3. Для облегчения связи с сотрудниками техподдержки заполните поля "Электронная почта" и "Номер телефона", после чего нажмите кнопку ```Сохранить```.

Вид приложения в CRM Битрикс24:
![bitrix_app.jpg](/minipbx/bitrix_app.jpg)

Нахождение кода активации в веб-интерфейсе АТС:
![pbx_activation_code.jpg](/minipbx/pbx_activation_code.jpg)

> После установки приложения и внесения указанной информации звонки начнут поступать в CRM Битрикс24. {.is-success}

> Внимание! Для корректной работы интеграции необходимо заполнить поле "Внутренний телефон" в профилях пользователей CRM Битрикс24. {.is-warning}

# Перенаправление звонка на ответственного менеджера
В CRM Битрикс24 для каждого контакта указывается ответственный менеджер, который обычно закрепляется за данным контактом и ведёт с ним общение. Чтобы упростить маршрутизацию звонков и сократить время соединения для клиентов, есть возможность автоматизировать перенаправление входящего звонка сразу на ответственного менеджера. Работает это так:
1. При поступлении входящего звонка от клиента АТС обращается в CRM Битрикс24 и ищет там контакт (по номеру звонящего).
2. Если контакт найден (т.е. он уже звонил ранее), извлекается номер телефона ответственного менеджера и звонок переводится на него.
3. Если контакт в CRM Битрикс24 не найден (т.е. это первое обращение), то обработка звонка производится без изменений - обычно это включение голосового меню и далее перевод на группу менеджеров.

Для включения подобной обработки необходимо добавить в сценарий обработки входящего звонка на АТС специальную команду - ```Bitrix24```. Команду имеет смысл добавлять в самом начале сценария, когда звонок только поступил, но можно и в другом месте, руководствуясь своей логикой обработки звонков. Команда передаст номер в CRM Битрикс24, а затем обработка продолжится по одной из двух веток: "Вызов ответственного" (левая ветка, если контакт найден и определён номер ответственного менеджера) и "Ответственный не определён" (правая ветка в остальных случаях). На изображении ниже показан возможный сценарий обработки входящего звонка с использованием данной команды.

![ivr_bitrix.jpg](/minipbx/ivr_bitrix.jpg)