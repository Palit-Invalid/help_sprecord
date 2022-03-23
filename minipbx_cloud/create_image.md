---
title: Создание образа
description: 
published: true
date: 2021-06-25T04:31:34.113Z
tags: 
editor: markdown
dateCreated: 2021-04-06T15:52:53.991Z
---

# Создание образа
## Создание tar архива
``` 
tar -cvf <имя_архива>.tar --exclude=<имя_архива>.tar --exclude=/proc --exclude=/sys  /
```
## Импорт и создание образа
``` 
docker image import <имя_архива>.tar <имя_образа>:<тэг>
```

Далее создаем Dockerfile:
```
FROM minipbx:base
ENV container docker
MAINTAINER "Palit_invalid" <dimkaaaa1@gmail.com>
RUN rm -rf /lib/systemd/system/sysinit.target.wants/*
RUN rm -rf /etc/systemd/system/*.wants/*
RUN rm -rf /lib/systemd/system/local-fs.target.wants/*
RUN rm -rf /lib/systemd/system/sockets.target.wants/*udev*
RUN rm -rf /lib/systemd/system/sockets.target.wants/*initctl*
RUN rm -rf /lib/systemd/system/basic.target.wants/*
RUN rm -rf /lib/systemd/system/anaconda.target.wants/*

RUN ln -s /lib/systemd/system/minipbx.service /etc/systemd/system/multi-user.target.wants/minipbx.service
RUN ln -s /lib/systemd/system/asterisk.service /etc/systemd/system/multi-user.target.wants/asterisk.service
RUN ln -s /lib/systemd/system/mariadb.service /etc/systemd/system/multi-user.target.wants/mariadb.service
RUN ln -s /lib/systemd/system/avahi-daemon.service /etc/systemd/system/multi-user.target.wants/avahi-daemon.service
RUN ln -s /lib/systemd/system/avahi-daemon.socket /etc/systemd/system/multi-user.target.wants/avahi-daemon.socket
RUN ln -s /lib/systemd/system/fail2ban.service /etc/systemd/system/multi-user.target.wants/fail2ban.service
RUN ln -s /lib/systemd/system/rsyslog.service /etc/systemd/system/multi-user.target.wants/rsyslog.service
RUN ln -s /lib/systemd/system/platform-fix.service /etc/systemd/system/multi-user.target.wants/platform-fix.service
RUN ln -s /lib/systemd/system/cron.service /etc/systemd/system/multi-user.target.wants/cron.service

STOPSIGNAL SIGRTMIN+3
VOLUME [ "/sys/fs/cgroup" ]
CMD [ "/usr/sbin/init" ]
```
Выполняем сборку нового базового образа:
```
docker build -t <новое_имя> .
```
Запускаем контейнер из нового образа:
```
docker run -d -v /sys/fs/cgroup:/sys/fs/cgroup:ro --tmpfs /tmp --tmpfs /run --tmpfs /run/lock --name=<имя_контейнера> <имя_образа> 
```
