___
Ветка (branch) - это указатель на коммит ([[git - internals]]).
Веткой также принято называть всю цепочку родителей того коммита, на который указывает ветка.

Технически ветка представляет собой файл в `.git/refs/heads/`, который хранит только хэш-код коммита. Имя файла - это имя ветки.

Веток может быть несколько, но только одна из них может быть выбрана текущей веткой. Имя текущей ветки храниться в файле `.git/HEAD`.
Если репозиторий находится в состоянии `detached head`, то текущей ветки нет.

При коммите текущая ветка перемещается вперед.

Посмотреть список веток можно с помощью команды `git branch`. Текущая ветка будет отмечена звездочкой:
 ```
* main
  natural
  expand
```

`git branch <branchname>`
- создать ветку, которая будет указывать на текущий коммит, но не переключаться на эту ветку

`git checkout <branchname>
- переключить ветку (изменить запись в файле `HEAD`), а также привести все файлы в рабочей директории в соответствие тому коммиту, на который указывает `<branchname>`

`git checkout -B <branchname>
- создать ветку и сразу переключиться на нее

`git branch -d <branchname>`
- удалить ветку

`git reset --soft <commit>`
`git reset --soft <branch>`
- можно переключить ветку на другой коммит

`git merge <branch>`
- вмержить `<branch>` в текущую ветку

`git rebase <branch>`
- перебазировать текущую ветку на `<branch>`

filter-branch