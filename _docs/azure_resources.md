---
layout: default
category: Platforms - Azure
title: Resources
order: 3
permalink: /azure_resources.html
---

# Azure Resources, SDK Links, Tutorials, Helpful Tidbits, and Permissions Management

## SDKs and Tools
### SDKs
Azure offers two types of SDKs. Unified SDKs are built on a "common core" and are recommended for all new projects. Standard SDKs are avilable for older projects or languages unsupported by Unified SDKs.

- [Unified SDKs](https://azure.github.io/azure-sdk/releases/latest/index.html) available for: .NET, Java, Python, TypeScript/JavaScript
- [Standard .NET SDKs](https://azure.github.io/azure-sdk/releases/latest/all/dotnet.html) (also included with Visual Studio 2019)
- [Standard Java SDKs](https://azure.github.io/azure-sdk/releases/latest/all/java.html) 
- [Standard Python SDKs](https://azure.github.io/azure-sdk/releases/latest/all/python.html) 
- [Standard TypeScript/JavaScript SDKs](https://azure.github.io/azure-sdk/releases/latest/all/js.html) 
- [Standard PHP SDKs](https://packagist.org/packages/microsoft/) 
- [Standard Go SDKs](https://docs.microsoft.com/en-us/azure/developer/go/azure-sdk-install) 
- [Azure SDKs Page](https://azure.microsoft.com/en-us/downloads/)

### Tools
- [Visual Studio 2019](https://visualstudio.microsoft.com/) has excellent integrations with Azure services, especially when using C#/.NET.
- [Azure Extensions for VSCode](https://code.visualstudio.com/docs/azure/extensions)
- [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) is a set of Azure tools for use in the command line.
- [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview)
- [Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/)
- [Azure Data Studio](https://docs.microsoft.com/en-us/sql/azure-data-studio/download-azure-data-studio?view=sql-server-2017)
- [Azure Tools Page](https://azure.microsoft.com/en-us/product-categories/developer-tools/)

## Tutorials
- [Microsoft Documentation Home](https://docs.microsoft.com/en-us/)
- [Getting Started with Azure](https://azure.microsoft.com/en-us/get-started/)
- [Microsoft Learn - Azure](https://docs.microsoft.com/en-us/learn/azure/)
- [Azure Documentation](https://docs.microsoft.com/en-us/azure/?product=featured)
- [Azure 15-Minute Tutorials](https://tutorials.visualstudio.com/)
- [Getting Started with Visual Studio](https://visualstudio.microsoft.com/vs/getting-started/)
- [Developing Azure Functions with Visual Studio](https://docs.microsoft.com/en-us/azure/azure-functions/functions-develop-vs)
- [Deploying Azure Applications with VSCode](https://code.visualstudio.com/docs/azure/deployment)
- [The Developer's Guide to Azure](https://azure.microsoft.com/en-us/campaigns/developer-guide/)

## Helpful Tidbits
### Take Advantage of Resource Groups
Most resources on Azure require supporting resources to be provisioned alongside the main offering. For example, provisioning a VM will also require a Storage Account, Virtual Networking, etc. All of a project's resources will be provisioned under the [resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal) that you specify at the time of creation. A resource group is a collection of related resources for a project. You can set permissions and other settings for the entire resource group instead of each resource individually. 

Remember to delete the **entire** resource group when deleting resources in order to avoid getting charged for vestigial resources.

### Take Note of Supported Protocols
The protocols that Azure supports for communicating with its services differs from that of AWS or GCP. Depending on the application you wish to build, the supported protocols may affect your architecture. Refer to the documentation for information specific to each service.

## Security and Permissions Management
### Role-Based Access Control (RBAC)
Access to Azure resources is managed through RBAC. RBAC allows you to define how to grant access. It can be done based on resource type, subscriptions, resource groups, or the specific resources themselves. Before continuing, see the [Azure documentation page on RBAC](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview). RBAC uses the concept of a **role assignment** to assign permissions. Role assignments have three parts.

- **Security Principals** are objects that represent the entity that must be granted permissions. There are four types of security principals: users, groups, service principals, and managed identities. See the [documentation page](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview#security-principal) for more info on what each type of security principal is for.

- **Role Definitions** are the actual set of permissions that will be granted to the security principal. Often simply called a **role**, the collection of role definitions covers everything from full administrative access to granular control over a specific service. Adding or removing definitions from the role controls what permission the principal has.

- **Scope** lets permissions be further constrained based on the resources in question. This allows for scenarios such as allowing a user to be a full admin over one resource group but no access to another. Thanks to this, granting a user the "Owner" permissions does not necessarily mean they will have owner access to the entire account.

Once again, refer to the [RBAC documentation](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview) for more detailed information on role assignements.

### Azure Active Directory (Azure AD)
Active Directory is a different from RBAC. While RBAC is focused on access management for cloud account users, Active Directory helps manage credentials and access for cloud applications and networks. Active Directory provides features like user management, user directories, reporting, password management and Single-Sign-On (SSO) services. It also supports using solutions like [OAuth](https://oauth.net/) to easily secure applications built with Azure. There are many more features and both free and numerous paid tiers for Active Directory. See the [Active Directory documentation](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-whatis) for information on how to use it.

Azure also provides many more tools and services for securing your applications. See the [Azure Security](https://docs.microsoft.com/en-us/azure/security/fundamentals/overview) page to get started.
