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
**R1** = right router  *public ip is 129.30.19.2*  
**R2** = left router   *public ip is 139.30.20.2*  

## R1
**1.** Phase I - ISAKMP Parameters  
  
`crypto isakmp policy 5`  
 `authentication pre-share`  
 `encryption aes 256`  
 `hash sha256`  
 `group 14`  
!  
`crypto isakmp key cisco123 address 139.30.20.2`  
  
**2.** Phase II - ESP Parameters  
`crypto ipsec transform-set TSET esp-aes 256 esp-sha-hmac`  
  
**3.** Interesting Traffic - Crypto ACL  
`access-list 101 permit ip 10.10.1.0 0.0.0.255 172.168.1.0 0.0.0.255`  
`access-list 101 permit ip 10.10.1.0 0.0.0.255 172.168.2.0 0.0.0.255`  
`access-list 101 permit ip 10.10.2.0 0.0.0.255 172.168.1.0 0.0.0.255`  
`access-list 101 permit ip 10.10.2.0 0.0.0.255 172.168.2.0 0.0.0.255`  
  
**4.** Link the above parameters to each other. This is done by using a Crypto Map Component  
`crypto map CMAP 10 ipsec-isakmp`  
 `match address 101`  
 `set peer 139.30.20.2`  
 `set transform-set TSET`  
   
**5.** Apply the Crypto Map on the outgoing Interface  
`Interface Gi0/0`   
 `crypto map CMAP`   

## R2
**1.** Phase I - ISAKMP Parameters  
`crypto isakmp policy 5`  
 `authentication pre-share`  
 `encryption aes 256`  
 `hash sha256`  
 `group 14`  
!  
`crypto isakmp key cisco123 address 129.30.19.2`  
  
**2.** Phase II - ESP Parameters  
`crypto ipsec transform-set TSET esp-aes 256 esp-sha-hmac`  
  
**3.** Interesting Traffic - Crypto ACL  
`access-list 101 permit ip 172.168.1.0 0.0.0.255 10.10.1.0 0.0.0.255`  
`access-list 101 permit ip 172.168.1.0 0.0.0.255 10.10.2.0 0.0.0.255`  
`access-list 101 permit ip 172.168.2.0 0.0.0.255 10.10.1.0 0.0.0.255`  
`access-list 101 permit ip 172.168.2.0 0.0.0.255 10.10.2.0 0.0.0.255`  
  
**4.** Link the above parameters to each other. This is done by using a Crypto Map Component  
`crypto map CMAP 10 ipsec-isakmp`  
 `match address 101`  
 `set peer 129.30.19.2`  
 `set transform-set TSET`  
   
**5.** Apply the Crypto Map on the outgoing Interface  
`Interface Gi0/0`   
 `crypto map CMAP`   
  
Verification Commands:  
`Phase I - Show crypto isakmp sa`  
`Phase II - Show crypto ipsec sa`  
  
Sample output from R1:  
{{< bokya src="img/l2lR1_1.jpg" >}}  

Data Packets are now traversing the link securely via IPSec as shown by below out, they are being encrypted and decrypted.  
{{< bokya src="img/l2lR1_2.jpg" >}}  

For further Notes the show crypto isamp sa command allows us to check the status of our negotiations  
**The following four modes are found in IKE main mode:**  
a. **MM_NO_STATE*** – ISAKMP SA process has started but has not continued to form (typically due to a connectivity issue with the peer)
