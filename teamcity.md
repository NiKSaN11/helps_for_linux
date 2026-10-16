# teamcity

## Содержание

- [Создайте пользователя teamcity](#создайте-пользователя-teamcity)
- [Качаем архив TeamCity](#качаем-архив-teamcity)
- [Даём су](#даём-су)

---

## Создайте пользователя teamcity

```bash
sudo adduser teamcity
```

## Качаем архив TeamCity

```bash
cd /opt
sudo wget https://download.jetbrains.com/teamcity/TeamCity-2022.10.1.tar.gz
sudo tar xfz TeamCity-2022.10.1.tar.gz
```

## Даём су

```bash
sudo chown -R teamcity:teamcity /opt/TeamCity
```