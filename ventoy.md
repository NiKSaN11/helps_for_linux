# ventoy hide iso

## Содержание

- [Настройка ventoy.json](#настройка-ventoyjson)

---

## Настройка ventoy.json

В корне первого раздела создайте папку `ventoy` (если её нет).

Внутри неё создайте текстовый файл `ventoy.json`.

Запишите в него:

```json
{
  "VTOY_DEFAULT_SEARCH_ROOT": "/ISO"
}
```


# Ventoy_cfg

## Меню загрузки Astra Linux через F6

```grub
set timeout=10
set default=0

clear
echo "========================================"
echo "   ВЫБОР ЗАГРУЗКИ ASTRA LINUX"
echo "========================================"
echo ""

### ASTRA LINUX 1.7.10 (ядро 6.1.161) — раздел sdb5
menuentry "Astra Linux 1.7.10 Core 6.1" --class=astra {
    echo "Загрузка Astra Linux 1.7.10..."
    search --no-floppy --fs-uuid --set=root bc630ce5-287f-4212-a9da-2237459664a6
    linux /boot/vmlinuz-6.1.161-1-generic root=UUID=bc630ce5-287f-4212-a9da-2237459664a6 ro quiet
    initrd /boot/initrd.img-6.1.161-1-generic
}

### ASTRA LINUX 1.8.5 (ядро 6.12.60) — раздел sdb4
menuentry "Astra Linux 1.8.5 Core 6.12" --class=astra {
    echo "Загрузка Astra Linux 1.8.5..."
    search --no-floppy --fs-uuid --set=root a7ac8178-fe62-4e5d-a989-96b80201d3fe
    linux /boot/vmlinuz-6.12.60-1-generic root=UUID=a7ac8178-fe62-4e5d-a989-96b80201d3fe ro quiet
    initrd /boot/initrd.img-6.12.60-1-generic
}

### ASTRA LINUX 1.8.5 (ядро 6.1.158 - запасное) — раздел sdb4
menuentry "Astra Linux 1.8.5 Core 6.1" --class=astra {
    echo "Загрузка Astra Linux 1.8.5 (старое ядро)..."
    search --no-floppy --fs-uuid --set=root a7ac8178-fe62-4e5d-a989-96b80201d3fe
    linux /boot/vmlinuz-6.1.158-1-generic root=UUID=a7ac8178-fe62-4e5d-a989-96b80201d3fe ro quiet
    initrd /boot/initrd.img-6.1.158-1-generic
}

menuentry "<-- Вернуться в Ventoy" --class=vtoyret VTOY_RET {
    echo 'Возврат'
}

### УТИЛИТЫ
menuentry "Перезагрузка" --class=reboot {
    reboot
}

menuentry "Выключение" --class=halt {
    halt
}
```