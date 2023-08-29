Регулярное выражение - это последовательность символов, которая определяет шаблон поиска в тексте. С помощью такого шаблона можно найти один или несколько подстрок в тексте, которые соответствуют этому шаблону.

К сожалению, не существует единого стандарта регулярных выражений. Есть целых три варианта регулярных выражений, которые широко применяются и заслуживают внимания: BRE, ERE, PCRE.
- BRE - basic regular expression
- ERE - extended regular expression
- PCRE - perl compatible regular expressions 

Все три широко испольуются:
- grep, sed используют BRE and ERE
- python (re-module) использует PCRE-similar
- С++ (std::regex) использует BRE, ERE, ECMAScript (PCRE-similar)
- MySQL использует ERE

Ссылки:
https://www.regular-expressions.info/
https://www.gnu.org/software/sed/manual/sed.html
[ecma-international.org ECMA-262 grammar](https://262.ecma-international.org/5.1/#sec-15.10)
[pubs.opengroup.org - BRE grammar](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap09.html#tag_09_03)
[pubs.opengroup.org - ERE grammar](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap09.html#tag_09_04)