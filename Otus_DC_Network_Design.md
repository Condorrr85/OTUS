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

- настроить OSPF в Underlay сети, для IP связанности между всеми устройствами NXOS
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

[Настройки](https://github.com/Condorrr85/OTUS/tree/main/config)

### Проверка доступности Loopback по Underlay OSPF сети на примере Leaf4

**Leaf4# show ip route ospf-1**
IP Route Table for VRF "default"
'*' denotes best ucast next-hop
'**' denotes best mcast next-hop
'[x/y]' denotes [preference/metric]
'%<string>' in via output denotes VRF <string>

**10.255.1.11/32**, ubest/mbest: 1/0
    *via **10.255.1.103, Eth1/1**, [110/131], 00:15:57, **ospf-1, intra**
**10.255.1.12/32**, ubest/mbest: 1/0
    *via **10.255.1.103, Eth1/1**, [110/131], 00:15:57, **ospf-1, intra**
**10.255.1.13/32**, ubest/mbest: 1/0
    *via **10.255.1.103, Eth1/1**, [110/131], 00:15:57, **ospf-1, intra**
**10.255.1.101/32**, ubest/mbest: 1/0
    *via **10.255.1.103, Eth1/1**, [110/91], 00:15:57, **ospf-1, intra**
**10.255.1.102/32**, ubest/mbest: 1/0
    *via **10.255.1.103, Eth1/1**, [110/91], 00:15:57, **ospf-1, intra**
**10.255.1.103/32**, ubest/mbest: 1/0
    *via **10.255.1.103, Eth1/1**, [110/41], 00:15:57, **ospf-1, intra**
**192.168.10.40/32**, ubest/mbest: 1/0
    *via **10.255.1.103, Eth1/1**, [110/81], 00:15:57, **ospf-1, intra**
