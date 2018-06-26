---
title: Microsoft Azure IoT options | Microsoft Docs
description: Choose how to implement your IoT solution using Azure IoT solution accelerators, Azure IoT Central, or Azure IoT Hub.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: dobett
---

# Compare Azure IoT options

The article [Azure and the Internet of Things](iot-accelerators-what-is-azure-iot.md) describes a typical IoT solution architecture with the following layers:

* Device connectivity and management
* Data processing and analytics
* Presentation and business integration

To implement this architecture, Azure IoT offers several options, each appropriate for different sets of customer requirements:

* [Azure IoT solution accelerators](../active-directory-domain-services/index.yml) is an enterprise-grade collection of [solution accelerators](iot-accelerators-what-are-solution-accelerators.md) built on Azure Platform-as-a-Service (PaaS) that enable you to accelerate the development of custom IoT solutions.

* [Azure IoT Central](https://www.microsoft.com/internet-of-things/iot-central-saas-solutions) is a Software-as-a-Service (SaaS) solution that uses a model-based approach to enable you to build enterprise-grade IoT solutions without requiring cloud solution development expertise.

## Azure IoT Hub

Azure IoT Hub is the core Azure PaaS that both Azure IoT Central and Azure IoT solution accelerators use. IoT Hub enables reliable and securely bidirectional communications between millions of IoT devices and a cloud solution. IoT Hub helps you meet IoT implementation challenges such as:

* High-volume device connectivity and management.
* High-volume telemetry ingestion.
* Command and control of devices.
* Device security enforcement.

## Compare Azure IoT solution accelerators and Azure IoT Central

Choosing your Azure IoT product is a critical part of planning your IoT solution. IoT Hub is an individual Azure service that doesn't, by itself, provide an end-to-end IoT solution. IoT Hub can be used as a starting point for any IoT solution, and you don’t need to use Azure IoT solution accelerators or Azure IoT Central to use it. Both Azure IoT solution accelerators and Azure IoT Central use IoT Hub along with other Azure services. The following table summarizes the key differences between Azure IoT solution accelerators and Azure IoT Central to help you choose the correct one for your requirements:

|                        | Azure IoT solution accelerators | Azure IoT Central |
| ---------------------- | --------- | ----------- |
| Primary usage | To accelerate development of a custom IoT solution that needs maximum flexibility. | To accelerate time to market for straightforward IoT solutions that don’t require deep service customization. |
| Access to underlying PaaS services          | You have access to the underlying Azure services to manage them, or replace them as needed. | SaaS. Fully managed solution, the underlying services aren't exposed. |
| Flexibility            | High. The code for the microservices is open source and you can modify it in any way you see fit. Additionally, you can customize the deployment infrastructure.| Medium. You can use the built-in browser-based user experience to customize the solution model and aspects of the UI. The infrastructure is not customizable because the different components are not exposed.|
| Skill level                 | Medium-High. You need Java or .NET skills to customize the solution back end. You need JavaScript skills to customize the visualization. | Low. You need modeling skills to customize the solution. No coding skills are required. |
| Get started experience | Solution accelerators implement common IoT scenarios. Can be deployed in minutes. | Application templates and device templates provide pre-built models. Can be deployed in minutes. |
| Pricing                | You can fine-tune the services to control the cost. | Simple, predictable pricing structure. |

The decision of which product to use to build your IoT solution is ultimately determined by:

* Your business requirements.
* The type of solution you want to build
* Your organization's skill set for building and maintaining the solution in the long term.

## Next steps

Based on your chosen product and approach, the suggested next steps are:

* **Azure IoT solution accelerators**: [What are the Azure IoT solution accelerators?](iot-accelerators-what-are-solution-accelerators.md)
* **Azure IoT Central**: [Azure IoT Central](https://www.microsoft.com/internet-of-things/iot-central-saas-solutions).
* **IoT Hub**: [Overview of the Azure IoT Hub service](../iot-hub/iot-hub-what-is-iot-hub.md).
