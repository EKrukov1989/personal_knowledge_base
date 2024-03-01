___
Каждый узел имеет нескоторый набор аттрибутов, которые служат для описания данного узла.

Каждый узел имеет определенный NodeClass (который тоже аттрибут) и в зависимости от этого NodeClass определяется набор аттрибутов, которые должны быть (или могут быть) у данного узла.
___
### Обязательные аттрибуты

Есть определенный набор атрибутов, которые обязательны для всех узлов. Вот список этих атрибутов:
- `NodeId` - канонический NodeId. Может существовать несколько NodeId, которые указывают на узел, но только этот считается каноническим, потому что записан в атрибуте узла.
- `NodeClass` - класс узла, например Object, Variable, Method, ObjectType и т.д.
- `BrowseName` - имя узла, никогда не локализуемое
- `DisplayName` - локализованное имя узла, которые демонстрируется пользователю
- `Description` - локализованное описание, необязательный аттрибут
- `WriteMask`
- `UserWriteMask`



