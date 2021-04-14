+++
author = "Bryan Florenosos"
title = "Zone Based Firewall Configuration"
date = "2019-03-11"
description = "ZBF Configuration"
tags = [
    "security",
]
+++

##### Zone Based Firewall (ZBF)

Allows a router to act as a stateful firewall, where we define and allocate security zones.
Just like a typical firewall interzone traffic is denied by default, unless specified by a rule/policy and Intrazone traffic is allowed by default.

{{< figure src="/images/zbf.jpg" title="Topology" >}}

###### Create the Zones

`zone security CORPORATE`
`zone security INTERNET`
`zone security GUST`

###### Assign the Interfaces to Zones