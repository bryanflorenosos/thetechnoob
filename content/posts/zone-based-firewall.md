+++
author = "Bryan Florenosos"
title = "Zone Based Firewall Guide"
date = "2021-04-14"
description = "Configuration and Syntax for ZBF"
tags = [
    "zbf",
    "security",
    "firewall",
]
categories = [
    "security",
]

+++

## What is a Zone Based Firewall ?

Basically a Zone Based Firewall configuration allows you to make a router act as a "**Stateful**" security appliance, where you define the security zones and allocate each zones to different interfaces.

#### Steps in configuring ZBF

* Configure and Define the Zone
  * zone security CORPORATE
  * zone security INTERNET
  * zone security GUEST

* Assign the Interfaces to Zones
  * interface Tunnel1  
    zone-member security CORPORATE

  * interface Gi1  
    zone-member security INTERNET

  * interface vlan 50  
    description GUEST-WIFI  
    zone-member security GUEST  

###### Configure Zone-Pair Policies to define the traffic that should be allowed thru the Zone-based Firewall. Zone-pair policies are between a pair of zones. It is directional. Each initiating Zone require a separate policy.

* A. Classify the traffic using a special Class-map for Zone-based Firewalls, integrate Access-list configuration
  * class-map type inspect match-any microsoftO365
   match access-group name microsoftO365


  


