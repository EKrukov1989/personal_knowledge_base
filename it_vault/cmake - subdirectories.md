___

Cmake позволяет выстроить проект любой сложности с помощью одного единственного CMakeLists.txt-файла. Однако удобнее организовать проект в виде нескольких cmake проектов, связанных в форме дерева.

___
### 1. add_subdirectory()

```
add_subdirectory(sourceDir [ binaryDir ] [ EXCLUDE_FROM_ALL ])
```

- `sourcDir` - директория в которой находится директория с исходными файлами, которые следует включить в проект (еще точнее директория с `CMakeLists.txt`). Может быть как абсолютной, так и относительной
- если `sourceDir` это относительная директория, то она будет относительна текущей source-директории, то есть текущему `CMakeLists.txt`
- `binaryDir` - директория, в которую следует поместить сгенерированные бинарные файлы
- если `sourceDir` это относительная директория, то `binaryDir` можно не указывать. В этом случае cmake создает в папке билда директорию, с таким же путем как и в `sourceDir`
- если `sourceDir` это абсолютная директория, то `binaryDir` должен быть указан явно
- `EXCLUDE_FROM_ALL` - исключить из списка дефолтных целей

___
### 2. include()

Команда `include()` имеет две формы:
```
include(fileName [OPTIONAL] [RESULT_VARIABLE myVar] [NO_POLICY_SCOPE])
include(module [OPTIONAL] [RESULT_VARIABLE myVar] [NO_POLICY_SCOPE])
```
- `filename` - имя cmake-файла, обычно с расширением `.cmake`
- `module` - имя именнованного модуля
- `NO_POLICY_SCOPE` - не создавать локальную области видимости для политик

Команда `include()` имеет следующие отличия от `add_subdirectory()`:
- нужно указывать файл, а не директорию
- файл, включенный с помощью `include()`, не имеет собственной области видимости
- переменные `CMAKE_CURRENT_SOURCE_DIR` и `CMAKE_CURRENT_BINARY_DIR` не изменяются

___
### 3. Специальные переменные

>[!note]
>Если из `dir_1/CMakeLists.txt` вызывается функция `add_subdirectory(source_dir)`, где `source_dir` == `dir_2/CMakeLists.txt`, то:
>- `dir_1/...`  называется родительским или вызывающим файлом для `dir_2/...`
>- a `dir_2/...` называет дочерним для `dir_1/...` 

Cmake имеет ряд встроенных переменных, чтобы упростить для пользователя процесс указания путей:
- `CMAKE_SOURCE_DIR` -> директория в которой находится корневой `CMakeLists.txt`
- `CMAKE_BINARY_DIR` -> корневая директория билда
- `CMAKE_CURRENT_SOURCE_DIR` -> директория текущего `CMakeLists.txt`
- `CMAKE_CURRENT_BINARY_DIR` -> текущая директория билда (то есть директория билда для текущего `CMakeLists.txt`)

- `CMAKE_CURRENT_LIST_DIR` -> директория текущего файла cmake-скрипта
- `CMAKE_CURRENT_LIST_FILE` -> путь к текущему файлу cmake-скрипта
- `CMAKE_CURRENT_LIST_LINE` -> номер строки файла, который обрабатывается в данный момент

Описание поведения переменных:
- `CMAKE_SOURCE_DIR` и `CMAKE_BINARY_DIR` -> никагда не изменяются.
- `CMAKE_CURRENT_LIST_DIR`, `CMAKE_CURRENT_LIST_FILE`, `CMAKE_CURRENT_LIST_LINE` -> всегда актуальны, т.е. указывают на текущий обрабатываемый файл и строку
- `CMAKE_CURRENT_SOURCE_DIR`, `CMAKE_CURRENT_BINARY_DIR` -> изменяются только при использовании команды `add_subdirectory()`

___
### 4. include_guard()

Некоторые файлы могут быть вызваны в проекте несколько раз, что может привести к нежелательным последствиям. Чтобы этого избежать используется следующий механизм:
```
if(DEFINED cool_stuff_include_guard)
return()
endif()
set(cool_stuff_include_guard 1)
# ...
```

Или лучше в начале файла написать вот так:
```
cmake_minimum_required(VERSION 3.25)  # не нужно забывать про это
include_guard()
```
Это гарантирует, что файл будет обрабатан всего один раз.


>[!note] Замечание
>При использования `add_subdirectory()` все переменные созданные в вызванном файле невидимы для вызывающего файла, однако это не распространяется на добавленные цели!

