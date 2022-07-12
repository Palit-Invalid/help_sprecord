---
title: Установка SpRecord на Linux
description: 
published: true
date: 2022-07-12T05:51:37.065Z
tags: установка, linux, sprecord
editor: markdown
dateCreated: 2022-02-18T10:15:43.449Z
---

# Поддерживаемые дистрибутивы
- Основанные на DEB: Ubuntu 16.04+, Debian 8+, Astra Linux и т.д
- Основанные на RPM: CentOS 7+, Fedora и т.д.

# Установка

Для работы SpRecord'а требуется установить два пакета: firebirdcs и sprecord

- | firebirdcs | sprecord
| --- | --- | --- |
| deb | [Ссылка](https://sprecord.ru/files/downloads/linux/native/firebirdcs_2.5.9.27139_amd64.deb) | [Ссылка](https://sprecord.ru/files/downloads/linux/native/sprecord_1.2.0-151_amd64.deb) |
| rpm | [Ссылка](https://sprecord.ru/files/downloads/linux/native/firebirdcs-2.5.9.27139.x86_64.rpm) | [Ссылка](https://sprecord.ru/files/downloads/linux/native/sprecord-1.2.0_151.x86_64.rpm) |

Загрузите нужные пакеты и запустите установку. На deb дистрибутивах установку можно запустить командой:
```bash
apt install <путь_к_файлу>
```
На rpm
```bash
yum install <путь_к_файлу>
```
> После установки SpRecord'а требуется переподключить устройство записи к компьютеру, если оно было до этого подключено.
{.is-warning}

Запуск осуществляется при помощи команды `sprecord` либо из меню приложений.

# Удаление
Загрузите нужные пакеты и запустите установку. На deb дистрибутивах установку можно запустить командой:
```bash
apt purge sprecord firebirdcs
```
На rpm
```bash
yum install <путь_к_файлу>
```

# Техническая поддержка

> Для получения версии программы для компьютеров с процессорами ARM64 обращайтесь в техническую поддержку: `support@sprecord.ru`
{.is-info}
