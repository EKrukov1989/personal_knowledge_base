___
Для ряда стандартных классов (таких как std::string или std::map) gdb выводит данные в особом формате.

`info pretty-printer`
- посмотреть список классов, для которых определен pretty-printer

`enable pretty-printer [object-regexp [name-regexp]]`
`disable pretty-printer [object-regexp [name-regexp]]`
- включить/выключить pretty-printer