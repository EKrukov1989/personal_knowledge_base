___
В данной статье рассматривается вопрос управления двигателем постоянного тока через мост L298N (без регулирования скорости).
### Компоненты

- Arduino NanoESP32
- L298N
___
### Подключение

1. Подключить выводы двигателя постоянного тока к контактам OUT1 и OUT2 в L298N.
2. Подключить выводы источника питания 12V к контактам GND и 12V  в L298N.
3. Перемычка Regulator должны быть установлена (в этом случае мы питаем регулятор отдельно, с Arduino).
4. Перемычка на ENA должна быть установлена (это значит двигатель работает на максимальной скорости).
5. Подключить вывод D7 (Arduino) к выводу IN1 (L298N).
6. Подключить вывод D8 (Arduino) к выводу IN2 (L298N).
7. Подключить вывод 3.3V (Arduino) к выводу 5V (L298N).
___
### Код для Arduino

```c
const int in1 = 8;
const int in2 = 7;

void setup()
{
	Serial.begin(115200);
	pinMode(in1, OUTPUT);
	pinMode(in2, OUTPUT);
}

void loop()
{
	Serial.println("ping");
	
	digitalWrite(in1, HIGH);
	digitalWrite(in2, LOW);
	delay(2000)
	
	digitalWrite(in1, LOW);
	digitalWrite(in2, LOW);
	delay(1000)
	
	digitalWrite(in1, LOW);
	digitalWrite(in2, HIGH);
	delay(2000)
	
	digitalWrite(in1, LOW);
	digitalWrite(in2, LOW);
	delay(1000)
}
```

