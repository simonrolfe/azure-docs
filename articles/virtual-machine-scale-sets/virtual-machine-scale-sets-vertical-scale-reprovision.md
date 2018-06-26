---
title: Vertically scale Azure virtual machine scale sets | Microsoft Docs
description: How to vertically scale a Virtual Machine in response to monitoring alerts with Azure Automation
services: virtual-machine-scale-sets
documentationcenter: ''
author: gatneil
manager: jeconnoc
editor: ''
tags: azure-resource-manager

ms.assetid: 16b17421-6b8f-483e-8a84-26327c44e9d3
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: negat

---
# Vertical autoscale with virtual machine scale sets
This article describes how to vertically scale Azure [Virtual Machine Scale Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/) with or without reprovisioning. For vertical scaling of VMs that are not in scale sets, refer to [Vertically scale Azure virtual machine with Azure Automation](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Vertical scaling, also known as *scale up* and *scale down*, means increasing or decreasing virtual machine (VM) sizes in response to a workload. Compare this behavior with [horizontal scaling](virtual-machine-scale-sets-autoscale-overview.md), also referred to as *scale out* and *scale in*, where the number of VMs is altered depending on the workload.

Reprovisioning means removing an existing VM and replacing it with a new one. When you increase or decrease the size of VMs in a virtual machine scale set, in some cases you want to resize existing VMs and retain your data, while in other cases you need to deploy new VMs of the new size. This document covers both cases.

Vertical scaling can be useful when:

* A service built on virtual machines is under-utilized (for example at weekends). Reducing the VM size can reduce monthly costs.
* Increasing VM size to cope with larger demand without creating additional VMs.

You can set up vertical scaling to be triggered based on metric based alerts from your virtual machine scale set. When the alert is activated, it fires a webhook that triggers a runbook that can scale your scale set up or down. Vertical scaling can be configured by following these steps:

1. Create an Azure Automation account with run-as capability.
2. Import Azure Automation Vertical Scale runbooks for virtual machine scale sets into your subscription.
3. Add a webhook to your runbook.
4. Add an alert to your virtual machine scale set using a webhook notification.

> [!NOTE]
> Vertical autoscaling can only take place within certain ranges of VM sizes. Compare the specifications of each size before deciding to scale from one to another (higher number does not always indicate bigger VM size). You can choose to scale between the following pairs of sizes:
> 
> | VM sizes scaling pair |  |
> | --- | --- |
> | Standard_A0 |Standard_A11 |
> | Standard_D1 |Standard_D14 |
> | Standard_DS1 |Standard_DS14 |
> | Standard_D1v2 |Standard_D15v2 |
> | Standard_G1 |Standard_G5 |
> | Standard_GS1 |Standard_GS5 |
> 
> 

## Create an Azure Automation Account with run-as capability
The first thing you need to do is create an Azure Automation account that hosts the runbooks used to scale the virtual machine scale set instances. Recently [Azure Automation](https://azure.microsoft.com/services/automation/) introduced the "Run As account" feature that makes setting up the Service Principal for automatically running the runbooks on a user's behalf. For more information, see:

* [Authenticate Runbooks with Azure Run As account](../automation/automation-sec-configure-azure-runas-account.md)

## Import Azure Automation Vertical Scale runbooks into your subscription
The runbooks needed to vertically scale your virtual machine scale sets are already published in the Azure Automation Runbook Gallery. To import them into your subscription follow the steps in this article:

* [Runbook and module galleries for Azure Automation](../automation/automation-runbook-gallery.md)

Choose the Browse Gallery option from the Runbooks menu:

![Runbooks to be imported][runbooks]

The runbooks that need to be imported are shown. Select the runbook based on whether you want vertical scaling with or without reprovisioning:

![Runbooks gallery][gallery]

## Add a webhook to your runbook
Once you've imported the runbooks, add a webhook to the runbook so it can be triggered by an alert from a virtual machine scale set. The details of creating a webhook for your Runbook are described in this article:

* [Azure Automation webhooks](../automation/automation-webhooks.md)

> [!NOTE]
> Make sure you copy the webhook URI before closing the webhook dialog as you will need this address in the next section.
> 
> 

## Add an alert to your virtual machine scale set
Below is a PowerShell script that shows how to add an alert to a virtual machine scale set. Refer to the following article to get the name of the metric to fire the alert on:
[Azure Monitor autoscaling common metrics](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [!NOTE]
> It is recommended to configure a reasonable time window for the alert in order to avoid triggering vertical scaling, and any associated service interruption, too often. Consider a window of least 20-30 minutes or more. Consider horizontal scaling if you need to avoid any interruption.
> 
> 

For more information on how to create alerts, see the following articles:

* [Azure Monitor PowerShell quickstart samples](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Azure Monitor Cross-platform CLI quickstart samples](../monitoring-and-diagnostics/insights-cli-samples.md)

## Summary
This article showed simple vertical scaling examples. With these building blocks - Automation account, runbooks, webhooks, alerts - you can connect a rich variety of events with a customized set of actions.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
