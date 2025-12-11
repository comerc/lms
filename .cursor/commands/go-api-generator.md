---
name: go-api-generator
description: Генерирует Go API endpoints с использованием Chi router и sqlx. Используйте когда пользователь просит создать API endpoint, обработчик запросов, или бэкенд функциональность. Активируется при упоминании API, endpoint, handler, Go, бэкенд.
allowed-tools: []
---

# Генератор Go API endpoints

## Обзор

Этот навык генерирует Go API endpoints с правильной структурой, использованием Chi router, sqlx для работы с БД, и стандартными Go практиками.

## Инструкции

<instructions>
  * ВСЕГДА используй Chi router для маршрутизации
  * Используй sqlx для работы с PostgreSQL
  * Следуй стандартному Go стилю кода
  * Включай обработку ошибок
  * Используй правильные HTTP методы и статус коды
  * Включай валидацию входных данных
  * Структурируй код по handlers, models, services
</instructions>

## Структура API endpoint

```go
package handlers

import (
    "encoding/json"
    "net/http"
    "github.com/go-chi/chi/v5"
    "github.com/jmoiron/sqlx"
)

type HandlerName struct {
    db *sqlx.DB
}

func (h *HandlerName) MethodName(w http.ResponseWriter, r *http.Request) {
    // валидация
    // бизнес-логика
    // ответ
}
```

## Примеры

### Ввод пользователя:

"Создай API endpoint для получения списка пользователей"

### Ожидаемый вывод:

```go
package handlers

import (
    "encoding/json"
    "net/http"
    "github.com/go-chi/chi/v5"
    "github.com/jmoiron/sqlx"
)

type UserHandler struct {
    db *sqlx.DB
}

type User struct {
    ID       int    `json:"id" db:"id"`
    Username string `json:"username" db:"username"`
    Level    int    `json:"level" db:"level"`
    XP       int    `json:"xp" db:"xp"`
}

func (h *UserHandler) GetUsers(w http.ResponseWriter, r *http.Request) {
    var users []User

    query := `SELECT id, username, level, xp FROM users ORDER BY xp DESC LIMIT 100`
    err := h.db.Select(&users, query)
    if err != nil {
        http.Error(w, "Failed to fetch users", http.StatusInternalServerError)
        return
    }

    w.Header().Set("Content-Type", "application/json")
    json.NewEncoder(w).Encode(users)
}

func RegisterUserRoutes(r chi.Router, db *sqlx.DB) {
    handler := &UserHandler{db: db}
    r.Get("/api/users", handler.GetUsers)
}
```
