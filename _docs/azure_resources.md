---
layout: default
category: Platforms - Azure
title: Resources
order: 3
permalink: /azure_resources.html
---

# Azure Resources, SDK Links, Tutorials, and Helpful Tidbits

## SDKs and Tools
### SDKs
Azure offers two types of SDKs. Unified SDKs are built on a "common core" and are recommended for all new projects. Standard SDKs are avilable for older projects or languages unsupported by Unified SDKs.

- [Unified SDKs](https://azure.github.io/azure-sdk/releases/latest/index.html#java-packages) available for: .NET, Java, Python, TypeScript/JavaScript
- [Standard .NET SDKs](https://azure.github.io/azure-sdk/releases/latest/all/dotnet.html) (also included with Visual Studio 2019)
- [Standard Java SDKs](https://azure.github.io/azure-sdk/releases/latest/all/java.html) 
- [Standard Python SDKs](https://azure.github.io/azure-sdk/releases/latest/all/python.html) 
- [Standard TypeScript/JavaScript SDKs](https://azure.github.io/azure-sdk/releases/latest/all/js.html) 
- [Standard PHP SDKs](https://packagist.org/packages/microsoft/) 
- [Standard Go SDKs](https://docs.microsoft.com/en-us/azure/developer/go/azure-sdk-install) 
- [Azure SDKs Page](https://azure.microsoft.com/en-us/downloads/)

### Tools
- [Visual Studio 2019](https://visualstudio.microsoft.com/) has excellent integrations with Azure services, especially when using C#/.NET.
- [Azure Visual Studio](https://azure.microsoft.com/en-us/products/visual-studio/)
- [Azure Extensions for VSCode](https://code.visualstudio.com/docs/azure/extensions)
- [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) is a set of Azure tools for use in the command line.
- [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview)
- [Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/)
- [Azure Data Studio](https://docs.microsoft.com/en-us/sql/azure-data-studio/download-azure-data-studio?view=sql-server-2017)
- [Azure Tools Page](https://azure.microsoft.com/en-us/product-categories/developer-tools/)

## Tutorials
- [Getting Started with Azure](https://azure.microsoft.com/en-us/get-started/)
- [Microsoft Learn - Azure](https://docs.microsoft.com/en-us/learn/azure/)
- [Azure Documentation](https://docs.microsoft.com/en-us/azure/?product=featured)
- [Azure 15-Minute Tutorials](https://tutorials.visualstudio.com/)
- [Getting Started with Visual Studio](https://visualstudio.microsoft.com/vs/getting-started/)
- [Developing Azure Functions with Visual Studio](https://docs.microsoft.com/en-us/azure/azure-functions/functions-develop-vs)
- [Deploying Azure Applications with VSCode](https://code.visualstudio.com/docs/azure/deployment)
- [The Developer's Guide to Azure](https://azure.microsoft.com/en-us/campaigns/developer-guide/)

## Helpful Tidbits
- Most resources on Azure require supporting resources to be provisioned alongside the main offering. For example, provisioning a VM will also require a Storage Account, Virtual Networking, etc. These resources will remain provisioned *even if you delete the main resource.* Remember to delete **all** resources when you are done using them to avoid charges.