___
Перед прочтением данной статьи рекомендуется ознакомиться с понятиями WD, repo и SVC здесь: [[git - SVC terminology]]

___
### 1. Пять состояний файлов

Прежде чем приступать к подготовке коммита нужно знать, какие существуют состояния файлов в git.  Все файлы внутри WD находятся в одном из трех состояний:
- ignored
- untracked
- tracked

Причем tracked файлы также имеют три состояния:
- commited (unmodified)
- modified
- staged

Таким образом всего существует пять состояний:
##### 1.1. Untracked

Файл, находящийся в рабочей директории, но не добавленный в  систему контроля версий.
Примечания:
- Например, это может быть только что созданный файл.
- Такие файлы перечисляются  в `git status` как `Untracked files`.
- Такие файлы не попадут в коммит
##### 1.2. Ignored

Файл, находящийся в рабочей директории, не добавленный в  систему контроля версий и более того - указанный в файлах `gitignore` как подлежащий игнорировнию.
Примечания:
- Такие файлы не отображаются в `git status` (если нет опции `--ignored`)
- Такие файлы не попадут в коммит
##### 1.3. Commited

Файл, добавленный в систему контроля версий, причем его текущее состояние никак не отличается от состояния в выбранной версии.
Примечания:
- Такие файлы никак не отображаются в `git status`
- При нормальной работе большинство файлов находятся именно в этом состоянии
- После совершения коммита staged файлы переходят в состояние commited
##### 1.4. Modified

Отслеживаемый файл, состояние которого отличается от состояния в выбранной версии, а также от стэйджед версии. Например:
- существующий файл был изменен -> он автоматически modified
- существующий файл был удален -> он автоматически modified
- измененный staged файл также становитя modified
Примечания:
- Такие файлы перечисляются  в `git status` как `Changes not staged for commit`.
- При выполнении команды `git commit` такие файлы не попадут в коммит, только при выполненнии команды `git commit -a`.
##### 1.5. Staged

Untracked или modifed файл, который был намерено добавлен пользователем в индекс с помощью команды `add`.
При добавлении файла в индекс, его текущее состояние сохраняется и кроме того он вносится в список staged файлов.
Примечания:
- Такие файлы перечисляются  в `git status` как `Changes to be committed`.
- При выполнении команды `git commit` такие файлы попадут в коммит
- staged файл должен отличаться от соответствующего файла в выранной версии
- если в WD изменить staged файл, то он будет одновременно modified и staged, причем staged версия файла - не изменится

>[!note]
>Файлы с одинаковыми именами могут одновременно находится в состоянии modifed и staged. Но это всего лишь файлы с одинаковыми именами, но содержимое у них отличается.

>[!note]
>Файл может одновременно находится в состоянии staged и untracked, например если удалить закомиченный файл командой `git rm --cached`.

>[!note]
>Такой категории как "удаленный" файл нет. Удаленный файл может быть modified или staged, но не может быть untracked или commited.

___
### 2. Изменения состояний

Рассмотрим доступные способы изменения состояний файлов. Рекомендуется предварительно ознакомиться с командой: [[git - status]].

Всего существует пять состояний, но ignored файлы не смысла рассматривать, поэтому ограничимся четырьмя.

Перемещения между ними происходят с помощью команд:
[[git - add rm mv]]
[[git - restore]]
[[git - commit]]

Все возможные перемещения изображены на следующей схеме:
![[git_filestates_scheme 1.png]]


