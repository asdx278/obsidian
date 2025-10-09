---
class: note
area: professional
tags:
  - cursor
  - ai
created: 2025-10-09
---
**MOC: [[MOC - Professional]]**

# Cursor - AGENTS.md

> [!tldr] ai review
> 

## Введение

AGENTS.md - это файл markdown для инструкций агентам.

Должен находиться в корне проекта, как альтернатива `.cursor/rules` для прямых инструкций.

Идеально подходит для простых, легких правил, но не имеет всех возможностей Project Rules.

## Примеры правил

```markdown
# Инструкции по проекту

## Стиль кода
- Используй TypeScript для всех новых файлов
- Предпочитай функциональные компоненты в React
- Используй snake_case для столбцов в базе данных

## Архитектура
- Следуй паттерну репозитория
- Размещай бизнес-логику в сервисных слоях
```

### Additional materials

https://docs.cursor.com/ru/context/rules#dobavlenie-novogo-parametra-nastroyki-v-cursor