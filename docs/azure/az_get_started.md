# Getting started with Microsoft Azure

## Tenant / Azure AD directory

A `tenant` is a single instance of Azure Active Directory (Azure AD or AAD)
representing a flat structure of users and groups.

When a user signs up for a Microsoft cloud service, a new Azure AD tenant is created
and the user signing up automatically becomes a member of the Global Administrator role.

Each tenant can be associated with multiple Azure subscriptions. 
Each subscription on the other hand can only be linked with one tenant or Azure AD directory. 
A subscription trusts Azure AD to authenticate users, services, and devices.

@see [What is Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-whatis)

## Azure AD (AAD)

Azure Active Directory (`Azure AD`) is a multi-tenant, cloud based identity and access
management service. Azure AD is the backbone of the Microsoft 365 system and can be synched with 
on-premise installations of Active Directory.

Azure AD can be used as an authentication provider for other cloud-based systems via OAuth2.

Azure AD is the place to go when you want to manage 
* users
* groups
* roles

### REST APIs 
Azure AD uses Representational State Transfer (REST) APIs to support communication to other web-based services

### Authentication
Azure AD uses cloud-based authentication protocols like OAuth2, SAML, and WS-Security for user authentication

### Network Organization
Each Azure AD instance is called a “tenant” which is a flat structure of users and groups

### Entitlement Management
Admins organize users into groups, and then give groups access to apps and resources

### Devices
Azure AD provides mobile device management with Microsoft Intune

### Desktops
Windows desktops can join Azure AD with Microsoft Intune

### Servers
Azure AD uses Azure AD Domain Services to manage servers that live in the Azure cloud virtual machine environment

## Subscription

An Azure `subscription` is a logical container to provision resources like virtual machines,
storage accounts, and web apps in the Azure cloud.

Or in other words: a subscription is a customer's agreement with Microsoft that enables them to obtain Azure services. 

Each subscription must best associated with a single Azure AD tenant at any time, although
a subscription can be moved to another tenant.

Subscriptions are not tied to Azure Regions, so they may contain resources running in different
regions.

All billing is done on a per subscription base.

## Users and Groups

Users and groups are the basic building blocks for Azure AD. 
Users can be organized into groups that will all behave similarly. 
For example, you may put your Product Development team in one Azure AD group and grant permissions at the group level, so when users leave the organization, you only need to deactivate one account, and the rest of the group stays the same.

Your Azure AD can contain identities for users inside of your organization and users from outside your organization that have a Microsoft account.

There are several methods to populate your users and groups in Azure AD.

* Use Azure AD Connect to sync users from Windows AD to Azure AD. Most enterprises that already have Windows AD use this method.
* You can create users manually in the Azure AD Management Portal.
* You can script the process to add new users with PowerShell.
* Or you could program the process with the Azure AD Graph API.

## Role

A 'role' consists of a set of permissions.
Roles can either be assigned to users or to groups.

## Region

A `region` is a set of datacenters at a particular geolocation deployed within a latency-defined parameter

Azure regions come in two flavours:

* A `recommended region` provides the broadest range of service capabilities and is designed
to support availability zones.
* An `alternate region` provides a second region for disaster recovery needs and optimization
of latency, but is not designed to support availability zones.

@see [Regions and Availability Zones in Azure](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview)

## Availability Zones

An `availability zone` is a unique physical location within a region.
Each zone is made up of one or more datacenters equipped with independent power, cooling, and networking. 
To ensure resiliency, there's a minimum of three separate zones in all enabled regions.

Availability zones are a high-availability offering which is not present at all regions.

If availability zones are available in a region and the Azure service of your choice supports zones, you can distribute
your resources across two zones. How these zones are mapped to actual datacenters in a region may differ from
subscription to subscription.

Unlike in AWS, Azure subnets are not bound to specific availability zones, only resources hosted inside them.

@see [Regions and Availability Zones in Azure](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview)
@see [Regions that support Availability Zones in Azure](https://docs.microsoft.com/en-us/azure/availability-zones/az-region)
@see [Regions and zones on Azure and AWS](https://docs.microsoft.com/en-us/azure/architecture/aws-professional/regions-zones)

## Resource Groups

A `resource group` is a container that holds related resources for an Azure solution. 
It may contain all resources or just a part of the resources for a specific Azure solution.

Thus, resource groups can be used to subdivide a complex Azure solution into layered stacks:

* The `network` resource group of an Azure solution provisions all network components like VNets, subnets, NAT Gateways, Route Tables etc.
* The `compute` resource group of the same Azure solution provisions all virtual machines, volumes and all Azure services attached to them on top of the `network` resource group. 

Any resource group is linked to a particular [region](#region) and a particular [subscription](#subscription).

Most resources allocated on Azure must be attached to a resource group. 
Resources bound to a particular resource group may be moved to a different resource group even in a different subscription.
 
@see [What is a resource group](https://docs.microsoft.com/en-ie/azure/azure-resource-manager/management/manage-resource-groups-portal#what-is-a-resource-group)
@see [Terraform azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html)

## Stock Keeping Unit (SKU)

Represents a purchasable `Stock Keeping Unit` (SKU) under a product. These represent the different shapes of the product.

__Example:__

* Virtual machines come in different sizes regarding CPU, RAM, Network/IO etc. So the SKU of a virtual machine on Azure defines
the instance type. 
* Azure services come in different quality of service levels or SKUs: Basic, Standard, Premium. The SKU of a service
determines the price you have to pay for a service: the higher the SKU the higher the price. 