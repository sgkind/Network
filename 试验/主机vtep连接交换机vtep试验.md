# vxlan试验

## 拓扑图
                       +------------------------------------+
                       |                                    | vlan 20
                       |                交换机               | vlan 200 -> ip addr: 192.168.30.1/24
                       |  13                            31  | 
                       +------------------------------------+
                           |                            |
                           |                            |
                           |  vlan 200 tagged           |   vlan 20 tagged
                           |enp5s0                      |enp5s0:
                           |  ip addr: 192.168.30.3/24  |  ip addr: 20.0.2.2/24
                       +-------+                    +-------+
                       |       |                    |       |
                       |  142  |                    |  143  |
                       |  vtep |                    |       |
                       +-------+                    +-------+
                      vxlan0:                     
                      ip地址: 20.0.2.3/24

## 交换机侧设置

### vlan
1. 添加vlan
```
vlan database
vlan 20
vlan 200
exit
```

2. 为vlan 200设置ip address
```
interface vlan 200
ip address 192.168.30.1/24
exit
```

### 端口设置
1. 13端口
```
interface ethernet 1/13
switchport allowed vlan add 200 tagged
switchport native vlan 200
exit
```

2. 31端口
```
interface ethernet 1/31
switchport allowed vlan add 20 tagged
switchport native vlan 20
exit
```

### vxlan设置
```
vxlan source-interface vlan 200
vxlan vni 1000
vxlan vni 1000 access-port interface ethernet 1/31 vlan 20
vxlan vni 1000 flood r-vtep 192.168.30.1
vxlan vni 1000 flood r-vtep 192.168.30.3
```

## 142主机设置

### enp5s0网卡
将从网卡enp5s0出的包打上vlan 200的tag
```shell
$ cd /etc/netplan
$ cat  config.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eno1:
     dhcp4: no
     addresses: [192.168.200.142/24]
     gateway4: 192.168.200.1
     nameservers:
       addresses: [114.114.114.114]
    enp5s0: {}
  vlans:
    vlan200:
      addresses: [192.168.30.3/24]
      id: 200
      link: enp5s0

```

应用网络设置
```shell
$ sudo netplan apply
```

### 在142主机上添加vtep
```
$ sudo ip link add vxlan0 type vxlan id 1000 dstport 4789 remote 192.168.30.1 local 192.168.30.3 dev enp5s0
$ sudo ip addr add 20.0.2.3/24 dev vxlan0
$ sudo ip link set vxlan0 up
```

## 143主机设置
将从网卡enp5s0出的包打上vlan 20的tag
```shell
$ cd /etc/netplan
$ cat config.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eno1:
     dhcp4: no
     addresses: [192.168.200.143/24]
     gateway4: 192.168.200.1
     nameservers:
       addresses: [114.114.114.114]
    enp5s0: {}
  vlans:
    vlan20:
      addresses: [20.0.2.2/24]
      id: 20
      link: enp5s0
```

应用网络设置
```shell
$sudo netplan apply
```

## 测试
在142主机上ping 20.0.2.3
```shell
$ ping 20.0.2.3
PING 20.0.2.3 (20.0.2.3) 56(84) bytes of data.
64 bytes from 20.0.2.3: icmp_seq=1 ttl=64 time=0.260 ms
64 bytes from 20.0.2.3: icmp_seq=2 ttl=64 time=0.254 ms
64 bytes from 20.0.2.3: icmp_seq=3 ttl=64 time=0.219 ms
```