___
В данной статье рассмотрена базовая задача контейнеризации:
- развертывание сервера

___
### 1. Подготовка хоста

1.1. Установка Docker:
- `sudo apt-get install docker.io`
- Поставить `DEFAULT_FORWARD_POLICY="ACCEPT"` в файле `/etc/default/ufw`.
- `sudo ufw reload`

1.2. Открыть порт, который предполагается использовать:
- `ufw allow <port_number>`
- проверить можно с помощью `sudo ufw status`

___
### 2. Подготовка образа

Структура файлов:
```
build_context_dir
	whatever_server.py
	Dockerfile
```
whatever_server.py:
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import sys
import socket as st

def process_request(data):
    content = 'HTTP/1.1 200 OK\r\nContent-Type: text/plain\r\n\r\nwhatever'.encode('utf-8')
    return content

def main() -> None:
    print('echo server started and running.')
    server = st.socket(st.AF_INET, st.SOCK_STREAM)
    server.bind( ('0.0.0.0', 80) )
    server.listen(16)
    try:
        while True: 
            client_socket, address = server.accept()
            data = client_socket.recv(1024).decode('utf-8')
            content = process_request(data)
            client_socket.send(content)
            client_socket.shutdown(st.SHUT_WR)
    except KeyboardInterrupt:
        server.close()
        print('echo server closed.')

if __name__ == '__main__':
    sys.exit(main())
```
Dockerfile:
```docker
FROM ubuntu
WORKDIR /workdir
RUN ["apt", "update"]
RUN ["apt-get", "install", "-y", "python3"]
COPY whatever_server.py /workdir
ENTRYPOINT [ "./whatever_server.py" ]
EXPOSE 80
```

Собрать образ можно с помощью команды:
```bash
sudo docker build -t gene/whatever:v1 .
```

___
### 3. Запуск контейнера

Сначала образ нужно сохранить, перенести каким-то образом на хост где он будет запущен и загрузить:
```bash
sudo docker save -o whatever_image gene/whatever:v1
sudo docker load -i whatever_image
```

Затем запустить образ:
```bash
sudo docker run -d -p 192.168.213.200:2000:80 gene/whatever:v1
```
После чего сервер должен быть доступным по адресу `192.168.213.200:2000`.
