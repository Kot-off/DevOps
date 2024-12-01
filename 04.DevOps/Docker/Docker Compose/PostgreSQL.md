#docker-compose

Создаём `docker-compose.yml` файла для запуска PostgreSQL и затем напишем `Makefile`, который будет содержать команды для управления контейнерами.

### 1. **docker-compose.yml**

Этот файл создаст контейнер PostgreSQL с базовыми настройками и окружением.

```yaml
version: '3.8'

services:
  db:
    image: postgres:13
    container_name: postgres-db
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"
    networks:
      - db-network
    restart: unless-stopped

volumes:
  postgres_data:

networks:
  db-network:
    driver: bridge
```

### Пояснение:

- **image**: Используется официальный образ PostgreSQL версии 13.
- **POSTGRES_USER, POSTGRES_PASSWORD, POSTGRES_DB**: Задаются переменные окружения для создания пользователя, пароля и базы данных при старте контейнера.
- **volumes**: Данные базы будут храниться в volume `postgres_data`, что позволяет сохранять данные между перезапусками контейнера.
- **ports**: Контейнер будет доступен на порту 5432 на вашей машине.
- **networks**: Для организации сетевого взаимодействия используется отдельная сеть `db-network`.
- **restart**: Контейнер будет перезапускаться, если он неожиданно завершит работу.

### 2. **Makefile**

Теперь создадим `Makefile`, который будет включать полезные команды для управления Docker контейнерами с PostgreSQL.

```makefile
# Название сервиса
SERVICE=db

# Образ и версия
IMAGE=postgres:13

# Основные команды Makefile
.PHONY: build up down restart logs ps exec shell clean

# Сборка Docker Compose (если нужно)
build:
	docker-compose build

# Запуск контейнеров в фоновом режиме
up:
	docker-compose up -d

# Остановка и удаление контейнеров, сетей, томов
down:
	docker-compose down

# Перезапуск контейнеров
restart:
	docker-compose restart

# Просмотр логов контейнера
logs:
	docker-compose logs -f $(SERVICE)

# Просмотр запущенных контейнеров
ps:
	docker-compose ps

# Открытие bash-сессии в контейнере PostgreSQL
exec:
	docker-compose exec $(SERVICE) bash

# Подключение к PostgreSQL через psql
shell:
	docker-compose exec $(SERVICE) psql -U user -d mydb

# Удаление volume с данными (внимание: удалит все данные)
clean:
	docker-compose down -v
```

### Пояснение команд в `Makefile`:

1. **build**: Строит контейнеры через `docker-compose build`. Обычно используется для обновлений образов.
2. **up**: Запускает контейнеры в фоновом режиме с помощью `docker-compose up -d`.
3. **down**: Останавливает контейнеры и удаляет все ресурсы, включая сеть и тома (но данные не удаляются).
4. **restart**: Перезапускает контейнеры через `docker-compose restart`.
5. **logs**: Показывает логи контейнера PostgreSQL в реальном времени.
6. **ps**: Показывает состояние всех контейнеров в проекте.
7. **exec**: Открывает Bash в контейнере PostgreSQL для работы с системой контейнера.
8. **shell**: Подключается к базе данных PostgreSQL через `psql` с использованием ранее заданных переменных окружения.
9. **clean**: Останавливает и удаляет контейнеры, тома и сети, включая удаление данных (будьте осторожны).

### Как использовать:

1. Сначала создайте файл `docker-compose.yml` и файл `Makefile` в одной директории.
2. Запустите контейнер PostgreSQL с помощью команды:
    
    ```bash
    make up
    ```
    
3. Для подключения к базе данных с использованием `psql`:
    
    ```bash
    make shell
    ```
    
4. Для просмотра логов:
    
    ```bash
    make logs
    ```
    
5. Для перезапуска контейнера:
    
    ```bash
    make restart
    ```
    
6. Если хотите очистить все контейнеры и тома (включая данные):
    
    ```bash
    make clean
    ```
    

Инструмент для запуска и управления контейнером PostgreSQL с помощью Docker Compose и Makefile!