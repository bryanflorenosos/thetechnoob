+++
title = "Zone Based Firewall Configuration Syntax"
description = "ZBF Configuration"
date = "2021-04-14"
aliases = ["ZBF", "zone-based"]
author = "Bryan Florenosos"
+++

## What is Zone Based Firewall ?

In a much simpler explanation ZBF  allows you to make a router act as a firewall. You will allocate and assigned security zones to different interfaces.

Actually in my opinion you can use this approach if your client or customer has a strict budget requirements and still wants an extra security.


## ZBF configuration Syntax.

1. Create the Zones
zone security CORPORATE
zone security INTERNET
zone security GUEST

