---
title: Установка SpRecord на Linux
description: 
published: true
date: 2022-04-29T11:02:52.423Z
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
wget https://sprecord.ru/files/downloads/linux/native/FirebirdCS-2.5.9.27139_amd64.deb -o firebird-2.5.9.deb
```
- Для arm64:
```
wget https://sprecord.ru/files/downloads/linux/native/FirebirdCS-2.5.9.27139_arm64.deb -o firebird-2.5.9.deb
```

2. Установите дистриубтив при помощи утилиты `gdebi`, которая автоматически разрешает все зависимости:
```
sudo apt install gdebi -y
sudo gdebi firebird-2.5.9.deb
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
wget https://sprecord.ru/files/downloads/sprecord_1.2.0-150_amd64.deb
```

2. Запустите установку при помощи gdebi:
```
sudo gdebi sprecord_1.2.0-150_amd64.deb
````

3. Запуск осуществляется при помощи команды "sprecord" либо из меню приложений.