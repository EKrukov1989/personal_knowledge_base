___
### 1. Функции

Определение функции в cmake:
```
function(name [arg1 [arg2 [...]]])
	# Function body (i.e. commands) ...
endfunction()
```
Функцию можно вызвать следующим образом: `name( [arg1 [arg2 [...]]] )`

Функция открывает свою область видимости, т.е. получает текущую копию всех переменных доступных в области видимости из которой она была вызвана. Кроме того внутри функции будут определено еще несколько переменных - аргументы и специальные переменные:
`ARGC` -> общее количество аргументов, переданных в функцию при вызове
`ARGV` -> полный список всех аргументов, переданных в функцию при вызове
`ARGN` -> список только неименованных аргументов
Можно обратиться к аргументу с номером `n` с помощью синтаксиса `ARG<n>`. Причем нумерация аргументов начинается с 1. Сначала идут именованные аргументы, а потом все остальные.

Пример:
```
function( fn argum1 argum2 )
	message(STATUS ${argum1})  # output: 1
	message(STATUS ${argum2})  # output: 2
	message(STATUS ${ARGC})    # output: 4
	message(STATUS ${ARGV})    # output: 1234
	message(STATUS ${ARGN})    # output: 34
	message(STATUS ${ARGV0})   # output: 1
	message(STATUS ${ARGV1})   # output: 2
	message(STATUS ${ARGV2})   # output: 3
	message(STATUS ${ARGV3})   # output: 4 
	message(STATUS ${ARGV4})   # output: (empty)
endfunction()
fn( 1 2 3 4 )
```

>[!warning]
>Если вызвать функцию с меньшим количеством аргументов чем указано в ее определении, то будет ошибка. Большее количество аргументов ошибкой не является.

___
### 2. cmake_parse_arguments

В cmake аргументы функции это просто список строк. Однако существует некоторое соглашение как их интрпретировать с помощью ключевых слов. Например:
```
foo( OPTION_1 OPTION_2 SINGLE_VAL_1 arg MULT_VAL_1 arg1 arg2 arg3 )
```
- `OPTION_1` и `OPTION_2` -> флаги, наличие такого аргумента означает что опция включена
- `SINGLE_VAL_1 arg` -> одно значение
- `MULT_VAL_1 arg1 arg2 arg3` -> список значений
- Аргументы должны стоять после соответствующих ключевых слов, в остальном порядок не важен
- Принято, что ключевые слова в cmake должны быть UPPERCASE + underscores.

В cmake есть встроенная функция для парсинга аргументов:
```
cmake_parse_arguments(prefix
					  noValueKeywords
					  singleValueKeywords
					  multiValueKeywords
					  argsToParse)
```
- как правило `argsToParse` == `"${ARGN}"`
- `prefix` - произвольная строка, которую выбирает программист, чтобы избежать конфликта имен, например `ARG`

Данная функция создает новые переменные для каждого `noValueKeywords`, `singleValueKeywords` и `multiValueKeywords`:
- для каждой из опций в `noValueKeywords` создается переменная: `${prefix}_optionname`. Если в аргументах упомянута данная опция, то эта переменная будет установлена в `TRUE`
- для опций в `singleValueKeywords` и `multiValueKeywords`создаются переменные, которые хранят список аргументов для соответствующего ключевого слова. Если опция не была упомянута в аргументах, то переменная будет неопределена.

Пример:
```
function(func)
	set(prefix ARG)
	set(noValues OPTION_1 OPTION_2)
	set(singleValues SVAL_1 SVAL_2)
	set(multiValues MVAL_1 MVAL_2)

	cmake_parse_arguments(
		${prefix} "${noValues}" "${singleValues}" "${multiValues}" ${ARGN} )

	# в результате применения функции cmake_parse_arguments() будут созданы следующие переменные:
	# ARG_OPTION_1 = TRUE
	# ARG_OPTION_2 = FALSE
	# ARG_SVAL_1 = x
	# ARG_MVAL_1 = a;b;c
	# Переменные ARG_SVAL_2 и ARG_MVAL_2 будут NOT DEFINED
	
endfunction()	
func(TARGET x IMAGES a b c OPTION_1)
```
___
### 3. Возвращение результатов

В cmake с помощью функции `return()` можно прервать выполнение функции и вернуть управление вызывающей функции. Однако функция `return()` не возвращает никаких значений.

Можно изменить значение переменной (или установить значение новой переменой) в области видимости, из которой функция была вызвана, с помощью опции `PARENT_SCOPE`:
```
set(varname value PARENT_SCOPE)
```

Можно предоставить пользователю функции возможность контролировать имя возвращаемой переменной с помощью следующего паттерна:
```
function(foo outvar_1 outvar_2)
	set(${outvar_1} value_1 PARENT_SCOPE)
	set(${outvar_2} value_2 PARENT_SCOPE)
endfunction()

fn( a b )
```

___
### 4. Макросы

Определение макроса в cmake:
```
macro(name arg1 arg2 ...)
	message(STATUS ${arg2})   # именно так следует обращаться к аргументу
endmacro()
```

Макрос работает практически так же как им функция, но имеет собственной области видимости, поэтому использовать его нужно очень осторожно.

___
### 5. Другие особенности использования функций

Функции можно использовать только ниже по тексту после их определения. В противном случае cmake выдаст ошибку.

Функции лучше поместить в отдельный файл с расширением `.cmake` и использовать следующим образом:

Файл utils.cmake:
```
include_guard()

function(foo arg)
	message(STATUS ${arg})
endfunction()
```

Файл CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.25)

include(utils.cmake)
foo(whatever)
```


