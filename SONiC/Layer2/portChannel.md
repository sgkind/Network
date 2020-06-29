## 动态端口聚合(LACP)
### 添加端口聚合
* 添加名为PortChannel1的端口聚合，并将Ethernet1和Ethernet2网卡添加到PortChannel1中

```shell
$ sudo config portchannel add PortChannel1
$ sudo config portchannel member add PortChannel1 Ethernet1
$ sudo config portchannel member add PortChannel1 Ethernet2
```

### 查看已经添加的端口聚合
* teamshow

```shell
$ sudo teamshow
```

* interface

```shell
$ show interface portchannel
```

### 查看端口聚合状态
```
$ sudo teamdctl PortChannel1 state view
```

### 删除端口聚合
* 删除portchannel中的成员端口
```shell
$ sudo config portchannel member del PortChannel1 Ethernet1
$ sudo config portchannel member del PortChannel1 Ethernet2
```

* 删除端口聚合
```shell
$ sudo config portchannel del PortChannel1
```

## 静态端口聚合
TBD

## 配置字段
端口聚合定义在PORTCHANNEL表中

```
{
"PORTCHANNEL": {
        "PortChannel0003": {
                "admin_status": "up",
        "min_links": "1",
        "members": [
            "Ethernet54"
        ],
        "mtu": "9100"
    },
    "PortChannel0004": {
        "admin_status": "up",
        "min_links": "1",
        "members": [
            "Ethernet56"
        ],
        "mtu": "9100"
    }
  }
}
```

Portchannel成员表

```
{
"PORTCHANNEL_MEMBER": {
    "PortChannel0001|Ethernet50": {}, 
    "PortChannel0002|Ethernet52": {}, 
    "PortChannel0003|Ethernet54": {}, 
    "PortChannel0004|Ethernet56": {}
  }
}
```