___
Constraints - это ограничения, которые накладыватся на значения таблицы.
В данной статье рассмотрены наиболее интересные и частоупотребимых ограничения.
___
##### 1. PRIMARY KEY
```mysql
syntax: CONSTRAINT <constraint_name> PRIMARY KEY (<column_name>,...)
```
Ограничение `PRIMARY KEY` указывает mysql, какой столбец (или может группу) столбцов использовать в качестве первичного ключа таблицы. Данные столбцы будут использованы для создания индекса, поэтому они должны быть уникальны и не NULL.
___
##### 2. UNIQUE
```mysql
syntax: CONSTRAINT <constraint_name> PRIMARY KEY (<column_name>,...)
```
Ограничение `UNIQUE` требует чтобы все значения в столбце были уникальны. При попытке вставить неуникальное значение в столбец будет сообщение об ошибке. При этом в столбце может быть несколько значений со значением `NULL`.
___
##### 3. FOREIGN KEY
```mysql
syntax: CONSTRAINT <constr> FOREIGN KEY (<col1>) REFERENCSES <table> ( <col2>)
```
Ограничение `FOREIGN KEY` требует, чтобы каждое значение столбца `<col1>` также присутствовало в `<table>` в столбце `<col2>`.
___
##### 4. CHECK
```mysql
syntax: CONSTRAINT <constr> CHECK (expr) [[NOT] ENFORCED]
```
Ограничение `CHECK` требует, чтобы было выполнено условие `expr`. Если условие нарушается, то будет сообщение об ошибке.

Ограничение `CHECK` может относиться к столбцу или ко всей таблице. Пример:
```mysql
col_name INT CONSTRAINT constr_name CHECK (col_name > 0),  # к столбцу
CONSTRAINT constr_name CHECK (c1 <> 0)          # к таблице
```

Пример использования ограничения `CHECK`:
```mysql
gender CHAR(1) CHECK (gender IN ('M', 'F'))
```