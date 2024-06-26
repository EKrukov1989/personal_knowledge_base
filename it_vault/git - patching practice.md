___
Практика по созданию и применению патчей.
##### 1. Создание патча

Создать патч можно с помощью команды дифф ([[git - diff]]):

`git diff [--patch] > patch_file`

(данная команда выводит все различия между рабочим деревом и индексом)

Опция `--patch` необязательно, так как `git diff` выводит информацию в нужном формает по умолчанию.

`patch_file` в результате должен содержать нечто следующее:
```
diff --git a/abc b/abc
index 2ef4533..807c5ba 100644
--- a/abc
+++ b/abc
@@ -3,3 +3,4 @@ bobby
 carol
 denis
 elona
+frank
```

##### 2. Применение патча

Нужно обязательно перейти в корень рабочей директории.

Можно сначала проверить патч (в случае успеха ничего не будет выведено):
`git apply --check patch_file`

Применить патч (изменения будет применены к рабочему дереву и индексу):
`git apply --index patch_file`

Можно сразу коммитить:
`git commit -q -m patch`
