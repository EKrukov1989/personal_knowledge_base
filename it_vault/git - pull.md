___
Команда `git pull` позволяет забрать (fetch) и интегрировать в локальную ветку.

### 1. Синтаксис:

`git pull [<options>] [<repository> [<refspec>...]]`

Команда `git pull` действует следующим образом:
- вызывает `git fetch`
- затем вызывает либо `git rebase` либо `git merge` в зависимости от настроек

Возможны три ситуации:
```
1) aux is behind the remote

A---B---C---D---E master
		^
		aux

2) aux is ahead the remote

A---B---C---D---E aux
		^
		master

3) aux and master have diverged

		  F---G master
		 /
A---B---C---D---E aux
```

- Если текущая ветка позади remote-ветки, то по умолчанию будет произведен fast-forward merge.
- Если текущая ветка впереди remote-ветки, то ничего делать не нужно.
- Наиболее сложная ситуация - когда ветки разветвились (diverged). Ее следует рассмотреть отдельно.
___
### 2. Diverged-ветки

В случае с разветвленными-ветками есть два варианта развития событий:
- будет вызван `merge`
- будет вызван `rebase`

В первую очередь рассматриваются флаги:
`-r, --rebase[=false|true|merges|interactive]` 
`--no-rebase` <-> `--rebase=false`
- если true ->  rebase
- если merges -> `rebase --rebase-merges`

Далее учитывается переменная `branch.<name>.rebase`.
При отсутствии всего вышеперечисленного рассматривается переменная `pull.rebase`.

Есть также переменные `branch.autoSetupRebase` и `branch.autoSetupMerge`, чтобы автоматически конфигурировать upstream-ветку...

___
Так как я не собираюсь пользоваться `git pull`, то нет смысла более подробно рассматривать эту команду.

>[!tip] Никогда не использовать `pull`, только `fetch`!