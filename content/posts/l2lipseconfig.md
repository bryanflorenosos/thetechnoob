+++
author = "Bryan Florenosos"
title = "Basic IPSEC L2L VPN Configuration"
date = "2021-04-24"
description = "How to configure basic L2L ipsec vpn"
tags = [
    "network",
    "security",
]
+++

In this example I will be configuring a Policy based site to site vpn tunnel. This types of vpn are configured by specifying the Interesting traffic [Traffic that needs to be encrypted] by using  a Policy [ACL]. If the traffic match the Policy [ACL] it is encapsulated within the **ESP header**. The **Outer header** will have the Public IP Addresses of the Tunnel Endpoints. Do note that everytime a new network is added, the ACL needs to be modified on both ends.

**Network Topology**
{{< bokya src="img/l2ltopology.jpg" >}}    



