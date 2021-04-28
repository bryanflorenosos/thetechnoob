+++
author = "Bryan Florenosos"
title = "Policy Based Routing based on Source Address"
date = "2021-04-16"
description = "How to configure PBR"
tags = [
    "network",
    "Routing",
]
+++

## Policy Based Routing

Policy based routing allows us to configure routing rules that are different from the normal IP routing. One of the most used method based on my experience is based on the IP source address rather that the destination.  

**Here is the Syntax for PBR based on source address:**  
configure terminal  
access-list 1 permit 10.10.10.0 0.0.0.255  
access-list 2 permit 10.10.11.0 0.0.0.255  
!  
route-map pbr 10  
match ip address 1  
set ip next-hop 10.20.20.1  
exit  
route-map pbr 20  
match ip address 2  
set interface Gi1    
exit  
!  
interface Gi0  
ip address 10.20.20.2 255.255.255.252  
ip policy route-map pbr  
exit  
!  

The configuration above defines a special routing policy for a group of IP adress or possibly users defined by the route-map "pbr". This name depends on you but the best practice is to name your route-map based on what its used for. The syntax above applies the route-map to all packets received on the interface Gi0.

The number 10 and 20 on each end of the route-map commands are used to specify the order that the router would apply this when its making routing decisions (basically it will be read from top to bottom and it always has a implicit deny at the very end much like ACL) packets that don't match either of the route-map policies will be unaffected and will instead use the normal routing path based on the global routing table. It's also a good practice in using spaced number like the example above  as its makes our life more easier if we need to insert a new line or policy in between the route-map clauses (ie inserting another policy route-map pbr 11).  
* `route-map pbr 10`  
* `match ip address 1`  
* `set ip next-hop 10.20.20.1`  
* `exit`  
