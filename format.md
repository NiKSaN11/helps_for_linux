# format

## Содержание

- [FAT32](#fat32)
- [NTFS](#ntfs)
- [exFAT](#exfat)
- [Ext4](#ext4)
- [XFS](#xfs)
- [Btrfs](#btrfs)

---

## Форматирование разделов

### FAT32

```bash
sudo mkfs.vfat /dev/sdb1
```

### NTFS

```bash
sudo mkfs.ntfs /dev/sdb1
```

### exFAT

```bash
sudo mkfs.exfat /dev/sdb1
```

### Ext4

```bash
sudo mkfs.ext4 /dev/sdb1   # аналогично для 1-3
```

### XFS

```bash
sudo mkfs.xfs /dev/sdb1
```

### Btrfs

```bash
sudo mkfs.btrfs /dev/sdb1
```