---
title: Альтернативные способы подключения
description: 
published: true
date: 2022-09-07T08:38:50.370Z
tags: 
editor: markdown
dateCreated: 2022-09-07T08:38:50.370Z
---

# SSH
Подключиться к устройству, используя протокол SSH можно несколькими способами:
1. Через программу Putty
2. Используя встроенный клиент SSH (доступен только на последних версиях Windows 10 и на всех дистрибутивах Linux)

## Putty
1. Загрузите и запустите программу [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
2. В интерфейсе программы выберите протокол `SSH` и 22-й порт.
3. В поле `Host Name` введите IP-адрес устройства и нажмите кнопку `Open`:
![putty.jpg](/m-mt/putty.jpg)
4. Откроется терминал, в котором требуется ввести логин и пароль. Логин - `root`, пароль - `orangepi`.
> При вводе пароля символы на экране не будут отображаться.
{.is-warning}
5. После проделанных шагов должен открыться интерфейс устройства.

## Встроенный клиент SSH
1. Откройте терминал и введите команду:
`ssh <имя>@<адрес>`, где
- <имя> - логин (root)
- <адрес> - IP-адрес устройства
2. Нажмите Enter
3. Если это первое подключение к устройству, то появится в предложение принять ключ. Введите `yes`, чтобы согласиться.
![fingerprint.jpg](/m-mt/fingerprint.jpg)
4. Далее потребуется ввести пароль. Введите `orangepi`.
> При вводе пароля символы на экране не будут отображаться.
{.is-warning}
5. После проделанных шагов должен открыться интерфейс устройства.

# VNC
Подключиться к устройству можно не только через браузер, но и с помощью программ удаленного доступа к рабочему столу через любой VNC-клиент. Например, можно использовать программу [TightVNC](https://sprecord.ru/files/downloads/m-mt/tightvnc-2.8.11-gpl-setup-64bit.msi).

Установите программу и запустите `TightVNC Viewer`. В поле `Remote Host` укажите IP-адрес устройства и порт 5900 через двойное двоеточие (например: 192.168.1.172::5900), затем нажмите `Connect`.

![tightvnc_setup.jpg](/m-mt/tightvnc_setup.jpg)

В ответ на запрос пароля укажите `123456`.

![tightvnc_con.jpg](/m-mt/tightvnc_con.jpg)

> Для того чтобы подключиться к устройству через Интернет, необходимо сделать так называемый проброс портов. В роутере, который подключен к Интернету, необходимо указать, что входящие пакеты на порт 5900 направить на порт 5900 и локальный адрес устройства. Поскольку на разных роутерах эта настройка по-разному настраивается, нет возможности указать более подробную инструкцию.
{.is-info}

# WinSCP
Чтобы получить прямой доступ к файлам записей, воспользуйтесь программой WinSCP (скачать программу можно на сайте sprecord.ru, раздел «Скачать»).
Откройте программу WinSCP. Выберите новое подключение.

Укажите IP-адрес в поле «Имя хоста», имя пользователя «sprecord» и пароль «sprecord». Нажмите кнопку «Войти».

Если появится окошко с вопросом, то выберите «Да».

Справа показаны файлы и папки устройства. Слева показаны файлы и папки вашего компьютера. Справа перейдите к папке хранения звукозаписей. По умолчанию это папка `/var/lib/sprecord/records/<год_месяц>/`.

Скопируйте файлы на ваш компьютер, нажав на правой панели кнопку «Получить».