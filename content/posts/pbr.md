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

Policy based routing allows us to configure routing rules that are different from the normal IP routing. One of the most used method in my experience is based on the IP source address rather that the destination.  

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
ip address 10.20.20.2 255.255.255.0  
ip policy route-map pbr  
exit  
!  

The configuration above defines a special routing policy for a group of IP adress or possibly users defined by the route-map "pbr". This name depends on you but the best practice is to name your route-map based on what its used for. The syntax above applies the route-map to all packets received on the interface Gi0.

The number 10 and 20 on each end of the route-map commands are used to specify the order that the router would apply this when its making routing decisions (basically it will be read from top to bottom and it always has a implicit deny at the very end much like ACL) packets that don't match either of the route-map policies will be unaffected and will instead use the normal routing path based on the global routing table. It's also a good practice in using spaced number like the example above  as its makes our life more easier if we need to insert a new line or policy in between the route-map clauses (ie inserting another policy route-map pbr 11).  

The first route-map policy sequence takes action based on the match command.  
* `route-map pbr 10`  
* `match ip address 1`  
* `set ip next-hop 10.20.20.1`  
* `exit`  
The above compares the IP header with what is defined in the access-list number 1, this means it will match any source in the range of 10.10.10.0/24. The acl used is a standard ACL(more about ACL in my future posts) which is used to match the source address. If the match is successful the route-map will apply the next line of syntax which is the "**set**" command. The set command in the route-map will bypass the routing table this means that in order for this to happen the "**next-hop**" address must be a directly connected network. If the next-hop is not a directly connected network the router will not be able to route the packet.

The second route-map policy works in a similar way, but this set a different next-hop value:  
* `route-map pbr 20`  
* `match ip address 2`  
* `set interface Gi1`  
* `exit`  
Any packets matching the rule above will be forwarded directly out the interface Gi1. This means the destination device must be connected to this segment.

We can also specify several different possible interface:  
`set interface gi1 gi2`  
This allows us to define what to do if there no way to route to of this next-hop interfaces. If the first address in the first interface is down the router will use the second, this is possible because the router will consult its routing table to decide if the specified interface is available or not.

A key note to remember because policy based routing overrides the normal routing decision it can result to some network issues. If for example the next hop 10.20.20.1 suddenly failed but the subnet did not drop out the routing table then there is no dynamic routing protocol that can find a new path. Sometimes you can still able to ping a for example a server on the other destination even though the next hop specified is down, this is because ICMP originating from the router are not affected by the policy based routing policy. So you can ping a server or application but users are complaining. Talking about headache right? To avoid this don't used PBR without proper planning or just don't use PBR in case no workaround can be applied.
