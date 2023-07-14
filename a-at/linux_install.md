---
title: Установка SpRecord на Linux
description: 
published: true
date: 2023-07-14T12:05:48.108Z
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
| deb | [Ссылка](https://sprecord.ru/files/downloads/linux/native/firebirdcs_2.5.9.27139_amd64.deb) | [Ссылка](https://sprecord.ru/files/downloads/linux/native/sprecord_1.2.0.151_amd64.deb) |
| rpm | [Ссылка](https://sprecord.ru/files/downloads/linux/native/firebirdcs-2.5.9.27139.x86_64.rpm) | [Ссылка](https://sprecord.ru/files/downloads/linux/native/sprecord-1.2.0.151.x86_64.rpm) |

### Для ALt Linux необходимо скачать и установить дополнительные пакеты

http://de.archive.ubuntu.com/ubuntu/pool/universe/a/appmenu-gtk-module/appmenu-gtk2-module_0.7.3-2_amd64.deb
http://de.archive.ubuntu.com/ubuntu/pool/universe/a/appmenu-gtk-module/appmenu-gtk-module-common_0.7.3-2_all.deb
http://de.archive.ubuntu.com/ubuntu/pool/universe/a/appmenu-gtk-module/libappmenu-gtk2-parser0_0.7.3-2_amd64.deb

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

# Настройка и прослушивание записей
Прослушивание записей осуществляется через удобный веб интерфейс, который работает на 8080-м порту. Для его открытия введите в адресной строке браузера `localhost:8080`. Логин по умолчанию `admin`, без пароля.
![sprecord_linux_web.jpg](/sprecord/screenshots/sprecord_linux_web.jpg)

Для доступа к записям с другого компьютера в адресной строке необходимо вместо `localhost` ввести IP-адрес компьютера, на котором установлен SpRecord.

# Удаление
На DEB-подобных дистрибутивах:
```bash
apt purge sprecord firebirdcs
```
На RPM:
```bash
yum remove sprecord firebirdcs
```

# Техническая поддержка

> Для получения версии программы для компьютеров с процессорами ARM64 обращайтесь в техническую поддержку: `support@sprecord.ru`
{.is-info}
