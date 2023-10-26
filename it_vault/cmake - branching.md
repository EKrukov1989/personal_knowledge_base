___
В cmake для ветвления используется следующая конструкция:
```cmake
if(expression_1)
# commands ...
elseif(expression_2)
# commands ...
else()
# commands ...
endif()
```

___
### 1. Проверка строки

В условии может быть записана строка или же разыменованная переменная:
```
if ( something )
if ( ${var} ) 
```
В любом случае переменная будет разыменована до проверки условия, поэтому проверка сводится к проверки строки.

Логика проверки строки очень запутанная, поэтому проще выразить ее с помощью псевдокода:
```cpp
bool is_true(string);
bool is_false(string);

bool test( string val ) // псевдокод
{
	if ( val заключен в кавычки )  // после cmake 3.1
		return false;

	if is_false( val )
		return false;

	if is_true( val )
		return true;

	val = ${val}

	return !is_false( val );
}

bool is_true(string val)
{
	return ( val это число не равное нулю ||
	         val равен ON, YES, TRUE или Y );
}

bool is_false(string val)
{
	return ( val это пустая строка ||
	         val == 0 ||
	         val равен OFF, NO, FALSE, N, IGNORE, NOTFOUND ||
	         val имеет суффикс -NOTFOUND );
}
```

>[!tip]
>Совпадение строки с именем какой-то переменной настолько опасно и непредсказуемо, что лучше писать код таким образом, чтобы в условии в любом случае оказывалась одна из констант.

>[!warning]
>Могут возникнуть проблемы типа `Policy CMP0012 is not set` если не указать `cmake_minimum_required()` и вся это логика не будет работать!

___
### 2. Логические операторы

В cmake существуют стандартные операторы `AND`, `OR`, `NOT`, но они функционируют несколько не так, как в нормальных языках:
- операторы AND и OR имеют одинаковый приоритет и вычисляются слева направо
- операторы AND и OR всегда вычисляют оба выражения, левое и правое

___
### 3.  Приоритет операторов 

В условном выражении приоритеты расставляются следующим образом:
1. Круглые скобки
2. Унарные команды вроде EXISTS и DEFINED
3. Бинарные команды вроде EQUAL, LESS
4. NOT
5. AND и OR (с одинаковым приоритетом)

___
### 4. Специальные команды для условий

Проверка вхождения в список:
```
<variable|string> IN_LIST <variable>
```

Проверка на соотвествие регулярному выражению:
```
<variable|string> MATCHES <regex>
```

Проверки существования объектов cmake:
```
COMMAND <command-name> # команда или макрос или функция существует
POLICY <policy-id>     # политика существует
TARGET <target-name>   # цель существует
TEST <test-name>       # тест существует
DEFINED <name>|CACHE{<name>}|ENV{<name>  # переменная установлена
```

Проверки файлов и директорий:
```
EXISTS <path-to-file-or-directory>
IS_DIRECTORY <path>
IS_SYMLINK <path>
IS_ABSOLUTE <path>
<file1> IS_NEWER_THAN <file2>
<variable|string> PATH_EQUAL <variable|string>
```

Сравнение действительных чисел:
```
<variable|string> LESS <variable|string>
GREATER
EQUAL
LESS_EQUAL
GREATER_EQUAL
```

Сравнение строк:
```
variable|string> STRLESS <variable|string>
STRGREATER
STREQUAL
STRLESS_EQUAL
STRGREATER_EQUAL

```

Сравнение версий: `major[.minor[.patch[.tweak]]]`:
```
<variable|string> VERSION_LESS <variable|string>
VERSION_GREATER
VERSION_EQUAL
VERSION_LESS_EQUAL
VERSION_GREATER_EQUAL
```




