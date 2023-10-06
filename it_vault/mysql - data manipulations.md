___
Существует четыре директивы для манипулирования таблицами:
- `SELECT`
- `INSERT`
- `DELETE`
- `UPDATE`

Для директивы `SELECT` имеет отдельная статья ([[mysql - select]]), с которой обязательно нужно ознакомиться перед прочтением этой статьи.
___
### 1. INSERT

С помощью директивы `INSERT` можно вставлять значени, таблицы и даже результаты `SELECT`-запросов. Синтаксис:
```sql
INSERT INTO tbl_name [(col_name [, col_name] ...)]
VALUES (value_list) [, (value_list)] ...

INSERT INTO tbl_name [(col_name [, col_name] ...)]
VALUES ROW(value_list)[, ROW(value_list)][, ...];

INSERT INTO tbl_name SET assignment_list

INSERT INTO tbl_name [(col_name [, col_name] ...)] SELECT ...;

INSERT INTO tbl_name [(col_name [, col_name] ...)] TABLE tbl_name;

где value_list -> value [, value] ...
	assignment_list -> col_name = value[, col_name = value]... 
```

Если вставка строки приводит к дублированию `PRIMARY KEY` или `UNIQUE`-столбца, то обычно ее нужно обновить, для этого существует специальный синтаксис:
```sql
INSERT INTO tbl_name VALUES ... ON DUPLICATE KEY UPDATE assignment_list;
```
___
### 2. DELETE

Синтаксис:
```sql
DELETE FROM tbl_name [[AS] tbl_alias] [WHERE where_condition];
```
Из таблицы будут удалены все строки, которые удовлетворяют условию фильтрации `WHERE`.
___
### 3. UPDATE

Синтаксис:
```sql
UPDATE [LOW_PRIORITY] [IGNORE] table_reference
    SET col_name = value [, col_name = value] ...
    [WHERE where_condition]
    [ORDER BY ...]
    [LIMIT row_count]   

где value -> expr | DEFAULT
```

Блок `ORDER BY` определяет порядок, в котором строки будут обновлены.
Блок `LIMIT` определяет максимальное количество строк, которые подлежат обновлению.