+++
author = "Bryan Florenosos"
title = "Basic IPSec L2L VPN using Crypto Maps"
date = "2021-04-24"
description = "How to configure basic L2L ipsec vpn"
tags = [
    "network",
    "security",
]
+++

This is for my reference on how to actually build a site to site ipsec vpn using crypto-maps. This is the most basic and has the fundamentals.  

Basically IPsec VPN has Two Phases, Phase 1 and Phase 2.

Phase 1 is only concerned in protecting the VPN tunnel session. The protocol that is used in this phase is **ISAKMP**/**IKE** and it is using **UDP 500**. There are **three main things** that are needed on both ends and this are the Key (either psk or rsa), encryption and a hashing algorithm. But basically below paremeters needs to match on both ends in order to establish the phase 1 tunnel. These parameters are (H.A.G.L.E):  

* > Hashing for data integrity (md5/sha)  
* > Authentication (certs or preshared key (PSK))
* > Group (Diffie-Hellman group)
* > Lifetime (how long will the tunnel be up for (seconds)
* > Encryption for data confidentiality (AES, 3DES, AES-256)  