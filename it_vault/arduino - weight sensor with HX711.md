___
Как получить показания с тензодатчика.

___
### Подключение

Тензодатчик имеет четыре разноцветных провода. HX711 имеет 6 контактов для соединения с тензодатчиками и 4 контакта для соединения с Arduino. Соединять следует следующим образом:
- красный провод (тензодатчик) -> E+ (HX711)
- черный провод (тензодатчик) -> E- (HX711)
- белый провод (тензодатчик) -> A- (HX711)
- зеленый провод (тензодатчик) -> A+ (HX711)

- GND (HX711) -> GND (Arduino)
- VCC (HX711) -> 3.3V (Arduino)
- DT (HX711) -> D2 (Arduino)
- SCK (HX711) -> D3 (Arduino)

___
### Установка библиотеки

sketch -> include library -> manage libraries.
Добавить библиотеку HX711 Arduino Library by Bogdan Necula.

___
### Код
```c
#include "HX711.h"
#define LOADCELL_DOUT_PIN = 2;
#define LOADCELL_SCK_PIN = 3;

HX711 scale;

void setup()
{
	Serial.begin(115200);
	scale.begin(LOADCELL_DOUT_PIN, LOADCELL_SCK_PIN);
	scale.set_scale(1.f);
	scale.tare();
}

void loop()
{
	if (scale.is_ready())
	{
		float weight = scale.get_units(5);
		Serial.print(weight);
	}
	else
	{
		Serial.println("HX711 not found");
	}
	delay(500);
}
```
