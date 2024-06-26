___

OPC (open platform communications) - набор открытых протоколов для управления объектами автоматизации и технологическими процессами.

OPC UA - unified architecture, последняя спецификация протокола. В отличие от остальных - не привязана к Windows.

**SCADA** (supervisory control and data acquisition) - программа для работы в реальном времени систем сбора, обработки, отображения и архивирования информации.

OPC DA (data access) - интерфейс для чтения, записи и мониторинга переменных.

UA server - приложение, которое предоставляет информацию
US client - приложение, которое потребляет информацию
Большинство приложений одновременно сочетают функции сервера и клиента.

OPC UA SDK - набор для разработки UA приложений.


Объектно-ориентированность!
Наследование!
Информация о типах так же предоставляется!


Full-Mesh Topology - топология, в которой каждый узел соединен со всеми остальными узлами


Node
NodeClass
NodeId - уникальный идентификатор нода на сервере

Нод может иметь несколько NodeId
Клиенты используют NodeId чтобы обращаться к нодам.

NodeId хранится в аттрибуте NodeId
NodeId содержит пространства имен

Каждый Node имеет следующие аттрибуты:
- NodeId
- NodeClass
- BrowseName
- DisplayName
- Description
- WriteMask
- UserWriteMask

BrowseName - так же содержит нэймспэйсы, так же должно быть уникальным в пределах сервера
DisplayName - локализованный вариант BrowseName


Reference - ссылка между двумя нодами
Имеет source, target, direction, и type


Ссылка может быть односторонней, может указывать на нод в другом сервере или на несуществующий нод. Можно рассматривать ссылку как указатель на другой нод (только хранящий не адрес а NodeId)

Ссылки бывают симметричные и несимметричные
симметричная ссылки имеют тип `is-sibling-of`
несимметричные ссылки имеют типы `has-parent` или`is-child-of`. Стрелочка на схемах рисуется от ребенка к родителю

ссылки в ноде неупорядочены, а вот например ссылки типа HasOrderedComponent - упорядочены

Нельзя добавить еще аттрибуты ноду, но может добавить проперти

Нельзя добавлять свойства к ссылкам
Если нужно присовить какую-то нформацию ссылке - то нужно врезать дополнительный нод между источником и таргетом

Сервер может определять свои собственные ReferenceTypes

Ссылки не имеют нодов, а ReferenceType - имеет

Дополнительные аттрибуты для типов ссылок (помимо обязательных аттрибутов, которые должны быть у каждого нода):
- IsAbstract - данный тип не используется для описания ссылок, а нужен только для построения иерархии
- Symmetric
- InverseName

Для ReferenceType BrowseName должен отображать смысл ссылки в прямом направлении.

Существует стандартная иеррахия типов ссылок. Можно использовать следующие стандартные типы:
- HasComponent
- HasSubType
- HasTypeDefinition

Hierarchial references нужны для отображения иерархических связей между узлами, чтобы показать что один из услов владеет другм

Наследование ссылочных типов не может быть множественным.

Ссылки типа Organizes могут быть цикличными, а ссылки типов, унаследованных от HasChild - нет.

Не может быть двух идентичных ссылок.


Примеры NodeClass
- Object
- Variable
- Method

Объекты могут содержать переменные и методы, а также создавать события.

Метод предоставляет только сигнатуру. Нет никакой возможности получить или уастновить имплементацию метода на сервер.

Нод Object используется только для структурирования и хранит только другие ноды, такие как variable и method.

методы всегда вызываются в контексте объекта

Объект может быть EventNotifier. В этом случае другие объекты смогут на него подписаться.

Объект обладает одним дополнительным аттрибутом (в дополнение к обязательным аттрибутам нода), а именно EventNotifier. Этот аттрибут определяет:
- можно ли подписываться на объект
- доступность истории событий
- доступность изменеия истории событий

NodeClass Variable обладает множеством дополнительных аттрибутов:


___

ReferenceFilter :
- Unused
- Child
- Aggregates
- NotOwner

DbAccess ReferenceMultiplicity
- Multiplicity_0_1
- Multiplicity_1_1
- Multiplicity_0_N
- Multiplicity_1_N
- None
Пока не до конца понятно что это