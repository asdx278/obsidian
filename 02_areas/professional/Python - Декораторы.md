---
class: note
area: professional
tags:
  - python
created: 2025-11-20
---
**MOC: [[MOC - Python]]**

# Python - Декораторы

> [!tldr] ai review
> 

## Header

Декоратор вызывает целевую функцию, получает ее аргументы, добавляет перед выполнение функции и после слова start, end, возвращает результат целевой функции:

```python
def test(func):
    def wrapper(*args, **kwargs):
        print('start')  # Выводим 'start' при вызове функции
        result = func(*args, **kwargs)  # Вызываем исходную функцию
        print('end')    # Выводим 'end' после завершения функции
        return result   # Возвращаем результат исходной функции
    return wrapper

# Пример использования декоратора
@test
def example_function():
    print("Функция выполняется")
    return "Результат"

# Тестируем
example_function()
```

### Additional materials