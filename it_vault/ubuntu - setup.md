___
## 1. Wi-Fi

Подключаем wi-fi.

___
### 2. Обновляем

```bash
sudo apt-get update
sudo apt-get upgrade
sudo reboot
```

___
### 3. Устанавливаем приложения

```bash
sudo apt-get install vim
sudo apt-get install meld
sudo apt-get install curl
sudo apt-get install gedit
sudo apt-get install veracrypt
sudo apt-get install kolourpaint
sudo snap install telegram-desktop
ln -s /media/gene /home/gene/flashdrives
```
Кроме того:
- install yandex browser
- install obsidian
- lxd (?)
- wireshark (?)
- visual studio code + настройка -> [[vscode - setup]]
- install and setup git -> [[git - global setup]]

___
### 4. bash-псевдонимы

Как добавлять bash-псевдонимы: [[bash - alias]]
```sh
alias gtext="gnome-text-editor"
alias prettyjson="python3 -m json.tool"
alias printpath="echo $PATH | sed 's/:/\n/g'"
alias brls="ls -1AN --group-directories-first"
alias fuls="ls -AhLN --group-directories-first"
alias printaslines="printf '[%s]\n'"
```

___
### 5. Настройка горячих клавиш

Настройка шорткатов: [[ubuntu - shortcuts]]

___
### 6. Добавление русской раскладки

1. settings -> region and language -> manage installed languages -> install/remove lanuages -> ( check russian language )
2. settings -> keyboard -> input sources -> ( add russian layout )

___
### 7. Запрет хостов

Для запрета хостов есть специальный набор файлов и все они храняться в папке:
- `scpipts/proh_hosts`
В этой папке храняться скрипты, а также список запрещенных хостов.

Нужно расположить всю эту папку по адресу:
`/home/gene/Desktop/station/scripts/`

Также следует добавить маленький скрипт в `/bin`, следующего содержания:
```bash
#!/bin/bash
"/home/gene/Desktop/station/scripts/proh_hosts/proh_hosts.scr" $@
```
Этот скрипт также можно найти в папке `proh_hosts`.

Чтобы обновить список запрещенных хостов достаточно ввести команду:
```bash
sudo proh_hosts
```
Чтобы запретить хост или хосты, нужно ввести команду:
```bash
sudo proh_hosts www.igromania.ru
sudo proh_hosts www.igromania.ru youtube.com yandex.com
```

___
### 8. Другие настройки

- settings -> displays -> multiple displays -> mirror
- Включаем фаервол: `sudo ufw enable`
- Добавить папку для скриптов и пользовательских программ
```
mkdir ~/bin
#add to ~/.bashr:
PATH=$PATH:$HOME/bin
```



На будущее:

Нужно сохранять обе пары ключей для гита и пароль к ним.
В этот файл (или лучше в файл настройки гита) добавить пункт переноса ключей