+++
author = "Bryan Florenosos"
title = "Zone Based Configuration Guide"
date = "2021-04-14"
description = "ZBF Syntax"
tags = [
    "ZBF",
    "Zone Based Firewall",
]
categories = [
    "syntax",
    "configuration",
]

+++

### What is Zone Based Firewall ?

In a much simpler explanation ZBF  allows you to make a router act as a firewall. You will allocate and assigned security zones to different interfaces.

Actually in my opinion you can use this approach if your client or customer has a strict budget requirements and still wants an extra security.


### ZBF configuration Syntax.

## Create the Zones

* zone security CORPORATE
* zone security INTERNET
* zone security GUEST

## Assign the Interfaces to Zones

interface Tunnel1  
zone-member security CORPORATE

interface Gi1  
zone-member security INTERNET
 
interface Vlan50  
description GUEST-WIFI  
zone-member security GUEST
