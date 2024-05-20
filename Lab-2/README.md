# Лабораторная работа №2
## Underlay. OSPF.

### Цель:

- Настроить протокол OSPF для Underlay сети
- Проверить связанность между устройствами

## Выполнение:

### Схема сети

![](images/UnderlayOSPF.png)

### Таблица адресов

| hostname | interface |   IP/MASK   | Description  |
| :------: | :-------: | :----------: | :---------: |
|  LEAF-1  | Loopback2 | 10.2.0.1 /32 | OSPF        |
|  LEAF-1  |  eth 1    | 10.4.1.1 /31 | OSPF        |
|  LEAF-1  |  eth 2    | 10.4.2.1 /31 | OSPF        |
|          |           |              |             |
|  LEAF-2  | Loopback2 | 10.2.0.2 /32 | OSPF        |
|  LEAF-2  |  eth 1    | 10.4.1.3 /31 | OSPF        |
|  LEAF-2  |  eth 2    | 10.4.2.3 /31 | OSPF        |
|          |           |              |             |
|  LEAF-3  | Loopback2 | 10.2.0.3 /32 | OSPF        |
|  LEAF-3  |  eth 1    | 10.4.1.5 /31 | OSPF        |
|  LEAF-3  |  eth 2    | 10.4.2.5 /31 | OSPF        |
|          |           |              |             |
| SPINE-1  | Loopback1 | 10.1.1.0/32  | OSPF        |
| SPINE-1  |  eth 1    | 10.4.1.0/31  | OSPF        |
| SPINE-1  |  eth 2    | 10.4.1.2/31  | OSPF        |
| SPINE-1  |  eth 3    | 10.4.1.4/31  | OSPF        |
|           |          |              |             |
| SPINE-2  | Loopback1 | 10.1.2.0/32  | OSPF        |
| SPINE-2  |  eth 1    | 10.4.2.0/31  | OSPF        |
| SPINE-2  |  eth 2    | 10.4.2.2/31  | OSPF        |
| SPINE-2  |  eth 3    | 10.4.2.2/31  | OSPF        |
### Конфигурация оборудования
- #### LEAF-1
```
hostname LEAF-1
interface Ethernet1
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Ethernet2
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Loopback2
   ip ospf area 0.0.0.0
router ospf 1
   router-id 10.1.0.1
   passive-interface default
   no passive-interface Ethernet1
   no passive-interface Ethernet2
   no passive-interface Loopback2
   max-lsa 12000
ip ospf router-id output-format hostnames
```
- #### LEAF-2
```
hostname LEAF-2
interface Ethernet1
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Ethernet2
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Loopback2
   ip ospf area 0.0.0.0
router ospf 1
   router-id 10.1.0.2
   passive-interface default
   no passive-interface Ethernet1
   no passive-interface Ethernet2
   no passive-interface Loopback2
   max-lsa 12000
ip ospf router-id output-format hostnames
```
- #### LEAF-3
```
hostname LEAF-3
interface Ethernet1
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Ethernet2
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Loopback2
   ip ospf area 0.0.0.0
router ospf 1
   router-id 10.1.0.3
   passive-interface default
   no passive-interface Ethernet1
   no passive-interface Ethernet2
   no passive-interface Loopback2
   max-lsa 12000
ip ospf router-id output-format hostnames
```
- #### SPINE-1
```
hostname SPINE-1
interface Ethernet1
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Ethernet2
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Ethernet3
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Loopback1
   ip ospf area 0.0.0.0
router ospf 1
   router-id 10.1.1.0
   passive-interface default
   no passive-interface Ethernet1
   no passive-interface Ethernet2
   no passive-interface Ethernet3
   no passive-interface Loopback1
   max-lsa 12000
ip ospf router-id output-format hostnames
```
- #### SPINE-2
```
hostname SPINE-2
interface Ethernet1
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Ethernet2
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Ethernet3
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
interface Loopback1
   ip ospf area 0.0.0.0
router ospf 1
   router-id 10.1.2.0
   passive-interface default
   no passive-interface Ethernet1
   no passive-interface Ethernet2
   no passive-interface Ethernet3
   no passive-interface Loopback1
   max-lsa 12000
ip ospf router-id output-format hostnames
```
### Проверка связанности устройств по протоколу OSPF
- #### SPINE-1
```
SPINE-1#sho ip os neighbor
Neighbor ID     Instance VRF      Pri State                  Dead Time   Address         Interface
10.1.0.1        1        default  0   FULL                   00:00:33    10.4.1.1        Ethernet1
10.1.0.2        1        default  0   FULL                   00:00:29    10.4.1.3        Ethernet2
10.1.0.3        1        default  0   FULL                   00:00:34    10.4.1.5        Ethernet3
```
- #### SPINE-2
```
SPINE-2#sho ip os neighbor
Neighbor ID     Instance VRF      Pri State                  Dead Time   Address         Interface
10.1.0.1        1        default  0   FULL                   00:00:37    10.4.2.1        Ethernet1
10.1.0.2        1        default  0   FULL                   00:00:34    10.4.2.3        Ethernet2
10.1.0.3        1        default  0   FULL                   00:00:34    10.4.2.5        Ethernet3
```
- ### PING
- #### SPINE-1
```
SPINE-1#ping 10.1.0.1
PING 10.1.0.1 (10.1.0.1) 72(100) bytes of data.
80 bytes from 10.1.0.1: icmp_seq=1 ttl=64 time=6.53 ms
80 bytes from 10.1.0.1: icmp_seq=2 ttl=64 time=3.84 ms
80 bytes from 10.1.0.1: icmp_seq=3 ttl=64 time=3.67 ms
80 bytes from 10.1.0.1: icmp_seq=4 ttl=64 time=4.92 ms
80 bytes from 10.1.0.1: icmp_seq=5 ttl=64 time=3.83 ms

--- 10.1.0.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 26ms
rtt min/avg/max/mdev = 3.673/4.564/6.536/1.083 ms, ipg/ewma 6.673/5.523 ms
SPINE-1#ping 10.1.0.2
PING 10.1.0.2 (10.1.0.2) 72(100) bytes of data.
80 bytes from 10.1.0.2: icmp_seq=1 ttl=64 time=5.41 ms
80 bytes from 10.1.0.2: icmp_seq=2 ttl=64 time=3.94 ms
80 bytes from 10.1.0.2: icmp_seq=3 ttl=64 time=4.00 ms
80 bytes from 10.1.0.2: icmp_seq=4 ttl=64 time=6.13 ms
80 bytes from 10.1.0.2: icmp_seq=5 ttl=64 time=3.88 ms

--- 10.1.0.2 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 29ms
rtt min/avg/max/mdev = 3.886/4.676/6.132/0.928 ms, ipg/ewma 7.444/5.045 ms
SPINE-1#ping 10.1.0.3
PING 10.1.0.3 (10.1.0.3) 72(100) bytes of data.
80 bytes from 10.1.0.3: icmp_seq=1 ttl=64 time=5.71 ms
80 bytes from 10.1.0.3: icmp_seq=2 ttl=64 time=3.52 ms
80 bytes from 10.1.0.3: icmp_seq=3 ttl=64 time=4.62 ms
80 bytes from 10.1.0.3: icmp_seq=4 ttl=64 time=5.12 ms
80 bytes from 10.1.0.3: icmp_seq=5 ttl=64 time=4.13 ms

--- 10.1.0.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 24ms
rtt min/avg/max/mdev = 3.522/4.623/5.718/0.766 ms, ipg/ewma 6.231/5.166 ms
```
- #### SPINE-2
```
SPINE-2#ping 10.1.0.1
PING 10.1.0.1 (10.1.0.1) 72(100) bytes of data.
80 bytes from 10.1.0.1: icmp_seq=1 ttl=64 time=6.43 ms
80 bytes from 10.1.0.1: icmp_seq=2 ttl=64 time=3.94 ms
80 bytes from 10.1.0.1: icmp_seq=3 ttl=64 time=3.88 ms
80 bytes from 10.1.0.1: icmp_seq=4 ttl=64 time=4.21 ms
80 bytes from 10.1.0.1: icmp_seq=5 ttl=64 time=3.82 ms

--- 10.1.0.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 25ms
rtt min/avg/max/mdev = 3.827/4.461/6.433/0.994 ms, ipg/ewma 6.436/5.412 ms
SPINE-2#ping 10.1.0.2
PING 10.1.0.2 (10.1.0.2) 72(100) bytes of data.
80 bytes from 10.1.0.2: icmp_seq=1 ttl=64 time=7.90 ms
80 bytes from 10.1.0.2: icmp_seq=2 ttl=64 time=4.34 ms
80 bytes from 10.1.0.2: icmp_seq=3 ttl=64 time=3.91 ms
80 bytes from 10.1.0.2: icmp_seq=4 ttl=64 time=4.24 ms
80 bytes from 10.1.0.2: icmp_seq=5 ttl=64 time=4.30 ms

--- 10.1.0.2 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 31ms
rtt min/avg/max/mdev = 3.914/4.944/7.909/1.490 ms, ipg/ewma 7.799/6.377 ms
SPINE-2#ping 10.1.0.3
PING 10.1.0.3 (10.1.0.3) 72(100) bytes of data.
80 bytes from 10.1.0.3: icmp_seq=1 ttl=64 time=6.44 ms
80 bytes from 10.1.0.3: icmp_seq=2 ttl=64 time=4.81 ms
80 bytes from 10.1.0.3: icmp_seq=3 ttl=64 time=4.29 ms
80 bytes from 10.1.0.3: icmp_seq=4 ttl=64 time=4.29 ms
80 bytes from 10.1.0.3: icmp_seq=5 ttl=64 time=3.87 ms

--- 10.1.0.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 25ms
rtt min/avg/max/mdev = 3.871/4.745/6.449/0.903 ms, ipg/ewma 6.476/5.548 ms
```