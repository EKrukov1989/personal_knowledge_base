Источники: `man pip3`, [pip_user_guide](https://pip.pypa.io/en/stable/user_guide/)
___
pip - инструмент для установки и управления пэкаджами пайтон.

Пэккадж - это файл, который может быть загружен и установлен.
Обычно пэккадж имеет формат `.whl` ([[python - _wheel]]).

>[!warning]
>pip это команда для установки пэкаджей Python 2; для установки пэкаджей Python 3 следует использовать pip3.

___
### 1. Синтаксис:

`pip3 <command> [options]`

___
### 2. Список команд

- `install` -> установить пэккадж
- `uninstall` -> деинсталлировать пэккадж
- `list` -> вывести список установленных пэккаджей
- `freeze` -> вывести список установленных пэккаджей в формате requirements
- `show <name>` -> вывести информацию об установленном пэккадже
- `search` -> DEPRECATED
- `wheel` -> собрать wheel согласно requirement ([[python - _wheel]])

>[!note]
>Команда `freeze` не выводит пэккаджей, которые были установлены изначально.

___
### 3. Команда  install

Все опции весьма специфические и при рядового пользователя не нужны.

`-i,--index-url <url>`
- указать `url` для Python Package Index
- по умолчанию https://pypi.python.org/simple/

`-U,--upgrade`
- обновить ВСЕ пэккаджи до последней версии

`--no-deps`
- не устанавливать зависимости (по умолчанию будут установлены все зависимости пэккаджа)

`-t,--target <dir>`
- установить пэккадж в `dir`

`-d,--download <dir>`
- загрузить пэккадж в `dir`

`-r,--requirement <file>`
- установить все пэккаджи из заданного requirement файла 
- можно указать несколько таких опций

___
### 4. Команда  uninstall

`-y, --yes`
- не спрашивать подтверждения при удалении

`-r,--requirement <file>`
- деинсталлировать все пэккаджи из заданного requirement файла 
- можно указать несколько таких опций


