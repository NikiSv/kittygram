# Kittygram

Kittygram - веб-приложение для обмена фотографиями котиков.

### Установка докера на компьютер
- **Windows**: установите WSL [по инструкции](https://www.docker.com/products/docker-desktop/), после чего установите [Docker Desktop](https://www.docker.com/products/docker-desktop/). 
- **macOS**: установите [Docker Desktop](https://www.docker.com/products/docker-desktop/). 
- **Linux**: установите Docker Engine, используя готовый скрипт от Docker:

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
```

### Установка и запуск проекта 

1. Клонируйте репозиторий проекта:

```bash
git clone git@github.com:NikiSv/infra_sprint1.git
```

2. Перейдите в директорию проекта:

```bash
cd проект-название
```

3. Создайте файл `.env` и заполните необходимые переменные окружения.

```
POSTGRES_DB=название_базы_данных
POSTGRES_USER=имя_пользователя
POSTGRES_PASSWORD=пароль
DB_NAME=название_базы_данных
DB_HOST=db
DB_PORT=5432
PORT=значение_порта
```
4. Запустите приложение с помощью Docker Compose:

```bash
docker compose up -d
```
5. После успешного развертывания, примените миграции для бэкенда:

```bash
docker compose exec backend python manage.py migrate
```

6. Затем соберите статические файлы для бэкенда:

```bash
docker compose exec backend python manage.py collectstatic
```

7. Также соберите статические файлы для фронтенда:

```bash
docker compose exec frontend npm run build
```

8. После завершения всех шагов, проверьте работу приложения, открыв его веб-интерфейс в браузере:

```bash
http://localhost:PORT
```

11. Для остановки контейнеров, выполните следующую команду:

```bash
docker compose down
```
