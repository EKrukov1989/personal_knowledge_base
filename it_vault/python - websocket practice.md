___
В данной статье я разберу практику создания простейшего сервера и клиента использующих websocket на python.

1. Создаем папку `folder1`
2. Открываем терминал в этой папке
3. Создаем виртуальное окружение и настраиваем его: 
```bash
python3 -m venv venv
source venv/bin/activate
pip install websockets
deactivate
```
4. Кладем в папку `folder1` два файла, со следующим содержимым:

client.py
```python
#!/usr/bin/env python
"""Client using the threading API."""

from websockets.sync.client import connect

def hello():
    with connect("ws://localhost:8765") as websocket:
        websocket.send("Hello from client!")
        message = websocket.recv()
        print(message)

if __name__ == "__main__":
    hello()
```

server.py
```python
#!/usr/bin/env python
"""Echo server using the threading API."""

from websockets.sync.server import serve

def echo(websocket):
    for message in websocket:
        websocket.send("Hello from server!")

def main():
    with serve(echo, "localhost", 8765) as server:
        server.serve_forever()

if __name__ == "__main__":
    main()
```

5. Запускаем сервер. Открываем терминал в `folder1` и вводим следующие команды:
```bash
source venv/bin/activate
./server.py
```
5. Запускаем клиент. Открываем терминал в `folder1` и вводим следующие команды:
```bash
source venv/bin/activate
./client.py
```

Ожидается, что в терминале из которого мы запускали клиент мы увидиме сообщение `"Hello from server!"`.
