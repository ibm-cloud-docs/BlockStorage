---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Brocade vRouter (Vyatta) 设置指南 - 针对具有耐久性或性能存储器的 VMware 环境

可以在使用耐久性或性能存储器的 VMware 环境中配置 Brocade vRouter (Vyatta) 设备 [高可用性 (HA) 配置]。将以下信息与[高级单站点 VMware 参考体系结构](https://console.bluemix.net/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window}结合使用，在 VMware 环境中设置其中一个存储器选项。

## Brocade vRouter (Vyatta) 概述

Brocade vRouter (Vyatta) 网关将充当环境的网关和路由器，并包含由子网组成的区域。将在区域之间设置防火墙规则，以便各区域可以相互通信。对于不需要与其他区域通信的区域，无需防火墙规则，因为将丢弃所有包。

在示例配置中，将在 Brocade vRouter (Vyatta) 中创建五个区域：

- SLSERVICE - SoftLayer 服务
- VMACCESS - 容量集群上的虚拟机 (VM)
- MGMT - 管理和容量集群以及管理 VM
- STORAGE - 存储服务器
- OUTSIDE - 公用因特网访问


图 1 描述了各个区域之间的通信。请注意，您的环境可能有所不同，可能需要不同的区域和防火墙规则。

![图 1：Brocade vRouter (Vyatta) 区域配置](/images/figure1_6.png)



## 配置 Brocade vRouter (Vyatta)

要配置 Brocade vRouter (Vyatta)，请执行以下操作：

1. 使用在“设备详细信息”屏幕上找到的 root 用户密码通过 SSH 登录到设备。
2. 输入“配置”以进入配置方式，然后执行后续部分中的步骤。

### 设置接口

在此部分中，将在两个 Brocade vRouter (Vyatta) 上配置结合接口以用于链接到环境中的子网。请记住将 VLAN（1101、1102 和 1103）替换为您环境中相应的 VLAN。另请注意，<> 中的指示信息应替换为您环境的相应详细信息（除去 <>）。

使用以下命令在 Brocade vRouter (Vyatta) 上配置结合接口。您必须处于配置方式。

Brocade vRouter (Vyatta) 1
```
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ （输入主专用子网中绑定到 VLAN 1101/管理的 IP 地址）
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ （输入可移植专用子网中绑定到 VLAN 1101/管理 VM 的 IP 地址）
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ （输入可移植专用子网中绑定到 VLAN 1102/存储路径 A 的 IP 地址）
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ （输入可移植专用子网中绑定到 VLAN 1102/存储路径 B 的 IP 地址）
set interfaces bonding bond0 vif 1103 address ‘##.###.###.###/##’ （输入可移植专用子网中绑定到 VLAN 1103/虚拟机的 IP 地址）
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 advertise-interval '1'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 preempt 'false'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 priority '253'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 virtual-address ‘<GATEWAY ADDRESS/MASK of Primary Private VLAN Bound to 1101/Management>’
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 virtual-address ‘<GATEWAY ADDRESS/MASK of Portable Private VLAN Bound to 1101/Management VMs>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 advertise-interval '1'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 preempt 'false'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 priority '253'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY ADDRESS/MASK of Primary Private VLAN Bound to 1102/Storage>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY ADDRESS/MASK of Portable Private VLAN Bound to 1102/Storage Path A>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY ADDRESS/MASK of Portable Private VLAN Bound to 1102/Storage Path B>’
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 advertise-interval '1'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 preempt 'false'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 priority '253'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 virtual-address ‘<GATEWAY ADDRESS/MASK of Primary Private VLAN Bound to 1103/Virtual Machines>’
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 virtual-address ‘<GATEWAY ADDRESS/MASK of Portable Private VLAN Bound to 1103/Virtual Machines>’
commit
save
```

Brocade vRouter (Vyatta) 2
```
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ （输入主专用子网中绑定到 VLAN 1101/管理的 IP 地址。必须不同于分配给 Brocade vRouter (Vyatta) 1 的 IP 地址）
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ （输入可移植专用子网中绑定到 VLAN 1101/管理 VM 的 IP 地址。必须不同于分配给 Brocade vRouter (Vyatta) 1 的 IP 地址）
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ （输入可移植专用子网中绑定到 VLAN 1102/存储路径 A 的 IP 地址。必须不同于分配给 Brocade vRouter (Vyatta) 1 的 IP 地址）
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ （输入可移植专用子网中绑定到 VLAN 1102/存储路径 B 的 IP 地址。必须不同于分配给 Brocade vRouter (Vyatta) 1 的 IP 地址）
set interfaces bonding bond0 vif 1103 address ‘##.###.###.###/##’ （输入可移植专用子网中绑定到 VLAN 1103/虚拟机的 IP 地址。必须不同于分配给 Brocade vRouter (Vyatta) 1 的 IP 地址）
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 advertise-interval '1'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 preempt 'false'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 priority '253'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 virtual-address ‘<GATEWAY ADDRESS/MASK of Primary Private VLAN Bound to 1101/Management>’
set interfaces bonding bond0 vif 1101 vrrp vrrp-group 2 virtual-address ‘<GATEWAY ADDRESS/MASK of Portable Private VLAN Bound to 1101/Management VMs>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 advertise-interval '1'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 preempt 'false'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 priority '253'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY ADDRESS/MASK of Primary Private VLAN Bound to 1102/Storage>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY ADDRESS/MASK of Portable Private VLAN Bound to 1102/Storage Path A>’
set interfaces bonding bond0 vif 1102 vrrp vrrp-group 3 virtual-address ‘<GATEWAY ADDRESS/MASK of Portable Private VLAN Bound to 1102/Storage Path B>’
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 advertise-interval '1'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 preempt 'false'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 priority '253'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 'rfc3768-compatibility'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 sync-group 'vgroup1'
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 virtual-address ‘<GATEWAY ADDRESS/MASK of Primary Private VLAN Bound to 1103/Virtual Machines>’
set interfaces bonding bond0 vif 1103 vrrp vrrp-group 4 virtual-address ‘<GATEWAY ADDRESS/MASK of Portable Private VLAN Bound to 1103/Virtual Machines>’
commit
save
```
### 配置 SNAT 进行外部访问

在此步骤中，我们将配置 SNAT，以便管理 VM 和容量集群上的 VM 可以访问因特网。从此步骤开始，只需在一个 Brocade vRouter (Vyatta) 上完成配置，因为我们将在稍后时间点对设置进行同步。

在配置方式下使用以下命令：
```
set nat source rule 10
set nat source rule 10 source address ##.###.###.###/## （可移植专用子网 VLAN1101/管理 VM）
set nat source rule 10 translation address ##.###.###.### (Brocade vRouter (Vyatta) bond1 IP)
set nat source rule 10 outbound-interface bond1
set nat source rule 20
set nat source rule 20 source address ##.###.###.###/## （可移植专用子网 VLAN1103/虚拟机）
set nat source rule 20 translation address ##.###.###.### (Brocade vRouter (Vyatta) bond1 IP)
set nat source rule 20 outbound-interface bond1
commit
save
```
### 配置防火墙组

接下来，将配置与特定 IP 范围关联的防火墙组。

在配置方式下使用以下命令：
```
set firewall group network-group SLSERVICES
set firewall group network-group SLSERVICES network 10.1.128.0/19
set firewall group network-group SLSERVICES network 10.0.86.0/24
set firewall group network-group SLSERVICES network 10.1.176.0/24
set firewall group network-group SLSERVICES network 10.1.64.0/19
set firewall group network-group SLSERVICES network 10.1.96.0/19
set firewall group network-group SLSERVICES network 10.1.192.0/20
set firewall group network-group SLSERVICES network 10.1.160.0/20
set firewall group network-group SLSERVICES network 10.2.32.0/20
set firewall group network-group SLSERVICES network 10.2.64.0/20
set firewall group network-group SLSERVICES network 10.0.64.0/19
set firewall group network-group SLSERVICES network 10.2.128.0/20
set firewall group network-group SLSERVICES network 10.2.200.0/24
set firewall group network-group SLSERVICES network 10.1.0.0/24
set firewall group network-group SLSERVICES network 10.1.24.0/24
set firewall group network-group SLSERVICES network 10.2.208.0/24
set firewall group network-group SLSERVICES network 10.1.236.0/24
set firewall group network-group SLSERVICES network 10.1.56.0/24
set firewall group network-group SLSERVICES network 10.1.8.0/24
set firewall group network-group SLSERVICES network 10.1.224.0/24
set firewall group network-group SLSERVICES network 10.2.192.0/24
set firewall group network-group SLSERVICES network 10.1.16.0/24
set firewall group network-group SLSERVICES network 10.0.0.0/14
set firewall group network-group 1101PRIMARY network ###.###.###.### （主专用子网 1101/管理）
set firewall group network-group 1101MGMT network ###.###.###.### （可移植专用子网 1101/管理 VM）
set firewall group network-group 1102PRIMARY network ###.###.###.### （主专用子网 1102/存储器）
set firewall group network-group 1102STORAGEA network ###.###.###.### （可移植专用子网 1102/存储路径 A）
set firewall group network-group 1102STORAGEB network ###.###.###.### （可移植专用子网 1102/存储路径 B）
set firewall group network-group 1103VMACCESS network ###.###.###.### （可移植专用子网 1103/虚拟机）
set firewall group address-group PERFORMANCEENDURANCE address '##.###.###.###' （性能/耐久性主 IP）
set firewall group address-group PERFORMANCEENDURANCE address '##.###.###.###' （性能/耐久性辅助 IP）
commit
save
```

### 配置防火墙名称规则

现在，将为每个方向的流量定义防火墙规则。

在配置方式下使用以下命令：
```
set firewall name INSIDE2OUTSIDE
set firewall name INSIDE2OUTSIDE default-action drop
set firewall name INSIDE2OUTSIDE rule 10 action accept
set firewall name INSIDE2OUTSIDE rule 10 protocol all
set firewall name INSIDE2OUTSIDE rule 10 source group network-group 1101MGMT
set firewall name INSIDE2OUTSIDE rule 20 action accept
set firewall name INSIDE2OUTSIDE rule 20 protocol all
set firewall name INSIDE2OUTSIDE rule 20 source group network-group 1103VMACCESS
set firewall name OUTSIDE2INSIDE
set firewall name OUTSIDE2INSIDE default-action drop
set firewall name OUTSIDE2INSIDE rule 10 action accept
set firewall name OUTSIDE2INSIDE rule 10 protocol udp
set firewall name OUTSIDE2INSIDE rule 20 action accept
set firewall name OUTSIDE2INSIDE rule 20 protocol udp
set firewall name OUTSIDE2INSIDE rule 20 destination port 4500
set firewall name OUTSIDE2INSIDE rule 30 action accept
set firewall name OUTSIDE2INSIDE rule 30 protocol udp
set firewall name OUTSIDE2INSIDE rule 30 destination port 500
set firewall name OUTSIDE2INSIDE rule 40 action accept
set firewall name OUTSIDE2INSIDE rule 40 ipsec match-ipsec
set firewall name OUTSIDE2INSIDE rule 50 action accept
set firewall name OUTSIDE2INSIDE rule 50 protocol gre
set firewall name OUTSIDE2INSIDE rule 60 action accept
set firewall name OUTSIDE2INSIDE rule 60 protocol tcp
set firewall name OUTSIDE2INSIDE rule 60 destination port 1723
set firewall name OUTSIDE2INSIDE rule 70 action accept
set firewall name OUTSIDE2INSIDE rule 70 protocol tcp
set firewall name OUTSIDE2INSIDE rule 70 destination port 80
set firewall name OUTSIDE2INSIDE rule 80 action accept
set firewall name OUTSIDE2INSIDE rule 80 protocol tcp
set firewall name OUTSIDE2INSIDE rule 80 destination port 443
set firewall name OUTSIDE2INSIDE rule 90 action accept
set firewall name OUTSIDE2INSIDE rule 90 state established enable
set firewall name SLSERVICE2INSIDE
set firewall name SLSERVICE2INSIDE default-action drop
set firewall name SLSERVICE2INSIDE rule 10 action accept
set firewall name SLSERVICE2INSIDE rule 10 protocol all
set firewall name SLSERVICE2INSIDE rule 10 source group network-group SLSERVICES
set firewall name INSIDE2SLSERVICE
set firewall name INSIDE2SLSERVICE default-action drop
set firewall name INSIDE2SLSERVICE rule 10 action accept
set firewall name INSIDE2SLSERVICE rule 10 protocol all
set firewall name INSIDE2SLSERVICE rule 10 destination group network-group SLSERVICES
set firewall name VMACCESS2MGMT
set firewall name VMACCESS2MGMT default-action drop
set firewall name VMACCESS2MGMT rule 10 action drop
set firewall name VMACCESS2MGMT rule 10 protocol all
set firewall name VMACCESS2MGMT rule 10 source group network-group 1103VMACCESS
set firewall name STORAGE2MGMT
set firewall name STORAGE2MGMT default-action drop
set firewall name STORAGE2MGMT rule 10 action accept
set firewall name STORAGE2MGMT rule 10 protocol all
set firewall name STORAGE2MGMT rule 10 source group network-group 1102PRIMARY
set firewall name STORAGE2MGMT rule 20 action accept
set firewall name STORAGE2MGMT rule 20 protocol all
set firewall name STORAGE2MGMT rule 20 source group network-group 1102STORAGEA
set firewall name STORAGE2MGMT rule 30 action accept
set firewall name STORAGE2MGMT rule 30 protocol all
set firewall name STORAGE2MGMT rule 30 source group network-group 1102STORAGEB
set firewall name MGMT2STORAGE
set firewall name MGMT2STORAGE default-action drop
set firewall name MGMT2STORAGE rule 10 action accept
set firewall name MGMT2STORAGE rule 10 protocol all
set firewall name MGMT2STORAGE rule 10 source group network-group 1101PRIMARY
set firewall name MGMT2STORAGE rule 10 source group network-group 1101MGMT
set firewall name INSIDE2SLSERVICE rule 6 action 'accept'
set firewall name INSIDE2SLSERVICE rule 6 destination group address-group ' PERFORMANCEENDURANCE '
set firewall name INSIDE2SLSERVICE rule 6 protocol 'all'
set firewall name INSIDE2SLSERVICE rule 6 source group network-group '1102STORAGEA'
set firewall name INSIDE2SLSERVICE rule 7 action 'accept'
set firewall name INSIDE2SLSERVICE rule 7 destination group address-group ' PERFORMANCEENDURANCE '
set firewall name INSIDE2SLSERVICE rule 7 protocol 'all'
set firewall name INSIDE2SLSERVICE rule 7 source group network-group '1102STORAGEB'
set firewall name INSIDE2SLSERVICE rule 5 action 'accept'
set firewall name INSIDE2SLSERVICE rule 5 destination group address-group ' PERFORMANCEENDURANCE '
set firewall name INSIDE2SLSERVICE rule 5 protocol 'all'
set firewall name INSIDE2SLSERVICE rule 5 source group network-group '1102PRIMARY'
set firewall name SLSERVICE2INSIDE rule 6 action 'accept'
set firewall name SLSERVICE2INSIDE rule 6 source group address-group ' PERFORMANCEENDURANCE '
set firewall name SLSERVICE2INSIDE rule 6 protocol 'all'
set firewall name SLSERVICE2INSIDE rule 6 destination group network-group '1102STORAGEA'
set firewall name SLSERVICE2INSIDE rule 7 action 'accept'
set firewall name SLSERVICE2INSIDE rule 7 source group address-group ' PERFORMANCEENDURANCE '
set firewall name SLSERVICE2INSIDE rule 7 protocol 'all'
set firewall name SLSERVICE2INSIDE rule 7 destination group network-group '1102STORAGEB'
set firewall name SLSERVICE2INSIDE rule 5 action 'accept'
set firewall name SLSERVICE2INSIDE rule 5 source group address-group ' PERFORMANCEENDURANCE '
set firewall name SLSERVICE2INSIDE rule 5 protocol 'all'
set firewall name SLSERVICE2INSIDE rule 5 destination group network-group '1102PRIMARY'
set firewall name INSIDE2SLSERVICE rule 8 action 'drop'
set firewall name INSIDE2SLSERVICE rule 8 destination group address-group ' PERFORMANCEENDURANCE '
set firewall name INSIDE2SLSERVICE rule 8 protocol 'all'
set firewall name SLSERVICE2INSIDE rule 8 action 'drop'
set firewall name SLSERVICE2INSIDE rule 8 source group address-group ' PERFORMANCEENDURANCE '
set firewall name SLSERVICE2INSIDE rule 8 protocol 'all'
commit
save
```
### 配置区域绑定

在此步骤中，要将特定区域绑定到 Brocade vRouter (Vyatta) 上的接口。

在配置方式下使用以下命令：
```
set zone-policy zone OUTSIDE description “Internet Zone”
set zone-policy zone OUTSIDE default-action drop
set zone-policy zone OUTSIDE interface bond1
set zone-policy zone SLSERVICE description “SoftLayer Services”
set zone-policy zone SLSERVICE default-action drop
set zone-policy zone SLSERVICE interface bond0
set zone-policy zone MGMT description “Management VMs & ESX Host Access”
set zone-policy zone MGMT default-action drop
set zone-policy zone MGMT interface bond0.1101
set zone-policy zone STORAGE description “Storage”
set zone-policy zone STORAGE default-action drop
set zone-policy zone STORAGE interface bond0.1102
set zone-policy zone VMACCESS description “VM Access”
set zone-policy zone VMACCESS default-action drop
set zone-policy zone VMACCESS interface bond0.1103
commit
save
```

### 将防火墙规则应用于区域

现在，将对区域之间的通信应用防火墙规则。

在配置方式下使用以下命令：
```
set zone-policy zone OUTSIDE from MGMT firewall name INSIDE2OUTSIDE
set zone-policy zone OUTSIDE from VMACCESS firewall name INSIDE2OUTSIDE
set zone-policy zone VMACCESS from OUTSIDE firewall name OUTSIDE2INSIDE
set zone-policy zone VMACCESS from SLSERVICE firewall name SLSERVICE2INSIDE
set zone-policy zone MGMT from OUTSIDE firewall name OUTSIDE2INSIDE
set zone-policy zone MGMT from SLSERVICE firewall name SLSERVICE2INSIDE
set zone-policy zone SLSERVICE from MGMT firewall name INSIDE2SLSERVICE
set zone-policy zone SLSERVICE from VMACCESS firewall name INSIDE2SLSERVICE
set zone-policy zone SLSERVICE from STORAGE firewall name INSIDE2SLSERVICE
set zone-policy zone STORAGE from SLSERVICE firewall name SLSERVICE2INSIDE
set zone-policy zone STORAGE from MGMT firewall name MGMT2STORAGE
commit
save
```

### 与 HA 对中的另一个 Brocade vRouter (Vyatta) 同步

我们已设置了 HA 对中的其中一个 Brocade vRouter (Vyatta)，因此必须将更改同步到另一个网关设备。

在配置方式下使用以下命令：
```
set system config-sync remote-router <OTHER BROCADE VROUTER (VYATTA) IP> password <OTHER BROCADE VROUTER (VYATTA) PASSWORD>
set system config-sync remote-router <OTHER BROCADE VROUTER (VYATTA) IP> sync-map 'SYNC'
set system config-sync remote-router <OTHER BROCADE VROUTER (VYATTA) IP> username <OTHER BROCADE VROUTER (VYATTA) USERNAME>
set system config-sync sync-map SYNC rule 1 action 'include'
set system config-sync sync-map SYNC rule 1 location 'nat'
set system config-sync sync-map SYNC rule 2 action 'include'
set system config-sync sync-map SYNC rule 2 location 'firewall'
set system config-sync sync-map SYNC rule 3 action 'include'
set system config-sync sync-map SYNC rule 3 location 'vpn'
set system config-sync sync-map SYNC rule 4 action 'include'
set system config-sync sync-map SYNC rule 4 location 'interfaces tunnel'
set system config-sync sync-map SYNC rule 5 action 'include'
set system config-sync sync-map SYNC rule 5 location 'firewall'
set system config-sync sync-map SYNC rule 6 action 'include'
set system config-sync sync-map SYNC rule 6 location 'zone-policy'
set system config-sync sync-map SYNC rule 7 action 'include'
set system config-sync sync-map SYNC rule 7 location 'vpn'
set system config-sync sync-map SYNC rule 8 action 'include'
set system config-sync sync-map SYNC rule 8 location 'protocols static route'
set system config-sync sync-map SYNC rule 9 action 'include'
set system config-sync sync-map SYNC rule 9 location 'protocols static table'
set system config-sync sync-map SYNC rule 10 action 'include'
set system config-sync sync-map SYNC rule 10 location 'policy route'
set system config-sync sync-map SYNC rule 11 action 'include'
set system config-sync sync-map SYNC rule 11 location 'nat'
commit
save
```
### 关联和路由 VLAN

在 Brocade vRouter (Vyatta) 上设置区域和防火墙规则后，必须将 VLAN 与其相关联，并通过 Brocade vRouter (Vyatta) 启用这些 VLAN 的路由。

1. 登录到 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 并单击**网络 > 网关设备**，然后单击 Brocade vRouter (Vyatta)。
2. 选择 **VLAN**，然后单击**关联**按钮。
3. 对于为环境创建的每个 VLAN，重复步骤 2。接下来，VLAN 需要启用路由，以便与 Brocade vRouter (Vyatta) 相关联。
4. 在**关联的 VLAN** 下找到所需 VLAN，并选中每个 VLAN 旁边的框。
5. 单击**批量操作**下拉菜单，然后选择**路由**。
6. 在弹出屏幕上，单击**确定**。

现在，VLAN 应该已通过 Brocade vRouter (Vyatta) 进行路由。如果您注意到两个区域之间存在通信障碍，请绕过相关的特定 VLAN，并检查 Brocade vRouter (Vyatta) 设置。

现在，您应该在 IBM Cloud 基础架构中拥有通过 Brocade vRouter (Vyatta) 保护的有效单站点 VMware 环境。

 
