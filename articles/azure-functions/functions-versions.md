---
title: Azure Functions runtime versions overview
description: Azure Functions supports multiple versions of the runtime. Learn the differences between them and how to choose the one that's right for you.
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: ''

ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: glenga

---
# Azure Functions runtime versions overview

 There are two major versions of the Azure Functions runtime.: 1.x and 2.x. This article explains how to choose which major version to use.

> [!IMPORTANT] 
> Runtime 1.x is the only version approved for production use.

| Runtime | Status |
|---------|---------|
|1.x|Generally Available (GA)|
|2.x|Preview|

For information about how to configure a function app or your development environment for a particular version, see [How to target Azure Functions runtime versions](set-runtime-version.md) and [Code and test Azure Functions locally](functions-run-local.md).

## Cross-platform development

Runtime 1.x supports development and hosting only in the portal or on Windows. Runtime 2.x runs on .NET Core, which means it can run on all platforms supported by .NET Core, including macOS and Linux. This enables cross-platform development and hosting scenarios that aren't possible with 1.x.

## Languages

Runtime 2.x uses a new language extensibility model. Initially, JavaScript and Java are taking advantage of this new model. Azure Functions 1.x experimental languages haven't been updated to use the new model, so they are not supported in 2.x. The following table indicates which programming languages are supported in each runtime version.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

For more information, see [Supported languages](supported-languages.md).

## Bindings 

Runtime 2.x uses a new [binding extensibility model](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) that offers these advantages:

* Support for third-party binding extensions.
* Decoupling of runtime and bindings. This allows binding extensions to be versioned and released independently. You can, for example, opt to upgrade to a version of an extension that relies on a newer version of an underlying SDK.
* A lighter execution environment, where only the bindings in use are known and loaded by the runtime.

All built-in Azure Functions bindings have adopted this model and are no longer included by default, except for the Timer trigger and the HTTP trigger. Those extensions are automatically installed when you create functions through tools like Visual Studio or through the portal.

The following table indicates which bindings are supported in each runtime version.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

## Known issues in 2.x

For more information about bindings support and other functional gaps in 2.x, see [Runtime 2.0 known issues](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues).

## Next steps

> [!div class="nextstepaction"]
> [Target the 2.0 runtime in your local development environment](functions-run-local.md)

> [!div class="nextstepaction"]
> [See Release notes for runtime versions](https://github.com/Azure/azure-webjobs-sdk-script/releases)