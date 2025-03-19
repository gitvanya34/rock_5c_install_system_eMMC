# rock_5c_install_system_eMMC
Radxa Rock5c Install the system on the eMMC Module without reader

Установка операционной системы на eMMC модуль Radxa Rock 5c без использования ридера

## Установка и использование rkdeveloptool

## Описание
**rkdeveloptool** — это инструмент от Rockchip для работы с устройствами через USB на платформах Linux/macOS. Его можно рассматривать как открытую версию утилиты **upgrade_tool**.

Если ваша операционная система не предоставляет **rkdeveloptool**, вам потребуется скомпилировать и установить его из исходного кода.

---
## Установка

### Debian / Ubuntu / ArchLinux
Выполните следующие команды для установки необходимых пакетов и компиляции:
```sh
sudo apt-get update
sudo apt-get install -y libudev-dev libusb-1.0-0-dev dh-autoreconf pkg-config libusb-1.0 build-essential git wget

git clone https://github.com/rockchip-linux/rkdeveloptool
cd rkdeveloptool

wget https://patch-diff.githubusercontent.com/raw/rockchip-linux/rkdeveloptool/pull/73.patch
wget https://patch-diff.githubusercontent.com/raw/rockchip-linux/rkdeveloptool/pull/85.patch

git am *.patch
autoreconf -i
./configure
make -j $(nproc)
sudo cp rkdeveloptool /usr/local/sbin/
```

### Установка на Ubuntu 22.10 и новее
Для более новых версий Ubuntu можно установить **rkdeveloptool** через пакетный менеджер:
```sh
sudo apt-get install rkdeveloptool
```

---
## Вход в режим Maskrom

1. Отключите карту microSD и питание устройства.
2. Установите eMMC.
3. Замкните отверстие Maskrom с помощью провода DuPont.
4. Подключите устройство к ПК через кабель USB-A — USB-A или USB-С — USB-A если позволяет материнская плата.
5. Подключите питание.
6. Если устройство обнаружено на ПК, оно вошло в режим Maskrom.


> **Примечание 1:** OTG-разъем на Radxa ROCK 5A — это порт USB 3.0 в верхней части платы. Он используется для подключения к ПК.

> **Примечание 2:** Проверить подключения можно командой
>
>  ~$ lsusb
> 
> Bus 001 Device 026: ID 2137:334b Fuzhou **Rockchip** Electronics Company

<img src="https://github.com/user-attachments/assets/bcb518e5-8c36-41c6-ba66-2bb41152fcf6" width="50%">

---
## Использование **rkdeveloptool**

### Запись образа на устройство

> **Важно:** **rkdeveloptool** не распаковывает файлы автоматически. Сначала необходимо распаковать образ вручную.

1. Распакуйте образ (пример для **rock-5c_debian_bullseye-test_kde_2.img.xz**):
   ```sh
   xz -d rock-5c_bookworm_kde_b1.output.img.xz
   ```

2. Запишите загрузчик:
   ```sh
   sudo rkdeveloptool db rk3588_spl_loader_v1.08.111.bin
   ```

3. Запишите системный образ:
   ```sh
   sudo rkdeveloptool wl 0 rock-5c_bookworm_kde_b1.output.img
   ```

4. Перезагрузите устройство:
   ```sh
   sudo rkdeveloptool rd
   ```

> **Примечание:** **rkdeveloptool** не поддерживает выбор Maskrom-устройства или носителя для записи. Подключайте только одно устройство и один носитель во избежание проблем. Если требуется прошить несколько устройств, используйте **upgrade_tool**.

---
## Дополнительная информация
Для получения дополнительной информации и диагностики используйте:
```sh
rkdeveloptool -h
```

Официальный репозиторий: [Rockchip Linux rkdeveloptool](https://github.com/rockchip-linux/rkdeveloptool)
[Guide](https://docs.radxa.com/en/rock5/rock5c/low-level-dev/maskrom/linux)
