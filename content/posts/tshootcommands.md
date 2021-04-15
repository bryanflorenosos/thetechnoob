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

###### > Debug commands at Cisco ASA  
terminal monitor  
logging buffer-size 1048576  
logging buffered 7  
logging monitor 7  
`debug crypto condition peer 45.250.160.114`  
`debug crypto ipsec 255`  
`debug crypto ikev2 protocol 255`  
`debug crypto ikev2 platform 255`  

##### > Set SSH/Telnet/www with a source ip or interface  
ip ssh source-interface (interface to use)  
ip telnet source-interface (interface to use)  
telnet 104.68.10.120 www /source-interface GigabitEthernet1  

###### > Display Cisco IOS Device Opened Ports  
R#`show control-plane host open-ports`
{{< figure src="/images/open-ports.jpg" >}}  

* Solution to close the listening to port 23 (telnet)  
R#conf t  
Enter configuration commands, one per line.  End with CNTL/Z.  
R(config)#class-map type port-filter match-any TCP_23  
R(config-cmap)#match port tcp 23  
R(config)#policy-map type port-filter FILTER_TCP_23  
R(config-pmap)#class TCP_23  
R(config-pmap-c)#drop   
R(config-pmap-c)#log  
R(config)#control-plane host  
R(config-cp-host)#service-policy type port-filter input FILTER_TCP_23  

###### > IOS Password Recovery Procedures
1. Shut down the router then Power on the router  
2. Press Break on the terminal keyboard within 60 seconds of power up in order to put the router into Rommon. (In some Keyboards, Pause key is used to enter into Rommon mode. You may not need Fn+Pause, or CTRL+ Break)
3. Once the Rommon1> prompt appears, enter this command: **`confreg 0x2142`**
4. Then type reset to reboot Cisco device.
5. When you are prompted to enter the initial configuration, type No, and press Enter.
6. At the *Router>* prompt, type **`enable`**
7. At the *Router#* prompt, enter the **`configure memory`** command, and press Enter in order to copy the startup configuration to the running configuration.
8. Use the **`config t`** command in order to enter global configuration mode.
9. Use this command in order to create a new user name and password:  
    * `router(config)#username test privilege 15 password test`  
10. Use this command in order to change the boot statement: 
    * `config-register 0x2102`  
11. Use this command in order to save the configuration: 
    * `write memory`

###### > Create Read only Account

**A**  
    `username local1 secret Cisco1234`  
    `username local1 privilege 15 autocommand show running`  
    `aaa new-model`  
    `aaa authentication login default local`  
    `aaa authorization exec default local`   
    `aaa authorization console`

**B**  
    `aaa new-model`  
    `aaa authentication login default local`  
    `aaa authorization exec default local`  
    `aaa authorization console`  
    `username local2 privilege 7 password Cisco1234`  
    `privilege exec level 7 show config`  

###### Cisco Switch Lights Meaning
"SYSTEM(SYST) Light  
Overall status of the switch.  
**Off:** Switch is not powered on  
**Green:** Switch is working fine  
**Amber:** Switch is powered on but faulty  

###### REDUNDANT POWER SUPPLY(RPS) Light
Provides backup power to the switch if the main supply goes off.  

**Off:** No RPS available,  
**Green:** RPS is working fine  
**Blinking Green:** Providing backup to some other device  
**Amber:** RPS is faulty  
**Flashing Amber:** RPS is providing backup(primary power off)  

###### SPEED
Speed status of the switch ports.  

**Off:** Switch port is operating at 10Mbps  
**Green:** Switch port is operating at 100Mbps  
**Flashing green:** Switch port is operating at 1000Mbps  

###### STATUS
Status of the switch ports.  

**Off:** No device connected/port is administratively down.    
**Green:** Device is connected.  
**Blinking green:** Port is sending/receiving data.  
**Alternating green amber:** Fault in link/Frames experiencing error  
**Amber:** Port is blocked by Spanning Tree Protocol  








 

















