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
| SPINE1 | E1/1 | Spine interconnect |  |
| | E1/2 | 10.2.1.1 | 255.255.255.252 |
|  | E1/3 | 10.2.1.5 | 255.255.255.252 |
|  | E1/4 | 10.2.1.9 | 255.255.255.252 |
|  | Lo0 | 10.255.1.101 | 255.255.255.255 |
| SPINE2 | E1/1 | Spine interconnect | |
| | E1/2 | 10.2.2.1 | 255.255.255.252 |
|  | E1/3 | 10.2.2.5 | 255.255.255.252 |
|  | E1/4 | 10.2.2.9 | 255.255.255.252 |
|  | Lo0 | 10.255.1.102 | 255.255.255.255 |
| SPINE3 | E1/1 | Spine interconnect | |
| | E1/2 | 10.2.3.1 | 255.255.255.252 |
| | Lo0 | 10.255.1.103 | 255.255.255.255 |
| LEAF1 | E1/1 | 10.2.1.2 | 255.255.255.252 |
|  | E1/2 | 10.2.2.2 | 255.255.255.252 |
|  | E1/3 | 172.16.1.1 | 255.255.255.252 |
|  | E1/4 | Port-channel1 |  |
|  | E1/5 | MCLAG to Client1 |  |
|  | E1/6 | Port-channel1 |  |
|  | Po1 | VPC peer-link |  |
|  | Lo0 | 10.255.1.11 | 255.255.255.255 |
| LEAF2 | E1/1 | 10.2.1.6 | 255.255.255.252 |
|  | E1/2 | 10.2.2.6 | 255.255.255.252 |
|  | E1/3 | 172.16.1.2 | 255.255.255.252 |
|  | E1/4 | Port-channel1 |  |
|  | E1/5 | MCLAG to Client1 | |
|  | E1/6 | Port-channel1 |  |
| | Po1 | VPC peer-link |  |
| | Lo0 | 10.255.1.12 | 255.255.255.255 |
| LEAF3 | E1/1 | 10.2.1.10 | 255.255.255.252 |
|  | E1/2 | 10.2.2.10 | 255.255.255.252 |
|  | E1/3 | 10.3.1.1 | 255.255.255.252 |
| | Lo0 | 10.255.1.13 | 255.255.255.255 |
| LEAF4 | E1/1 | 10.2.3.2 | 255.255.255.252 |
| | E1/2 | 10.4.1.1 | 255.255.255.252 |
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
