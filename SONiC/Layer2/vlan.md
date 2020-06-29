## 命令行配置
### 查看已创建的VLAN
* 查看已创建的VLAN


```shell
$ show vlan brief
+-----------+-----------------+------------+----------------+-----------------------+
|   VLAN ID | IP Address      | Ports      | Port Tagging   | DHCP Helper Address   |
+===========+=================+============+================+=======================+
|        42 | 192.168.42.1/24 | Ethernet37 | tagged         |                       |
+-----------+-----------------+------------+----------------+-----------------------+
|        43 | 192.168.43.1/24 | Ethernet37 | tagged         |                       |
+-----------+-----------------+------------+----------------+-----------------------+
```

* 查看VLAN配置


```shell
$ show vlan table
Name      VID  Member      Mode
------  -----  ----------  ------
Vlan42     42  Ethernet37  tagged
Vlan43     43  Ethernet37  tagged
```

### 创建VLAN
创建id为2的VLAN
```shell
$ sudo config vlan add 2
```

### 为VLAN添加ip地址
```shell
$ sudo config interface ip add Vlan2 192.168.6.1/24
```

### 将端口添加到VLAN中
* 端口为tagged
```shell
$ sudo config vlan member add 2 Ethernet5
$ sudo config vlan member add 2 PortChannel6
```
* 端口为untagged
```shell
$ sudo config vlan member add -u 2 Ethernet6
$ sudo config vlan member add -u 2 PortChannel6
```

### 将端口从VLAN中删除
```shell
$ sudo config vlan member del 2 Ethernet5
$ sudo config vlan member del 2 PortChannel3
```

### 删除VLAN
```shell
$ sudo config vlan del 2
```

## config DB

### VLAN
dhcp_servers是配置在此VLAN上的dhcp中继
```
{
"VLAN": {
	"Vlan1000": {
		"dhcp_servers": [
			"192.0.0.1",
			"192.0.0.2",
			"192.0.0.3",
			"192.0.0.4"
		],
		"members": [
			"Ethernet0",
			"Ethernet4",
			"Ethernet8",
			"Ethernet12"
		],
		"vlanid": "1000"
	}
  }
}
```

### VLAN_MEMBER
```
{
"VLAN_MEMBER": {
	"Vlan1000|PortChannel47": {
		"tagging_mode": "untagged"
	},
	"Vlan1000|Ethernet8": {
		"tagging_mode": "untagged"
	},
	"Vlan2000|PortChannel47": {
		"tagging_mode": "tagged"
	}
  }
}
```