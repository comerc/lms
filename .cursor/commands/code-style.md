---
name: code-style
description: Определяет стандарты стиля кода для проекта. Используйте при написании React компонентов, TypeScript кода, Go кода или при работе со стилями. Активируется автоматически при генерации любого кода.
allowed-tools: []
---

# Стандарты стиля кода

## Обзор

Этот навык определяет правила стиля кода для всех языков и технологий, используемых в проекте.

## Инструкции

<instructions>
  * ВСЕГДА следуй этим стандартам при написании кода
  * При генерации компонентов используй функциональные компоненты и хуки
  * Используй TypeScript strict mode
  * Соблюдай правила именования
  * Применяй Tailwind utility-first подход
  * Используй Ant Design для сложных компонентов
</instructions>

## Стандарты React и TypeScript

**Компоненты:**

- Функциональные компоненты с хуками
- TypeScript strict mode обязателен
- Именование: `camelCase` для переменных и функций
- Именование: `PascalCase` для компонентов

**Стилизация:**

- Tailwind CSS utility-first подход
- Ant Design для сложных компонентов (таблицы, формы, модальные окна)
- Избегай inline стилей, используй классы Tailwind

## Стандарты Go

**Стиль:**

- Стандартный Go стиль кода (gofmt)
- Chi router для маршрутизации
- sqlx для работы с базой данных PostgreSQL
- Следуй Go conventions для именования

## Примеры

### Правильно (React):

```typescript
import { useState } from 'react';
import { Button } from 'antd';

interface UserCardProps {
  userName: string;
  userLevel: number;
}

export const UserCard: React.FC<UserCardProps> = ({ userName, userLevel }) => {
  const [isExpanded, setIsExpanded] = useState(false);

  return (
    <div className="p-4 rounded-lg bg-white shadow-md">
      <h3 className="text-lg font-semibold">{userName}</h3>
      <p className="text-sm text-gray-600">Уровень: {userLevel}</p>
      <Button onClick={() => setIsExpanded(!isExpanded)}>
        {isExpanded ? 'Свернуть' : 'Развернуть'}
      </Button>
    </div>
  );
};
```

### Правильно (Go):

```go
package handlers

import (
    "net/http"
    "github.com/go-chi/chi/v5"
)

type UserHandler struct {
    db *sqlx.DB
}

func (h *UserHandler) GetUser(w http.ResponseWriter, r *http.Request) {
    // обработка запроса
}
```
