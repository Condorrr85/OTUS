# Дизайн сетей ЦОД
## Содержание:
  1. [Технологии построения фабрик](https://github.com/Condorrr85/OTUS/blob/main/Otus_DC_Network_Design.md#%D1%82%D0%B5%D1%85%D0%BD%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D0%B8-%D0%BF%D0%BE%D1%81%D1%82%D1%80%D0%BE%D0%B5%D0%BD%D0%B8%D1%8F-%D1%84%D0%B0%D0%B1%D1%80%D0%B8%D0%BA)
2. [Построение Underlay сети(OSPF)](https://github.com/Condorrr85/OTUS/blob/main/Otus_DC_Network_Design.md#%D0%BF%D0%BE%D1%81%D1%82%D1%80%D0%BE%D0%B5%D0%BD%D0%B8%D0%B5-underlay-%D1%81%D0%B5%D1%82%D0%B8ospf)
3. [Построение Underlay сети(IS-IS)](https://github.com/Condorrr85/OTUS/blob/main/Otus_DC_Network_Design.md#%D0%BF%D0%BE%D1%81%D1%82%D1%80%D0%BE%D0%B5%D0%BD%D0%B8%D0%B5-underlay-%D1%81%D0%B5%D1%82%D0%B8is-is)
4. [Построение Underlay сети(BGP)](https://github.com/Condorrr85/OTUS/blob/main/Otus_DC_Network_Design.md#%D0%BF%D0%BE%D1%81%D1%82%D1%80%D0%BE%D0%B5%D0%BD%D0%B8%D0%B5-underlay-%D1%81%D0%B5%D1%82%D0%B8bgp)
5. [Multicast PIM (Sparse)](https://github.com/Condorrr85/OTUS/blob/main/Otus_DC_Network_Design.md#multicast-pim-sparse)
6. [VXLAN/Multicast](https://github.com/Condorrr85/OTUS/blob/main/Otus_DC_Network_Design.md#vxlanmulticast)
7. [VxLAN. Route type 2](https://github.com/Condorrr85/OTUS/blob/main/Otus_DC_Network_Design.md#vxlan-route-type-2)
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

[Схема сети](https://github.com/Condorrr85/OTUS/blob/main/DC_Design.PNG)

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

### Проверка  доступности адресов Loopback и сетей PtP на примере коммутатора Leaf1

``` 
Leaf1# show ip route
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

10.2.1.0/31, ubest/mbest: 1/0
    *via 10.2.1.2, [20/0], 2w0d, bgp-65511, external, tag 65501
10.2.1.2/31, ubest/mbest: 1/0, attached
    *via 10.2.1.3, Eth1/1, [0/0], 2w0d, direct
10.2.1.3/32, ubest/mbest: 1/0, attached
    *via 10.2.1.3, Eth1/1, [0/0], 2w0d, local
10.2.1.4/31, ubest/mbest: 1/0
    *via 10.2.1.2, [20/0], 2w0d, bgp-65511, external, tag 65501
10.2.1.6/31, ubest/mbest: 1/0
    *via 10.2.1.2, [20/0], 2w0d, bgp-65511, external, tag 65501
10.2.2.0/31, ubest/mbest: 1/0
    *via 10.2.2.2, [20/0], 2w0d, bgp-65511, external, tag 65501
10.2.2.2/31, ubest/mbest: 1/0, attached
    *via 10.2.2.3, Eth1/2, [0/0], 2w0d, direct
10.2.2.3/32, ubest/mbest: 1/0, attached
    *via 10.2.2.3, Eth1/2, [0/0], 2w0d, local
10.2.2.4/31, ubest/mbest: 1/0
    *via 10.2.2.2, [20/0], 2w0d, bgp-65511, external, tag 65501
10.2.2.6/31, ubest/mbest: 1/0
    *via 10.2.2.2, [20/0], 2w0d, bgp-65511, external, tag 65501
10.2.3.0/31, ubest/mbest: 2/0
    *via 10.2.1.2, [20/0], 2w0d, bgp-65511, external, tag 65501
    *via 10.2.2.2, [20/0], 2w0d, bgp-65511, external, tag 65501
10.2.3.2/31, ubest/mbest: 2/0
    *via 10.2.1.2, [20/0], 00:16:44, bgp-65511, external, tag 65501
    *via 10.2.2.2, [20/0], 00:16:44, bgp-65511, external, tag 65501
10.255.1.11/32, ubest/mbest: 2/0, attached
    *via 10.255.1.11, Lo0, [0/0], 2w1d, local
    *via 10.255.1.11, Lo0, [0/0], 2w1d, direct
10.255.1.12/32, ubest/mbest: 2/0
    *via 10.2.1.2, [20/0], 2w0d, bgp-65511, external, tag 65501
    *via 10.2.2.2, [20/0], 2w0d, bgp-65511, external, tag 65501
10.255.1.13/32, ubest/mbest: 2/0
    *via 10.2.1.2, [20/0], 2w0d, bgp-65511, external, tag 65501
    *via 10.2.2.2, [20/0], 2w0d, bgp-65511, external, tag 65501
10.255.1.14/32, ubest/mbest: 2/0
    *via 10.2.1.2, [20/0], 00:13:43, bgp-65511, external, tag 65501
    *via 10.2.2.2, [20/0], 00:13:43, bgp-65511, external, tag 65501
10.255.1.101/32, ubest/mbest: 1/0
    *via 10.2.1.2, [20/0], 2w0d, bgp-65511, external, tag 65501
10.255.1.102/32, ubest/mbest: 1/0
    *via 10.2.2.2, [20/0], 2w0d, bgp-65511, external, tag 65501
10.255.1.103/32, ubest/mbest: 2/0
    *via 10.2.1.2, [20/0], 00:16:44, bgp-65511, external, tag 65501
    *via 10.2.2.2, [20/0], 00:16:44, bgp-65511, external, tag 65501
192.168.10.40/32, ubest/mbest: 2/0
    *via 10.2.1.2, [20/0], 2w0d, bgp-65511, external, tag 65501
    *via 10.2.2.2, [20/0], 2w0d, bgp-65511, external, tag 65501



``` 
```
Leaf1#  ping 10.255.1.14
PING 10.255.1.14 (10.255.1.14): 56 data bytes
64 bytes from 10.255.1.14: icmp_seq=0 ttl=251 time=35.672 ms
64 bytes from 10.255.1.14: icmp_seq=1 ttl=251 time=17.902 ms
64 bytes from 10.255.1.14: icmp_seq=2 ttl=251 time=31.25 ms
64 bytes from 10.255.1.14: icmp_seq=3 ttl=251 time=21.297 ms
64 bytes from 10.255.1.14: icmp_seq=4 ttl=251 time=24.48 ms

--- 10.255.1.14 ping statistics ---
5 packets transmitted, 5 packets received, 0.00% packet loss
round-trip min/avg/max = 17.902/26.12/35.672 ms

```

# Multicast PIM (Sparse) 
## _Домашнее задание №5_

## План работ

- Настроите PIM на всех устройствах (кроме коммутаторов доступа);
  *Для IP связанности между устройствами можно использовать любой протокол динамической маршрутизации;
- Проверка состояния PIM связности между всеми устройствами сети (кроме коммутаторов доступа)
- проверка настройки bsr/rp для устройств сети
- Проверка  наличия IGMP Join запросов от клиентов на подключение к Multicast группе
- Проверка наличия мультикаст маршрутов соответствующей группы: {*.G} и {S.G} нотации.

## Схема сети

[Схема сети](https://github.com/Condorrr85/OTUS/blob/main/OTUS%20Multicast%20PIM.PNG)

## таблица ip-адресов

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
|  | E1/7 | 10.10.120.254 | 255.255.255.0 |
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
|  | E1/3 | 10.10.100.254 | 255.255.255.0 |
| | Lo0 | 10.255.1.13 | 255.255.255.255 |
| LEAF4 | E1/1 | IP unnumbered |  |
| | E1/2 | 10.10.110.254 | 255.255.255.0 |
| | Lo0 | 10.255.1.14 | 255.255.255.255 |
| Client1 | E0/0 | Port-channel 1 |  |
| | E0/1 | Port-channel 1 | |
| | Po1 | Trunk to VPC(Leaf1\Leaf2) |  |
| Client2 | E0/0 | Trunk to Leaf3 | - |
|  | E0/1 | Access to Multicast source | vlan 100 |
| Client3 | E0/0 | Trunk to Leaf4 | - |

### Настройка Multicast PIM (Sparse) на всех устройствах сети(кроме коммутаторов доступа)
[Настройки](https://github.com/Condorrr85/OTUS/tree/main/config/Multicast%20PIM(Sparse))


### Проверка состояния PIM связности между всеми устройствами сети

```
Spine1# show ip pim neighbor
PIM Neighbor Status for VRF "default"
Neighbor        Interface            Uptime    Expires   DR       Bidir-  BFD
 ECMP Redirect
                                                         Priority Capable State
    Capable
192.168.10.40   Ethernet1/1          06:33:13  00:01:39  1        no     n/a
 no
10.255.1.11     Ethernet1/2          00:55:50  00:01:27  1        yes     n/a
  no
10.255.1.12     Ethernet1/3          00:56:12  00:01:31  1        yes     n/a
  no
10.255.1.13     Ethernet1/4          06:33:12  00:01:28  1        yes     n/a 
  no
```
```
Spine2# show ip pim neighbor
PIM Neighbor Status for VRF "default"
Neighbor        Interface            Uptime    Expires   DR       Bidir-  BFD
 ECMP Redirect
                                                         Priority Capable State
    Capable
192.168.10.40   Ethernet1/1          06:34:29  00:01:27  1        no     n/a
 no
10.255.1.11     Ethernet1/2          00:57:03  00:01:40  1        yes     n/a
  no
10.255.1.12     Ethernet1/3          00:57:24  00:01:42  1        yes     n/a
  no
10.255.1.13     Ethernet1/4          06:34:26  00:01:24  1        yes     n/a
  no
```
```
Spine3# show ip pim neighbor
PIM Neighbor Status for VRF "default"
Neighbor        Interface            Uptime    Expires   DR       Bidir-  BFD
 ECMP Redirect
                                                         Priority Capable State
    Capable
192.168.10.40   Ethernet1/1          06:35:11  00:01:17  1        no     n/a
 no
10.255.1.14     Ethernet1/2          06:34:50  00:01:24  1        yes     n/a
  no
```
### проверка настройки bsr/rp для устройств сети
```
Spine1# show run pim
!Command: show running-config pim
!Running configuration last done at: Mon Dec 13 18:43:33 2021
!Time: Mon Dec 13 20:17:11 2021

version 9.2(2) Bios:version
feature pim

ip pim bsr bsr-candidate loopback0 priority 90
ip pim bsr rp-candidate loopback0 group-list 224.0.0.0/4 priority 90
ip pim log-neighbor-changes
ip pim ssm range 232.0.0.0/8
ip pim bsr forward listen


interface loopback0
  ip pim sparse-mode

interface Ethernet1/1
  ip pim sparse-mode

interface Ethernet1/2
  ip pim sparse-mode

interface Ethernet1/3
  ip pim sparse-mode

interface Ethernet1/4
  ip pim sparse-mode
```
```
NXOS_Interconnect#show run
Building configuration...

Current configuration : 1389 bytes
!
! Last configuration change at 20:46:09 EET Mon Dec 13 2021
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname NXOS_Interconnect
!

ip pim bsr-candidate Loopback0 32 100
ip pim rp-candidate Loopback0 priority 100
```
```
Spine2# show run pim
!Command: show running-config pim
!Running configuration last done at: Mon Dec 13 18:45:29 2021
!Time: Mon Dec 13 20:19:31 2021

version 9.2(2) Bios:version
feature pim

ip pim bsr bsr-candidate loopback0 priority 90
ip pim bsr rp-candidate loopback0 group-list 224.0.0.0/4 priority 90
ip pim log-neighbor-changes
ip pim ssm range 232.0.0.0/8
ip pim bsr forward listen

interface loopback0
  ip pim sparse-mode

interface Ethernet1/1
  ip pim sparse-mode

interface Ethernet1/2
  ip pim sparse-mode

interface Ethernet1/3
  ip pim sparse-mode

interface Ethernet1/4
  ip pim sparse-mode

```
### Проверка  наличия IGMP Join запросов от клиентов на подключение к Multicast группе
```
Leaf1# show ip igmp groups
IGMP Connected Group Membership for VRF "default" - 1 total entries
Type: S - Static, D - Dynamic, L - Local, T - SSM Translated, H - Host Proxy
      * - Cache Only
Group Address      Type Interface              Uptime    Expires   Last Reporter
239.0.0.100        L   Ethernet1/7            00:53:12  never     10.10.120.254

```
```
Leaf4# show ip igmp groups
IGMP Connected Group Membership for VRF "default" - 1 total entries
Type: S - Static, D - Dynamic, L - Local, T - SSM Translated, H - Host Proxy
      * - Cache Only
Group Address      Type Interface              Uptime    Expires   Last Reporter
239.0.0.100        L   Ethernet1/2            06:41:13  never     10.10.110.254

```
### Проверка наличия мультикаст маршрутов соответствующей группы: {*.G} и {S.G} нотации.
```
Leaf4# show ip mroute
IP Multicast Routing Table for VRF "default"
(*, 232.0.0.0/8), uptime: 06:47:03, pim ip
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)

(*, 239.0.0.100/32), uptime: 06:45:25, igmp ip pim
  Incoming interface: Ethernet1/1, RPF nbr: 10.255.1.103
  Outgoing interface list: (count: 1)
    Ethernet1/2, uptime: 06:45:25, igmp

(10.10.100.1/32, 239.0.0.100/32), uptime: 01:28:29, ip mrib pim
  Incoming interface: Ethernet1/1, RPF nbr: 10.255.1.103
  Outgoing interface list: (count: 1)
    Ethernet1/2, uptime: 01:28:29, mrib
```
```
Leaf1# show ip mroute
IP Multicast Routing Table for VRF "default"
(*, 232.0.0.0/8), uptime: 01:10:39, pim ip
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)

(*, 239.0.0.100/32), uptime: 00:59:17, igmp ip pim
  Incoming interface: Ethernet1/1, RPF nbr: 10.255.1.101
  Outgoing interface list: (count: 1)
    Ethernet1/7, uptime: 00:59:17, igmp

(10.10.100.1/32, 239.0.0.100/32), uptime: 00:59:17, ip mrib pim
  Incoming interface: Ethernet1/2, RPF nbr: 10.255.1.102
  Outgoing interface list: (count: 1)
    Ethernet1/7, uptime: 00:59:17, mrib
```
```
Spine1# show ip mroute
IP Multicast Routing Table for VRF "default"
(*, 232.0.0.0/8), uptime: 06:49:12, pim ip
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)

(*, 239.0.0.100/32), uptime: 01:32:28, pim ip
  Incoming interface: loopback0, RPF nbr: 10.255.1.101
  Outgoing interface list: (count: 2)
    Ethernet1/2, uptime: 01:00:18, pim
    Ethernet1/1, uptime: 01:32:28, pim

(10.10.100.1/32, 239.0.0.100/32), uptime: 01:30:30, pim mrib ip
  Incoming interface: Ethernet1/4, RPF nbr: 10.255.1.13, internal
  Outgoing interface list: (count: 0)
```
# VXLAN/Multicast
## _Домашнее задание №5.1_

## План работ

- Настроите PIM на всех устройствах (кроме коммутаторов доступа);
  *Для IP связанности между устройствами можно использовать любой протокол динамической маршрутизации;
- Настроить Anycast RP и Pim bi-direct для корректной работы VXLAN с Multicast
- Настройка VXLAN: Настройка Vlan, vlan-to-VXLAN mapping, настройка nve интерфейса на VTEP, настройка multicast group для VNI
- Проверка корректной работы VXLAN через multicast (наличие нужной multucast group в {*,G} и {S,G} нотациях, проверка активности nve peers, пинг между клиентами в широковещательном домене, подключенные к разных leaf коммутаторам и разным Spine)

## Vlan\VNI mapping
| Device | Interface | Vlan | IP-адрес  | VNI  |
| ----------- | ----------- | ----------- | ----------- |----------- |
| Client1 | Port-channel 1 | Vlan 100 | 10.10.100.10/24  | 8000 | 
| Client2 | E0/0 | Vlan 100 | 10.10.100.15/24 | 8000 |

### Настройка VXLAN с использованием Multicast
[Настройки](https://github.com/Condorrr85/OTUS/tree/main/config/VXLAN%20via%20Multicast)

### Проверка настройки PIM/Anycast RP для Leaf/Spine коммутаторов
```
Leaf1# show run pim

!Command: show running-config pim
!Running configuration last done at: Sun Dec 26 19:08:25 2021
!Time: Sun Dec 26 21:27:10 2021

version 9.2(2) Bios:version
feature pim

ip pim rp-address 10.1.1.1 group-list 239.1.1.0/24 bidir
ip pim log-neighbor-changes
ip pim ssm range 232.0.0.0/8


interface loopback0
  ip pim sparse-mode

interface loopback1
  ip pim sparse-mode

interface Ethernet1/1
  ip pim sparse-mode

interface Ethernet1/2
  ip pim sparse-mode

```
```
Leaf2# show run pim

!Command: show running-config pim
!Running configuration last done at: Sun Dec 26 19:07:56 2021
!Time: Sun Dec 26 21:27:37 2021

version 9.2(2) Bios:version
feature pim

ip pim rp-address 10.1.1.1 group-list 239.1.1.0/24 bidir
ip pim log-neighbor-changes
ip pim ssm range 232.0.0.0/8


interface loopback0
  ip pim sparse-mode

interface loopback1
  ip pim sparse-mode

interface Ethernet1/1
  ip pim sparse-mode

interface Ethernet1/2
  ip pim sparse-mode

```
```
Spine1# show run pim

!Command: show running-config pim
!Running configuration last done at: Sun Dec 26 19:13:47 2021
!Time: Sun Dec 26 21:28:37 2021

version 9.2(2) Bios:version
feature pim

ip pim rp-address 10.1.1.1 group-list 239.1.1.0/24 bidir
ip pim log-neighbor-changes
ip pim ssm range 232.0.0.0/8
ip pim anycast-rp 10.1.1.1 10.255.1.101
ip pim anycast-rp 10.1.1.1 10.255.1.102
ip pim anycast-rp 10.1.1.1 10.255.1.103


interface loopback0
  ip pim sparse-mode

interface loopback1
  ip pim sparse-mode

interface Ethernet1/1
  ip pim sparse-mode

interface Ethernet1/2
  ip pim sparse-mode

interface Ethernet1/3
  ip pim sparse-mode

interface Ethernet1/4
  ip pim sparse-mode

```
### Проверка настройки VXLAN: Настройка Vlan, vlan-to-VXLAN mapping, настройка nve интерфейса на VTEP, настройка multicast group для VNI
```
Leaf1# show run int nve 1

interface nve1
  no shutdown
  source-interface loopback0
  member vni 8000 mcast-group 239.1.1.1

Leaf1# show run vlan 100

vlan 100
vlan 100
  vn-segment 8000
  
Leaf1# show run int lo0

interface loopback0
  ip address 10.255.1.11/32
  ip address 10.0.1.11/32 secondary
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode

```
```
Leaf2# show run interface nve 1

interface nve1
  no shutdown
  source-interface loopback0
  member vni 8000 mcast-group 239.1.1.1

Leaf2# show run vlan 100

vlan 100
vlan 100
  vn-segment 8000


Leaf2# show run interface loopback 0

interface loopback0
  ip address 10.255.1.12/32
  ip address 10.0.1.11/32 secondary
  ip router ospf 1 area 0.0.0.0
  ip pim sparse-mode

```
```
Leaf3# show run int nve 1

interface nve1
  no shutdown
  source-interface loopback0
  member vni 8000 mcast-group 239.1.1.1

Leaf3# show run vlan 100

vlan 100
vlan 100
  vn-segment 8000

```
### Проверка корректной работы VXLAN через multicast (наличие нужной multucast group в {*,G} и {S,G} нотациях, проверка активности nve peers, пинг между клиентами в широковещательном домене, подключенные к разных leaf коммутаторам и разным Spine)
```
Leaf1# show ip mroute
IP Multicast Routing Table for VRF "default"

(*, 232.0.0.0/8), uptime: 02:25:29, pim ip
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)


(*, 239.1.1.0/24), bidir, uptime: 02:25:29, pim ip
  Incoming interface: Ethernet1/2, RPF nbr: 10.255.1.102
  Outgoing interface list: (count: 1)
    Ethernet1/2, uptime: 02:25:29, pim, (RPF)


(*, 239.1.1.1/32), bidir, uptime: 02:25:29, nve ip pim
  Incoming interface: Ethernet1/2, RPF nbr: 10.255.1.102
  Outgoing interface list: (count: 2)
    Ethernet1/2, uptime: 02:25:29, pim, (RPF)
    nve1, uptime: 02:25:29, nve

Leaf1# show nve peers
Interface Peer-IP          State LearnType Uptime   Router-Mac
--------- ---------------  ----- --------- -------- -----------------
nve1      10.255.1.13      Up    DP        02:20:33 n/a

Leaf1# show nve vni 8000 detail
VNI: 8000
  NVE-Interface       : nve1
  Mcast-Addr          : 239.1.1.1
  VNI State           : Up
  Mode                : data-plane
  VNI Type            : L2 [100]
  VNI Flags           :
  Provision State     : vni-add-complete
  Vlan-BD             : 100
  SVI State           : n/a

```
```
Leaf3# show ip mroute
IP Multicast Routing Table for VRF "default"

(*, 232.0.0.0/8), uptime: 02:26:41, pim ip
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)


(*, 239.1.1.0/24), bidir, uptime: 02:26:41, pim ip
  Incoming interface: Ethernet1/2, RPF nbr: 10.255.1.102
  Outgoing interface list: (count: 1)
    Ethernet1/2, uptime: 02:26:41, pim, (RPF)


(*, 239.1.1.1/32), bidir, uptime: 02:26:41, nve ip pim
  Incoming interface: Ethernet1/2, RPF nbr: 10.255.1.102
  Outgoing interface list: (count: 2)
    Ethernet1/2, uptime: 02:26:41, pim, (RPF)
    nve1, uptime: 02:26:41, nve

Leaf3# show nve peers
Interface Peer-IP          State LearnType Uptime   Router-Mac
--------- ---------------  ----- --------- -------- -----------------
nve1      10.0.1.11        Up    DP        02:21:37 n/a

Leaf3# show nve vni 8000 detail
VNI: 8000
  NVE-Interface       : nve1
  Mcast-Addr          : 239.1.1.1
  VNI State           : Up
  Mode                : data-plane
  VNI Type            : L2 [100]
  VNI Flags           :
  Provision State     : vni-add-complete
  Vlan-BD             : 100
  SVI State           : n/a

```
```
Spine2# show ip mroute
IP Multicast Routing Table for VRF "default"

(*, 232.0.0.0/8), uptime: 02:28:26, pim ip
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)


(*, 239.1.1.0/24), bidir, uptime: 02:24:38, pim ip
  Incoming interface: loopback1, RPF nbr: 10.1.1.1
  Outgoing interface list: (count: 0)


(*, 239.1.1.1/32), bidir, uptime: 02:24:38, pim ip
  Incoming interface: loopback1, RPF nbr: 10.1.1.1
  Outgoing interface list: (count: 3)
    Ethernet1/4, uptime: 02:24:38, pim
    Ethernet1/3, uptime: 02:24:38, pim
    Ethernet1/2, uptime: 02:24:38, pim

```
```
Client1#show run int vlan 100

interface Vlan100
 mac-address 0000.0000.0101
 ip address 10.10.100.10 255.255.255.0

Client1#ping 10.10.100.15
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.10.100.15, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 21/26/30 ms

Client2#show run int vlan 100

interface Vlan100
 mac-address 0000.0000.2222
 ip address 10.10.100.15 255.255.255.0
end

Client2#ping 10.10.100.10
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.10.100.10, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 23/32/44 ms

```

# VxLAN. Route type 2
## Домашнее задание №6

## План работ

- Настроить BGP peering между Leaf и Spine в AF l2vpn evpn;
- Spine работает в качестве route-reflector;
- Настроена связанность между клиентами в первой зоне;
- План работы, адресное пространство, схема сети, настройки - зафиксированы в документации.


![Scheme](https://github.com/Condorrr85/OTUS/blob/main/VXLAN%20route%20type%202.PNG)

**Настройка NEXUS:**

<details>
<summary>Spine1</summary>
<pre><code>
conf t
!
hostname Spine1
!
nv overlay evpn
feature ospf
feature bgp
feature nv overlay
!
interface Ethernet1/1
  no switchport
  medium p2p
  ip unnumbered loopback0
  ip ospf network point-to-point
  ip router ospf 1 area 0.0.0.0
  no shutdown
!
interface Ethernet1/2
  no switchport
  medium p2p
  ip unnumbered loopback0
  ip ospf network point-to-point
  ip router ospf 1 area 0.0.0.0
  no shutdown
!
interface Ethernet1/3
  no switchport
  medium p2p
  ip unnumbered loopback0
  ip ospf network point-to-point
  ip router ospf 1 area 0.0.0.0
  no shutdown
!
interface Ethernet1/4
  no switchport
  medium p2p
  ip unnumbered loopback0
  ip ospf network point-to-point
  ip router ospf 1 area 0.0.0.0
  no shutdown
!
interface loopback0
  ip address 10.255.1.101/32
  ip router ospf 1 area 0.0.0.0
!
interface loopback1
  ip address 10.1.1.1/32
  ip router ospf 1 area 0.0.0.0
!
line console
  exec-timeout 0
line vty
  exec-timeout 0
!
router ospf 1
  router-id 10.255.1.101
 !
router bgp 65000
  template peer LEAF
    remote-as 65000
    update-source loopback0
    address-family l2vpn evpn
      send-community
      send-community extended
      route-reflector-client
  neighbor 10.255.1.11
    inherit peer LEAF
  neighbor 10.255.1.12
    inherit peer LEAF
  neighbor 10.255.1.13
    inherit peer LEAF
 !
end
copy run star
</code></pre>
</details>
<details>
<summary>Spine2</summary>
<pre><code>
conf t
!
hostname Spine2
!
nv overlay evpn
feature ospf
feature bgp
feature nv overlay

!
interface Ethernet1/1
  no switchport
  medium p2p
  ip unnumbered loopback0
  ip ospf network point-to-point
  ip router ospf 1 area 0.0.0.0
  no shutdown
!
interface Ethernet1/2
  no switchport
  medium p2p
  ip unnumbered loopback0
  ip ospf network point-to-point
  ip router ospf 1 area 0.0.0.0
  no shutdown
!
interface Ethernet1/3
  no switchport
  medium p2p
  ip unnumbered loopback0
  ip ospf network point-to-point
  ip router ospf 1 area 0.0.0.0
  no shutdown
!
interface Ethernet1/4
  no switchport
  medium p2p
  ip unnumbered loopback0
  ip ospf network point-to-point
  ip router ospf 1 area 0.0.0.0
  no shutdown
!
interface loopback0
  ip address 10.255.1.102/32
  ip router ospf 1 area 0.0.0.0
!
interface loopback1
  ip address 10.1.1.1/32
  ip router ospf 1 area 0.0.0.0
!
line console
  exec-timeout 0
line vty
  exec-timeout 0
!
router ospf 1
  router-id 10.255.1.102
!
router bgp 65000
  template peer LEAF
    remote-as 65000
    update-source loopback0
    address-family l2vpn evpn
      send-community
      send-community extended
      route-reflector-client
  neighbor 10.255.1.11
    inherit peer LEAF
  neighbor 10.255.1.12
    inherit peer LEAF
  neighbor 10.255.1.13
    inherit peer LEAF
!
end
copy run star
</code></pre>
</details>
<details>
<summary>Leaf1</summary>
<pre><code>
conf t
!
hostname Leaf1
!
nv overlay evpn
feature ospf
feature bgp
feature interface-vlan
feature vn-segment-vlan-based
feature lacp
feature vpc
feature nv overlay
!
fabric forwarding anycast-gateway-mac 0000.0000.1111
vlan 1,100,200
vlan 100
  vn-segment 8000
vlan 200
  vn-segment 9000
!
vpc domain 1
  peer-switch
  peer-keepalive destination 172.16.1.2 source 172.16.1.1 vrf VPC
  peer-gateway
  ip arp synchronize
!
interface Vlan100
  no shutdown
  no ip redirects
  ip address 10.10.100.254/24
  no ipv6 redirects
  fabric forwarding mode anycast-gateway
!
interface Vlan200
  no shutdown
  no ip redirects
  ip address 10.10.200.254/24
  no ipv6 redirects
  fabric forwarding mode anycast-gateway
!
interface port-channel1
  switchport mode trunk
  spanning-tree port type network
  no lacp suspend-individual
  vpc peer-link
!
interface port-channel2
  switchport mode trunk
  vpc 1
!
interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  member vni 8000
    ingress-replication protocol bgp
  member vni 9000
    ingress-replication protocol bgp
!
interface Ethernet1/1
  no switchport
  medium p2p
  ip unnumbered loopback0
  ip ospf network point-to-point
  ip router ospf 1 area 0.0.0.0
  no shutdown
!
interface Ethernet1/2
  no switchport
  medium p2p
  ip unnumbered loopback0
  ip ospf network point-to-point
  ip router ospf 1 area 0.0.0.0
  no shutdown
!
interface Ethernet1/3
  no switchport
  vrf member VPC
  ip address 172.16.1.1/30
  no shutdown
!
interface Ethernet1/4
  switchport mode trunk
  channel-group 1 mode active
!
interface Ethernet1/5
  switchport mode trunk
  channel-group 2 mode active
!
interface Ethernet1/6
  switchport mode trunk
  channel-group 1 mode active
!
interface loopback0
  ip address 10.255.1.11/32
  ip router ospf 1 area 0.0.0.0
!
interface loopback1
  ip address 10.100.1.11/32
  ip address 10.0.1.11/32 secondary
  ip router ospf 1 area 0.0.0.0
!
line console
  exec-timeout 0
line vty
  exec-timeout 0
!
router ospf 1
  router-id 10.255.1.11
!
router bgp 65000
  template peer SPINE
    remote-as 65000
    update-source loopback0
    address-family l2vpn evpn
      send-community
      send-community extended
  neighbor 10.255.1.101
    inherit peer SPINE
  neighbor 10.255.1.102
    inherit peer SPINE
!
end
copy run star
</code></pre>
</details>
<details>
<summary>Leaf2</summary>
<pre><code>
configure terminal
!
hostname Leaf2
!
nv overlay evpn
feature ospf
feature bgp
feature isis
feature interface-vlan
feature vn-segment-vlan-based
feature lacp
feature vpc
feature nv overlay
!
fabric forwarding anycast-gateway-mac 0000.0000.1111
vlan 1,100,200
vlan 100
  vn-segment 8000
vlan 200
  vn-segment 9000
!
vpc domain 1
  peer-switch
  peer-keepalive destination 172.16.1.1 source 172.16.1.2 vrf VPC
  peer-gateway
  ip arp synchronize
!
interface Vlan100
  no shutdown
  no ip redirects
  ip address 10.10.100.254/24
  no ipv6 redirects
  fabric forwarding mode anycast-gateway
!
interface Vlan200
  no shutdown
  no ip redirects
  ip address 10.10.200.254/24
  no ipv6 redirects
  fabric forwarding mode anycast-gateway
!
interface port-channel1
  switchport mode trunk
  spanning-tree port type network
  no lacp suspend-individual
  vpc peer-link
!
interface port-channel2
  switchport mode trunk
  no lacp suspend-individual
  vpc 1
!
interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  member vni 8000
    ingress-replication protocol bgp
  member vni 9000
    ingress-replication protocol bgp
!
interface Ethernet1/1
  no switchport
  medium p2p
  ip unnumbered loopback0
  ip ospf network point-to-point
  ip router ospf 1 area 0.0.0.0
  no shutdown
!
interface Ethernet1/2
  no switchport
  medium p2p
  ip unnumbered loopback0
  ip ospf network point-to-point
  ip router ospf 1 area 0.0.0.0
  no shutdown
!
interface Ethernet1/3
  no switchport
  vrf member VPC
  ip address 172.16.1.2/30
  no shutdown
!
interface Ethernet1/4
  switchport mode trunk
  channel-group 1 mode active
!
interface Ethernet1/5
  switchport mode trunk
  channel-group 2 mode active
!
interface Ethernet1/6
  switchport mode trunk
  channel-group 1 mode active
!
interface loopback0
  ip address 10.255.1.12/32
  ip router ospf 1 area 0.0.0.0
!
interface loopback1
  ip address 10.100.1.12/32
  ip address 10.0.1.11/32 secondary
  ip router ospf 1 area 0.0.0.0
!
line console
  exec-timeout 0
line vty
  exec-timeout 0
!
router ospf 1
  router-id 10.255.1.12
!
router bgp 65000
  template peer SPINE
    remote-as 65000
    update-source loopback0
    address-family l2vpn evpn
      send-community
      send-community extended
  neighbor 10.255.1.101
    inherit peer SPINE
  neighbor 10.255.1.102
    inherit peer SPINE
!
end
copy run star
</code></pre>
</details>
<details>
<summary>Leaf3</summary>
<pre><code>
configure terminal
!
hostname Leaf3
!
nv overlay evpn
feature ospf
feature bgp
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay
!
fabric forwarding anycast-gateway-mac 0000.0000.1111
vlan 1,100,200
vlan 100
  vn-segment 8000
vlan 200
  vn-segment 9000
!
interface Vlan100
  no shutdown
  ip address 10.10.100.254/24
  fabric forwarding mode anycast-gateway
!
interface Vlan200
  no shutdown
  ip address 10.10.200.254/24
  fabric forwarding mode anycast-gateway
!
interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback1
  member vni 8000
    ingress-replication protocol bgp
  member vni 9000
    ingress-replication protocol bgp
!
interface Ethernet1/1
  no switchport
  medium p2p
  ip unnumbered loopback0
  ip ospf network point-to-point
  ip router ospf 1 area 0.0.0.0
  no shutdown
!
interface Ethernet1/2
  no switchport
  medium p2p
  ip unnumbered loopback0
  ip ospf network point-to-point
  ip router ospf 1 area 0.0.0.0
  no shutdown
!
interface Ethernet1/3
  switchport mode trunk
!
interface loopback0
  ip address 10.255.1.13/32
  ip router ospf 1 area 0.0.0.0
!
interface loopback1
  ip address 10.100.1.13/32
  ip router ospf 1 area 0.0.0.0
!
line console
  exec-timeout 0
line vty
  exec-timeout 0
!
router ospf 1
  router-id 10.255.1.13
!
router bgp 65000
  template peer SPINE
    remote-as 65000
    update-source loopback0
    address-family l2vpn evpn
      send-community
      send-community extended
  neighbor 10.255.1.101
    inherit peer SPINE
  neighbor 10.255.1.102
    inherit peer SPINE
!
end
copy run star
</code></pre>
</details>

проверим, что bgp peering между Leaf и Spine устройствами в AF L2vpn evpn работает корректно:
<details>
<summary>Spine1</summary>
<pre><code>
Spine1# show bgp l2vpn evpn summary
BGP summary information for VRF default, address family L2VPN EVPN
BGP router identifier 10.255.1.101, local AS number 65000
BGP table version is 181, L2VPN EVPN config peers 3, capable peers 3
14 network entries and 14 paths using 3080 bytes of memory
BGP attribute entries [7/1148], BGP AS path entries [0/0]
BGP community entries [0/0], BGP clusterlist entries [0/0]
!
Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.255.1.11     4 65000     181     173      181    0    0 00:34:55 4
10.255.1.12     4 65000     170     177      181    0    0 00:34:43 4
10.255.1.13     4 65000     189     168      181    0    0 02:04:11 6
</code></pre>
</details>
<details>
<summary>Spine2</summary>
<pre><code>
Spine2# show bgp l2vpn evpn summary
BGP summary information for VRF default, address family L2VPN EVPN
BGP router identifier 10.255.1.102, local AS number 65000
BGP table version is 203, L2VPN EVPN config peers 3, capable peers 3
14 network entries and 14 paths using 3080 bytes of memory
BGP attribute entries [7/1148], BGP AS path entries [0/0]
BGP community entries [0/0], BGP clusterlist entries [0/0]
!
Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.255.1.11     4 65000     202     194      203    0    0 00:48:56 4
10.255.1.12     4 65000     193     198      203    0    0 00:48:43 4
10.255.1.13     4 65000     217     189      203    0    0 02:20:06 6
</code></pre>
</details>
<details>
<summary>Leaf1</summary>
<pre><code>
Leaf1# show bgp l2vpn evpn summary
BGP summary information for VRF default, address family L2VPN EVPN
BGP router identifier 10.255.1.11, local AS number 65000
BGP table version is 308, L2VPN EVPN config peers 2, capable peers 2
16 network entries and 22 paths using 3520 bytes of memory
BGP attribute entries [11/1804], BGP AS path entries [0/0]
BGP community entries [0/0], BGP clusterlist entries [2/8]
!
Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.255.1.101    4 65000     275     170      308    0    0 00:49:53 6
10.255.1.102    4 65000     277     171      308    0    0 00:49:53 6
</code></pre>
</details>
<details>
<summary>Leaf2</summary>
<pre><code>
Leaf2# show bgp l2vpn evpn summary
BGP summary information for VRF default, address family L2VPN EVPN
BGP router identifier 10.255.1.12, local AS number 65000
BGP table version is 308, L2VPN EVPN config peers 2, capable peers 2
16 network entries and 22 paths using 3520 bytes of memory
BGP attribute entries [11/1804], BGP AS path entries [0/0]
BGP community entries [0/0], BGP clusterlist entries [2/8]

!
Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.255.1.101    4 65000     285     166      308    0    0 00:50:28 6
10.255.1.102    4 65000     287     169      308    0    0 00:50:27 6
</code></pre>
</details>
<details>
<summary>Leaf3</summary>
<pre><code>
Leaf3# show bgp l2vpn evpn summary
BGP summary information for VRF default, address family L2VPN EVPN
BGP router identifier 10.255.1.13, local AS number 65000
BGP table version is 471, L2VPN EVPN config peers 2, capable peers 2
18 network entries and 30 paths using 4456 bytes of memory
BGP attribute entries [16/2624], BGP AS path entries [0/0]
BGP community entries [0/0], BGP clusterlist entries [4/16]
!
Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.255.1.101    4 65000     258     179      471    0    0 02:20:44 8
10.255.1.102    4 65000     260     183      471    0    0 02:22:39 8
</code></pre>
</details>
Проверка nve peers и таблицы маршрутизации для BGP EVPN:
<details>
<summary>Leaf1</summary>
<pre><code>
Leaf1# show nve peers
Interface Peer-IP          State LearnType Uptime   Router-Mac
--------- ---------------  ----- --------- -------- -----------------
nve1      10.100.1.13      Up    CP        02:32:19 n/a
!
Leaf1# show bgp l2vpn evpn
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 342, Local Router ID is 10.255.1.11
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-i
njected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - b
est2

   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.255.1.11:32867    (L2VNI 8000)
*>l[2]:[0]:[0]:[48]:[0050.7966.680c]:[0]:[0.0.0.0]/216
                      10.0.1.11                         100      32768 i
*>i[2]:[0]:[0]:[48]:[0050.7966.680e]:[0]:[0.0.0.0]/216
                      10.100.1.13                       100          0 i
*>l[2]:[0]:[0]:[48]:[0050.7966.680c]:[32]:[10.10.100.10]/248
                      10.0.1.11                         100      32768 i
*>i[2]:[0]:[0]:[48]:[0050.7966.680e]:[32]:[10.10.100.20]/248
                      10.100.1.13                       100          0 i
*>l[3]:[0]:[32]:[10.0.1.11]/88
                      10.0.1.11                         100      32768 i
*>i[3]:[0]:[32]:[10.100.1.13]/88
                      10.100.1.13                       100          0 i

Route Distinguisher: 10.255.1.11:32967    (L2VNI 9000)
*>i[2]:[0]:[0]:[48]:[0050.7966.680f]:[0]:[0.0.0.0]/216
                      10.100.1.13                       100          0 i
*>i[2]:[0]:[0]:[48]:[0050.7966.680f]:[32]:[10.10.200.20]/248
                      10.100.1.13                       100          0 i
*>l[3]:[0]:[32]:[10.0.1.11]/88
                      10.0.1.11                         100      32768 i
*>i[3]:[0]:[32]:[10.100.1.13]/88
                      10.100.1.13                       100          0 i

Route Distinguisher: 10.255.1.13:32867
*>i[2]:[0]:[0]:[48]:[0050.7966.680e]:[0]:[0.0.0.0]/216
                      10.100.1.13                       100          0 i
* i                   10.100.1.13                       100          0 i
* i[2]:[0]:[0]:[48]:[0050.7966.680e]:[32]:[10.10.100.20]/248
                      10.100.1.13                       100          0 i
*>i                   10.100.1.13                       100          0 i
*>i[3]:[0]:[32]:[10.100.1.13]/88
                      10.100.1.13                       100          0 i
* i                   10.100.1.13                       100          0 i

Route Distinguisher: 10.255.1.13:32967
* i[2]:[0]:[0]:[48]:[0050.7966.680f]:[0]:[0.0.0.0]/216
                      10.100.1.13                       100          0 i
*>i                   10.100.1.13                       100          0 i
* i[2]:[0]:[0]:[48]:[0050.7966.680f]:[32]:[10.10.200.20]/248
                      10.100.1.13                       100          0 i
*>i                   10.100.1.13                       100          0 i
* i[3]:[0]:[32]:[10.100.1.13]/88
                      10.100.1.13                       100          0 i
*>i                   10.100.1.13                       100          0 i

</code></pre>
</details>
<details>
<summary>Leaf2</summary>
<pre><code>
Leaf2# show nve peers
Interface Peer-IP          State LearnType Uptime   Router-Mac
--------- ---------------  ----- --------- -------- -----------------
nve1      10.100.1.13      Up    CP        02:34:55 n/a
!
Leaf2# show bgp l2vpn evpn
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 353, Local Router ID is 10.255.1.12
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-i
njected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - b
est2

   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.100.1.12:32867    (L2VNI 8000)
*>l[2]:[0]:[0]:[48]:[0050.7966.680c]:[0]:[0.0.0.0]/216
                      10.0.1.11                         100      32768 i
*>i[2]:[0]:[0]:[48]:[0050.7966.680e]:[0]:[0.0.0.0]/216
                      10.100.1.13                       100          0 i
*>l[2]:[0]:[0]:[48]:[0050.7966.680c]:[32]:[10.10.100.10]/248
                      10.0.1.11                         100      32768 i
*>i[2]:[0]:[0]:[48]:[0050.7966.680e]:[32]:[10.10.100.20]/248
                      10.100.1.13                       100          0 i
*>l[3]:[0]:[32]:[10.0.1.11]/88
                      10.0.1.11                         100      32768 i
*>i[3]:[0]:[32]:[10.100.1.13]/88
                      10.100.1.13                       100          0 i

Route Distinguisher: 10.255.1.12:32967    (L2VNI 9000)
*>i[2]:[0]:[0]:[48]:[0050.7966.680f]:[0]:[0.0.0.0]/216
                      10.100.1.13                       100          0 i
*>i[2]:[0]:[0]:[48]:[0050.7966.680f]:[32]:[10.10.200.20]/248
                      10.100.1.13                       100          0 i
*>l[3]:[0]:[32]:[10.0.1.11]/88
                      10.0.1.11                         100      32768 i
*>i[3]:[0]:[32]:[10.100.1.13]/88
                      10.100.1.13                       100          0 i

Route Distinguisher: 10.255.1.13:32867
*>i[2]:[0]:[0]:[48]:[0050.7966.680e]:[0]:[0.0.0.0]/216
                      10.100.1.13                       100          0 i
* i                   10.100.1.13                       100          0 i
*>i[2]:[0]:[0]:[48]:[0050.7966.680e]:[32]:[10.10.100.20]/248
                      10.100.1.13                       100          0 i
* i                   10.100.1.13                       100          0 i
*>i[3]:[0]:[32]:[10.100.1.13]/88
                      10.100.1.13                       100          0 i
* i                   10.100.1.13                       100          0 i

Route Distinguisher: 10.255.1.13:32967
*>i[2]:[0]:[0]:[48]:[0050.7966.680f]:[0]:[0.0.0.0]/216
                      10.100.1.13                       100          0 i
* i                   10.100.1.13                       100          0 i
*>i[2]:[0]:[0]:[48]:[0050.7966.680f]:[32]:[10.10.200.20]/248
                      10.100.1.13                       100          0 i
* i                   10.100.1.13                       100          0 i
* i[3]:[0]:[32]:[10.100.1.13]/88
                      10.100.1.13                       100          0 i
*>i                   10.100.1.13                       100          0 i

</code></pre>
</details>
<details>
<summary>Leaf3</summary>
<pre><code>
Leaf3# show nve peers
Interface Peer-IP          State LearnType Uptime   Router-Mac
--------- ---------------  ----- --------- -------- -----------------
nve1      10.0.1.11        Up    CP        02:36:07 n/a
!
Leaf3# show bgp l2vpn evpn
BGP routing table information for VRF default, address family L2VPN EVPN
BGP table version is 493, Local Router ID is 10.255.1.13
Status: s-suppressed, x-deleted, S-stale, d-dampened, h-history, *-valid, >-best
Path type: i-internal, e-external, c-confed, l-local, a-aggregate, r-redist, I-i
njected
Origin codes: i - IGP, e - EGP, ? - incomplete, | - multipath, & - backup, 2 - b
est2

   Network            Next Hop            Metric     LocPrf     Weight Path
Route Distinguisher: 10.100.1.12:32867
* i[2]:[0]:[0]:[48]:[0050.7966.680c]:[0]:[0.0.0.0]/216
                      10.0.1.11                         100          0 i
*>i                   10.0.1.11                         100          0 i
* i[2]:[0]:[0]:[48]:[0050.7966.680c]:[32]:[10.10.100.10]/248
                      10.0.1.11                         100          0 i
*>i                   10.0.1.11                         100          0 i
*>i[3]:[0]:[32]:[10.0.1.11]/88
                      10.0.1.11                         100          0 i
* i                   10.0.1.11                         100          0 i

Route Distinguisher: 10.255.1.11:32867
* i[2]:[0]:[0]:[48]:[0050.7966.680c]:[0]:[0.0.0.0]/216
                      10.0.1.11                         100          0 i
*>i                   10.0.1.11                         100          0 i
* i[2]:[0]:[0]:[48]:[0050.7966.680c]:[32]:[10.10.100.10]/248
                      10.0.1.11                         100          0 i
*>i                   10.0.1.11                         100          0 i
* i[3]:[0]:[32]:[10.0.1.11]/88
                      10.0.1.11                         100          0 i
*>i                   10.0.1.11                         100          0 i

Route Distinguisher: 10.255.1.11:32967
* i[3]:[0]:[32]:[10.0.1.11]/88
                      10.0.1.11                         100          0 i
*>i                   10.0.1.11                         100          0 i

Route Distinguisher: 10.255.1.12:32967
* i[3]:[0]:[32]:[10.0.1.11]/88
                      10.0.1.11                         100          0 i
*>i                   10.0.1.11                         100          0 i

Route Distinguisher: 10.255.1.13:32867    (L2VNI 8000)
* i[2]:[0]:[0]:[48]:[0050.7966.680c]:[0]:[0.0.0.0]/216
                      10.0.1.11                         100          0 i
*>i                   10.0.1.11                         100          0 i
*>l[2]:[0]:[0]:[48]:[0050.7966.680e]:[0]:[0.0.0.0]/216
                      10.100.1.13                       100      32768 i
*>i[2]:[0]:[0]:[48]:[0050.7966.680c]:[32]:[10.10.100.10]/248
                      10.0.1.11                         100          0 i
* i                   10.0.1.11                         100          0 i
*>l[2]:[0]:[0]:[48]:[0050.7966.680e]:[32]:[10.10.100.20]/248
                      10.100.1.13                       100      32768 i
*>i[3]:[0]:[32]:[10.0.1.11]/88
                      10.0.1.11                         100          0 i
* i                   10.0.1.11                         100          0 i
*>l[3]:[0]:[32]:[10.100.1.13]/88
                      10.100.1.13                       100      32768 i

Route Distinguisher: 10.255.1.13:32967    (L2VNI 9000)
*>l[2]:[0]:[0]:[48]:[0050.7966.680f]:[0]:[0.0.0.0]/216
                      10.100.1.13                       100      32768 i
*>l[2]:[0]:[0]:[48]:[0050.7966.680f]:[32]:[10.10.200.20]/248
                      10.100.1.13                       100      32768 i
*>i[3]:[0]:[32]:[10.0.1.11]/88
                      10.0.1.11                         100          0 i
* i                   10.0.1.11                         100          0 i
*>l[3]:[0]:[32]:[10.100.1.13]/88
                      10.100.1.13                       100      32768 i

</code></pre>
</details>



Проверка связности между клиентами в первой зоне:
</code></pre>
</details>
<details>
<summary>VPC12</summary>
<pre><code>
VPCS> ip 10.10.100.10 255.255.255.0 10.10.100.254
Checking for duplicate address...
PC1 : 10.10.100.10 255.255.255.0 gateway 10.10.100.254

VPCS>
VPCS> ping 10.10.100.254

84 bytes from 10.10.100.254 icmp_seq=1 ttl=255 time=30.357 ms
84 bytes from 10.10.100.254 icmp_seq=2 ttl=255 time=6.918 ms
^C
VPCS> ping 10.10.100.20

84 bytes from 10.10.100.20 icmp_seq=1 ttl=64 time=44.957 ms
84 bytes from 10.10.100.20 icmp_seq=2 ttl=64 time=24.115 ms
84 bytes from 10.10.100.20 icmp_seq=3 ttl=64 time=27.472 ms
84 bytes from 10.10.100.20 icmp_seq=4 ttl=64 time=21.649 ms
84 bytes from 10.10.100.20 icmp_seq=5 ttl=64 time=21.290 ms
</code></pre>
</details>
</code></pre>
</details>
<details>
<summary>VPC13</summary>
<pre><code>
VPCS> ip 10.10.200.10 255.255.255.0 10.10.200.254
Checking for duplicate address...
PC1 : 10.10.200.10 255.255.255.0 gateway 10.10.200.254

VPCS>
VPCS> ping 10.10.200.254

10.10.200.254 icmp_seq=1 timeout
84 bytes from 10.10.200.254 icmp_seq=2 ttl=255 time=7.481 ms
84 bytes from 10.10.200.254 icmp_seq=3 ttl=255 time=25.172 ms
^C
VPCS> ping 10.10.200.20

84 bytes from 10.10.200.20 icmp_seq=1 ttl=64 time=26.299 ms
84 bytes from 10.10.200.20 icmp_seq=2 ttl=64 time=29.568 ms
84 bytes from 10.10.200.20 icmp_seq=3 ttl=64 time=31.852 ms
84 bytes from 10.10.200.20 icmp_seq=4 ttl=64 time=29.311 ms
84 bytes from 10.10.200.20 icmp_seq=5 ttl=64 time=29.753 ms
</code></pre>
</details>
</code></pre>
</details>
<details>
<summary>VPC14</summary>
<pre><code>
VPCS> ip 10.10.100.20 255.255.255.0 10.10.100.254
Checking for duplicate address...
PC1 : 10.10.100.20 255.255.255.0 gateway 10.10.100.254

VPCS> ping 10.10.100.254

84 bytes from 10.10.100.254 icmp_seq=1 ttl=255 time=22.312 ms
84 bytes from 10.10.100.254 icmp_seq=2 ttl=255 time=9.459 ms
^C
VPCS> ping 10.10.100.10

84 bytes from 10.10.100.10 icmp_seq=1 ttl=64 time=29.747 ms
84 bytes from 10.10.100.10 icmp_seq=2 ttl=64 time=32.468 ms
^C
VPCS> ping 10.10.100.10

84 bytes from 10.10.100.10 icmp_seq=1 ttl=64 time=25.230 ms
84 bytes from 10.10.100.10 icmp_seq=2 ttl=64 time=35.162 ms
</code></pre>
</details>
Вывод:

1. Настроен BGP peering между Leaf и Spine в AF l2vpn evpn
2. Spine работает в качестве route-reflector
3. Есть связность между VPC в одном VNI, подключенных к разным Leaf\Spine