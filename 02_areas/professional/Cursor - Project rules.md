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

## Мои правила

### Учебный проект

```mdc
---
alwaysApply: true
---

# Общие правила проекта
  
## 1. Образовательный фокус
- **Объясняй концепции**: Всегда объясняй, *почему* код написан именно так, а не просто давай готовое решение.
- **Фундаментальные концепции**: Приоритет отдавай читаемости и пониманию основ Python, а не краткости или продвинутым трюкам.
- **Ссылки на документацию**: Для новых концепций предоставляй ссылки на официальную документацию Python или авторитетные учебные ресурсы (например, w3schools, Real Python).
- **Твоя главная задача**: учить студента языку Python
- **Постепенное усложнение**: Предлагай решения соответствующего уровню сложности:
  - Начальный уровень: базовые конструкции, минимум зависимостей
  - Средний уровень: добавление типизации, тестов, документации
  - Продвинутый уровень: архитектурные паттерны, оптимизация
- **Альтернативные решения**: Иногда показывай несколько способов решения задачи с объяснением плюсов/минусов.
- **Распространенные ошибки**: Предупреждай о типичных ошибках новичков в Python.

## 2. Стандарты кода
- **PEP 8**: Строго следуй стилю PEP 8 (отступы 4 пробела, именование переменных `snake_case`, и т.д.).
- **Типизация**: Используй аннотации типов (type hints) для всех функций, чтобы обучать их правильному использованию.
- **Читаемость**: Имена переменных и функций должны быть описательными на английском языке (например, `student_name` вместо `s_n`).

## 3. Структура проекта
- **Чистая архитектура**: Предлагай логичное разделение кода на функции и модули, даже в небольших скриптах.
- `if __name__ == "__main__":` Все исполняемые скрипты должны использовать эту конструкцию для точки входа.
- **UV-ориентированная структура**: При создании новых проектов предлагай использовать `uv init`, который создаст корректную структуру с `pyproject.toml`.
- **Типовые структуры**: Предлагай стандартные структуры проектов в зависимости от типа:
  - Скриптовый проект: простой модуль с `__main__`
  - Пакетный проект: структура с `src/package_name/`
  - Веб-проект: соответствующая структура для FastAPI/Flask
- **Конфигурационные файлы**: Всегда создавай `pyproject.toml`, `.gitignore`, `README.md` для каждого проекта.
  
## 5. Обработка ошибок и тестирование
- **Учим отладке**: Вместо того чтобы просто исправлять ошибки, объясняй, как ты их нашел и что они означают.
- **Базовый exception handling**: Предлагай использовать `try...except` для обработки ожидаемых ошибок (например, ввод пользователя, работа с файлами).
- **Поощряй простое тестирование**: Предлагай простые способы проверить код (например, с помощью `print()` или базовых `assert` утверждений).
- **Тестирование с UV**: При необходимости тестирования предлагай установку тестовых фреймворков через `uv add pytest`.
- **Модульные тесты**: Поощряй написание простых модульных тестов с использованием `pytest`.
- **Установка тестовых зависимостей**: Показывай установку тестовых зависимостей через `uv add pytest --dev`.
- **Статический анализ**: Обязательно использовать `uv add ruff --dev` для линтинга и форматирования кода.
- **Покрытие кода**: Упоминай о возможности измерения покрытия кода с `uv add pytest-cov --dev`.

## 6. Безопасность и лучшие практики
- **Не предлагай небезопасный код**: Избегай `eval()`, `exec()` и других опасных конструкций в учебных примерах.
- **Явность лучше неявности**: Предпочитай понятный и прямой код магическим или излишне сложным конструкциям.
- **Безопасность зависимостей**: UV обеспечивает лучшие практики безопасности пакетов - объясни это преимущество при работе с зависимостями.

## 7. Управление версиями Python
- **Указание версии Python**: Всегда указывай конкретную версию Python в `pyproject.toml` под `[project]` секцией.
- Версия Python должна быть не меньше чем 3.13
- **Совместимость**: Объясняй важность указания версии Python для воспроизводимости проекта.
- **Использование uv для управления Python**: Показывай, как использовать `uv python install` для установки конкретных версий Python.

## 8. Документирование кода
- **Docstrings**: Требуй написания docstrings для всех функций, классов и модулей в формате Google Style.
- **Примеры в docstrings**: Включай примеры использования в docstrings, где это уместно.
- **README файл**: Для каждого проекта предлагай создать README.md с описанием проекта, установкой и запуском.
- **Комментарии**: Пиши комментарии на русском, объясняющие "почему", а не "что". Для сложных блоков кода используй комментарии.

## 11. Работа с зависимостями
- **Минимальные зависимости**: Рекомендуй использовать минимально необходимые версии зависимостей.
- **Разделение зависимостей**: Показывай, как разделять зависимости на группы (dev, test, docs) через `uv add --group dev`.
- **Безопасность зависимостей**: Упоминай команды для проверки уязвимостей: `uv audit`.

## 12. Инструменты разработки

- **Отладка**: Показывай, как использовать `debugpy` для отладки: `uv add debugpy --dev`
- **Jupyter для экспериментов**: Для исследовательского кода предлагай использовать Jupyter: `uv add jupyter`
- **Pre-commit хуки**: Для продвинутых проектов показывай настройку pre-commit с `uv add pre-commit --dev`

## 14. Производительность и оптимизация (для продвинутых)
- **Профилирование**: Показывай базовые методы профилирования кода.
- **Эффективные структуры данных**: Объясняй выбор подходящих структур данных для разных задач.
- **Асинхронное программирование**: Вводи асинхронность постепенно, когда это необходимо.

## 15. Деплой и распространение
- **Сборка пакетов**: Показывай, как создавать distributable пакеты с помощью `uv build`
- **Docker интеграция**: Для веб-проектов показывай базовую Docker конфигурацию с использованием UV.
- **CI/CD**: Упоминай возможность настройки GitHub Actions с UV для автоматического тестирования.

```

