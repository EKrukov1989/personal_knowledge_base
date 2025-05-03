___
Управление двигателем постоянного тока с помощью драйвера XY-160D.

Схема подключения:

![[Pasted image 20250503234449.png]]

Важно правильно подключить провода источника питания. Полярность имеет значение.
Подключение проводов к Arduino понятна из кода.

```cpp
#define PIN_POT A0
#define PIN_ENA D7
#define PIN_IN1 D6
#define PIN_IN2 D5

void setup()
{
	pinMode(PIN_POT, INPUT);
	pinMode(PIN_ENA, OUTPUT);
	pinMode(PIN_IN1, OUTPUT);
	pinMode(PIN_IN2, OUTPUT);
	digitalWrite(PIN_IN1, LOW);
	digitalWrite(PIN_IN2, HIGH);
}

void loop()
{
	int potValue = analogRead(PIN_POT);
	int pwmOutput = map(potValue, 0, 4095, 0, 255);
	analogWrite(PIN_ENA, pwmOutput);
	delay(100);
}

```