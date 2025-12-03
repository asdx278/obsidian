---
class: note
area: professional
tags:
  - claude
  - ai
created: 2025-12-04
---
**MOC: [[MOC - Professional]]**

# Claude - Установка собственного навыка Claude Code на Linux

> [!tldr] ai review
> 

## Структура навыков

Навыки в Claude Code хранятся в директории:
```
~/.claude/skills/
```

Каждый навык представляет собой папку с файлом `skill.md`, который содержит инструкции для Claude.

## Шаг 1: Создание директории для навыков

Откройте терминал и выполните:

```bash
mkdir -p ~/.claude/skills
```

## Шаг 2: Создание вашего навыка

Создайте папку для вашего навыка. Например, для навыка "my-finance-skill":

```bash
mkdir -p ~/.claude/skills/my-finance-skill
```

## Шаг 3: Создание файла skill.md

Создайте файл с описанием навыка:

```bash
nano ~/.claude/skills/my-finance-skill/skill.md
```

Или используйте любой текстовый редактор:
```bash
code ~/.claude/skills/my-finance-skill/skill.md
# или
gedit ~/.claude/skills/my-finance-skill/skill.md
# или
vim ~/.claude/skills/my-finance-skill/skill.md
```

## Шаг 4: Структура файла skill.md

Вот пример содержимого для финансового навыка:

```markdown
# Finance Dashboard Skill

Этот навык помогает создавать и настраивать интерфейсы для приложений по учету личных финансов.

## Когда использовать этот навык

Используйте этот навык когда пользователь просит:
- Создать дашборд для финансов
- Добавить визуализацию финансовых данных
- Создать формы для ввода транзакций
- Разработать отчеты по доходам и расходам

## Стиль и дизайн

При создании финансовых интерфейсов используй:
- Чистый, профессиональный дизайн
- Четкую типографику для цифр (моноширинные шрифты)
- Цветовое кодирование (зеленый для доходов, красный для расходов)
- Интерактивные графики и диаграммы
- Адаптивный дизайн для мобильных устройств

## Компоненты

Включай следующие компоненты:
1. Карточки статистики (баланс, доходы, расходы)
2. Список транзакций
3. Графики расходов по категориям
4. Быстрые действия для добавления данных

## Технологии

Предпочитаемые технологии:
- Vanilla HTML/CSS/JS для простых интерфейсов
- React для сложных приложений
- Chart.js или D3.js для визуализаций
- Tailwind CSS для стилизации
```

## Шаг 5: Проверка установки

После создания файла, проверьте структуру:

```bash
ls -la ~/.claude/skills/my-finance-skill/
```

Вы должны увидеть файл `skill.md`.

## Шаг 6: Использование навыка

В Claude Code используйте команду:

```
/skill my-finance-skill
```

Или просто упомяните навык в своем запросе:
```
Используй навык my-finance-skill, чтобы создать страницу отчетов
```

## Пример: Создание простого навыка для тестирования

Быстрый тест:

```bash
# Создаем директорию
mkdir -p ~/.claude/skills/test-skill

# Создаем файл
cat > ~/.claude/skills/test-skill/skill.md << 'EOF'
# Test Skill

Это тестовый навык.

## Инструкции

Когда этот навык активирован, ответь: "Тестовый навык работает! ✅"
EOF

# Проверяем
cat ~/.claude/skills/test-skill/skill.md
```

Теперь в Claude Code напишите:
```
/skill test-skill
```

## Расширенные возможности

### Добавление дополнительных файлов

Вы можете добавить дополнительные ресурсы в папку навыка:

```bash
~/.claude/skills/my-finance-skill/
├── skill.md              # Основной файл навыка
├── templates/            # Шаблоны кода
│   ├── dashboard.html
│   └── form.html
├── examples/             # Примеры
│   └── example.js
└── docs/                 # Документация
    └── usage.md
```

### Ссылки на другие файлы в skill.md

В `skill.md` можно ссылаться на другие файлы:

```markdown
## Шаблоны

Используй шаблон из файла `templates/dashboard.html` как основу.

## Примеры

Смотри `examples/example.js` для примеров использования.
```

## Управление навыками

### Просмотр всех навыков

```bash
ls ~/.claude/skills/
```

### Редактирование навыка

```bash
nano ~/.claude/skills/my-finance-skill/skill.md
```

### Удаление навыка

```bash
rm -rf ~/.claude/skills/my-finance-skill
```

### Резервное копирование

```bash
# Создать backup
tar -czf claude-skills-backup.tar.gz ~/.claude/skills/

# Восстановить
tar -xzf claude-skills-backup.tar.gz -C ~/
```

## Советы

1. **Четкие инструкции**: Пишите конкретные и понятные инструкции в skill.md
2. **Примеры**: Добавляйте примеры кода и использования
3. **Контекст**: Объясняйте, когда навык должен использоваться
4. **Тестирование**: Протестируйте навык перед использованием
5. **Версионность**: Используйте git для версионирования ваших навыков

```bash
cd ~/.claude/skills/my-finance-skill
git init
git add .
git commit -m "Initial skill version"
```

## Поиск проблем

Если навык не работает:

1. Проверьте путь:
   ```bash
   ls -la ~/.claude/skills/my-finance-skill/skill.md
   ```

2. Проверьте содержимое:
   ```bash
   cat ~/.claude/skills/my-finance-skill/skill.md
   ```

3. Проверьте права доступа:
   ```bash
   chmod 644 ~/.claude/skills/my-finance-skill/skill.md
   ```

4. Перезапустите Claude Code

## Полезные ресурсы

- Официальная документация: https://github.com/anthropics/claude-code
- Примеры навыков: https://github.com/anthropics/claude-code/tree/main/examples/skills
- Community skills: Проверьте репозитории на GitHub с тегом "claude-code-skill"

---

**Готово!** Теперь вы можете создавать и использовать собственные навыки в Claude Code на Linux.


### Additional materials