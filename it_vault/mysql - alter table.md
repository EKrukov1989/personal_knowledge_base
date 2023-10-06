___
Директива `ALTER TABLE` изменяет структуру таблицы.

Синтаксис:
```mysql
ALTER TABLE table_name [alter_option [, alter_option] ...]
```

Возможные варианты `alter_option`:
```mysql
# добавить столбец
ADD column_definition

# добавить столбец до или после столбца existing_column_name
ADD column_definition FIRST existing_column_name
ADD column_definition AFTER existing_column_name

# удалить столбец
DROP existing_column_name

# изменить имя столбца:
RENAME COLUMN existing_column_name TO new_column_name

# изменить стобец:
MODIFY existing_column_name INT NOT NULL;

# изменить стобец и его имя:
CHANGE existing_column_name new_column_name BIGINT NOT NULL;

# добавление и удаление ограничений на примере foreign key:
ADD CONSTRAINT name FOREIGN KEY (...)
DROP FOREIGN KEY fk_name;
```
