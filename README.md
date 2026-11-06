# Установка Angie

- [Ubuntu](#ubuntu-2404)
- [Docker](#docker)

## Ubuntu 24.04

1. Установка вспомогательных пакетов для подключения репозитория Angie:

```
sudo apt-get update
sudo apt-get install -y ca-certificates curl
```

2. Получение открытого ключа репозитория Angie для проверки подлинности пакетов:

```
sudo curl -o /etc/apt/trusted.gpg.d/angie-signing.gpg \
            https://angie.software/keys/angie-signing.gpg
```

3. Подключение репозитория Angie:

```
echo "deb https://download.angie.software/angie/$(. /etc/os-release && echo "$ID/$VERSION_ID $VERSION_CODENAME") main" \
    | sudo tee /etc/apt/sources.list.d/angie.list > /dev/null
```

4. Обновление индексов репозиториев:

```
sudo apt-get update
```

5. Установка пакетов Angie:

```
sudo apt-get install -y angie
```

6. Установка сторонних модулей (https://angie.software/angie/docs/installation/external-modules/#install-thirdpartymodules):

```
sudo apt install -y angie-module-brotli angie-module-zstd
```

## Docker