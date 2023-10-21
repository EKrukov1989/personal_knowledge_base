___
Зачастую чтобы протестировать объект, нужно воссоздать его окружение из нескольких других объектов. Иногда можно использовать для этих целей настоящие объекта, а иногда - специальные заменители.

Существует два виде таких заменителей:
- `fake object` -> объект, который обладает рабочей имплементацией, пусть и в урезанной форме
- `mock object` -> объект без рабочей имплементации, который позволяет только фиксировать вызовы функций этого объекта

В данной статье рассматриваются только моки.
___
### 1. Подключение библиотеки gmock

Файл makefile:
```make
test_files = test.cpp

testapp : $(test_files)
g++ -o testapp \
	-I thirdparty/googletest/googletest/include \
	-I thirdparty/googletest/googlemock/include \
	-lgmock_main -lgtest_main -lgmock -lgtest\
	-Lthirdparty/googletest/build/lib \
	$(test_files)
```

>[!warning]
>Библиотеки должны быть перечислены именно в таком порядке:
>- `-lgmock_main -lgtest_main -lgmock -lgtest`

___
### 2. Правила создания моков

Как создать мок:
- создать класс с именем `Mock<ClassName>`
- для каждого метода в исходном классе написать макрос `MOCK_METHOD`
Пример мока:
```cpp
#include "gtest/gtest.h"
#include "gmock/gmock.h"

class Widget {
public:
	virtual ~Widget() {}
	virtual void me_1() = 0;
	virtual void me_2() const = 0;
	virtual double me_3(int, int) const = 0;
};

class MockWidget : public Widget {
public:
	MOCK_METHOD(void, me_1, (), (override) );
	MOCK_METHOD(void, me_2, (), (const, override) );
	MOCK_METHOD(double, me_3, (int, int), (const, override) );
};

TEST( TestSuiteName, TestName )
{
	MockWidget m;
	EXPECT_CALL(m, me_3(7, 11)).Times(::testing::AtLeast(1));
}
```
___
### 3. Ожидания

Ожидание (expectation) - макрос, который регистрирует ожидания разработчика относительно вызовов определенного метода.

Например:
```cpp
EXPECT_CALL(mock_object, methodname(matcher))
```
Данный макрос регистрирует, что у объекта `mock_object` в будущем должен быть вызван метод `methodname`, причем с аргументом, который подходит к `matcher`.

При вызове деструктора `mock_object` (что произойдет при завершении теста) будет проверено, что ожидание оправдалось. Если ожидание не оправдалось - тест будет провален.

Пример использования:
```cpp
TEST( TestSuiteName, TestName )
{
	MockWidget m;
	EXPECT_CALL(m, me_3(7, 11)).Times(::testing::AtLeast(1));
	// ожидание должно быть установлено до вызова метода!
	m.me_3(7,11);
} // при завершении теста ожидание будет проверено
```

Если метод не перегружен, то достаточно указать имя метода:
```cpp
EXPECT_CALL(mock_object, non-overloaded-method)
```

Для проверки аргументов можно использовать самые разные матчеры:
- `_`  ->  подходит любой аргумент
- `me(100)` <=> `me(Eq(100))` -> аргумент равен 100
- `me(Ge(100))` -> аргумента больше или равен 100

Для каждого ожидания можно указать дополнительные параметры - clauses.
Более подробно здесь:
- [[gtest - EXPECT_CALL]]

___
### 4. Порядок ожиданий 

Основное правило которое нужно помнить про порядок ожиданий:
>[!note]
>- Ожидания разных методов по умолчанию неупорядочены.
>- Ожидания одного метода упорядочены всегда.
##### 4.1. Порядок разных методов

По умолчанию ожидания разных методов неупорядочены.
Их можно упорядочить с помощью кляуз `.After` и `InSequence` ([[gtest - EXPECT_CALL]]) или с помощью объекта `InSequence`:
```cpp
TEST(..., ...) {
  {
    ::testing::InSequence seq;
    EXPECT_CALL(m, me_1());
    EXPECT_CALL(m, me_2(100));
    EXPECT_CALL(m, me_1());
  }
}
```
Все ожидания внутри области видимости объекта `InSequence` теперь должны оправдываться в том же порядке, в котором написаны ожидания.
##### 4.2. Порядок ожидания одинаковых методов с разными аргументами

Для одинаковых методов порядок всегда важен.

Когда вызывается метод мока, gMock ищет подходящее ожидание снизу вверх, пока не найдет подходящее. Поэтому в некоторых случаях порядок ожиданий не важен, а в некоторых наборот. Примеры:
```cpp
// порядок не важен:
EXPECT_CALL(m, me(5));
EXPECT_CALL(m, me(10));
// если вызвать me(5), то gMock найдет первое ожидание подходящим и оправдает его.

// порядок важен:
EXPECT_CALL(t, me(10));
EXPECT_CALL(t, me(::testing::_));
t.me_4(10);  // оправдает me(::testing::_)
t.me_4(5);   // не сможет оправдать me(10)
```

>[!tip]
>Нужно использовать `::testing::_` с большой осторожностью. Прежде следует исследовать все особенности его поведения.

##### 4.3. Порядок ожидания одинаковых методов с одинаковыми аргументами

 Если существует несколько ожиданий с одинаковыми аргументами, то для gMock всегда подходящим будет последнее:
```cpp
EXPECT_CALL(m, me(10));
EXPECT_CALL(m, me(10)).Times(2);
// При вызове me(10); me(10); me(10); будет fail, потому что все три вызова упадут в последнее ожидание. Когда gMock ищет ожидания сверху вниз, пока не найдет подходящее.
```
Здесь следует использовать `.RetiresOnSaturation` ([[gtest - EXPECT_CALL]])

___
### 5. Uninteresing calls

Если у объекта мока вызвать метод, который не ожидается (не упомянут ни в одном `EXPECT_CALL`-макросе), то gMock классифицирует это вызов как `uninteresting call` и выдаст предупреждение (но это не приведет к падению теста).

Для каждого отдельного объекта мока это поведение можно переопределить с помощью специальных шаблонов:
```cpp
// предупреждения об uninteresting call не будут выводиться:
::testing::NiceMock<MockClassName>

// любой uninteresting call будет приводить к падению теста:
::testing::StrictMock<MockClassName>
```

>[!tip] Рекомендуется использовать NiceMock
