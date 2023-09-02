___
В данной статье рассматривается практика работы обращения к mysql из python-кода.

В любом случае прежде всего следует установить коннектор:
```bash
pip3 install mysql-connector-python
```

Описание коннектора находится здесь: https://dev.mysql.com/doc/connector-python/en/
___
### Пример обращения к mysql из python-кода

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import sys
import mysql.connector as mycon

def main() -> None:
    word = 'fish'
    connection = mycon.connect(
        user='robot',
        password='svanidze',
        host='127.0.0.1',
        database='db_online_dictionary')
    cursor = connection.cursor()
    query = ('SELECT trans FROM translations WHERE word = \'{}\''.format(word) )
    cursor.execute(query)
    res = [t[0] for t in cursor]
    cursor.close()
    connection.close()
    print(res)

if __name__ == '__main__':
    sys.exit(main())
```
