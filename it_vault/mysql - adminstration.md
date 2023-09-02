___
### 1. Установка

Как установить и настроить MySQL на Ubuntu 20.04: [digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04)

```bash
# Установить mysql
sudo apt update
sudo apt install mysql-server
sudo systemctl start mysql.service

# Прежде чем запускать скрипт `mysql_secure_installation` следует настроить пароль на root, чтобы избежать дурацкого бага:
sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
quit

# Выполнить скрипт по настройке безопасности:
sudo mysql_secure_installation

# Настроить root обратно на аутентификацию без пароля:
mysql -u root -p
ALTER USER 'root'@'localhost' IDENTIFIED WITH auth_socket;
quit
```
___
### 2. Аутентификация

Чтобы выполнить любое действие в mysql нужно сначала пройти процедуру аутентификации.
Способ аутентификации зависит от `auth_plugin`, который выбран для пользователя.

Если `auth_plugin == auth_socket`, то следует выполнить команду:
```bash
mysql
```
При этом не нужно указывать имя пользователя mysql, так как подразумевается (и требуется), чтобы имя пользователя mysql совпадало с именем пользователя ОС.

Чтобы зайти под рутом следует выполнить команду:
```bash
sudo mysql
```

Если `auth_plugin == mysql_native_password`, то следует ввести команду:
```bash
mysql -u <username> -p
# после чего будет предложено ввести пароль
```
___
### 3. Управление пользователями

В mysql имеется root-юзер, который создан по умолчанию и имеет все возможные права доступа. Всех остальных пользователей нужно создавать и настраивать им права.

```mysql
# Вывести таблицу пользователей:
SELECT Host, User FROM mysql.user;

# Создать пользователя можно с помощью команды (сокращенный вариант):
CREATE USER [IF NOT EXISTS] user [auth_option]
Примеры:
CREATE USER 'gene'@'localhost' IDENTIFIED WITH auth_socket;
CREATE USER 'robot'@'localhost' IDENTIFIED WITH mysql_native_password BY '123';

# Изменить способ аутентификации пользователя:
ALTER USER 'gene'@'localhost' IDENTIFIED WITH auth_socket;

# Удалить пользователя:
DROP USER 'gene'@'localhost';
```

>[!note]
>Если пользователь создан с `auth_option == auth_socket`, то аутентифицироваться можно будет только если имя пользователя mysql совпадает с именем пользователя ОС, который осуществляет вход.
___
### 4. Управление привилегиями

Возможные действия с привилегиями:
```mysql
# показать привилегии для текущего пользователя:
SHOW GRANTS;  # для текущего пользователя:
SHOW GRANTS FOR 'username'@'host';

# Предоставить привилегии:
GRANT PRIVILEGES ON database.table TO 'username'@'host' WITH GRANT OPTION;

# Забрать привилегии:
REVOKE [IF EXISTS] PRIVILEGES FROM 'username'@'host'

# Можно предоставить привилегии только в отношении таблиц определенной базы данных (specific_base):
GRANT ALL PRIVILEGES on specific_base.* TO 'username'@'localhost';
```

`WITH GRANT OPTION` -> этот суффикс означает, что указанному пользователю разрешается предоставлять все данные ему привилегии другим пользователям.
`PRIVILEGES` - это список привилегий через запятую.

Список возможных привилегий (неполный):
- `ALL PRIVILEGES` -> все возможные привилегии
- `CREATE` -> создавание баз данных, таблиц или индексов
- `ALTER` -> изменение структур таблиц
- `DROP` -> удаление баз данных, таблиц
- `INSERT, UPDATE, DELETE` -> вставка, изменение, удаление строк в таблицах
- `INDEX` -> создание и удаление индексов
- `SELECT` -> получение строк из таблиц
___
### 5. Управление базами данных

Возможные действия с базами данных:
```mysql
# вывести таблицу с именами всех баз данных:
SHOW DATABASES;

# создать/удалить базу данных
CREATE DATABASE database_name;
DROP DATABASE database_name;

# выбрать базу данных в качестве текущей:
USE database_name;
```
___
### 6. Управление таблицами

```mysql
# Прежде чем работать с таблицами, следует выбрать базу данных:
USE database_name;

# выбрать базу данных в качестве текущей:
SHOW TABLES;

# показать схему таблицы:
DESCRIBE table_name;

# показать индексы таблицы:
SHOW INDEXES table_name;

# Удалить таблицу:
DROP TABLE table_name;

# Создать таблицу (пример):
CREATE TABLE translations (
    id SERIAL,
    word VARCHAR(256) NOT NULL,
    trans VARCHAR(256) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL
);
```

Директивы `CREATE TABLE` и `ALTER TABLE` разобраны в отдельной статье:
- [[mysql - scheme]]


