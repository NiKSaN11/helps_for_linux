# 📖 Samba — настройка и шпаргалка

## Содержание

### Основное

- [1. Установка](#1-установка)
- [2. Запуск службы](#2-запуск-службы)
- [3. Пользователи и права](#3-пользователи-и-права)
- [4. Общая папка](#4-общая-папка)
- [5. Конфигурация smb.conf](#5-конфигурация-smbconf)
- [6. Доступ с клиентов](#6-доступ-с-клиентов)

### Дополнительно

- [7. Примечания для Astra Linux](#7-примечания-для-astra-linux)
- [8. Шпаргалка](#8-шпаргалка)

### Приложение

- [9.1. Refusing to operate on alias](#91-refusing-to-operate-on-alias)
- [9.2. Диагностика](#92-диагностика)

---

## 1. Установка

```bash
sudo apt install fly-admin-samba
```

> **NOTE.** Пакет `fly-admin-samba` тянет за собой `samba`, `smbclient`, `cifs-utils` и графическую утилиту настройки.

---

## 2. Запуск службы

```bash
sudo systemctl enable --now smbd.service
sudo systemctl enable --now nmbd.service
```

> **IMPORTANT.** Используйте именно `smbd` и `nmbd`, а не алиасы `smb` / `nmb`. Подробнее — в разделе [9.1](#91-refusing-to-operate-on-alias).

---

## 3. Пользователи и права

### 3.1. Системные пользователи

```bash
sudo useradd -m -s /usr/sbin/nologin ivanov
sudo useradd -m -s /usr/sbin/nologin petrov
```

- `-m` — создать домашний каталог
- `-s /usr/sbin/nologin` — запретить локальный вход, оставить только Samba

### 3.2. Группа для доступа к шаре

```bash
sudo groupadd smbusers
sudo usermod -aG smbusers ivanov
sudo usermod -aG smbusers petrov
```

### 3.3. Пользователи Samba

> **IMPORTANT.** База паролей Samba отдельная от системной.

```bash
sudo smbpasswd -a ivanov
sudo smbpasswd -a petrov
```

Проверка:

```bash
sudo pdbedit -L
```

---

## 4. Общая папка

```bash
sudo mkdir -p /srv/share
sudo chown -R root:smbusers /srv/share
sudo chmod -R 2770 /srv/share
```

**🔐 Разбор `2770`**

| Бит | Значение |
|---|---|
| 2 | SGID — новые файлы наследуют группу `smbusers` |
| 7 | Владелец — полный доступ |
| 7 | Группа — полный доступ |
| 0 | Остальные — нет доступа |

---

## 5. Конфигурация smb.conf

Редактируем `/etc/samba/smb.conf`. В конце файла добавляем:

```ini
[share]
   comment = Общая папка для пользователей
   path = /srv/share
   browseable = yes
   read only = no
   guest ok = no
   valid users = @smbusers
   force group = smbusers
   force create mode = 0660
   force directory mode = 0770
```

| Параметр | Назначение |
|---|---|
| `valid users = @smbusers` | Доступ только участникам группы |
| `guest ok = no` | Запрет анонимного входа |
| `force group = smbusers` | Все новые файлы получают группу `smbusers` |
| `force create mode` | Права на новые файлы |
| `force directory mode` | Права на новые папки |

Проверка и перезапуск:

```bash
testparm
sudo systemctl restart smbd nmbd
```

---

## 6. Доступ с клиентов

| Клиент | Команда / действие |
|---|---|
| Linux | `sudo mount -t cifs //IP/share /mnt/share -o username=ivanov` |
| Windows | `\\IP-АДРЕС\share` в проводнике |

Установка CIFS-утилит на Linux-клиенте:

```bash
sudo apt install cifs-utils
sudo mkdir -p /mnt/share
```

Проверка с сервера:

```bash
smbclient //127.0.0.1/share -U ivanov
```

При успехе — приглашение `smb: \>`.

> **WARNING.** При ошибке монтирования — см. раздел [9.2](#92-ошибка-монтирования-cifs).

---

## 7. Примечания для Astra Linux

- ☑ Графическая утилита: **Панель управления → Сеть → Общие папки (Samba)**
- ☐ Мандатный контроль (Special Edition): добавить `use socket MAC label = YES`
- ☑ Пользователь Linux ≠ пользователь Samba
- ☑ `testparm` — обязателен перед перезапуском
- ☑ `Weak crypto is allowed by GnuTLS` — не ошибка, а fallback для старых клиентов

> **CAUTION.** В защищённых версиях Astra Linux (ЗПС) мандатные метки могут блокировать доступ. Проверяйте `pdpl-file` для каталога `/srv/share`.

---

## 8. Шпаргалка

```bash
# 1. Установка
sudo apt install fly-admin-samba

# 2. Пользователи
sudo useradd -m -s /usr/sbin/nologin ivanov
sudo smbpasswd -a ivanov

# 3. Группа и папка
sudo groupadd smbusers
sudo usermod -aG smbusers ivanov
sudo mkdir -p /srv/share
sudo chown -R root:smbusers /srv/share
sudo chmod -R 2770 /srv/share

# 4. Конфиг
sudo nano /etc/samba/smb.conf
testparm
sudo systemctl restart smbd nmbd

# 5. Проверка
smbclient //127.0.0.1/share -U ivanov
```

---

## 9. Приложение: ошибки и решения

### 9.1. Refusing to operate on alias

**Симптом:**

```console
astra@astra:~$ sudo systemctl enable --now nmb.service
Failed to enable unit: Refusing to operate on alias name or linked unit file: nmb.service
```

**Причина:** `nmb.service` — это алиас (символическая ссылка) на реальный юнит `nmbd.service`. Systemd намеренно отказывается включать алиасы, чтобы не плодить циклические и битые ссылки в `.wants/`.

**Решение:**

```bash
sudo systemctl enable --now nmbd.service
```

> **TIP.** Та же логика для `smb.service` → всегда `smbd.service`.

**🧪 Варианты команды**

```bash
# 1. Классика с юзером
sudo mount -t cifs //192.168.0.173/share /mnt/share -o username=ivanov

# 2. С указанием домена/рабочей группы
sudo mount -t cifs //192.168.0.173/share /mnt/share -o username=ivanov,workgroup=WORKGROUP

# 3. Через файл с учётными данными
sudo mount -t cifs //192.168.0.173/share /mnt/share -o credentials=/etc/samba/credentials/share
```

> **WARNING.** Если `smbclient -L` показывает ресурс, а `mount` падает — проблема только в опциях монтирования, а не в сервере.

### 9.2. Диагностика

```bash
lsmod | grep cifs
systemctl status smbd nmbd
sudo pdbedit -L
ls -ld /srv/share
smbclient -L //192.168.0.173 -U ivanov
```