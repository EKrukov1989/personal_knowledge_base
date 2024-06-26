___
Дана строка s, найти самую длинную последовательность-палиндром (ПП) в этой строке. Обозначим это как функцию f(s).

Последовательность может быть образована посредством удаления одного или нескольких элементов из начальной подстроки.
___
### 1. Рекурсия с мемоизацией

Допустим у нас есть строка `s = abcde`. В это строке буквы только указывают на местоположение, буквы могут быть другие. Тогда:
- если `a` == `e`, то `f(abcde) = f(bcd) + 2` 
- в противном случае `f(abcde) = max(f(abcd), f(bcde))`

Такуюу задачу можно решить методом рекурсии с мемоизацией:

```cpp
#include <algorithm>

class Solution
{
	std::string s_;
	std::vector<int> cache_;
	std::size_t n_ = 0;
	
	int lpss_( std::size_t first, std::size_t last )
	{
		if ( first == last )
			return 1;
		if ( first > last )
			return 0;
		
		auto& csr = cache_[first * n_ + last]; // csr = cache slot ref
		if ( csr != 0 )
			return csr;
		
		if ( s_[first] == s_[last] )
			csr = 2 + lpss_( first + 1, last - 1 );
		else
			csr = std::max(lpss_(first + 1, last), lpss_(first, last - 1));		  
		return csr;
	}

public:
	int longestPalindromeSubseq(string s)
	{
		n_ = s.size();
		s_ = std::move(s);
		cache_ = std::vector<int>( n_ * n_, 0 );
		return lpss_( 0, n_-1 );
	}
};
```


___
### 2. Динамическое программирование

Дана строка `s = abcde`, построим следующую таблицу:

>[!note]
>Далее предполагается, что вместо букв `abcde` могут быть любые буквы.

-|-|a|b|c|d
-|-|-|-|-|-
-|0|0|0|0|0
d|0||||
c|0||||
b|0||||
a|0||||x

Предположим, что у нас имеется такой способ заполнения таблицы, что в правой нижней клетке (x) будет образовываться результат f(s).

Данная таблица будет иметь оптимальную субструктуру.  Т.е. например в следующем таблице квадрат, отмеченный точками будет действовать аналогично для строки `abcd`.

-|-|a|b|c|d|e
-|-|-|-|-|-|-
-|0|0|0|0|0|0
e|0|||||
d|0|.|.|.|.|
c|0|.|.|.|.|
b|0|.|.|.|.|
a|0|.|.|.|.|x

И так далее.

Тогда чтобы получить необходимый результат нужно провести следующие вычисления для каждой клетки:
- если соответствующие буквы по горизонтали и вертикали совпали - 1 + верхняя левая.
- если нет - максимум из левой или верхней:
