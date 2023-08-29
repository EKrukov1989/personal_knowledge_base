___
Принципы работы с регулярными выражениями такие же как в [[cpp - std regex]].

>[!note]
>Таргет и паттерн могут иметь тип string и bytes, но оба должны быть какого-то одного типа.

>[!note]
>Паттерн предпочительно записывать в виде raw string: `r""`.

___
### 1. Терминология

- `target` -> текст в котором ищем
- `pattern` -> строка с регулярным выражением
- `regexp` -> конечный автомат, полученный в результате компиляции регулярного выражения
- `match_object` -> объект, содержащий результаты поиска
- `group` -> подстрока, соответствующая регулярному выражению внутри круглых скобок. `match_object` может содержать несколько `group`

___
### 2. Функции

###### search
- находит первую подходящую подстроку и возвращает match-объект.
- если ничего не найдено - возвращает `None`
- пример:
```python
target = "mycmd --debug=true --optimization-level=3 --output=result.txt";
pattern = "(--([a-zA-z0-9_-]+)=([a-zA-z0-9_.-]+))"
regexp = re.compile(pattern)
match_object = regexp.search(target)
print(match_object.groups())
# Вывод:
# ('--debug=true', 'debug', 'true')
```

###### match
- проверяет есть ли подходящая подстрока, начало которой, совпадает с началом таргет

###### fullmatch
- проверяет является ли таргет подходящей подстрокой

###### findall
- максимально удобная функция для программиста
- находит все подходящие подстроки и выводит их в виде массива
- пример:
```python
# ... здесь то же, что и для search
all_matches = regexp.findall(target)
print(all_matches)
# Вывод:
# [('--debug=true', 'debug', 'true'), ('--optimization-level=3', 'optimization-level', '3'), ('--output=result.txt', 'output', 'result.txt')]
```

###### finditer
- возвращает итератор на последовательность подходящих подстрок
- пример:
```python
# ... здесь то же, что и для search
iter = regexp.finditer(target)
for obj in iter:
	print(obj.groups())
# Вывод:
# ('--debug=true', 'debug', 'true')
# ('--optimization-level=3', 'optimization-level', '3')
# ('--output=result.txt', 'output', 'result.txt')
```

