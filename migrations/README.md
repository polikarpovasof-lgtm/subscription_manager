# Запуск базы данных

## Требования

- Установленный Docker Desktop
- Запущенный Docker Engine

Проверить установку:

docker --version
docker compose version

---

## Первый запуск

Перейти в корень проекта:

cd subscription_manager

Запустить PostgreSQL:

docker compose up -d

Проверить, что контейнер запущен:

docker ps

В списке должен появиться контейнер:

subscription_manager_db

---

## Подключение к базе данных

Подключиться к PostgreSQL внутри контейнера:

docker exec -it subscription_manager_db psql -U postgres -d subscription_manager

После подключения должна появиться строка:

subscription_manager=#

---

## Проверка таблиц

Выполнить:

\dt

Должны отображаться таблицы:

categories
notifications
subscriptions
users

---

## Проверка данных

Выполнить:

SELECT * FROM categories;

Должны отобразиться категории:

Музыка
Видео
AI
Игры
Образование
Другое

---

## Остановка базы данных

docker compose down

---

## Полная очистка базы данных

Удаляет контейнер и все сохраненные данные:

docker compose down -v

После повторного запуска:

docker compose up -d

PostgreSQL автоматически выполнит все миграции и создаст базу заново.

---

## Параметры подключения для Backend

Host: localhost
Port: 5432
Database: subscription_manager
User: postgres
Password: postgres

---

# Следующий этап разработки Backend

После успешного запуска базы данных необходимо реализовать Backend на Drogon.

Структура проекта:

backend/
├── controllers/
├── services/
├── repositories/
├── models/
├── config/
└── main.cc

---

## Этап 1. Подключение Drogon к PostgreSQL

Настроить подключение к БД и проверить его выполнением запроса:

SELECT * FROM categories;

из C++ приложения.

Если категории успешно считываются из PostgreSQL, значит соединение настроено корректно.

---

## Этап 2. Реализация Repository слоя

Создать UserRepository.

Методы:

createUser()
findByEmail()
findById()

Repository отвечает только за работу с базой данных и выполнение SQL-запросов.

---

## Этап 3. Реализация Service слоя

Создать AuthService.

Методы:

registerUser()
loginUser()

Service содержит бизнес-логику приложения.

---

## Этап 4. Реализация Controller слоя

Создать AuthController.

Endpoints:

POST /register
POST /login

Контроллер принимает HTTP-запросы и вызывает соответствующие сервисы.

---

# Текущий статус проекта

[✓] Docker
[✓] PostgreSQL
[✓] SQL миграции
[✓] Таблицы users
[✓] Таблицы categories
[✓] Таблицы subscriptions
[✓] Таблицы notifications

[ ] Backend на Drogon
[ ] Подключение к PostgreSQL из C++
[ ] Repository слой
[ ] Регистрация пользователей
[ ] Авторизация пользователей
[ ] JWT токены
[ ] CRUD подписок
[ ] Аналитический модуль
[ ] Уведомления
[ ] Frontend