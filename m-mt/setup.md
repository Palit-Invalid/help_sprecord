---
title: Настройка МТ
description: 
published: true
date: 2022-05-31T06:40:13.318Z
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
apt update && apt install -y xorg xserver-xorg-video-dummy lightdm lightdm-gtk-greeter xfce4
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