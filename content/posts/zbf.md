+++
author = "Bryan Florenosos"
title = "Zone Based Firewall Configuration"
date = "2019-03-11"
description = "Sample article showcasing basic Markdown syntax and formatting for HTML elements."
tags = [
    "security",
]
categories = [
    "security",
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]
+++

##### Zone Based Firewall (ZBF)

Allows a router to act as a stateful firewall, where we define and allocate security zones.
Just like a typical firewall interzone traffic is denied by default, unless specified by a rule/policy and Intrazone traffic is allowed by default.

{{< container-image path="images/zbf.jpg" >}}