___
https://docs.python.org/3/tutorial/venv.html

Виртуальное окружение (далее ВО) - искусственно созданное окружение для python-програмы, помещающееся в отдельной папке.

К окружению относяться:
- интерпретатор
- пэккаджи
___
### 1. Создание ВО

Создать ВО можно с помощью команды:
```bash
python3 -m venv tutorial-env
```

Будет создана директория `tutorial-env`, в которой находится ВО. Созданное ВО уже содержит:
- python (интерпретатор)
- site-packages (в том числе pip)
- скрипт activate

___
### 2. Активация ВО

Можно активировать ВО с помощью команды:
```bash
source tutorial-env/bin/activate
```

Данный скрипт делает несколько всего интересных вещей:
- присваивает переменной `$VIRTUAL_ENV` путь к директории с ВО
- сбрасывает переменную `$PYTHONHOME`
- меняет подсказку командной строки (prompt)
- исполняет команды `hash`

Чтобы выйти из ВО достаточно написать:
```bash
deactivate
```

___
### 3. Управление пакетами

Когда ВО активировано можно устанавливать и удалять пакеты с помощью `pip`. Разумеется, делать это можно только в той оболочке, где оно активировано.

Когда ВО активировано `pip` взаимодействует только с пэккаджами, которые установлены в ВО. Все команды `pip` работают как полагается.

___
### 4. Практика использования ВО

##### 4.1. Создание нового проекта

1. Создать ВО в папке проекта, назвать `venv`
2. Добавить всю папку `venv` в `.gitignore`
3. Создать в папке проекта вспомогательный скрипт `act` (activate virtual environment) со следующим содержимым:
```bash
#!/bin/bash
venv_dir_basename="venv"
this_script_dir=$(dirname "$PWD/$0")
source "$this_script_dir/$venv_dir_basename/bin/activate"
```

>[!tip]
>Чтобы использовать этот скрипт нужно выполнить команду `. act` или `source act`

4. Создать в папке проекта пустой файл `requirements.txt`
5. При каждом изменении состава пэккаджей обновлять файл `requirements.txt` с помощью команды:
```bash
#!/bin/bash
pip3 freeze > requirements.txt
```

##### 4.2. Клонирование проекта

1. Создать ВО в папке проекта, назвать `venv`
2. Из виртуального окружения выполнить команду:
```bash
python -m pip install -r requirements.txt
```

После переключения на другую версию проекта нужно сначала удалить папку `venv`, а потом выполнить действия изложенные выше.

##### 4.3. Скрипт для обновления ВО

Переключение между версиями это довольно частая операция, поэтому можно автоматизировать ее с помощью скрипта `upd` (update virtual environment):
```bash
#!/bin/bash
this_script_dir=$(dirname "$PWD/$0")
venv_dir_basename="venv"
venv_dir="$this_script_dir/$venv_dir_basename"
activate_path="$venv_dir/bin/activate"
req_path="$this_script_dir/requirements.txt"

if ! [ -e "$req_path" ]
then
    printf "ERROR: $req_path doesn't exists!\n" >&2
    exit 1
fi

if ! [ -f "$req_path" ]
then
    printf "ERROR: $req_path isn't a regular file!\n" >&2
    exit 1
fi

if [ -e "$venv_dir" ]
then
    if ! [ -d "$venv_dir" ]
    then
        printf "ERROR: $venv_dir is not a directory!\n" >&2
        exit 1
    fi

    if ! [ -e "$activate_path" ]
    then
        printf "ERROR: $activate_path doesn't exists!\n" >&2
        exit 1
    fi

    if ! [ -f "$activate_path" ]
    then
        printf "ERROR: $activate_path isn't a regular file!\n" >&2
        exit 1
    fi

    source "$activate_path"

    python3 -m pip freeze | diff - "$req_path" > /dev/null

    if [ $? -eq 0 ]
    then
        printf "Virtual environment is up to date already.\n"
        exit 0
    fi

    deactivate

    rm -rf "$venv_dir"
    printf "Virtual environment was removed.\n"
fi

python3 -m venv "$venv_dir"

if ! [ $? -eq 0 ]
then
    printf "Virtual environment creation was failed.\n" >&2
    exit 1
fi

source "$activate_path"
python3 -m pip install -r "$req_path"

if ! [ $? -eq 0 ]
then
    printf "Virtual environment reinstallation was failed.\n" >&2
    exit 1
fi

printf "Virtual environment was successfully installed.\n"
```
Так же этот скрипт поможет сразу после клонирования проекта.

>[!warning]
>В этой системе есть как минимум одно уязвимое место: файл requirements.txt не содержит версии python.



