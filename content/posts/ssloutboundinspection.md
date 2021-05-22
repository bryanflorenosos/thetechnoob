+++
author = "Bryan Florenosos"
title = "How to configure SSL Outbound Inspection using Firewall generated certificate"
date = "2021-05-22"
description = "Paloalto SSL Outbound Inspection"
tags = [
    "firewall",
    "security",
]
+++

Self-signed Certificates—The firewall can act as a CA and generate self-signed certificates that thefirewall can use to sign the certificates for sites which require SSL decryption. The firewall can signa copy of the server certificate to present to the client and establish the SSL session. This methodrequires that you need to install the self-signed certificates on all of your network devices so that thosedevices recognize the firewall’s self-signed certificates. Because the certificates must be deployed toall devices, this method is better for small deployments and proof-of-concept (POC) trials than for largedeployments.