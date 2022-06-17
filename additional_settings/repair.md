---
title: Восстановление работоспособности после сбоя 
description: Возврат к заводскому состоянию
published: true
date: 2022-06-17T07:42:50.971Z
tags: 
editor: markdown
dateCreated: 2022-06-17T07:37:45.129Z
---

# Прошивка

Необходимо вскрыть корпус устройства и извлечь карту памяти MicroSD. Данную карту подключить к компьютеру через кардридер и далее действовать по инструкции:

1. Скачайте с нашего сайта образ в соответствии с объёмом карты памяти и распакуйте его (после распаковки он занимает 16, 32 или 64 гб соответственно):
 Образ | Объём (гб) | Особенности
| :---: | :---: | :--- |
| [Ссылка](https://sprecord.ru/files/downloads/m-mt/sprecord-orange-pi-zero-distr-16gb-2021-10-26.zip) | 16 | - |
| [Ссылка](https://sprecord.ru/files/downloads/m-mt/sprecord-orange-pi-zero-distr-32gb-2021-10-26.7z) | 32 | - |
| [Ссылка](https://sprecord.ru/files/downloads/m-mt/sprecord-orange-pi-zero-distr-64gb-2020-09-29.zip) | 64 | - |
| [Ссылка](https://sprecord.ru/files/downloads/m-mt/sprecord-orange-pi-zero-distr-16gb-2020-03-23-static-ip-192-168-88-111.zip) | 16 | Предустановлен статической адрес 192.168.88.111 (после установки можно поменять) |

2. Установите программу [Win32DiskImager](https://sourceforge.net/projects/win32diskimager/)

3. При помощи данной программы запишите образ на карту памяти, затем установите карту памяти в устройство и проверьте его работу.
> Удостоверьтесь, что правильно выбрано устройство для записи! Все данные на нём будут перезаписаны.
{.is-warning}

![imager.jpg](/m-mt/imager.jpg)
