+++
author = "Bryan Florenosos"
title = "DMVPN configuration using Front Door VRF"
date = "2021-04-14"
description = "How to configure dmvpn"
tags = [
    "network",
    "security",
]
+++

#### This article will show how to configure dmvpn HUB and spoke topology.

###### What is FVRF ?

In my experience FVRF or front door vrf is where you place your public facing physical interface or an interface facing your L3VPN inside a VRF. I mainly encounter this setup in DMVPN environments. I will be sharing in this article on how we setup a hub and spoke dmvpn using fvrf.  

Basically a vrf is a separate routing table from your global routing table. The tunnel interface will not be put inside a vrf but instead it should only be aware on where to forward the traffic correctly (vrf aware vpn).  
{{< bokya src="img/fvrf.jpg" alt="Bad Lighthouse Performance" >}}

###### Configuration of the Cloud Router  
The cloud router simulates the Internet the configuration only involves putting in the Public IP address nothing fancy.

###### _HUB_ config:

**Configure and define the VRF and assign the physical interface that faces the ISP and creating a default route to the internet**  
`vrf definition wan`  
`!`  
 `address-family ipv4`  
 `exit-address-family`  
`!`  
`interface GigabitEthernet1`  
 `vrf forwarding wan`  
 `ip address 200.138.55.2 255.255.255.252`  
 `negotiation auto`  
`!`  
`ip route vrf wan 0.0.0.0 0.0.0.0 200.138.55.1`  

###### Configure the interconnection from the WAN Router going to the Coreswitch (allocate a sub interface)

`interface GigabitEthernet2`  
 `description To Coreswitch g0/0`  
 `no ip address`
 `negotiation auto`  
!  
`interface GigabitEthernet2.200` 
 `description Interconnect` 
 `encapsulation dot1Q 200`  
 `ip address 10.10.11.1 255.255.255.252`  
!  

###### Tunnel configuration

`interface Tunnel100`  
 `ip address 10.0.0.1 255.255.255.0`  
 `no ip redirects`  
 `ip mtu 1400`  
 `ip nhrp map multicast dynamic`   
 `ip nhrp network-id 100`  
 `ip nhrp redirect`  
 `ip tcp adjust-mss 1360`  
 `load-interval 30`  
 `if-state nhrp`  
 `cdp enable`  
 `tunnel source GigabitEthernet1`  
 `tunnel mode gre multipoint`  
 `tunnel key 100`  
 `tunnel vrf wan`  
!  

###### Routing configuration. I had used eigrp named mode, by the way eigrp is the most common protocol used for dmvpn networks I will post more about this later in my future technical notes. 

`router eigrp bangan`  
 !  
 `address-family ipv4 unicast autonomous-system 123`  
  !  
  `af-interface default`  
  `passive-interface`  
  `exit-af-interface`  
  !  
  `af-interface Tunnel100`  
   `no passive-interface`  
   `no split-horizon`  
   `summary-address 10.10.8.0 255.255.252.0`  
  `exit-af-interface`
  !  
  `af-interface GigabitEthernet2`  
   `no passive-interface`  
  `exit-af-interface`  
  !
  `af-interface GigabitEthernet2.200`  
   `no passive-interface`  
  `exit-af-interface`  
  !  
  `topology base`  
  `exit-af-topology`  
  `network 10.0.0.1 0.0.0.0`  
  `network 10.10.11.1 0.0.0.0`  
  `eigrp router-id 10.0.0.1`  
 `exit-address-family`  
!  

###### HUB Core switch configuration

`interface GigabitEthernet0/0`  
 `description to WAN Router`
 `switchport trunk encapsulation dot1q`  
 `switchport mode trunk`  
 `media-type rj45`  
 `negotiation auto`  
!  
`vlan 10`  
`vlan 12`  
`vlan 13`  
`vlan 200`  

 `interface Vlan1`  
 `no ip address`  
 `shutdown`  
!  
`interface Vlan10`  
 `description Corporate LAN`  
 `ip address 10.10.8.1 255.255.255.0`
 `no shut`  
