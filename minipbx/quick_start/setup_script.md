---
title: Настройка сценария
description: 
published: true
date: 2021-04-06T14:55:01.017Z
tags: 
editor: markdown
dateCreated: 2021-04-06T14:54:57.263Z
---

> На данной странице описан алгоритм настройки входящей маршрутизации в режиме сценариев. По умолчанию он отключен. Включить его можно в **Система** - **Управление** - **Режим интерфейса**.
{.is-info}

# <a id="base_setup_script"></a>**Базовая настройка сценария**
1. Откройте вкладку меню ```Сценарии```.
2. Нажмите ![plus.png](/minipbx/icons/plus.png) в правом верхнем углу.
3. Введите ```Название``` сценария - любой номер или имя (только латиница).
4. Введите ```Номер``` сценария - любой номер, данный номер дополнительно можно использовать для тестирования маршрута АТС после настройки.
5. Нажмите значок ```Выбор внешних линии``` ![](/minipbx/icons/trunk_b.png =25x25) - значок добавится в основном поле настройки.
6. Нажмите надпись ```Не выбрано```.
7. Выберите линии, которые будут использоваться в данном сценарии, нажмите ```ОК```, список выбранных линии будет указан в поле значка .
8. Нажмите ![plus.png](/minipbx/icons/plus.png) под списком выбранных линии.
9. Нажмите значок ```Вызов абонента или группы``` ![dial](/minipbx/icons/dial.png =25x25).
10. Выберите абонентов, которые будут отвечать на звонки по данной линии.
11. Нажмите ```ОК``` - модуль появится в поле сценария.
12. Нажмите ```ОК``` - сценарий появится в списке доступных.

![example_script.jpg](/minipbx/screenshots/example_script.jpg)

## **Применение сценария для входящих вызовов**
1. Откройте меню ```Сценарии```.
2. Найдите строчку ```from_inbound - вызов с внешней линии``` и нажмите два раза.
3. Нажмите на модуль ```IVR меню``` и выберите строку ```Удалить все```.
4. Нажмите ![plus](/minipbx/icons/plus.png) под значком начало выполнения сценария.
5. Нажмите значок ```Выполнить сценарии``` ![script_exec](/minipbx/icons/script.png =25x25).
6. Выберите из выпадающего списка ```Сценарий```, который хотите применить.
7. Нажмите ```ОК```, имя сценария будет указано в поле значка .
8. Нажмите ```ОК``` - сценарий применен.

> Теперь можно принимать входящие звонки и осуществлять исходящие вызовы. Для просмотра настройки всех модулей сценария перейдите в раздел описание сценария {.is-success}

# <a id="advanced_setup_script"></a>Полная настройка сценария
## **Базовые сценарии:**
1. **from_local - вызов с локального номера**
   ![from_local.jpg](/minipbx/screenshots/from_local.jpg)
   действие при вызове с внутреннего номера. Единый сценарии для всех пользователей, настройка по умолчанию:
   - вызов набранного номера;
   - разговор - абонент ответил на вызов;
   - занято - при занятости сотрудника выполнить: действие - произносится фраза "Номер занят", действие - "Положить трубку";
   - таймаут - при неответе сотрудника указанное время выполнить: действие - произносится фраза "Номер не отвечает", действие - "Положить трубку";
   - недоступен - телефон отключен или недоступен выполнить: действие - произносится фраза "Номер не доступен", действие - "Положить трубку".
   
>    Настройте сценарии по Вашему желанию. При удалении сценария из списка возвращаются настройки по умолчанию.
{.is-info}


2. **from_inbound - вызов с внешней линии**
   ![from_inbound.jpg](/minipbx/screenshots/from_inbound.jpg)
   действие при вызове по внешним линиям, настройка по умолчанию:
   - IVR меню - выполнить: действие - произносится фраза "Наберите номер";
   - любая другая клавиша - введен неправильный или запрещенный номер выполнить: действие - произносится фраза "Номер не существует", действие - "Положить трубку";
   - таймаут - в указанное время не производился набор номера выполнить: действие - произносится фраза "Время ожидания истекло", действие - "Положить трубку";
   - донабор номера - при наборе внутреннего номера вызов абонента.
   

> Настройте сценарии по Вашему желанию. Рекомендуем настроить отдельные сценарии, что позволит быстро заменить маршрутизацию входящих вызовов. При удалении сценария из списка автоматический переходит в режим настройки по умолчанию.{.is-info}

3. **from_extra - вызов с исключительного номера**
   ![from_extra.jpg](/minipbx/screenshots/from_extra.jpg)
   действие при звонке с АТС с исключительного номера, например для звонка с линии АТС, настройка по умолчанию:
   - IVR меню - выполнить: действие - произносится фраза "Введите номер назначения", абонент может ввести любой номер разрешенный маршрутизацией;
   - любая другая клавиша - введен неправильный или запрещенный номер выполнить: действие - произносится фраза "Неправильно набран номер", действие - "Возврат в IVR меню";
   - таймаут - в указанное время не производился набор номера выполнить: действие - произносится фраза "Время ожидания истекло", действие - положить трубку;
   - донабор номера - введен правильный внешний номер или внутреннего абонента АТС, действия:
      - занято - при занятости сотрудника выполнить: действие - произносится фраза "Номер занят", действие - "Возврат в IVR меню";
      - таймаут - при неответе сотрудника указанное время выполнить: действие - произносится фраза "Номер не отвечает", действие - "Возврат в IVR меню";
      - недоступен - телефон отключен или недоступен выполнить: действие - произносится фраза "Номер не доступен", действие - "Возврат в IVR меню";
      - разговор - абонент ответил на вызов.

> Настройте сценарии по Вашему желанию. При удалении сценария из списка автоматический переходит в режим настройки по умолчанию.{.is-info}

[Следующая страница >>](./address_book)

[<< Предыдущая страница](./setup_out_route)