---
title: Настройка МТ
description: 
published: true
date: 2022-06-07T09:40:00.912Z
tags: 
editor: markdown
dateCreated: 2022-05-31T06:40:13.318Z
---

# Настройка ОС
## Статический MAC-адрес
1. Создать текстовый файл `nanopi-neo-stable-mac.dts` со следующим содержимым:
```
/dts-v1/;
/plugin/;

/ {
    compatible = "allwinner,sun8i-h3";

    /* 
     * uboot tries to write a MAC address from ${mac_node}
     * to the device tree at 'local-mac-address' within 'ethernet0'
     *   fdt set ethernet0 local-mac-address ${mac_node}
     * This obviously doesn't work if the device tree does not match the  
     * expected structure, resulting in the kernel creating a random MAC.
     * 
     * This overlay adjusts the device tree to accept uboot's MAC address 
     * by adding 
     * - 'ethernet0' alias for symbol '/soc/ethernet@1c30000'
     *   (or change to whatever path your existing symbol 'emac' points to)
     * - 'local-mac-address' to structure at 'emac'
     * 
     * Tested to work on a NanoPi NEO 1.4 - adjust for other devices as required
     */

    fragment@0 {
        target-path = "/aliases";
        __overlay__ {
            ethernet0 = "/soc/ethernet@1c30000";
        };
    };

    fragment@1 {
        target = <&emac>;
        __overlay__ {
            local-mac-address = [00 00 00 00 00 00];
        };
    };
};
```
2. Добавить его в загрузку:
```
armbian-add-overlay nanopi-neo-stable-mac.dts   
```
Взято отсюда: https://forum.armbian.com/topic/14525-mac-address-of-eth0-changes-on-every-boot/

## Установка необходимых пакетов
```
apt update && apt install -y xorg xserver-xorg-video-dummy lightdm xfce4 xfce4-goodies avahi-daemon iptables-persistent tightvncserver x11vnc usbmount
```
## Создание виртуального монитора
Необходимо создать файл `/usr/share/X11/xorg.conf.d/xorg.conf` с содержимым:
```
Section "Device"
    Identifier  "Configured Video Device"
    Driver      "dummy"
    VideoRam 192000
EndSection

Section "Monitor"
    Identifier  "Configured Monitor"
    HorizSync 31.5-48.5
    VertRefresh 50-70
EndSection

Section "Screen"
    Identifier  "Default Screen"
    Monitor     "Configured Monitor"
    Device      "Configured Video Device"
    DefaultDepth 24
    SubSection "Display"
      Viewport 0 0
      Depth 24
      Modes "1280x720"
      Virtual 1280 720
    EndSubSection
EndSection
```
## Настройка автологина
Необходимо создать файл `/etc/lightdm/lightdm.conf.d/11-armbian.conf` с содержимым:
```
[Seat:*]
user-session=xfce
greeter-show-manual-login=false
greeter-hide-users=false
allow-guest=false
autologin-user=sprecord
autologin-user-timeout=0
```
## Настройка VNC
> `tightvncserver` по сути нужен только для команды `vncpasswd`, которая меняет пароль. Её нужно как минимум один раз запустить от пользователя `sprecord`, чтобы пароль создался.

Для добавления в автозагрузку VNC-сервера надо создать файл `/home/sprecord/.config/autostart/x11vnc.desktop`
```
[Desktop Entry]
Encoding=UTF-8
Version=0.9.4
Type=Application
Name=x11vnc
Comment=
Exec=x11vnc -usepw -display :0 -q -forever
OnlyShowIn=XFCE;
StartupNotify=false
Terminal=false
Hidden=false
```

## Автомонтирование
Отредактировать файл настроек `/etc/usbmount/usbmount.conf` :
   - в параметр "MOUNTOPTIONS" добавить строку без кавычек ",uid=1000,gid=1000,utf8=1"
   
## Перенаправление портов
```
iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports 8080 && \
iptables -t nat -A POSTROUTING -p tcp --sport 8080 -j SNAT --to-source :80 && \
iptables-save > /etc/iptables/rules.v4
```

