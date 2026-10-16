# Основные команды для работы

## Содержание

### Система и загрузка

- [Версия GRUB](#версия-grub)
- [Версия хотфикса](#версия-хотфикса)
- [Список запущенных служб](#список-запущенных-служб)
- [Проверить, слушает ли кто порт 22](#проверить-слушает-ли-кто-порт-22)
- [Синхронизация часов](#синхронизация-часов)
- [Сменить hostname](#сменить-hostname)
- [Загрузка при конфликте видеодрайверов](#загрузка-при-конфликте-видеодрайверов)
- [Ядра](#ядра)

### Журналы

- [Все логи с момента последней загрузки](#все-логи-с-момента-последней-загрузки)
- [Что привело к падению в прошлый раз](#что-привело-к-падению-в-прошлый-раз)
- [Список приоритетных загрузок](#список-приоритетных-загрузок)

### Диски и файловые системы

- [Бэкап nikonov на носитель](#бэкап-nikonov-на-носитель)
- [Узнать UUID диска подкачки](#узнать-uuid-диска-подкачки)
- [Проверка SSD](#проверка-ssd)
- [Включить защиту файла от перезаписи](#включить-защиту-файла-от-перезаписи)
- [Проверка и восстановление диска от ошибок](#проверка-и-восстановление-диска-от-ошибок)
- [Увидеть права в формате цифр](#увидеть-права-в-формате-цифр)
- [Сравнить ISO и диск побайтово](#сравнить-iso-и-диск-побайтово)
- [Обрезать вывод по символу](#обрезать-вывод-по-символу)

### Пакеты и репозитории

- [Если нет сертификатов на подключение репозитория](#если-нет-сертификатов-на-подключение-репозитория)
- [Вывод обновляемых пакетов после подключения репозитория](#вывод-обновляемых-пакетов-после-подключения-репозитория)
- [Добавление репозитория в sources.list через консоль](#добавление-репозитория-в-sourceslist-через-консоль)
- [Установить deb-файлы из примонтированного ISO](#установить-deb-файлы-из-примонтированного-iso)
- [Как загрузить зависимости пакетов](#как-загрузить-зависимости-пакетов)
- [Установка всех пакетов из текущей папки](#установка-всех-пакетов-из-текущей-папки)
- [Удаление пакета, игнорируя зависимости](#удаление-пакета-игнорируя-зависимости)
- [Как загрузить зависимости пакетов dkms](#как-загрузить-зависимости-пакетов-dkms)
- [Поиск по маске остатков от deb](#поиск-по-маске-остатков-от-deb)
- [Удаление ISO сборки](#удаление-iso-сборки)
- [Проверка, что удалится с зависимостями](#проверка-что-удалится-с-зависимостями)
- [Преобразование косячного кода в Unix-формат](#преобразование-косячного-кода-в-unix-формат)

### Безопасность

- [Проверка уровня защищённости](#проверка-уровня-защищённости)
- [Включить запрос sudo](#включить-запрос-sudo)
- [Включить ptrace полностью (до перезагрузки)](#включить-ptrace-полностью-до-перезагрузки)
- [Включить ptrace постоянно](#включить-ptrace-постоянно)
- [Проверка состояния ptrace](#проверка-состояния-ptrace)
- [Смена пароля root](#смена-пароля-root)

### VirtualBox

- [Добавить astra пользователя в группу vboxsf](#добавить-astra-пользователя-в-группу-vboxsf)
- [Узнать версию образа дополнений гостевой ОС](#узнать-версию-образа-дополнений-гостевой-ос)
- [Удалить гостевую ОС](#удалить-гостевую-ос)

### EFI

- [Увидеть параметры загрузки EFI](#увидеть-параметры-загрузки-efi)
- [Выставить свои загрузочные параметры](#выставить-свои-загрузочные-параметры)

### NVIDIA

- [Скачать драйвер](#скачать-драйвер)
- [Распаковать драйвер без установки и вывести список поддерживаемых карт](#распаковать-драйвер-без-установки-и-вывести-список-поддерживаемых-карт)

### Сеть

- [Пингануть всю подсеть (в примере 192.168.4.x)](#пингануть-всю-подсеть-в-примере-1921684x)

### xrdp

- [Когда не работает xrdp](#когда-не-работает-xrdp)

---

## Система и загрузка

### Версия GRUB

```bash
sudo grub-install --version
```

### Версия хотфикса

```bash
cat /etc/astra/hotfix_version
cat /etc/astra/hotfix_build_version
```

### Список запущенных служб

```bash
systemctl --type=service --state=running
```

### Проверить, слушает ли кто порт 22

```bash
sudo ss -tlnp | grep :22
```

### Синхронизация часов

```bash
sudo timedatectl set-ntp true
```

Проверка:

```bash
sudo systemctl status systemd-timesyncd
timedatectl
```

### Сменить hostname

```bash
sudo hostnamectl set-hostname <новое-имя>
```

### Загрузка при конфликте видеодрайверов

В меню GRUB нажать `e`, добавить в строку ядра `nomodeset`, затем `F10`.

### Ядра

Посмотреть названия:

```bash
sudo grep menuentry /boot/grub/grub.cfg | cut -d "'" -f2
```

Прописать дефолтное ядро — в `/etc/default/grub`, в самом низу:

```bash
GRUB_DEFAULT='Astra GNU/Linux, with Linux 6.1.152-1-generic'
```

Удаление старых версий ядер:

```bash
sudo apt purge linux-image-X.X.X-X-generic
sudo update-grub
```

Пересборка всех ядер:

```bash
sudo update-initramfs -u -k all
```

---

## Журналы

### Все логи с момента последней загрузки

```bash
journalctl -b
```

### Что привело к падению в прошлый раз

```bash
sudo journalctl -b -1
```

Или по конкретному boot id:

```bash
journalctl -b a73415fade0e4e7f4bea60913883d180dc
```

Ограничиться конкретным модулем:

```bash
journalctl -u ntp
```

### Список приоритетных загрузок

```bash
journalctl --list-boots
```

---

## Диски и файловые системы

### Бэкап nikonov на носитель

```bash
rsync -av --exclude='.git' /home/astra/nikonov/ /mnt/nikonov/
```

### Узнать UUID диска подкачки

```bash
sudo blkid | grep swap
# или
sudo blkid /dev/sda3
```

Если отличается от `fstab` — редактируем:

```bash
sudo nano /etc/fstab
```

Активируем новый id для swap:

```bash
sudo swapon -a   # далее перезагружаем
```

### Проверка SSD

```bash
sudo apt install smartmontools
sudo smartctl -a /dev/nvme0n1
sudo smartctl -l selftest /dev/nvme0n1
```

### Включить защиту файла от перезаписи

```bash
sudo chattr +i /path   # -i снимает иммунитет
```

### Проверка и восстановление диска от ошибок

Можно использовать при установке:

```bash
e2fsck -y /dev/диск
```

### Увидеть права в формате цифр

```bash
stat -c "%a %n" *
```

### Сравнить ISO и диск побайтово

Ничего не выводит, если нет ошибок:

```bash
cmp -l /dev/sr0 ./pakt-2026.02.06.iso
```

### Обрезать вывод по символу

```bash
cut -d'/' -f1 > base_pack.txt
```

---

## Пакеты и репозитории

### Если нет сертификатов на подключение репозитория

Подключаем временно stable:

```bash
apt install -y apt-transport-https ca-certificates
```

### Вывод обновляемых пакетов после подключения репозитория

```bash
apt list --upgradable
# установка
apt dist-upgrade
```

### Добавление репозитория в sources.list через консоль

```bash
echo "deb http://192.168.56.1/repo/alse/main stable main contrib non-free" | sudo tee -a /etc/apt/sources.list
```

### Установить deb-файлы из примонтированного ISO

```bash
find /mnt -type f -name "*.deb" -exec sudo dpkg -i {} \;
```

### Как загрузить зависимости пакетов

```bash
sudo apt install --download-only nvidia-driver-525
# пакеты будут сохранены в:
ls /var/cache/apt/archives/*.deb
cp /var/cache/apt/archives/*.deb ~/my_debs
```

### Установка всех пакетов из текущей папки

```bash
sudo dpkg -i *.deb
```

### Удаление пакета, игнорируя зависимости

```bash
sudo dpkg --remove --force-depends *.deb
```

### Как загрузить зависимости пакетов dkms

```bash
sudo apt-get --print-uris --yes -d --reinstall install dkms \
  $(apt-cache depends dkms | grep "  Depends:" | sed 's/  Depends://' | sed ':a;N;$!ba;s/\n//g') \
  | grep ^\' | cut -d\' -f2 > downloads.list
sudo wget --input-file ~/downloads.list
```

Скачивание пакетов, которые могут быть обновлены:

```bash
sudo apt upgrade --download-only
# или
sudo apt upgrade --print-uris | grep ^\' | cut -d\' -f2 >> downloads.list
```

### Поиск по маске остатков от deb

```bash
ls /var/lib/dpkg/info/ | grep <test>
```

Вывод инфы по удалению:

```bash
echo sudo rm -f /var/lib/dpkg/info/<test>* | tr ' ' '\n'
```

### Удаление ISO сборки

```bash
sudo apt remove ask-ka          # без зависимостей
sudo apt autoremove ask-ka
```

### Проверка, что удалится с зависимостями

```bash
sudo apt autoremove --dry-run имя_пакета
```

### Преобразование косячного кода в Unix-формат

```bash
dos2unix make_repository.py
```

---

## Безопасность

### Проверка уровня защищённости

```bash
sudo astra-modeswitch get
```

- `0` — Орёл (base)
- `1` — Воронеж (advanced)
- `2` — Смоленск (maximum)

```bash
sudo astra-modeswitch set x   # x = цифра уровня
```

### Включить запрос sudo

```bash
sudo astra-sudo-control enable
```

### Включить ptrace полностью (до перезагрузки)

```bash
sudo sysctl -w kernel.yama.ptrace_scope=0
# запретить:
sudo sysctl -w kernel.yama.ptrace_scope=1   # или 2, 3
```

### Включить ptrace постоянно

```bash
sudo astra-ptrace-lock enable   # disable — выключить
```

### Проверка состояния ptrace

```bash
cat /proc/sys/kernel/yama/ptrace_scope
```

### Смена пароля root

```bash
sudo passwd root
```

---

## VirtualBox

### Добавить astra пользователя в группу vboxsf

```bash
sudo usermod -aG vboxsf astra
```

### Узнать версию образа дополнений гостевой ОС

```bash
VBoxClient --version
```

### Удалить гостевую ОС

```bash
ls /opt/ | grep VBoxGuestAdditions   # отобразит версию
/opt/VBoxGuestAdditions-X.X.X/uninstall.sh
```

---

## EFI

### Увидеть параметры загрузки EFI

```bash
efibootmgr -v
```

### Выставить свои загрузочные параметры

```bash
sudo efibootmgr -o 0004,0002
```

---

## NVIDIA

### Скачать драйвер

```bash
wget https://us.download.nvidia.com/XFree86/Linux-x86_64/470.256.02/NVIDIA-Linux-x86_64-470.256.02.run
```

### Распаковать драйвер без установки и вывести список поддерживаемых карт

```bash
sudo bash ./NVIDIA-Linux-x86_64-470.256.02.run --extract-only
cd NVIDIA-Linux-x86_64-470.256.02
grep -i "geforce" README.txt
```

Рекомендовано после установки драйвера:

```bash
sudo update-initramfs -u -k all
```

---

## Сеть

### Пингануть всю подсеть (в примере 192.168.4.x)

```bash
for i in {1..254}; do ping -c 1 192.168.4.$i | grep "bytes from" & done
```

---

## xrdp

### Когда не работает xrdp

Версии после 1.8.3 Astra Linux:

```bash
sudo apt purge dbus-user-session
```

Перезагрузка.
