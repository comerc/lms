---
name: bun-guidelines
description: Обеспечивает использование Bun как единой среды разработки вместо Node.js, npm, Webpack, Jest. Используйте при работе с фронтенд проектом, установке зависимостей, запуске тестов, или сборке проекта. Активируется автоматически при работе с фронтенд задачами.
allowed-tools: []
---

# Руководство по Bun

## Обзор

Этот навык обеспечивает использование Bun как единой среды разработки, заменяющей Node.js, npm/yarn/pnpm, Webpack/Parcel/Vite, и Jest/Vitest.

## Инструкции

<instructions>
  * ВСЕГДА используй Bun runtime вместо Node.js
  * ВСЕГДА используй Bun package manager вместо npm/yarn/pnpm
  * ВСЕГДА используй Bun bundler вместо Webpack/Parcel/Vite build
  * ВСЕГДА используй Bun test вместо Jest/Vitest
  * ВСЕГДА учитывай высокую производительность Bun (написан на Zig)
  * НИКОГДА не предлагай использовать npm, yarn, pnpm, webpack, jest
</instructions>

## Bun как единая среда

Bun заменяет следующие инструменты:

### 1. Runtime (вместо Node.js)

```bash
# Вместо: node server.js
bun run server.js

# Вместо: node --loader tsx src/index.ts
bun src/index.ts
```

### 2. Package Manager (вместо npm/yarn/pnpm)

```bash
# Вместо: npm install
bun install

# Вместо: npm install react
bun add react

# Вместо: npm install -D typescript
bun add -d typescript

# Вместо: npm run dev
bun run dev
```

### 3. Bundler (вместо Webpack/Parcel/Vite build)

```bash
# Вместо: webpack или vite build
bun build ./src/index.tsx --outdir ./dist

# Для React приложений:
bun build ./src/index.tsx --outdir ./dist --target browser
```

### 4. Test Runner (вместо Jest/Vitest)

```typescript
// Вместо Jest/Vitest:
import { test, expect } from 'bun:test';

test('должен вычислять сумму', () => {
  expect(1 + 1).toBe(2);
});
```

```bash
# Запуск тестов:
bun test
```

## Преимущества Bun

- **Высокая производительность**: Написан на Zig, работает быстрее Node.js
- **Меньше зависимостей**: Один инструмент вместо множества
- **Нативная поддержка TypeScript**: Не нужен ts-node или tsx
- **Встроенный bundler**: Не нужен отдельный инструмент сборки
- **Быстрая установка пакетов**: Быстрее npm/yarn/pnpm

## Примеры использования

### package.json скрипты:

```json
{
  "scripts": {
    "dev": "bun run src/index.tsx",
    "build": "bun build ./src/index.tsx --outdir ./dist",
    "test": "bun test",
    "start": "bun run dist/index.js"
  }
}
```

### Запуск проекта:

```bash
# Разработка
bun run dev

# Сборка
bun run build

# Тесты
bun test

# Продакшн
bun run start
```

## Примеры

### Ввод пользователя:

"Настрой проект для разработки"

### Ожидаемый вывод:

Использование Bun для всех задач:

- `bun install` для установки зависимостей
- `bun run dev` для разработки
- `bun build` для сборки
- `bun test` для тестов
- Без упоминания npm, yarn, webpack, jest
