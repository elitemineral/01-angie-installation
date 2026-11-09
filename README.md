# Установка Angie

- [Ubuntu](#ubuntu-2404)
- [Docker](#docker)

## Ubuntu 24.04

1. Установка вспомогательных пакетов для подключения репозитория Angie

```
sudo apt-get update
sudo apt-get install -y ca-certificates curl
```

2. Получение открытого ключа репозитория Angie для проверки подлинности пакетов

```
sudo curl -o /etc/apt/trusted.gpg.d/angie-signing.gpg \
            https://angie.software/keys/angie-signing.gpg
```

3. Подключение репозитория Angie

```
echo "deb https://download.angie.software/angie/$(. /etc/os-release && echo "$ID/$VERSION_ID $VERSION_CODENAME") main" \
    | sudo tee /etc/apt/sources.list.d/angie.list > /dev/null
```

4. Обновление индексов репозиториев

```
sudo apt-get update
```

5. Установка пакетов Angie

```
sudo apt-get install -y angie
```

6. Установка сторонних модулей (https://angie.software/angie/docs/installation/external-modules/#install-thirdpartymodules)

```
sudo apt install -y angie-module-brotli angie-module-zstd
```

## Docker

1. Создание самоподписанного сертификата

```
sudo mkdir -p /etc/angie/ssl
sudo openssl req -x509 -nodes -days 365 \
    -newkey rsa:2048 \
    -keyout /etc/angie/ssl/server.key \
    -out /etc/angie/ssl/server.crt \
    -subj "/CN=localhost"
```

2. Подключение сертификата

```
server {
        listen 443 ssl default_server;
        server_name localhost;

        ssl_certificate     /etc/angie/ssl/server.crt;
        ssl_certificate_key /etc/angie/ssl/server.key;

        location / {
            root /usr/share/angie/html;
            index index.html;
        }
    }
```

3. Запуск контейнера

```
docker run --rm --name angie \
    -v /var/www:/usr/share/angie/html:ro \
    -v /etc/angie:/etc/angie:ro \
    -p 8080:80 -p 8443:443 \
    -d docker.angie.software/angie:latest
```

4. Проверка http

```
curl -I http://localhost:8080
```

5. Проверка https

```
curl -kI https://localhost:8443
```