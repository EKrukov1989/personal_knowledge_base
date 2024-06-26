___
Директива SELECT используется для того, чтобы получить строки из одной или более таблиц. Порядок обработки запроса:

- Перед выполнением запроса сервер выполняет проверку привилегий, правильность синтаксиса запроса и передает запрос оптимизатору запросов.
- Оптимизатор рассматривает порядок соедиения таблиц, индексы и в результате составляет план выполнения запроса.
- По завершении выполнения запроса сервер возвращает результирующий набор в вызывающее приложение (result set). Результирующий набор - всегда таблица, если ничего не найдено - пустая таблица

Select-запрос представляет собой последовательность директив, которая однозначно определяет требуемый результат.

Структура запрос выглядит вот так:
```sql
SELECT
	[ALL | DISTINCT | DISTINCTROW ]
    select_expr [, select_expr] ...
    [FROM table_references
    [WHERE where_condition]
    [GROUP BY {col_name | expr | position}, ... [WITH ROLLUP]]
    [HAVING where_condition]
    [ORDER BY {col_name | expr | position}
      [ASC | DESC], ... [WITH ROLLUP]]
    [LIMIT {[offset,] row_count | row_count OFFSET offset}]
    (список возможных директив неполный!)
```

>[!warning] Пункты запроса должны использоваться именно в этом порядке.

Все составляющие запроса рассмотрены далее.
___
### 1. SELECT

```sql
SELECT select_expr
```

`select_expr` определяет список столбцов, которые нужно получить (не менее одного).
Нужно через запятую перечислить имена столбцов, которые следует выбрать. Возможны следующие варианты:
- `col_name` -> выбрать столбец
- `table_name.col_name` -> имя столбца с указанием таблицы
- `*` -> выбрать все столбцы
- `table_name.*` -> выбрать все столбцы из `table_name`
- `FOO(col_name)` -> результат выполнения функции
- `42` -> константа

По умолчанию после выполнения запроса столбцы в результирующем наборе будут иметь такие же имена, которые были указаны в `select_expr` (кроме `*`, конечно). Можно задать имена столбцов в результирующем наборе следующими способами:
- `col_name_1 alias_1, col_name_2 alias_2`
- `col_name_1 AS alias_1, col_name_2 AS alias_2`

`select_expr` может содержать специальный оператор `DISTINCT`.
При использовании этого оператора из результирующего набора будут удалены все дубликаты строк. Пример использования: `SELECT DISTINCT name, accout`

___
### 2. FROM

```sql
FROM table_references
```

`table_references` определяет таблицу, из которой следует получить столбцы и может представлять собой:
- имя таблицы
- подзапрос ([[mysql - subquery]])
- соединение ([[mysql - join]])
___
### 3. WHERE

В блоке `WHERE` происходит фильтрация строке в `table_references` на основе `where_condition` ([[mysql - condition]]).

Все строки, не удовлетворяющие условию отсеиваются уже на этом этапе.

>[!note]
>В блоке `WHERE` можно использовать любую функцию, кроме aggregate-функций.

___
### 4. GROUP BY

Блок `GROUP BY` позволяет сгруппировать строки.
```sql
GROUP BY col_name # здесь col_name_1 из select_expr
```
При наличии такого блока, в результирующем наборе останется только по одной строке для каждого из присутствующих значений `col_name`.

Можно сгруппировать сразу по нескольким столбцам.
```sql
GROUP BY col_name_1, col_name_2
```

Группировка позволяет использовать аггрегатные функции ([[mysql - aggregate]]).
___
### 5. HAVING

Блок `HAVING` аналогичен блоку `WHERE`.
Только позволяет использовать аггрегатные функции ([[mysql - aggregate]]).
___
### 6. ORDER BY

Строки результирующего набора возвращаются в произвольном порядке.
Их можно упорядочить с помощью блока `ORDER BY`.  Например:
```sql
ORDER BY column_name_1, column_name_2 [DESC]; 
```
Такой блок упорядочит результаты по паре значений.
По умолчанию выполняется сортировка по возрастанию, чтобы отсортировать по убыванию надо добавить в конце ключевое слово `DESC`
___
### 7. LIMIT

С помощью блока `LIMIT` можно ограничить количество строк в результирующем наборе. Есть два варианта синтаксиса:
```sql
LIMIT [offset,] row_count
LIMIT row_count OFFSET offset
```
где:
- `offset` -> количество строк в начале таблицы, которые будут выброшены
- `row_count` -> количество строк, которое будет в итогов результирующем наборе