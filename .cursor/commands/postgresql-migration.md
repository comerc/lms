---
name: postgresql-migration
description: Генерирует миграции PostgreSQL для базы данных. Используйте когда пользователь просит создать миграцию, изменить схему БД, или добавить таблицу/колонку. Активируется при упоминании миграции, базы данных, PostgreSQL, схемы БД.
allowed-tools: []
---

# Генератор миграций PostgreSQL

## Обзор

Этот навык генерирует миграции PostgreSQL с использованием sqlx для Go бэкенда.

## Инструкции

<instructions>
  * ВСЕГДА создавай миграции в формате SQL
  * ВСЕГДА используй транзакции для безопасности
  * ВСЕГДА включай откат миграции (down migration)
  * ВСЕГДА добавляй индексы для часто используемых полей
  * ВСЕГДА учитывай внешние ключи и ограничения
  * ВСЕГДА используй правильные типы данных PostgreSQL
  * ВСЕГДА добавляй комментарии к таблицам и колонкам
</instructions>

## Структура миграции

```sql
-- +goose Up
-- SQL in this section is executed when the migration is applied.

-- +goose Down
-- SQL in this section is executed when the migration is rolled back.
```

## Примеры миграций

### Создание таблицы пользователей:

```sql
-- +goose Up
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    level INTEGER DEFAULT 1 NOT NULL,
    xp INTEGER DEFAULT 0 NOT NULL,
    streak INTEGER DEFAULT 0 NOT NULL,
    is_premium BOOLEAN DEFAULT FALSE NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW() NOT NULL,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW() NOT NULL
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_username ON users(username);
CREATE INDEX idx_users_level ON users(level);
CREATE INDEX idx_users_xp ON users(xp DESC);

COMMENT ON TABLE users IS 'Таблица пользователей платформы';
COMMENT ON COLUMN users.level IS 'Текущий уровень пользователя';
COMMENT ON COLUMN users.xp IS 'Количество опыта пользователя';
COMMENT ON COLUMN users.streak IS 'Текущая серия дней подряд';

-- +goose Down
DROP INDEX IF EXISTS idx_users_xp;
DROP INDEX IF EXISTS idx_users_level;
DROP INDEX IF EXISTS idx_users_username;
DROP INDEX IF EXISTS idx_users_email;
DROP TABLE IF EXISTS users;
```

### Добавление колонки:

```sql
-- +goose Up
ALTER TABLE users
ADD COLUMN last_active_at TIMESTAMP WITH TIME ZONE;

CREATE INDEX idx_users_last_active ON users(last_active_at DESC);

COMMENT ON COLUMN users.last_active_at IS 'Время последней активности пользователя';

-- +goose Down
DROP INDEX IF EXISTS idx_users_last_active;
ALTER TABLE users DROP COLUMN IF EXISTS last_active_at;
```

### Создание таблицы с внешними ключами:

```sql
-- +goose Up
CREATE TABLE tasks (
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    title VARCHAR(200) NOT NULL,
    description TEXT,
    subject VARCHAR(50) NOT NULL,
    difficulty VARCHAR(20) NOT NULL CHECK (difficulty IN ('easy', 'medium', 'hard')),
    status VARCHAR(20) NOT NULL DEFAULT 'pending' CHECK (status IN ('pending', 'in_progress', 'completed', 'skipped')),
    xp_reward INTEGER DEFAULT 10 NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW() NOT NULL,
    completed_at TIMESTAMP WITH TIME ZONE,
    CONSTRAINT tasks_user_id_fkey FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

CREATE INDEX idx_tasks_user_id ON tasks(user_id);
CREATE INDEX idx_tasks_status ON tasks(status);
CREATE INDEX idx_tasks_subject ON tasks(subject);
CREATE INDEX idx_tasks_created_at ON tasks(created_at DESC);

COMMENT ON TABLE tasks IS 'Таблица задач для пользователей';
COMMENT ON COLUMN tasks.xp_reward IS 'Количество XP за выполнение задачи';

-- +goose Down
DROP INDEX IF EXISTS idx_tasks_created_at;
DROP INDEX IF EXISTS idx_tasks_subject;
DROP INDEX IF EXISTS idx_tasks_status;
DROP INDEX IF EXISTS idx_tasks_user_id;
DROP TABLE IF EXISTS tasks;
```

## Организация миграций

```
migrations/
├── 000001_create_users_table.up.sql
├── 000001_create_users_table.down.sql
├── 000002_create_tasks_table.up.sql
├── 000002_create_tasks_table.down.sql
├── 000003_add_last_active_to_users.up.sql
└── 000003_add_last_active_to_users.down.sql
```

## Примеры

### Ввод пользователя:

"Создай миграцию для таблицы задач"

### Ожидаемый вывод:

Полная миграция с:

- CREATE TABLE с правильными типами
- Индексами для часто используемых полей
- Внешними ключами (если нужно)
- CHECK ограничениями
- Комментариями к таблице и колонкам
- Down миграцией для отката
