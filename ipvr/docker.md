---
title: Запуск IPVideoRecord через Docker
description: 
published: true
date: 2022-04-26T10:00:15.237Z
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
Назначение переменных:
| Переменная | Назначение |
| :--- | :--- |
| `CONFIG_PATH` | Директория для хранения конфигурационных файлов и базы данных. |
| `LOG_PATH` | Директория для хранения логов. |
| `VIDEO_PATH` | Директория для хранения видеозаписей. |
| `PHOTO_PATH` | Директория для хранения снимков. |
| `PREPARATOR` | Если установлен `TRUE`, то при запуске контейнера перед запуском сервера запустится программа, подготавливающая базу данных. Использовать только в том случае, если производится обновление с одной версии на другую. |

## Запуск через docker-compose
```
version: '2.4'
services:
  ipvideoserver:
    image: cr.yandex/crp9f1mct0nssd2k2cic/ipvideoserver:4.0.4.519
    restart: unless-stopped
    ports:
      - "9460:9460"
      - "9860:9860"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9860"]
      interval: 10s
      timeout: 5s
      retries: 3
      start_period: 30s
    volumes:
      - /mnt/config:/etc/ipvideorecord:rw
      - /mnt/log:/var/log/ipvideorecord:rw
      - /mnt/video:/videocam:rw
      - /mnt/photo:/photocam:rw
    environment:
      - PREPARATOR=TRUE
    networks:
      ipvr_net_backend:
        ipv4_address: 172.16.1.1
        aliases:
          - ipvr
          - ipvr-server

networks:
  ipvr_net_backend:
    driver: bridge
    ipam:
      config:
        - subnet: 172.16.1.0/24

```
Запуск осуществляется командой:
```
docker-compose up
```
Остановка:
```
docker-compose down
```
