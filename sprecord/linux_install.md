---
title: Установка SpRecord на Linux
description: 
published: true
date: 2022-04-29T09:09:41.728Z
tags: 
editor: markdown
dateCreated: 2022-02-18T10:15:43.449Z
---

## Поддерживаемые дистрибутивы
- Ubuntu 16.04/18.04/20.04
- Linux Mint 20

## Установка Firebird
1. Загрузите дистрибутив по ссылке: https://sprecord.ru/files/downloads/linux/native/FirebirdCS-2.5.9.27139_amd64.deb
```
wget https://sprecord.ru/files/downloads/FirebirdCS-2.5.9.27139_amd64.deb
```

2. Установите дистриубтив при помощи утилиты `gdebi`, которая автоматически разрешает все зависимости:
```
sudo apt install gdebi -y
sudo gdebi FirebirdCS-2.5.9.27139_amd64.deb
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
1. Загрузите дистрибутив по ссылке: https://sprecord.ru/files/downloads/linux/native/sprecord_1.2.0-151_amd64.deb
```
wget https://sprecord.ru/files/downloads/sprecord_1.2.0-150_amd64.deb
```

2. Запустите установку при помощи gdebi:
```
sudo gdebi sprecord_1.2.0-150_amd64.deb
````

3. Запуск осуществляется при помощи команды "sprecord" либо из меню приложений.