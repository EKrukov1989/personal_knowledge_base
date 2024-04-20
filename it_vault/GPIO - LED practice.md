___
Управление светодиодом на Raspberry PI 3B+ с помощью RPi.GPIO.
Предполагается, что уже установлен Ubuntu Server, на котором есть python3.
___
### 1. Настройка окружения

```bash
sudo apt-get update
sudo apt install python3-dev
sudo apt-get install build-essential
sudo apt install python3.8-venv
mkdir led
cd led
python3 -m venv venv
```

Далее согласно [[python - venv]]:
- добавить скрипт `act`
- создать файл `requirements.txt`

Устанавливаем пакеты:
```bash
source act (или . act)
pip3 install wheel
pip3 install --upgrade setuptools
pip3 install RPi.GPIO
```

___
### 2. Программа

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import sys
from time import sleep
import RPi.GPIO as gpio

LED_PIN = 11

def main() -> None:
    """LED control"""
    try:
        print ('Start')
        gpio.setmode(gpio.BOARD)
        gpio.setup(LED_PIN, gpio.OUT)
        for i in range(0,200):
            print(i)
            gpio.output(LED_PIN, i % 2)
            sleep(0.02)
        print ('loop finish')
    except KeyboardInterrupt:
        print ('keyboard interrupt')
    finally:
        gpio.cleanup()
        print ('GPIO cleanup executed')
    print('Finish')

if __name__ == '__main__':
    sys.exit(main())
```

___
### 3. Подключение и запуск

Подключение:
- подключить через резистор 1кОм
- короткую ножку светодиода соединить с GND
- длинную ножку светодиода подключить к любому GPIO порту, например 11

Обращаться к GPIO можно только с правами суперпользователя. Чтобы использовать виртуальное окружение от суперпользователя, то нужно запустить скрипт `act` также с правами суперпользователя. В итоге последовательность действий следующая:
```bash
sudo -s
. act
./led.py
```
