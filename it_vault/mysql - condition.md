___
В целом синтаксис условий MySQL инуитивно понятен.
В этой статье сначала кратко перечислен весь стандартный синтаксис, а потом коротко рассмотрены разные частности.
___
### 1. Стандартные элементы синтаксиса

Список интуитивно понятный элементов:
1. `and or not`
- наибольший приоритет у `not`, наименьший у `or`
2. `()`
- с помощью скобок можно изменять порядок вычисления условий
3. `= != < > <>`
- следует отметить, что `!=` и `<>` это одно и то же, но предпочтительно использовать `<>`, так как он есть в стандарте
___
### 2. Элементы синтаксиса условий
##### *BETWEEN*
Оператор `BETWEEN` это просто короткая форма записи двух неравенств
```sql
variable BETWEEN value_1 AND value_2
variable >= value_1 AND variable => value_2  # то же самое
```
##### *IN, ALL, ANY*
Оператор `IN` позволяет проверить наличие значения в определенном наборе.
Набор может быть выражен в видео массива или подзапроса.
```sql
variable IN ( 'Alice', 'Bob', 'Carol')
variable IN ( SELECT name FROM names )
```
Оператор `ANY` проверяет, что по крайней мере один элемент набора соответствует условию. Оператор `ALL` проверяет, что все элементы набора соответствует условию.
```sql
'Bob' = ANY ('Alice', 'Bob', 'Carol');
12 > ALL ( 10, 8, 7 );
```

>[!warning]
>ANY и ALL это операторы,а не функции. Перед скобкой должен стоять пробел!
##### *LIKE*
Подобие глоббинга, который поддерживает всего два wildcard-символа:
```
_ (аналог .)
% (аналог *)
```
Оператор `LIKE` проверяет соответствие паттерну:
```sql
variable LIKE '%.txt'
```
##### *REGEXP, RLIKE, REGEXP_LIKE()*
Проверки на соответствие регулярному выражению:
```sql
variable REGEXP '.*\.txt$'
REGEXP_LIKE(expr, pattern) # возвращает 1, если expr соответствует паттерну
```
##### *IS NULL*
Оператор `IS NULL` проверяет, что значение переменной `NULL`
>[!warning]
>Так как `NULL` не равен самому себе, то условие `variable = NULL` всегда будет ложно. Вместо него следует использовать `variable IS NULL`.
##### *EXISTS*
Оператор `EXISTS` проверяет, что результирующий набор подзапроса не пуст.
```sql
EXISTS ( subquery )
```
