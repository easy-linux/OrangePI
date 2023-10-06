# Настройка OrangePI Zero LTS как контроллера умного дома

Видео по данной теме:
https://youtu.be/W4ATKyVfDDY

## Подготовка карты памяти

Образ операционной системы на базе Debian 11.7
https://sd-card-images.johang.se/boards/orange_pi_zero.html

Использованные комманды:

    mkdir ORANGEPI
    cd ./ORANGEPI

    wget https://dl.sd-card-images.johang.se/boots/2023-10-01/boot-orange_pi_zero.bin.gz

    wget https://dl.sd-card-images.johang.se/debians/2023-10-02/debian-bullseye-armhf-oyao7u.bin.gz
    
    gzip -d boot-orange_pi_zero.bin.gz
    gzip -d debian-bullseye-armhf-oyao7u.bin.gz

    cat boot-orange_pi_zero.bin debian-bullseye-armhf-oyao7u.bin > orangepi-debian-11.7.img

    gzip orangepi-debian-11.7.img


Тело скрипта записи на карту памяти:

    #!/bin/sh

    filename=$1
    
    sudo gzip -dc ./${filename} | sudo dd status=progress bs=4M of=/dev/yourDiskName


## Начальная настройка

    # создаем нового пользователя
    adduser easyit

    # меняем пароль текущему пользователю (root)
    passwd

    # обновляем пакеты до последних версий
    apt-get update
    apt-get upgrade

    # устанавливаем некоторые пакеты
    apt install -y sudo mc wget curl screen git parted

    # добавляем пользователя в sudo
    usermod -aG sudo easyit

    # перелогин пользователем easyit

    # Расширяем раздел до размера 60Гб (карта 64Гб)

    sudo parted

    # команды внутри parted

    resizepart 2
    yes
    60000MB
    quit

    # команды в терминале
    sudo resize2fs /dev/mmcblk0p2

## Установка Domoticz

https://www.domoticz.com/downloads/

    sudo bash -c "$(curl -sSfL https://install.domoticz.com)"

Подключение к Domoticz (по умолчанию):

    http://ваш_ip_адрес:8080

## Установка MQTT сервера

    sudo apt install mosquitto mosquitto_clients
    
    cd /etc/mosquitto
    sudo mosquitto_passwd password.txt public

    cd conf.d

    sudo mcedit ./local.conf

содержимое файла конфига:
    
    per_listener_settings true
    listener 1883 0.0.0.0
    allow_anonymous false
    password_file /etc/mosquitto/password.txt

мониторинг сервера:

    mosquitto_sub -h localhost -t \# -u public -P public

отправка сообщения:

    mosquitto_pub -h localhost -t domoticz/out -u public -P public -m '{"test": 123, "message": "Hello!"}'

## Установка Node-Red

https://nodered.org/docs/getting-started/raspberrypi

    sudo apt install build-essential git curl
    bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)

Подключение к Node-red:

    http://ваш_ip_адрес:1880

Доустановить плагин для dashboard