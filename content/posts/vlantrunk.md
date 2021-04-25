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
> **a.** This task requires identifying designated ports and configuring the root guard feature on them. The **root guard** feature prevents a designated port from becoming a root port. If a port on which the root guard feature receives a superior BPDU, it moves the port into a root-inconsistent state, thus maintaining the current root bridge status.  
> **b** First do a verification for each vlan as such: `show spanning-tree vlan 10`  
{{< bokya src="img/switching/vlan10_1.jpg" >}}  
**From Switch2:**  
{{< bokya src="img/switching/vlan10_switch2.jpg" >}}  
**From Switch3:**  
{{< bokya src="img/switching/vlan10_switch3.jpg" >}}  
**From Switch4:**  
{{< bokya src="img/switching/vlan10_switch4.jpg" >}}  
  
Following this, enable the root guard feature on all designated ports using the spanningtree
guard root interface configuration command. Referencing the list above, root guard
would be enabled on the following interfaces or ports:  
**a.** Switc1: gi0/0, gi0/1, gi0/2  
**b.** Switch2: gi0/1, g0/2    
**c.** Switch3: gi0/0  
**d.** Switch4: None - because this switch has no designated ports  
This task is completed by enabling root guard on the ports above as follows:  
**Switch1:**  
`int range g0/0 - 2`  
`spanning-tree guard root`  
**Switch2:**  
`int range g0/1 - 2`  
`spanning-tree guard root`  
**Switch3:**  
`int  g0/0`  
`spanning-tree guard root`  
  
Test your solution by setting the priority of a VLAN to 0 or 4096 and then verifying the port state on the adjacent segment designated bridge. For example, if you changed the priority of VLAN 10 on Switch4 to 0, all peer ports enabled for root guard with which this switch connects will be placed into a root inconsistent state as follows:  
**Switch4**  
`spanning-tree vlan 10 priority 0`  
Output from **Switch1:**  
*Apr 25 05:14:46.380: %SPANTREE-2-ROOTGUARD_CONFIG_CHANGE: Root guard enabled on port GigabitEthernet0/0.  
*Apr 25 05:14:46.398: %SPANTREE-2-ROOTGUARD_CONFIG_CHANGE: Root guard enabled on port GigabitEthernet0/1.  
*Apr 25 05:14:46.414: %SPANTREE-2-ROOTGUARD_CONFIG_CHANGE: Root guard enabled on port GigabitEthernet0/2.  
Output from **Switch2:**  
*Apr 25 05:14:52.826: %SPANTREE-2-ROOTGUARD_CONFIG_CHANGE: Root guard enabled on port GigabitEthernet0/1.  
*Apr 25 05:14:52.842: %SPANTREE-2-ROOTGUARD_CONFIG_CHANGE: Root guard enabled on port GigabitEthernet0/2.  
*Apr 25 05:15:13.466: %SPANTREE-2-ROOTGUARD_BLOCK: Root guard blocking port GigabitEthernet0/1 on VLAN0010.  
Output from **Switch3:**  
*Apr 25 05:14:59.824: %SPANTREE-2-ROOTGUARD_CONFIG_CHANGE: Root guard enabled on port GigabitEthernet0/0.  
*Apr 25 05:15:11.984: %SPANTREE-2-ROOTGUARD_BLOCK: Root guard blocking port GigabitEthernet0/0 on VLAN0010.  

Aside from the logged message, you can also use the show spanning-tree inconsistentports and the show spanning-tree interface commands to view root inconsistent ports:  
{{< bokya src="img/switching/switch1inconsistent.jpg" >}}  
  
## Task 5   
Configure the trunk links on  Switch2 so that they will be disabled automatically in the event that they do not receive BPDUs. Test and verify your configuration by disabling default BPDU transmission from one of the upstream switches.  
*This task requires the configuration of Loop Guard on switch2. The Loop Guard detects root ports and blocked ports, and ensures they continue to receive BPDUs. When enabled, should one of these ports stop receiving BPDUs, it is moved into a loop inconsistent state.*  
**Switch 2**  
`configure terminal`  
`spanning-tree loopguard default`  
**Switch 1**  
`int g0/0`  
`spanning-tree bpdufilter enable`  
**Switch2 logs:**  
Switch2(config)#  
*Apr 25 05:17:59.552: %SPANTREE-2-ROOTGUARD_BLOCK: Root guard blocking port GigabitEthernet0/2 on VLAN0001.  
*Apr 25 05:18:05.552: %SPANTREE-2-ROOTGUARD_UNBLOCK: Root guard unblocking port GigabitEthernet0/2 on VLAN0001.  
*Apr 25 05:18:18.605: %SPANTREE-2-ROOTGUARD_BLOCK: Root guard blocking port GigabitEthernet0/1 on VLAN0001.  
*Apr 25 05:18:19.608: %SPANTREE-2-ROOTGUARD_BLOCK: Root guard blocking port GigabitEthernet0/2 on VLAN0010.  

## Task 6  
Ports Gi0/3 and g1/0 - 3 on switches Switch3 and Switch4 will be connected to hosts in the future. These hosts will reside in VLAN 10. Using only **TWO** commands, perform the following activities on these ports:  
**a.** Assign all of these individual ports to VLAN 10  
**b.** Configure the ports as static access port  
**c.** Enable PortFast for all the ports  
**d.** Disable bundling, i.e. EtherChannel, for these ports  
**Switch3 and Switch4 Configuration:**  
`int range g0/3, g1/0 - 3`  
`switchport access vlan 10`  
`switchport host`  
*The* `switchport host` *command is an inbuilt Cisco IOS macro that performs three actions under the specified port(s):*  
**a** It configures the switchport for access mode  
**b** It enables portfast  
**c** It disables Etherchannel capabilities for the port  
{{< bokya src="img/switching/swhost.jpg" >}}  
Another verification command that I always used is the `show interface (number) switchport` it will show the output as below. The highlighted are the information that I always checked first.
{{< bokya src="img/switching/swaccess.jpg" >}}  




