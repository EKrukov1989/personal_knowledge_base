___
Основано на: [gnu.org](https://www.gnu.org/software/gnulib/manual/html_node/Exported-Symbols-of-Shared-Libraries.html). и https://gcc.gnu.org/wiki/Visibility

В заголовочном файле, если символ нужно сделать доступным снаружи динамической библиотеки, нужно отметить его с помощью аттрибута:
- `__attribute__ ((visibility ("default")))`
Чтобы сделать недоступным:
- `__attribute__ ((visibility ("hidden")))`

По умолчанию в gcc видимость `"default"`, но если при компилировании исходных файлов использовать опцию `-fvisibility=hidden`, то видимость по умолчанию будет `"hidden"`.

___
### Пример реализации для gcc

Файл `dllexport.h`:
```cpp
#define DLL_PUBLIC __attribute__ ((visibility ("default")))
#define DLL_LOCAL  __attribute__ ((visibility ("hidden")))
```
Файл `makefile`:
```make
build/a.o : a.cpp a.h
	g++ -c -fvisibility=hidden -fpic -o build/a.o a.cpp

build/liba.so : build/a.o
	g++ -shared -o build/liba.so build/a.o
```
Файл `a.h`:
```cpp
#include "dllexport.h"
DLL_PUBLIC int foo();  // public outside the DSO
DLL_LOCAL  int bar();  // local
           int baz();  // local
```
Файл `a.cpp`:
```cpp
#include "a.h"
int foo() { return 11; }  // в .cpp-файлах никакие
int bar() { return 17; }  // макросы не нужны
int baz() { return 23; }
```

___
### Использование для класса

```cpp
class DLL_PUBLIC SomeClass
{
   DLL_LOCAL void privateMethod();  // local
public:
   void publicMethod();  // public
};

```


