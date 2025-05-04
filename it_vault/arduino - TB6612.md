___
Подключение:
- VM - источник питания "+"
- VCC - 3.3V arduino
- GND - источник питания "-", а также GND  arduino
- AO1 - двигатель
- AO2 - двигатель
- PWMA - D7 arduino
- AIN1 - D6 arduino
- AIN2 - D5 arduino
- STBY - 3.3V arduino

Код:
```c
#define PIN_PWMA D7
#define PIN_AIN1 D6
#define PIN_AIN2 D5

void setup()
{
  Serial.begin(115200);
  pinMode(PIN_PWMA, OUTPUT);
  pinMode(PIN_AIN1, OUTPUT);
  pinMode(PIN_AIN2, OUTPUT);
  digitalWrite(PIN_AIN1, LOW);
  digitalWrite(PIN_AIN2, LOW);
  analogWrite(PIN_PWMA, 210);
}

void loop()
{
  Serial.println("ping3");
  digitalWrite(PIN_AIN1, HIGH);
  digitalWrite(PIN_AIN2, LOW);
  delay(2000);

  digitalWrite(PIN_AIN1, LOW);
  digitalWrite(PIN_AIN2, LOW);
  delay(1000);

  digitalWrite(PIN_AIN1, LOW);
  digitalWrite(PIN_AIN2, HIGH);
  delay(2000);

  digitalWrite(PIN_AIN1, LOW);
  digitalWrite(PIN_AIN2, LOW);
  delay(1000);
}
```
