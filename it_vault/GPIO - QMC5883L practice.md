___
В данной статье разбирается процесс получения результатов от QMC5883L на raspberry pi.

___
Подключение контактов в целом интуитивно понятно.
- контакт vcc можно подключать либо 3V, либо к 5V.

Нужно установить следующие библиотеки:
- pip3 install qmc5883l (https://pypi.org/project/qmc5883l/)
- pip3 install smbus2

Код:
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import sys
from time import sleep
import qmc5883l as magn
 
def main():
    print("start")
    try:
        while True:
            mag_sens = magn.QMC5883L()
            [x, y, z] = mag_sens.get_magnet()
            print("x:", x, " y:", y, " z:", z)
            sleep(0.1)
    except KeyboardInterrupt:
        print("keyboard interrupt")
    print("finish")
    
if __name__ == '__main__':
    sys.exit(main())
```
