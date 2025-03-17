# Инструкция по использованию Docker с проектом Strapi

## Описание
Этот репозиторий содержит готовые файлы `Dockerfile` и `docker-compose.yml` для развертывания проекта [Strapi](https://strapi.io/) в контейнере Docker.

## Файлы в репозитории
- `Dockerfile` – описывает сборку образа Strapi.
- `docker-compose.yml` – конфигурационный файл для управления контейнерами.
- `.env` - конфигурационный файл с переменными окружения.
## Требования
Перед началом убедитесь, что у вас установлены:
- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)
- Поддерживаемая версия Node.js
- Существующий проект Strapi 5 или новый
- (необязательно) Yarn, установленный на вашей машине
- (необязательно) Docker Compose установлен на вашей машине
## Описание `Dockerfile`
Файл `Dockerfile` содержит следующие ключевые шаги:

```dockerfile
# Используем официальный образ Node.js
FROM node:18-alpine

# Устанавливаем рабочую директорию
WORKDIR /app

# Копируем файлы package.json и package-lock.json
COPY package*.json ./

# Устанавливаем зависимости
RUN npm install

# Копируем остальные файлы проекта
COPY . .

# Открываем порт для Strapi
EXPOSE 1337

# Запускаем Strapi
CMD ["npm", "run", "develop"]
```

## Описание `docker-compose.yml`
Файл `docker-compose.yml` используется для запуска Strapi с базой данных PostgreSQL:

```yaml
version: '3.8'
services:
  strapi:
    build: .
    ports:
      - "1337:1337"
    environment:
      - NODE_ENV=development
      - DATABASE_CLIENT=postgres
      - DATABASE_HOST=db
      - DATABASE_PORT=5432
      - DATABASE_NAME=strapi
      - DATABASE_USERNAME=strapi
      - DATABASE_PASSWORD=strapi
    depends_on:
      - db
  
  db:
    image: postgres:13
    environment:
      POSTGRES_USER: strapi
      POSTGRES_PASSWORD: strapi
      POSTGRES_DB: strapi
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

## Как запустить контейнеры
### Сборка и запуск
Выполните команду:
```sh
docker-compose up -d
```

### Остановка контейнеров
```sh
docker-compose down
```

### Очистка данных
Если нужно удалить базу данных:
```sh
docker volume rm имя_тома
```

Теперь ваш проект Strapi готов к работе в Docker!

