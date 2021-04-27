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

Task 1
Disable VTP on all switches. All switches should support the configuration,
modification and deletion of VLANs.


Task 2
Configure EtherChannel trunks on the switches illustrated in the topology. Only switches
DLS1 and DLS2 should actively initiate channel establishment; switches ALS1 and
ALS2 should be configured as passive links that will should not actively attempt to
establish an EtherChannel. Please note that depending upon your switch model, you may
have to choose between ISL and dot1q encapsulation manually (as we do in the
solution). The EtherChannels should be configured as follows:
The two ports between DLS1 and DLS2 belong to channel group 1 and use mode
on
The two ports between DLS1 and ALS1 belong to channel group 2 and use
LACP
The two ports between DLS1 and ALS2 belong to channel group 3 and use PAgP
The two ports between DLS2 and ALS1 belong to channel group 4 and use
LACP
The two ports between DLS2 and ALS2 belong to channel group 5 and use PAgP

Task 3
Following the EtherChannel configuration, protect the switched network from any future
misconfigurations by ensuring misconfigured EtherChannels are automatically disabled.
Additionally, configure the switches to automatically bring up EtherChannels that were
disabled due to misconfigurations after 10 minutes.
Task 4
Configure VLANs 100, 200, 300, 400, 500, 600, 700, and 800 all switches. Use the
default VLAN names or specify your own names, if so desired.
Task 5
Selecting your own name and revision number, configure Multiple STP as follows:
VLANs 100 and 200 will be mapped to MST Instance # 1: Switch DLS1 should
be root
VLANs 300 and 400 will be mapped to MST Instance # 2: Switch DLS2 should
be root
VLANs 500 and 600 will be mapped to MST Instance # 3: Switch ALS1 should
be root
VLANs 700 and 800 will be mapped to MST Instance # 4: Switch ALS2 should
be root
Task 6
Configure Spanning Tree so that interface cost values appear to the 802.1t (long)
version