+++
author = "Bryan Florenosos"
title = "LACP, PAGP, MSTP"
date = "2021-04-15"
description = "Switching Fundamentals"
tags = [
    "network",
    "switching",
]
+++

## Topology of the Network:  
{{< bokya src="img/switching/etherchannel.jpg" >}}  

**Task 1**  
Disable VTP on all switches. All switches should support the configuration, modification and deletion of VLANs.  
`vtp mode transparent`  

**Task 2**  
Configure EtherChannel trunks on the switches illustrated in the topology. Only switches Switch1 and Switch2 should actively initiate channel establishment; switches Swiwtch3 and Switch4 should be configured as passive links that will should not actively attempt to
establish an EtherChannel.The EtherChannels should be configured as follows:  
**a.** The two ports between Switch1 and Switch2 belong to channel group 1 and use mode on  
**b.** The two ports between Switch1 and Switch3 belong to channel group 2 and use LACP  
**c.** The two ports between Switch1 and Switch4 belong to channel group 3 and use PAgP  
**d.** The two ports between Switch2 and Switch3 belong to channel group 4 and use LACP  
**e.** The two ports between Switch2 and Switch4 belong to channel group 5 and use PAgP  

**A**:  
**Switch1:**  
`interface range GigabitEthernet0/0 - 1`  
 `description to Switch2 etherchannel PO1`  
 `switchport trunk encapsulation dot1q`  
 `switchport mode trunk`  
 `media-type rj45`  
 `negotiation auto`  
 `channel-group 1 mode on`  
**Switch2:**  
`interface range GigabitEthernet0/0 - 1`  
 `description to Switch1 etherchannel PO1`  
 `switchport trunk encapsulation dot1q`  
 `switchport mode trunk`  
 `media-type rj45`  
 `negotiation auto`  
 `channel-group 1 mode on`  
  
**B**  
**Switch1:**  
`interface range GigabitEthernet0/2 - 3`  
 `description to Switch3 LACP PO2`  
 `switchport trunk encapsulation dot1q`
 `switchport mode trunk`
 `media-type rj45`
 `negotiation auto`
 `channel-group 2 mode active`  
 **Switch3:**  
`interface range GigabitEthernet0/2 - 3`  
 `description to Switch1 LACP PO2`  
 `switchport trunk encapsulation dot1q`
 `switchport mode trunk`
 `media-type rj45`
 `negotiation auto`
 `channel-group 2 mode passive`  
  
**C**  
**Switch1:**  
`interface range GigabitEthernet1/0 - 1`  
 `description to Switch4 PAGP Po3`  
 `switchport trunk encapsulation dot1q`  
 `switchport mode trunk`  
 `media-type rj45`  
 `negotiation auto`  
 `channel-group 3 mode desirable`  
**Switch4:**  
`interface range GigabitEthernet1/0 - 1`  
 `description to Switch1 PAGP Po3`  
 `switchport trunk encapsulation dot1q`  
 `switchport mode trunk`  
 `media-type rj45`  
 `negotiation auto`  
 `channel-group 3 mode auto`  
   
**D**  
**Switch2:**
`interface range GigabitEthernet1/0 - 1`  
 `description to Switch3 LACP PO4`  
 `switchport trunk encapsulation dot1q`  
 `switchport mode trunk`  
 `media-type rj45`  
 `negotiation auto`  
 `channel-group 4 mode active`  
**Switch3:**
`interface range GigabitEthernet1/0 - 1`  
 `description to Switch2 LACP PO4`  
 `switchport trunk encapsulation dot1q`  
 `switchport mode trunk`  
 `media-type rj45`  
 `negotiation auto`  
 `channel-group 4 mode passive`  
  
**E**  
**Switch2**  
`interface range GigabitEthernet0/2 - 3`  
 `description to Switch4 PAGP PO5`  
 `switchport trunk encapsulation dot1q`  
 `switchport mode trunk`  
 `media-type rj45`  
 `negotiation auto`  
 `channel-group 5 mode desirable`  
**Switch4**  
`interface range GigabitEthernet0/2 - 3`  
 `description to Switch2 PAGP PO5`  
 `switchport trunk encapsulation dot1q`  
 `switchport mode trunk`  
 `media-type rj45`  
 `negotiation auto`  
 `channel-group 5 mode auto`  
  
