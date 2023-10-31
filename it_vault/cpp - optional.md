___
Класс optional хранит некоторое значение или ничего. Optional хранит именно значение, а не указатель.
```cpp
template<class T>
class std::optional
```

___
### Важные замечания

- Дефолтный конструктор создает optional без значения, равный `std::nullopt`.
- Проверить наличие значения optional можно тремя способами:
```cpp
// std::optional<int> opt;
opt.has_value()
(oi != std::nullopt)
bool( opt )  // самый краткий вариант
```
- Получить значение optional можно двумя способами:
```cpp
opt.value()
*opt
```
- Обращение к методу значения внутри optional:
```cpp
opt->method()
```
- При попытке получить значение или обратиться к методу optional, которые не хранит значения, будет выброшено исключение `std::bad_optional_access`.
- метод `reset()` выбрасывает значение optional и делает его равным `std::nullopt`.
- можно избежать ненужных копирований если использовать `emplace()` и `make_optional()`



