---
title: Traffic Manager Endpoint Types | Microsoft Docs
description: This article explains different types of endpoints that can be used with Azure Traffic Manager
services: traffic-manager
documentationcenter: ''
author: kumudd
manager: timlt
editor: ''

ms.assetid: 4e506986-f78d-41d1-becf-56c8516e4d21
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2017
ms.author: kumud
---

# Traffic Manager endpoints
Microsoft Azure Traffic Manager allows you to control how network traffic is distributed to application deployments running in different datacenters. You configure each application deployment as an 'endpoint' in Traffic Manager. When Traffic Manager receives a DNS request, it chooses an available endpoint to return in the DNS response. Traffic manager bases the choice on the current endpoint status and the traffic-routing method. For more information, see [How Traffic Manager Works](traffic-manager-how-traffic-manager-works.md).

There are three types of endpoint supported by Traffic Manager:
* **Azure endpoints** are used for services hosted in Azure.
* **External endpoints** are used for services hosted outside Azure, either on-premises or with a different hosting provider.
* **Nested endpoints** are used to combine Traffic Manager profiles to create more flexible traffic-routing schemes to support the needs of larger, more complex deployments.

There is no restriction on how endpoints of different types are combined in a single Traffic Manager profile. Each profile can contain any mix of endpoint types.

The following sections describe each endpoint type in greater depth.

## Azure endpoints

Azure endpoints are used for Azure-based services in Traffic Manager. The following Azure resource types are supported:

* 'Classic' IaaS VMs and PaaS cloud services.
* Web Apps
* PublicIPAddress resources (which can be connected to VMs either directly or via an Azure Load Balancer). The publicIpAddress must have a DNS name assigned to be used in a Traffic Manager profile.

PublicIPAddress resources are Azure Resource Manager resources. They do not exist in the classic deployment model. Thus they are only supported in Traffic Manager's Azure Resource Manager experiences. The other endpoint types are supported via both Resource Manager and the classic deployment model.

When using Azure endpoints, Traffic Manager detects when a 'Classic' IaaS VM, cloud service, or a Web App is stopped and started. This status is reflected in the endpoint status. See [Traffic Manager endpoint monitoring](traffic-manager-monitoring.md#endpoint-and-profile-status) for details. When the underlying service is stopped, Traffic Manager does not perform endpoint health checks or direct traffic to the endpoint. No Traffic Manager billing events occur for the stopped instance. When the service is restarted, billing resumes and the endpoint is eligible to receive traffic. This detection does not apply to PublicIpAddress endpoints.

## External endpoints

External endpoints are used for services outside of Azure. For example, a service hosted on-premises or with a different provider. External endpoints can be used individually or combined with Azure Endpoints in the same Traffic Manager profile. Combining Azure endpoints with External endpoints enables various scenarios:

* In either an active-active or active-passive failover model, use Azure to provide increased redundancy for an existing on-premises application.
* To reduce application latency for users around the world, extend an existing on-premises application to additional geographic locations in Azure. For more information, see [Traffic Manager 'Performance' traffic routing](traffic-manager-routing-methods.md#performance).
* Use Azure to provide additional capacity for an existing on-premises application, either continuously or as a 'burst-to-cloud' solution to meet a spike in demand.

In certain cases, it is useful to use External endpoints to reference Azure services (for examples, see the [FAQ](traffic-manager-faqs.md#traffic-manager-endpoints)). In this case, health checks are billed at the Azure endpoints rate, not the External endpoints rate. However, unlike Azure endpoints, if you stop or delete the underlying service, health check billing continues until you disable or delete the endpoint in Traffic Manager.

## Nested endpoints

Nested endpoints combine multiple Traffic Manager profiles to create flexible traffic-routing schemes and support the needs of larger, complex deployments. With Nested endpoints, a 'child' profile is added as an endpoint to a 'parent' profile. Both the child and parent profiles can contain other endpoints of any type, including other nested profiles. For more information, see [nested Traffic Manager profiles](traffic-manager-nested-profiles.md).

## Web Apps as endpoints

Some additional considerations apply when configuring Web Apps as endpoints in Traffic Manager:

1. Only Web Apps at the 'Standard' SKU or above are eligible for use with Traffic Manager. Attempts to add a Web App of a lower SKU fail. Downgrading the SKU of an existing Web App results in Traffic Manager no longer sending traffic to that Web App.
2. When an endpoint receives an HTTP request, it uses the 'host' header in the request to determine which Web App should service the request. The host header contains the DNS name used to initiate the request, for example 'contosoapp.azurewebsites.net'. To use a different DNS name with your Web App, the DNS name must be registered as a custom domain name for the App. When adding a Web App endpoint as an Azure endpoint, the Traffic Manager profile DNS name is automatically registered for the App. This registration is automatically removed when the endpoint is deleted.
3. Each Traffic Manager profile can have at most one Web App endpoint from each Azure region. To work around for this constraint, you can configure a Web App as an External endpoint. For more information, see the [FAQ](traffic-manager-faqs.md#traffic-manager-endpoints).

## Enabling and disabling endpoints

Disabling an endpoint in Traffic Manager can be useful to temporarily remove traffic from an endpoint that is in maintenance mode or being redeployed. Once the endpoint is running again, it can be re-enabled.

Endpoints can be enabled and disabled via the Traffic Manager portal, PowerShell, CLI or REST API, all of which are supported in both Resource Manager and the classic deployment model.

> [!NOTE]
> Disabling an Azure endpoint has nothing to do with its deployment state in Azure. An Azure service (such as a VM or Web App remains running and able to receive traffic even when disabled in Traffic Manager. Traffic can be addressed directly to the service instance rather than via the Traffic Manager profile DNS name. For more information, see [how Traffic Manager works](traffic-manager-how-traffic-manager-works.md).

The current eligibility of each endpoint to receive traffic depends on the following factors:

* The profile status (enabled/disabled)
* The endpoint status (enabled/disabled)
* The results of the health checks for that endpoint

For details, see [Traffic Manager endpoint monitoring](traffic-manager-monitoring.md#endpoint-and-profile-status).

> [!NOTE]
> Since Traffic Manager works at the DNS level, it is unable to influence existing connections to any endpoint. When an endpoint is unavailable, Traffic Manager directs new connections to another available endpoint. However, the host behind the disabled or unhealthy endpoint may continue to receive traffic via existing connections until those sessions are terminated. Applications should limit the session duration to allow traffic to drain from existing connections.

If all endpoints in a profile are disabled, or if the profile itself is disabled, then Traffic Manager sends an 'NXDOMAIN' response to a new DNS query.


## Next steps

* Learn [how Traffic Manager works](traffic-manager-how-traffic-manager-works.md).
* Learn about Traffic Manager [endpoint monitoring and automatic failover](traffic-manager-monitoring.md).
* Learn about Traffic Manager [traffic routing methods](traffic-manager-routing-methods.md).
