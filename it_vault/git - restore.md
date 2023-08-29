___
Команда `git restore` позволяет восcтанавливать состояние файла.

Чтобы восстановить файл `git` должен получить следующую информацию:
- пути файлов, которые следует восстановить
- источник восстановления
- локация восстановления (которую следует восстановить)

Синтаксис: `git restore [<options>] <pathspec>...`

Набор файлов, которые следует восстановить определяется исходя из `pathspec`, которая может содержать как имена файлов, так и глоббинг-патерны.

Возможные локации восстановления (restore location):
- worktree ( `-W, --worktree`, но это вариант по умолчанию)
- staged area ( `-S, --staged` )
- worktree и staged area ( `-S -W` )

Источник восстановления определяется с помощью опции:
`-s <tree>, --source=<tree>`
Можно указать коммит, бранч или тэг.
Если ничего не указано то:
- если указана опция `--staged`: источником восстановления будет HEAD
- в противном случае источником восстановления будет индекс

Примеры:
Допустим у нас есть файл `f.txt`, который сейчас имеет следующее состояние:
```
f.txt:   commited: aaa   staged: bbb   workdir: ccc
```
Вот какой результат будет получен после выполнения следующих команд:
```
git restore f.txt ( restore workdir from index )
f.txt:   commited: aaa   staged: bbb   workdir: bbb

git restore --staged f.txt ( restore staged from HEAD )
f.txt:   commited: aaa   staged: aaa   workdir: ccc

git restore -S -W f.txt ( restore staged and worktree from HEAD )
f.txt:   commited: aaa   staged: aaa   workdir: aaa

git restore -s HEAD f.txt ( restore worktree from HEAD )
f.txt:   commited: aaa   staged: aaa   workdir: aaa
```

___
### Памятка

Описание|команда
-|-
Восстановить рабочее дерево из индекса | `git restore <file>`
Восстановить в индексе из HEAD |`git restore --staged <file>`
Восстановить рабочее дерево из HEAD |`git restore -s HEAD <file>`

Следующие команды эквивалентны (кроме того, что INDEX - нельзя использовать):
`git restore --worktree --source=INDEX <file>` <-> `git restore <file>`
`git restore --staged   --source=HEAD  <file>` <-> `git restore --staged <file>`
`git restore --worktree --source=HEAD  <file>` <-> `git restore -s HEAD <file>`
