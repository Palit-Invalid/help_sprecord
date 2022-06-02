---
title: Установка SpRecord на Linux
description: 
published: true
date: 2022-06-02T13:54:54.113Z
tags: 
editor: markdown
dateCreated: 2022-02-18T10:15:43.449Z
---

## Поддерживаемые дистрибутивы
- Ubuntu 16.04/18.04/20.04
- Linux Mint 20
- Astra Linux

## Установка Firebird
1. Загрузите дистрибутив.
- Для amd64:
```
wget https://sprecord.ru/files/downloads/linux/native/firebirdCS_2.5.9.27139_amd64.deb -O firebird.deb
```
- Для arm64:
```
wget https://sprecord.ru/files/downloads/linux/native/firebirdcs_2.5.9.27139_arm64.deb -O firebird.deb
```

2. Установите дистрибутив при помощи утилиты `apt`:
```
sudo apt install ./firebird.deb
```

3. Убедитесь, что служба `firebird-cs` запустилась:
```
systemctl status firebird-cs
```
Если нет, то запустите вручную:
```
systemctl enable --now firebird-cs
```

## Установка SpRecord
1. Загрузите дистрибутив.
- Для amd64:
```
wget https://sprecord.ru/files/downloads/linux/native/sprecord_1.2.0-151_amd64.deb -O sprecord_1.2.0.deb
```
- Для arm64
```
wget https://sprecord.ru/files/downloads/linux/native/sprecord_1.2.0-138_arm64.deb -O sprecord_1.2.0.deb
```

2. Запустите установку при помощи `apt`:
```
sudo apt install ./sprecord_1.2.0.deb
````

3. Запуск осуществляется при помощи команды "sprecord" либо из меню приложений.