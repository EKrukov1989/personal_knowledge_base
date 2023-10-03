___
В этой статье собраны разрозненные заметки, которые не попали в другие статьи.

___
`shell <command string>`
`!<command string>` 
- выполнить bash-команду не покидая gdb

___
### Директории исходного кода

gdb содержит список директорий для поиска файлов - `source path`. Когда gdb требуется исходный файл, то он ищет его в `source path`.
`source path` всегда содержит две директории:
- директорию компиляции (`$cdir`), если указан в отладочной информации
- текущую рабочую директорию (`$cwdir`)

`show directories`
- вывести `source path`
`dir dirname ...`
- добавить директории в `source path`
`directory`
- установить исходный список `source path`