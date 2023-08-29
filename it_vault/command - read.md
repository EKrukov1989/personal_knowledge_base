___
read - это shell-builtin. По умолчанию читает из стандартного ввода пока не достигнет конца строки.

___
### 1. Варианты применения

`read`
Сохраняет ввод в переменную оболочки `REPLY`.

`read var`
Сохраняет ввод в переменную `var`.

`read var1 var2`
Сохраняет ввод до первого пробела в переменную `var1`, оставшийся ввод - в переменную `var2`. Пример:
```
read var1 var2
ввод:123   456   789

результат: 
var1=123
var2=456   789
```

___
### 2. Опции

По умолчанию команда read воспринимает обратный слэш как указание для line continuation. Что удобно, когда необходимо ввести длинный текст, однако не дает использовать escape-последовательности.

`-r`
Интерпретировать escape-последовательности буквально, например ввод `abc\ndef` с этой опцией будет прочитан как `abc\ndef`.

`-p`
Вывести подсказку перед запросом пользовательского ввода. Пример:
`read -p "enter your username:" username_var`

`-a`
Сохранить пользовательский ввод в массив. Пример:
`read -a arr_var`

___
### 3. Пример построчного чтения файла

С помощью следующего кода можно прочитать файл построчно:
```bash
while read # no name supplied so the variable REPLY is used
do
	# do something with "$REPLY" here
done < filename.txt
```

___
### 3. Пример чтения таблицы

Допустим у нас есть comma-separated файл, в котором хранится нечто вроде таблицы:
```text
Ross,Geller,555-8818
Rachel,Green,555-1234,some_comment
Chandler,Bink,555-7531
```
Мы хотим прочитать из этой таблицы второй и третий столбцы. Мы можем реализовать это следующим образом:
```bash
while IFS="," read a surname phone c
do
    printf "surname: %-10s phone: %s\n" $surname $phone
done < filename
```
```text
# вывод
surname: Geller     phone: 555-8818
surname: Green      phone: 555-1234
surname: Bink       phone: 555-7531
```
