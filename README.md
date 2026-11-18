# Kittygram — социальная сеть для любителей котиков

## Описание проекта

Kittygram — веб-приложение, в котором пользователи могут делиться фотографиями своих котиков, публиковать информацию о них и просматривать записи других пользователей.

Цель проекта — предоставить удобную платформу для обмена фотографиями и информацией о домашних питомцах.

### Функциональность

- Регистрация и авторизация пользователей.
- Добавление котиков с фотографиями и описанием.
- Просмотр опубликованных котиков.
- Редактирование и удаление собственных записей.
- Работа с API через Django REST Framework.

## Технологический стек

- **Python 3.9** — язык программирования серверной части.
- **Django** — веб-фреймворк.
- **Django REST Framework** — создание REST API.
- **PostgreSQL 13** — система управления базами данных.
- **React** — клиентская часть приложения.
- **Nginx** — веб-сервер и обратный прокси.
- **Docker** — контейнеризация приложения.
- **Docker Compose** — управление контейнерами.
- **GitHub Actions** — автоматизация тестирования и сборки.
- **Docker Hub** — хранение Docker-образов.

## Структура проекта

```text
kittygram_final/
├── backend/                  # Серверная часть Django
│   ├── kittygram_backend/    # Настройки и конфигурация Django
│   ├── cats/                 # Приложение для работы с котиками
│   ├── Dockerfile
│   └── requirements.txt
├── frontend/                 # Клиентская часть React
│   └── Dockerfile
├── nginx/                    # Конфигурация Nginx
│   ├── Dockerfile
│   └── nginx.conf
├── tests/                    # Автоматические тесты
├── .github/
│   └── workflows/
│       └── main.yml          # GitHub Actions
├── .env.example              # Пример переменных окружения
├── docker-compose.yml        # Локальный запуск
├── docker-compose.production.yml
├── kittygram_workflow.yml    # Копия workflow для автотестов
└── tests.yml                 # Данные для автотестов
```

## Подготовка к запуску

Для запуска проекта необходимо установить:

- Docker Desktop или Docker Engine.
- Docker Compose.

### Настройка переменных окружения

В корневой директории проекта создайте файл `.env` на основе `.env.example`.

Пример содержимого:

```env
SECRET_KEY='ваш-секретный-ключ'
DEBUG=False
ALLOWED_HOSTS=localhost,127.0.0.1

POSTGRES_DB=kittygram
POSTGRES_USER=kittygram_user
POSTGRES_PASSWORD=kittygram_password
DB_NAME=kittygram
```

Для `SECRET_KEY` рекомендуется сгенерировать уникальное значение средствами Django.

Файл `.env` не должен попадать в репозиторий Git.

## Запуск проекта локально

Находясь в корневой директории проекта, выполните:

### 1. Сборка Docker-образов

```bash
docker compose build
```

### 2. Запуск контейнеров

```bash
docker compose up -d
```

### 3. Выполнение миграций базы данных

```bash
docker compose exec backend python manage.py migrate
```

### 4. Сбор статических файлов

```bash
docker compose exec backend python manage.py collectstatic --no-input
```

После запуска приложение будет доступно по адресу:

http://localhost:9000

### Остановка проекта

```bash
docker compose down
```

Для остановки и удаления контейнеров вместе с томами:

```bash
docker compose down -v
```

**Внимание:** команда с параметром `-v` удаляет тома, в том числе данные PostgreSQL.

## CI/CD и публикация Docker-образов

В проекте настроен GitHub Actions.

При отправке изменений в ветку `main` автоматически запускаются:

1. Проверка файлов проекта с помощью pytest.
2. Сборка Docker-образов backend, frontend и gateway.
3. Публикация образов в Docker Hub.

Используются следующие образы:

- `stiborg/kittygram_backend:latest`
- `stiborg/kittygram_frontend:latest`
- `stiborg/kittygram_gateway:latest`

Для работы GitHub Actions в настройках репозитория необходимо добавить Secrets:

| Название | Значение |
|---|---|
| `DOCKER_USERNAME` | Логин Docker Hub |
| `DOCKER_PASSWORD` | Access Token Docker Hub |

Файл workflow находится в `.github/workflows/main.yml`.

## Автоматическое тестирование

Для локального запуска тестов:

1. Создайте виртуальное окружение.
2. Установите зависимости:

```bash
pip install -r backend/requirements.txt
```

3. Запустите тесты:

```bash
pytest
```

Для проверок, связанных с развёртыванием, в корне проекта необходимо создать `tests.yml`:

```yaml
repo_owner: Stiborg-sudo
kittygram_domain: https://ваш-домен-киittygram
taski_domain: https://ваш-домен-taski
dockerhub_username: stiborg
```

Укажите реальные домены развёрнутых приложений. Без работающих доменов проверки доступности и подключения не пройдут.

Для автотестов Практикума также требуется файл `kittygram_workflow.yml` в корневой директории. Его содержимое должно соответствовать `.github/workflows/main.yml`.

## Автор

GitHub: [Stiborg-sudo](https://github.com/Stiborg-sudo)