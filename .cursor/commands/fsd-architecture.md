---
name: fsd-architecture
description: Обеспечивает соблюдение Feature-Sliced Design архитектуры. Используйте при создании новых файлов, компонентов, или при организации структуры проекта. Активируется автоматически при работе с файловой структурой проекта.
allowed-tools: []
---

# Архитектура Feature-Sliced Design

## Обзор

Этот навык обеспечивает правильную организацию кода по принципам Feature-Sliced Design (FSD).

## Инструкции

<instructions>
  * ВСЕГДА сопоставляй каждый файл с уровнями FSD (app, pages, widgets, features, entities, shared)
  * ВСЕГДА соблюдай шаблон публичного API (index.ts для экспортов)
  * ВСЕГДА соблюдай четкие границы функций и модулей
  * ВСЕГДА правильно организуй shared слой (ui, lib, api, config)
  * НИКОГДА не нарушай правила импортов между слоями
</instructions>

## Структура FSD

```
src/
├── app/                    # Инициализация приложения
│   ├── providers/          # Провайдеры (Router, Theme, Store)
│   └── index.tsx          # Точка входа
│
├── pages/                  # Страницы приложения
│   ├── tasks/             # Страница задач
│   │   ├── index.tsx
│   │   └── TasksPage.tsx
│   └── profile/            # Страница профиля
│
├── widgets/               # Крупные составные блоки
│   ├── header/            # Шапка сайта
│   ├── sidebar/           # Боковая панель
│   └── task-list/         # Список задач
│
├── features/              # Бизнес-функции
│   ├── task-search/       # Поиск задач
│   ├── task-filter/       # Фильтрация задач
│   └── user-level-up/     # Повышение уровня
│
├── entities/              # Бизнес-сущности
│   ├── task/              # Сущность задачи
│   │   ├── model/         # Типы, интерфейсы
│   │   ├── api/           # API запросы
│   │   └── ui/            # UI компоненты
│   └── user/              # Сущность пользователя
│
└── shared/                # Переиспользуемый код
    ├── ui/                # UI компоненты (Button, Input)
    ├── lib/               # Утилиты (helpers, validators)
    ├── api/               # Базовые API настройки
    └── config/            # Конфигурация
```

## Правила импортов

**Разрешенные импорты:**

- `app` → может импортировать из всех слоев
- `pages` → может импортировать из `widgets`, `features`, `entities`, `shared`
- `widgets` → может импортировать из `features`, `entities`, `shared`
- `features` → может импортировать из `entities`, `shared`
- `entities` → может импортировать только из `shared`
- `shared` → не может импортировать из других слоев

**Запрещенные импорты:**

- ❌ `features` → `widgets` (обратная зависимость)
- ❌ `entities` → `features` (обратная зависимость)
- ❌ `shared` → любой другой слой

## Публичный API

Каждый модуль должен иметь `index.ts` для экспорта:

```typescript
// entities/task/index.ts
export { TaskCard } from './ui/TaskCard';
export { TaskList } from './ui/TaskList';
export type { Task } from './model/types';
export { useTask } from './model/useTask';
export { taskApi } from './api/taskApi';
```

## Примеры

### Ввод пользователя:

"Создай компонент карточки задачи"

### Ожидаемый вывод:

```
Файл должен быть создан в: entities/task/ui/TaskCard.tsx

Структура:
entities/task/
├── ui/
│   ├── TaskCard.tsx      # Компонент карточки
│   └── TaskList.tsx      # Список карточек
├── model/
│   ├── types.ts          # Типы Task
│   └── useTask.ts        # Хуки для работы с задачей
├── api/
│   └── taskApi.ts        # API запросы
└── index.ts              # Публичный API

Импорты в TaskCard:
- Можно: shared/ui, shared/lib
- Нельзя: features, widgets, pages
```
