___
Команда `df`  выводит сводку об использовании дискового пространства в файловой системе. Команда выводит следующую информацию:
- объем файловой системы
- использованный объем
- доступный объем
- процент использования
- точка монтирования
##### Синтаксис:

```
df [OPTION]... [FILE]...
```
##### Варианты использования:

- `df` -> показать информацию обо всех файловых системах, которые примонтированы на данный момент
- `df filename` -> показать информацию о файловой системе, которая содержит указанный `filename`
#### Опции:

`-h`
- (human-readable) -> показать размер в мегабайтах или гигабайтах
`-T`
- показать тип файловой системы

