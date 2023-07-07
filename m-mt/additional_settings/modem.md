---
title: Настройка модема
description: 
published: true
date: 2023-07-07T07:07:54.364Z
tags: модем, интернет, usb, 3g
editor: markdown
dateCreated: 2021-06-01T07:36:15.810Z
---

# Настройка через NetworkManager
## Установка пакетов
```bash
apt update && apt install ppp modemmanager && systemctl enable --now ModemManager
```
## Проверка модема
```bash
mmcli -L
```
Модем может появиться не сразу! Если не появился, то подождать минуту. Иногда требуется  физическое переподключение модема.
## Настройка подключения
Для автозаполнения можно использовать табуляцию.
```bash
nmcli connection add ifname <tty*> autoconnect yes con-name <имя_соединения> type gsm apn <apn> 
nmcli connection up <имя_соединения>
```

# Настройка через Sakis3g
### Загрузка
``` bash
mkdir ~/3g
cd 3g/
wget http://sourceforge.net/projects/vim-n4n0/files/sakis3g.tar.gz
tar -xzvf sakis3g.tar.gz
sudo chmod +x sakis3g
```
### Проверка программы в интерактивном режиме
```bash
./sakis3g --interactive
```
## Umtskeeper
### Загрузка
```bash
wget http://raw.githubusercontent.com/daladim/umtskeeper/master/umtskeeper
chmod +x umtskeeper
```
### Проверка
```bash
./umtskeeper --sakisoperators "USBINTERFACE='0' OTHER='USBMODEM' USBMODEM='12d1:1001' APN='CUSTOM_APN' CUSTOM_APN='internet.mts.ru' APN_USER='mts' APN_PASS='mts'" --sakisswitches "--sudo --console" --devicename 'Huawei' --nat 'no'
```
### Автозагрузка
```bash
echo '/home/sprecord/3g/umtskeeper --sakisoperators "USBINTERFACE='0' OTHER='USBMODEM' USBMODEM='12d1:1001' APN='CUSTOM_APN' CUSTOM_APN='internet.mts.ru' SIM_PIN='0000' APN_USER='mts' APN_PASS='mts'" --sakisswitches "--sudo --console" --devicename 'Huawei' --log --silent --nat 'no' & >> /etc/rc.local
```
### Как правильно должен выгледеть файл автозагрузки /etc/rc.local
![mt_etc_rc.local.jpg](/m-mt/mt_etc_rc.local.jpg)
