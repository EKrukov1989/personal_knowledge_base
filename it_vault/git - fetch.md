___
Команда `git fetch` выполняет следующее:
- забирает ссылки (ветки и/или тэги) из удаленного репозитория
- забирает из удаленного репозитория все объекты, которые необходимы чтобы воссоздать историю, и сохряняет их
- обновляет remote-tracking ветки
___
### 1. Синтаксис

`git fetch [<options>] [<repository> [<refspec>...]]`

`<refspec>` -> спецификация, которая описывает как именно выполнять fetch, подробнее ниже
`<repository>` -> удаленный репозиторий, либо URL либо имя
- Если  `repository` не указан, то используется remote из соответствующей upsream-ветки для текущей ветки.
- Если upstream-ветка не сконфигурирована, то используется origin.
___
### 2. Fetch refspec

Refspec -> спецификация, которая определяет какие ссылки забирать, а какие обновлять.

Если в командной строке refspec не указан, то ссылки, которые нужно забрать читаются из переменной `remote.<repository>.fetch`.
#### 2.1. Формат

`[+]<src>`
`[+]<src>:<dst>`
`<src>` и `<dst>` это в общем случае - паттерны.

Если ссылка в удаленном репозитории подходит к паттерну `<src>`, она забирается.
Если `<dst>` не пустой, то делается попытка обновить соответствующую локальную ссылку. Под локальной ссылкой понимается remote-tracking ветка или тэг.

Обновление будет произведено в следующих случаях:
- наличие опции `--force`
- наличие символа `+` в refspec
- возможен ли fast-forward merge ([[git - merge]])
#### 2.2. Паттерны

###### 2.2.1. Астериск

Пример: `+refs/heads/*:refs/remotes/origin/*`
Такой паттерн означает, что ветка в удаленном репозитории с именем `refs/heads/<branch>`, будет соответствовать ссылке с именем `refs/remotes/origin/<branch>`
###### 2.2.2. Карет

Если паттерн предварен символом карет (`^`), то такой refspec называется негативным.

>[!warning] 
>Ссылка считается подходящей, если она подходит хотя бы к одной refspec и не подходит ни к одной негативной refspec.

Пример: `+^refs/heads/qa/*`
Такой паттерн означает, что ссылки, которые начинаются с префикса `+^refs/heads/qa/` не следует забирать/обновлять и т.д.
##### 2.2.3. Тэги
`tag <tag>` означает то же самое, что `refs/tags/<tag>:refs/tags/<tag>`
___
### 3. Опции

`--all`
`git fetch --all [<options>]`
- забрать обновления из всех удаленных репозиториев

`--multiple`
`git fetch --multiple [<options>] [<repository>...]`
- с данной опцией можно указать несколько репозиториев, но нельзя указывать refspec

`--depth=<depth>`
- забрать историю на заданную глубину

`--deepen=<depth>`
- забрать историю на заданную глубину относительно глубины в настоящий момент

`--prune`
- перед тем как забирать обновления, удалить stale-ветки

`--prune-tags`
- перед тем как забирать обновления, удалить stale-ветки

`-t, --tags`
- забрать все тэги из удаленного репозитория

___
### 4. Pruning

По умолчанию git хранит все данные, пока их явно не потребуют удалить.

`fetch.prune true`
`remote.<name>.prune true`
- если эти опции установлены -> при fetch будет автоматечески происходить prune

>[!tip]
>Если будут возникать проблемы с накоплением избыточной информации в репозитории - его всегда можно склонировать заново.


