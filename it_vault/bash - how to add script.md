Чтобы bash-script можно было вызвать по имени из любой директории, нужно скопировать скрипт в `/usr/local/bin`:
```bash
sudo cp example.sh /usr/local/bin/example
```
или положить символьную ссылку:
```bash
sudo ln -s /home/me/scripts/example.sh /usr/local/bin/example
```
или добавить псевдоним [[bash - alias]]:
```bash
alias example=/home/me/scripts/example.sh
```