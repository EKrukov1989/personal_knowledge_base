`ip address show` или `ip address` или `ip a` - показывает сетевые интерфейсы и связанные с ними IP-адреса (то же что и команда ifconfig, только лучше).
Пример вывода команды `ip -br a`:
```
lo       UNKNOWN   127.0.0.1/8 ::1/128 
enp3s0   DOWN       
wlp4s0   UP        192.168.212.37/24 fe80::9c36:973d:dc5a:af70/64
```
Если использовать опцию -d, то можно получить более подробный вывод.
Здесь:
`lo` - интерфейс loop-back, используется для диагностики
`enp3s0` - en -> ethernet, p0 -> bus-number, s10 -> slot number
`wlp4s0` - wl -> wlan

С помощью этой команды мы можем управлять IP-адресами. Можем смотреть, добавлять и удалять IP-адреса. Например:
```sh
ip address add [ip_address] dev [interface]
ip address del [ip_address] dev [interface]
```

Рассмотрим подробно, какую информацию нам дает эта команда.

## 1. Подробное рассмотрение вывода команды

Рассмотрим вывод команды на примере ip -j -d -h address для ноутбука.
Выводится json, вида:
```
[
    {
        "ifindex": 1,
        "ifname": "lo",
		...
    },
    {
        "ifindex": 2,
        "ifname": "enp3s0",
	    ...
    },
    {
        "ifindex": 3,
        "ifname": "wlp4s0",
        ...
    }
]
```
`ifindex` и `ifname` -> индекс и имя интерфейса, соответственно

### 1.1. Loopback (lo)
127.0.0.1 <-> lo <-> localhost <-> loopback
Такой IP-адрес принадлежит внутренней петле и может работать на компьютере даже без сетевой карты. Обычно ему присвоено имя localhost (`/etc/hosts`). Но на самом деле это не один IP-адрес: 127.0.0.1/8.
Например в самом начале файла `/etc/hosts` можно увидеть следующие строчки:
```
127.0.0.1 localhost
127.0.1.1 <hostname>
```
Эти строчки указывают на внутренюю петлю.

### 1.2. Wifi (wlp4s0)
Рассмотрим подробнее вывод для wlp4s0 с помощью команды:
```
ip -j -d -h address show wlp4s0
```
Вывод:
```
[
    {
        "ifindex": 3,
        "ifname": "wlp4s0",
        "flags": [
            "BROADCAST",
            "MULTICAST",
            "UP",
            "LOWER_UP"
        ],
        "mtu": 1500,
        "qdisc": "noqueue",
        "operstate": "UP",
        "group": "default",
        "txqlen": 1000,
        "link_type": "ether",
        "address": "74:4c:a1:90:ab:97",
        "broadcast": "ff:ff:ff:ff:ff:ff",
        "promiscuity": 0,
        "allmulti": 0,
        "min_mtu": 256,
        "max_mtu": 2304,
        "num_tx_queues": 1,
        "num_rx_queues": 1,
        "gso_max_size": 65536,
        "gso_max_segs": 65535,
        "tso_max_size": 65536,
        "tso_max_segs": 65535,
        "gro_max_size": 65536,
        "parentbus": "pci",
        "parentdev": "0000:04:00.0",
        "addr_info": [
            {
                "family": "inet",
                "local": "192.168.212.39",
                "prefixlen": 24,
                "broadcast": "192.168.212.255",
                "scope": "global",
                "dynamic": true,
                "noprefixroute": true,
                "label": "wlp4s0",
                "valid_life_time": 8430,
                "preferred_life_time": 8430
            },
            {
                "family": "inet6",
                "local": "fe80::9c36:973d:dc5a:af70",
                "prefixlen": 64,
                "scope": "link",
                "noprefixroute": true,
                "valid_life_time": 4294967295,
                "preferred_life_time": 4294967295
            }
        ]
    }
]
```

>[!note]
>Мы можем получить значение любого поля, с помощью команды:
>`sudo cat /sys/class/net/wlp4s0/<attribute_name>`

Полей очень много. Что действительно интересно?

##### link_type
`"link_type": "ether"` - хотя это интерфейс от Wi-fi. Дело в том, что Wi-Fi совместим с Ethernet настолько, что система может воспринимать соединение как ether.

##### физический адрес
`"address": "74:4c:a1:90:ab:97"` - физический адрес устройства
`"broadcast": "ff:ff:ff:ff:ff:ff"` - физический адрес для широковещания

##### operational state
`"operstate": "UP"` - operational state (RFC2863 state).
https://docs.kernel.org/networking/operstates.html
`UP` - работоспособен и включен
`DOWN` - интерфейс в порядке но не может работать, например кабель не подключен, или интерфейс намерено отключен
Есть и другие значения, которые означаются более экзотчечские состояния. В целом этот параметр предназначен для отображения как состояния самого интерфейса, так и примененных к нему административных мер.

##### flags
`BROADCAST`, `MULTICAST` - интерфейс поддерживает broadcast и multicast
`UP` - интерфейс готов к работе (драйвера загружены и работают)
`LOWER_UP` - есть соединение на физическом уровне, например кабель подключен
`NO-CARRIER` - кабель отключен

>[!note]
>В чем разница между flags и operstate?
>Можно увидеть это на примере сетевого интерфейса Ethernet с отключеным кабелем:
>	flags : \[NO-CARRIER, UP\] 
>	operstate : DOWN
>flags.UP - означает, что сетевой интерфейс готов к работе
>operstate.DOWN - означает что по факту он ничего не может сделать

##### IP-адреса
Для inet:
`"local": "192.168.212.39"` -> IP-адрес
`"broadcast": "192.168.212.255"` -> широковещательный адрес
`"prefixlen": 24` -> длина префикса
Для inet6:
`"local": "fe80::9c36:973d:dc5a:af70"`
`"prefixlen": 64`

>[!note]
>Здесь важно заметить, что IPv6 не поддерживает broadcast, поэтому семейство inet6 не содержит поля "boradcast".

##### как два IP-адреса работают вместе
Сразу бросается в глаза, что `addr_info` содержит два семейства адресов - inet и inet6. Легко представить, что пакеты могут приходить на любой из этих IP-адресов, но какой адрес указывается в качестве адреса отправителя?

В данном случае ответ очевиден.
inet -> scope : global
inet6 -> scope : link (видим только на самом устройстве)

##### Динамический или статический IP
`"dynamic": true` - означает, что адрес определяется DHCP-сервером.