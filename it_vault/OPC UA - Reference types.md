___
>[!note] Источник
>Статья основана на Wolfgang Mahnke - OPC UA, Chapter 2.3.
### 1. Немного информации о ссылках:

- ссылка это связь между двумя узлами
- нельзя получить доступ к некоему общему списку ссылок, можно получить доступ к ссылкам только через узел с которым они связаны
- у ссылок самих по себе нет аттрибутов и свойств, однако ссылка несет информацию о том, как именно два узла соединены между собой (с помощью типа ссылки)

Ссылка определяется тремя параметрами:
- источник
- цель
- тип ссылик
Не может быть двух ссылок у которых все три параметра совпадают.
Также не может быть двух ссылок у которых совпадают цель и источник, а типы ссылок разные, но при этом один из типов является подтипом другого.
___
### 2. Тип ссылки

 >[!tip] Тип ссылки (ReferenceType) определяет смысл связи между двумя узлами.

Каждая ссылка обязательно имеет определенный тип.

Типы ссылок представляют собой узлы, которые имеют все обязательные аттрибуты (`BrowseName, DisplayName, Description`), а также еще три аттрибута:
- `IsAbstract (Boolean)` - `true` если тип нужен только для организационных целей в иерархии типов и никогда напрямую не будет использован как тип ссылки. Абстрактный тип может использоваться как базовый класс для наследования, а также как параметр при фильтации
- `Symmetric (Boolean)` -  `true` если ссылка такого типа имеет одинаковый смысл в обоих направлениях
- `InverseName (LocalizedText)` - имя, которое отражает смысл ссылки в обратном направлении. Этот аттрибут нужен только для неабстрактных и несимметричных типов
 Кроме того есть дополнительные ограничения для аттрибутов типов ссылок:
 - `BrowseName` типа ссылок должен быть уникальным на сервере
 - `BrowseName` должен выражать смысл ссылки в прямом направлении
 - `DisplayName` должен быть локализованной версией `BrowseName`

Все типы ссылок встроены в иерархию.

___
### 3. Иерархия типов

Все типы ссылок выстроены в единую иерархию. На следующем рисунке изображена иерархия стандартных типов ссылок:
![[reftypes_graph.png]]
- NOTE: Курсивом выделены абстрактные типы.
- NOTE: Связи внутри иерархии типов ссылок в свою очередь выражены с помощью одного из этих типов - HasSubtype.

>[!tip]
>Иерархия типов ссылок имеет смысл наследования: тип обладает всеми свойствами и смыслами всех типов от которых он происходит.

Принципы иерархии ссылок:
- Каждый тип ссылки должен иметь ровно один базовый класс (кроме типа  References)
- Наследоваться от типа  References могут только типы HierarchialReferences и NonHierarchialReferences (из чего следует что каждый тип должен быть либо иерархическим либо нет)
- Существует много стандартных типов, но могут быть определены и пользовательские

Существует еще ряд ограничений.
Например нельзя напрямую наследоваться от `HierarchialReferences`, но можно от `Organizes` и `HasChild`.

___
### 4. Описание стандартных типов

Тип `References` не несет какой-либо отдельного смысла. Нужен только для организационных целей.

Тип `HierarchialReferences` указывает владение (источник владеет целью). 
Тип `NonHierarchialReferences` - наоборот.

Тип `Organizes` является иерархическим типом, который при этом не несет никакой дополнительной смысловой нагрузки.

Ссылки типа `Organizes` могут образовывать циклы.

Тип `HasChild` добавляет дополнительное свойство: ссылки этого типа (и производных от него) не могут образовывать циклы. Однако, этот тип не запрещает иметь несколько родителей.

Тип `Aggregates` добавляет следующий смысл: цель является частью источника.
Тип `HasComponent` отличается от `Aggregates` тем, что является конкретным (неабстратным).

Тип `HasSubtype` добавляет следующей смысл: цель является наследником источника.

Тип `HasTypeDefinition` это пример неиерархического типа. Он означает связь одного узла (например класса `Object`) с его определением (например класса `ObjectType`). Очевидно, что в данном случае связь есть, а владения нет.

Тип ссылки может накладывать разные сложные ограничения:
- например запрещать источники или цели определенного класса или типа.
- определять количество ссылок определенного типа

Например:
- Источник может иметь только одну ссылку типа SubType с целевым узлом класса ReferenceType.

 Однако все эти ограничения никак не выражаютя в адресном пространстве (может быть только в виде описания). Соблюдение ограничений должно проверяться на стороне программ, которые используют адрессное пространство.

