___
MySQL поддерживает операции над таблицами:
- union
- intersection
- except

Это операции возможны, только если таблицы удовлетворяют следующим условиям:
- В обеих таблицах одинаковое число столбцов;
- Типы столбцов совпадают, или же известно преобразования из одного типа в другой.

___
### 1. Синтаксис

```sql
SELECT ...
UNION
SELECT ...;

# или 
SELECT ... UNION SELECT ...;
```

___
### 2. UNION

`UNION` -> объединение строк из двух таблиц (все д)
`UNION ALL` -> объединение строк из всех столбцов, включая дубликаты

Пример:
```
A = ( 1 1 1 2 )
B = ( 1 1 3 )
A UNION B     -> ( 1 2 3 ) # все дубликаты удалены
A UNION ALL B -> ( 1 1 1 2 1 1 3 )
```

___
### 3. INTERSECT

Операция `INTERSECT` находит пересечения двух таблиц и ее результат довольно очевиден.
___
### 4. EXCEPT

```sql
A EXCEPT B
```
В результирующем наборе будут все значения таблицы `A`, за вычетом значений которые присутствуют в таблице `B`.
```sql
A EXCEPT ALL B
```
Аналогично, однако из результирующего набора не может быть исключено больше элементов, чем присутствует в `B`.

Пример:
```
A = ( 1 1 1 2 )
B = ( 1 1 )
A EXCEPT B -> ( 2 )
A EXCEPT ALL B -> ( 1 2 )
```
