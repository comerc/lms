---
name: additional-requirements
description: Обеспечивает учет дополнительных требований при создании интерфейсов: горячие клавиши, скелетоны загрузки, детали развертывания, адаптивность. Используйте при создании UI компонентов или страниц. Активируется автоматически при работе с интерфейсами.
allowed-tools: []
---

# Дополнительные требования

## Обзор

Этот навык обеспечивает учет всех дополнительных требований для полноценного пользовательского интерфейса.

## Инструкции

<instructions>
  * ВСЕГДА добавляй горячие клавиши для опытных пользователей
  * ВСЕГДА создавай скелетоны загрузки для всех асинхронных операций
  * ВСЕГДА указывай детали развертывания (self-hosted или cloud)
  * ВСЕГДА учитывай адаптивность (mobile-first подход)
  * НИКОГДА не забывай про состояния загрузки
</instructions>

## Горячие клавиши

Для опытных пользователей, работающих с большими списками:

```typescript
import { useEffect } from 'react';

const useKeyboardNavigation = (
  items: Item[],
  onSelect: (item: Item) => void,
) => {
  useEffect(() => {
    const handleKeyPress = (e: KeyboardEvent) => {
      if (e.key === 'j' || e.key === 'ArrowDown') {
        // Навигация вниз
        e.preventDefault();
      } else if (e.key === 'k' || e.key === 'ArrowUp') {
        // Навигация вверх
        e.preventDefault();
      } else if (e.key === 'Enter') {
        // Открыть выбранный элемент
        e.preventDefault();
      } else if (e.key === 'Escape') {
        // Закрыть модальное окно
        e.preventDefault();
      }
    };

    window.addEventListener('keydown', handleKeyPress);
    return () => window.removeEventListener('keydown', handleKeyPress);
  }, [items, onSelect]);
};

// Горячие клавиши:
// j / ↓ - следующая задача
// k / ↑ - предыдущая задача
// Enter - открыть задачу
// Esc - закрыть модальное окно
// / - фокус на поиск
```

## Скелетоны загрузки

Для всех асинхронных операций:

```typescript
import { Skeleton } from 'antd';

const TaskListSkeleton = () => (
  <div className="space-y-4">
    {[1, 2, 3, 4, 5].map((i) => (
      <div key={i} className="p-4 bg-white rounded-lg shadow-sm">
        <Skeleton active paragraph={{ rows: 2 }} />
      </div>
    ))}
  </div>
);

// Использование:
{
  isLoading ? <TaskListSkeleton /> : <TaskList tasks={tasks} />;
}
```

## Детали развертывания

Всегда указывай в спецификации:

```markdown
## Развертывание

**Backend (Go):**

- Self-hosted: Docker контейнер на сервере
- Или: Cloud (AWS ECS, Google Cloud Run)
- База данных: PostgreSQL (self-hosted или managed service)
- Переменные окружения: .env файл с настройками

**Frontend (React):**

- Build: `bun run build`
- Hosting: Vercel, Netlify, или статический хостинг
- CDN: Cloudflare для статических ресурсов

**Meilisearch:**

- Self-hosted: Docker контейнер
- Или: Meilisearch Cloud (managed service)
- Конфигурация: master key, порт 7700
```

## Адаптивность

Mobile-first подход с breakpoints:

```typescript
// Tailwind breakpoints:
// sm: 640px
// md: 768px
// lg: 1024px
// xl: 1280px
// 2xl: 1536px

// Пример адаптивного компонента:
<div className="
  w-full          // mobile: 100%
  md:w-1/2        // tablet: 50%
  lg:w-1/3        // desktop: 33%
  xl:w-1/4        // large: 25%
  p-2             // mobile: 8px
  md:p-4           // tablet+: 16px
  lg:p-6           // desktop+: 24px
">
  {/* контент */}
</div>

// Скрытие элементов на мобильных:
<div className="hidden md:block">
  {/* видно только на tablet+ */}
</div>

// Адаптивная навигация:
<Menu
  mode={isMobile ? 'vertical' : 'horizontal'}
  className="w-full md:w-auto"
/>
```

## Примеры

### Ввод пользователя:

"Создай страницу со списком задач"

### Ожидаемый вывод:

Спецификация включает:

- Горячие клавиши (j/k для навигации, Enter для открытия)
- Скелетоны загрузки для начальной загрузки и фильтрации
- Детали развертывания (где и как деплоить)
- Адаптивный дизайн (mobile-first, breakpoints)
