# locale_repair

## Содержание

### Локаль и время

- [Сгенерировать локаль](#сгенерировать-локаль)
- [Установить все настройки](#установить-все-настройки)
- [Обновить текущую сессию](#обновить-текущую-сессию)
- [Установить время](#установить-время)

---

## Локаль и время

### Сгенерировать локаль

```bash
sudo locale-gen ru_RU.UTF-8
```

### Установить все настройки

```bash
sudo update-locale LANG=ru_RU.UTF-8 LC_ALL=ru_RU.UTF-8 LC_NUMERIC=ru_RU.UTF-8
```

### Обновить текущую сессию

```bash
export LANG=ru_RU.UTF-8 LC_ALL=ru_RU.UTF-8 LC_NUMERIC=ru_RU.UTF-8
```

### Установить время

```bash
sudo timedatectl set-timezone Europe/Moscow
```