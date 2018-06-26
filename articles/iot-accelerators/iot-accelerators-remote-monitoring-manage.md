---
title: Device management in remote monitoring solution - Azure | Microsoft Docs
description: This tutorial shows you how to manage devices connected to the remote monitoring solution.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 05/01/2018
ms.topic: conceptual
---

# Manage and configure your devices

This tutorial shows the device management capabilities of the Remote Monitoring solution. To introduce these capabilities, the tutorial uses a scenario in the Contoso IoT application.

Contoso has ordered new machinery to expand one of their facilities to increase output. While you wait for the new machinery to be delivered, you want to run a simulation to verify the behavior of your solution. As an operator, you want to manage and configure the devices in the Remote Monitoring solution.

To provide an extensible way to manage and configure devices, the Remote Monitoring solution uses IoT Hub features such as [jobs](../iot-hub/iot-hub-devguide-jobs.md) and [direct methods](../iot-hub/iot-hub-devguide-direct-methods.md). To learn how a device developer implements methods on a physical device, see [Customize the Remote Monitoring solution accelerator](iot-accelerators-remote-monitoring-customize.md).

In this tutorial, you learn how to:

>[!div class="checklist"]
> * Provision a simulated device.
> * Test the simulated device.
> * Call device methods from the solution.
> * Reconfigure a device.

## Prerequisites

To follow this tutorial, you need a deployed instance of the Remote Monitoring solution in your Azure subscription.

If you haven't deployed the Remote Monitoring solution yet, you should complete the [Deploy the Remote Monitoring solution accelerator](iot-accelerators-remote-monitoring-deploy.md) tutorial.

## Add a simulated device

Navigate to the **Devices** page in the solution and then choose **+ New device**. In the **New device** panel, choose **Simulated**:

![Provision a simulated device](./media/iot-accelerators-remote-monitoring-manage/devicesprovision.png)

Leave the number of devices to provision set to **1**. Choose the **Faulty Engine** device model, and then choose **Apply** to create the simulated device:

![Provision a simulated engine device](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine.png)

To learn how to provision a *physical* device, see [Connect your device to the Remote Monitoring solution accelerator](iot-accelerators-connecting-devices-node.md).

## Test the simulated device

To view details of your new simulated device, select it in the list of devices on the **Devices** page. Information about your device displays in the **Device detail** panel:

![View the new simulated engine device](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew.png)

In **Device detail**, verify your new device is sending telemetry. To view the different telemetry streams from your device, choose a telemetry name such as **vibration**:

![Select a telemetry stream to view](./media/iot-accelerators-remote-monitoring-manage/devicesvibration.png)

The **Device detail** panel displays other information about the device such as tag values, the methods it supports, and the properties reported by the device.

To view detailed diagnostics, scroll down to view **Diagnostics**.

## Act on a device

To act on one or more devices, select them in the list of devices and then choose **Jobs**. The **Engine** device model specifies three methods a device must support:

![Engine methods](./media/iot-accelerators-remote-monitoring-manage/devicesmethods.png)

Choose **FillTank**, set the job name to **FillEngineTank**, and then choose **Apply**:

![Schedule the restart method](./media/iot-accelerators-remote-monitoring-manage/devicesrestartengine.png)

To track the status of the job on the **Maintenance** page, choose **Jobs**:

![Monitor the schedules job](./media/iot-accelerators-remote-monitoring-manage/maintenancerestart.png)

### Methods in other devices

When you explore the different simulated device types, you see that other device types support different methods. In a deployment with physical devices, the device model specifies the methods the device should support. Typically, the device developer is responsible for developing the code that makes the device act in response to a method call.

To schedule a method to run on multiple devices, you can select multiple devices in the list on the **Devices** page. The **Jobs** panel shows the types of method common to all the selected devices.

## Reconfigure a device

To change the configuration of a device, select it in the device list on the **Devices** page, then choose **Jobs**, and then choose **Reconfigure**. The jobs panel shows the property values for the selected device that you can change:

![Reconfigure a device](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure.png)

To make a change, add a name for the job, update the property values, and choose **Apply**:

![Update a device property value](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical.png)

To track the status of the job on the **Maintenance** page, choose **Jobs**.

## Next steps

This tutorial showed you how to:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Provision a simulated device.
> * Test the simulated device.
> * Call device methods from the solution.
> * Reconfigure a device.

Now that you have learned how to manage your devices, the suggested next steps are to learn how to:

* [Troubleshoot and remediate device issues](iot-accelerators-remote-monitoring-maintain.md).
* [Test your solution with simulated devices](iot-accelerators-remote-monitoring-test.md).
* [Connect your device to the Remote Monitoring solution accelerator](iot-accelerators-connecting-devices-node.md).

<!-- Next tutorials in the sequence -->