---
title: Запуск контейнера
description: 
published: true
date: 2021-05-05T14:16:40.460Z
tags: 
editor: markdown
dateCreated: 2021-04-06T15:53:40.103Z
---

Перед запуском необходимо создать подсеть:
```
docker network create --subnet=172.18.0.0/16 <имя_подсети>
```
Запуск осуществляется через скрипт:
```
#!/bin/bash
WEB_PORT=8080
SIP_PORT=5060
MIN_RTP=10000
MAX_RTP=10100
NAME_IMAGE=minipbx
NAME_CONTAINER=test_name
IP=<адрес>
NETWORK=<имя_подсети>
S3FS=/путь/к/s3

docker run -d -v /sys/fs/cgroup:/sys/fs/cgroup:ro \
		--tmpfs /tmp --tmpfs /run --tmpfs /run/lock \
    -v $S3FS:/mnt/records:rw \
    -p $WEB_PORT:80 \
    -p $SIP_PORT:5060 -p $SIP_PORT:5060/udp \
    -p $MIN_RTP-$MAX_RTP:$MIN_RTP-$MAX_RTP \
    -p $MIN_RTP-$MAX_RTP:$MIN_RTP-$MAX_RTP/udp \
    --name $NAME_CONTAINER \
    --net $NETWORK \
    --ip $IP \
    -e MIN_RTP=$MIN_RTP \
    -e MAX_RTP=$MAX_RTP \
    --privileged $NAME_IMAGE
```