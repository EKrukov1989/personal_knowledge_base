___
### 1. Hello, World!

Структура файлов проекта:
```
makefile
main.cpp
[app] -> ( результат компиляции )
```
Содержимое main.cpp:
```c++
#include <iostream>
int main()
{
	std::cout << "Hello, World!" << std::endl;
	return 0;
}
```
Содержимое makefile:
```make
app : main.cpp
	g++ -o app main.cpp
```

>[!note]
>Так как по умолчанию g++ добавляет отладочные символы - полученный исполняемый файл можно дебажить.

___
### 2. Precompiled headers

Механизм работы с precompiled headers описан в [[gcc - cpp cheat sheet]].

Структура файлов проекта:
```
makefile
main.cpp
pch.h
[pch.h.gch]
[app]
```
Содержимое makefile:
```makefile
app : main.cpp pch.h.gch
	g++ -o app main.cpp

pch.h.gch : pch.h
	g++ -o pch.h.gch pch.h
```
Содержимое main.cpp:
```C++
include <pch.h>
int main()
{
	std::cout << "Hello, World!" << std::endl;
	return 0;
}
```
Содержимое pch.h:
```C++
include <iostream>
include <string>
include <vector>
```

___
### 3. Static library

Структура файлов проекта:
```
build
	[utils.o]
	[summator.o]
	[libutils.a]
	[app]
utils
	utils.cpp
	utils.h
	summator.cpp
	summator.h
	makefile
main.cpp
makefile
```
Содержимое makefile:
```makefile
build_dir = build

$(build_dir)/app : main.cpp $(build_dir)/libutils.a
	g++ -o $(build_dir)/app main.cpp -lutils -L$(build_dir)

include utils/makefile
```
Содержимое utils/makefile:
```
utils_dir = utils

$(build_dir)/utils.o : $(utils_dir)/utils.cpp $(utils_dir)/utils.h
	g++ -c -o $(build_dir)/utils.o $(utils_dir)/utils.cpp

$(build_dir)/summator.o : $(utils_dir)/summator.cpp $(utils_dir)/summator.h
	g++ -c -o $(build_dir)/summator.o $(utils_dir)/summator.cpp

utils_objects = $(build_dir)/utils.o \
	$(build_dir)/summator.o

$(build_dir)/libutils.a : $(utils_objects)
	ar rcs -o $(build_dir)/libutils.a $(utils_objects)
```
Содержимое main.cpp:
```cpp
#include <iostream>
#include "utils/utils.h"
#include "utils/summator.h"
int main()
{
    std::cout << "Hello, World!" << std::endl;
    std::cout << utils::mul(2, 3) << std::endl;
    utils::Summator s;
    s.add(2);
    s.add(3);
    std::cout << s.get() << std::endl;
    return 0;
}
```
Содержимое utils.h:
```cpp
namespace utils
{
int sum(int a, int b);
int mul(int a, int b);
} // namespace utils
```
Содержимое utils.cpp:
```cpp
#include "utils.h"
namespace utils
{
int sum(int a, int b) { return a + b; }
int mul(int a, int b) { return a * b; }
} // namespace utils
```
Содержимое summator.h:
```cpp
namespace utils
{
class Summator
{
public:
    void add(int);
    int get();
private:
    int s_ = 0;
};
} // namespace utils
```
Содержимое summator.cpp:
```cpp
#include "summator.h"
namespace utils
{
void Summator::add(int a) { s_ += a; }
int Summator::get() { return s_; }
} // namespace utils
```

___
### 4. ThirdParty

Допустим нам нужно подключить сторонюю библиотеку. У нас есть только бинарный файл `.a` и заголовочный файл.

Структура файлов проекта:
```
build
	[app]
thridparty
	utils
		libutils.a
		utils.h
main.cpp
makefile
```
Содержимое makefile:
```
build_dir = build
third_dir = thirdparty

$(build_dir)/app : main.cpp $(third_dir)/utils/libutils.a
	g++ -o $(build_dir)/app main.cpp \
        -I $(third_dir) -lutils -L$(third_dir)/utils
```

___
### 5. Shared library

Основано на [cprogramming.com](https://www.cprogramming.com/tutorial/shared-libraries-linux-gcc.html).

Структура файлов проекта:
```
build
	[utils.o]
	[summator.o]
	[libutils.so]
	[app]
utils
	utils.cpp
	utils.h
	summator.cpp
	summator.h
	makefile
main.cpp
makefile
```
Содержимое makefile:
```
build_dir = build

$(build_dir)/app : main.cpp $(build_dir)/libutils.so
	g++ -o $(build_dir)/app main.cpp \
	    -Wl,-rpath='$$ORIGIN' -lutils -L$(build_dir)

include utils/makefile
```
Содержимое utils/makefile:
```
utils_dir = utils

$(build_dir)/utils.o : $(utils_dir)/utils.cpp $(utils_dir)/utils.h
	g++ -c -fpic -o $(build_dir)/utils.o $(utils_dir)/utils.cpp
$(build_dir)/summator.o : $(utils_dir)/summator.cpp $(utils_dir)/summator.h
	g++ -c -fpic -o $(build_dir)/summator.o $(utils_dir)/summator.cpp

utils_objects = $(build_dir)/utils.o $(build_dir)/summator.o

$(build_dir)/libutils.so : $(utils_objects)
	g++ -shared -o $(build_dir)/libutils.so $(utils_objects)
```

Создание динамической библиотеки от статической отличается только опцией `-fpic` при компиляции и опций `-shared` при линковке

При подключении динамической библиотеки к исполняемому файлу (или к другой динамической библиотеке), нужно указать путь по которому будет находится сам файл динамической библиотеки. Существует несколько способов сделать это:
- добавить путь к `LD_LIBRARY_PATH`
- положить файл в `/usr/lib` и вызвать команду `ldconfig`, после чего динамическая библиотека станет доступной для всех в системе
- прописать путь в исполняемом файле во время линковки с помощью опции `-rpath`

Подробно рассмотрим способ с использованием `-rpath` (run path):

1. При создании исполняемого файла нужно передать в линкер опцию  `-rpath=dir`, где `dir` - это директория, в которой находится файл динамической библиотеки. Это путь будет прописан в исполняемом файле и в дальнейшем не может быть изменен
2. Если используется g++, то чтобы передать опцию в линкер следует использовать опцию `-Wl,`, где после запятой прописать опцию, которую следует передать. Например: `-Wl,-rpath=<dir>`
3. При этом если указать  относительную директорию, то она будет относительна `PWD`, что неприемлемо. Чтобы указать директорию относительно исполняемого файла следует использовать специальную переменную `ORIGIN`.

В итоге в make это приобретается следующую форму:
```
g++ -o $(build_dir)/app main.cpp
    -lutils -L$(build_dir)
    -Wl,-rpath='$$ORIGIN'
```
