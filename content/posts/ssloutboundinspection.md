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

Self-signed Certificates—The firewall can act as a CA and generate self-signed certificates that thefirewall can use to sign the certificates for sites which require SSL decryption. The firewall can signa copy of the server certificate to present to the client and establish the SSL session. This methodrequires that you need to install the self-signed certificates on all of your network devices so that thosedevices recognize the firewall’s self-signed certificates. Because the certificates must be deployed to all devices, this method is better for small deployments and proof-of-concept (POC) trials than for large deployments.  

**Step 1**  
Login to the Palo Alto Firewall:  
Device > Certificate Management > Certificate > Click Generate  
Certificate name: lab-internal-ca    
Common Name: 172.31.20 -> this is the ip of the trust interface  
Check: Certificate Authority  
Click Generate  
{{< bokya src="img/Step1.jpg" >}}  


**Step 2**  
Click the generated certificate which is Internal-CA  
Check the forward Trust Certificate  
{{< bokya src="img/forwardtrust.jpg" >}}  
The forward trust tells the firewall, anytime a certificate is presented by a webserver for an outbound connection use this cerficate (lab-internal-ca) to proxy that particular certificate, basically the firewall would present itself to the inside client.  

**Step 3**
Go to Objects > Decryption Profile  
Click Add  
Name: SSL-Proxy-Decryption-Profile  
{{< bokya src="img/dercyptionprofile.jpg" >}}  

**Step 4**  
Go to Policies > Decryption  
Click Add  
Name: Decrypt
Important parameters are:  
Action = Decrypt  
Type = ssl-forward-proxy  
Decryption profile = SSL-Proxy-Decryption-Profile  
{{< bokya src="img/decryption.jpg" >}}  


After doing the above steps go back to Certificates, check the certificate that is the Self Signed Certifcate from the firewall (lab-internal-ca) then click export, then no need to export private, and then click ok. Save the file then install to the client machine.  