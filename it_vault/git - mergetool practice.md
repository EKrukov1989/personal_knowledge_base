___
Для разрешения конфликтных ситуаций при мерже рекомендуется использоваться специальные инструменты - мерджтулы.

В данной статье будет рассмотрена практика использования `vimdiff3` для разрешения конфликтных ситуаций.

___
##### Настройки

`git config --global merge.tool vimdiff3`
- использовать `vimdiff3` по умолчанию

`git config --global mergetool.keepBackup false`
- не создавать бэкап-файлы с расширением `.orig`
___
##### Процесс мержа

Допустим у нас есть две ветки, которые мы хотим смержить:
- `main` (`HEAD`)
- `expand` (которую мы хотим вмержить в `main`)

1. Вызываем команду `git merge -m <merge_comment> expand` .
2. Выясняется что существуют конфликты, процесс мержа прерывается и нам предлагается разрешить конфликты вручную
3. Вызываем команду `git mergetool`, если мы уже настроили переменную `merge.tool`, или же вызываем тул напрямую -> `git mergetool -t vimdiff3`.
4. Разрешаем конфликты во всех файлах и сохраняем их с помощью команды `:wq`
5. После разрешения конфликтов файлы будут уже добавлены в индексе и останется их только закоммитить с помощью команды `git merge --continue`
