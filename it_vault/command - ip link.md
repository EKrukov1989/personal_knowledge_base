Управление сетевыми устройствами. Пример вывода командs `ip -br link`:
```
lo      UNKNOWN  00:00:00:00:00:00 <LOOPBACK,UP,LOWER_UP> 
enp3s0  DOWN     08:8f:c3:25:7f:38 <NO-CARRIER,BROADCAST,MULTICAST,UP> 
wlp4s0  UP       74:4c:a1:90:ab:97 <BROADCAST,MULTICAST,UP,LOWER_UP>
```
С помощью этой команды можно добавлять/удалять, включать/выключать, а также настраивать сетевые устройства. Например:
```sh
ip link set [interface] up    # включить интерфейс
ip link set [interface] down  # выключить интерфейс
```

Пример вывода команды `ip -d -j -h link`:

```
[
    {
        "ifindex": 1,
        "ifname": "lo",
        "flags": [
            "LOOPBACK",
            "UP",
            "LOWER_UP"
        ],
        "mtu": 65536,
        "qdisc": "noqueue",
        "operstate": "UNKNOWN",
        "linkmode": "DEFAULT",
        "group": "default",
        "txqlen": 1000,
        "link_type": "loopback",
        "address": "00:00:00:00:00:00",
        "broadcast": "00:00:00:00:00:00",
        "promiscuity": 0,
        "allmulti": 0,
        "min_mtu": 0,
        "max_mtu": 0,
        "inet6_addr_gen_mode": "eui64",
        "num_tx_queues": 1,
        "num_rx_queues": 1,
        "gso_max_size": 65536,
        "gso_max_segs": 65535,
        "tso_max_size": 524280,
        "tso_max_segs": 65535,
        "gro_max_size": 65536
    },
    {
        "ifindex": 2,
        "ifname": "enp3s0",
        "flags": [
            "NO-CARRIER",
            "BROADCAST",
            "MULTICAST",
            "UP"
        ],
        "mtu": 1500,
        "qdisc": "pfifo_fast",
        "operstate": "DOWN",
        "linkmode": "DEFAULT",
        "group": "default",
        "txqlen": 1000,
        "link_type": "ether",
        "address": "08:8f:c3:25:7f:38",
        "broadcast": "ff:ff:ff:ff:ff:ff",
        "promiscuity": 0,
        "allmulti": 0,
        "min_mtu": 68,
        "max_mtu": 9194,
        "inet6_addr_gen_mode": "none",
        "num_tx_queues": 1,
        "num_rx_queues": 1,
        "gso_max_size": 64000,
        "gso_max_segs": 64,
        "tso_max_size": 64000,
        "tso_max_segs": 64,
        "gro_max_size": 65536,
        "parentbus": "pci",
        "parentdev": "0000:03:00.0"
    },
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
        "linkmode": "DORMANT",
        "group": "default",
        "txqlen": 1000,
        "link_type": "ether",
        "address": "74:4c:a1:90:ab:97",
        "broadcast": "ff:ff:ff:ff:ff:ff",
        "promiscuity": 0,
        "allmulti": 0,
        "min_mtu": 256,
        "max_mtu": 2304,
        "inet6_addr_gen_mode": "none",
        "num_tx_queues": 1,
        "num_rx_queues": 1,
        "gso_max_size": 65536,
        "gso_max_segs": 65535,
        "tso_max_size": 65536,
        "tso_max_segs": 65535,
        "gro_max_size": 65536,
        "parentbus": "pci",
        "parentdev": "0000:04:00.0"
    }
]
```