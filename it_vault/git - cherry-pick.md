___
Команда `git cherry-pick` применяет изменения из одного или более коммитов к текущей ветке.

По механизму работы `cherry-pick` похож на `merge` и `rebase`, но применяется только к выбранным коммитам.

В этой статье будут рассмотрены только особенности `cherry-pick`.
___
### Синтаксис

- `git cherry-pick [options] <commit>...​`
- `git cherry-pick (--continue | --skip | --abort | --quit)`

>[!warning] Перед выполнением cherry-pick рабочее дерево должно быть чистым!

___
### Опции

`-e, --edit`
- можно отредактировать сообщение коммита

`-n, --no-commit`
- не создавать коммит, применить все изменения к индексу

`-x`
- при создании коммита добавить к коммиту строку `cherry picked from commit <original_commit>`

___
### Примеры

`git cherry-pick f0ea4ac`
`git cherry-pick aux_branch`
`git cherry-pick aux_branch~4 aux_branch~2` 

Способы задания коммитов и диапазонов коммитов: [[git - revisions]]