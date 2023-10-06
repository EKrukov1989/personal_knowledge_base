___
Директива `CREATE` создает таблицу в текущей базе данных.

Синтаксис:
```mysql
CREATE TABLE [IF NOT EXISTS] table_name (create_definition)
```
где `create_definition` представляет собой перечисление через запятую описаний столбцов, их аттрибутов, а также наложенных ограничений.

Пример:
```mysql
CREATE TABLE translations (
    id SERIAL,
    word VARCHAR(256) NOT NULL,
    trans VARCHAR(256) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL
);
```
Более подробно о `create_definition` можно прочитать здесь:
- [[mysql - scheme]]

Или можно создать таблицу с такой же схемой как у другой таблицы:
```mysql
CREATE TABLE [IF NOT EXISTS] tbl_name LIKE old_tbl_name;
```


