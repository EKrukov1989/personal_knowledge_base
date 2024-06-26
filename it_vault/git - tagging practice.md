___
В данной статье поверхностно рассмотрены все основные вопросы работы с тэгами.

Краткая справка:
- Тэг это ссылка на какой-либо объект, но обычно - на коммит.
- Есть два типа тэгов: легкие и аннотированные.
- Легкий тэг - это в сущности ссылка и имеет только имя.
- Аннотированный тэг - это ссылка на специальный объект тэга, который уже в свою очередь ссылается на объект ([[git - internals]]).
___
### 1. Шпаргалка

команда | описание
-|-
`git tag`|вывести список тэгов
`git tag -l`|вывести список тэгов
`git tag -list`|вывести список тэгов
`git tag -l <regex>`|вывести список тэгов
`git show <tag_name>`|вывести информацию о тэге
`git tag <tag_name>`|создание легкого тэга
`git tag -a <name> -m <msg>`|создание аннотированного тэга
`git tag <name> <objhash>`|создание тэга для заданного объекта
`git push <remote> <name>`|отправить тэг на `<remote>`
`git push <remote> --tags`|отправить все тэги на `<remote>` 
`git push <remote> --follow-tags`|отправить все аннотированные тэги на `<remote>` 
`git tag -d <name>`|удалить тэг
`git push <remote> -d <name>`|удалить тэг из удаленного репозитория

___
### 2. Просмотр тэгов

По умолчанию команда git log декорирует коммиты именами следующих ссылок:
- `HEAD`
- `refs/heads/`
- `refs/remotes/`
- `refs/stash/`
- `refs/tags/`

Тэги могут сильно загромождать обзор, чтобы убрать тэги можно воспользоваться опциями:
- `--decorate-refs=<pattern>`
- `--decorate-refs-exclude=<pattern>`
Например, вывести лог без тэгов можно с помощью команды:
`git log --oneline --graph --all --decorate-refs-exclude=refs/tags/`

По умолчанию команда `git log` показывает только ветку под `HEAD`. Можно вывести ветки под всеми тэгами с помощью команды:
`git log --oneline --graph --tags`

Вывести ветки под конкретными тэгами можно следующим образом:
`git log --oneline --graph --tags=<glob_pattern>`

>[!note]
>Здесь есть особые правила для globbing паттернов:
>- Чтобы указать конкретный тэг нужно добавить астериск в начало:  `*<tag_name>`
>- Если в паттерне не указано никаких wildcard-символов, то к нему неявно добавляется `/*`. Т.е. паттерн `qa` будет подходит к `qa/f1`, `qa/f1`, но не будет подходить к `qa`.

Ну или можно чекаутить тэг, а затем вывести его ветку:
```bash
git checkout <tag_name>
git log --oneline --graph
```

___
### 3. Tag fetch

По умолчанию забирается любой тэг, который указывает на забираемые коммиты. Т.е. если мы обычно выкачиваем всего несколько веток - то все тэги на этих ветках будут забраны, что обычно и требуется.

Скачать ВСЕ тэги в репозитории можно с помощью команды:
- `git fetch --tags`

Удаленный репозиторий может содержать немеренное количество тэгов, причему на всех ветках, поэтому выполнение такой команды повлечет за собой скачивание всех веток. Лучше действовать избирательно.

Можно указать в конфиге, какие тэги скачивать. Например:
- `fetch = +refs/tags/qa/*:refs/tags/qa/*`

Или если нужно скачать определенный тэг, то это можно сделать явно:
- `git fetch origin refs/tags/<tag_name>:refs/tags/<tag_name>`
