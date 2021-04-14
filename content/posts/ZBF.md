+++
author = "Bryan Florenosos"
title = "Zone Based Firewall"
date = "2021-04-14"
description = "ZBF Syntax Guide"
tags = [
    "markdown",
    "css",
    "html",
]
categories = [
    "ZBF",
    "security"
    "syntax",
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]
+++

### What is Zone Based Firewall ?

In a much simpler explanation ZBF  allows you to make a router act as a firewall. You will allocate and assigned security zones to different interfaces.

Actually in my opinion you can use this approach if your client or customer has a strict budget requirements and still wants an extra security.


### ZBF configuration Syntax.

#### Create the Zones

* zone security CORPORATE
* zone security INTERNET
* zone security GUEST

#### Assign the Interfaces to Zones

{{< typography font="monoton" size="36px" weight="bold" >}}
interface Tunnel1  
zone-member security CORPORATE
{{< /typography >%}}

interface Gi1  
zone-member security INTERNET
 
interface Vlan50  
description GUEST-WIFI  
zone-member security GUEST
