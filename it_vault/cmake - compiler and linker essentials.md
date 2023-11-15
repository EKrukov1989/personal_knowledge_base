___
Cmake-скрипты содержат описание проекта, которое не зависит от конкретного сборочного инструмента, а также компиляторов и линкеров. 

Например команда `target_link_libraries(target PUBLIC lib)` указывает, что нужно определенным образом связать `target` с указанной `lib`, но не указывает какие именно флаги следует использовать линкеру.

На этапе конфигурирования пользователь выберет генератор проектных файлов и cmake сможет на основе cmake-скриптов создать проектные файлы, в которых будут указаны настоящие команды к линкеру со всеми флагами.

Для управления компилятором и линкером cmake предоставляет два механизма:
1. Переменные: `CMAKE_<LANG>_FLAGS`, `CMAKE_<LANG>_FLAGS_<CONFIG>`, `...`
2. Свойства: `INCLUDE_DIRECTORIES`, `COMPILE_DEFINITIONS`, `...`
3. Функции: `target_link_libraries()`, `target_include_directories()`, `...`

Управление компилятором и линкером в конечном итоге все равно происходит через переменные и свойства, функции в данном случае - это всего лишь более удобный способ управления свойствами.
___
### 1. Свойства компилятора

Существуют следующие свойства `TARGET`-свойства для управления компилятором. Также есть одноименные `DIRECTORY`-свойства; их влияние описано отдельной для каждого из свойств.

`INCLUDE_DIRECTORIES`
- список абсолютных директорий, которые будут использованы как пути для поиска header-файлов. Аналог опции `-I` в gcc.
- изначальное значение свойства наследуется от свойства директории

`COMPILE_DEFINITIONS`
- список определений для компилятора в форме `VAR` или `VAR=VALUE` (без префиксов `-D` или `/D`)
- итоговый список определений для компилятора будет результатом конкатенации  свойства цели и одноименного свойства директории

`COMPILE_OPTIONS`
- любой другой флаг компилятора, кроме `-I` и `-D` (то есть перечисленных выше) должен быть добавлен к этому свойству
- изначальное значение свойства наследуется от свойства директории

`INTERFACE_INCLUDE_DIRECTORIES`
`INTERFACE_COMPILE_DEFINITIONS`
`INTERFACE_COMPILE_OPTIONS`
- данные свойства применяются не к цели, а ко всем целям, которые линкуются непосредственно к ней
- изначально все эти свойства пусты

Все эти свойства поддерживают генераторные выражения.
Есть еще одноименные `SOURCE`-свойства.

>[!warning]
>`TARGET`-свойство наследует `DIRECTORY`-свойство в момент создания цели, например в момент исполнения команды `add_executable()`!

___
### 2. Свойства линкера

`LINK_LIBRARIES`
- список всех библиотек, к которым цель должна прилинковаться непосредственно
- может содержать пути, имя библиотеки (без префиксов и суффиксов), имя cmake-цели
- изначально список пустой

`LINK_FLAGS`
- флаги для линкера 
- изначально список пустой

`STATIC_LIBRARY_FLAGS`
- флаги для архиватора, используется только для статических библиотек

`INTERFACE_LINK_LIBRARIES`
- данное свойство применяется не к цели, а ко всем целям, которые линкуются непосредственно к ней
- изначально пусто

`LINK_FLAGS` и `STATIC_LIBRARY_FLAGS` не поддерживают генераторные выражения. Чтобы специфицировать логику в зависимости от конфигурации нужно использовать свойства `LINK_FLAGS_<CONFIG>` и `STATIC_LIBRARY_FLAGS_<CONFIG>`.

___
### 3. Команды для управления свойствами

Для управления свойствами рекомендуется использовать специальные команды:
```cmake
target_link_libraries(targetName
	<PRIVATE|PUBLIC|INTERFACE> item1 [item2 ...]
	[<PRIVATE|PUBLIC|INTERFACE> item3 [item4 ...]]
	...)

target_include_directories(targetName [BEFORE]
	<PRIVATE|PUBLIC|INTERFACE> dirs
	...)

target_compile_definitions(targetName
	<PRIVATE|PUBLIC|INTERFACE> items
	...)

target_compile_options(targetName [BEFORE]
	<PRIVATE|PUBLIC|INTERFACE> items
	...)
```

