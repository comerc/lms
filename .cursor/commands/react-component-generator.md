---
name: react-component-generator
description: Генерирует React компоненты с использованием Ant Design и Tailwind CSS. Используйте когда пользователь просит создать компонент, UI элемент, или интерфейс. Активируется при упоминании React, компонента, UI, интерфейса.
allowed-tools: []
---

# Генератор React компонентов

## Обзор

Этот навык генерирует React компоненты с правильной структурой, TypeScript типами, Ant Design компонентами и Tailwind CSS стилями.

## Инструкции

<instructions>
  * ВСЕГДА создавай функциональные компоненты с TypeScript
  * Используй Ant Design для сложных компонентов (Table, Form, Modal, Select, DatePicker)
  * Применяй Tailwind CSS для стилизации (utility-first)
  * Включай полные TypeScript интерфейсы для пропсов
  * Используй хуки (useState, useEffect, useCallback, useMemo) где необходимо
  * Следуй FSD архитектуре при определении места файла
  * Указывай точные классы Tailwind с размерами
  * Указывай конкретные иконки из @ant-design/icons
  * Включай обработку состояний загрузки и ошибок
</instructions>

## Структура компонента

```typescript
import { useState, useCallback } from 'react';
import { Button } from 'antd';
import { SearchOutlined } from '@ant-design/icons';

interface ComponentNameProps {
  // типизированные пропсы
}

export const ComponentName: React.FC<ComponentNameProps> = ({ ...props }) => {
  // хуки
  // обработчики
  // рендер
};
```

## Примеры

### Ввод пользователя:

"Создай компонент поиска с фильтром"

### Ожидаемый вывод:

```typescript
import { useState } from 'react';
import { Input, Button, Select } from 'antd';
import { SearchOutlined, FilterOutlined } from '@ant-design/icons';

interface SearchWithFilterProps {
  onSearch: (query: string, filter: string) => void;
  placeholder?: string;
}

export const SearchWithFilter: React.FC<SearchWithFilterProps> = ({
  onSearch,
  placeholder = 'Поиск...',
}) => {
  const [query, setQuery] = useState('');
  const [filter, setFilter] = useState('all');

  const handleSearch = () => {
    onSearch(query, filter);
  };

  return (
    <div className="flex gap-2 p-4 bg-white rounded-lg shadow-sm">
      <Input
        className="flex-1"
        placeholder={placeholder}
        value={query}
        onChange={(e) => setQuery(e.target.value)}
        prefix={<SearchOutlined className="text-gray-400" />}
        onPressEnter={handleSearch}
      />
      <Select
        className="w-32"
        value={filter}
        onChange={setFilter}
        suffixIcon={<FilterOutlined />}
      >
        <Select.Option value="all">Все</Select.Option>
        <Select.Option value="active">Активные</Select.Option>
      </Select>
      <Button type="primary" onClick={handleSearch} icon={<SearchOutlined />}>
        Найти
      </Button>
    </div>
  );
};
```
