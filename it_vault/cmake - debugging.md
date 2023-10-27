___
### 1. message
```cmake
message([mode] msg1 [msg2]...)
```
Вывести сообщение в стандартный вывод.
Если в команде указано несколько `msg`, то они будут выведены подряд, без пробелов.
Существуют следующие режимы вывода:

режим|тип|коментарий
-|-|-
-|важная информация|вывести как есть
`STATUS`|обычная информация|перед сообщением будет два дефиса
`WARNING`|предупреждение|будет выведено красным
`AUTHOR_WARNING`|предупреждение|будет выведено только если в командной строке было указано `-Wdev`
`SEND_ERROR`|ошибка|продолжить стадию конфигурации, но стадию генерации даже не начинать
`FATAL_ERROR`|ошибка|немедленно завершить работу
`DEPRECATION`|ошибка, предупреждение или ничего|ошибка если установлено `CMAKE_ERROR_DEPRECATED`, предупреждение если установлено `CMAKE_WARN_DEPRECATED`

>[!tip]
>Сообщения без указания режима могут быть выведены не в порядке написания относительно других сообщений, поэтому могут ввести в заблуждение.

___
### 2. variable_watch
```cmake
variable_watch(myVar [command])
```
Каждая попытка чтения или записи переменной `myVar` будет логироваться.
Если указать в качестве `command` имя функции или макроса, то он будет вызываться каждый раз при попытки чтения или записи. В качестве аргументов будут передавать все необходимые данные: имя переменной, тип доступа, значение, имя текущего файла, и даже стэк файлов.