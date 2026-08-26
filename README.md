# Taski в Docker

Приложение для планирования задач, упакованное в контейнеры,
с автоматическим деплоем через GitHub Actions.

> Форк учебного репозитория Яндекс Практикума.

## О проекте

Та же задача-планировщик, что и в проекте Taski, но перенесённая на Docker.
Бэкенд, фронтенд, база данных и Nginx-шлюз собираются в отдельные контейнеры и
запускаются одной командой. Настроен CI/CD: при пуше в основную ветку проект
проходит тесты, образы собираются и публикуются на Docker Hub, после чего
приложение автоматически обновляется на сервере.

## Возможности

- Создание, редактирование и удаление задач
- Отметка о выполнении
- REST API
- Запуск всего окружения одной командой
- Автоматические тесты, сборка образов и деплой при пуше

## Технологии

- **Бэкенд:** Python 3, Django, Django REST Framework
- **Фронтенд:** React
- **База данных:** PostgreSQL
- **Инфраструктура:** Docker, Docker Compose, Nginx
- **CI/CD:** GitHub Actions, Docker Hub

## Переменные окружения

В корне проекта создайте файл `.env`:

```
POSTGRES_DB=<имя базы>
POSTGRES_USER=<пользователь базы>
POSTGRES_PASSWORD=<пароль базы>
DB_HOST=db
DB_PORT=5432
SECRET_KEY=<секретный ключ Django>
DEBUG=False
ALLOWED_HOSTS=<домен или IP через запятую>
```

## Запуск локально

```bash
docker compose up --build
```

Затем применить миграции и собрать статику:

```bash
docker compose exec backend python manage.py migrate
docker compose exec backend python manage.py collectstatic
```

Приложение будет доступно по адресу http://localhost:8000/

## Запуск на сервере

```bash
docker compose -f docker-compose.production.yml up -d
```

## Секреты GitHub Actions

| Секрет | Назначение |
|---|---|
| `DOCKER_USERNAME` | Логин на Docker Hub |
| `DOCKER_PASSWORD` | Пароль или токен Docker Hub |
| `HOST` | Адрес сервера |
| `USER` | Пользователь на сервере |
| `SSH_KEY` | Приватный SSH-ключ |
| `SSH_PASSPHRASE` | Пароль от SSH-ключа |
| `TELEGRAM_TO` | ID чата для уведомлений |
| `TELEGRAM_TOKEN` | Токен Telegram-бота |

## Структура

```
taski-docker/
├── backend/                         # Django-приложение
├── frontend/                        # React-приложение
├── gateway/                         # Nginx-шлюз
├── docker-compose.yml               # локальное окружение
└── docker-compose.production.yml    # продакшн-окружение
```
