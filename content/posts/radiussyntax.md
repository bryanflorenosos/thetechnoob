+++
author = "Bryan Florenosos"
title = "Radius Syntax in Cisco Devices"
date = "2021-04-15"
description = "Radius Template for Cisco Devices"
tags = [
    "network",
    "routing",
    "switching",
]
+++

interface Loopback0  
 description Management Loopback  
 ip address {$MGMT_IP} 255.255.255.255  
!  
aaa new-model  
!  
aaa group server radius AAA_AUTH_RADIUS  
server name AAA_AUTH_RADIUS  
ip radius source-interface loopback0  
deadtime 1  
!  
aaa authentication login default group AAA_AUTH_RADIUS local  
aaa authorization exec default group radius group AAA_AUTH_RADIUS local   
!  
radius-server dead-criteria time 5  
radius-server host {Radius-Server IP Adrress} auth-port 1812 acct-port 1813 key 7 password123  
radius-server retry method reorder  
radius-server transaction max-tries 3  
radius-server retransmit 2  
radius-server timeout 2  
radius-server key 7 password123  
