___
Dockerfile - файл с инструкциями для создания образа (image).

Докерфайл содержит набор инструкций и их аргументов в следующей форме:
```docker
INSTRUCTION1 args ...
INSTRUCTION2 args ...
INSTRUCTION3 args1 \
	arg2 arg3 ...
# comment
```

При создании образа все инструкции выполняются последовательно (сверху вниз).
Каждая новая инструкция добавляет еще один слой к образу.

Выполнение каждой инструкции происходит следующим образом:
- из имеющегося на данном этапе образа запускается контейнер
- в него вносятся изменения согласно инструкции
- полученный контейнер сохраняется в новый образ

___
### 1. Инструкции

##### 1.1. FROM

Инструкция `FROM` определяет базовый образ, на который будут накатываться дальнейшие изменения. Инструкция `FROM` обязательна и должна быть первой инструкцией в докерфайле.
##### 1.2. MAINTAINER

Информация об авторе. Включает имя автора и электронную почту:
- `Homer Simpson sayhellotomylittlefriend@mail.ru`
##### 1.3. RUN

Выполнить указанную команду в процессе построения образа.
##### 1.4. CMD и ENTRYPOINT

`CMD command | array_syntax_cmd`
- выполнить команду после запуска контейнера при условии, что в `docker run` команда не указана
- возможна только одна `CMD`-инструкция в докерфайле

`ENTRYPOINT command | array_syntax_cmd`
- выполнить команду после запуска контейнера
- все что указано `docker run` после container_id будет передано в указанную команду в качестве аргументов

Можно использовать обе инструкции следующим образом:
```
ENTRYPOINT ep_command
CMD default_args
```
В этом случае при запуске будет выполнена команда `ep_command <args>`, где 
Если в `docker run` после container_id приведены аргуметы, то они будут использованы в качестве `<args>`. В противном случае будут использованы `default_args`.
##### 1.5. WORKDIR

Инструкция `WORKDIR dir` выполняет следующее:
```bash
mkdir dir
cd dir
```

В дальнейшем текущая директория повлияет на результат инструкций: `ADD, CMD, RUN, COPY, ENTRYPOINT`.

Если в докерфайл отсутствует инструкция `WORKDIR`, то по умолчанию будет использована корневая директория (`/`).

Инструкция `WORKDIR` может встречаться несколько раз.
##### 1.6. ENV

Установить переменные окружения:
```docker
ENV VARNAME1=value1 VARNAME2=value2
```
Переменная окружения, установленная с помощью инструкции `ENV`:
- может быть использована в Dockerfile с помощью синтаксиса `$VARNAME`
- будет переменной окружения для любой последующей инструкции `RUN`
- будет переменной окружения внутри контейнера
##### 1.7. USER

С помощью инструкции `USER` можно указать пользователя, от имени которого будет выполняться команды в контейнере.

Нужно, чтобы указанное имя пользователя уже было зарегистрировано в системе:
```docker
RUN groupadd -r user && useradd -r -g user user
USER user
```
##### 1.8. VOLUME

С помощью команды `VOLUME` можно добавить один или несколько хранилищ.
```
VOLUME ["/opt/project", "/data" ]
```

Volume - это специальная директория на хосте, с которой могут взаимодействовать контейнеры.
##### 1.9. COPY

Инструкция ` COPY` копирует файл или директорию из build context (директории, в которой находится докерфайл) в образ.
```
COPY source_file /workdir
COPY source_dir/ /workdir
```
##### 1.10. ADD

Аналогична команде `COPY`, но дополнительно:
- позволяет копировать из URL
- распаковывает tar-архивы
##### 1.11. LABEL

Добавить метаданные в образ в формате ключ значение:
```
LABEL key1="value1" key2="value2" key3="value3"
```

После создания образа можно прочитать эти данные с помощью `docker inspect`.
##### 1.12. EXPOSE

Указать докеру, что контейнер должен слушать определенные порты.
```
EXPOSE port_number ...
```

Можно указать, что следует слушать UDP порт. Например `2000/udp`.
По умолчанию TCP.

На самом деле инструкция EXPOSE портов не открывает, а только указаывает их. Чтобы открыть порты нужно воспользоваться опциями `-p` или `-P` в `docker run`.
___
### 2. Array syntax

Существует два способа записи команд в Dockerfile:
```
RUN echo 'helloworld'
RUN [ "/bin/bash", "-c", "echo", "helloworld" ]  # array syntax
```
В первом случае будет выполнена команда с помощью shell:
- `/bin/sh -c echo 'helloworld'`
Поэтому рекомендуется при записи команд всегда использовать array-syntax.

___
### 3. Примерный шаблон для Dockerfile

Что должно быть примерно в каждом Dockerfile:
```
FROM base_image
MAINTAINER Homer Simpson homersimpson@mail.ru
WORKDIR /workdir
COPY binary_dir/ /workdir
RUN [ "apt-get", "-qq", "update" ]
RUN [ "apt-get", "install", "-y" "python3" ]
ENTRYPOINT [ "start_command" ]
CMD [ "start_command_default_args" ]
EXPOSE port_number
```



