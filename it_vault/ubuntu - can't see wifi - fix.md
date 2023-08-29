___
На основе: [wifigid.ru](https://wifigid.ru/reshenie-problem-i-oshibok/ubuntu-ne-vidit-wi-fi?ysclid=lk9zyusfs664596307)

1. Проверяем, что устройство видно с помощью команды:
```bash
sudo lspci
```
На моем acer-aspire это:
```
04:00.0 Network controller: MEDIATEK Corp. MT7921 802.11ax PCI Express Wireless Network Adapter
```

2. Смотрим список модулей:
```bash
sudo lsmod
```
Нас интересуют модули для Network Controller.
На моем acer-aspire это:
```
mt7921e
mt7921_common
```

3. Делаем modprobe модулю:
```bash
sudo modprobe <modulename>
```
На моем acer-aspire это:
```bash
sudo modprobe mt7921e
```
