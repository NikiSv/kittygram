# Kittygram

Kittygram - веб-приложение для обмена фотографиями котиков.

## Клонирование проекта с GitHub на сервер
- Вместо `<ваш_аккаунт>` подставьте свой логин, который используете на GitHub.

```bash
git clone git@github.com:ваш_аккаунт/infra_sprint1.git
```

### Установка докера на компьютер
Windows: установите WSL [по инструкции](https://www.docker.com/products/docker-desktop/), после чего установите [Docker Desktop](https://www.docker.com/products/docker-desktop/). 
macOS: установите [Docker Desktop](https://www.docker.com/products/docker-desktop/). 
Linux: установите Docker Engine, используя готовый скрипт от Docker:
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
```

### Создание Docker-образов

```bash
    cd frontend
    docker build -t <ваш_логин_на_DockerHub>/kittygram_frontend .
    cd ../backend
    docker build -t <ваш_логин_на_DockerHub>/kittygram_backend .
    cd ../nginx
    docker build -t <ваш_логин_на_DockerHub>/kittygram_gateway . 
```

Загрузите образы на DockerHub:

```bash
    docker push ваш_логин_на_DockerHub/kittygram_frontend
    docker push ваш_логин_на_DockerHub/kittygram_backend
    docker push ваш_логин_на_DockerHub/kittygram_gateway
```

### Управление контейнерами с Docker Compose
Запуск всех описанных в docker-compose.yml контейнеров:
```bash
docker compose up
```
Остановка всех контейнеров:
```bash
docker compose stop
```

### Деплой на сервер

 Установите Docker Compose на сервер:

```bash
    sudo apt update
    sudo apt install curl
    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh
    sudo apt install docker-compose
```

Скопируйте файлы `docker-compose.production.yml` и `.env` в директорию `вашего_проекта/` на сервере

Запустите Docker Compose в режиме демона:

```bash
    sudo docker-compose -f /home/имя_пользователя/ваш_проект/docker-compose.production.yml up -d
```

Выполните миграции, соберите статические файлы бэкенда и скопируйте их в `/backend_static/static/`:

```bash
    sudo docker-compose -f /home/имя_пользователя/имя_проекта/docker-compose.production.yml exec backend python manage.py migrate
    sudo docker-compose -f /home/имя_пользователя/имя_проекта/docker-compose.production.yml exec backend python manage.py collectstatic
    sudo docker-compose -f /home/имя_пользователя/имя_проекта/docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```

Откройте конфигурационный файл Nginx в редакторе nano `sudo nano /etc/nginx/sites-enabled/default` и измените настройки `location` в секции `server`, и замените `<порт>`, на котором должен работать проект:

```bash
    location / {
        proxy_set_header Host $http_host;
        proxy_pass http://127.0.0.1:<порт>;
    }
```

Перезапустите Nginx:

```bash
    sudo service nginx reload
```

### Настройка CI/CD

Файл workflow уже существует, для работы с ним добавьте секреты в GitHub Actions:

```bash
    DOCKER_USERNAME                # имя пользователя в DockerHub
    DOCKER_PASSWORD                # пароль пользователя в DockerHub
    HOST                           # IP-адрес сервера
    USER                           # имя пользователя
    SSH_KEY                        # содержимое приватного SSH-ключа (cat ~/.ssh/id_rsa)
    SSH_PASSPHRASE                 # пароль для SSH-ключа

    TELEGRAM_TO                    # ID вашего телеграм-аккаунта (можно узнать у @userinfobot, команда /start)
    TELEGRAM_TOKEN                 # токен вашего бота (получить токен можно у @BotFather, команда /token, имя бота)
```
