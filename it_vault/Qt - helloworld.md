___
Пример простейшей программы с помощью библиотек Qt.

>[!note]
>Предварительно Qt был установлен в директорию `home/gene/Qt`.

___
### 1. С помощью Make

Makefile:
```Make
qt_dir = "/home/gene/Qt/6.6.1/gcc_64"

app : main.cpp pch.h.gch
	g++ -o app \
	-L"${qt_dir}/lib" \
	main.cpp \
	-lQt6Widgets \
	-lQt6Core \
	-Wl,-rpath="${qt_dir}/lib"

pch.h.gch : pch.h
	g++ -o pch.h.gch pch.h -I "${qt_dir}/include"
```
main.cpp:
```cpp
#include "pch.h"

int main(int argc, char** argv)
{
	QApplication app(argc, argv);
	QLabel lbl("Hello, World" ) ;
	lbl.show();
	return app.exec();
}
```
pch.h:
```cpp
#include <QtWidgets/QtWidgets>
```

___
### 2. С помощью Cmake

CMakeLists.txt:
```
cmake_minimum_required( VERSION 3.25 )
project( App LANGUAGES CXX )

set( qt_dir "/home/gene/Qt/6.6.1/gcc_64" )
set( qt_include_dir "${qt_dir}/include" )
set( qt_lib_dir "${qt_dir}/lib" )

add_library( Qt6Core SHARED IMPORTED)
target_include_directories( Qt6Core INTERFACE "${qt_include_dir}" )
set_target_properties( Qt6Core PROPERTIES IMPORTED_LOCATION "${qt_lib_dir}/libQt6Core.so" )

add_library( Qt6Widgets SHARED IMPORTED)
target_include_directories( Qt6Widgets INTERFACE "${qt_include_dir}" )
set_target_properties( Qt6Widgets PROPERTIES IMPORTED_LOCATION "${qt_lib_dir}/libQt6Widgets.so" )

add_executable( app main.cpp )
target_link_libraries( app PRIVATE Qt6Core Qt6Widgets )
target_precompile_headers(app PRIVATE "pch.h")
```

`main.cpp` и `pch.h` такие же как в примере с make за тем исключением, что в `main.cpp` можно не добавлять строчку `#include "pch.h"`.