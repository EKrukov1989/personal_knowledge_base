___
В целом looping в cmake интуитивно понятен, поэтому в этой статье перечисленые особеннсоти синтаксиса с некоторыми замечаниями:
### 1. foreach

Цикл foreach имеет несколько форм:
```
foreach(loopVar arg1 arg2 ...)
	# ...
endforeach()
```
- loopVar будут последовательно присвоены значения `arg1 arg2 ...`

```
foreach(loopVar IN [LISTS listVar1 ...] [ITEMS item1 ...])
	# ...
endforeach()
```
- могут быть только LISTS или только ITEMS
- если указаны и LISTS и ITEMS, то  ITEMS должен быть после LISTS
- переменная списка может содержать пустой списо

```
foreach(loopVar RANGE start stop [step])
```
- loopvar будут присвоены все значения от start до stop (!включительно!)
- т.е. 

```
foreach(loopVar RANGE value)
```
- <=> `foreach(loopVar RANGE 0 value)`
- loopvar будут присвоены все значения от 0 до value (!включительно!)

___
### 2. while

```
while(condition)
	# ...
endwhile()
```

___
### 3. break & continue

В cmake break и continue работают без сюрпризов, только выглядят как функции.
Синтаксис:
```
break()
continue()
```