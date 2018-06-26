---
title: Border connectivity network integration considerations for Azure Stack integrated systems | Microsoft Docs
description: Learn what you can do to plan for datacenter border network connectivity with multi-node Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''

ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeffgilb
ms.reviewer: wamota
---

# Border connectivity 
Network integration planning is an important prerequisite for successful Azure Stack integrated systems deployment, operation, and management. Border connectivity planning begins by choosing whether or not to use dynamic routing with border gateway protocol (BGP). This requires assigning a 16-bit BGP autonomous system number (public or private) or using static routing, where a static default route is assigned to the border devices.

> [!IMPORTANT]
> The top of rack (TOR) switches require Layer 3 uplinks with Point-to-Point IPs (/30 networks) configured on the physical interfaces. It is not supported to use Layer 2 uplinks with TOR switches supporting Azure Stack operations. 

## BGP routing
Using a dynamic routing protocol like BGP guarantees that your system is always aware of network changes and facilitates administration. 

As shown in the following diagram, advertising of the private IP space on the TOR switch is restricted using a prefix-list. The prefix lists denies the private IP subnets and applying it as a route-map on the connection between the TOR and the border.

The Software Load Balancer (SLB) running inside the Azure Stack solution peers to the TOR devices so it can dynamically advertise the VIP addresses.

To ensure that user traffic immediately and transparently recovers from failure, the VPC or MLAG configured between the TOR devices allows the use of multi-chassis link aggregation to the hosts and HSRP or VRRP that provides network redundancy for the IP networks.

![BGP routing](media/azure-stack-border-connectivity/bgp-routing.png)

## Static routing
Using static routes adds more fixed configuration to the border and TOR devices. It requires thorough analysis before any change. Issues caused by a configuration error may take more time to rollback depending on the changes made. It is not the recommended routing method, but it is supported.

To integrate Azure Stack into your networking environment using this method, the border device must be configured with static routes pointing to the TOR devices for traffic destined to external network, public VIPs.

The TOR devices must be configured with a static default route sending all traffic to the border devices. The one traffic exception to this rule is for the private space which will be blocked using an Access Control List applied on the TOR to border connection.

Everything else should be the same as the first method. The BGP dynamic routing will still be used inside the rack because it is an essential tool for the SLB and other components and can’t be disabled or removed.

![Static routing](media/azure-stack-border-connectivity/static-routing.png)

## Transparent proxy
If your datacenter requires all traffic to use a proxy, you must configure a *transparent proxy* to process all traffic from the rack to handle it according to policy, separating traffic between the zones on your network.

> [!IMPORTANT]
> The Azure Stack solution doesn’t support normal web proxies.  

A transparent proxy (also known as an intercepting, inline, or forced proxy) intercepts normal communication at the network layer without requiring any special client configuration. Clients need not to be aware of the existence of the proxy.

![Transparent proxy](media/azure-stack-border-connectivity/transparent-proxy.png)

## Next steps
[DNS integration](azure-stack-integrate-dns.md)