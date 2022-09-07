---
title: Дополнительные настройки
description: 
published: true
date: 2022-09-07T08:36:15.771Z
tags: 
editor: markdown
dateCreated: 2022-09-07T08:32:41.382Z
---

# Настройка модема
## NetworkManager
1. Установка пакетов
```bash
apt update && apt install ppp modemmanager && systemctl enable --now ModemManager
```
2. Проверка модема
```bash
mmcli -L
```
> Модем может появиться не сразу! Если не появился, то подождать минуту. Иногда требуется  физическое переподключение модема.
{.is-warning}

3. Настройка подключения

Для автозаполнения можно использовать табуляцию.
```bash
nmcli connection add ifname <tty*> autoconnect yes con-name <имя_соединения> type gsm apn <apn> 
nmcli connection up <имя_соединения>
```

## Настройка через Sakis3g
1. Загрузка
``` bash
mkdir ~/3g
cd 3g/
wget http://sourceforge.net/projects/vim-n4n0/files/sakis3g.tar.gz
tar -xzvf sakis3g.tar.gz
sudo chmod +x sakis3g
```
2. Проверка программы в интерактивном режиме
```bash
./sakis3g --interactive
```

3. Загрузка Umtskeeper
```bash
wget http://raw.githubusercontent.com/daladim/umtskeeper/master/umtskeeper
chmod +x umtskeeper
```
4. Проверка
```bash
./umtskeeper --sakisoperators "USBINTERFACE='0' OTHER='USBMODEM' USBMODEM='12d1:1001' APN='CUSTOM_APN' CUSTOM_APN='internet.mts.ru' APN_USER='mts' APN_PASS='mts'" --sakisswitches "--sudo --console" --devicename 'Huawei' --nat 'no'
```
5. Автозагрузка
```bash
echo '/home/sprecord/3g/umtskeeper --sakisoperators USBINTERFACE=0 OTHER=USBMODEM USBMODEM=12d1:1001 APN=CUSTOM_APN CUSTOM_APN=internet.mts.ru SIM_PIN=0000 APN_USER=mts APN_PASS=mts --sakisswitches --sudo --console --devicename Huawei --log --silent --nat no &' >> /etc/rc.local
```

> Данные настройки повлияют только на последующие записи. Предыдущие не будут копироваться!
{.is-warning}

# Настройка выгрузки записей
## Копирование записей на сервер SpRecord
Устройство позволяет выгружать записи на удалѐнный сервер SpRecord и просмотр записей с его помощью. Для этого необходимо установить на любой компьютер под управлением Windows программу [SpRecord](https://sprecord.ru/files/downloads/SpRecord3103USB.zip) и настроить его как сервер:

![server_setup.jpg](/m-mt/server_setup.jpg)

Устройство нужно настроить как клиент, указав IP-адрес сервера SpRecord и порт.

![upload_sprecord.jpg](/m-mt/upload_sprecord.jpg)

После этого все записи будут автоматически копироваться на сервер, с которого можно их удобно просматривать при помощи ПО SpRecord.

## Копирование записей в облачный сервис [SpRecord Cloud](https://sprecord.com)

Устройство позволяет автоматически выгружать записи в облачный сервис SpRecord. Для этого необходимо зарегистрироваться в облачном сервисе sprecord.com и включить опцию копирования записей в облако в настройках, указав идентификатор/токен пользователя.

![token.jpg](/m-mt/token.jpg)

Настройка:

![upload_cloud.jpg](/m-mt/upload_cloud.jpg)

> Для выгрузки также необходим доступ в Интернет. Поэтому надо убедиться, что заданы корректные сетевые настройки.
{.is-info}

