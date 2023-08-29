Минимальный python скрипт должен содержать следующее:

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import sys
from typing import List, Dict, Tuple

def main() -> None:
    """Description"""
    print ('Hello, World!')

if __name__ == '__main__':
    sys.exit(main())
```

##### Подробнее:

`#!/usr/bin/env python3` -> shebang. Указывает на интерпретатор, который следует использовать при запуске.

`# -*- coding: utf-8 -*-` -> python source code encodings. определяет кодировку в файле. Используется разными редакторами

`sys.exit(main())` -> корректный способ обработки ошибок

`from typing import List, Dict, Tuple` - поддержка type hints

`if __name__ == '__main__':`  -> если данный файл был вызван непосредственно, а не через другой скрипт, то это утверждение будет верным. И будет вызвана функция main.
