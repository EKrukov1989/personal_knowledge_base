___
Существует несколько вариантов ветвления в MySQL

Аналог if-then-else:
```sql
CASE
	WHEN condition_1 THEN expression_1
	WHEN condition_2 THEN expression_2
	...
	WHEN condition_N THEN expression_N
	[ELSE expression_default]
END
```

Аналог switch ():
```sql
CASE variable
	WHEN value_1 THEN expression_1
	WHEN value_2 THEN expression_2
	...
	WHEN value_N THEN expression_N
	[ELSE expression_default]
END
```

Применять условную логику следует в `select_expr`:
```sql
SELECT name, CASE ... END FROM ...;
```
