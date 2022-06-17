---
title: Установка SpRecord на Linux
description: 
published: true
date: 2022-06-17T10:36:44.206Z
tags: 
editor: markdown
dateCreated: 2022-02-18T10:15:43.449Z
---

## Поддерживаемые дистрибутивы
- Основанные на DEB: Ubuntu 16.04+, Debian 9+, Astra Linux и т.д
- Основанные на RPM: CentOS 7+, Fedora и т.д.

Для работы SpRecord'а требуется установить два пакета: firebirdcs и sprecord

- | firebirdcs | sprecord
| --- | --- | --- |
| deb | [Ссылка](https://sprecord.ru/files/downloads/linux/native/firebirdcs_2.5.9.27139_amd64.deb) | [Ссылка](https://sprecord.ru/files/downloads/linux/native/sprecord_1.2.0-151_amd64.deb) |
| rpm | [Ссылка](https://sprecord.ru/files/downloads/linux/native/firebirdcs-2.5.9.27139.x86_64.rpm) | [Ссылка](https://sprecord.ru/files/downloads/linux/native/sprecord-1.2.0_151.x86_64.rpm) |

Загрузите нужные пакеты и запустите установку. На deb дистрибутивах установку можно запустить командой:
```
apt install <путь_к_файлу>
```
На rpm
```
yum install <путь_к_файлу>
```

Запуск осуществляется при помощи команды "sprecord" либо из меню приложений.