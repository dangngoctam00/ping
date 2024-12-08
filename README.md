# ping

Example imcp protocol implemented by raw socket in golang using unix package. For learning purpose, it only captures popular icmp message type such as echo reply, destination unreachable.

# example

Testing in container with following network interface
```
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.2  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:ac:11:00:02  txqueuelen 0  (Ethernet)
        RX packets 823  bytes 355599 (355.5 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 684  bytes 57966 (57.9 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 1950  bytes 112766 (112.7 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1950  bytes 112766 (112.7 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

veth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 20.1.0.10  netmask 255.255.255.0  broadcast 0.0.0.0
        ether 3e:e3:1e:81:64:c9  txqueuelen 1000  (Ethernet)
        RX packets 78  bytes 3276 (3.2 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 703  bytes 51176 (51.1 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

veth2: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 20.1.0.11  netmask 255.255.255.0  broadcast 0.0.0.0
        ether 2e:1f:92:cb:90:99  txqueuelen 1000  (Ethernet)
        RX packets 703  bytes 51176 (51.1 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 78  bytes 3276 (3.2 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

1) Ping to ip address `20.1.0.11` via veth `veth1`
```bash
go run main.go 5 20.1.0.11 veth1

ping to: 20.1.0.11, response: ping reply, seq: 1, latency:0.000414
ping to: 20.1.0.11, response: ping reply, seq: 2, latency:0.000079
ping to: 20.1.0.11, response: ping reply, seq: 3, latency:0.000072
ping to: 20.1.0.11, response: ping reply, seq: 4, latency:0.000069
ping to: 20.1.0.11, response: ping reply, seq: 5, latency:0.000057
```

2) Ping to ip address `20.1.0.12` (unknown address) via `veth1`
```bash
go run main.go 5 20.1.0.12 veth1

ping to: 20.1.0.12, response: destination unreachable, seq: 1, latency:3.102430
ping to: 20.1.0.12, response: destination unreachable, seq: 2, latency:3.102498
ping to: 20.1.0.12, response: destination unreachable, seq: 3, latency:3.102499
ping to: 20.1.0.12, response: destination unreachable, seq: 4, latency:3.102500
ping to: 20.1.0.12, response: destination unreachable, seq: 5, latency:3.102498
```
