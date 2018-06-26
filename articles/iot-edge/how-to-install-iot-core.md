---
title: Install Azure IoT Edge on IoT Core | Microsoft Docs 
description: Install the Azure IoT Edge runtime on a Windows IoT Core device
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: veyalla
ms.date: 03/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
---

# Install the IoT Edge runtime on Windows IoT Core - preview

Azure IoT Edge and [Windows IoT Core](https://docs.microsoft.com/windows/iot-core/) work together to enable edge computing on even small devices. The Azure IoT Edge Runtime can run even on tiny Single Board Computer (SBC) devices which are very prevalent in the IoT industry. 

This article walks through provisioning the runtime on a [MinnowBoard Turbot][lnk-minnow] development board running Windows IoT Core. Windows IoT Core supports Azure IoT Edge only on Intel x64-based processors. 

## Install the runtime

1. Install [Windows 10 IoT Core Dashboard][lnk-core] on a host system.
1. Follow the steps in [Set up your device][lnk-board] to configure your board with the MinnowBoard Turbot/MAX Build 16299 image. 
1. Turn on the device, then [login remotely with PowerShell][lnk-powershell].
1. In the PowerShell console, install the container runtime: 

   ```powershell
   Invoke-WebRequest https://master.dockerproject.org/windows/x86_64/docker-0.0.0-dev.zip -o temp.zip
   Expand-Archive .\temp.zip $env:ProgramFiles -f
   Remove-Item .\temp.zip
   $env:Path += ";$env:programfiles\docker"
   SETX /M PATH "$env:Path"
   dockerd --register-service
   start-service docker
   ```

   >[!NOTE]
   >This container runtime is from the Moby project build server, and is intended for evaluation purposes only. It's not tested, endorsed, or supported by Docker.

1. Install the IoT Edge runtime and verify your configuration:

   ```powershell
   Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
   ```

   This script provides the following: 
   * Python 3.6
   * The IoT Edge control script (iotedgectl.exe)

You may see informational output from the iotedgectl.exe tool in green in the remote PowerShell window. This doesn't necessarily indicate errors. 

## Next steps

Now that you have a device running the IoT Edge runtime, learn how to [Deploy and monitor IoT Edge modules at scale][lnk-deploy].

<!--Links-->
[lnk-minnow]: https://minnowboard.org/ 
[lnk-core]: https://docs.microsoft.com/windows/iot-core/connect-your-device/iotdashboard
[lnk-board]: https://developer.microsoft.com/windows/iot/Docs/GetStarted/mbm/sdcard/stable/getstartedstep2
[lnk-powershell]: https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell
[lnk-deploy]: how-to-deploy-monitor.md
[lnk-docker-install]: https://docs.docker.com/engine/installation/linux/docker-ce/binaries#install-server-and-client-binaries-on-windows
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers