+++
author = "Bryan Florenosos"
title = "My Flat Management Baseline"
date = "2021-04-15"
description = "fresh rotuer management baseline my preference"
tags = [
    "network",
    "routing",
    "switching",
]
+++

hostname (Name)  
banner motd ^C  
****************************** WARNING!!! **********************************  
 This is a company private property. If you have accessed this facility by     
 mistake, please disconnect immediately. Unauthorized access to this system  
 may subject you to disciplinary action and criminal prosecution.  
****************************** WARNING!!! **********************************   
^C  
!  
!  
ip domain-name mydomain.com.ph  
!  
clock timezone UTC +8  
!  
service timestamps debug datetime msec localtime show-timezone  
service timestamps log datetime msec show-timezone  
service timestamps log datetime msec localtime show-timezone  
logging on  
!  
ip options ignore  
!  
no boot network  
!  
no service config  
!  
no ip bootp server  
!  
no ip source-route  
!  
no service finger  
!  
no service pad  
!  
no ip http server   
no ip http secure-server  
!  
service password-encryption  
!  
service tcp-keepalives-in  
service tcp-keepalives-out  
!  
aaa new-model  
aaa local authentication attempts max-fail 3  
aaa authentication login default local  
!  
username admin secret cisco123  
!  
enable secret cisco123    
no enable password  
!  
!  
crypto key generate rsa modulus 2048  
!  
ip ssh time-out 60  
ip ssh authentication-retries 3  
!  
ip ssh version 2  
!  
!  
access-list 1 remark VTY Access  
access-list 1 permit 172.168.100.0 0.0.0.255  
!  
line vty 0 4  
 access-class 1 in vrf-also  
 exec-timeout 10 0  
 login authentication default    
 access-class 1 in  
 transport input ssh  
 transport output ssh  
 transport preferred none  
 history size 256  
!  
line con 0  
 login authentication default  
 password cisco123  
 exec-timeout 10 0  
 history size 256  
 transport preferred none  
!  
line aux 0  
 transport input none  
 transport output none  
 no exec  
 exec-timeout 0 1  
 no password  
!  
no logging console  
no logging monitor  
logging buffered 16384 6  
!  





