# QNX_node

## Содержание

- [Генерация хостовых ключей](#генерация-хостовых-ключей)
- [Добавление в автозагрузку](#добавление-в-автозагрузку)
- [Настройка sshd_config](#настройка-sshd_config)
- [Запуск sshd](#запуск-sshd)

---

## Генерация хостовых ключей

```bash
ssh-keygen -t rsa -b 2048 -f /etc/ssh/ssh_host_rsa_key -N ''
ssh-keygen -t ecdsa -b 521 -f /etc/ssh/ssh_host_ecdsa_key -N ''
ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -N ''
```

## Добавление в автозагрузку

```bash
echo "/usr/sbin/sshd" >> /etc/rc.d/rc.local
```

## Настройка sshd_config

```bash
echo "PubkeyAuthentication yes" >> /etc/ssh/sshd_config
echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
```

## Запуск sshd

```bash
/usr/sbin/sshd
```