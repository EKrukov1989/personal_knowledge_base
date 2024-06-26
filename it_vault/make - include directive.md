___
Директива `include` заставляет `make` прервать чтение текущего `makefile` и прочитать один или более `makefiles` перед тем как продолжить.
```make
include filenames...
```

После включения файлов весь полученный текст воспринимается как один makefile:
- если первый найденная цель во включенном файле - она станет default goal
- включенные файлы не имеют какого-то набора своих локальных переменных, все переменные общие
- пути все общие, относительно текущей директории
___
### 1. Переменная окружения MAKEFILES

Если переменная окружения `MAKEFILES` установлена, то `make` включит все файлы указанные в этой переменной перед тем, как читать какие-либо другие. Причем:
- `make` никогда не устанавливает `default goal` из этих файлов;
- если какой-либо файл из `MAKEFILES` не найден - это не ошибка

___
### 2. Особенности поиска makefiles

В директиве `include` вместо `filenames` может содержать глоббинг-паттерны.

Если имя файла не абсолютное, то его поиск будет осуществляться:
- сначала в текущей директории
- в директориях, которые указаны с помощью опций `-I`, `--include-dir`
- prefix/include (обычно `/usr/local/include` ) `/usr/gnu/include`, `/usr/local/include`, `/usr/include`

Некоторые включенные `makefiles` сами могут быть `targets`, поэтому если включенный `makefile` не найден - это не сразу фатальная ошибка. `make` продолжит исполнять текущий файл дальше, а потом попробует пересоздать или обновить неактуальные `makefiles`.

Следующие директивы просто пропустит включенные файлы, если они не найдены:
```make
-include filenames...
sinclude filenames... (alternative syntax)
```