В каждой из команд нужно обязательно использовать одно из следующих ключевых слов: `<PRIVATE|PUBLIC|INTERFACE>`. Значения этих слов следующие:
- `PRIVATE` -> элементы, перечисленные после этого ключевого слова, оказывают влияние только на `targetName`
- `INTERFACE` -> оказывают влияние только на цели, прилинкованные к `targetName`
- `PUBLIC` -> оказывает влияние на цели и прилинкованные к ней цели

Технически это работает следующим образом:
- `PRIVATE` изменяет только свойства без префикса `INTERFACE_`, `PUBLIC` - свойства с префиксом `INTERFACE_`, а `PUBLIC` - и все.

В целом значение команд интуитивно понятно. Следует выделить только следующие замечания:
- `BEFORE` - добавить элементы в начало списка
- В отличие от свойства `INCLUDE_DIRECTORIES`, в команде `target_include_directories` можно указывать относительные пути (относительно директории текущего `CMakeLists.txt`)
___
### 4. Устаревшие команды

Существует ряд устаревших команд, которые не рекомендуется использовать:
```
include_directories()
add_definitions()
remove_definitions()
add_compile_definitions()
add_compile_options()
link_libraries()
link_directories()
```
Рассматривать их нет смысла.
___
### 5. Переменные

Можно устанавливать флаги линкера и компилятора с помощью переменных. Отличие переменных от свойств, заключается в том, что они действуют сразу на все цели.

Существуют следующие переменные:
- `CMAKE_<LANG>_FLAGS`
- `CMAKE_<LANG>_FLAGS_<CONFIG>`
- `CMAKE_<TARGETTYPE>_LINKER_FLAGS`
- `CMAKE_<TARGETTYPE>_LINKER_FLAGS_<CONFIG>`

`<TARGETTYPE>` может принимать одно из следующих значений:
- `EXE`, `SHARED`, `STATIC`, `MODULE`

>[!warning] АХТУНГ!
>При создании цели будут использованы значения переменных, которые получены на момент завершения чтения соответствующего `CMakeLists.txt`, а не на момент вызова команды `add_<targettype>()`. Соответственно имеет смысл устанавливать эти переменные в конце файла.

___
### 6. Пример

 Допустим у нас имеется такой cmake-файл:
```
cmake_minimum_required(VERSION 3.25)
project(App LANGUAGES CXX)
include(CMakePrintHelpers)

add_executable(app main.cpp)

target_compile_definitions(app PUBLIC mydef1)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Dmydef2")

cmake_print_properties(TARGETS app PROPERTIES COMPILE_DEFINITIONS)
cmake_print_properties(TARGETS app PROPERTIES INTERFACE_COMPILE_DEFINITIONS)
cmake_print_variables( CMAKE_CXX_FLAGS )
```

Bывод при конфигурировании:
```
 Properties for TARGET app:
   app.COMPILE_DEFINITIONS = "mydef1"
 Properties for TARGET app:
   app.INTERFACE_COMPILE_DEFINITIONS = "mydef1"
-- CMAKE_CXX_FLAGS=" -Dmydef2"
-- Configuring done
-- Generating done
```

Если теперь выполнить команду `make VERBOSE=1`, то можно получить список команд, которые будут выполнены. В данном случае:
```
/usr/bin/c++ -Dmydef1 -Dmydef2 -MD -MT CMakeFiles/app.dir/main.cpp.o \
	-MF CMakeFiles/app.dir/main.cpp.o.d \
	-o CMakeFiles/app.dir/main.cpp.o -c \
	<workdir>/main.cpp

/usr/bin/c++ -Dmydef2 CMakeFiles/app.dir/main.cpp.o -o app 
```

>[!warning]
>`CMAKE_<LANG>_FLAGS` передаются как в компилятор, так и в линкер!
