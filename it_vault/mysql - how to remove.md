___
Как удалить Mysql на Ubuntu:

```bash
sudo service mysql stop
sudo killall -KILL mysql mysqld_safe mysqld
sudo apt remove mysql-client mysql-server -y
sudo apt autoremove -y
sudo apt autoclean -y
```
