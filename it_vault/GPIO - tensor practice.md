___
### 1. Подключение

Плата HX7111 имеет два независимых канала:
- А (коэффициент усиления по входу 64)
- B (коэффициент усиления по входу 128)
Можно подключать к одной плате два независимых датчика.

Подключение тензодатчика к АЦП:
- красный -> E+
- черный -> E-
- белый -> A-
- зеленый -> A+

Подключение АЦП к Raspberry:
- GND -> GND
- DT -> Pin 29 (GPIO 5)
- SCK -> Pin 31 (GPIO 6)
- VCC - 3.3V
___
### 2. Настройка

Сначала нужно настроить как в LED-практике ([[GPIO - LED practice]]).

Устанавливаем пакеты:
```bash
source act (или . act)
pip3 install hx711
```

___
### 3. Программа

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import sys
from time import sleep
from hx711 import HX711
import RPi.GPIO as gpio

DT_PIN = 5
SCK_PIN = 6

def main() -> None:
    """Force measuring"""
    try:
        print ('Start')
        hx711 = HX711(dout_pin=DT_PIN, pd_sck_pin=SCK_PIN, channel='A', gain=64)
        hx711.reset()
        for i in range(0,200):
            measures = hx711.get_raw_data(2)
            print(i, " ", measures[0])
        print ('Loop finish')
    except KeyboardInterrupt:
        print ('keyboard interrupt')
    finally:
        gpio.cleanup()
        print ('GPIO cleanup executed')
    print('Finish')

if __name__ == '__main__':
    sys.exit(main())
```

>[!warning]
>К сожалению, такая способ позволяет получить не слишком отзывчивый датчик. Скорость обновления - около 5Hz.

