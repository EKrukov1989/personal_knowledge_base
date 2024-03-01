___
При написании рекурсивной лямбды нужно помнить следующие правила:
- тип нужно указывать полностью с помощью враппера std::function
- саму лямбду также нужно передавать в лямбду через список декларации (обязательно по ссылке!)
```cpp
#include <functional>

std::function<int(int)> recursive_lambda_name = [&recursive_lambda_name](int a)
{
	if (a  < 10)
		return a;
	a -= 10;	
	return recursive_lambda_name(a);
};

```