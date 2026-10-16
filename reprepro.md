# reprepro

## Содержание

- [Удаление макета из репозитория](#удаление-макета-из-репозитория)
- [Добавление папки пакетов в репо](#добавление-папки-пакетов-в-репо)
- [Добавление бинарного deb-пакета](#добавление-бинарного-deb-пакета)

---

## Удаление макета из репозитория

```bash
sudo reprepro -b /home/astra/work/repo/product/pakt remove inftest-drv unpci
```

## Добавление папки пакетов в репо

```bash
sudo reprepro -b /home/astra/work/repo/product/pakt includedeb inftest-drv /home/astra/work/PAKT/*.deb
```

## Добавление бинарного deb-пакета

```bash
sudo reprepro -b /home/astra/work/repo/product/pakt includedeb inftest-drv /home/astra/work/PAKT/p_mds32-1.0.0-Linux.deb
```