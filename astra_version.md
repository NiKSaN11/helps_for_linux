# Astra_version

## Содержание

- [1.7.$VER_PATCH](#17ver_patch)
- [1.8.$VER_PATCH](#18ver_patch)

---

## 1.7.$VER_PATCH

```bash
case $VER_PATCH in
    2.11|3.10|4.11|5.16|6.14|7.7|9.46) VER_PATH="/uu/1" ;;
    3.13|6.15|7.9) VER_PATH="/uu/2" ;;
    1.8|2.5|3.7|4.7|5.9|6.11|7.6|8.6|9.41|10.72) VER_PATH="" ;;
```

## 1.8.$VER_PATCH

```bash
case $VER_PATCH in
    1.12|2.8|3.8) VER_PATH="/uu/1" ;;
    1.16) VER_PATH="/uu/2" ;;
    1.6|2.7|3.7|4.48|5.46|6.39) VER_PATH="" ;;
```