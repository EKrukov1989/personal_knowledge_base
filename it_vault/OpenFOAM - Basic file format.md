___
### 1. Общие правила

- переход на новую строку не имеет какого-то особого значения, так же как в С++
- любой текст после `//` является комментом
- любой текст между `/*text*/` является комментом

___
### 2. Словарь

Большинство файлов OpenFOAM (исходных и выходных) представляют собой словари.
Словарь это последовательно пар: ключ-значение, со следующим синтаксисом:
```
<keyword1>  <dataEntry1>;
<keyword2>  <dataEntry2>;
<keyword3>  <dataEntry3> … <dataEntryN>;
```
 Внутри словарей могут быть подсловари и так далее:
```
<keyword1>  <dataEntry1>;
<keyword2>  <dataEntry2>;
<subdictionaryName>  
{  
	… keyword entries …  
}
```

___
### 3. Заголовок файла

Каждый файл содержит примерно следующий заголовок (который представляет собой словарь):
```
FoamFile  
{  
	format      ascii;  
	class       dictionary;  
	location    "system";  
	object      controlDict;  
}
```
Содержимое заголовка:
- version - врсия формата, по умолчанию 2.0
- format - формат данных, `ascii` или `binary`
- class - тип данных в файле, `dictionary` или `field`
- object - имя файла, в котором этот заголово находится
- location - путь к директории файла (относительно директории кейса)

___
### 4. Список

  Варианты синтаксиса списка:
```
<listname>
(  
	... entries ...
);

<listname>
<number of entries>
(  
	... entries ...
);

<listname>
List<scalar>
<number of entries>
(  
	... entries ...
);
```

Список созданный OpenFOAM всегда имеет имя, которое представляет собой количество записей в списке:
```
3
(
	5 6 7
);
```
Иногда OpenFOAM предваряет список его типом:
```
inGroups    List<word> 1(wall);
```
где `List<word>` это тип, `1` - имя списка (оно же - число элементов в списке).

___
### 5. Скаляры, векторы и тензоры

scalar - просто число
vector - список из трех значений
tensor - список из девяти значений

___
### 6. Единицы измерения

Размерность определяется с помощью массива из 7 целых чисел и записывается следующим образом:
```
[0 2 -1 0 0 0 0]
```
Здесь каждое число означает степень соответствующей единицы измерения.
Порядок единицы измерения следующий:
```
0     1     2     3     4     5     6
kg    m     s     K     kgmol A     cd
```
Например физическкую константу можно записать следующим образом:
```
nu  [0 2 -1 0 0 0 0]  1;
```

___
### 7. Поле

Field - описания поля распределения какой-то величины. Выглядит следующим образом:
```
dimensions      [0 2 -2 0 0 0 0];

internalField   uniform 0;

boundaryField
{
    inlet
    {
        type            zeroGradient;
    }

    outlet
    {
        type            fixedValue;
        value           uniform 0;
    }

	...
}
```

  Файл с описанием поля обязательно содержит: 
  - `dimensions` - размерность
  - `internalField` - описывание величины внутри области вычисления
  - `boundaryField` - описание величины на границах области вычисления. Представляет собой несколько словарей, в каждом из которых должно быть определен как минимум ключ `type`

В поле `internalField` если оно не равномерное может быть указан список значений:
```
internalField nonuniform <List>;
```

___
### 8. Директивы

`#include "<fileName>"` - вставить содержимое файла вместо директивы
`#inputMode <modename>` - указывает поведение файла (?)
`#remove <keywordEntry>` - удалить пару с данным ключом
`#codeStream` - можно вписать кусок C++ кода, который будет скомпилирован и исполнен в рантайме
___
### 9. Макросы

Можно подставить значение с помощью макроса:
```
key1 1e+05;
key2 $key1
```