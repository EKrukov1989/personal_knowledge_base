___
### 1. Основной набор утверждений

>[!note]
>Большинство утверждений имеют вариант с `ASSERT_*`, но здесь они не указаны.

>[!warning]
> Нужно помнить что `ASSERT_*` завершает только текущую функцию. Если вызвать `ASSERT_*` внутри вспомогательной функции, то тест продолжит выполнение.

- `FAIL()` -> признать тест полностью проваленным (fatal failure)
- `ADD_FAILURE()` -> признать тест проваленным, но продолжить выполнение (nonfatal failure)

`EXPECT_TRUE( condition )` 
`EXPECT_FALSE( condition )`
`EXPECT_EQ( val1, val2 )`
`EXPECT_NE( val1, val2 )`
`EXPECT_LT( val1, val2 )`
`EXPECT_LE( val1, val2 )`
`EXPECT_GT( val1, val2 )`
`EXPECT_GE( val1, val2 )`  

`EXPECT_STREQ( str1, str2 )`
`EXPECT_STRNE( str1, str2 )`
`EXPECT_STRCASEEQ( str1, str2 )`
`EXPECT_STRCASENE( str1, str2 )`

`EXPECT_FLOAT_EQ( val1, val2 )`
`EXPECT_DOUBLE_EQ( val1, val2 )`
`EXPECT_NEAR( val1, val2, abs_error )`
___
### 2. EXPECT_THAT

`EXPECT_THAT(value, matcher)`  
`ASSERT_THAT(value, matcher)`

Проверяет значение на соответствие  специальному объекту - матчеру.
Примеры интересных матчеров:
- `StartsWith(prefix)` -> проверить что строка начинается с `prefix`
- `WhenSorted(m)` -> проверяет, что контейнер отсортирован

Есть масса матчеров и лучше взять существующий, чем колхозить свою функцию:
- http://google.github.io/googletest/reference/matchers.html
___
### 3. Исключения

Можно проверять исключения вручную:
```cpp
try
{
	DoSomethingExceptable();
	FAIL();
}
catch(const std::exception& e)
{
}
```
Но значительно приятнее использовать готовые:
- `EXPECT_THROW(statement,exception_type)` -> бросает
- `EXPECT_ANY_THROW(statement)`
- `EXPECT_NO_THROW(statement)`

>[!note]
>Внутри макроса может быть блок кода:
>```cpp
>EXPECT_NO_THROW(
>	{
>		int n = 5;
>		DoSomething(&n);
>	}
>);
>```

___
### 4. Death assertions

- `EXPECT_EXIT( statement, predicate, matcher )`
- проверить, что в результате выполнения `statement` программа была завершена, статус выхода соответствует `predicate`, а сообщение об ошибке в `stderr` соответствует `matcher`
`EXPECT_DEATH( statement, matcher )`
- то же, но проверить что статус выхода не равен нулю, без `predicate`

Пример:
```cpp
EXPECT_DEATH(
	{
		int n = 5;
		DoSomething(&n);
	}, "Error on line .* of DoSomething()"
);	
```


