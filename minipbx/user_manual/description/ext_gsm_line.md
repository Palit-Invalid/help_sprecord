---
title: Внешние линии GSM
description: 
published: true
date: 2021-04-06T15:40:52.823Z
tags: 
editor: markdown
dateCreated: 2021-04-06T15:40:49.188Z
---

## Основное

*Использовать линию*
- Позволяет отключить внешнюю линию, не удаляя настройки.
> Если галочка рядом с «Использовать линию» не установлена, эта линия не будет использована в маршрутизации и в списке линий будет выделена красным.
{.is-warning}

*Название*
- Название линии, по которому ее можно будет идентифицировать при настройке маршрутизации.

*Номер абонента*
- Номер абонента, закрепленный за SIM-картой. Позволяет при выполнении настроек быстрее сориентироваться какая SIM-карта на каком порту используется.

*Отображаемое имя*
- Имя (CallerID name), которое будет отображаться у принимающей стороны. Подменяет имя, передаваемое внешней линией.

*CallerID*
- Номер (CallerID number), который будет отображаться у принимающей стороны. Под­меняет номер, передаваемый внешней линией.

## Медиа

> Параметры этой вкладки могут быть скопированы из общих настроек GSM нажатием кнопки  в правом верхнем углу.
{.is-info}

*Усиление/ослабление громкости*
- Коррекция уровня громкости.

*Распознавать DTMF*
- DTMF — тональные сигналы. Часто используются для управления функциями АТС, например, при донаборе номера или переводе вызова.

*Тип распознавания DTMF*
- Для GSM поддерживаются 2 метода распознавания: внутриполосный с relax-значениями (рекомендуется) и традиционный внутриполосный.

*Распознавание DTMF*
- Параметры, влияющие на процесс распознавания DTMF. При возникновении проблем с распознаванием в первую очередь попробуйте уменьшить мин. длительность DTMF.

## Прочее

*Другие настройки*
- Здесь можно указать дополнительные параметры для Asterisk (dongle.conf, секция данной внешней линии).