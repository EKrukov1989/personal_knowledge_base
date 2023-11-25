___
В этой статье собрана вся необходимая информация о том, как собрать тестовое приложение и использовать его.
___
### 1. Как собрать googletest

Лучше всего собрать библиотеки из исходников:
```bash
cd <workdir>
git clone https://github.com/google/googletest.git
cd googletest
mkdir build
cd build
cmake ../
cmake --build .
```
В результате будут собраны библиотеки
- `libgmock.a` 
- `libgmock_main.a`
- `libgtest.a`
- `libgtest_main.a`

___
### 2. Как собрать тестовое приложение с помощью make

Структура файлов проекта:
```
thirdparty
	googletest...
makefile
test.cpp
[testapp]
```
Файл test.cpp:
```cpp
#include "gtest/gtest.h"
TEST(TestSuiteName, TestName) {
	EXPECT_EQ(42, 42);
}
```
Файл makefile:
```make
test_files = test.cpp

testapp : $(test_files)
	g++ -o testapp \
        -I thirdparty/googletest/googletest/include \
        -lgtest_main -lgtest \
        -Lthirdparty/googletest/build/lib \
        $(test_files)
```

>[!warning]
>Библиотеки `-lgtest_main -lgtest` должны быть указаны именно в таком порядке!

Чтобы собрать testapp нужно выполнить команду `make`.
В результате будет собрано приложение `testapp`, которое будет содержать один тест - `TestSuiteName.TestName`.

Как собрать и использовать тестовое приложение с помощью cmake: [[cmake - gtest]]
___
### 4. Как использовать тестовое приложение

Тестовое приложение предназначено для запуска тестов. Далее приведен краткий перечень интересных команд для тестового приложения:

- `./testapp` -> запустить все тесты
- `--gtest_list_tests` -> вывести список тестов
- `./testapp --gtest_also_run_disabled_tests` -> запустить все тесты, даже `disabled`
- `./testapp --help` -> посмотреть список доступных команды

Наиболее важная команда - фильтрация:
- `--gtest_filter=POSITIVE_PATTERNS[-NEGATIVE_PATTERNS]` -> запустить только те тесты, которые проходят фильтры

Паттерны могут содержать wildcard-символы (`?` и `*`) и должны быть разделены между собой знаком `:`.

По умолчанию все паттерны в списке позитивные, до тех пор пока в списке не будет встречен паттерн начинающийся со знака минус (`-`). Это паттерн и все последующие паттерны будут негативными до тех пор, пока не будет встречен следующий паттерн с минусом. Примеры:
- `--gtest_filter=POSPAT1`
- `--gtest_filter=POSPAT1:POSPAT2`
- `--gtest_filter=POSPAT1:POSPAT2:-NEGPAT1:NEGPAT2`
- `--gtest_filter=-NEGPAT1:NEGPAT2`
- `--gtest_filter=POSPAT1:-NEGPAT1:NEGPAT2:-POSPAT2` (минус на минус дает плюс)
где `POSPAT` это паттерн, который будет действовать как позитивный, а `NEGPAT` - как негативный.

Будут запущены только те тесты, которые подойдут хотя бы к одному из позитивных паттернов, но не подойдут ни к одному из негативных.