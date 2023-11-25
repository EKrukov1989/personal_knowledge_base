___
### 1. Создание ПЗФ

Для создания предкомпилируемых заголовочных файлов (ПЗФ) в cmake существует следующая команда:
```
target_precompile_headers(<target>
	<INTERFACE|PUBLIC|PRIVATE> [header1...]
	[<INTERFACE|PUBLIC|PRIVATE> [header2...] ...])
```

Данная команда создает для цели `target` ПЗФ из перечисленных заголовочных файлов. Файлы можно указывать следующим образом:
- `abc/project_header.h` -> такой хедер указывается относительно текущей директории, то есть `CMAKE_CURRENT_SOURCE_DIR`
- `[["abc/other_header.h"]]`, `<unordered_map>` -> поиск таких хедеров будет выполнен так же как для файлов, указанных с помощью директивы `#include` (квадратные скобки нужны, чтобы cmake верно трактовал кавычки)

ПЗФ будут созданы в папке `<build>/CMakeFiles/app.dir` и будут иметь имена вроде `cmake_<headername>xx.gch`. 

ПЗФ будут включены во все исходные файлы цели. Их не нужно включать с помощью директивы `#include`.

Если в какой-то из исходных файлов цели не нужно добавлять ПЗФ в определенный исходный файл цели можно использовать свойство файла `SKIP_PRECOMPILE_HEADERS`.
___
### 2. Переиспользование ПЗФ

К сожалению, созданные таким образом ПЗФ будут использоваться только в этой же цели. Чтобы переиспользовать ПЗФ в другой цели нужно использовать следующую команду:
```
target_precompile_headers(<target> REUSE_FROM <other_target>)
```
- В результате этой команды для `<target>` добавляется еще одна зависимость, теперь `<target>` будет зависеть от `<other_target>`.

Получается, что в проекте нужно выбрать одну какую-то цель и привязать к ней почти все остальные цели с помощью этой команды.

>[!note]
>Возможно в реальных проектах, чтобы не писать для каждой цели `target_precompile_headers` используются свойства `PRECOMPILE_HEADERS` и `INTERFACE_PRECOMPILE_HEADERS`, которые задаются для директории и потом наследуются целями.

___
### 3. Пример

Структура файлов проекта:
```
build
	...
main.cpp
pch.h
CMakeLists.txt
```

Файл main.cpp:
```cpp
// включать хедер pch.h не нужно!
int main()
{
	std::cout << "hello-world" << std::endl;
	return 0;
}
```

Файл pch.h:
```cpp
#pragma once
#include <iostream>
```

Файл CMakeLists.txt:
```cmake
cmake_minimum_required( VERSION 3.25 )
project( App LANGUAGES CXX )  
add_executable( app main.cpp )
target_precompile_headers( app PRIVATE pch.h )
```


