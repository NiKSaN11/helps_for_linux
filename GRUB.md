# GRUB

## Содержание

### Основные пути и файлы

- [Папка загрузчиков](#папка-загрузчиков)
- [Конфиг grub.cfg](#конфиг-grubcfg)
- [Обновление grub](#обновление-grub)

### Опции в /etc/default/grub

- [Отключение recovery режима](#отключение-recovery-режима)
- [Отключение подменю](#отключение-подменю)

### Добавление Windows в загрузку

- [Включить os-prober](#включить-os-prober)
- [Кастомный пункт загрузки](#кастомный-пункт-загрузки)
- [Убрать пункт загрузки](#убрать-пункт-загрузки)

### Ручное добавление пункта меню

- [Пользовательские настройки 40_custom](#пользовательские-настройки-40_custom)
- [Узнать UUID EFI](#узнать-uuid-efi)

### Список загрузки

- [Реальный список загрузки в grub](#реальный-список-загрузки-в-grub)

### Ядра

- [Узнать названия ядер](#узнать-названия-ядер)
- [Прописать дефолтное ядро](#прописать-дефолтное-ядро)
- [Удаление старых версий ядер](#удаление-старых-версий-ядер)
- [Удаление старого ядра](#удаление-старого-ядра)
- [Пересборка всех ядер](#пересборка-всех-ядер)

### Пример конфига

- [Example](#example)

---

## Основные пути и файлы

### Папка загрузчиков

```bash
/etc/grub.d
```

Папка, где все загрузчики по порядку. Порядок меняется изменением цифр.

### Конфиг grub.cfg

```bash
/boot/grub/grub.cfg
```

Конфиг, удаляется после обновления `/etc/default/grub`.

### Обновление grub

```bash
sudo update-grub
```

---

## Опции в /etc/default/grub

### Отключение recovery режима

```bash
GRUB_DISABLE_RECOVERY="true"
```

Строка отвечает за отключение recovery режима.

### Отключение подменю

```bash
GRUB_DISABLE_SUBMENU=y
```

Отключает подменю.

---

## Добавление Windows в загрузку

Если винда не подтянулась.

### Включить os-prober

```bash
sudo nano /etc/default/grub
```

```bash
GRUB_DISABLE_OS_PROBER=false
```

Проверяет наличие других операционных систем.

```bash
sudo update-grub
```

### Кастомный пункт загрузки

Если винда подгрузилась, но надо сделать кастомку — инфу по подгруженной ОС берём из `/boot/grub/grub.cfg`.

### Убрать пункт загрузки

Убрать какой-то из пунктов загрузки можно просто убрав права:

```bash
sudo chmod -x /etc/grub.d/30_os-prober
```

---

## Ручное добавление пункта меню

Ручное добавление без пароля для загрузки ОС (`--unrestricted`).

### Пользовательские настройки 40_custom

Открываем файл для пользовательских настроек:

```bash
sudo nano /etc/grub.d/40_custom
```

Вставьте в конец файла:

```
menuentry "Windows 10/11 (UEFI)" --unrestricted --class windows --class os {
    insmod part_gpt
    insmod fat
    insmod chain
    search --fs-uuid --set=root XXXX-XXXX
    chainloader /EFI/Microsoft/Boot/bootmgfw.efi
}
```

### Узнать UUID EFI

```bash
sudo blkid | grep -i efi | cut -d '"' -f 2
```

---

## Список загрузки

### Реальный список загрузки в grub

```bash
grep -E "^menuentry|^submenu" /boot/grub/grub.cfg | nl -v 0
```

---

## Ядра

### Узнать названия ядер

```bash
sudo grep menuentry /boot/grub/grub.cfg | cut -d "'" -f2
```

### Прописать дефолтное ядро

```bash
kate /etc/default/grub
```

В самом низу:

```bash
GRUB_DEFAULT='Astra GNU/Linux, with Linux 6.1.152-1-generic'
```

### Удаление старых версий ядер

```bash
sudo apt purge linux-image-X.X.X-X-generic
```

Затем:

```bash
sudo update-grub
```

### Удаление старого ядра

```bash
sudo apt remove linux-image-5.4
```

### Пересборка всех ядер

```bash
update-initramfs -u -k all
```

---

## Пример конфига

### Example

```bash
GRUB_TIMEOUT=10
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
GRUB_CMDLINE_LINUX_DEFAULT="parsec.mac=0 quiet net.ifnames=0 intel_iommu=off"
GRUB_CMDLINE_LINUX=""
GRUB_CMDLINE_LINUX_HARDENED="slub_debug=P page_poison=1 slab_nomerge pti=on user.max_user_namespaces=0 kernel.kptr_restrict=1 vsyscall=none"
GRUB_GFXMODE=1920x1080
GRUB_DISABLE_SUBMENU=y
GRUB_DEFAULT='gnulinux-6.1.90-1-generic-advanced-230d73e6-1632-4fc6-8f7f-67a74fe4f881'
GRUB_SAVEDEFAULT=true
GRUB_BACKGROUND=/media/sf_VMs_SHARE/grub.jpg
GRUB_COLOR_NORMAL="blue/black"
GRUB_COLOR_HIGHLIGHT="red/white"
```