___
Это всего лишь обертка, котороя вызывает ssh и прикрепляет к ней имя вызвавшей программы. Использовать можно следующим образом:

Создаем софтлинки с именнами интересных нам хостов.
```sh
sudo ln -sf /usr/bin/ssh-argv0 /usr/local/bin/login1@hostname1
sudo ln -sf /usr/bin/ssh-argv0 /usr/local/bin/login2@hostname2
```
Теперь следующие две команды будут эквивалентны:
```sh
ssh login1@hostname1 ...
login1@hostname1 ...
```
Что сэкономит аж 4 символа.