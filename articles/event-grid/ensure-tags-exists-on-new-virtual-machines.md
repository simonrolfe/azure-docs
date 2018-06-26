---
title: Integrate Azure Automation with Event Grid | Microsoft Docs
description: Learn how to automatically add a tag when a new VM is created and send a notification to Microsoft Teams.
keywords: automation, runbook, teams, event grid, virtual machine, VM
services: automation
documentationcenter: ''
author: eamonoreilly
manager: 
editor: 

ms.service: automation
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/06/2017
ms.author: eamono

---

# Integrate Azure Automation with Event Grid and Microsoft Teams

In this tutorial, you learn how to:

> [!div class="checklist"]
> * Import an Event Grid sample runbook.
> * Create an optional Microsoft Teams webhook.
> * Create a webhook for the runbook.
> * Create an Event Grid subscription.
> * Create a VM that triggers the runbook.

If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.

## Prerequisites

To complete this tutorial, an [Azure Automation account](../automation/automation-offering-get-started.md) is required to hold the runbook that is triggered from the Azure Event Grid subscription.

## Import an Event Grid sample runbook
1. Select your Automation account, and select the **Runbooks** page.

   ![Select runbooks](./media/ensure-tags-exists-on-new-virtual-machines/select-runbooks.png)

2. Select the **Browse gallery** button.

3. Search for **Event Grid**, and select **Integrating Azure Automation with Event grid**. 

    ![Import gallery runbook](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)

4. Select **Import** and name it **Watch-VMWrite**.

5. After it has imported, select **Edit** to view the runbook source. Select the **Publish** button.

## Create an optional Microsoft Teams webhook
1. In Microsoft Teams, select **More Options** next to the channel name, and then select **Connectors**.

    ![Microsoft Teams connections](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)

2. Scroll through the list of connectors to **Incoming Webhook**, and select **Add**.

3. Enter **AzureAutomationIntegration** for the name, and select **Create**.

4. Copy the webhook to the clipboard, and save it. The webhook URL is used to send information to Microsoft Teams.

5. Select **Done** to save the webhook.

## Create a webhook for the runbook
1. Open the Watch-VMWrite runbook.

2. Select **Webhooks**, and select the **Add Webhook** button.

3. Enter **WatchVMEventGrid** for the name. Copy the URL to the clipboard, and save it.

    ![Configure webhook name](media/ensure-tags-exists-on-new-virtual-machines/copy-url.png)

4. Select **Configure parameters and run settings**, and enter the Microsoft Teams webhook URL for **CHANNELURL**. Leave **WEBHOOKDATA** blank.

    ![Configure webhook parameters](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)

5. Select **OK** to create the Automation runbook webhook.


## Create an Event Grid subscription
1. On the **Automation Account** overview page, select **Event grid**.

    ![Select Event Grid](media/ensure-tags-exists-on-new-virtual-machines/select-event-grid.png)

2. Select the **+ Event Subscription** button.

3. Configure the subscription with the following information:

    *	Enter **AzureAutomation** for the name.
    *	In **Topic Type**, select **Azure Subscriptions**.
    *	Clear the **Subscribe to all event types** check box.
    *	In **Event Types**, select **Resource Write Success**.
    *	In **Subscriber Endpoint**, enter the webhook URL for the Watch-VMWrite runbook.
    *   In **Prefix Filter**, enter the subscription and resource group where you want to look for the new VMs created. It should look like:
 `/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines`

4. Select **Create** to save the Event Grid subscription.

## Create a VM that triggers the runbook
1. Create a new VM in the resource group you specified in the Event Grid subscription prefix filter.

2. The Watch-VMWrite runbook should be called and a new tag added to the VM.

    ![VM tag](media/ensure-tags-exists-on-new-virtual-machines/vm-tag.png)

3. A new message is sent to the Microsoft Teams channel.

    ![Microsoft Teams notification](media/ensure-tags-exists-on-new-virtual-machines/teams-vm-message.png)

## Next steps
In this tutorial, you set up integration between Event Grid and Automation. You learned how to:

> [!div class="checklist"]
> * Import an Event Grid sample runbook.
> * Create an optional Microsoft Teams webhook.
> * Create a webhook for the runbook.
> * Create an Event Grid subscription.
> * Create a VM that triggers the runbook.

> [!div class="nextstepaction"]
> [Create and route custom events with Event Grid](../event-grid/custom-event-quickstart.md)
