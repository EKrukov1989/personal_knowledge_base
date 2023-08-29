___
Допустим у нас есть два компьютера с установленной операционной системой ubuntu 23.04. Наша задача - подключиться по SSH с одного компьютера к другому используя пароль для аутентификации.

>[!note]
>В дальнейшем я буду использовать следующую терминологию:
>- client-PC - компьютер, с которого я подключаюсь
>- server-PC -> компьютер, к которомя я подключаюсь

___
### 1. Предварительные приготовления

Во-первых, нужно обеспечить подключение через Ethernet-кабель, или через Wi-Fi. Во-вторых - сконфигурировать сеть. Как конфигурировать сеть описано здесь: [[network - static LAN configuring]].
Проверить наличие соединения можно с помощью команды `ping`.

___
### 2. Настройка firewall

Общая информация для настройки firewall на ubuntu: [[network - iptables]] [[network - UFW]].
На Client-PC достаточно дефолтных настроек UFW.
На Server-PC нужно открыть порт для SSH: `sudo ufw allow ssh`. 
Проверить состояние UFW можно с помощью команды: `sudo ufw status`

___
### 3. Установка и настройка OpenSSH-server

По умолчанию, на ubuntu ssh-сервер не установлен. Можно проверить наличие и состояние ssh-сервера с помощью команды `sudo systemctl list-unit-files`.

Установить ssh-сервер можно с помощью команды:
`sudo apt install openssh-server`
`sudo systemctl enable ssh` -> включить автозапуск сервера
`sudo systemctl start ssh` -> запустить сервер
`sudo systemctl status ssh.service` -> проверить статус сервера

После проведения всех этих операций можно проверить готовность сервера запустив на Client-PC команду `sudo nmap -PS <server_pc_IP>`. Порт ssh должен быть открыт:
```
Nmap scan report for nwtpc (192.168.213.200)
Host is up (0.00032s latency).
Not shown: 999 filtered tcp ports (no-response)
PORT   STATE SERVICE
22/tcp open  ssh
MAC Address: 38:D5:47:25:78:4E (Asustek Computer)
```

В ubuntu ssh аутентификация через пароль по умолчанию включена. Это можно проверить:
-  в файле `/etc/ssh/sshd_config` строка `PasswordAuthentication` должна быть закомментирована или же должно быть указано `PasswordAuthentication yes`.

___
### 4. Подключение

При подключении по ssh к удаленному компьютеру мы подключаемся от имени какого-то пользователя. Который уже должен быть зарегистрирован на удаленном компьютере.

На Client-PC вводим команду: `ssh -l <server_pc_login> <server_pc_IP>`
После чего вводим пароль для пользователя `<server_pc_login>`.
Доступ должен быть предоставлен.

Если мы вперые подключаемся к этому хосту, то скорее всего ssh выведет на экран следующее предупреждение:
```
The authenticity of host '<hostname> (192.168.213.200)' can't be established.
ED25519 key fingerprint is SHA256:vaOB+ra/w/CX3TGGRwJBjdcZ0XXXXXXXXXXXXXXXXX.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
```

Нужно проверить указанный фингерпринт, действительно ли он соответствует публичному ключу хоста, к которому нам нужно подключиться.
Более подробно про фингерпринты: [[ssh - public and private keys]]

Если выбрать yes, то ключ будет добавлен в known_hosts:
```
Warning: Permanently added '<hostname>' (ED25519) to the list of known hosts.
```

>[!tip] Ctrl + D
Закрыть ssh-сессию можно с помощью сочетания клавиш `Ctrl-D`.