+++
author = "Bryan Florenosos"
title = "Zone Based Firewall Configuration"
date = "2021-04-14"
description = "ZBF Configuration"
tags = [
    "security",
]
+++

##### Zone Based Firewall (ZBF)

Allows a router to act as a stateful firewall, where we define and allocate security zones.
Just like a typical firewall interzone traffic is denied by default, unless specified by a rule/policy and Intrazone traffic is allowed by default.

{{< figure src="/images/zbf.jpg" title="Topology" >}}

###### Create the Zones

* `zone security CORPORATE`
* `zone security INTERNET`
* `zone security GUEST`

###### Assign the Interfaces to Zones

`interface Tunnel1`   
`zone-member security CORPORATE`  

`interface Gi1`  
 `zone-member security INTERNET`  

`interface Vlan50`  
 `description GUEST-WIFI`  
`zone-member security GUEST`  

*Configure Zone-Pair Policies to define the traffic that should be allowed thru the Zone-based Firewall. Zone-pair policies are between a pair of zones. It is directional. Each initiating Zone require a separate policy.*

###### Classify the traffic using a special Class-map for Zone-based Firewalls.

*Basically an **object-group** will act as a container in my example they are a collection of two IP addresses but this can be network address, ports etc., we will call this inside our Access-list configuration.*

`object-group network googleDNS`  
 `host 8.8.8.8`  
 `host 8.8.4.4`  
`ip access-list extended public-IP`  
 `permit tcp any object-group googleDNS eq 443`  
 `permit icmp any object-group googleDNS`  

*The **inspect** command will inspect traffic in layer 4 and 7*  
`class-map type inspect match-any public-IP`  
 `match access-group name public-IP`  




