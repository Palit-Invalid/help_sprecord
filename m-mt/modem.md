---
title: Настройка модема
description: 
published: true
date: 2022-06-09T07:50:36.936Z
tags: 
editor: markdown
dateCreated: 2021-06-01T07:36:15.810Z
---

# Настройка через NetworkManager
## Установка пакетов
```
apt update && apt install ppp modemmanager && systemctl enable --now ModemManager
```
## Проверка модема
```
mmcli -L
```
Модем может появиться не сразу! Если не появился, то подождать минуту. Иногда требуется  физическое переподключение модема.
## Настройка подключения
Для автозаполнения можно использовать табуляцию.
```
nmcli connection add ifname <tty*> autoconnect yes type gsm apn <apn> con-name <имя_соединения>
nmcli connection up <имя_соединения>
```

# Настройка через Sakis3g
### Загрузка
``` 
mkdir ~/3g
cd 3g/
wget http://sourceforge.net/projects/vim-n4n0/files/sakis3g.tar.gz
tar -xzvf sakis3g.tar.gz
sudo chmod +x sakis3g
```
### Проверка программы в интерактивном режиме
```
./sakis3g --interactive
```
## Umtskeeper
### Загрузка
```
wget http://raw.githubusercontent.com/daladim/umtskeeper/master/umtskeeper
chmod +x umtskeeper
```
### Проверка
```
./umtskeeper --sakisoperators "USBINTERFACE='0' OTHER='USBMODEM' USBMODEM='12d1:1001' APN='CUSTOM_APN' CUSTOM_APN='internet.mts.ru' APN_USER='mts' APN_PASS='mts'" --sakisswitches "--sudo --console" --devicename 'Huawei' --nat 'no'
```
### Автозагрузка
```
nano /etc/rc.local
/home/sprecord/3g/umtskeeper --sakisoperators \"USBINTERFACE='0' OTHER='USBMODEM' USBMODEM='12d1:1001' APN='CUSTOM_APN' CUSTOM_APN='internet.mts.ru' SIM_PIN='0000' APN_USER='mts' APN_PASS='mts'" --sakisswitches "--sudo --console" --devicename 'Huawei' --log --silent --nat 'no' &
```
