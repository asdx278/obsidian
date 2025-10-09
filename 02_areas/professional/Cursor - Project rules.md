---
class: note
area: professional
tags:
  - cursor
  - ai
created: 2025-10-09
---
**MOC: [[MOC - Professional]]**

# Cursor - Project rules

> [!tldr] ai review
> 

## Введение

Правила проекта находятся в `.cursor/rules` в корне проекта или могут быть вложенными:

```
project/
  .cursor/rules/             # Общие правила проекта
  backend/
    server/
      .cursor/rules/    # Правила для backend'а
  frontend/
    .cursor/rules/      # Правила для frontend'а
```

Каждое правило - отдельный файл под контролем версий.

Каждое правило можно ограничивать по шаблонам путей, подключать вручную или по степени релевантности. Вложенные директории могут содержать собственный `.cursor/rules`, который применяется в пределах этой директории.

Правила проекта нужны для: 
- стандартизации проекта по стилю, архитектуре, используемым технологиям
- автоматизации рабочих процессов, шаблонов проекта
- документирования знаний о кодовой базе

## Рекомендации для правил

- Правило должно быть менее 500 строк
- Крупные правила разбивать на более мелкие
- Добавлять в правила примеры, ссылки на файлы
- Правило должно быть конкретным, понятным
- Переиспользовать правила при подсказках в чате

## Синтаксис правил

Каждый файл правил - это отдельный файл формата `.mdc`, которые поддерживает метаданные и содержимое.

Правило может применяться по разному, для этого есть настройка типа:
- Always - всегда применяется. Для базовых правил проекта.
- Auto attached - включается, когда упоминаются файлы, которые попадают под шаблон указанный в glob. Для правил, специфичных для типов файлов
- Agent requested - это правило НЕ применяется автоматически, а только когда AI или пользователь явно запросит его по описанию. Правила с description полезны для специализированной информации, которая нужна не всегда, но может быть полезна в определенных ситуациях.
- Manual - применяется только тогда, когда явно упоминается через символ `@`.

## Комбинирование правил

Правила можно комбинировать, но лучше создавать отдельные.
Возможные комбинации: alwaysApply + description, globs + description.
**Избегать комбинации - always + globs!**

## Примеры правил

### Ex. 1 - все три блока. Допустимо, но избыточно.

```mdc
---
description: Заготовка сервиса RPC
globs:
alwaysApply: false
---

- Используй наш внутренний RPC-паттерн при определении сервисов
- Всегда используй snake_case для названий сервисов.
```

### Ex. 2 - alwaysApply: true

- Описывает структуру проекта
- Указывает основные файлы и их назначение
- Контекст образовательного проекта

```mdc
---
alwaysApply: true
---

# Project Structure Guide

This is a Python game project "Alien Invasion" built with Pygame for learning purposes.

## Main Files:

- [alien_invasion.py](mdc:alien_invasion.py) - Main game class and entry point
- [main.py](mdc:main.py) - Simple main function (currently just a placeholder)
- [pyproject.toml](mdc:pyproject.toml) - Project configuration and dependencies

## Project Context:

- Educational project based on "Изучаем Python" book by Мэтиз Э.
- Uses Pygame for game development
- Python 3.12+ required
- Currently in early development stage with basic game loop implemented
```

### Ex. 3 - globs: *.py

- Стандарты кодирования Python
- Конвенции Pygame
- Паттерны разработки игр

```mdc
---
globs: *.py
---

# Python Coding Standards for Alien Invasion

## Code Style:

- Use descriptive variable and function names in English
- Follow PEP 8 style guidelines
- Use docstrings for classes and methods
- Keep functions focused and single-purpose

## Pygame Conventions:

- Initialize pygame with `pygame.init()`
- Use `pygame.time.Clock()` for frame rate control
- Handle events in the main game loop
- Use `pygame.display.flip()` to update the screen
- Always call `pygame.quit()` when exiting

## Game Development Patterns:

- Separate game logic from rendering
- Use classes to organize game objects
- Implement proper event handling
- Use constants for game settings (screen size, colors, etc.)
```


### Ex. 4 - description

- Текущее состояние игры
- Следующие шаги разработки
- Лучшие практики Pygame

```mdc
---
description: Game development patterns and Pygame best practices
---

# Game Development Guidelines

## Current Game State:

The game currently has:
- Basic game loop in [alien_invasion.py](mdc:alien_invasion.py)
- Screen setup (1200x800 pixels)
- Event handling for window closing
- 60 FPS frame rate

## Next Development Steps:

1. Create ship class for player
2. Add alien sprites
3. Implement bullet system
4. Add collision detection
5. Create game states (menu, playing, game over)
6. Add scoring system

## Pygame Best Practices:

- Use sprite groups for managing multiple objects
- Implement proper game state management
- Use vector math for movement calculations
- Optimize rendering with dirty rectangle updates
- Handle input with proper key states
```

### Ex. 5 - автоматизация рабочих процессов разработки и генерация документации

```
Это правило автоматизирует анализ приложения:Когда просят проанализировать приложение:

1. Запусти dev‑сервер командой `npm run dev`
2. Собери логи из консоли
3. Предложи оптимизации производительности

Это правило помогает генерировать документацию:Помоги подготовить документацию, выполняя:

- Извлечение комментариев из кода
- Анализ README.md
- Генерацию документации в Markdown
```

## Создание и генерация правил

Правила можно сгенерировать командой - `/Generate Cursor Rules`

Создать правило можно с помощью `New Cursor Rule` или `Cursor Settings > Rules`.

### Additional materials

https://docs.cursor.com/ru/context/rules#dobavlenie-novogo-parametra-nastroyki-v-cursor