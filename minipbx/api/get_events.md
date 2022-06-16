---
title: Получение событый
description: 
published: true
date: 2022-06-16T09:30:53.341Z
tags: 
editor: markdown
dateCreated: 2021-04-06T14:49:24.910Z
---

> Схемы запросов и ответов сервера https://api.sprecord.cloud/docs {.is-info}

# Механизмы получения событий

Для получения событий в реальном времени существует 2 механизма:
1. **Web hook** - сервер выполняет специальный запрос на сервер клиента
2. **SSE** - механизм получения асинхронных событий без применения сервера клиента

## Web Hook
Для получения web hook на свой сервер, необходимо выполнить запрос /sys/subscribe_events с параметрами:

```
{
  "hook": "https://your_url_here.com",
  "subscribe": [
    "answer_elsewhere",
    "incoming_call",
    "call_end"
  ]
}
```

- **https://your_url_here.com** - адрес сервера клиента
- **subscribe** - список событий, по которым присылать уведомления:
	1. **startup** - включение АТС 
	2. **incoming_call** - начало входящего звонка
	3. **outgoing_call** - начало исходящего звонка
	4. **start_talk**    - начало разговора
	5. **call_end**        - конец разговора
	6. **incoming_sms**  - входящая смс
	7. **answer_elsewhere** - перенаправление звонка

Перед тем как сохранить параметры API сервер проверяет, что сервер клиента доступен. Для этого он делает POST запрос c параметрами:

```
{
  device_id = device_id,
  event_type = verification,
  payload = verification_str
}
```
    
Сервер клиента должен вернуть `MD5 hash` строки verification_str. Если они совпадут этого события добавятся в список хуков и сервер вернет ответ вида 

```
{
  "hook": "https://your_url_here.com",
  "subscribe": [
    "answer_elsewhere",
    "incoming_call",
    "call_end"
  ]
}
```

где будут отражены текущие настройки web hook.

## SSE

SSE может использоваться, когда развёртывание сервера на стороне клиента затруднительно. Делается GET запрос `/sys/events`. Соединение данного типа не закрывается и данные от сервера приходят асинхронно. Сервер поддерживает до 3-х одновременных соединений. Принцип передачи похож на WebSocket, только в одну сторону (от сервера к клиенту). Тестировать можно с помощью браузера.

## Формат данных сообщений.

Формат данных при использовании хуков и SSE идентичен и содержит в себе следующую информацию:
```
{"device_id": "Y7d6hP0mjNBI+WBT+yptpgY7uY+zX1Oumg9CQ7nr7HACJHy9Fg8IhCr41wO3j0Jl7zUU=", "event_type": "incoming_call",
"payload": {"call_id": 8,
            "call_src": "+798299xxxxx",
            "call_dst": "107", 
            "trunk": "GSM1"}}}
```

- **device_id** - уникальный id устройства
- **event_type** - тип события
- **payload** - дополнительные данные, могут отличаться в зависимости от события.

Пример события конца разговора:
```
{"device_id": "Y7d6hP0mjNBI+WBT+yptpgY7uY+zX1Oumg9CQ7nr7HACJHy9Fg8IhCr41wO3j0Jl7zUU=", "event_type": "call_end",
 "payload": {"call_id": "8", 
             "record_id": 8,
             "duration": 0,
              "status": "no_answer"}
```

## Payload события

Payload в зависимости от событий может отличаться. Ниже приведены данные для основных событий

### startup :
{} 

для события startup нет дополнительных данных

### incoming_call,  outgoing_call, start_talk :
```
    call_id: str
    call_src: str
    call_dst: str
    trunk: str
```

- **call_id** - идентификатор звонка
- **call_src** - номер, с которого идет звонок
- **call_dst** - внутренний номер АТС
- **trunk** - имя транка, на который совершен звонок

### call_end :
```
    call_id: str
    record_id: int
    duration: Optional[int]
    status: RecordCallStatus
```

- **record_id**   - идентификатор записи разговора
- **duration** - длительность звонка в секундах

### status - статус разговора
```   
    'not_def'
    'no_answer'
    'overload'
    'error'
    'busy'
    'success'
```

### incoming_sms : 
```
    sms_src: str
    text: str
    trunk: str
    date: datetime
```

- **sms_src** - номер с которого пришла смс
- **text** - текст смс
- **trunk** -  имя транка на который пришла смс
- **date** - дата прихода смс


### answer_elsewhere :
```
    call_id: str
    number: str
```

- **number** - внутренний номер, на котором произошло событие.
