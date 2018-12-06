# Azure Ramp-Up
A guide to get going fast on the Azure platform under editing with progress of learning path. 
Links, best-practices, explanantions and comments helpful before I started using Azure.

**DRAFT VERSION 0.1**

## Table of Contents

* [Fundamental Concepts](#fundamental-concepts)
    * [Basic Terminology](#basic-terminology)
    * [Environments](#environments)
    * [Regions](#regions)
    * [Authentication and Authorization](#authentication-and-authorization)
    * [Azure Resource Manager](#azure-resource-manager)
* [Developer Tooling](#developer-tooling)
* [Infrastructure as a Service (IaaS)](#infrastructure-as-a-service-iaas)
* [Platform as a Service (IaaS)](#platform-as-a-service-paas)
* [Software as a Service (SaaS)](#software-as-a-service-saas)
* [Open-Source Ecosystem](#open-source-ecosystem)
* [Learning Paths](#learning-paths)
* [Free Resources](#free-resources)
    * [Free eBooks](#free-ebooks)
    * [Webcasts](#webcasts)
    * [Official Whitepapers](#whitepapers)
    * [Recommended Links](#links)
    * [Twitter](#twitter)

## Fundamental Concepts
This chapter is about the the foundational building blocks of the Azure platform. 
This chapter will help you understand the basic terminology and concepts you will
need every day when working with Microsoft Azure. 
### Basic Terminology
- Azure Portal: A graphical user interface to manage and operate your cloud-based environment.
- Classic Portal: The old graphical user interface using the ASM deployment model (deprecated)
- Azure Environment: A strictly isolated part of the Azure Cloud Platform
- Azure Geography: A defined area of the world that contains at least one Azure Region 
- Azure region: A geographical location within a geography containing one or more Azure data-centers
- Azure Subscription: A manageable group of resources for the accounting department
- Azure Resource Group: A logical container associated with exactly one subscription that holds related resources for an Azure solution
- Microsoft account: A consumer account that has been created via [https://signup.live.com/](https://signup.live.com/)
- Work or school account: An account that has been created in an (Azure) Active Directory. Includes Office365 accounts.
- Azure Resource Manager (ARM): The modern deployment model in Azure.
- Azure Service Manager (classic): The classic deployment model. Do not use for new projects. 
- Azure Resource:  A manageable item that is available through Azure. Some common resources are a virtual machine, storage account, web app, database, and virtual network, but there are many more.

### Environments
Azure is comprised of currently four different so-called *environments* that are strictly isolated from each other. 
*Strictly isolated* means:
- They are operated and managed through [different endpoints](https://github.com/arafato/Azure-RampUp/blob/master/resources/endpoints.json) (same API interfaces, though)
- Their authentification mechanisms (Azure Active Directory) do not have a trust-relationship with each other. Thus, environments 
do not provide a single sign-on experience amongst each other  
- They are managed through distinct graphical user interfaces (Azure speak: portals) since a portal also needs to authenticate and operate against the different management and service endpoints


Azure currently provides the following environments:

| Azure Environment | Name to Use |
| --- | --- |
| International Cloud | `AzureCloud` |
| German Cloud | `AzureGermanCloud` |
| US Government Cloud | `AzureUSGovernment` |
| China Cloud | `AzureChinaCloud` |

where *Name to Use* is the name to be used in the context of our [Developer Tooling](#developer-tooling). 

**Gotcha**
>- The *Azure Cloud* environment is the only one which has a trust relationship with the
>Microsoft consumer identity system. This lets you signup and login with a Microsoft account
>(formely known as LiveID). This is also the reason why you cannot use a Microsoft account to
>signup and login into the other Cloud environments since these do *not* have a trust relationship
>with the Microsoft consumer identity system due to their restricted privacy and data regulations.
>- When using our Azure CLIs and SDKs be sure to configure them accordingly to let 
them talk to the correct environment (see section [Developer Tooling](#developer-tooling) for more information)

#### Azure Cloud
Also known as the International Cloud. Currently comprised of 30 regions world-wide.

Sign Up: [https://azure.microsoft.com/free/](https://azure.microsoft.com/free/) 

Portal: [https://portal.azure.com](https://portal.azure.com) 

Endpoints: are [listed here](https://github.com/arafato/Azure-RampUp/blob/master/resources/endpoints.json#L4-L10) for your convenience or can be programmatically fetched via   
`$ az cloud list --query "[?name == 'AzureCloud'].endpoints"`

#### Azure German Cloud
Also known as *Microsoft Cloud Deutschland*. Comprised of 2 regions, one located in 
Frankfurt, the other one in Magdeburg. Operated by T-Systems International GmbH, a 
subsidiary of Deutsche Telekom. Serves as trustee, protecting disclosure of data
to third parties except as the customer directs or as required by German law. 
Even Microsoft does not have access to customer data or the data centres 
without approval from and supervision by the German data trustee.
More information at [https://azure.microsoft.com/overview/clouds/germany](https://azure.microsoft.com/overview/clouds/germany)

Sign Up: [https://azure.microsoft.com/free/germany/](https://azure.microsoft.com/free/germany/)

Portal: [https://portal.microsoftazure.de](https://portal.microsoftazure.de)

Endpoints: are [listed here](https://github.com/arafato/Azure-RampUp/blob/master/resources/endpoints.json#L61-L67) for your convenience or can be programmatically fetched via  
`$ az cloud list --query "[?name == 'AzureGermanCloud'].endpoints"`

For further information on the German cloud, the data trustee model, and the data trustee agreement, please refer to the according resources in the [German Cloud](#german-cloud) section.

*Please note:* There are some changes occuring due to general European/data sovereignty.There are 2 more regions planned for Germany.

#### Azure China Cloud
Azure China Cloud is available through a unique partnership between Microsoft and 21Vianet, one of the country’s largest Internet providers.

Sign Up: [https://www.windowsazure.cn](https://www.windowsazure.cn)

Portal: [https://portal.azure.cn/](https://portal.azure.cn/)

Endpoints: are [listed here](https://github.com/arafato/Azure-RampUp/blob/master/resources/endpoints.json#L23-L29) for your convenience or can be programmatically fetched via  
`$ az cloud list --query "[?name == 'AzureChinaCloud'].endpoints"`

#### Azure Gov Cloud
Also known as *Microsoft Azure Government Cloud*. Comprised of 4 regions in USA. No public registration.
More information at [https://azure.microsoft.com/overview/clouds/government/](https://azure.microsoft.com/overview/clouds/government/) 

Trial Registration Form: [https://azuregov.microsoft.com/trial/azuregovtrial](https://azuregov.microsoft.com/trial/azuregovtrial)

Endpoints: are [listed here](https://github.com/arafato/Azure-RampUp/blob/master/resources/endpoints.json#L42-L48) for your convenience or can be programmatically fetched via  
`$ az cloud list --query "[?name == 'AzureUSGovernment'].endpoints"`

### Regions
The Azure platform is currently comprised of 54 regions world-wide with 10 regions announced.
A region is a geographical location of a cluster of Azure data-centers. Each region is assigned to one and only one environment.
More information at [https://azure.microsoft.com/regions/](https://azure.microsoft.com/regions/)

Azure regions are organized as so-called *Paired Regions*. Each Azure region is paired with another region within the same geography, together making a regional pair. 
The exception is Brazil South, which is paired with a region outside its geography.
More information at [https://docs.microsoft.com/azure/best-practices-availability-paired-regions](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)
#### Gotchas
>- When deploying new resources onto Azure you can select the region to deploy to, however, 
>as of now you cannot choose amongst the individual data-centers available in that region. 
>- Availability of Azure services depend on the region. Not every service that is *Generally Available* is available in every region.
>Please check services available by region at [https://azure.microsoft.com/regions/services/](https://azure.microsoft.com/regions/services/)

### Authentication and Authorization
Understanding Authentication in Azure can be complicated at the beginning. Most users
are confused about all the different pieces and terminologies such as *Azure AD, Tenant ID,
Account Owner, Subscription Owner, Subscription Admin, Directory Admin, Co-Admin, RBAC* etc. 
coming together. It doesn't help neither that
terms are used inconsistently throughout the web, and that Azure has two different
authentication models, depending on whether you are using ARM or ASM (see section [Azure Resource Manager](#azure-resource-manager)). 
We will focus on ARM which is the current model and the one you should use for every new project.

So let's provide clarity.

#### Short tale about two different authentication schemas
Before we look at the different aspects and services let's quickly define what we mean when we
refer to *Control Plane* and *Data Plane*.

>A **Control Plane** is the set of APIs that allow you to provision and configure a resource.  
>
>A **Data Plane** is the set of APIs that allow you to actually use the resource.

Example: In order to provision a Storage Account, I need to use a different set of APIs compared to when I want to actually store some data on it. Likewise, I'm using a different set of APIs when I want to
provision an EventHub compared to when I actually push data to it.  

Why is this important? Because Azure provides two different authentication mechanisms.
The one that is based on Azure AD (authentication) and RBAC (authorization) which we will
thoroughly discuss in the subsequent sections, and one that is based on *Shared Keys*.

Every operation on the **Control Plane** needs to be authenticated against Azure AD. Every operation. Period.
Not every operation against the **Data Plane**, however, needs to be also authenticated against Azure AD. Some
services such as [Azure Storage Service](https://docs.microsoft.com/azure/storage/storage-introduction),
[Service Bus](https://docs.microsoft.com/azure/service-bus/), and [Event Hubs](https://docs.microsoft.com/azure/event-hubs/) rely on so-called *Shared Keys*. 

So if you want to provision a Storage Account you will need to authenticate against Azure AD.
In order to read and write data from it (which are operations on the data plane) you are using *Shared Keys*. For a more detailed discussion on this topic with a strong focus on Azure Storage, we recommend the following link: [https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1)

Most services on Azure, however, rely on Azure AD and RBAC for managing the Control Plane *and* the Data Plane.

#### Azure Active Directory
Azure Active Directory (Azure AD) is Microsoft’s multi-tenant cloud based directory 
and identity management service (see [https://docs.microsoft.com/azure/active-directory/active-directory-whatis](https://docs.microsoft.com/azure/active-directory/active-directory-whatis)).
In Azure AD, a tenant is representative of an organization. It is a dedicated instance 
of the multi-tenant Azure AD service that an organization receives and owns when it signs up for a 
Microsoft cloud service such as Azure, Microsoft Intune, or Office 365. 
Each Azure AD tenant is distinct and separate from other Azure AD tenants.

An Azure AD tenant has always the following domain assigned **.onmicrosoft.com.* For example, 
if you sign up with your MS consumer account *joe.doe@outlook.com* an Azure AD tenant is 
automatically created for you similiar to `joedoeoutlook.onmicrosoft.com`. A user *Lilli* created in this directory would be referred to as lilli@joedoeoutlook.onmicrosoft.com. Of yourse, if you have your own domain, you can create a CNAME to `joedoeoutlook.onmicrosoft.com` so that users do not need to use these awkward-looking user-name accounts.

A tenant houses the users in a company and the information about them - their passwords, 
user profile data, permissions, and so on. It also contains groups, applications, 
and other information pertaining to an organization and its security.

There are two types of accounts you can use to sign in: a **Microsoft account** 
(formerly known as Microsoft Live ID) and a **work or school account**, which is an 
account stored in Azure AD. There is a federation relationship between
Azure AD and the Microsoft account consumer identity system. As a result, Azure AD is
able to authenticate "guest" Microsoft accounts as well as "native" Azure AD accounts, assuming that the Azure AD tenant is living in the International Cloud.

##### Gotchas
>- Account Overlap: It is not possible anymore to create a new personal Microsoft 
>account using a work/school email address, when the email domain is configured in Azure AD.
>See [https://blogs.technet.microsoft.com/enterprisemobility/2016/09/15/cleaning-up-the-azure-ad-and-microsoft-account-overlap/](https://blogs.technet.microsoft.com/enterprisemobility/2016/09/15/cleaning-up-the-azure-ad-and-microsoft-account-overlap/) for details

#### Subscriptions
An Azure subscription is just a manageable group of resources for the accounting department.
Every Azure subscription has a trust relationship with an Azure AD instance. 
This means that it trusts that directory to authenticate users, services, and devices. 
Multiple subscriptions can trust the same directory, but a subscription trusts one and only
one directory. 

This trust relationship that a subscription has with a directory is unlike the relationship
that a subscription has with all other resources in Azure (websites, databases, and so on),
which are more like child resources of a subscription. 
If a subscription expires, then access to those other resources associated with 
the subscription also stops. But the directory remains in Azure, and you can 
associate another subscription with that directory and continue to manage the 
directory users.

Please see [https://docs.microsoft.com/en-us/azure/active-directory/active-directory-how-subscriptions-associated-directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-how-subscriptions-associated-directory) for a more detailed discussion.
#### Understanding User and Role Management
First, let's define the terminology because this often leads to confusion.
Generally speaking, an indentity in Azure AD can be of two types: *Administrator* and *User*.

A *User* can only manage Azure resources such as VMs or Storage depending on the according access rights he has been granted through [RBAC](#role-based-access-control). He is not allowed to change any Azure AD properties. 

An *Administrator* can manage properties in Azure AD such as creating, deleting, and modifying users, and also - depending on his granted access rights through [RBAC](#role-based-access-control) - manage Azure resources.

When we talk about administrators in the context of Azure we usually 
do not refer to this *Administrator* type. Instead, people are usually using the
notion of *Azure Active Directory Admin* and *Azure Subscription Admin*. Both are, however, two separate concepts.

Let's examine both of them in more detail.

**Azure Active Directory Admin**  
Also known as *Account Admin* an Azure AD admin (*Administrator* type) can manage properties in the Azure AD like performing 
directory administration tasks using tools such as Azure AD PowerShell or 
Office 365 Admin Center. They have not necessarily access to the associated subscriptions. It is possible but this isn’t required.

An Azure AD Admin is always assigned a specific administrator role. To have access to all 
administrative features of Azure AD, an administrator needs to be assigned the so-called
*Global Administrator* role. Only those can also assign other administrator roles. 
Administrator that have not been assigned the *Global Administrator* role, are usually referred
to as *Limited Administrators*. Please see [https://docs.microsoft.com/en-us/azure/active-directory/active-directory-assign-admin-roles](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-assign-admin-roles) for
a detailed discussion and overview of the individual roles.  
Much of these roles, however, are not really relevant in the context of Azure. Note, that
Azure AD is used amongst a lot of different Microsoft products such as Office365, Dynamics365, 
Skype for Business, Intune, and many more. This is why there are so many more roles.
The ones that are usually relevant in the context of Azure are
- Global Administrator
- User account administrator
 
Typically, the account you are using for the initial sign up for an Azure account, is both
an Azure AD Admin (Global Administrator role) and an Azure Subscription Admin (see next section). But again, this is not required.

**Azure Subscription Admin**   
An Azure Subscription Admin (can be both an *Administrator* type or *User* type) is an identity that has been assigned 
an owner role on subscription level. 
That means it has has full access to all Azure resources including the right to delegate access to others.
Access Management in Azure is done via *Role-based Access Control (RBAC)* (see [next section](#role-based-access-control))
which lets you assign appropriate roles to users, groups, and applications at different scopes
such as subscription, resource groups, or a single subscription.

In the old ASM world the equivalent role is often referred to as *Service Administrator* or
*Co-Admin*, and unfortunately they are still used in the ARM world. Do not use them since 
they do not provide the same power and flexibility as the new concepts discussed here. 
In particular, they are lacking the entire *RBAC* functionality and force you to use the classic portal
if you need to make any changes.

So in essence an Azure Subscription Admin is only a specialization of a regular *User* type.
That is, someone with an *Owner* role at subscription level. We could also think about
an identity with only *Contributor* role on a certain resource group scope. From a conceptual
point of view both users do not differ except for their assigned roles and rights.

Now what exactly is this RBAC, Owner, and Contributor role all about? Next!

##### Role-based Access Control
Role-based Access Control (RBAC) is all about defining *what* your users are allowed to do.
For this purpose it provides built-in roles that you can use to assign to a user, groups, and
applications.  
Azure RBAC has three basic roles that apply to all resource types:
- **Owner** has full access to all resources including the right to delegate access to others.
- **Contributor** can create and manage all types of Azure resources but can’t grant access to others.
- **Reader** can view existing Azure resources.

The rest of the RBAC roles in Azure allow management of specific Azure resources. 
For example, the **Virtual Machine Contributor** role allows the user to create and 
manage virtual machines. It does not give them access to the virtual network or the 
subnet that the virtual machine connects to.    
A more detailed discussion can be found at [https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is](https://docs.microsoft.com/en-gb/azure/active-directory/role-based-access-control-what-is)

If you're looking to define your own roles for even more control, see how to build [Custom roles in Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).
Note, however, that custom roles are currently supported only via [Powershell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell), 
the [Azure CLI](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-azure-cli),
and the [REST API](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-rest).

Now let's talk about **Groups** and **Group Owners**.

A group in the RBAC-context provide a means to group users, and let them utilize the privileged access it grants.
From a conceptual point of view, they are really similiar to GNU/Linux groups for access control. 
What is interesting is, that you can assign a so-called **Group Owner** to a particular group. 
The membership of the group is now managed by the group owner. 
The resource owner effectively delegates the permission to assign users to the subscription, resource group, or resource 
to the owner of the group.  
A detailed discussion of this topic can be found at [https://docs.microsoft.com/azure/active-directory/active-directory-manage-groups](https://docs.microsoft.com/azure/active-directory/active-directory-manage-groups).

#### Authenticating an app or a script via Service Principals
Until now we only talked about authenticating real users, human beings. But what if your script
or application needs to authenticate itself (via certificates) in order to access Azure resources? 
Or if it needs to participate in Authentication Workflows such as OAuth2? 
This is where **Service Principals** come into play. Please refer to both links below in order
to learn more about this topic, and to learn how to set up a service principal.  
- [Application and service principal objects in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-application-objects)
- [Use portal to create Active Directory application and service principal that can access resources](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal)

Once you have setup a service principal, you can use it to make calls to the Azure APIs (see section [Resource Provider](#resource-provider) for details what we mean by the term *Azure APIs*) from a script or an application, for example. Our SDKs will do much of the heavy lifting regarding authentication, token handling, and so on. See next chapter to learn what's really going on under the hood. 

#### Authentication flow using service principals
Let's briefly look under the hood to better understand what's going on when an SDK authenticates a call. The overall process looks as follows:

1. We need to acquire an OAuth2 bearer token by authenticating ourselves against an identity provider such as Azure AD
2. We use this bearer token to sign our requests to authenticate against the relying party which is the Azure Management API in our case

The example assumes we are using the Internation Cloud. The use of another Azure cloud environment requires you to use the according endpoints of that respective environment. See section [Environments](#environments) for how to get them with our [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview).

 The JSON Web Token (JWT) can be acquired with a `POST` call to `https://login.windows.net/<tenantId>/oauth2/token`. The tenant id is your Azure AD tenant that is associated to your subscription, and where you have created a service principal. Think of `https://login.windows.net` as a landing page that forwards you to the correct identity provider depending on whether you are using a work or school account (re-directs you to your Azure AD tenant), or a Microsoft account (re-directs you the Microsoft consumer identity system).

 In our example we want a token that can be used to sign requests against the ARM API of the **International Cloud**. For this we need to specify the correct *audience* which is `https://management.core.windows.net`. We provide this information and our service principal credentials in the body of this request.

After having acquired this token we can add it to the Authorization Header of our HTTP-request against the service management API which is hosted at [https://management.azure.com/](https://management.azure.com/). See section [Resource Provider](#resource-provider) for a more detailed discussion on how this API works.

We recommend to take a look at this [easy to understand code example](https://github.com/arafato/funcy-azure/blob/master/lib/utils/ARMRest.js#L5-L36) (NodeJS) that walks the path we have just outlined on a high-level.

For a more detailed disucssion on different authentication scenarions we recommend the following links:  
- [https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-rest-api](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-rest-api)
- [https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-authentication-scenario](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-authentication-scenarios)

### Azure Resource Manager
Azure Resource Manager (ARM) is the recommended model for deploying and managing your applications on Azure. 
It is comprised of two parts:
- A set of APIs (accessible via REST, CLI, SDKs, and the Portal) that you use to 
provision and manage your resources. These APIs are offered by so-called **Resource-Providers**  
- A deployment model allowing you to group your resource into so-called **resource-groups**, and hence manage them as a unit, and to declaratively specify your resources in so-called **ARM-Templates** 

Let's take a deeper look into the various aspects.

#### Resource Provider
Think of a resource provider as a service that exposes various APIs for you to create and manage different kinds of Azure resources such as VMs, storage accounts, networking, Stream Analytics jobs, etc. There are a lot of different resource providers available, which you can query in the portal or with our new CLI as follows:
```
$ az provider list
```   
You can find the according output [here](https://github.com/arafato/Azure-RampUp/blob/master/resources/azure-resource-providers.json) for your convenience (last updated March 2nd 2017).

**Gotcha**
>- Our CLIs and SDKs are configured to talk to the *Internation Cloud* environment 
>by default. The above output thus lists only the resource providers available in
>the regions of the **International Cloud**. See section 
>[Developer Tooling](#developer-tooling) for how to configure them to use different
>Azure environments.

Resource providers are organized in namespaces such as `Microsoft.Compute`, `Microsoft.Storage`, or `Microsoft.StreamAnalytics`. We will use the term *namespace* and *resource provider* interchangeably. 

So for example, looking at the namespace `Microsoft.Compute`, you can find multiple *resource types*, each one listed with its available API versions and regions. A *resource type* represents an Azure resource such as a virtual machine, an availability set, a disk, or available locations. A resource type adheres to the following naming convention:

`<Namespace>.resourceType(/resourceType)*`  

That means resource types can be nested. For example: `Microsoft.Compute.virtualMachineScaleSets/virtualMachines/networkInterfaces`

This denotes the resource type of a network interfaces of virtual machines within a scale set.

Each of these resource types provide their own set of REST-full operations and APIs. Depending on the Azure environment (see section [Environments](#environments)) you are using, the management endpoint that hosts these APIs is different. 
For instance, the resource provider APIs of the **Internation Cloud** are hosted at `https://management.azure.com`, the APIs of the **German Cloud** are hosted at `https://management.microsoftazure.de`).

Example: In order to talk to the REST API of a virtual machine named `myVM` in the resource group `myrg` in subscription `8d4dee44-4b28-4e05-9927-3a5d34a42bf5` in the **Internation Cloud** you would call

`https://management.azure.com/subscriptions/8d4dee44-4b28-4e05-9927-3a5d34a42bf5/resourceGroups/myrg/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2016-03-01`

You will find the official Azure REST API reference here: [https://docs.microsoft.com/azure/templates/](https://docs.microsoft.com/azure/templates/).
If you know the resource type, you can go directly to it with the following URL format: `https://docs.microsoft.com/azure/templates/{provider-namespace}/{resource-type}`. For example, the SQL database reference content is available at: [https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases)

Note that most Azure service REST APIs have a corresponding client SDK library, which handles much of the client code for you. We will take a more detailed look at these SDKs, and how to configure them accordingly in the [Developer Tooling](#developer-tooling) section.

#### Resource Groups
A resource group is a container that holds related resources for an Azure solution. The resource group can include all the resources for the solution, or only those resources that you want to manage as a group. You decide how you want to allocate resources to resource groups based on what makes the most sense for your organization. 

Note, that you need to assing a location to a resource group. The individual resources within a resource group can, however, be deployed into different regions.

Usually, it is recommended to group those resources together that have the same lifecycle (web application servers in one resource group, database servers in another). 

You will find a more detailed discussion on this topic at [https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups)

>**Gotcha**
>- The resource group stores metadata about the resources. Therefore, when you specify a location for the resource group, you are specifying where that metadata is stored. For compliance reasons, you may need to ensure that your data is stored in a particular region

#### ARM Templates
An ARM-Template is a JavaScript Object Notation (JSON) file that defines one or more resources to deploy to a resource group. It also defines the dependencies between the deployed resources. The template can be used to deploy the resources consistently and repeatedly in a so-called *declarative syntax* meaning "Here is what I intend to create". This frees you to think about the sequence of programming commands you have to use.

Using ARM-Templates to provision your resources is the recommended way, and defintely a best-practice. 

ARM-Templates also provide the possibility to use [numeric and string functions](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) increasing their expressiveness, and also a means to split them into smaller units through so-called [Linked Templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-linked-templates).

We recommend the following links to learn more about how to author them, what to consider, and making them world-class ARM-Templates.
- [Authoring Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)  
A gentle introduction to the structure of ARM-Templates

- [Resource Manager template walkthrough](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-template-walkthrough)  
An end-to-end walkthrough based on a real example  

- [Azure Quickstart ARM-Templates](https://github.com/Azure/azure-quickstart-templates)  
A huge collection of example ARM-Templates that will help you get going fast. Ranging from simple Linux or Windows based VMs, to complex Elastic Search, Cassandra, and Zookeeper setups. 

- [https://docs.microsoft.com/azure/templates](https://docs.microsoft.com/azure/templates)  
The official Azure REST API reference. If you know the resource type, you can go directly to it with the following URL format: `https://docs.microsoft.com/azure/templates/{provider-namespace}/{resource-type}`. For example, the SQL database reference content is available at: [https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases)

- [Free Ebook: World-class ARM-Templates - Considerations and Proven Practices](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World%20Class%20ARM%20Templates%20-%20Considerations%20and%20Proven%20Practices.pdf)   
Over 60 pages full of best-practices and design considerations

##### Round-Trip Engineering
Sometimes you will hear that ARM-Templates support Round-Trip Engineering. This is true in that the internal representation of an Azure resource refers to the same JSON schema as an ARM-Template. You will find the JSON schemas at [https://github.com/Azure/azure-resource-manager-schemas](https://github.com/Azure/azure-resource-manager-schemas)

So in essence this enables you to look at the JSON description of already deployed resources, copy / adapt them to your ARM-Templates, re-deploy, and repeat if needed. Just how can you look at the JSON description of already provisioned resources?
There are multiple ways to do that:
- In the Azure portal you can export the JSON representation of an entire resource group. You'll find that button in the according resource group blade under `Settings - Automation Script`

- In the Azure portal search for `Resource Explorer` under `More Services`

- Visit [https://resources.azure.com](https://resources.azure.com) for an explorer that is similar to the one in the Azure portal, however, providing more features such as calling available REST APIs directly from the web interface  

>*Gotcha*
>- The internal JSON representation of an Azure resource will usually include many more attributes than your original ARM-Template, since the internal representation is explicit. Many attributes have standard values that you often do not explicitly set or are only set and known at provision time such as internal domain name suffixes of your network interface cards. 

##### Tool Support
Writing complex infrastructure setups "by hand" is possible but sometimes people end up writing their own ARM-Template generation tools to speed up things. According projects are still in its infancy, however, support is growing. We'll take a look at these kind of projects in section [Open-Source Ecosystem](#open-source-ecosystem).

There is, however, already intellisense support for a wider range of different editors. We will look at them in section [Developer Tooling](#developer-tooling).

Also there is a [free and commercial plugin](http://t4-editor.tangible-engineering.com/T4-Editor-Visual-T4-Editing.html) available for Visual Studio made by a German-based company [Tangible Engineering](http://www.tangible-engineering.com) that allows you to derive your ARM-Templates from a graphical model.

## Developer Tooling
This section will discuss the different means to programmatically talk to Microsoft Azure, and the tooling that is provided by us and third party vendors.

### Talking to Microsoft Azure
Essentially, Microsoft Azure provides 4 different means to talk to the platform:
- Azure Portal
- Command Line Interface (CLI)
- SDKs for various programming languages
- REST API

Let's look at each of them and discuss how to use and configure them.

>**Gotcha**
>- Do not assume (yet) that there is feature parity between all four ways to talk to Microsoft Azure. Unfortunately, there are features that are available via the Portal that might not be available via our SDKs, or available via CLI but not via the Portal. This is a fast moving target so be sure to consult the according documentation on whether the feature is supported. Every feature, however, is available via our [REST API](https://docs.microsoft.com/rest/api/resources/).   
>
>- As a rule of thumb, assume that per default, all our tools usually use the endpoints of the International Cloud.

### Azure Portal
While not being a programmatic means to talk to Azure, for many it is often the first starting point. Keep in mind that every [Azure Environment](#environments) has its dedicated portal running at a dedicated URL, requiring you to use separate login credentials, stored in [Azure Active Directory Tenants](#azure-active-directory) tied to the according environment. No single sign-on experience is provided across different Azure Environments since they do not have a trust-relationship amongst each other.

Please refer to the section [Azure Environments](#environments) to get an overview, under which URL an according portal for a given environment can be found. 

### Command Line Interface (CLI)
Azure is currently providing two different cross-platform CLIs:
- The old one based on NodeJS, still supporting the old ASM and the new ARM model. It is available via [Node Package Manager (NPM)](https://www.npmjs.com/package/azure-cli), and hosted on [Github](https://github.com/Azure/azure-xplat-cli).
- The new one often referred to as **Azure CLI 2.0**, written in Python, and only supporting the new ARM model. It is also hosted on [Github](https://github.com/azure/azure-cli). For installing instructions please refer to our [install guide](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

We strongly recommend to use the new Azure CLI 2.0. For the rational behind introducing a new CLI, please refer to the official [announcement](https://azure.microsoft.com/en-us/blog/announcing-azure-cli-2-preview/). We will refer to new Azure CLI 2.0 for the remainder of this section.

**Overview and Documentation:**  
[https://docs.microsoft.com/en-us/cli/azure/overview](https://docs.microsoft.com/en-us/cli/azure/overview).

**Installation**  
[https://docs.microsoft.com/en-us/cli/azure/install-azure-cli](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)

**Environment Configuration**   
To change the environment to be used, type  

`$ az cloud set --name <environment_name>`   

where `<environment_name>` is any of our [currently available Azure Environments](https://github.com/arafato/Azure-RampUp/blob/master/resources/environments.md).

>**Gotcha**
>-  If you use both CLIs, remember that `azure` is the old CLI, and that `az` is the new Azure CLI 2.0.


### SDKs and Tools
You can get an overview about our officially supported SDKs and tools at  
[https://docs.microsoft.com/en-us/azure/#pivot=sdkstools&panel=sdkstools-all](https://docs.microsoft.com/en-us/azure/#pivot=sdkstools&panel=sdkstools-all)

Our officially supported SDKs are all open-source and available on Github. In the remainder of this section we will sho how to configure these SDKs for use with different Azure environments other than the International Cloud. In particular, we will demonstrate how to do a interactive logins, logins via service principals, a call to the control plane, and a call to the data plane via shared access keys. See section [Short Tale About Two Different Authentication Schemas](https://github.com/arafato/Azure-RampUp#short-tale-about-two-different-authentication-schemas)  for details on control- and access planes, and shared access keys.
#### .NET SDK
- [Source code on Github](https://github.com/Azure/azure-sdk-for-net)

- [Currently supported Azure Environments and names to use](https://github.com/Azure/azure-sdk-for-net/blob/cbcb6e489788c58b6edb849ae49bfb398de20dbd/src/Authentication/Common.Authentication/Models/AzureEnvironment.Methods.cs#L71-L143)



#### Java SDK
- [Source code on Github](https://github.com/Azure/azure-sdk-for-java)

- [Currently supported Azure Environments and names to use](https://github.com/Azure/azure-sdk-for-java/blob/fd4ce079a84c662d8a11e23a933fb4d7a33baa4e/runtimes/azure-client-runtime/src/main/java/com/microsoft/azure/AzureEnvironment.java#L63-L94)

- [Login using and authentication file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md)

- [Service principal login](https://github.com/arafato/Azure-RampUp/blob/master/resources/sdk/javaSDK_Login.java#L15-L26)

- [Azure Active Directory login](https://github.com/arafato/Azure-RampUp/blob/master/resources/sdk/javaSDK_Login.java#L31-L39)

#### NodeJS SDK
- [Source code on Github](https://github.com/Azure/azure-sdk-for-node):   

- [Currently supported Azure Environments and names to use](https://github.com/Azure/azure-sdk-for-node/blob/master/runtime/ms-rest-azure/lib/azureEnvironment.js#L48-L124)    

- [Azure Environment Configuration](https://github.com/arafato/Azure-RampUp/blob/master/resources/sdk/nodejs_env.js)  
Sample on how to configure this SDK to use a specific Azure environment (e.g. German Cloud).
Per default, it uses the International Cloud.


#### Python SDK
- [Python SDK](https://github.com/Azure/azure-sdk-for-python)

- [Currently supported Azure Environments and endpoint URLs to use](https://github.com/arafato/Azure-RampUp/blob/master/resources/sdk/Python_Endpoint_URLs.md)

- [Azure Environment Configuration](https://github.com/Azure/azure-sdk-for-python/blob/e92b5eda3eaea20541fd582305f6c3c30bae3a4c/doc/multicloud.rst)  
Sample on how to configure this SDK to use a specific Azure environment (e.g. German Cloud).


#### Ruby SDK
- [Ruby SDK](https://github.com/Azure/azure-sdk-for-ruby)

- [Currently supported Azure Environments and names to use](https://github.com/Azure/azure-sdk-for-ruby/blob/aa02e9b930a44908c8a61af9d2de9e017cd0550d/runtime/ms_rest_azure/lib/ms_rest_azure/azure_environment.rb#L87-L161)



#### PHP SDK
- [PHP SDK](https://github.com/Azure/azure-sdk-for-php)

- Currently supported Azure Environments and names to use:
The user has to provide the environment endpoint URL. 



#### iOS SDK
[iOS SDK](https://github.com/Azure/azure-mobile-apps-ios-client)

#### Android SDK
[Android SDK](https://github.com/Azure/azure-mobile-apps-android-client)

#### Golang SDK
Work in Progress, not yet officially supported:
[Go SDK](https://github.com/Azure/azure-sdk-for-go)

This repository is under heavy ongoing development and is likely to break over time. We currently do not have any releases yet. 

#### Azure IoT SDKs
You will find an overview about all currently supported Azure IoT SDKs on this landing page:

[https://github.com/Azure/azure-iot-sdks#microsoft-azure-iot-sdks-1](https://github.com/Azure/azure-iot-sdks#microsoft-azure-iot-sdks-1)


### REST API
TODO
[https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-rest-api](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-rest-api)

https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli
TODO: Portal, CLIs, SDKs, IDEs and according configuration

- [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/)

## Infrastructure as a Service (IaaS)
TODO: VMs, Storage, VNET, Availability Set, Managed Disks
https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-sizes

## Platform as a Service (PaaS)
TODO: App Service 

>**Gotcha**  
>https://blogs.msdn.microsoft.com/benjaminperkins/2016/03/02/how-to-find-you-outgoing-azure-app-service-ip-address/

## Software as a Service (SaaS)
TODO: Azure Marketplace

## Open Source Ecosystem
This chapter is about third party OSS projects in the context of Microsoft Azure.
### Compute
- [Serverless Framework](https://serverless.com/)  
The Serverless Framework allows you to deploy auto-scaling, pay-per-execution, event-driven functions to any cloud. 
It currently supports **Microsoft Azure**, AWS Lambda, Apache OpenWhisk, and is expanding to support other cloud providers.

### Storage
- [Azurite](https://github.com/arafato/azurite)  
A lightweight server clone of Azure Blob Storage that simulates most of the
commands supported by it with minimal dependencies. Written in NodeJS.
- [S3Proxy](https://github.com/andrewgaul/s3proxy)  
AWS S3 Proxy written in Java with support for Azure Blob Storage and many other storage backends.
- [BlobPorter](https://github.com/Azure/blobporter)  
A high throughput blob copier, optimized for large files. Written in Golang.

### DevOps
- [Proto](https://github.com/hoffmann/tropo)  
A Python library to generate Azure Resource Manager (ARM) Templates. Kudos to [Peter Hoffmann](https://twitter.com/peterhoffmann) from [BlueYonder](https://twitter.com/BlueYonderTech) for maintaining this project.

## Learning Paths
The following link points you to an overview listing all available learning paths we
are currently offering for the Azure platform.
Use these learning paths to guide yourself through the documentation for our services so you can start to build effective cloud applications on Azure.  

These learning paths do not encompass all of our services currently available. For many use-cases, however, we think that the most relevant are covered. 

[https://azure.microsoft.com/documentation/learning-paths/](https://azure.microsoft.com/documentation/learning-paths/)

## Free Resources
### Free Ebooks
- [https://azureinfohub.azurewebsites.net/MediaType?mediaType=Ebook](https://azureinfohub.azurewebsites.net/MediaType?mediaType=Ebook)  
A comprehensive and curated list of free Azure-related eBooks. 
A big thank you goes out to [Holger Sirtl](https://twitter.com/hsirtl) who maintains this site.
- [Überblick über Microsoft Azure (German only)](http://aka.ms/azureoverview)  
A great resource to get an overview about our broad service offering by [Holger Sirtl](https://twitter.com/hsirtl), including Analytics, IoT, Security, Networking, DevOps, Storage, Web Applications, and Media Services.  

### Webcasts
- [Azure Friday](https://azure.microsoft.com/resources/videos/azure-friday/)  
Join Scott Hanselman every Friday as he engages one-to-one with the engineers who 
build the services that power Microsoft Azure as they demo capabilities, 
answer Scott’s questions and share their insights.
- [Microsoft Virtual Academy - Azure Courses](https://mva.microsoft.com/training-topics/cloud-app-development#!index=2&prod=Microsoft%20Azure&jobf=Developer$IT%20Professional&lang=1033)  
Free Microsoft Azure trainings delivered by experts, ranging from our IaaS to PaaS Offerings such as App Service, Azure Search, Nano-Services with Azure Functions, and Microservice with Docker on Azure Container Service. 

### Whitepapers
- [Azure Info Hub - Whitepapers](https://azureinfohub.azurewebsites.net/MediaType?mediaType=Whitepaper)  
A comprehensive and curated list of free Azure-related Whitepapers. 
A big thank you goes out to [Holger Sirtl](https://twitter.com/hsirtl) who maintains this site.
- [Azure Whitepapers by David Chappell](http://www.davidchappell.com/writing/white_papers.php)  
Great high-level and conceptual explanations of Azure-related services and technologies by David Chappell
- [Securing the Microsoft Cloud](http://download.microsoft.com/download/D/5/E/D5E0E59E-B8BC-4D08-B222-8BE36B233508/Securing_Microsoft_Cloud_Strategy_Brief_.pdf)
- [Azure Security, Privacy, and Compliance](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
- [Security Management in Microsoft Azure](http://download.microsoft.com/download/7/0/E/70E3858E-8764-4233-A00F-49A3C6C3143C/Security_Management_in_Microsoft_Azure-11062014.pdf)
- [Cloud Operations Excellence and Reliability Strategy Paper](http://download.microsoft.com/download/C/5/5/C55C7170-9AA0-4187-9A78-C5AE85C8161D/Cloud_Infrastructure_Operational_Excellence_and_Reliability_Strategy_Brief.pdf)
- [Leveraging Stored Energy for Handling Power Emergencies](http://download.microsoft.com/download/3/1/9/319FE711-93A7-462D-9681-DB7F2E5875CC/Leveraging_Stored_Energy_for_Handling_Power_Emergencies_White_Paper.pdf)
- [Resilience by Design for Cloud Services](http://download.microsoft.com/download/7/8/7/78716493-0876-4CDF-8595-F5EEE13FD0E1/Resilience_by_Design_for_Cloud_Services_Strategy_Brief.pdf)
- [Information Security Management](http://download.microsoft.com/download/A/0/3/A03FD8F0-6106-4E64-BB26-13C87203A763/Information_Security_Management_System_for_Microsofts_Cloud_Infrastructure.pdf)
- [Where and How your Data is Stored](https://www.microsoft.com/en-us/trustcenter/about/transparency)

### German Cloud
- [Whitepaper German Trustee Concept (German only)](https://github.com/arafato/Azure-RampUp/blob/master/resources/data_trustee_mcd_de.pdf)  
An in-depth view at the German Cloud and the data trustee model.
- [Official Customer Data Trustee Agreement (DE)](http://microsoftvolumelicensing.com/Downloader.aspx?DocumentId=11453)
- [Official Customer Data Trustee Agreement (EN)](http://microsoftvolumelicensing.com/Downloader.aspx?DocumentId=11452)  
The Customer-Data Trustee Agreement is a legally separate contract between Customer and Data Trustee. Microsoft is not a party to this agreement. 

### Links
- [Official Azure Blog](https://azure.microsoft.com/blog/)  
Great resource to get informed about all new services and feature announcements
- [Official Azure Documentation](https://docs.microsoft.com/azure)  
Extensive documentation about Azure services, SDKS/Tools, and architectural best-practices
- [Azure Info Hub](https://azureinfohub.azurewebsites.net/)
A curated Website full of all sorts of information regarding Microsoft Azure. From querying Azure Feature Announcements, lists of different Webcasts, ebooks, whitepapers, and tools. Must-know resource for everyone building applications with Azure.
- [Official Microsoft Azure Github Repository](https://github.com/Azure)  
APIs, SDKs and open source projects from Microsoft Azure (350+ projects) 
- [Azure Networking and ARM FAQ by Igor Pagliai](https://blogs.msdn.microsoft.com/igorpag/2016/07/24/my-personal-azure-faq-on-azure-networking-and-arm/)  
A little bit outdated but still relevant and highly informative FAQ that goes really deep on Networking and ARM topics
- [A curated list of Azure Bookmarks](https://blogs.technet.microsoft.com/tangent_thoughts/2016/02/04/bookmark-this-aka-msazureshortcuts/)  
Links to follow-up Azure articles ranging from topics such as App Service Environments, 
Availability Best-Practices, Blobs, Data Lake Store, all the way up to Security, Trainig, VNETs and WebHooks.

### Twitter
We recommend the following Twitter handles to follow to be first to know if 
something mind-boggling is happening in the Azure universe.
- [Microsoft Azure](https://twitter.com/Azure)  
The official account for Microsoft Account. Follow for news and updates from the #Azure team and community.
- [Corey Sanders](https://twitter.com/CoreySandersWA)  
Director of Program Manager responsible for IaaS and Cloud Services. 
- [Mark Russinovich](https://twitter.com/markrussinovich)  
CTO of Microsoft Azure
- [Scott Guthrie](https://twitter.com/scottgu)  
Runs the Cloud & Enterprise division at Microsoft