## Сервисы по управлению GPIO
```
wget sprecord.ru/files/downloads/linux/native/mt/gpio-tools.deb
apt install ./gpio-tools.deb
```
Для OrangePI надо можно отключить службы за ненадобностью:
```
systemctl disable latest && \
systemctl disable launch-usb && \
systemctl disable poweroff-button && \
systemctl disable fan-control
```

# Установка SpRecord и Firebird
## Firebird
На сервере лежит DEB-пакет, который нужно установить
```
wget sprecord.ru/files/downloads/linux/native/firebirdcs_2.5.9.27139_armhf.deb -O firebird.deb
apt install ./firebird.deb
rm firebird.deb
```
По итогу должен запуститься соек `firebird-cs`
```
systemctl status firebird-cs.socket
```

## SpRecord
### Установка зависимостей
``` 
apt install libmp3lame0 libmp3lame-dev libsndfile1 at-spi2-core wmctrl yad
```
### Установка программы
Скачать архив и распаковать его:
```
wget sprecord.ru/files/downloads/linux/native/mt/sprecord_mt.zip &&
unzip sprecord_mt.zip
```
| Директория | Расположение |
| --- | --- |
| bin | /home/sprecord |
| applications | /home/sprecord/.local/share |
| graceful-logout | /home/sprecord/.config |
| sprecord | /var/lib |
Файлы из директории `lib` поместить в `/usr/local/lib`.
Дать права на исполнение:
```
chmod +x /home/sprecord/bin/*
```
Всё недостающее можно установить запустив скрипт обновления sprecord'а:
```
/home/sprecord/bin/update_sprecord.sh
```
Создать директории и файлы с необходимыми правами:
```
mkdir /var/log/sprecord && mkdir /var/cache/sprecord && touch /etc/sprecord.conf && \
chown -R sprecord:sprecord /var/log/sprecord && \
chown -R sprecord:sprecord /var/cache/sprecord && \
chown sprecord:sprecord /etc/sprecord.conf
```
Заблокировать загрузку модуля `ftdi_sio`
```
echo "blacklist ftdi_sio" > /etc/modprobe.d/ftdi.conf
```
Добавить в udev правило
```
echo 'ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6001", ATTRS{product}=="SpRecord USB Device", MODE="0666"' > /etc/udev/rules.d/99-ftdi-sprecord.rules
```
создать скрипт "\usr\bin\sprecord" с таким содержимым (наличие такого скрипта в этой папке позволяет запускать программу спрекорд из любой папки):
```
#!/bin/sh
$HOME/bin/sprecord
exit $?  
```
### Дать доступ к GPIO (для реагирования на кнопки)  
```
groupadd gpio && adduser sprecord gpio
```
Создать `/etc/udev/rules.d/98-gpio.rules`.
- Для OrangePi:
```
SUBSYSTEM=="gpio*", PROGRAM="/bin/sh -c 'chown -R root:gpio /sys/class/gpio && chmod -R 770 /sys/class/gpio; chown -R root:gpio /sys/devices/platform/sunxi-pinctrl/gpio && chmod -R 770 /sys/devices/platform/sunxi-pinctrl/gpio;'"
```
- Для NanoPi:
```
SUBSYSTEM=="gpio*", PROGRAM="/bin/sh -c 'chown -R root:gpio /sys/class/gpio && chmod -R 770 /sys/class/gpio; chown -R root:gpio /sys/devices/platform/soc/1c20800.pinctrl/gpiochip0/gpio && chmod -R 770 /sys/devices/platform/soc/1c20800.pinctrl/gpiochip0/gpio;'"
```
### Добавить в автозагрузку

Для добавления в автозагрузку SpRecord'а надо создать файл `/home/sprecord/.config/autostart/sprecord.desktop`
```
[Desktop Entry]
Encoding=UTF-8
Version=0.9.4
Type=Application
Name=sprecord
Comment=
Exec=sprecord
OnlyShowIn=XFCE;
StartupNotify=false
Terminal=false
Hidden=false
```
### Убрать управление eth0 у NetworkManager'а
В файле `/etc/NetworkManager/NetworkManager.conf` поставить `managed=false`
> Иначе устройство будет получать два IP-адреса по DHCP.
