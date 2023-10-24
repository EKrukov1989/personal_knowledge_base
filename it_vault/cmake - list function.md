___
В cmake списки это обычные строки, в которых элементы разеделены точками с запятой. Для работы со списками существует функция list:

```cmake
list(ACTION listVar args)
```
- второй аргумент всегда список, причем именно переменная-список

Действия:
```
list(LENGTH listVar outVar)
list(GET listVar index [index...] outVar)
list(APPEND listVar item [item...])
list(INSERT listVar index item [item...])
list(FIND myList value outVar)
list(REMOVE_ITEM myList value [value...])
list(REMOVE_AT myList index [index...])
list(REMOVE_DUPLICATES myList)
list(REVERSE myList)
list(SORT myList)
```

 Если указать отрицательный индекс, то отсчет элементов будет вестись от конца списка.