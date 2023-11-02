___
Свойство - это переменный параметр, который пользователь может читать и изменять. В отличие от переменной, свойство всегда относиться к какой-то сущности - директории, файлу, цели, тесту или кэш-переменной.

Пользователь имеет возможность создавать свои собственные свойства, но предполагается что в нормальном рабочем процессе будет достаточно свойств определенных в cmake.
___
### 1. define_property()

С помощью команды `define_property()` можно определить свойство для некоторого класса сущностей. Данная команда не устанавливает никаких значений, только определяет свойство.
```
define_property( entitySpecificType
                 PROPERTY <name> [INHERITED]
                 [BRIEF_DOCS <brief-doc> [docs...]]
                 [FULL_DOCS <full-doc> [docs...]])

Например:
define_property( SOURCE PROPERTY myprop)
```
- `entitySpecificType` -> один из следующих типов: `GLOBAL, DIRECTORY, TARGET, SOURCE, TEST, VARIABLE or CACHED_VARIABLE`
- `BRIEF_DOCS` -> краткое описание свойства
- `FULL_DOCS` -> полное описание свойства

Если указана опция `INHERITED` то свойство может наследоваться. При запросе к c помощью `get_property()` если для указанной сущности свойство неустановлено, то будет возвращено одноименное свойство для родительской директори и так далее.

Цепочка наследования следующая:
- собственно свойство для указанной сущности (`entitySpecific == DIRECTORY, TARGET, SOURCE`)
- одноименное свойство родительской директории (`entitySpecific == DIRECTORY`)
- ... все директории вплоть до корневой директории (`entitySpecific == DIRECTORY`)
- одноименное свойство для `entitySpecific == GLOBAL`

>[!note]
>Под родительской директорией понимается директория, в которой находится `CMakeLists.txt` из которого был вызван данный `CMakeLists.txt`.

___
### 2. get_property()

С помощью команды ` get_property()` можно получить значение свойства:
```
get_property(resultVar entitySpecific
		     PROPERTY propName
			 [DEFINED | SET | BRIEF_DOCS | FULL_DOCS])
```
- `entitySpecific`-> сущность, к которой относится задаваемое свойство
Возможные варианты `entitySpecific`:
```
GLOBAL
DIRECTORY dir  # если dir не указана - current dir
TARGET target1
SOURCE source1
INSTALL file1
TEST test1
CACHE cachevar1
VARIABLE
```
Данная команда может быть использована сразу для разныx задач:

**_Получение значение свойства:**_
```
get_property( resultVar entitySpecific PROPERTY propName )
```
- получить значение свойства
- указывать конкретную сущность обязательно

**_Получение значение обычной переменной:**_
```
get_property( resultVar VARIABLE PROPERTY varName )
```

**_Узнать определена ли переменная:**_
```
get_property( resultVar entitySpecific PROPERTY propName DEFINED )
```
- установить `resultVar` в `TRUE` если для данного класса сущностей определено указанное свойство (определено с помощью `define_property()`)
- Не нужно указывать конкретную сущность, нужно указать только класс.

**_Узнать установлена ли переменная:**_
```
get_property( resultVar entitySpecific PROPERTY propName SET  )
```
- установить `resultVar` в `TRUE` если для данной конкретной сущности установлено указанное свойство (т.е. у нее есть значение)
- Нужно указать конкретную сущность

**_Получение описания свойства:**_
```
get_property( resultVar entitySpecific PROPERTY propName BRIEF_DOCS )
get_property( resultVar entitySpecific PROPERTY propName FULL_DOCS  )
```
- получить краткое или полное описание свойства
- нужно указать только тип сущности

**_Получение типа кэш-переменной:**_
```
get_property( resultVar CACHE PROPERTY cachevaname TYPE )
```

>[!note]
>Важно отметить, что установленные однажды свойства необязательно должны быть видимы по всему проекту. Например свойства установленные для `DIRECTORY` - будут видны в других областях видимости, а `SOURCE` - только в той директории в которой свойство установлено.

___
### 3. set_property()

Установить свойство для одной или нескольких сущностей:
```
set_property(entitySpecific [APPEND] [APPEND_STRING]
			 PROPERTY propName [value1 [value2 [...]]])
```
- `entitySpecific` -> сущность, к которой относится задаваемое свойство
Возможные варианты `entitySpecific`:
```
GLOBAL
DIRECTORY dir  # если dir не указана - current dir
TARGET target1 target2 ...
SOURCE source1 source2 ...
INSTALL file1 file2 ...
TEST test1 test2 ...
CACHE var1 var2 ...
```

С помощью опций `APPEND` и `APPEND_STRING` можно указать тип обновления свойства:
- Если опций не указано, то по умолчанию значение будет заменено
- Если указано `APPEND`, то указанное значение будет добавлено как элемент списка
- Если указано `APPEND_STRING`, то указанное значение будет конкатенировано к уже существующему

___
### 4. get_cmake_property()

С помощью команды `get_cmake_property()` можно получить значение свойства, которое относится к классу `GLOBAL`:
`get_cmake_property(res name)` <=> `get_property(res GLOBAL PROPERTY name)`

Кроме того, есть куда более полезное применение для этой команды:
- можно получить список следующих элементов:
- `get_cmake_property(res VARIABLES)`
- `get_cmake_property(res CACHE_VARIABLES)`
- `get_cmake_property(res COMMANDS)`
- `get_cmake_property(res MACROS)`
- `get_cmake_property(res COMPONENTS)`

___
### 5. Синтаксический сахар

Cmake предлагает набор команд, которые позволяют заменить `set_property` и `get_property` в ряде случаев. Зачастую это просто синтаксический сахар.

**_DIRECTORY**_
```
set_directory_properties(PROPERTIES prop1 val1 [prop2 val2 ...])
get_directory_property(resultVar [DIRECTORY dir] property)
```

Но есть одна особенность: с помощью этой команды можно получить значение переменной в другой области видимости.
```
get_directory_property(resultVar [DIRECTORY dir] DEFINITION varName)
```
>[!tip] Лучше такой возможностью никогда не пользоваться.

**_TARGET**_
```
set_target_properties(target1 [target2...] PROPERTIES
					  propertyName1 value1
					  [propertyName2 value2] ... )

get_target_property(resultVar target propertyName)
```

**_SOURCE**_
```
set_source_files_properties(file1 [file2...] PROPERTIES
							propertyName1 value1
							[propertyName2 value2] ... )

get_source_file_property(resultVar sourceFile propertyName)
```

___
### 6. Вывод свойств

Можно вывести свойства с помощью встроенного модуля `CMakePrintHelpers`:
```
include(CMakePrintHelpers) # подключаем модуль

cmake_print_properties([TARGETS target1 [target2...]]
	[SOURCES source1 [source2...]]
	[DIRECTORIES dir1 [dir2...]]
	[TESTS test1 [test2...]]
	[CACHE_ENTRIES var1 [var2...]]
	PROPERTIES property1 [property2...]
```
