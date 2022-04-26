---
title: Запуск IPVideoRecord через Docker
description: 
published: true
date: 2022-04-26T09:51:15.043Z
tags: 
editor: markdown
dateCreated: 2022-04-26T09:51:15.043Z
---

# Установка Docker
Перед установкой АТС необходимо убедиться в наличии и в случае необходимости установить следующие компоненты:
- docker

В Ubuntu/Debian/Astra это можно сделать командой:
```
apt update && apt install -y docker.io
```
В CentOS/Fedora/RedHat:
```
yum install -y docker
```

# Загрузка образа
Загрузить образ можно при помощи команды:
```
docker pull cr.yandex/crp9f1mct0nssd2k2cic/ipvideoserver:4.0.4.519
```

# Запуск
## Через скрипт
```
IMAGE=cr.yandex/crp9f1mct0nssd2k2cic/ipvideoserver:4.0.4.519
SCRIPT_PATH=$(dirname $(readlink -f $0))
CONFIG_PATH="$SCRIPT_PATH/config"
LOG_PATH="$SCRIPT_PATH/log"
VIDEO_PATH="$SCRIPT_PATH/video"
PHOTO_PATH="$SCRIPT_PATH/photo"
PREPARATOR=disable

if [ $(id -u) !=  0 ]; then
echo "Run as root!"
exit 1
fi

docker run -it \
	-p 9460:9460 \
	-p 9860:9860 \
	--rm \
	--env PREPARATOR="$PREPARATOR" \
	-v "$CONFIG_PATH":/etc/ipvideorecord:rw \
	-v "$LOG_PATH":/var/log/ipvideorecord:rw \
	-v "$VIDEO_PATH":/videocam:rw \
	-v "$PHOTO_PATH":/photocam:rw \
	"$IMAGE"
```