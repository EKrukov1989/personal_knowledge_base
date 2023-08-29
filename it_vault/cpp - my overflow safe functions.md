___
В случае переполнения `unsigned integer` результат будут равен:
`result == correct_result % uint_max`
В случае переполнения `signed integer` -> `undefined behavior`.

В любом случае такая ситуация обычно нежелательна.

Ниже приведены две функции (и тесты к ним), которые позволяют безопасно умножать и складывать числа.

>[!note]
>Возможно лучше выбрасывать исключения вместо возвращения optional.

```c++
#include <iostream>
#include <limits>
#include <vector>
#include <map>
#include <climits>
#include <optional>

std::optional<int> safe_mult(int a, int b)
{
    if (b == 0)
        return 0;

    if ( a >= 0 )
    {
        if ( b > 0 )
        { 
            if ( INT_MAX / b < a ) return std::nullopt;
        }
        else
        {
            if ( b != -1 && INT_MIN / b < a ) return std::nullopt;
        }
    }
    else  // a < 0
    {
        if ( b > 0 )
        { 
            if ( INT_MIN / b > a ) return std::nullopt;
        }
        else
        {
            if ( INT_MAX / b > a ) return std::nullopt;
        }
    }

    return a * b;
}

std::optional<int> safe_add(int a, int b)
{
    if ( a > 0 )
    {
        if ( INT_MAX - a < b )
            return std::nullopt;
    }
    else
    {
        if ( INT_MIN - a > b )
            return std::nullopt;
    }

    return a + b;
}

void safe_add_test()
{
    std::map<std::pair<int, int>, std::optional<int>> cases =
    {
        { {100, 100}, 200 },
        { {100, -100}, 0 },
        { {0, 0}, 0 },
        { {0, -10}, -10 },
        { {INT_MAX, INT_MAX}, std::nullopt },
        { {INT_MAX, -100},  INT_MAX - 100 },
        { {INT_MAX, -INT_MAX},  0 },
        { {-100, -INT_MAX}, std::nullopt }
    };

    for (auto& [pair, res] : cases)
    {
        std::cout << pair.first << " | " << pair.second << " | <<
        (res.has_value() ? std::to_string(res.value()) : std::string("null") )
        << " | " <<
        ( safe_add(pair.first, pair.second) == res ? "passed" : "failed")
        << std::endl;
    }
}

static const int cn = 12;
std::string nullopt_str_ = "nullopt";
std::string c_nullopt_str = nullopt_str_ + std::string(cn -   nullopt_str_.size(), ' ');

std::string to_str(int i)
{
    auto s = std::to_string(i);
    return s + std::string(cn - s.size(), ' ');
}

std::string to_str( const std::optional<int>& a )
{
    return a.has_value() ? to_str(a.value()) : c_nullopt_str;
}

void safe_mult_test()
{
    std::vector<int> values = { INT_MIN, INT_MIN+1, -1000*1000, -10,-1, 0, 1, 10, 1000*1000, INT_MAX-1, INT_MAX };

    std::map<std::pair<int, int>, std::optional<int>> cases;

    auto n = values.size();
    for ( int i = 0; i < n; ++i)
    {
        for (int j = i; j < n; ++j)
        {
            int64_t a = values[i];
            int64_t b = values[j];
            int64_t c = a * b;
            auto r = ( int64_t(INT_MIN) <= c && c <= int64_t(INT_MAX) )
            ? std::optional<int>(int(c)) : std::nullopt;
            cases.insert( {{values[i], values[j]}, r });
        }
    }

    for (auto& [pair, ctrl] : cases)
    {
        auto a = pair.first;
        auto b = pair.second;
        auto c = safe_mult(a, b);
        std::cout << "A: " << to_str(a)
                  << "B: " << to_str(b)
                  << "C: " << to_str(c)
                  << "CTRL: " << to_str(ctrl)
                  << "| test " << ( c == ctrl ? "passed" : "failed") << std::endl;
    }
}

int main()
{
    safe_mult_test();
    return 0;
}
```