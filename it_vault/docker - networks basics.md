___
Тема сетей в докере довольно обширная, однако здесь я рассмотрю только практические аспекты.
___

В docker есть три типа сетей:
- **_bridge**_ (по умолчанию) -> все контейнеры в этом сети будут соединены друг с другом (имеются в виду контейнеры запущенные в одном docker-engine)
- **_host**_ -> все контайнеры в этой сети могут взаимодействовать с сетью хоста, как если был они были процессами на хосте
- **_none**_ -> контейнеры в этой сети не имет никаких связей

Контейнер можно подключить к любой из этих сетей.

Все серверы внутри контейнеров следует подключать к интерфейсу `0.0.0.0`.
___

Чтобы соединить контейнеры не только между собой, нужно опубликовать их порты. Это производится в два этапа:

1. Следует включить в докерфайл инструкцию `EXPOSE`, где прописать все порты, которые следует опубликовать.
2. При запуске контейнера использовать опции `-p` или `-P`.

`docker run -p container_port_number`
- связать `container_port_number` в контейнере с рандомным портом на хосте
`docker run -p host_port_number:container_port_number`
- с `host_port_number` на хосте
`docker run -p interface:host_port_number:container_port_number`
- с интерфейсом `interface` на хосте
`docker run -P`
- связать все `EXPOSE`-порты в контейнере с рандомными портами хоста

Инструкция `EXPOSE` сама по себе не публикует порты, если не использовать опции `-p` или `-P` в команде `run`.

>[!note]
>По умолчанию связываются tcp-порты, чтобы указать udp нужно использовать синтаксис: `port_number/udp`.

Существует специальная команда с помощью которой можно посмотреть информацию об опубликованных портах контейнера:
- `docker port container_id`

Рабочий пример:
- сервер в контейнере слушает порт `0.0.0.0:80`
- в Dockerfile включена инструкция `EXPOSE 80`
- команде `run` вызвана с опцией `-p 192:168:213.200:2000:80`

В результате сервер в контейнере будет слушать порт на хосте `192:168:213.200:2000`.

