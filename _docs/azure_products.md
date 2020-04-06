---
layout: default
category: Platforms - Azure
title: Products
order: 1
permalink: /azure_products.html
---

# Before we get started...

This page covers what some of the most commonly used Azure services are, what they do, some tips on using them, and where you can get detailed documentation. We'll also briefly discuss which services on other cloud platforms offer similar features. Before getting started, visit the [Azure free trial page](https://azure.microsoft.com/en-us/free/)  to familiarize yourself with the free tier offerings currently available. If you have not already, visit the [cloud account setup page on the documentation site for this hackathon]({% link _docs/cloud_account_setup.md %}) to learn how to sign up for a free account.

# Serverless Computing
Azure's serverless computing offering is called [Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview). These functions do not require the user to configure any server infrastructure. Azure function usage is billed on a pay-per-use model. Azure functions support multiple languages including C#, Java, Python, and Javascript to name a few. Azure functions run only when triggered. Triggers can include schedules, messages, HTTP requests, and more. Azure functions also support package/dependency managers like NuGet and NPM as well as OAuth integrations with Azure Active Directory, Google, Facebook, and others. Additionally, rich integrations with other Azure services are supported. AWS' counterpart to this service is [AWS Lambda](https://aws.amazon.com/lambda/), while GCP has [Cloud Functions](https://cloud.google.com/functions). 

# Virtual Machines
Azure's core VM solution is [Azure Virtual Machines](https://azure.microsoft.com/en-us/services/virtual-machines/). VMs can be scaled from as little as one or two vCPUs and a couple gigabytes of RAM to hundreds of vCPUs and several terabytes of RAM. Azure VMs also ofer virtualized networking among other services to allow for better control over the system. GPU accelerated VMs are also available. Each increase in VM capability increases the cost of operation. VMs cost money to run, just as a physical computer generates electricity costs to operate. As such, as long as a **VM is operational it will incur charges**, even if no software is running on the VM. Additionally, any supplementary services that are provisioned alongside the VM, such as virtual networking switches will also generate charges. Remember to delete any services that are not in use to avoid unecessary charges. Azure VMs are available with either Windows or Linux images, and can be deployed from either an [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/) image or a custom user-supplied image. Visit the informational page to see all available features.

# Containers
More to come soon!

# Databases
More to come soon!

# Storage
More to come soon!

# Events & Messaging
More to come soon!

# Logging
More to come soon!

# Machine Learning
More to come soon!

# Internet of Things
More to come soon!

# Security & Identity Management
More to come soon!