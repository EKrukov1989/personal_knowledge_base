___
При создании таблицы аттрибуты указываются после типов данных.
Список наиболее интересных аттрибутов:
___
#####  NULL и NOT NULL

Аттрибуты `NULL` указывает на то, что данное поле может не иметь значения. Соответственно `NOT NULL` - требует чтобы значение было установлено.  По умолчанию `NULL`.
___
##### DEFAULT

Можно задать значение по умолчанию, с помощью синтаксиса:
- `<type> DEFAULT <default_value>`
___
##### AUTO_INCREMENT

Аттрибут `AUTO_INCREMENT` может быть применен только к данным типа integer или floating point. Если пользователь при вставке строчки указывает вместо значения NULL или 0, то mysql находит максимальное значение в этом столбце и присваивает полю значение max+1.
Минимальное значение, которое можно получить с помощью автоинкремента - 1.