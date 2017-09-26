---

copyright:
  years: 2014, 2017
lastupdated: "2017-09-08"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Brocade vRouter (Vyatta) Set up Guide for VMware Environments with Endurance or Performance Storage

You can configure a Brocade vRouter (Brocade vRouter (Vyatta) appliance [high availability (HA) configuration] within a VMware environment that is using either Endurance or Performance storage. Use the following information in conjunction with the [Advanced Single-Site VMware Reference Architecture](http://knowledgelayer.softlayer.com/articles/advanced-single-site-vmware-reference-architecturesoftlayer) to set up one of these storage options in your VMware environment.

## Brocade vRouter (Vyatta) Overview

The Brocade vRouter (Vyatta) gateway will serve as a gateway and router for your environment and contain zones comprising of subnets. Firewall rules will be set in place between zones so they can communicate with each other. For those zones that do not need to communicate with other zones, no firewall rule is needed as all packets will be dropped.

In our example configuration, there will be five zones created in the Brocade vRouter (Vyatta):

- SLSERVICE – SoftLayer services
- VMACCESS – Virtual machines(VMs) on the capacity cluster
- MGMT – Management and capacity clusters as well as management VMs
- STORAGE – Storage server(s)
- OUTSIDE – Public Internet access


Figure 1 describes the communication between each zone. Note that your environment may be different and may require different zones and firewall rules.

![Figure 1: Brocade vRouter (Vyatta) Zone Configuration](/images/figure1_6.png)



## Configure Brocade vRouter (Vyatta)

To configure the Brocade vRouter (Vyatta):

1. SSH into the appliance using the root password found on the Device Details screen.
2. Type Configure to enter the configuration mode and follow the steps in the subsequent sections.

### Set up Interfaces

In this section, we will configure the bond interfaces on both Brocade vRouters (Vyatta) to be linked to the subnets in the environment. Remember to replace our VLANs (1101, 1102, and 1103) with the corresponding VLANs in your environment. Also note that instructions made with <> should be replaced with your environment’s details (with the <> removed).

Use the following commands to configure the bond interfaces on the Brocade vRouters (Vyatta). You must be in configure mode.

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
### Configure SNAT for External Access

In this step, we will configure SNAT so that management VMs and VMs on the capacity cluster can access the Internet. From this step forward, configuration only needs to be done on one Brocade vRouter (Vyatta) since we will sync the setup at a later point in time.

Use the following commands in configure mode:
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
### Configure Firewall Groups

Next, we will configure the firewall groups that are associated with certain IP ranges.

Use the following commands in configure mode:
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

### Configure Firewall Name Rules

We will now define the firewall rules for each direction of traffic.

Use the following commands in configure mode:
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
### Configure Zone Bindings

In this step, we will bind particular zones to interfaces on the Brocade vRouter (Vyatta).

Use the following commands in configure mode:
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

### Apply Firewall Rules to Zones

We will now apply the firewall rules to the communication between zones.

Use the following commands in configure mode:
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

### Sync with other Brocade vRouter (Vyatta) in the HA Pair

Since we have set up one of the Brocade vRouter’s (Vyatta) in the HA pair, we must sync the changes to the other gateway device.

Use the following commands in configure mode:
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
### Associate and Route VLANs

Once the zones and firewall rules have been set up on the Brocade vRouter (Vyatta), we must associate the VLANs to it and enable routing of the VLANs via the Brocade vRouter (Vyatta).

1. Log in to the [Customer portal](https://control.softlayer.com/) and click on **Network > Gateway Appliance** and click on the Brocade vRouter (Vyatta).
2. Select a **VLAN** and click on the **Associate** button.
3. Repeat step 2 for each VLAN you created for your environment. The VLANs next need to have routing enabled in order to be associated with the Brocade vRouter (Vyatta).
4. Locate the VLANs under **Associated VLANs** and check the box next to each one.
5. Click on the **Bulk Actions** drop–down menu and select **Route**.
6. Click **OK** on the pop-up screen.

Your VLANs should now be routed via the Brocade vRouter (Vyatta). If you notice that communication is hindered between two zones, bypass the particular VLAN(s) in question and check your Brocade vRouter (Vyatta) settings.

You should now have a working single-site VMware environment secured by a Brocade vRouter (Vyatta) within IBM Cloud Infrastructure.

 
