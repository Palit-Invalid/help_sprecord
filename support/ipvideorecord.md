---
title: IPVideoRecord
description: 
published: true
date: 2022-04-25T12:20:14.631Z
tags: 
editor: markdown
dateCreated: 2022-04-25T12:14:44.140Z
---

# Установка через Docker
## Dockerfile
```
FROM ubuntu:20.04

RUN echo "deb http://security.ubuntu.com/ubuntu xenial-security main" >> /etc/apt/sources.list
RUN apt-get update 
RUN apt-get install -y --no-install-recommends \
    libssl1.0.0 \
    libqt5xmlpatterns5 \
    libqt5serialport5 \    
    libqca-qt5-2 \
    libqca-qt5-2-plugins \
    libqt5sql5 \
    libqt5sql5-sqlite \
    libqt5xml5 \
    libboost-timer1.71.0 \
    libgomp1 \
    libswresample3 \
    libavcodec58 \
    libavutil56 \
    libswscale5 \
    libavformat58

WORKDIR /root

RUN mkdir -p /usr/lib/ipvideoserver/plugins/crypto
RUN mkdir -p /usr/lib/ipvideoserver/plugins/sqldrivers
RUN mkdir -p /usr/lib/x86_64-linux-gnu/plugins/crypto
RUN mkdir -p /usr/lib/x86_64-linux-gnu/plugins/sqldrivers

RUN cp /usr/lib/x86_64-linux-gnu/qca-qt5/crypto/libqca-ossl.so /usr/lib/ipvideoserver/plugins/crypto
RUN cp /usr/lib/x86_64-linux-gnu/qt5/plugins/sqldrivers/libqsqlite.so /usr/lib/ipvideoserver/plugins/sqldrivers
RUN cp /usr/lib/x86_64-linux-gnu/qca-qt5/crypto/libqca-ossl.so /usr/lib/x86_64-linux-gnu/plugins/crypto
RUN cp /usr/lib/x86_64-linux-gnu/qt5/plugins/sqldrivers/libqsqlite.so /usr/lib/x86_64-linux-gnu/plugins/sqldrivers

COPY bin/3.9.4.508/iprecord/ipvideoserver_3.9.4.508_amd64.deb /root/ipvr.deb
COPY docker/install.sh /root

CMD ["/root/install.sh"]
```

## install.sh
```
dpkg -i ipvr.dev
/usr/bin/ipvideoserver -e
```