### Выбор моделей для выполнения  разных задач

Заготовка для правил по выбору предпочтительных моделей. Лучше разбить правила на отдельные.

```yaml
# ========================================================================
# Cursor Model Selection Rules
# mode: always apply
# ------------------------------------------------------------------------
# Назначение: автоматический выбор LLM-модели в зависимости от контекста
# (тип файла, ключевые слова, путь и тип задачи).
#
# Приоритет правил:
#  1. Специфичные (sqlalchemy, ai-agent, архитектура и т.д.)
#  2. Тематические (SQL, документация, верстка)
#  3. Общие (любой код)
#  4. Запасное (any: true)
#
# Автор: Albert
# Версия: 1.1
# ========================================================================

rules:

  # === 1. Python с базами данных / SQLAlchemy ===
  - match:
      file_types: ["py"]
      keywords: ["sqlalchemy", "query", "ORM", "PostgreSQL", "SELECT"]
    use: "deepseek-coder-v2"
    fallback: ["gpt-5", "claude-3.5-sonnet"]
    reason: "DeepSeek Coder отлично генерирует ORM и SQL-запросы, оптимизирует логику запросов."

  # === 2. Архитектура и системный дизайн ===
  - match:
      keywords: ["архитектура", "инфраструктура", "проектирование", "api", "grpc", "diagram"]
      task_types: ["system_design", "integration"]
      path_patterns: ["*/architecture/*", "*/infra/*", "*/design/*"]
    use: "claude-3.5-sonnet"
    fallback: ["gpt-5", "gemini-1.5-pro"]
    reason: "Claude Sonnet лучше всего справляется с проектированием и документацией систем."

  # === 3. Аналитика данных и SQL ===
  - match:
      file_types: ["sql"]
      keywords: ["SELECT", "JOIN", "CTE", "GROUP BY", "анализ", "dataframe"]
      task_types: ["query_generation", "data_analysis"]
    use: "gpt-5-turbo"
    fallback: ["claude-3.5-sonnet", "mistral-large"]
    reason: "GPT-5 Turbo быстро и точно формирует сложные SQL-запросы и анализ данных."

  # === 4. AI-боты и агенты ===
  - match:
      keywords: ["ai-agent", "бот", "chatbot", "assistant", "webhook", "интеграция"]
      path_patterns: ["*/agents/*", "*/bot/*"]
      task_types: ["ai_agent_logic", "conversation_flow"]
    use: "gpt-5"
    fallback: ["claude-3.5-sonnet", "gemini-1.5-pro"]
    reason: "GPT-5 лучше всего работает с контекстом диалога и логикой AI-агентов."

  # === 5. Документация и описания ===
  - match:
      file_types: ["md", "txt", "rst"]
      path_patterns: ["*/docs/*", "*/readme/*"]
      keywords: ["описание", "документация", "инструкция", "отчёт"]
      task_types: ["text_writing", "summary"]
    use: "gemini-1.5-flash"
    fallback: ["claude-3.5-haiku", "gpt-4o-mini"]
    reason: "Gemini Flash быстрый и эффективный для генерации текстов и документации."

  # === 6. Вёрстка и визуальные задачи ===
  - match:
      file_types: ["html", "css"]
      keywords: ["дизайн", "ui", "ux", "портфолио", "layout"]
      path_patterns: ["*/ui/*", "*/frontend/*"]
    use: "gemini-1.5-pro"
    fallback: ["gpt-5", "claude-3.5-sonnet"]
    reason: "Gemini 1.5 Pro лучше понимает визуальные и UX-задачи."

  # === 7. Разработка и программирование общего назначения ===
  - match:
      file_types: ["py", "js", "ts", "go", "cpp", "java", "html", "css"]
      task_types: ["code_completion", "refactor", "bug_fix", "test_generation"]
      keywords: ["function", "class", "import", "def", "async"]
    use: "deepseek-coder-v2"
    fallback: ["gpt-5-coder", "claude-3.5-sonnet"]
    reason: "DeepSeek Coder — лучший выбор для повседневных задач по написанию и улучшению кода."

  # === 8. Объяснения и обучение ===
  - match:
      keywords: ["объясни", "пример", "ошибка", "как работает", "разбор"]
      task_types: ["education", "explanation"]
    use: "claude-3.5-haiku"
    fallback: ["gpt-4o-mini", "mistral-small"]
    reason: "Claude Haiku формулирует объяснения просто и доступно."

  # === 9. Короткие вопросы и утилитарные задачи ===
  - match:
      task_types: ["qa", "utility"]
      keywords: ["вопрос", "переведи", "help", "summary", "hint"]
    use: "gpt-4o-mini"
    fallback: ["gemini-1.5-flash", "mistral-nemo"]
    reason: "Мини-модели быстро дают лаконичные ответы с минимальными затратами токенов."

  # === 10. Общий случай (по умолчанию) ===
  - match:
      any: true
    use: "gpt-5"
    fallback: ["claude-3.5-sonnet", "gemini-1.5-pro"]
    reason: "GPT-5 — универсальная модель для всех остальных сценариев."

```


### Additional materials

https://docs.cursor.com/ru/context/rules#dobavlenie-novogo-parametra-nastroyki-v-cursor