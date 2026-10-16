# FSTAB_mount

## Содержание

- [Основные команды](#основные-команды)
- [Примеры fstab](#примеры-fstab)
- [Применение изменений](#применение-изменений)
- [Дополнительные примеры](#дополнительные-примеры)

---

## Основные команды

### Древо дисков

```bash
lsblk
```

### Создание точек монтирования

```bash
sudo mkdir /mnt/...
```

### Монтирование

```bash
sudo mount /dev/<disk> /mnt/
```

### Временное монтирование Shared/nikonov

```bash
sudo mount -t cifs -o username=nikonov //192.168.0.48/Shared/nikonov /mnt
```

### Узнать UUID диска

```bash
sudo lsblk
sudo blkid /dev/диск
```

---

## Примеры fstab

### Диск 1 (Linux)

```
UUID=12345678-90ab-cdef-1234-567890abcdef /mnt/data1 ext4 defaults,noatime 0 2
```

### Диск 2 (NTFS для Windows)

```
UUID=ABCDEF1234567890 /mnt/windows ntfs-3g defaults,uid=1000,gid=1000,umask=022 0 0
```

---

## Применение изменений

### Редактируем fstab и монтируем диски

```bash
sudo nano /etc/fstab
```

### Проверяем на ошибки

```bash
sudo mount -a
```

### Обновляем fstab

```bash
sudo systemctl daemon-reload
```

---

## Дополнительные примеры

Пример рабочего fstab:

```
UUID=6a92f87d-0380-4fa1-a3ee-01b494cff555 /                       ext4    defaults        1 1
UUID=FD33-DDA3          /boot/efi               vfat    umask=0077,shortname=winnt 0 2
UUID=d0e357e0-7fc2-49ea-ad7d-32e1c837a52c /home                   ext4    defaults        1 2
UUID=d9eb4ade-cf9f-48ed-9871-87106f300174 none                    swap    defaults        0 0
UUID=65d8f51d-313f-4b3d-b2d0-fa65cbfc8c35 /mnt/astra17                ext4    defaults         0 2
UUID=E8FA2FAFFA2F7948   /mnt/win                ntfs-3g defaults,uid=1000,gid=1000,umask=022 0 0
/dev/sr0 /media/cdrom0 udf,iso9660 user,noauto 0 0
//server/share /network    cifs     credentials=/etc/smbpass 0 0
```

### Монтирование локальной папки

```
/исходная/папка /целевая/папка none bind 0 0
```

### CIFS-шара с логином и паролем

```bash
//192.168.1.100/Shared /mnt/win_share cifs username=User,password=Pass123,vers=3.0 0 0
```

Пароль и версию 3.0 можно по идее не писать.