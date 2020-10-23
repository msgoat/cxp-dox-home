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

@see [Regions and Availability Zones in Azure](https://docs.microsoft.com/de-de/azure/availability-zones/az-overview)

## Availability Zones

An `availability zone` is a unique physical location with a region.
Each zone is made up of one or more datacenters equipped with independent power, cooling, and networking. 
To ensure resiliency, there's a minimum of three separate zones in all enabled regions.

Availability zones are a high-availability offering which is not present at all regions.

@see [Regions that support Availability Zones in Azure](https://docs.microsoft.com/de-de/azure/availability-zones/az-region)