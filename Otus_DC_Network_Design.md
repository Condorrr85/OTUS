# Содержание:
    1. [Технологии построения фабрик](https://github.com/Condorrr85/OTUS/blob/main/Otus_DC_Network_Design.md#%D1%82%D0%B5%D1%85%D0%BD%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D0%B8-%D0%BF%D0%BE%D1%81%D1%82%D1%80%D0%BE%D0%B5%D0%BD%D0%B8%D1%8F-%D1%84%D0%B0%D0%B1%D1%80%D0%B8%D0%BA)
    2. [Построение Underlay сети(OSPF)](https://github.com/Condorrr85/OTUS/blob/main/Otus_DC_Network_Design.md#%D0%BF%D0%BE%D1%81%D1%82%D1%80%D0%BE%D0%B5%D0%BD%D0%B8%D0%B5-underlay-%D1%81%D0%B5%D1%82%D0%B8ospf)
    3. [Построение Underlay сети(IS-IS)](https://github.com/Condorrr85/OTUS/blob/main/Otus_DC_Network_Design.md#%D0%BF%D0%BE%D1%81%D1%82%D1%80%D0%BE%D0%B5%D0%BD%D0%B8%D0%B5-underlay-%D1%81%D0%B5%D1%82%D0%B8is-is)
    4. [Построение Underlay сети(BGP)](https://github.com/Condorrr85/OTUS/blob/main/Otus_DC_Network_Design.md#%D0%BF%D0%BE%D1%81%D1%82%D1%80%D0%BE%D0%B5%D0%BD%D0%B8%D0%B5-underlay-%D1%81%D0%B5%D1%82%D0%B8bgp) 

# Технологии построения фабрик

## _Домашнее задание №1_

## План работ

- Собрать топологию CLOS с 3 Spine и 4 Leaf.3 Leaf подключены к 2 Spine. 1 Leaf подключен к оставшемуся Spine. Все Spine связаны между собой через дополнительный коммутатор для L2 и L3 связанности Spine между собой (рекомендуется использовать IOL)
- 2 Leaf необходимо связать между собой для дальнейшей настройки VPC пары
- Добавите 3 клиента будущей фабрики. Один клиент подключен к VPC паре. Остальные клиенты подключены к оставшимся Leaf(в качестве клиентов рекомендуется использовать IOL образы)
- Распределить адресное пространство для Underlay сети

## Адресное пространство

### таблица ip-адресов

| Device | Interface | IP address | Subnet mask |  
| ----------- | ----------- | ----------- | ----------- |
| SPINE1 | E1/1 | 10.2.1.0 | 255.255.255.254 |
| | E1/2 | 10.2.1.2 | 255.255.255.254 |
|  | E1/3 | 10.2.1.4 | 255.255.255.254 |
|  | E1/4 | 10.2.1.6 | 255.255.255.254 |
|  | Lo0 | 10.255.1.101 | 255.255.255.255 |
| SPINE2 | E1/1 | 10.2.2.0  | 255.255.255.254 |
| | E1/2 | 10.2.2.2 | 255.255.255.254 |
|  | E1/3 | 10.2.2.4 | 255.255.255.254 |
|  | E1/4 | 10.2.2.6 | 255.255.255.254 |
|  | Lo0 | 10.255.1.102 | 255.255.255.255 |
| SPINE3 | E1/1 | 10.2.3.0 | 255.255.255.254 |
| | E1/2 | 10.2.3.2 | 255.255.255.254 |
| | Lo0 | 10.255.1.103 | 255.255.255.255 |
| LEAF1 | E1/1 | 10.2.1.3 | 255.255.255.254 |
|  | E1/2 | 10.2.2.3 | 255.255.255.254 |
|  | E1/3 | 172.16.1.1 | 255.255.255.252 |
|  | E1/4 | Port-channel1 |  |
|  | E1/5 | MCLAG to Client1 |  |
|  | E1/6 | Port-channel1 |  |
|  | Po1 | VPC peer-link |  |
|  | Lo0 | 10.255.1.11 | 255.255.255.255 |
| LEAF2 | E1/1 | 10.2.1.5 | 255.255.255.254 |
|  | E1/2 | 10.2.2.5 | 255.255.255.254 |
|  | E1/3 | 172.16.1.2 | 255.255.255.252 |
|  | E1/4 | Port-channel1 |  |
|  | E1/5 | MCLAG to Client1 | |
|  | E1/6 | Port-channel1 |  |
| | Po1 | VPC peer-link |  |
| | Lo0 | 10.255.1.12 | 255.255.255.255 |
| LEAF3 | E1/1 | 10.2.1.7 | 255.255.255.254 |
|  | E1/2 | 10.2.2.7 | 255.255.255.252 |
|  | E1/3 | 10.3.1.0 | 255.255.255.254 |
| | Lo0 | 10.255.1.13 | 255.255.255.255 |
| LEAF4 | E1/1 | 10.2.3.3 | 255.255.255.254 |
| | E1/2 | 10.4.1.0 | 255.255.255.254 |
| | Lo0 | 10.255.1.14 | 255.255.255.255 |
| Client1 | E0/0 | Port-channel 1 |  |
| | E0/1 | Port-channel 1 | |
| | Po1 | Header | Title |
| Client2 | E0/0 | - | - |
| Client3 | E0/0 | - | - |



## Схема сети

![Схема сети](https://github.com/Condorrr85/OTUS/blob/main/DC_Design.PNG)

## Настройки
[Настройки](https://github.com/Condorrr85/OTUS/tree/main/config)

# Построение Underlay сети(OSPF) 
## _Домашнее задание №2_

## План работ

- настроить протокол OSPF в качестве протокола маршрутизации Underlay сети для IP связанности между всеми устройствами NXOS
- Проверка работы протокола OSPF и доступности адресов Loopback


## Адресное пространство

### таблица ip-адресов

| Device | Interface | IP address | Subnet mask |  
| ----------- | ----------- | ----------- | ----------- |
| SPINE1 | E1/1 | IP unnumbered |  |
| | E1/2 | IP unnumbered |  |
|  | E1/3 | IP unnumbered |  |
|  | E1/4 | IP unnumbered |  |
|  | Lo0 | 10.255.1.101 | 255.255.255.255 |
| SPINE2 | E1/1 | IP unnumbered | |
| | E1/2 | IP unnumbered |  |
|  | E1/3 | IP unnumbered |  |
|  | E1/4 | IP unnumbered |  |
|  | Lo0 | 10.255.1.102 | 255.255.255.255 |
| SPINE3 | E1/1 | IP unnumbered | |
| | E1/2 | IP unnumbered |  |
| | Lo0 | 10.255.1.103 | 255.255.255.255 |
| LEAF1 | E1/1 | IP unnumbered | |
|  | E1/2 | IP unnumbered |  |
|  | E1/3 | 172.16.1.1 | 255.255.255.252 |
|  | E1/4 | Port-channel1 |  |
|  | E1/5 | MCLAG to Client1 |  |
|  | E1/6 | Port-channel1 |  |
|  | Po1 | VPC peer-link |  |
|  | Lo0 | 10.255.1.11 | 255.255.255.255 |
| LEAF2 | E1/1 | IP unnumbered |  |
|  | E1/2 | IP unnumbered ||
|  | E1/3 | 172.16.1.2 | 255.255.255.252 |
|  | E1/4 | Port-channel1 |  |
|  | E1/5 | MCLAG to Client1 | |
|  | E1/6 | Port-channel1 |  |
| | Po1 | VPC peer-link |  |
| | Lo0 | 10.255.1.12 | 255.255.255.255 |
| LEAF3 | E1/1 | IP unnumbered |  |
|  | E1/2 | IP unnumbered |  |
|  | E1/3 | trunk to Client2 |  |
| | Lo0 | 10.255.1.13 | 255.255.255.255 |
| LEAF4 | E1/1 | IP unnumbered |  |
| | E1/2 | trunk to Client2 |  |
| | Lo0 | 10.255.1.14 | 255.255.255.255 |
| Client1 | E0/0 | Port-channel 1 |  |
| | E0/1 | Port-channel 1 | |
| | Po1 | Trunk to VPC(Leaf1\Leaf2) |  |
| Client2 | E0/0 | Trunk to Leaf3 | - |
| Client3 | E0/0 | Trunk to Leaf4 | - |

## Настройка Underlay сети OSPF

[Настройки](https://github.com/Condorrr85/OTUS/tree/main/config/OSPF%20Underlay)

### Проверка доступности Loopback по Underlay OSPF сети на примере Leaf4
```
Leaf4# show ip route ospf-1
10.255.1.11/32, ubest/mbest: 1/0
    *via 10.255.1.103, Eth1/1, [110/131], 00:15:57, ospf-1, intra
10.255.1.12/32, ubest/mbest: 1/0
    *via 10.255.1.103, Eth1/1, [110/131], 00:15:57, ospf-1, intra
10.255.1.13/32, ubest/mbest: 1/0
    *via 10.255.1.103, Eth1/1, [110/131], 00:15:57, ospf-1, intra
10.255.1.101/32, ubest/mbest: 1/0
    *via 10.255.1.103, Eth1/1, [110/91], 00:15:57, ospf-1, intra
10.255.1.102/32, ubest/mbest: 1/0
    *via 10.255.1.103, Eth1/1, [110/91], 00:15:57, ospf-1, intra
10.255.1.103/32, ubest/mbest: 1/0
    *via 10.255.1.103, Eth1/1, [110/41], 00:15:57, ospf-1, intra
192.168.10.40/32, ubest/mbest: 1/0
    *via 10.255.1.103, Eth1/1, [110/81], 00:15:57, ospf-1, intra
```    

# Построение Underlay сети(IS-IS) 
## _Домашнее задание №3_

## План работ

- настроить протокол IS-IS в качестве протокола маршрутизации Underlay сети  для IP связанности между всеми устройствами NXOS
- Проверка IS-IS adjacency на LEAF коммутаторах
- Проверка  доступности адресов Loopback на примере коммутатора Leaf4


## Настройка протокола IS-IS в качестве протокола маршрутизации Underlay сети

[Настройки](https://github.com/Condorrr85/OTUS/tree/main/config/IS-IS%20Underlay)

### Проверка IS-IS adjacency на LEAF коммутаторах

``` 
Leaf1# show isis adjacency
IS-IS process: 1 VRF: default
IS-IS adjacency database:
Legend: '!': No AF level connectivity in given topology
System ID       SNPA            Level  State  Hold Time  Interface
Spine1          N/A             2      UP     00:00:31   Ethernet1/1
Spine2          N/A             2      UP     00:00:29   Ethernet1/2
``` 

``` 
Leaf2# show isis adjacency
IS-IS process: 1 VRF: default
IS-IS adjacency database:
Legend: '!': No AF level connectivity in given topology
System ID       SNPA            Level  State  Hold Time  Interface
Spine1          N/A             2      UP     00:00:26   Ethernet1/1
Spine2          N/A             2      UP     00:00:32   Ethernet1/2
``` 

``` 
Leaf3# show isis adjacency
IS-IS process: 1 VRF: default
IS-IS adjacency database:
Legend: '!': No AF level connectivity in given topology
System ID       SNPA            Level  State  Hold Time  Interface
Spine1          N/A             2      UP     00:00:32   Ethernet1/1
Spine2          N/A             2      UP     00:00:30   Ethernet1/2
``` 

```
Leaf4# show isis adjacency
IS-IS process: 1 VRF: default
IS-IS adjacency database:
Legend: '!': No AF level connectivity in given topology
System ID       SNPA            Level  State  Hold Time  Interface
Spine3          N/A             2      UP     00:00:28   Ethernet1/1
```

### Проверка  доступности адресов Loopback на примере коммутатора Leaf4
```
Leaf4# show ip route isis-1
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.255.1.11/32, ubest/mbest: 1/0
    *via 10.255.1.103, Eth1/1, [115/131], 00:23:06, isis-1, L2
10.255.1.12/32, ubest/mbest: 1/0
    *via 10.255.1.103, Eth1/1, [115/131], 00:23:06, isis-1, L2
10.255.1.13/32, ubest/mbest: 1/0
    *via 10.255.1.103, Eth1/1, [115/131], 00:23:06, isis-1, L2
10.255.1.101/32, ubest/mbest: 1/0
    *via 10.255.1.103, Eth1/1, [115/91], 00:23:06, isis-1, L2
10.255.1.102/32, ubest/mbest: 1/0
    *via 10.255.1.103, Eth1/1, [115/91], 00:23:06, isis-1, L2
10.255.1.103/32, ubest/mbest: 1/0
    *via 10.255.1.103, Eth1/1, [115/41], 00:23:07, isis-1, L2
     via 10.255.1.103, Eth1/1, [250/0], 00:23:11, am
192.168.10.40/32, ubest/mbest: 1/0
    *via 10.255.1.103, Eth1/1, [115/90], 00:23:06, isis-1, L2
```

# Построение Underlay сети(BGP) 
## _Домашнее задание №4_

## План работ

- настроить протокол BGP в качестве протокола маршрутизации Underlay сети  для IP связанности между всеми устройствами NXOS
- Проверка состояния BGP сессий на SPINE коммутаторах
- Проверка  доступности адресов Loopback и сетей PtP на примере коммутатора Spine1

### таблица ip-адресов и BGP AS Number

| Device | Interface | IP address | Subnet mask | BGP AS number |  
| ----------- | ----------- | ----------- | ----------- | ----------- |
| SPINE1 | E1/1 | 10.2.1.0 | 255.255.255.254 | 65501 |
| | E1/2 | 10.2.1.2 | 255.255.255.254 |
|  | E1/3 | 10.2.1.4 | 255.255.255.254 |
|  | E1/4 | 10.2.1.6 | 255.255.255.254 |
|  | Lo0 | 10.255.1.101 | 255.255.255.255 |
| SPINE2 | E1/1 | 10.2.2.0  | 255.255.255.254 | 65501 |
| | E1/2 | 10.2.2.2 | 255.255.255.254 |
|  | E1/3 | 10.2.2.4 | 255.255.255.254 |
|  | E1/4 | 10.2.2.6 | 255.255.255.254 |
|  | Lo0 | 10.255.1.102 | 255.255.255.255 |
| SPINE3 | E1/1 | 10.2.3.0 | 255.255.255.254 | 65501 |
| | E1/2 | 10.2.3.2 | 255.255.255.254 |
| | Lo0 | 10.255.1.103 | 255.255.255.255 |
| LEAF1 | E1/1 | 10.2.1.3 | 255.255.255.254 | 65511 |
|  | E1/2 | 10.2.2.3 | 255.255.255.254 |
|  | E1/3 | 172.16.1.1 | 255.255.255.252 |
|  | E1/4 | Port-channel1 |  |
|  | E1/5 | MCLAG to Client1 |  |
|  | E1/6 | Port-channel1 |  |
|  | Po1 | VPC peer-link |  |
|  | Lo0 | 10.255.1.11 | 255.255.255.255 |
| LEAF2 | E1/1 | 10.2.1.5 | 255.255.255.254 | 65512 |
|  | E1/2 | 10.2.2.5 | 255.255.255.254 |
|  | E1/3 | 172.16.1.2 | 255.255.255.252 |
|  | E1/4 | Port-channel1 |  |
|  | E1/5 | MCLAG to Client1 | |
|  | E1/6 | Port-channel1 |  |
| | Po1 | VPC peer-link |  |
| | Lo0 | 10.255.1.12 | 255.255.255.255 |
| LEAF3 | E1/1 | 10.2.1.7 | 255.255.255.254 | 65513 |
|  | E1/2 | 10.2.2.7 | 255.255.255.252 |
|  | E1/3 | 10.3.1.0 | 255.255.255.254 |
| | Lo0 | 10.255.1.13 | 255.255.255.255 |
| LEAF4 | E1/1 | 10.2.3.3 | 255.255.255.254 | 65514 |
| | E1/2 | 10.4.1.0 | 255.255.255.254 |
| | Lo0 | 10.255.1.14 | 255.255.255.255 |
| Client1 | E0/0 | Port-channel 1 |  |
| | E0/1 | Port-channel 1 | |
| | Po1 | - | - |
| Client2 | E0/0 | - | - |
| Client3 | E0/0 | - | - |
| R8 | E0/0 | 10.2.1.1 | 255.255.255.254 | 65508 |
| | E0/1 | 10.2.2.1 | 255.255.255.254 |
| | E0/2 | 10.2.3.1 | 255.255.255.254 |
| | Lo0 | 192.168.10.40 | 255.255.255.255 |

### Настройка протокола BGP в качестве протокола маршрутизации Underlay сети

[Настройки](https://github.com/Condorrr85/OTUS/tree/main/config/BGP%20Underlay)

### Проверка состояния BGP сессий на SPINE коммутаторах
``` 
Spine1# show ip bgp summary
BGP summary information for VRF default, address family IPv4 Unicast
BGP router identifier 10.255.1.101, local AS number 65501
BGP table version is 50, IPv4 Unicast config peers 4, capable peers 4
18 network entries and 32 paths using 5696 bytes of memory
BGP attribute entries [11/1804], BGP AS path entries [9/90]
BGP community entries [0/0], BGP clusterlist entries [0/0]
18 received paths for inbound soft reconfiguration
18 identical, 0 modified, 0 filtered received paths using 0 bytes

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.2.1.1        4 65508  224871  229658       50    0    0     1w0d 18
10.2.1.3        4 65511  227817  227793       50    0    0     1w0d 3
10.2.1.5        4 65512  227616  227567       50    0    0     1w0d 3
10.2.1.7        4 65513  227439  227429       50    0    0     1w0d 3

``` 

### Проверка  доступности адресов Loopback и сетей PtP на примере коммутатора Spine1

``` 
Spine1# show bgp ipv4 unicast neighbors 10.2.1.1 received-routes

Peer 10.2.1.1 routes for address family IPv4 Unicast:
BGP table version is 50, Local Router ID is 10.255.1.101
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-injected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - best2

   Network            Next Hop            Metric     LocPrf     Weight Path

* e10.2.1.0/31        10.2.1.1                 0                     0 65508 ?
* e10.2.1.2/31        10.2.1.1                                       0 65508 65501 ?
* e10.2.1.4/31        10.2.1.1                                       0 65508 65501 ?
* e10.2.1.6/31        10.2.1.1                                       0 65508 65501 ?
*>e10.2.2.0/31        10.2.1.1                 0                     0 65508 ?
* e10.2.2.2/31        10.2.1.1                                       0 65508 65501 ?
* e10.2.2.4/31        10.2.1.1                                       0 65508 65501 ?
* e10.2.2.6/31        10.2.1.1                                       0 65508 65501 ?
*>e10.2.3.0/31        10.2.1.1                 0                     0 65508 ?
*>e10.2.3.2/31        10.2.1.1                                       0 65508 65501 ?
* e10.255.1.11/32     10.2.1.1                                       0 65508 65501 65511 i
* e10.255.1.12/32     10.2.1.1                                       0 65508 65501 65512 ?
* e10.255.1.13/32     10.2.1.1                                       0 65508 65501 65513 ?
*>e10.255.1.14/32     10.2.1.1                                       0 65508 65501 65514 ?
* e10.255.1.101/32    10.2.1.1                                       0 65508 65501 ?
*>e10.255.1.102/32    10.2.1.1                                       0 65508 65501 ?
*>e10.255.1.103/32    10.2.1.1                                       0 65508 65501 ?
*>e192.168.10.40/32   10.2.1.1                 0                     0 65508 ?
``` 

### Проверка  доступности адресов Loopback и сетей PtP на примере коммутатора Leaf4

``` 
Leaf4# show ip route
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.2.1.0/31, ubest/mbest: 1/0
    *via 10.2.3.2, [20/0], 1w0d, bgp-65514, external, tag 65501
10.2.1.2/31, ubest/mbest: 1/0
    *via 10.2.3.2, [20/0], 1w0d, bgp-65514, external, tag 65501
10.2.1.4/31, ubest/mbest: 1/0
    *via 10.2.3.2, [20/0], 1w0d, bgp-65514, external, tag 65501
10.2.1.6/31, ubest/mbest: 1/0
    *via 10.2.3.2, [20/0], 1w0d, bgp-65514, external, tag 65501
10.2.2.0/31, ubest/mbest: 1/0
    *via 10.2.3.2, [20/0], 1w0d, bgp-65514, external, tag 65501
10.2.2.2/31, ubest/mbest: 1/0
    *via 10.2.3.2, [20/0], 1w0d, bgp-65514, external, tag 65501
10.2.2.4/31, ubest/mbest: 1/0
    *via 10.2.3.2, [20/0], 1w0d, bgp-65514, external, tag 65501
10.2.2.6/31, ubest/mbest: 1/0
    *via 10.2.3.2, [20/0], 1w0d, bgp-65514, external, tag 65501
10.2.3.0/31, ubest/mbest: 1/0
    *via 10.2.3.2, [20/0], 1w0d, bgp-65514, external, tag 65501
10.2.3.2/31, ubest/mbest: 1/0, attached
    *via 10.2.3.3, Eth1/1, [0/0], 1w0d, direct
10.2.3.3/32, ubest/mbest: 1/0, attached
    *via 10.2.3.3, Eth1/1, [0/0], 1w0d, local
10.255.1.11/32, ubest/mbest: 1/0
    *via 10.2.3.2, [20/0], 1w0d, bgp-65514, external, tag 65501
10.255.1.12/32, ubest/mbest: 1/0
    *via 10.2.3.2, [20/0], 1w0d, bgp-65514, external, tag 65501
10.255.1.13/32, ubest/mbest: 1/0
    *via 10.2.3.2, [20/0], 1w0d, bgp-65514, external, tag 65501
10.255.1.14/32, ubest/mbest: 2/0, attached
    *via 10.255.1.14, Lo0, [0/0], 1w1d, local
    *via 10.255.1.14, Lo0, [0/0], 1w1d, direct
10.255.1.101/32, ubest/mbest: 1/0
    *via 10.2.3.2, [20/0], 1w0d, bgp-65514, external, tag 65501
10.255.1.102/32, ubest/mbest: 1/0
    *via 10.2.3.2, [20/0], 1w0d, bgp-65514, external, tag 65501
10.255.1.103/32, ubest/mbest: 1/0
    *via 10.2.3.2, [20/0], 1w0d, bgp-65514, external, tag 65501
192.168.10.40/32, ubest/mbest: 1/0
    *via 10.2.3.2, [20/0], 1w0d, bgp-65514, external, tag 65501

``` 
```
Leaf4#  ping 10.255.1.101
PING 10.255.1.101 (10.255.1.101): 56 data bytes
64 bytes from 10.255.1.101: icmp_seq=0 ttl=252 time=62.814 ms
64 bytes from 10.255.1.101: icmp_seq=1 ttl=252 time=10.606 ms
64 bytes from 10.255.1.101: icmp_seq=2 ttl=252 time=11.359 ms
64 bytes from 10.255.1.101: icmp_seq=3 ttl=252 time=30.902 ms
64 bytes from 10.255.1.101: icmp_seq=4 ttl=252 time=8.481 ms

--- 10.255.1.101 ping statistics ---
5 packets transmitted, 5 packets received, 0.00% packet loss
round-trip min/avg/max = 8.481/24.832/62.814 ms
```
