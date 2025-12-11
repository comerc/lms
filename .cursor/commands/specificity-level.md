---
name: specificity-level
description: Обеспечивает максимальный уровень детализации (10/10) при создании компонентов и интерфейсов. Используйте при создании UI компонентов, дизайн-спецификаций, или когда требуется детальное описание интерфейса. Активируется автоматически при работе с UI/UX задачами.
allowed-tools: []
---

# Уровень детализации (10/10)

## Обзор

Этот навык обеспечивает максимальную детализацию при создании компонентов и интерфейсов, чтобы разработчик мог реализовать их без дополнительных вопросов.

## Инструкции

<instructions>
  * ВСЕГДА указывай шестнадцатеричные коды для каждого цвета с контекстом использования
  * ВСЕГДА указывай точные классы Tailwind с размерами (text-sm, p-4, w-80, rounded-lg)
  * ВСЕГДА указывай конкретные иконки из @ant-design/icons (SearchOutlined, FilterOutlined, ClockCircleOutlined, DollarOutlined)
  * ВСЕГДА создавай ASCII диаграммы для макетов компонентов
  * ВСЕГДА указывай точные размеры, отступы и spacing
  * НИКОГДА не оставляй стили на усмотрение разработчика
</instructions>

## Требования к детализации

### Цвета

Всегда указывай hex-коды с контекстом:

- Primary: `#1890ff` (основной цвет кнопок, ссылок)
- Success: `#52c41a` (успешные действия)
- Warning: `#faad14` (предупреждения)
- Error: `#ff4d4f` (ошибки)
- Background: `#f0f0f0` (фон страницы)
- Card background: `#ffffff` (фон карточек)
- Text primary: `#262626` (основной текст)
- Text secondary: `#8c8c8c` (вторичный текст)

### Tailwind классы

Всегда указывай полные классы с размерами:

- `text-sm` (14px), `text-base` (16px), `text-lg` (18px)
- `p-2` (8px), `p-4` (16px), `p-6` (24px)
- `w-80` (320px), `w-full`, `w-1/2`
- `rounded-lg` (8px), `rounded-xl` (12px)
- `shadow-sm`, `shadow-md`, `shadow-lg`

### Иконки

Всегда указывай конкретные иконки из @ant-design/icons:

- `SearchOutlined` для поиска
- `FilterOutlined` для фильтров
- `ClockCircleOutlined` для времени
- `DollarOutlined` для финансов
- `UserOutlined` для пользователя
- `TrophyOutlined` для достижений

### ASCII диаграммы

Создавай визуальные схемы макетов:

```
┌─────────────────────────────────────┐
│  Header (h-16, bg-white, shadow)   │
├─────────────────────────────────────┤
│  Sidebar │  Main Content            │
│  (w-64)  │  (flex-1, p-6)          │
│          │  ┌───────────────────┐   │
│          │  │ Card Component    │   │
│          │  │ (p-4, rounded-lg) │   │
│          │  └───────────────────┘   │
└─────────────────────────────────────┘
```

## Примеры

### Ввод пользователя:

"Создай карточку пользователя"

### Ожидаемый вывод:

```typescript
// Цвета:
// - Фон карточки: #ffffff
// - Текст заголовка: #262626
// - Текст описания: #8c8c8c
// - Кнопка primary: #1890ff

// Размеры:
// - Padding: p-4 (16px)
// - Border radius: rounded-lg (8px)
// - Ширина: w-80 (320px)
// - Тень: shadow-md

<div className="w-80 p-4 bg-white rounded-lg shadow-md">
  <h3 className="text-lg font-semibold text-gray-800">Иван Иванов</h3>
  <p className="text-sm text-gray-500 mt-2">Уровень 15 • 2500 XP</p>
  <Button
    type="primary"
    className="mt-4"
    style={{ backgroundColor: '#1890ff' }}
  >
    Профиль
  </Button>
</div>
```
