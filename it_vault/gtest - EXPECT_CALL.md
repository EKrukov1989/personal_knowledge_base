___
`EXPECT_CALL` - макрос, который устанавливает некоторые ожидания, относительно вызовов указанного метода.

Синтаксис: `EXPECT_CALL( mock_object, method_name( matchers... ))`

Данный макрос регистрирует, что у объекта `mock_object` в будущем должен быть вызван метод `methodname`, причем с аргументом, который подходит к `matcher`.

Можно более точно указать требования к ожиданию с помощью кляуз (clause). Кляузы должны располагаться в строго определенном порядке:
```cpp
EXPECT_CALL(mock_object, method_name(matchers...))
    .With(multi_argument_matcher)  // Can be used at most once
    .Times(cardinality)            // Can be used at most once
    .InSequence(sequences...)      // Can be used any number of times
    .After(expectations...)        // Can be used any number of times
    .WillOnce(action)              // Can be used any number of times
    .WillRepeatedly(action)        // Can be used at most once
    .RetiresOnSaturation();        // Can be used at most once
```

___
### 1. With

С помощью кляузы `.With` можно дополнительно ограничить сферу действия ожидания. gMock при определении подходящего ожидания проверяет только, что аругменты подходят под матчеры по отдельности. С помощью кляузы `.With` можно задать матчер, который будет проверять их совместно.

Синтаксис: `.With( multi_argument_matcher )`
___
### 2. Times

С помощью кляузы `.Times` можно специфицировать количество вызовов метода.
Примеры:
```cpp
.Times( cardinality ) // объект cardinality определяет сколько
						// раз метод должен быть вызван
// Возможные варианты:
.Times( AnyNumber() )
.Times( AtLeast(n) )
.Times( AtMost(n) )
.Times( Between(n, m) )
.Times( Exactly(n) )
.Times( n )  // то же что .Times( Exactly(n) )
```

>[!note]
>Если Times, WillOnce, WillRepeatedly отсутствуют, что по умолчанию .Times(1).

___
### 3. WillOnce и WillRepeatedly

По умолчанию все методы мока возвращают только значение по умолчанию (если возвращаемое значение - default constructible). Можно самостоятельно задать возвращаемые значения с помощью кляуз WillOnce и/или WillRepeatedly. Например:
```cpp
EXPECT_CALL(turtle, GetX())
	.WillOnce( Return(100) )
	.WillOnce( Return(200) )
	.WillRepeatedly( Return(300) );
// GetX будет возвращать следующие значения: 100, 200, 300, 300, 300 ...

EXPECT_CALL(turtle, GetX())
	.WillOnce( Return(100) )
	.WillOnce( Return(200) );
// GetX будет возвращать следующие значения: 100, 200, 0, 0, 0 ...
```

___
### 4. RetiresOnSaturation

По умолчанию, если ожидание содержит точное указание на количество вызовов, то превышение этого количества приводит к провалу теста. Например:

```cpp
EXPECT_CALL(m, me(10))
	.WillOnce( ::testing::Return(300) );

EXPECT_CALL(m, me(10))
	.WillOnce( ::testing::Return(100) )
	.WillOnce( ::testing::Return(200) );

me(10);
me(10);
me(10); // test fail. Второй expectation пытаемся
		// вызвать третий раз, а он предполагает два
```

>[!note] В этом примере важно:
>- Ожидания оправдываются в обратном порядке.
>- Здесь в обоих ожиданиях один и тот же метод вызывается с одинаковым набором аргументов.

Если добавить кляузу `RetiresOnSaturation`, то после оправдания второго ожидания будет оправдываться первое:
```cpp
EXPECT_CALL(m, me(10))
	.WillOnce( ::testing::Return(300) );

EXPECT_CALL(m, me(10))
	.WillOnce( ::testing::Return(100) )
	.WillOnce( ::testing::Return(200) )
	.RetiresOnSaturation();

me(10); // return 100
me(10); // return 200
me(10); // return 300 not fail
```

После оправдания ожидания с кляузой `RetiresOnSaturation` можно считать, что оно не существует. Например:
```cpp
EXPECT_CALL(m, me(10))
	.WillOnce( ::testing::Return(100) )
	.RetiresOnSaturation();

me(10); // return 100
me(10); // return 0  (NOT FAIL!)
```

___
### 5. InSequence

>[!warning]
>На момент написания статьи нормального описания механизма работы InSequence не было и решил не тратить время. Существует еще целых два нормально описанных механизма упорядочения.

___
### 6. After

С помощью кляузы `.After` можно упорядочить ожидания:
Синтаксис: 
- `.After(expectations...)`
- `.After(expectations_set...)`
Пример:
```cpp
Expectation me_1 = EXPECT_CALL(m, me_1());
Expectation me_2 = EXPECT_CALL(m, me_2());
EXPECT_CALL(m, me_3())
    .After(me_1, me_2);
```
В результате `me_3` должен быть вызван после методов `me_1` и `me_2`.
