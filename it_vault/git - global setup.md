___
В этой статье должны будут перечислены все действия по установке и настройке git.
___
### 1. Installation

`sudo apt install git-all`

___
### 2. Global config

Обязательные настройки имени и почты:
```
`git config --global user.name "John Doe"`
`git config --global user.email johndoe@example.com`
```

Отключить предупреждение при переключении в detached head состояние:
`git config --global advice.detachedHead false`

Отключить предупреждение при инициализации репозитория:
`git config --global init.defaultBranch main`

Не выводить полные пути к ссылкам в логах (можно понять что за ссылка по цвету):
`git config --global log.decorate short`

Настройки диффа:
`git config --global diff.tool meld`

Показывать исходный вариант при мерже:
`git config --global merge.conflictStyle diff3`

Настройки мерджа:
`git config --global merge.tool vimdiff3`
`git config --global mergetool.keepBackup false`

Всегда использовать refspec при пуше:
`git config --global push.default nothing`

___
### 3. Aliases

```bash
git config --global alias.flog 'log --oneline --graph --all'
git config --global alias.co 'checkout'
git config --global alias.comqm 'commit -q -m'
git config --global alias.sts 'status -s'
git config --global alias.dt 'difftool -y'
git config --global alias.dts 'difftool -y --staged'
```

___
### 4. Global gitignore
