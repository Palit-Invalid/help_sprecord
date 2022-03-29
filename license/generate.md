---
title: Генерация ключей
description: 
published: true
date: 2022-03-29T06:37:41.889Z
tags: 
editor: markdown
dateCreated: 2022-03-29T06:37:41.889Z
---

Сначала необходимо залогиниться в Container Registry:
```
docker login \
         --username oauth \
         --password <токен> \
         cr.yandex
```
Токен находится [здесь](https://oauth.yandex.ru/authorize?response_type=token&client_id=1a6990aa636648e9b2ef855fa7bec2fb).
# miniPBX
Контейнер:
```
docker pull cr.yandex/crp5420n8nnorope90n0/minipbx_web_licenser:latest
```
Запуск осуществляется со следующими параметрами:
```
docker run -it -p 8080:80 \
    --name "web_pbx_licenser"  \
    -d \
    -m 50m \
    --restart always \
    -v /sys:/sys \
    --privileged \
    -v /etc/network:/etc/network \
    -w /web_app \
    --env LIC_PORT=42100 \
    --env LICENSE_IP=192.168.88.31 \
    cr.yandex/crp5420n8nnorope90n0/minipbx_web_licenser  \
    /bin/sh -c "gunicorn --chdir /web_app app:app -b 0.0.0.0:80 --timeout 240"
```

# Resident
Контейнер:
```
cr.yandex/crp5420n8nnorope90n0/voipresident-licenser:latest
```
Запуск осуществляется со следующими параметрами:
```
cd $(dirname $0)
DIR=$(pwd)
IMAGE="cr.yandex/crp5420n8nnorope90n0/voipresident-licenser"
mkdir -p shareddir

sudo docker run \
  -v "$DIR/shareddir":/root/shareddir \
  --name voipresident-licenser \
  --rm -it \
  "$IMAGE" licenser

exit 0
```
Определение платформы осуществляется по последним символам в коде активации:
- `==` - платформа PC
- `E=` - платформа arm

# Rutoken miniPBX
Контейнер
```

```


