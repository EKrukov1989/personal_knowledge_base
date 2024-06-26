___
В данной статье рассматриваются способы подключения и использования gtest с помощью cmake. Подключение с помощью make рассмотрено здесь: [[gtest - testapp]].
___
### 1. Подключение как импортной библиотеки:

Структура проекта:
```
build
	...
	[app]
	[testapp]
CMakeLists.txt
main.cpp
test.cpp
utils.h
utils.cpp
```
Файл utils.h:
```cpp
namespace utils {
int sum(int a, int b);

class Widget {
public:
	virtual ~Widget() {}
	virtual void me() = 0;
};
} // namespace utils
```
Файл utils.cpp:
```cpp
#include "utils.h"
namespace utils {
	int sum(int a, int b) { return a + b; }
}
```
Файл main.cpp:
```cpp
#include <iostream>
#include "utils.h"
int main() {
	std::cout << utils::sum(2,3) << std::endl;
	return 0;
}
```
Файл test.cpp:
```cpp
#include "utils.h"
#include "gtest/gtest.h"
#include "gmock/gmock.h"

class MockWidget : public utils::Widget {
public:
	MOCK_METHOD(void, me, (), (override) );
};

TEST(TestSuiteName, UsualTest){
	EXPECT_EQ( utils::sum(2,3), 5);
}

TEST(TestSuiteName, TestWithMock){
	MockWidget m;
	EXPECT_CALL(m, me()).Times(::testing::AtLeast(1));
	m.me();
}
```
Файл CMakeLists.txt:
```cmake
cmake_minimum_required( VERSION 3.25 )
project( App LANGUAGES CXX )
  
set( googletest_dir "/home/gene/Desktop/hubreps/googletest" )

add_library( gtest STATIC IMPORTED )
target_include_directories( gtest INTERFACE "${googletest_dir}/googletest/include")
set_target_properties( gtest PROPERTIES IMPORTED_LOCATION "${googletest_dir}/build/lib/libgtest.a")

add_library( gmock STATIC IMPORTED )
target_include_directories( gmock INTERFACE "${googletest_dir}/googlemock/include")
set_target_properties( gmock PROPERTIES IMPORTED_LOCATION "${googletest_dir}/build/lib/libgmock.a")

add_library( gtest_main STATIC IMPORTED )
set_target_properties( gtest_main PROPERTIES IMPORTED_LOCATION "${googletest_dir}/build/lib/libgtest_main.a")

add_executable( testapp EXCLUDE_FROM_ALL test.cpp utils.cpp)
target_link_libraries( testapp PRIVATE gtest gmock gtest_main )

add_executable( app main.cpp utils.cpp)
```

Собрать и запустить тесты можно следующим образом:
```bash
cd build
cmake ../
cmake --build . --target testapp
./testapp
```

___
### 2. Подключение с помощью FindGTest-модуля

Предварительно нужно:
- скачать gtest с github
- собрать gtest
- установить gtest с помощью make install

В cmake можно создать тестовое приложение и добавить в него кейсы следующим образом:
```cmake
enable_testing()
add_executable(testapp test.cpp)
find_package(GTest REQUIRED)
target_link_libraries(testapp PRIVATE GTest::gtest GTest::gtest_main)
add_test(NAME testapp COMMAND testapp)
```

В этом случае все тест-кейсы, которые будут найдены в `test.cpp` (и  других файлах) будут собраны в одно приложение `testapp`. При запуске `ctest` они будут отображаться как один тест.

Можно добавить все тесты в приложение по отдельности с помощью `gtest_add_tests`:
```
enable_testing()
add_executable(testapp test.cpp)
find_package(GTest REQUIRED)
target_link_libraries(testapp PRIVATE GTest::gtest GTest::gtest_main)
gtest_add_tests(testapp "" test.cpp)
```



