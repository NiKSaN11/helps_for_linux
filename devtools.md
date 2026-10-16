# devtools

## Содержание

- [Сравнение пакетов 1.7.8 и 1.8.5](#сравнение-пакетов-178-и-185)
- [Скачивание devtools](#скачивание-devtools)

---

## Сравнение пакетов 1.7.8 и 1.8.5

| Пакет | 1.7.8 | 1.8.5 |
|---|---|---|
| autoconf | есть | есть |
| binutils | установлен | установлен |
| qtcreator | есть (не отображается в synaptic) | есть (отображается в synaptic) |
| cmake | есть | есть |
| subversion | есть | есть |
| git | есть | есть |
| cifs-utils | есть | есть |
| xrdp | есть | есть |
| mesa-common-dev | есть (не отображается в synaptic) | есть (не отображается в synaptic) |
| qt*5-dev, qt*5-dev-tools | — | 95 файлов, 30 из которых повторяются из другого ПО |
| qttranslations5-l10n | установлен | установлен |
| build-essential | установлен | установлен |
| libqt5svg5-dev | есть | есть |
| libarchive-zip-perl | есть | **УСТАНОВЛЕН** |
| libqt5remoteobjects5 | есть | есть |
| libqt5serialport5-dev | есть | есть |
| libqt5serialbus5 | есть | есть |

---

## Скачивание devtools

### Для 1.7

```bash
sudo apt update
DEVTOOL_VER=1.7.11
sudo apt clean
sudo apt install autoconf qtcreator cmake subversion git ssh krdc cifs-utils xrdp mesa-common-dev qt*5-dev qt*5-dev-tools libqt5svg5-dev libarchive-zip-perl libqt5remoteobjects5 libqt5serialport5-dev libqt5serialbus5 -y --download-only
sudo mkdir -p /home/astra/test/devtools-$DEVTOOL_VER
sudo mv /var/cache/apt/archives/*.deb /home/astra/test/devtools-$DEVTOOL_VER
```

### Для 1.8

```bash
sudo apt update
DEVTOOL_VER=1.8.4
sudo apt clean
sudo apt install autoconf qtcreator cmake subversion git ssh krdc cifs-utils xrdp mesa-common-dev qt*5-dev qt*5-dev-tools libqt5svg5-dev libqt5remoteobjects5 libqt5serialport5-dev libqt5serialbus5 -y --download-only
sudo mkdir -p /home/astra/test/devtools-$DEVTOOL_VER
sudo mv /var/cache/apt/archives/*.deb /home/astra/test/devtools-$DEVTOOL_VER
```