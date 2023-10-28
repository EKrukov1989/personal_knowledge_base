___
Существует три типа наследования в C++:
- public
- protected
- private

 Допустим у нас есть базовый класс:
```cpp
class Base
{
public:
	void base_public_memfn();
protected:
	void base_protected_memfn();
private:
	void base_private_memfn();
};
```
Унаследуем от него производный класс:
```cpp
class Derived : <public|protected|private> Base
{
};
```

При `public` наследовании для `Derived` класса все члены базового класса будут вести себя так же, как они указаны в базовом классе.

Для `protected` наследования отличие в том, что public члены будут вести себя как protected. То есть будут доступны только для производных классов.

Для `private` наследования отличие в том, что public и protected члены будут вести себя как private. То есть будут недоступны.

___
Еще одно отличие `private` наследования в том, что невозможно скастовать указатель на производный класс к указателю на базовый класс.
