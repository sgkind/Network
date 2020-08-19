# AOS交换机操作

## 系统命令
1. 进入配置模式
```
configure
```

2.  重启
```
reload
```

3. 进入linux命令行
```
linux shell
```

4. 配置路由
在linux命令行下执行
```
ip route add default via 192.168.200.1
```

## vlan

### 配置vlan
```
vlan  database
vlan 20
vlan 200
exit
```

### 设置ip地址
```
interface vlan 200
ip address 192.168.110.1/24
exit
```

## 端口

### 端口设置vlan

```
interface ethernet 1/33
switchport allowed vlan add 20 tagged
switchport native vlan 20
exit
```

## vxlan

### vxlan设置source-interface
```
vxlan source-interface vlan 200
```

对进入交换机的带vlan 200 tag的数据包，交换机将vlan tag剥离，然后对数据包进行vxlan解包

### 添加vxlan的vni
```
vxlan vni 1000
```

### 指定vxlan中某个vni的access-port
```
vxlan vni 1000 access-port interface ethernet 1/33 vlan 20
```
对从端口33进入的vlan id为20的数据包进行vxlan封包，其中vxlan的vni为1000，然后根据目的mac地址进行查表后转发，如果表中无此地址则进行泛洪到交换机设置的所有vtep

### 设置交换机的远端vtep
```
vxlan vni 1000 flood r-vtep 192.168.100.1
```

