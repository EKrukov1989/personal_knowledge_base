___
Данная статья составлена как обзор `Chapter 18. Working with files` книги `Craig Scott - Professional Cmake` и содержит только некоторые самые интересные замечания.

___
### 1. Команда get_filename_component

`get_filename_component(outVar input component [CACHE])`
- получение компонентов имени файла
`get_filename_component(outVar input component [BASE_DIR dir] [CACHE])`
- получение компонентов имени файла относительно пути `dir`
`get_filename_component(progVar input PROGRAM [PROGRAM_ARGS argVar] [CACHE])`
- где `input` -> команда
- это команда находит путь к исполняемому файлу, вызываемому этой командой
___
### 2. Команда configure_file

```
configure_file(source destination [COPYONLY | @ONLY] [ESCAPE_QUOTES])
```
- копировать один файл немедлено (на этапе конфигурирования) и выполнить подстановку всех cmake-переменных в этом файле (в форме `${var}` или `@var@`)
- `@ONLY` - выполнить подстановки переменных только в форме `@var@`
- `COPYONLY` - не выполнять подстановки переменных
- `ESCAPE_QUOTES` - при подстановке переменных оставить кавычки экранированными

>[!note]
>Cуществует зависимость между `source` и `destination` файлами. Если модифицировать `source`-файл, то  при вызове команды `cmake --build` весь проект будет переконфигурирован пере билдом.

Существует `CONFIGURE`-форма команды string с такой же функциональностью:
- `string(CONFIGURE input outVar [@ONLY] [ESCAPE_QUOTES])`
___
### 3. Команда file

`file(RELATIVE_PATH outVar relativeToDir input)`
- конвертирование относительного пути в абсолютный

>[!note] Статья незавершена...

