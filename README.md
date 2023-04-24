# Настройка одноплатного микрокомпьютера на примере OrangePI Zero LTS

## Подготовка карты памяти

Просмотр всех подключенных дисков
```
# Mac:
diskutil list

# Mac && Linux:
df -h
```


Размонтировать карточку перед записью (/dev/diskXXX - Ваше устройство)
```
# Mac:
diskutil unmountDisk /dev/diskXXX

# Linux:
umount /dev/diskXXX
```

Запись образа диска на карточку
```
# Mac && Linux:
sudo dd if=<disk_image>.img of=/dev/diskXXX bs=1m
```

## Подключение к OranePI по USB (где /dev/tty.YourTTY это устройство через которое подключена Апельсинка)
```
screen /dev/tty.YourTTY 19200
```

## Настройка беспроводной сети

Список всех сетевых интерфейсов
```
nmcli d
```

Убеждаемся, что вайфай включен
```
nmcli r wifi on
```

Просмотр доступных WIFI сетей
```
nmcli d wifi list
```

Подключиться к сети (где my_wifi - имя точки доступа)
```
nmcli d wifi connect my_wifi password <password>
```

Подключение к скрытой сети:
```
nmcli c add type wifi con-name <name> ifname wlan0 ssid <ssid>
nmcli c modify <name> wifi-sec.key-mgmt wpa-psk wifi-sec.psk <password>
nmcli c up <name>
```

## Добавление пользователя с правами админа

Создание пользователя
```
adduser username
```

Добавление юзера в группу SUDO
```
usermod -aG sudo easyit
```

## Обновление всех установленных пакетов
```
sudo apt update
sudo apt upgrade
```

## Дополнительная установка

Доустановка трех пакетов
```
sudo apt install mc git wget
```

Установка nvm и nodekjs
```
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash

nvm ls-remote
nvm install 16
```





