+++
author = "Bryan Florenosos"
title = "Troubleshooting Cisco IOS Commands"
date = "2021-04-14"
description = "Commands I used for network troubleshooting"
tags = [
    "network",
]
+++

1. Basic Troubleshooting Commands  
`Ping`  
`Traceroute`  
`Telnet`  
`Show interfaces (show interfaces GigabitEthernet 3/6)`  
`Show ip interface`  
`Show ip route`  
`Show running-config`  
`Show startup-config`  
`show ip sockets`  
`show conn`  
`show tcp brief`  

2. Archive Command  
show archive config differences  
show archive log config all  
show archive  

###### Basic Commands to Enable Telnet/SSH on Cisco Devices
a. **Telnet Access**  
no aaa new-model  
username test privilege 15 secret test  
line vty 0 15  
login local  
no password  
transport input telnet  

b. **SSH Access:**  
ip domain-name test.com  
crypto key generate rsa general-usage modulus 2048  
ip ssh time-out 60  
ip ssh version 2  
line vty 0 15  
transport input ssh  

###### Debug commands at Cisco ASA  
terminal monitor  
logging buffer-size 1048576  
logging buffered 7  
logging monitor 7  
`debug crypto condition peer 45.250.160.114`  
`debug crypto ipsec 255`  
`debug crypto ikev2 protocol 255`  
`debug crypto ikev2 platform 255`  

#####Set SSH/Telnet/Ping/Traceroute with a source ip or interface
ip ssh source-interface <interface to use>  
ip telnet source-interface <interface to use>  






