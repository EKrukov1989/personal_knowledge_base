___
Функция string в cmake позволяет выполнять самые разыне операции над строками.
Синтаксис:
```cmake
string( ACTION args )
```
___
### 1. Элементарные действия

```
string(SUBSTRING input index length outVar)
	# если length = -1, то outVar будет содержать всю
	# правую часть строки начиная с index

string(STRIP input outVar) # удалить все whitespaces из начала и конца

string(LENGTH input outVar)

string(TOLOWER input outVar)

string(TOUPPER input outVar)
```

___
### 2. Действия посложнее

**_Action: FIND**_
```cmake
string( FIND inputString subString outVar [REVERSE] )
```
- `outVar` будет помещен индекс первого найденого вхождения, если указано `REVERSE` - последнего
- если `subString` не найден - в `outVar` будет помещено -1

**_Action: REPLACE**_
```cmake
string(REPLACE matchString replaceWith outVar input [input...])
```
- заменить все найденные `matchString` на `replaceWith` в `input` и положить результат в `outVar`

**_Action: REGEX**_
```cmake
string(REGEX MATCH regex outVar input [input...])
string(REGEX MATCHALL regex outVar input [input...])
string(REGEX REPLACE regex replaceWith outVar input [input...])
```
- строки input соединяются предварительно


