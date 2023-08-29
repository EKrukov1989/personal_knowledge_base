___
Команда `git remote` позволяет управлять набором удаленных репозиториев.

Команда без аргументов выводит список удаленных репозиториев:
- `git remote [-vv]`

Далее описаны все субкоманды `git remote`.
Терминология описана в [[git - remotes overview]].
___
### 1. show

`git remote show [-n] <remote>`
- Команда git remote show выводит информацию о состоянии удаленного репозитория.

Если опция `-n` не указана, то перед перед выводом информации на экран команда запрашивает у удаленного репозитория информацию о всех ветках.

Примерный вывод:
```
* remote origin
  Fetch URL: git@github.com:<repository_name>.git
  Push  URL: git@github.com:<repository_name>.git
  HEAD branch: main
  Remote branches:
    main            tracked
    branch_1        tracked
    branch_2        tracked
    branch_3        new (next fetch will store in remotes/origin)
    branch_4        stale (use 'git remote prune' to remove)
  Local branches configured for 'git pull':
    main            merges with remote main
  Local refs configured for 'git push':
    main            pushes to main            (local out of date)
```

Здесь перечислены все ветки в удаленном репозитории и их статус. Есть три варианта:
- tracked -> ветка существует в удаленном репозитории, а также существует соответствующая remote-tracking ветка в локальном репозитории
- new -> ветка существует в удаленном репозитории, но не забрана в локальный
- stale -> remote-tracking ветка существует в локальном репозитории, но ее remote-ветка в удаленном репозитории уже удалена

___
### 2. Add, remove, rename

Удаленный репозиторий необязательно должен иметь что-то общее с локальным репозиторием. Добавить можно вообще любой репозиторий.

Синтаксис:
`git remote add [-t <branch>] [-m <master>] [-f] [--[no-]tags] <name> <URL>`

`git remote add <name> <URL>`
- добавить удаленный репозиторий `<url>` под именем `<name>`

`-f`
- выполнить `git fetch <name>` сразу после добавления удаленного репозитория

`--tags, --no-tags`
- импортировать/не импортировать тэги из удаленного репозитория
- по умолчанию импортируются все тэги, но только на забираемых ветках

`-t <branch>`
- следить только за указанной веткой
- может быть указано несколько таких опций

>[!note]
>Технически это реализуется с помощью файла `.git/config`. В этом файле прописываются только указанные ветки: `fetch = +refs/heads/<branch>:refs/remotes/<remote>/<branch>`
>По умолчанию - все ветки: `fetch = +refs/heads/*:refs/remotes/<remote>/*`

`git remote rename <old> <new> `
- переименовать

`git remote rm <name> `
`git remote remove <name> `
- удалить удаленный репозиторий
- все remote-tracking ветки и настройки для удаленного репозиторий будут удалены (сам репозиторий, конечно, не удаляется)

___
### 3. Set-head

`git remote set-head <name> <branch>`
- установить `<branch>` в качестве ветки по умолчанию для удаленного репозитория `<name>`
`git remote set-head -d <name>`
- отказаться от ветки по умолчанию для удаленного репозитория `<name>`

___
### 4. Set-branches

`git remote set-branches <name> <branch>...`
- установить список отслеживаемых веток
`git remote set-branches --add <name> <branch>...`
- добавить ветки в список отслеживаемых

Чтобы реализовать какую-то более сложную логику, например паттерны, лучше вручную изменить refspec.
___
### 5. Get-url, set-url

`git remote get-url <name>`
`git remote get-url --push <name>`
`git remote get-url --all <name>`
- получить URL для удаленного репозитория

`git remote set-url [--push] <name> <newurl> [<oldurl>]`
- изменить `<oldurl>` на `<newurl>`
- если `<oldurl>` не указан, то изменить первый url
- с опцией `--push` взаимодействует с push-url
`git remote set-url --add [--push] <name> <newurl>`
- добавить url
`git remote set-url --delete [--push] <name> <URL>`
- удалить все url, которые подходят к regex `<URL>`

Можно устанавливать несколько url для одного репозитория.

___
### 6. Prune

`git remote prune [-n | --dry-run] <name>...`
- удалить stale-ссылки связанные с удаленным репозиторием `<name>`
- если опция `-n` не указана, то перед перед выводом информации на экран команда запрашивает у удаленного репозитория информацию о всех ветках

___
### 7. Update

`git remote [-v | --verbose] update [-p | --prune] [<remote>...]`
- забрать обновления

>[!tip]
>Лучше использовать просто git `fetch`.
