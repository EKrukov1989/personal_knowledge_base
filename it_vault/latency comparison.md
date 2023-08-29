### Latency Comparison Numbers

	L1 cache reference                         0.5   ns
	Branch mispredict                            5   ns
	L2 cache reference                           7   ns
	Mutex lock/unlock                           25   ns
	Main memory reference                      100   ns
	Compress 1K bytes with Zippy            10,000   ns       10 us
	Send 1 KB bytes over 1 Gbps network     10,000   ns       10 us
	Read 4 KB randomly from SSD*           150,000   ns      150 us
	Read 1 MB sequentially from memory     250,000   ns      250 us
	Round trip within same datacenter      500,000   ns      500 us
	Read 1 MB sequentially from SSD*     1,000,000   ns    1,000 us    1 ms
	HDD seek                            10,000,000   ns   10,000 us   10 ms
	Read 1 MB sequentially from 1 Gbps  10,000,000   ns   10,000 us   10 ms
	Read 1 MB sequentially from HDD     30,000,000   ns   30,000 us   30 ms
	Send packet CA->Netherlands->CA    150,000,000   ns  150,000 us  150 ms

### Handy metrics based on numbers above:
	Read sequentially from HDD at 30 MB/s
	Read sequentially from 1 Gbps Ethernet at 100 MB/s
	Read sequentially from SSD at 1 GB/s
	Read sequentially from main memory at 4 GB/s
	6-7 world-wide round trips per second
	2,000 round trips per second within a data center

### What is the most important conclusions for me:

Comparisons between memory reference:

	L1 cache reference                         0.5   ns
	L2 cache reference                           7   ns
	Main memory reference                      100   ns
	Read 4 KB randomly from SSD*           150,000   ns      150 us
	HDD seek                            10,000,000   ns   10,000 us   10 ms

Comparison for sequential read:

	Read 1 MB sequentially from memory     250,000   ns      250 us
	Read 1 MB sequentially from SSD*     1,000,000   ns    1,000 us    1 ms
	Read 1 MB sequentially from HDD     30,000,000   ns   30,000 us   30 ms

Comparisons for networks:

	Send 1 KB over 1 Gbps network           10,000   ns       10 us
	Round trip within same datacenter      500,000   ns      500 us
	Send packet CA->Netherlands->CA    150,000,000   ns  150,000 us  150 ms

