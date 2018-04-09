---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 具有耐久性或效能儲存空間之 VMware 環境的 Brocade vRouter (Vyatta) 設定手冊

您可以在使用「耐久性」或「效能」儲存空間的 VMware 環境內配置 Brocade vRouter（Brocade vRouter (Vyatta) 應用裝置 [高可用性 (HA) 配置]）。將下列資訊與[進階單一網站 VMware 參照架構](https://console.bluemix.net/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window}一起使用，以在 VMware 環境中設定其中一個儲存空間選項。

## Brocade vRouter (Vyatta) 概觀

Brocade vRouter (Vyatta) 閘道將作為環境的閘道及路由器，並包含由子網路組成的區域。在區域之間會設定防火牆規則，以彼此通訊。對於不需要與其他區域通訊的區域，不需要任何防火牆規則，因為將會捨棄所有封包。

在範例配置中，將會在 Brocade vRouter (Vyatta) 中建立五個區域：

- SLSERVICE - SoftLayer 服務
- VMACCESS - 容量叢集上的虛擬機器 (VM)
- MGMT - 管理和容量叢集，以及管理 VM
- STORAGE - 儲存空間伺服器
- OUTSIDE - 公用網際網路存取


圖 1 說明每一個區域之間的通訊。請注意，您的環境可能有所不同，因此可能需要不同的區域及防火牆規則。

![圖 1：Brocade vRouter (Vyatta) 區域配置](/images/figure1_6.png)



## 配置 Brocade vRouter (Vyatta)

若要配置 Brocade vRouter (Vyatta)，請執行下列動作：

1. 使用在「裝置詳細資料」畫面上找到的 root 密碼，以 SSH 連接至應用裝置。
2. 鍵入 Configure 以進入配置模式，並遵循後續各節中的步驟。

### 設定介面

在本節中，我們將在兩台 Brocade vRouter (Vyatta) 上都配置結合介面，以鏈結至環境中的子網路。請記得，將我們的 VLAN（1101、1102 及 1103）取代為您環境中的對應 VLAN。也請注意，使用 <> 所做的指示應該取代為您環境的詳細資料（並移除 <>）。

使用下列指令，在 Brocade vRouter (Vyatta) 上配置結合介面。您必須處於配置模式。

Brocade vRouter (Vyatta) 1
```
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ (Enter an IP address from Primary Private Subnet Bound to VLAN 1101/Management)
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ (Enter an IP address from Portable Private Subnet Bound to VLAN 1101/Management VMs)
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ (Enter an IP address from Portable Private Subnet Bound to VLAN 1102/Storage Path A)
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ (Enter an IP address from Portable Private Subnet Bound to VLAN 1102/Storage Path B)
set interfaces bonding bond0 vif 1103 address ‘##.###.###.###/##’ (Enter an IP address from Portable Private Subnet Bound to VLAN 1103/Virtual Machines)
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
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ (Enter an IP address from Primary Private Subnet Bound to VLAN 1101/Management. Must be different than the one assigned to Brocade vRouter (Vyatta)1)
set interfaces bonding bond0 vif 1101 address ‘##.###.###.###/##’ (Enter an IP address from Portable Private Subnet Bound to VLAN 1101/Management VMs. Must be different than the one assigned to Brocade vRouter (Vyatta)1)
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ (Enter an IP address from Portable Private Subnet Bound to VLAN 1102/Storage Path A. Must be different than the one assigned to Brocade vRouter (Vyatta)1)
set interfaces bonding bond0 vif 1102 address ‘##.###.###.###/##’ (Enter an IP address from Portable Private Subnet Bound to VLAN 1102/Storage Path B. Must be different than the one assigned to Brocade vRouter (Vyatta)1)
set interfaces bonding bond0 vif 1103 address ‘##.###.###.###/##’ (Enter an IP address from Portable Private Subnet Bound to VLAN 1103/Virtual Machines. Must be different than the one assigned to Brocade vRouter (Vyatta)1)
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
### 配置 SNAT 進行外部存取

在此步驟中，我們將配置 SNAT，讓管理 VM 以及容量叢集上的 VM 可以存取網際網路。從此步驟開始，只需要在一台 Brocade vRouter (Vyatta) 上執行配置，因為我們會在稍後的時間點同步設定。

請在配置模式中使用下列指令：
```
set nat source rule 10
set nat source rule 10 source address ##.###.###.###/## (Portable Private Subnet VLAN1101/Management VMs)
set nat source rule 10 translation address ##.###.###.### (Brocade vRouter (Vyatta) bond1 IP)
set nat source rule 10 outbound-interface bond1
set nat source rule 20
set nat source rule 20 source address ##.###.###.###/## (Portable Private Subnet VLAN1103/Virtual Machines)
set nat source rule 20 translation address ##.###.###.### (Brocade vRouter (Vyatta) bond1 IP)
set nat source rule 20 outbound-interface bond1
commit
save
```
### 配置防火牆群組

接下來，我們將配置與特定 IP 範圍相關聯的防火牆群組。

請在配置模式中使用下列指令：
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
set firewall group network-group 1101PRIMARY network ###.###.###.### (Primary Private Subnet 1101/Management)
set firewall group network-group 1101MGMT network ###.###.###.### (Portable Private Subnet 1101/Management VMs)
set firewall group network-group 1102PRIMARY network ###.###.###.### (Primary Private Subnet 1102/Storage)
set firewall group network-group 1102STORAGEA network ###.###.###.### (Portable Private Subnet 1102/Storage Path A)
set firewall group network-group 1102STORAGEB network ###.###.###.### (Portable Private Subnet 1102/Storage Path B)
set firewall group network-group 1103VMACCESS network ###.###.###.### (Portable Private Subnet 1103/Virtual Machines)
set firewall group address-group PERFORMANCEENDURANCE address '##.###.###.###' (Performance/Endurance Primary IP)
set firewall group address-group PERFORMANCEENDURANCE address '##.###.###.###' (Performance/Endurance Secondary IP)
commit
save
```

### 配置防火牆名稱規則

我們現在將定義每一個資料流量方向的防火牆規則。

請在配置模式中使用下列指令：
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
### 配置區域連結

在此步驟中，我們會將特定區域連結至 Brocade vRouter (Vyatta) 上的介面。

請在配置模式中使用下列指令：
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

### 將防火牆規則套用至區域

我們現在會將防火牆規則套用至區域之間的通訊。

請在配置模式中使用下列指令：
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

### 與 HA 配對中的另一台 Brocade vRouter (Vyatta) 同步

因為我們已設定 HA 配對中的其中一台 Brocade vRouter (Vyatta)，所以必須將變更同步至另一台閘道裝置。

請在配置模式中使用下列指令：
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
### 關聯及遞送 VLAN

在 Brocade vRouter (Vyatta) 上設定區域及防火牆規則之後，我們必須將 VLAN 與其相關聯，並啟用透過 Brocade vRouter (Vyatta) 遞送 VLAN。

1. 登入 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}，按一下**網路 > 閘道應用裝置**，然後按一下 Brocade vRouter (Vyatta)。
2. 選取 **VLAN**，然後按一下**關聯**按鈕。
3. 針對您為環境建立的每一個 VLAN 重複步驟 2。接下來需要啟用 VLAN 的遞送，才能與 Brocade vRouter (Vyatta) 相關聯。
4. 在**關聯的 VLAN** 下找出 VLAN，然後勾選每一個 VLAN 旁的勾選框。
5. 按一下**大量動作**下拉功能表，然後選取**遞送**。
6. 在蹦現畫面上，按一下**確定**。

現在應該已透過 Brocade vRouter (Vyatta) 遞送您的 VLAN。如果您發現兩個區域之間的通訊受阻，請略過討論中的特定 VLAN，並檢查 Brocade vRouter (Vyatta) 設定。

您現在應該有運作中的單一網站 VMware 環境，並且由 IBM Cloud Infrastructure 內的 Brocade vRouter (Vyatta) 加以保護。

 
