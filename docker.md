# docker

## Содержание

- [Дополнительная изоляция Docker](#дополнительная-изоляция-docker)
- [Образы](#образы)
- [Docker Hub](#docker-hub)
- [Контейнеры](#контейнеры)
- [Копирование файлов](#копирование-файлов)
- [Логи и массовые операции](#логи-и-массовые-операции)
- [Создание образа через debootstrap](#создание-образа-через-debootstrap)
- [informtest-visa](#informtest-visa)
- [Проброс X11](#проброс-x11)
- [sources.list внутри контейнера](#sourceslist-внутри-контейнера)
- [Если образ не загружается с кучей ошибок](#если-образ-не-загружается-с-кучей-ошибок)

---

## Дополнительная изоляция Docker

### Установка вспомогательного пакета для rootless-режима

```bash
sudo apt install rootless-helper-astra
```

### Запуск rootless-службы Docker (с меткой безопасности 0:0:0:0)

```bash
sudo systemctl start rootless-docker@astra@0:0:0:0
sudo systemctl enable rootless-docker@astra@0:0:0:0
```

---

## Образы

### Посмотреть все образы

```bash
docker images
```

### Создать образ из Dockerfile

```bash
docker build -t <имя> .
```

### Переименовать образ (добавить тег)

```bash
docker tag <старое_имя> <новое_имя>:<тег>
```

### Удалить образ

```bash
docker rmi <имя_образа>
```

### Сохранить образ в файл

```bash
docker save -o <файл>.tar <имя_образа>
```

### Загрузить образ из файла

```bash
docker load -i <файл>.tar
```

---

## Docker Hub

### Поиск образа в Docker Hub

```bash
docker search astra
```

### Скачать образ

```bash
docker pull <имя_образа>
```

---

## Контейнеры

### Посмотреть работающие контейнеры

```bash
docker ps
```

### Посмотреть все контейнеры (включая остановленные)

```bash
docker ps -a
```

### Создать и запустить контейнер в фоне

```bash
docker run -d --name <имя> <образ> sleep infinity
```

### Создать и запустить контейнер

```bash
docker run -ti --name <имя> <образ> /bin/bash
```

### Войти в работающий контейнер в фоне

```bash
docker exec -ti <имя_контейнера> /bin/bash
```

### Запустить остановленный контейнер

```bash
docker start <имя_контейнера>
```

### Остановить контейнер

```bash
docker stop <имя_контейнера>
```

### Удалить контейнер

```bash
docker rm <имя_контейнера>
```

### Удалить контейнер принудительно (даже работающий)

```bash
docker rm -f <имя_контейнера>
```

### Сохранить изменения контейнера как новый образ

```bash
docker commit <имя_контейнера> <новый_образ>:<тег>
```

### Узнать hostname (ID контейнера)

```bash
hostname
```

---

## Копирование файлов

### Скопировать файл в контейнер

```bash
docker cp <файл> <контейнер>:/путь/
```

### Скопировать файл из контейнера

```bash
docker cp <контейнер>:/путь/файл ./
```

---

## Логи и массовые операции

### Посмотреть логи контейнера

```bash
docker logs <имя_контейнера>
```

### Удалить ВСЕ контейнеры (работающие и остановленные)

```bash
docker rm -f $(docker ps -aq)
```

### Удалить все остановленные контейнеры

```bash
docker container prune -f
```

### Остановить все работающие контейнеры

```bash
docker stop $(docker ps -q)
```

---

## Создание образа через debootstrap

### Создать образ

```bash
sudo debootstrap \
    --include=systemd,apt,gnupg,ca-certificates \
    --components=main,contrib,non-free \
    orel /tmp/astra-rootfs \
    https://dl.astralinux.ru/astra/stable/orel/repository
```

### Импортировать в Docker

```bash
sudo tar -C /tmp/astra-rootfs -c . | docker import - my-astra:latest
```

### Выкачивание нужной версии образа контейнера

```bash
sudo debootstrap \
    --verbose \
    --components=main,contrib,non-free \
    1.8_x86-64 \
    ./chroot-1.8.5 \
    https://dl.astralinux.ru/astra/frozen/1.8_x86-64/1.8.5/main-repository/
```

Импорт в Docker (после создания chroot):

```bash
sudo tar -C ./chroot-1.8.5 -c . | docker import - astra-1.8.5:latest
```

Проверка:

```bash
docker images
docker run -ti astra-1.8.5:latest cat /etc/astra_version
```

---

## informtest-visa

### Принудительная установка informtest-visa

```bash
dpkg --force-depends --force-confold -i /tmp/informtest-visa-5.8.7.3-Linux.deb
```

### Сказать dpkg, что пакет настроен (несмотря на ошибки)

```bash
echo 'unpci hold' | dpkg --set-selections
dpkg --force-all --configure -a
```

### Установить Qt5 и зависимости

```bash
apt update
apt install -y libqt5widgets5 libqt5gui5 libqt5core5a libqt5dbus5 \
    libqt5network5 libqt5xml5 libxcb-xinerama0 libxcb-icccm4 \
    libxcb-image0 libxcb-keysyms1 libxcb-randr0 libxcb-render-util0 \
    libxcb-shape0 libxcb-xfixes0 libxcb-xkb1 libxkbcommon-x11-0
```

---

## Проброс X11

### На хосте ОБЯЗАТЕЛЬНО

```bash
xhost +local:docker
```

### Создать и запустить контейнер с пробросом X11 и сетью хоста

```bash
docker run -ti --privileged \
    --name astra-visa \
    --network host \
    -e DISPLAY=$DISPLAY \
    -e QT_X11_NO_MITSHM=1 \
    -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
    -v /dev:/dev:rw \
    -v /lib/modules:/lib/modules:ro \
    astra-visa:1.6 \
    bash
```

### Чтобы потом зайти на него по id

```bash
docker exec -ti e2d5d8dc11b4 bash
```

### Если не стартует — сначала пробросить X11 и запустить

```bash
xhost +local:docker
docker start e2d5d8dc11b4
```

---

## sources.list внутри контейнера

### Создать файл sources.list

```bash
cat > /etc/apt/sources.list << 'EOF'
deb [trusted=yes] http://ci.inftest.loc:81/debianrepo/frozen/1.8_x86-64/1.8.5/main-repository/ 1.8_x86-64 contrib main non-free
deb [trusted=yes] http://ci.inftest.loc:81/debianrepo/frozen/1.8_x86-64/1.8.5/extended-repository/ 1.8_x86-64 contrib main non-free
deb [trusted=yes] http://ci.inftest.loc:81/inftest-drv/stable/ inftest-drv main
EOF
```

---

## Если образ не загружается с кучей ошибок

```bash
sudo systemctl stop docker
sudo mkdir -p /etc/docker
echo '{ "astra-sec-level": 6 }' | sudo tee /etc/docker/daemon.json
sudo systemctl start docker
sudo systemctl status docker
```