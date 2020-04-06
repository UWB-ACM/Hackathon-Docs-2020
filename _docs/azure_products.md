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
Azure's core VM solution is [Azure Virtual Machines](https://azure.microsoft.com/en-us/services/virtual-machines/). VMs can be scaled from as little as one or two vCPUs and a couple gigabytes of RAM to hundreds of vCPUs and several terabytes of RAM. Azure VMs also ofer virtualized networking among other services to allow for better control over the system. GPU accelerated VMs are also available. Each increase in VM capability increases the cost of operation. VMs cost money to run, just as a physical computer generates electricity costs to operate. As such, as long as a **VM is operational it will incur charges**, even if no software is running on the VM. Additionally, any supplementary services that are provisioned alongside the VM, such as virtual networking switches will also generate charges. Remember to delete any services that are not in use to avoid unecessary charges. Azure VMs are available with either Windows or Linux images, and can be deployed from either an [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/) image or a custom user-supplied image. Visit the informational page to see all available features. AWS' competing offering is [Amazon Elastic Compute Cloud (EC2)](https://aws.amazon.com/ec2/), while GCP offers [Compute Engine: Virtual Machines](https://cloud.google.com/compute/).

# Containers
Containers are a way of packaging up an application and its dependencies and deploying it onto physical hardware. It's similar to a virtual machine, but there is no Guest OS and no Hypervisor. Instead, containers run on a container runtime and are managed by container orchestration tools. Google has a good explainer on containers [here](https://cloud.google.com/compute/). Azure's container offering is called [Azure Container Instances (ACI)](https://azure.microsoft.com/en-us/services/container-instances/#overview). This service allows you to deploy containers without worrying about the underlying infrastructure needed to run them. They can be managed with [Azure Kubernetes Service](https://azure.microsoft.com/en-us/services/kubernetes-service/?&OCID=AID2000586_SEM_XoVDagAAAF2_uDSC:20200406212031:s&msclkid=b7624fa8c66214d108e711235b7eda00&ef_id=XoVDagAAAF2_uDSC:20200406212031:s) and connected to other Azure services. ACI also allows for adding a hypervisor for increased isolation between containers, thereby comabining the security of a VM with the simplicity of a container. See [ACI Docs](https://docs.microsoft.com/en-us/azure/container-instances/) for more information on how to use this offering. AWS offers container services of its own through [Amazon Elastic Container Service (ECS)](https://aws.amazon.com/ecs/). ECS offers slightly more control than ACI. ECS allows you to either deploy your container on an EC2 VM, or on [AWS Fargate](https://aws.amazon.com/fargate/). AWS Fargate is analagous to ACI in that you do not manage the underlying infrastructure, whereas with and EC2 deployement, you do manage the container orchestration software, among other things. GCP offers container services through [Google Kubernete Engine](https://cloud.google.com/kubernetes-engine).

# Databases
## SQL Databases
Azure offers robust SQL database services. Their primary offering is [Azure SQL Database](https://azure.microsoft.com/en-us/services/sql-database/), and they also offer special versions of the service for [MySQL](https://azure.microsoft.com/en-us/services/mysql/) and [PostgreSQL](https://azure.microsoft.com/en-us/services/postgresql/). Azure SQL databases support all the expected SQL database features as well as built-in machine learning optimization, scaling, high availability, and security. Read the [documentation](https://docs.microsoft.com/en-us/azure/sql-database/) for more information on how to get started. AWS's competing service is [Amazon Relational Database Service (RDS)](https://aws.amazon.com/rds/). GCP's product is named [Cloud SQL](https://cloud.google.com/sql/).
## NoSQL Databases
Azure's NoSQL solution is [CosmosDB](https://azure.microsoft.com/en-us/services/cosmos-db/#overview). It offers turnkey distribution, low latency, automatic scaling, and API endpoints compatible with various SQL and NoSQL databases as well as other Azure services. See the [documentation](https://azure.microsoft.com/en-us/services/cosmos-db/#documentation) for more info on everything Cosmos DB can do. AWS offers three slightly different NoSQL services: [DynamoDB](https://aws.amazon.com/dynamodb/), [SimpleDB](https://aws.amazon.com/simpledb/), and [DocumentDB](https://aws.amazon.com/documentdb/). GCP's offering is called [Cloud Datastore](https://cloud.google.com/datastore/).

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

# More Resources
More to come soon!