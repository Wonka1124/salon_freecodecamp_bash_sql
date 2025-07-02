# Bash-скрипт "My Salon"

Консольное приложение на Bash для записи клиентов в салон красоты с использованием базы данных PostgreSQL.

## Возможности

- Просмотр списка услуг
- Запись клиента на услугу по телефону и имени
- Сохранение информации о клиентах, услугах и записях в базе данных
- Проверка существования клиента по номеру телефона
- Подтверждение записи с указанием времени

## Требования

- Bash
- PostgreSQL
- Утилита psql
- Пользователь и база данных PostgreSQL:
  - Пользователь: freecodecamp
  - База данных: salon

## Структура базы данных

В базе данных `salon` используются три таблицы:

- **services** — услуги
  - `service_id` SERIAL PRIMARY KEY
  - `name` VARCHAR NOT NULL

- **customers** — клиенты
  - `customer_id` SERIAL PRIMARY KEY
  - `name` VARCHAR NOT NULL
  - `phone` VARCHAR(15) NOT NULL UNIQUE

- **appointments** — записи
  - `appointment_id` SERIAL PRIMARY KEY
  - `customer_id` INT REFERENCES customers(customer_id)
  - `service_id` INT REFERENCES services(service_id)
  - `time` VARCHAR NOT NULL

## Инициализация базы данных

Пример SQL-скрипта для создания базы и таблиц:

```sql
CREATE DATABASE salon;
\c salon

CREATE TABLE services (
  service_id SERIAL PRIMARY KEY,
  name VARCHAR NOT NULL
);

CREATE TABLE customers (
  customer_id SERIAL PRIMARY KEY,
  name VARCHAR NOT NULL,
  phone VARCHAR(15) NOT NULL UNIQUE
);

CREATE TABLE appointments (
  appointment_id SERIAL PRIMARY KEY,
  customer_id INT REFERENCES customers(customer_id),
  service_id INT REFERENCES services(service_id),
  time VARCHAR NOT NULL
);
```

Для наполнения таблицы услуг:
```sql
INSERT INTO services (name) VALUES ('cut'), ('color'), ('perm'), ('style'), ('trim');
```

## Запуск скрипта

1. Убедитесь, что база данных и таблицы созданы, а пользователь `freecodecamp` имеет доступ к базе `salon`.
2. Запустите скрипт:
   ```bash
   bash имя_вашего_скрипта.sh
   ```
3. Следуйте инструкциям в терминале: выберите услугу, введите телефон, имя (если новый клиент), и время записи.

## Примечания

- Скрипт экранирует ввод пользователя для безопасности SQL-запросов.
- Для работы требуется доступ к psql без пароля (например, через .pgpass или peer-авторизацию).
- Для запуска в другой среде измените параметры подключения в переменной `PSQL` в начале скрипта. 