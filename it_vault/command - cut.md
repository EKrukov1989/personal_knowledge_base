___
>[!warning] Cтатья содержит только один пример использования команды cut

Допустим у нас есть comma-separated файл, в котором хранится нечто вроде таблицы:
```text
Ross,Geller,555-8818
Rachel,Green,555-1234,some_comment
Chandler,Bink,555-7531
```

Можно извлечь данные из таблицы с помощью команды `cut`:
```bash
cut -d<separator> -f<range> filename
# <separator> - один символ
# <range> - столбцы, которые следует вывести. Пример: 2 2,3 2-4
# пример использования команды:

cut -d, -f1,3 filename
# вывод:
Ross,555-8818
Rachel,555-1234
Chandler,555-7531
```

