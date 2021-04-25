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
  
**Task 1**  
We will disable all VTP modes on all switches. All switches should support the configuration,
modification and deletion of VLANs.  
* Commandd to put in **all switches**:  
`configure terminal`  
`vtp mode transparent`  
  

**Task 2**  
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
