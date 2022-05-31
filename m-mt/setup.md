---
title: Настройка МТ
description: 
published: true
date: 2022-05-31T11:30:28.026Z
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

## Установка графической оболочки
### Установка необходимых пакетов
```
apt update && apt install -y xorg xserver-xorg-video-dummy lightdm xfce4 xfce4-goodies
```
### Создание виртуального монитора
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
      Modes "1600x900"
      Virtual 1600 900
    EndSubSection
EndSection
```
### Настройка автологина
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
Установить пакеты:
```
apt install tightvncserver x11vnc
```
> `tightvncserver` по сути нужен только для команды `vncpasswd`, которая меняет пароль. Её нужно как минимум один раз запустить от пользователя `sprecord`, чтобы пароль создался.

Для добавления в автозагрузку VNC-сервера надо создать файлx `/home/sprecord/.config/autostart/x11vnc.desktop`
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

# Установка SpRecord и Firebird
## Firebird
На сервере лежит DEB-пакет, который нужно установить
```
wget sprecord.ru/files/downloads/linux/native/firebirdcs_2.5.9.27139_armhf.deb -O firebird.deb
apt install ./firebird.deb
rm firebird.deb
```
По итогу должна запуститься служба `firebird-cs`
```
systemctl status firebird-cs
```

## SpRecord
Перенести директории `/home/sprecord/bin` и `/var/lib/sprecord{http,Localize}`. Дать права на исполнение:
```
chmod +x /home/sprecord/bin/*
```
Создать директории и файлы с необходимыми правами:
```
mkdir /var/log/sprecord && mkdir /var/cache/sprecord && touch /etc/sprecord.conf && \
chown -R sprecord:sprecord /var/log/sprecord && \
chown -R sprecord:sprecord /var/cache/sprecord && \
chown sprecord:sprecord /etc/sprecord.conf
```
Перенести библиотеку `libftd2xx.so.1.4.8` в `/usr/local/lib` и создать ссылку:
```
ln -sf /usr/local/lib/libftd2xx.so.1.4.8 /usr/local/lib/libftd2xx.so
```
Заблокировать загрузку модуля `ftdi_sio`
```
echo "blacklist ftdi_sio" > /etc/modprobe.d/ftdi.conf
```
Добавить в udev правило
```
echo 'ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6001", ATTRS{product}=="SpRecord USB Device", MODE="0666"' > /etc/udev/rules.d/99-ftdi-sprecord.rules
```

В ~/.bashrc добавить путь /home/sprecord/bin, чтобы можно было запускать sprecord хоть откуда
```
echo "PATH=$HOME/bin:$PATH" >> ~/.bashrc
```
### Дать доступ к GPIO (для реагирования на кнопки)  
```
groupadd gpio && adduser sprecord gpio
```
Создать `/etc/udev/rules.d/98-gpio.rules`:
```
SUBSYSTEM=="gpio*", PROGRAM="/bin/sh -c 'chown -R root:gpio /sys/class/gpio && chmod -R 770 /sys/class/gpio; chown -R root:gpio /sys/devices/platform/sunxi-pinctrl/gpio && chmod -R 770 /sys/devices/platform/sunxi-pinctrl/gpio;'"
```

### Установка зависимостей
MP3-кодек:
```
apt install libmp3lame0 at-spi2-core
```
Всё недостающее можно установить запустив скрипт обновления sprecord'а:
```
/home/sprecord/bin/update_sprecord.sh
```
