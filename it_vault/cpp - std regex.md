___
### 1. Немного теории и терминологии

`pattern` -> собственно, регулярное выражение, хранится в объекте `std::regex`.

Помимо регулярного выражения, объекты `std::regex` хранят и другую информацию. Например `syntax_flags`.

`syntax_flags` -> набор настроек для regex-движка. С помощью этих флагов можно в первую очередь указать flavor и некоторые другие настройки.

`target_sequence` -> текст, в котором следует производить поиск

`match_results` -> объект, который движок возвращает нам, как результат проведения поиска. Он хранит всю нужную нам информацию о найденной подходящее подстроке

`sub_match` -> часть найденной подстроки, которая соответствует части регулярного выраженя, выделенной круглыми скобками. Например регулярное выражение `a(bc)d(ef)g`, в случае успеха будет иметь три  `sub_match`  -> `abcdefg`, `bc`, `ef` . Вся найденная подстрока тоже считается `sub_match` и всегда идет первой.

___
### 2. Функции

Стандартная библиотека предоставляет нам три удобных функции:
- regex_match -> проверяет, соответствует ли target_sequence регулярному выражению полностью. 
- regex_search -> находит в target_sequence построку, подходящую под регулярное выражение.
- regex_replace -> находит и заменяет ВСЕ найденные подстроки. Замену можно форматировать.

___
### 3. Пример использования std::regex_search

regex_search - это основная функция, в следующем примере я использую ее чтобы вручную перебрать все найденные подстроки.

```C++
#include <iostream>
#include <string>
#include <regex>

int main()
{
    std::string target_sequence =
    "mycmd --debug=true --optimization-level=3 --output=result.txt";
    std::regex pattern(
		"--([a-zA-z0-9_-]+)=([a-zA-z0-9_.-]+)",
		std::regex_constants::extended);
    auto start = target_sequence.cbegin();
    auto end = target_sequence.cend();
    
    while (true)
    {
        std::smatch m;    
        if (!std::regex_search(start, end, m, pattern))
            break;

		std::string subs;
		for (size_t i = 0; i < m.size(); ++i)
			subs += m.str(i) + " ";
		std::stringstream ss;
		ss << "submatches: " << subs << std::endl;
		std::cout << ss.str();

        start = start + m.prefix().length() + m.length();
    }
    return 0;
}
```

Результат:
```text
submatches: --debug=true debug true 
submatches: --optimization-level=3 optimization-level 3 
submatches: --output=result.txt output result.txt 
```

___
### 4. Пример использования std::regex_iterator

Чтобы облегчить процесс поиска всех подстрок в таргете в стандартную библиотеку был введен regex_iterator. Того же результата можно добиться следующим образом:

```C++
#include <iostream>
#include <string>
#include <sstream>
#include <regex>

int main()
{
    std::string ts =
    "mycmd --debug=true --optimization-level=3 --output=result.txt";
    std::regex pattern(
	    "--([a-zA-z0-9_-]+)=([a-zA-z0-9_.-]+)",
	    std::regex_constants::extended);
    
    auto beg = std::sregex_iterator(ts.begin(), ts.end(), pattern);
    auto end = std::sregex_iterator();

    for (auto it = beg; it != end; ++it)
    {
        std::smatch m = *it;
        std::string subs;
        for (size_t i = 0; i < m.size(); ++i)
            subs += m.str(i) + " ";
        std::stringstream ss;
        ss << "submatches: " << subs << std::endl;
        std::cout << ss.str();
    }
    return 0;
}
```

Результат будет таким же.

### 5. Compile time regex

Сейчас из коробки нет, обещают в C++23.

