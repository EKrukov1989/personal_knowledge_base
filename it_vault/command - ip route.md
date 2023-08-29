Команда для управления таблицами маршрутизации ([[network - routing table]]).
Пример обычного вывода команды `ip route`:
```
default via 192.168.212.1 dev wlp4s0 proto dhcp src 192.168.212.39 metric 600 
169.254.0.0/16 dev enp3s0 scope link metric 1000 linkdown 
192.168.212.0/24 dev wlp4s0 proto kernel scope link src 192.168.212.39 metric 600 
```

Подробно рассмотрим вывод с помощью команды `ip -d -j -h route`:
```
[
    {
        "type": "unicast",
        "dst": "default",
        "gateway": "192.168.212.1",
        "dev": "wlp4s0",
        "protocol": "dhcp",
        "scope": "global",
        "prefsrc": "192.168.212.39",
        "metric": 600,
        "flags": []
    },
    {
        "type": "unicast",
        "dst": "169.254.0.0/16",
        "dev": "wlp4s0",
        "protocol": "boot",
        "scope": "link",
        "metric": 1000,
        "flags": []
    },
    {
        "type": "unicast",
        "dst": "192.168.212.0/24",
        "dev": "wlp4s0",
        "protocol": "kernel",
        "scope": "link",
        "prefsrc": "192.168.212.39",
        "metric": 600,
        "flags": []
    }
]
```

Значения полей (https://wiki.mikrotik.com/wiki/Manual:IP/Route):

##### dst
адрес назначения
default -> 0.0.0.0/0, что значит буквально - любой адрес

##### gateway
IP-адрес, куда пакет должен быть отправлен

##### dev
device - устройство для отправки пакета.

##### prefsrc
Локальный IP-адрес, который нужно указать в качестве пункта отправления. Только для пакетов, которые были созданы на этом устройстве.

Таблицу маршрутизации можно изменять вручную.