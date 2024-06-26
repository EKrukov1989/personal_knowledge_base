___
Команда diff позволяет посмотреть различия между коммитами, деревьями и так далее.

___
### 1. Варианты применения

`git diff [<path>]`
- посмотреть различия в рабочем дерево относительно индекса

`git diff --no-index <path> <path>`
- посмотреть различия между двумя файлами
`git diff [<options>] <blob> <blob>`
- посмотреть различия между двумя блобами

`git diff <commit> [<path>]`
- посмотреть различия между рабочим деревом и коммитом. По умолчанию - используется текущий коммит

`git diff --staged [<commit>] [<path>]`
- посмотреть различия между индексом и коммитом. По умолчанию - используется текущий коммит,  то есть команда выводит - застейдженные изменения
- можно использовать `--cached` вместо `--staged`

`git diff [<options>] <commit> <commit> [<path>]`
- посмотреть различия между двумя коммитами

___
### 2. Как читать diff

В файл `text`, который содержал слово `hello`, была добавлена еще одна строчка со словом `world`. Дифф выглядит вот так:

```text
diff --git a/text b/text          входные данные для команды
index ce01362..94954ab 100644     метаданные
--- a/text                        маркер изменений для файла: -
+++ b/text                        маркер изменений для файла: +
	@@ -1 +1,2 @@                 chunk header
 hello                            <- chunk
+world                            <- chunk  
```

Chunk header в общем случае содержит четыре числа:
`@@ -c,d +e,f @@`
Эти числа значат, что в следующем фрагменте представлена `d` строка из файла `a`, начиная со `c`-ой строки и `f` строки из файла `b`, начиная с `e`-ой строки.

В дальнейшем фрагменте перечислены строки из файла со следующими префиксами:
- `-` -> строка из файла `a`
- `+` -> строка из файла `b`
- `без префикса` -> строка одинаковая для файлов `a` и `b`