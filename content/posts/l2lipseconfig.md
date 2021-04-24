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
`show crypto session`  
  
Sample output from R1:  
{{< bokya src="img/l2lR1_1.jpg" >}}  
**The following mode is found in IKE Quick Mode, phase 2**  
* **QM_IDLE** – The ISAKMP SA is idle and authenticated  
  
For further Notes the show crypto isamp sa command allows us to check the status of our negotiations  
**The following four modes are found in IKE main mode:**  
a. **MM_NO_STATE*** – ISAKMP SA process has started but has not continued to form (typically due to a connectivity issue with the peer)  
b. **MM_SA_SETUP*** – Both peers agree on ISAKMP SA parameters and will move along the process.  
c. **MM_KEY_EXCH*** – Both peers exchange their DH keys and are generating their secret keys. (This state could also mean there is a mis-matched authentication type or PSK, if it does not proceed to the next step)    
d. **MM_KEY_AUTH*** – ISAKMP SA’s have been authenticated in main mode and will proceed to QM_IDLE immediately.  
  
**The following three modes are found in IKE aggressive mode:**  
a. **AG_NO_STATE** – ISAKMP SA process has started but has not continued to form (typically do to a connectivity issue with the peer)  
b. **AG_INIT_EXCH** – Peers have exchanged their first set of packets in aggressive mode, but have not authenticated yet.  
c. **AG_AUTH** – ISAKMP SA’s have been authenticated in aggressive mode and will proceed to QM_IDLE immediately.  
    
Data Packets are now traversing the link securely via IPSec as shown by below out, they are being encrypted and decrypted. The #pkts encaps/encrypt/decap/decrypt, these numbers tell us how many packets have actually traversed the IPSec tunnel.   
{{< bokya src="img/l2lR1_2.jpg" >}}  
  
The crypto session will give us a list of all IKE and IPSec SA Sessions  
{{< bokya src="img/l2lshcryptosession.jpg" >}}  
Some of the common session statuses are as follows:  
   
a. **Up-Active** – IPSec SA is up/active and transferring data.  
b. **Up-IDLE** – IPSsc SA is up, but there is not data going over the tunnel  
c. **Up-No-IKE** – This occurs when one end of the VPN tunnel terminates the IPSec VPN and the remote end attempts to keep using the original SPI, this can be avoided by issuing crypto isakmp invalid-spi-recovery  
d. **Down-Negotiating** – The tunnel is down but still negotiating parameters to complete the tunnel.  
e. **Down** – The VPN tunnel is down.  