**Task 3**  
Following the EtherChannel configuration, protect the switched network from any future misconfigurations by ensuring misconfigured EtherChannels are automatically disabled. Additionally, configure the switches to automatically bring up EtherChannels that were
disabled due to misconfigurations after 10 minutes.  
`Switch1(config)#spanning-tree etherchannel guard misconfig`  
`Switch1(config)#errdisable recovery cause channel-misconfig`  
`Switch1(config)#errdisable recovery interval 600`  
`Switch2(config)#spanning-tree etherchannel guard misconfig`  
`Switch2(config)#errdisable recovery cause channel-misconfig`  
`Switch2(config)#errdisable recovery interval 600`  
`Switch3(config)#spanning-tree etherchannel guard misconfig`  
`Switch3(config)#errdisable recovery cause channel-misconfig`  
`Switch3(config)#errdisable recovery interval 600`  
`Switch4(config)#spanning-tree etherchannel guard misconfig`  
`Switch4(config)#errdisable recovery cause channel-misconfig`  
`Switch4(config)#errdisable recovery interval 600`  
Verfication:  
{{< bokya src="img/switching/eth1.jpg" >}}  
{{< bokya src="img/switching/eth2.jpg" >}}  
  
**Task 4**  
Configure VLANs 100, 200, 300, 400, 500, 600, 700, and 800 all switches. Use the default VLAN names or specify your own names, if so desired.  
  
**Task 5**  
Selecting your own name and revision number, configure Multiple STP as follows:  
**a**. VLANs 100 and 200 will be mapped to MST Instance # 1: Switch1 should be root  
`Switch1(config)#spanning-tree mst configuration`  
`Switch1(config-mst)#name LAB`  
`Switch1(config-mst)#revision 0`  
`Switch1(config-mst)#instance 1 vlan 100,200`  
`Switch1(config-mst)#instance 2 vlan 300,400`  
`Switch1(config-mst)#instance 3 vlan 500,600`  
`Switch1(config-mst)#instance 4 vlan 700,800`  
`Switch1(config-mst)#show current`  
{{< bokya src="img/switching/mstshow1.jpg" >}}  
  
**b**. VLANs 300 and 400 will be mapped to MST Instance # 2: Switch2 should be root  
`Switch2(config)#spanning-tree mst configuration`  
`Switch2(config-mst)#name LAB`  
`Switch2(config-mst)#revision 0`  
`Switch2(config-mst)#instance 1 vlan 100,200`  
`Switch2(config-mst)#instance 2 vlan 300,400`  
`Switch2(config-mst)#instance 3 vlan 500,600`  
`Switch2(config-mst)#instance 4 vlan 700,800`  
`Switch2(config-mst)#exit`  
`Switch2(config)#spanning-tree mst 2 priority 0`  
`Switch2(config)#spanning-tree mode mst`  
  
**c**. VLANs 500 and 600 will be mapped to MST Instance # 3: Switch3 should be root  
`Switch3(config)#spanning-tree mst configuration`  
`Switch3(config-mst)#name LAB`  
`Switch3(config-mst)#revision 0`  
`Switch3(config-mst)#instance 1 vlan 100,200`  
`Switch3(config-mst)#instance 2 vlan 300,400`  
`Switch3(config-mst)#instance 3 vlan 500,600`  
`Switch3(config-mst)#instance 4 vlan 700,800`  
`Switch3(config-mst)#exit`  
`Switch3(config)#spanning-tree mst 3 priority 0`  
`Switch3(config)#spanning-tree mode mst`  
  
**d**.VLANs 700 and 800 will be mapped to MST Instance # 4: Switch4 should be root   
`Switch4(config)#spanning-tree mst configuration`  
`Switch4(config-mst)#name LAB`  
`Switch4(config-mst)#revision 0`  
`Switch4(config-mst)#instance 1 vlan 100,200`  
`Switch4(config-mst)#instance 2 vlan 300,400`  
`Switch4(config-mst)#instance 3 vlan 500,600`  
`Switch4(config-mst)#instance 4 vlan 700,800`  
`Switch4(config-mst)#exit`  
`Switch4(config)#spanning-tree mst 4 priority 0`  
`Switch4(config)#spanning-tree mode mst`  
   
This is how we verify a spanning-tree running via MST. I will be checking it from Switch4:  
To check the MST instance use the command `show spanning-tree mst {instance Number}:  
{{< bokya src="img/switching/mst1.jpg" >}}  
the output above shows that the MST instance 1 is the root for vlans 100 and 200, more to that I can also see the **Root Address** with a priority of 1 this root address is the bia of interface PO1 in switch1 because Switch1 is the root for MST instance 1 for vlans 100 and 200. We can also see the Root port in order to reach the root switch for this MST instance.  
*Switch Interface Po1 BIA:*  
{{< bokya src="img/switching/switch1bia2.jpg" >}}  

Repeat the steps above to other switch and check each MST Instance.  
