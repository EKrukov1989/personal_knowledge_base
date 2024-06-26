Команда ls выводит информацию о файлах в текущей директории.

### Содержимое long-listing таблицы
С помощью команды `ls -l` мы получаем таблицу со следующим содержимым:

№|пример|пояснение
-|-|-
1|-rwxr-xr-x|[[linux - access permissions]]
2|1|количество хардлинков на файл или директорию
3|gene|пользователь
4|gene|группа
5|4096|размер в килобайтах
6|Apr|месяц
7|25|число
8|2017 или 13:08|год или время, если год текущий
9|'vim tutorial.pdf'|имя файла

### Опции
`-a`  -> вывести все файлы, в том числе начинающиеся с точки
`-A`  -> то же, что `-a`, но не выводить файлы `.` и `..`
`-1`  -> выводить по одному файлу в строке
`--author`  -> вывести автора (вместе с опцией `-l`)
`-h`  -> человекочитаемый размер файлов (вместе с опцией `-l`)
`-l`  -> использовать формат для длинного списка
`-n`  -> то же, но вместо имен пользователей и групп - идентификаторы
`-o`  -> то же, но вообще без информации о группе
`-g` -> то же, но без информации о пользователе
`-G` -> без информации о группах
`-N`  -> вывести имена файлов без кавычек
`-R`  -> вывести поддиректории рекурсивно
`-L`  -> для символической ссылки показывать информацию о файле
`--group-directories-first` - сначала вывести директории


>[!note]
>В будущем можно написать свою команду, чтобы вырезать информацию о времени изменения файла, которая обычно не нужна

### Псевдонимы

Итого обычно я бы хотел видеть следующий вывод:
`alias brls="ls -1AN --group-directories-first"` (краткий вывод)
`alias fuls="ls -AhLN --group-directories-first"` (полный вывод)