!  
`interface Vlan12`  
 `description Engineer Department`  
 `ip address 10.10.9.1 255.255.255.0` 
 `no shut` 
!  
`interface Vlan13`  
 `description Corporate SERVERS`  
 `ip address 10.10.10.1 255.255.255.0`
 `no shut`  
!  
`interface Vlan200`  
 `description Interconnection to Router`  
  `ip address 10.10.11.2 255.255.255.252`
  `no shut`     
!  

`router eigrp bangan` 
 !  
 `address-family ipv4 unicast autonomous-system 123`  
  !  
  `af-interface default`  
   `passive-interface`  
  `exit-af-interface`  
  !  
  `af-interface Vlan200`  
  `no passive-interface`  
  `exit-af-interface`  
  !  
  `af-interface Vlan13`  
   `no passive-interface`  
  `exit-af-interface`  
  !  
  `topology base`  
  `exit-af-topology`  
  `network 10.0.0.0`  
  `eigrp router-id 10.10.11.2`  
 `exit-address-family`  
!  

###### _Spoke1_ Configuration

`vrf definition wan`  
 !  
 `address-family ipv4`  
 `exit-address-family`  
!  
`interface GigabitEthernet1`  
 `description Public Facing Internet`  
 `vrf forwarding wan`  
 `ip address 198.162.80.2 255.255.255.252`  
 `negotiation auto`  
!  
`ip route vrf wan 0.0.0.0 0.0.0.0 198.162.80.1`  
!  
`interface GigabitEthernet2`  
 `no ip address`  
 `negotiation auto`  
!  
`interface GigabitEthernet2.100`  
 `description Core_SW G0/0`    
 `encapsulation dot1Q 100`  
 `ip address 10.10.15.1 255.255.255.252`  
!  

`router eigrp bangan`  
 !  
 `address-family ipv4 unicast autonomous-system 123`  
  !  
  `af-interface default`  
  `passive-interface`  
  `exit-af-interface`  
  !  
  `af-interface Tunnel100`  
   `no passive-interface`  
  `exit-af-interface`  
  !  
  `af-interface GigabitEthernet2`  
   `no passive-interface`  
  `exit-af-interface`  
  !  
  `af-interface GigabitEthernet2.100`  
   `no passive-interface`  
  `exit-af-interface`  
  !  
  `topology base`  
  `exit-af-topology`  
  `network 10.0.0.2 0.0.0.0`  
  `network 10.10.15.1 0.0.0.0`  
  `eigrp router-id 10.0.0.2`  
 `exit-address-family`  
!  
###### SPOKE Coreswitch Configuration

`interface GigabitEthernet0/0`
 `description to WAN Router Gi2`  
 `switchport trunk encapsulation dot1q`  
 `switchport mode trunk`  
 `media-type rj45`  
 `negotiation auto`  
!  

`vlan 20`  
`vlan 21`  
`vlan 22`  

`interface Vlan1`  
 `no ip address`  
 `shutdown`  
!  
`interface Vlan20`  
 `ip address 10.10.12.1 255.255.255.0`  
!  
`interface Vlan21`  
 `ip address 10.10.13.1 255.255.255.0`  
!  
`interface Vlan22`  
 `ip address 10.10.14.1 255.255.255.0`  
!  
`interface Vlan100`  
 `description Interconnect to Router`  
 `ip address 10.10.15.2 255.255.255.252`  

 `router eigrp bangan`  
 !  
 `address-family ipv4 unicast autonomous-system 123`  
  !  
  `af-interface default`  
   `passive-interface`  
  `exit-af-interface`  
  !  
  `af-interface Vlan100`  
   `no passive-interface`  
  `exit-af-interface`  
  !
  `topology base`    
  `exit-af-topology`  
  `network 10.10.0.0 0.0.255.255`  
  `eigrp router-id 10.10.15.2`  
 `exit-address-family`  
!  

###### _SPOKE3_ Configuration:

