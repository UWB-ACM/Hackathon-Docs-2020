---
layout: default
category: Platforms - Azure
title: Products
order: 1
permalink: /azure_products.html
---

# Azure Product Overview

## Before Getting Started...

This page covers what some of the most commonly used Azure services are, what they do, and where you can get detailed documentation. We'll also briefly mention which services on other cloud platforms offer similar features. 

Before getting started, visit the [Azure free trial page](https://azure.microsoft.com/en-us/free/)  to familiarize yourself with the free tier offerings currently available. If you have not already, visit the [cloud account setup page on the documentation site for this hackathon]({% link _docs/cloud_account_setup.md %}) to learn how to sign up for a free account.

# Serverless Computing
Azure's serverless computing offering is called [Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview). These functions do not require the user to configure any server infrastructure. 

Key features:
* Azure function usage is billed on a pay-per-use model. 
* Azure functions support multiple languages including C#, Java, Python, and Javascript to name a few. 
* Azure functions run only when triggered. Triggers can include schedules, messages, HTTP requests, and more. 
* Azure functions also support package/dependency managers like NuGet and NPM as well as OAuth integrations with Azure Active Directory, Google, Facebook, and others. Additionally, rich integrations with other Azure services are supported. 

AWS' counterpart to this service is [AWS Lambda](https://aws.amazon.com/lambda/), while GCP has [Cloud Functions](https://cloud.google.com/functions). 

# Virtual Machines
Azure's core VM solution is [Azure Virtual Machines](https://azure.microsoft.com/en-us/services/virtual-machines/). VMs can be scaled from as little as one or two vCPUs and a couple gigabytes of RAM to hundreds of vCPUs and several terabytes of RAM. 

Additional features include:

* Azure VMs also offer virtualized networking among other services to allow for better control over the system. 
* GPU accelerated VMs are also available. 

Each increase in VM capability increases the cost of operation. VMs cost money to run, just as a physical computer generates electricity costs to operate. As such, as long as a **VM is operational it will incur charges**, even if no software is running on the VM. Additionally, any supplementary services that are provisioned alongside the VM, such as virtual networking switches will also generate charges. Remember to delete any services that are not in use to avoid unecessary charges. 

Azure VMs are available with either Windows or Linux images, and can be deployed from either an [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/) image or a custom user-supplied image. Visit the informational page to see all available features. AWS' competing offering is [Amazon Elastic Compute Cloud (EC2)](https://aws.amazon.com/ec2/), while GCP offers [Compute Engine: Virtual Machines](https://cloud.google.com/compute/).

# Containers
Containers are a way of packaging up an application and its dependencies and deploying it onto physical hardware. It's similar to a virtual machine, but there is no Guest OS and no Hypervisor. Instead, containers run on a container runtime and are managed by container orchestration tools. Google has a good explanation of containers [here](https://cloud.google.com/compute/). 

Azure's container offering is called [Azure Container Instances (ACI)](https://azure.microsoft.com/en-us/services/container-instances/#overview). This service allows you to deploy containers without worrying about the underlying infrastructure needed to run them. They can be managed with [Azure Kubernetes Service](https://azure.microsoft.com/en-us/services/kubernetes-service/) and connected to other Azure services. ACI also allows for adding a hypervisor for increased isolation between containers, thereby combining the security of a VM with the simplicity of a container. See [ACI Docs](https://docs.microsoft.com/en-us/azure/container-instances/) for more information on how to use this offering. 

AWS offers container services of its own through [Amazon Elastic Container Service (ECS)](https://aws.amazon.com/ecs/). ECS offers slightly more control than ACI. ECS allows you to either deploy your container on an EC2 VM, or on [AWS Fargate](https://aws.amazon.com/fargate/). AWS Fargate is analogous to ACI in that you do not manage the underlying infrastructure, whereas with and EC deployment, you do manage the container orchestration software, among other things. GCP offers container services through [Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine).

# Databases
## SQL Databases
Azure offers robust SQL database services. Their primary offering is [Azure SQL Database](https://azure.microsoft.com/en-us/services/sql-database/), and they also offer special versions of the service for [MySQL](https://azure.microsoft.com/en-us/services/mysql/) and [PostgreSQL](https://azure.microsoft.com/en-us/services/postgresql/). Azure SQL databases support all the expected SQL database features as well as built-in machine learning optimization, scaling, high availability, and security. Read the [documentation](https://docs.microsoft.com/en-us/azure/sql-database/) for more information on how to get started. AWS's competing service is [Amazon Relational Database Service (RDS)](https://aws.amazon.com/rds/). GCP's product is named [Cloud SQL](https://cloud.google.com/sql/).
## NoSQL Databases
Azure's NoSQL solution is [CosmosDB](https://azure.microsoft.com/en-us/services/cosmos-db/#overview). It offers turnkey distribution, low latency, automatic scaling, and API endpoints compatible with various SQL and NoSQL databases as well as other Azure services. See the [documentation](https://azure.microsoft.com/en-us/services/cosmos-db/#documentation) for more info on everything Cosmos DB can do. AWS offers three slightly different NoSQL services: [DynamoDB](https://aws.amazon.com/dynamodb/), [SimpleDB](https://aws.amazon.com/simpledb/), and [DocumentDB](https://aws.amazon.com/documentdb/). GCP's offering is called [Cloud Datastore](https://cloud.google.com/datastore/).

# Storage
Azure offers simple object storage via [Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction). Blobs are designed for storing raw data meant to be accessed via cloud and web services, or by other Azure products. It can also be used for storing backups and logging data. Blobs can be accessed via the [Azure Storage REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), Azure command line tools, or libraries available for a variety of languages. AWS's counterpart is the [Amazon Simple Storage Service (S3)](https://aws.amazon.com/s3/). GCP offers similar features with their [Cloud Storage](https://cloud.google.com/storage) product.

# Events & Messaging
Azure offers multiple products for passing messages and events. [Azure Queue Storage](https://azure.microsoft.com/en-us/services/storage/queues/) offers scalable message queuing services accessible through a REST API. [Azure Service Bus](https://azure.microsoft.com/en-us/services/service-bus/) allows for messaging services across applications and Azure products. It is possbile to completely decouple applications and have them interact exclusively through messages passed through the service bus. Service bus even supports hybrid cloud deployments, where you have a mix of cloud and on-premises systems. Service Bus is alsos used to manage publish/subscribe (pub/sub) systems. Lastly, [Azure Event Grid](https://azure.microsoft.com/en-us/services/event-grid/) is a service that routes events between any endpoints using a publish/subscribe model. It is most often used alongside serverless application deployments. AWS offers simila features to Queue Storage and Service Bus through their [Amazon Simple Queue Storage (SQS)](https://aws.amazon.com/sqs/) product. AWS also has a competing product to Event Grid known as [Amazon Simple Notification Service (SNS)](https://aws.amazon.com/sns/?whats-new-cards.sort-by=item.additionalFields.postDateTime&whats-new-cards.sort-order=desc). GCP offers similar products in the form of [Cloud Pub/Sub](https://cloud.google.com/pubsub/) and [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/).

# Monitoring
[Azure Monitor](https://azure.microsoft.com/en-us/services/monitor/) allows users to collect, store, and perform analytics on telemetry data from various Azure services. Monitor has a specialized analytic engine and uses machine learning to help discern patterns and generate insights from collected telemetry data. Monitor also integrates with DevOps tools for a streamlined workflow. 

AWS offers similar features in [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) and [AWS X-Ray](https://aws.amazon.com/xray/). [Cloud Logging](https://cloud.google.com/logging) and [Cloud Monitoring](https://cloud.google.com/monitoring) are GCP's competing offerings. 

# Machine Learning
There are many machine learning products available on Azure for different uses cases. These include training and deploying models, working with bots, Natural Language Processing, Speech, Computer Vision, Contextual Services, and more. Check out Azure's [primer on their machine learning technologies](https://docs.microsoft.com/en-us/azure/architecture/data-guide/big-data/machine-learning-at-scale) for a better understanding of what products to use and when to use them. AWS' machine learning page is also available [here](https://aws.amazon.com/machine-learning/), and GCP's [here](https://cloud.google.com/products/ai/).

# Internet of Things
Azure's IoT offering is comprised of several products. Firstly, [Azure IoT Hub](https://azure.microsoft.com/en-us/services/iot-hub/#overview) is the main service used to manage IoT deployments and communication between IoT devices through Azure. [Azure IoT Edge](https://azure.microsoft.com/en-us/services/iot-edge/) allows for cloud IoT logic to be deployed directly to an edge device, thus allowing a local device to service local requests or pass messages between local devices. [Event Hubs](https://azure.microsoft.com/en-us/services/event-hubs/) handle massive ingestion of small data pieces from IoT devices and processes them, routing them to the correct endpoints. Lastly, [Digital Twins](https://azure.microsoft.com/en-us/services/digital-twins/) allows users to virtually map out physical IoT deployments and model interactions between devices and people. This allows for working with devices and data spatially instead of by IDs or other less descriptive identifiers. AWS offers all of these features in their own products. Briefly these are: [AWS IoT](https://aws.amazon.com/iot/), [AWS Greengrass](https://aws.amazon.com/greengrass/), [Kinesis Streams](https://aws.amazon.com/kinesis/data-streams/), [Kinesis Firehose](https://aws.amazon.com/kinesis/data-firehose/), [AWS IoT Things Graph](https://aws.amazon.com/iot-things-graph/). GCP's offerings are grouped together under the [Google Cloud IoT](https://cloud.google.com/solutions/iot/) umbrella.

# Web Applications
Azure offers multiple resources for building web apps. These can be websites, APIs, or any other type of application that is meant for accessing through the internet. Web apps can be easily configured and deployed using [App Services](https://azure.microsoft.com/en-us/services/app-service/). [API Management](https://azure.microsoft.com/en-us/services/api-management/) is a turnkey solution for configuring and deploying APIs to both external users and apps as well as to other internal Azure services. [Azure Content Delivery Network](https://azure.microsoft.com/en-us/services/cdn/) is a scalable CDN solution for serving various types of multimedia and applications. Finally, [Azure Front Door](https://azure.microsoft.com/en-us/services/frontdoor/) offers automated load balancing and routing features. Analogous AWS products include: [Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/), [API Gateway](https://aws.amazon.com/api-gateway/), [AWS CloudFront](https://aws.amazon.com/cloudfront/), and [AWS Global Accelerator](https://aws.amazon.com/global-accelerator/?blogs-global-accelerator.sort-by=item.additionalFields.createdDate&blogs-global-accelerator.sort-order=desc&aws-global-accelerator-wn.sort-by=item.additionalFields.postDateTime&aws-global-accelerator-wn.sort-order=desc). GCP also has various solutions for web apps [here](https://cloud.google.com/solutions/web-hosting).

# Security & Identity Management
Azure's security features are numerous and varied, as each of their products needs to have its own security features. Thus, it is impractical to cover them all here. The main products to be aware of are [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) and [Role-based Access Control](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview). These two services allow for control over who can access an Azure account and what permissions they have. For a detailed overview of all of Azure's security features, see [Azure security documentation](https://docs.microsoft.com/en-us/azure/security/). For reference, here is [AWS' security page](https://aws.amazon.com/security/) and [GCP's trust and security page](https://cloud.google.com/security/).

# More Resources
Here are some helpful links that offer more information into Azure products and services. Finally, don't underestimate the power of a quick internet search! There are many resources available to make learning and working with Azure easier.

## From the Hackathon Docs
- [Azure Resources]({% link _docs/azure_resources.md %})
- [Azure Secrets]({% link _docs/azure_secrets.md %})

## From the Internet
- [Azure Website](https://azure.microsoft.com/en-us/)
- [Complete Azure Product List](https://azure.microsoft.com/en-us/services/)
- [Azure Documentation](https://docs.microsoft.com/en-us/azure/?product=featured)
- [Azure Tutorials](https://docs.microsoft.com/en-us/learn/azure/)
- [Azure vs. AWS Services Comparison](https://docs.microsoft.com/en-us/azure/architecture/aws-professional/services)
- [Azure for AWS Professionals](https://docs.microsoft.com/en-us/azure/architecture/aws-professional/)
- [Azure Integrations for Visual Studio](https://visualstudio.microsoft.com/vs/features/azure/)
- [Azure CLI Getting Started Guide](https://docs.microsoft.com/en-us/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)
- [Azure Pricing](https://azure.microsoft.com/en-us/pricing/)
- [Azure for Students](https://azure.microsoft.com/en-us/free/students/)
