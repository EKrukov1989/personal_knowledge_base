___
`ON_CALL` - макрос, который позволяет задать возвращаемое значение для некоторого метода мока.

В отличие от `EXPECT_CAL` он не накладывает никаких ограничений на количество вызовов.

Синтаксис:
```cpp
ON_CALL( mock_object, method_name ( matchers... ) )
	.With( multi_argument_matcher )
    .WillByDefault(Return( ret_value ));
```





