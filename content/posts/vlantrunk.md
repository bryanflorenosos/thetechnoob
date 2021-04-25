+++
author = "Bryan Florenosos"
title = "Configure VLAN, STP, VTP, DTP"
date = "2021-04-14"
description = "Switching Fundamentals"
tags = [
    "network",
    "switching",
]
+++

This Lab will be a good guide on understanding the basics behind the configuration of VLAN's, VTP, DTP and STP on Cisco IOS Switches.  

### The Network Topology:  
{{< bokya src="img/switching/switchtopology1.jpg" >}}
  
## Task 1  
We will disable all VTP modes on all switches. All switches should support the configuration,
modification and deletion of VLANs.  
* Commandd to put in **all switches**:  
`configure terminal`  
`vtp mode transparent`  
  

## Task 2  
Configure trunking on the switches so that all ports are explicitly trunking and DTP
should never be used on any of these ports.  

**Switch1:**  
 `interface range g0/0 - 2`  
 `switchport trunk encapsulation dot1q`  
 `switchport mode trunk`  
 `switchport nonegotiate`  
**Switch2:**  
 `interface range g0/0 - 2`  
 `switchport trunk encapsulation dot1q`  
 `switchport mode trunk`  
 `switchport nonegotiate`  
 **Switch3:**  
 `interface range g0/0 - 2`  
 `switchport trunk encapsulation dot1q`  
 `switchport mode trunk`  
 `switchport nonegotiate`  
 **Switch4:**  
 `interface range g0/0 - 2`  
 `switchport trunk encapsulation dot1q`  
 `switchport mode trunk`  
 `switchport nonegotiate`  
**Verification Command:**  
`show interface trunk`  
  
## Task 3   
Configure VLANs 10 and 20 on all switches and use the default IOS VLAN names. Explicitly configure Switch1 with a priority of 8192 so it is elected root for both of these VLANs. Additionally, also explicitly configure switch Switch2 so it is elected as a backup root for both VLANs.  
**Switch1:**  
`vlan 10`  
`vlan 20`  
`exit`  
`spanning-tree vlan 10 prior 8192`  
`spanning-tree vlan 20 prior 8192`  
To verify: `show spanning-tree root`  
{{< bokya src="img/switching/switch1showspanningroot.jpg" >}}    
*the image above shows that the switch itself is the root the Root ID is the Burn in Address (bia) of the switch1 interface:*    
{{< bokya src="img/switching/switch1bia.jpg" >}}    
  
**Switch2:**  
`vlan 10`  
`vlan 20`  
`exit`  
`spanning-tree vlan 10 prior 16384`  
`spanning-tree vlan 20 prior 16384`  
To verify: `show spanning-tree root`  
{{< bokya src="img/switching/switch2showspanningroot.jpg" >}}  
*switch2 knows that the root switch is Switch1 indicated by the root id which shows the bia address of switch1*  
  
**Switch3:**  
`vlan 10`  
`vlan 20`  
`exit`  
*the same output from show spanning-tree root will be seen from this switch*  
**Switch4:**  
`vlan 10`  
`vlan 20`  
`exit`  
*the same output from show spanning-tree root will be seen from this switch*  
  
## Task 4  
Configure Spanning Tree so that it will remain root bridge in the event that another switch is inadvertently misconfigured with a lower priority for VLANs 10 and 20. Test and verify your configuration by specifying a priority of 0 for VLAN 10 on any of these ports.  
> **a.** This task requires identifying designated ports and configuring the root guard feature on them. The root guard feature prevents a designated port from becoming a root port. If a port on which the root guard feature receives a superior BPDU, it moves the port into a root-inconsistent state, thus maintaining the current root bridge status.  
> **b** First do a verification for each vlan as such: `show spanning-tree vlan 10`  


## Task 5   
Configure the trunk links on switch Switch2 so that they will be disabled automatically in the event that they do not receive BPDUs. Test and verify your configuration by disabling default BPDU transmission from one of the upstream switches.  

## Task 6  
Ports Gi0/3 through Gi0/7 on switches Switch3 and Switch4 will be connected to hosts in the future. These hosts will reside in VLAN 10. Using only **TWO** commands, perform the following activities on these ports:  
**a.** Assign all of these individual ports to VLAN 10  
**b.** Configure the ports as static access port  
**c.** Enable PortFast for all the ports  
**d.** Disable bundling, i.e. EtherChannel, for these ports  
