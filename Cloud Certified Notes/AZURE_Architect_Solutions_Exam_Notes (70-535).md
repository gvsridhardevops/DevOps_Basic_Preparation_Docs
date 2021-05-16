# Architecting Microsoft Azure Solutions (70-535)
Disclaimer: I passed the 70-535 exam with a score of 800! As a result I won't be maintaining this anymore. Feel free to create a pull request. Another disclaimer I want to make is that all of the content is available in public domain, so this repository is just a collection for easy reference.

### Azure Geos and Regions
- 42 regions globally
- Geos: East US, Canada Central, North Europe.
- Each region has a pair for highest-speed connections and highest availability.
- Availability zones:
  - An Availability Zone is a physically separate zone within an Azure region. There are three Availability Zones per supported Azure region. 
  - Each Availability Zone has a distinct power source, network, and cooling, and is logically separate from the other Availability Zones within the Azure region.
- VNet Peeting: Global VNet Peering enables resources in your virtual network to communicate across Azure regions privately through the Microsoft backbone. VMs across virtual networks can communicate directly without gateways, extra hops, or transit over the public internet.

### Azure Virtual Networks
- IP Addresses:
  - Difference between public and private.
  - Dynamic assigned.
  - You can point DNS using CNAME records. 
  - Static IP, allows A records to be set. SSL security. Useful for firewall settings. 4096 Private IP's per Vnet. 60 Public dynamic IP's, 20 Status public IP's
  - The first and last IP addresses of each subnet are reserved for protocol conformance, along with the x.x.x.1-x.x.x.3 addresses of each subnet, which are used for Azure services.
- Network Security Groups (NSGs):
  - can contain list of ACL rules.
  - Allow or denies access to resources based on priority.
  - Associated with vnets, subnets, vm's.
  - When Associated with a subnet the ACL is applied on that subnet.
  - Can only be applied within a region.
  - Service Tags simplifies the security definition for Virtual Networks, so you can define larger, more complex network security policies with fewer rules, based on tags, e.g. Storage, Sql, and AzureTrafficManager tags.
- Resource groups: 
  - Allows to operate on a group of resources.
  - Allows access control, billing, in-group communication.
  - Resource can only be part of one group.
  - No nesting is allowed. You can link however.
  - No limits.
  - Unable to rename after creation.
- Ingress/Egress:
  - Ingress: Data inbound to Azure. No charges for ingress.
  - Egress: Data outbound to Azure. There are charges for egress, unless using a metered ExpressRoute connection.
- Virtual Network (VNet) service endpoints:
  - Extends your virtual network private address space and the identity of your VNet to the Azure services, over a direct connection.
  - Traffic from your VNet to the Azure service always remains on the Microsoft Azure backbone network, so no exposure to the outside world.

### Azure Compute
- Azure Compute:
  - Azure App Service (web apps, mobile apps, logic apps, api apps).
  - In App Service, an app runs in an App Service plan. An App Service plan defines a set of compute resources for a web app to run.
  - Different tiers:
    - Shared compute: Free and Shared, the two base tiers, runs an app on the same Azure VM as other App Service apps, including apps of other customers. These tiers allocate CPU quotas to each app that runs on the shared resources, and the resources cannot scale out.
    - Dedicated compute: The Basic, Standard, Premium, and PremiumV2 tiers run apps on dedicated Azure VMs. Only apps in the same App Service plan share the same compute resources. The higher the tier, the more VM instances are available to you for scale-out.
    - Isolated: This tier runs dedicated Azure VMs on dedicated Azure Virtual Networks, which provides network isolation on top of compute isolation to your apps. It provides the maximum scale-out capabilities.
    - Consumption: This tier is only available to function apps. It scales the functions dynamically depending on workload. For more information, see Azure Functions hosting plans comparison.
  - Azure App Service Environment is an Azure App Service feature that provides a fully isolated and dedicated environment for securely running App Service apps at high scale.
    - An ASE is dedicated exclusively to a single subscription and can host 100 App Service Plan instances. The range can span 100 instances in a single App Service plan to 100 single-instance App Service plans, and everything in between.
- Web Apps:
  - Azure App Service Web Apps = Web Apps
  - Is a service for hosting web applications, REST APIs, and mobile back ends. 
  - Shared or dedicated virtual machines, managed, any language, ASP.net, NodeJS, PHP or Python.
  - Supported languages: NodeJs, Java, PHP, .NET Core, Ruby, Go, Apache Tomcat
  - Powershell as scripting.
  - CI/CD capabilities and security options.
  - The standard pricing tier allows unlimited number of apps.
  - Limited only by CPU, Storage and RAM.
  - In fact, all tiers at Basic or above are unlimited.
  - Deployment options: FTP, Local Git, GitHub, Bitbucket
- Mobile Apps: 
  - Supports any language with the Azure SDK.
  - Supports single sign on via Azure AD FS.
  - Build offline ready apps.
  - Supports push notifications. 
  - Supports staging environments.
- API Apps:
  - Easy migrate existing API's
  - supports CORS
  - Access control and supports integration with Logic Apps. 
- Logic Apps:
  - Logic Apps helps you build, schedule, and automate processes as workflows so you can integrate apps, data, systems, and services across enterprises or organizations.
  - Build workflows.
     - Trigger based (If this then that), causes actions to happen. 
     - Works with templates. 
     - Can be created in the browser.
- Virtual Machines: 
  - Virtual Machines which come in many different flavors and types.
  - Microsoft limits you to 10,000 VMs in a single subscription.
  - Scale-up: make it bigger
  - Scale-out: add more instances
  - Types:
    - A = general purpose VM's with HHD temporary drive
    - D = SSD temporary drive and faster processor than A
    - F = Compute uptimized with 2GB of RAM and 16GB of SSD per core
    - G = Performance compute with massive scale and SSD temporary drives
    - H = High performance VM's
    - Ls = Storage optimized with high throughput and I/O
    - N = GPU capabilities for graphical intensive workloadsac
  - Stopped: The virtual machine is no longer running, but you are still paying for it.
  - Deallocated: You are no longer charged for the VM. Can use the API or Portal to deallocate the VM.
  - Availability Sets:
    - An availability set is a logical grouping of VMs within a datacenter that allows Azure to understand how your application is built to provide for redundancy and availability.
    - Availability sets ensure that the VMs you deploy on Azure are distributed across multiple isolated hardware nodes in a cluster.
    - At least two instances are deployed to meet the 99.95% Azure SLA.
    - Do NOT mix workloads in an Availability Set.

### VPN & Express Route
- P2S Point-to-Site VPN: 
  - VPN connection for single connection or single computer.
  - Useful for development or debugging.
  - Uses the Public Internet. 
- S2S Site-to-Site VPN:
  - VPN connection for multiple computers. 
  - Requires a Gateway.
  - Network is secure encrypted.
  - Not suited for huge amount of traffic.
  - Uses the Public Internet and speed is limited. Up to 200mbs speed.
- ExpressRoute:
  - Enterprise dedicated connection.
  - Not via the internet.
  - Up to 10GB speed.
  - Offers dedicated servers for for storage, backup, and recovery, as if it runs in your own data center.

### Azure Services & Solutions:
- Azure Load Balancer:
  - Load Balancer to balance the web traffic.
  - You can also reach a load balancer front end from an on-premises network in a hybrid scenario. Both scenarios use a configuration that is known as an internal load balancer.
  - Checks with 15 seconds interval is servers are still alive.
  - Allows Port forward traffic to a specific port on specific VMs with inbound network address translation (NAT) rules.
  - Azure Load Balancer works at layer-4, or transport level.
  - Stickyness, uses 5 tuples (source IP, destination IP, protocol (tcp/udp), source port and destination port) so that user's traffic is always routed to same machine.
    - SourceIP (2 tuples): source and destination IP only
    - SourceIPProtocol (3 tuples): source, destination IP and also protocol
- Internal Load Balancer:
  - Load Balancer to balance internal traffic.
  - Allows different ports and security configuration options.
- Azure Application Gateway:
  - Azure Application Gateway is a web traffic load balancer that enables you to manage traffic to your web applications.
  - Gateway that has a WAF (Web Application Firewall) to inspect traffic.
  - Application Gateway you can be even more specific. For example, you can route traffic based on the incoming URL. So if /images is in the incoming URL, you can route traffic to a specific set of servers (known as a pool) configured for images.
  - Is able to perform robin traffic.
  - Allows Secure Sockets Layer-offload (SSL).
  - Cookie-based session affinity feature is useful when you want to keep a user session on the same server. 
  - Can route traffic based on content inspection.
  - Azure Application Gateway is a layer-7 (application layer) load balancer.
- Traffic Manager:
  - Microsoft Azure Traffic Manager allows you to control the distribution of user traffic for service endpoints in different datacenters. 
  - Manages and balances the load between different regions.
  - Service endpoints supported by Traffic Manager include Azure VMs, Web Apps, and cloud services.
  - You can also use Traffic Manager with external, non-Azure endpoints.
  - Traffic Manager is not a proxy or a gateway. Traffic Manager does not see the traffic passing between the client and the service.
  - Works at DNS level. Traffic Manager uses the Domain Name System (DNS) to direct client requests to the most appropriate endpoint based on a traffic-routing method and the health of the endpoints. 
  - Uses different routing methods:
    - Performance: based on the DNS location of the user
    - Priority: primary and failover targets
    - Weighted: Round-robin with weights
    - Geographic: Target based on geo-location
- Azure Media Services:
  - Does encoding and decoding of media.
  - Useful for websites that work with video and video-streaming content.
- Azure Content Delivery Network (CDN):
  - A content delivery network (CDN) is a distributed network of servers that can efficiently deliver web content to users.
  - CDNs store cached content on edge servers in point-of-presence (POP) locations that are close to end users, to minimize latency.
  - Serves out static content from much faster locations.
- Azure Active Directory:
  - Identity management
- Microsoft Azure Redis Cache:
  - Caching engine using Redis.
  - Useful for frequently accessed content or sessions.
  - If you want to create caches larger than 53 GB or want to shard data across multiple Redis nodes, you can use Redis clustering which is available in the Premium tier.
  - Import/Export is an Azure Redis Cache data management operation which allows you to import data into Azure Redis Cache or export data from Azure Redis Cache by importing and exporting a Redis Cache Database (RDB) snapshot from a premium cache to a page blob in an Azure Storage Account. 
- Azure Multi-Factor Authentication: 
  - Additional Authentication with mobile phone (sms), mobile app or telephone call
- Azure Service Bus: 
  - message queue which can be used to decouple applications.

### Managed Identities
- Azure Active Directory (AD):
  - Azure AD Domain Services: is the managed identify infrastructure services, instead of doing AD in a Azure VM yourself.
  - Multi-tentant, which means it facilitates groups of users with common access rights.
  - Typically used together with SaaS.
  - Enables and allows Single Sign On (SSO) to any cloud or on prem app.
  - Has a Free, Basic and Premium plan.
  - Active Directory Connect is what Microsoft recommends to synchronize an on prem AD instance and an Azure Active Directory instance. Alternative is to use federation and password synchronisation.
  - Users and Groups are supported.
  - By default no Kerberos is enabled.
  - Authentication via protocols such as SAML, WS-Federation and OAuth.
  - Uses the Graph REST API
  - Management is done via Powershell or the Portal
  - Azure AD MFI: Multi factor authentification, e.g. use an additional mobile phone for authentification.
  - Azure AD P2: Privileged Identity Management.
    - Combination of groups, roles, MFI and temporary assigned access.
    - Supports Identity protection. Centralized view of risks, access to all Cloud, etc.
  - Important Azure AD features:
    - Azure AD Connect:
	  - Azure AD Connect will integrate your on-premises directories with Azure Active Directory.
      - Azure AD Connect is the best way to connect your on-premises directory with Azure AD and Office 365.
	  - Single tool to provide an easy deployment experience for synchronization and sign-in.
	  - Users can use a single identity to access on-premises applications and cloud services such as Office 365.
	  - Azure Federation Services
	    - Federation is an optional part of Azure AD Connect and can be used to configure a hybrid environment using an on-premises AD FS infrastructure. This can be used by organizations to address complex deployments, such as domain join SSO, enforcement of AD sign-in policy, and smart card or 3rd party MFA.
	  - Azure AD Synchronization
	    - This component is responsible for creating users, groups, and other objects. It is also responsible for making sure identity information for your on-premises users and groups is matching the cloud.
	  - Azure AD Connect Health [^](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-health/)
        - enables you to monitor and gain insights into the overall health of the integration between your on-premises Windows Server Active Directory/Active Directory Federation Service and Azure AD (or Office 365).
    - Azure AD B2C (business to consumer)[^](https://azure.microsoft.com/documentation/articles/active-directory-b2c-overview/)
	  - Azure Active Directory (Azure AD) B2C is an identity management service that enables you to customize and control how customers sign up, sign in, and manage their profiles when using your applications. 
      - Leverages existing social accounts(Facebook, MS, Google, Amazon, LinkedIn) or custom accounts
	  - Azure AD B2C supports OpenID Connect for all customer experiences.
      - It is the evolution of Azure AD Access Control Sevice - classic service (ACS) 
    - Azure AD B2B (business to business)[^](https://azure.microsoft.com/documentation/articles/active-directory-b2b-collaboration-overview/)
	  - Azure Active Directory (Azure AD) business-to-business (B2B) collaboration capabilities enable any organization using Azure AD to work safely and securely with users from any other organization, small or large. 
      - Enable access to your organization’s applications from external business partner identities
	  - Partners use their own credentials, No requirement for partners to use Azure AD
      - Allows your business partners to use their own authentication credentials
    - Azure AD Application Proxy[^](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/)
      - enables users to leverage SSO to securely access on-premises web applications such as SharePoint sites and Outlook Web Access—without the need for a VPN.
      - Application Proxy is available for the Basic and Premium editions of Azure AD
    - Azure AD Directory Join [^](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-windows10-devices-overview/)
      - Directory Join enables Windows 10 devices to connect with Azure AD.
      - Allows users to sign-in to Windows using Azure AD accounts, will enable SSO to Azure resources, Windows Store, device access restriction using group policy.
      - Suitable for devices that cannot domain join
    - Azure AD Domain Services [^](https://azure.microsoft.com/documentation/articles/active-directory-ds-overview/)
      - Domain Services provide fully managed domain services such as domain join, group policy, LDAP, Kerberos/NTLM, and so on that are compatible with Windows Server Active Directory.
    - Azure AD Device Registration [^](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access-device-registration-overview/)
      - Enables mobile devices (such as iOS, Android, and Windows devices) to be registered in Azure AD
      - Enable conditional access to on-premises or Office 365 applications
    - Azure AD Cloud App Discovery[^](https://azure.microsoft.com/documentation/articles/active-directory-cloudappdiscovery-whatis/)
      - Finds the applications being used (usage metrics), identifies users, and enables offline data analysis.
      - Enables IT departments to discover cloud applications used in their organization
      - Allows the applications to be brought under IT control to help mitigate risk of potential data leakage or other security threats
    - Azure AD Identity Protection [^](https://azure.microsoft.com/documentation/articles/active-directory-identityprotection/) 
      - Is a security service that enables you to gain insights into potential security vulnerabilities affecting users in your organization (more specifically, their identities)
    - Azure Active Directory Privileged Identity Management (PIM):
	  - Azure Active Directory Identity Protection is a feature of the Azure AD Premium P2 edition that provides you an overview of the risk events and potential vulnerabilities affecting your organization’s identities.
	  - With Azure Active Directory Privileged Identity Management (PIM), you can manage, control, and monitor access to Azure Resources within your organization.
	  - Identity Protection uses adaptive machine learning algorithms and heuristics to detect anomalies and risk events that may indicate that an identity has been compromised.
	- Managed Service Identity
	  - Managed Service Identity (MSI) is a public preview feature of Azure Active Directory. Managed Service Identity (MSI) gives Azure services an automatically managed identity in Azure Active Directory (Azure AD), so there's no need to store the credentials within the service.
	    - System Assigned Identity = on the Azure service instance. Identity is automatically trusted, but als removed when resources are deleted.
	    - User Assigned Identity (public preview) = Based on subscription level.
	  - Azure Key Vault provides a way to securely store credentials and other keys and secrets, but your code needs to authenticate to Key Vault to retrieve them. Managed Service Identity (MSI) makes solving this problem simpler by giving Azure services an automatically managed identity in Azure Active Directory (Azure AD).
	- Active Directory editions [^](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-whatis#choose-an-edition) [^](https://azure.microsoft.com/en-us/pricing/details/active-directory/)
      - **Free** - manage users, synchronize with on-premises Active Directory, establish SSO across Azure and Office 365, and access SaaS applications in the Azure AD application gallery
      - **Basic** - Free tier, plus self-service password resets, group-based application access, customizable branding, Azure AD Application Proxy, and a 99.9 percent availability service level agreement (SLA)
      - **Premium (P1, P2)** - Free and Basic tiers, plus self-service group management, advanced security reports and alerts, Multi-Factor Authentication, and licenses for Microsoft Identity Manager. P2: provides Identity Protection and Privileged Identity Management.

| **Basic** | **Premium (P1 & P2)**|
|---:|:---|
|99.9% uptime SLA | Includes Microsoft Identity Manager (MIM) 2016 (an on-premises identity and access management suite)|
| Self-service password reset |  Self-service password reset with write-back |
| Azure AD Join for Windows 10 | Multi-facator authentication (MFA) |
| SSO for 10 apps / user |  No SSO app limit |
| Azure AD Application Proxy | MDM auto-enrollment|
| | P2: Identity Protection and Privileged Identity Management|
    
- IDMaaS:
  - Identity Management as a Service.
- Graph API:
  - Rest API to authenticate.
  - Works with a token and time intervals.
- OAuth & OpenID: 
  - uses the open standards for authentication.
  - User does not have to provide username and password to Azure.
  - 3rd party takes care of this.

- Considerations regarding Active Directory:
  - My organization has made large investments in on-premises Windows Server Active Directory, but we want to extend identity to the cloud.
    - The most widely used Azure identity solution is hybrid identity. If you’ve already made investments in on-premises AD DS, you can easily extend identity to the cloud using Azure AD Connect.
  - My business was born in the cloud and we have no investments in on-premises identity solutions.
    - Azure Active Directory is the best choice for cloud-only businesses with no on-premises investments.
  - I need lightweight Azure VM configuration and control to meet on-premises identity requirements for app development and testing.
    - Azure AD Domain Services is a good choice if you need to use AD DS for lightweight Azure VM configuration control or are looking to develop or migrate legacy, directory-aware on-premises applications to the cloud.
  - I need to support a few virtual machines in Azure, but my company is still heavily invested in on-premises Active Directory (AD DS).
    - Use DIY AD DS to use Azure VMs when you need to support a few virtual machines and have large AD DS investments on-premises.
  
### Hybrid Identities
- Hybrid entities:
  - Mixure of cloud and on prem network.
  - Allows one identity regardless where the apps are hosted.
- SAML Claims:
  - Security Assertions Markup Language.
  - SAML tokens are called claims.
  - SAML tokens are signed.
- Azure AD FS:
  - Federated Identities.
  - Responsibility is turned over to a 3rd party.
- AD Application Proxy:
  - SSO for remote employee to access web applications hosted inside.
  - No need to have a VPN.
  - Authentication through the proxy. 

### Network Security Group
- Network Security Group
  - is a virtual firewall.
  - Uses inbound and outbound rules.
  - NSG can be associated to multiple NIC's or subnets.
  - Each NIC or subnet only uses one NSG.
  - Default; only allows outbound. No inbound.
- Data security in transit and at rest:
  - SSL/TLS (Https).
  - For data at rest the Azure Storage Service Encryption (SSE) must be used.
  - SSE works by encrypting the data when it is written to Azure Storage, and can be used for Azure Blob Storage and File Storage (currently in Preview).
  - 256-bit AES.
  - ARM only. 
  - For Block blobs, page blobs and append blobs. Client side encryption is possible as well.
  - Azure Disk Encryption = Encrypt the disks used by Virtual Machines.
  - Encryption in Transit:
    - Transport-level encryption, such as HTTPS when you transfer data into or out of Azure Storage.
    - Wire encryption, such as SMB 3.0 encryption for Azure File shares.
    - Client-side encryption, to encrypt the data before it is transferred into storage and to decrypt the data after it is transferred out of storage.
  - Encryption at rest:
    - Storage Service Encryption allows you to request that the storage service automatically encrypt data when writing it to Azure Storage.
	  - All data written to Azure Storage is encrypted through 256-bit AES encryption, one of the strongest block ciphers available.
	  - Storage Service Encryption is enabled for all new and existing storage accounts and cannot be disabled. Because your data is secured by default, you don't need to modify your code or applications to take advantage of Storage Service Encryption.
    - Client-side Encryption also provides the feature of encryption at rest.
    - Azure Disk Encryption allows you to encrypt the OS disks and data disks used by an IaaS virtual machine.
	  - Azure Disk Encryption is a new capability that helps you encrypt your Windows and Linux IaaS virtual machine disks. 
	  - Azure Disk Encryption leverages the industry standard BitLocker feature of Windows and the DM-Crypt feature of Linux to provide volume encryption for the OS and the data disks. 
	  - The solution is integrated with Azure Key Vault to help you control and manage the disk-encryption keys and secrets in your key vault subscription.
- Azure Operations Management Suite:
  - A Suite to monitor traffic, see unusual things, etc.

### Role-Based Security
- Concept is to give roles access rights and assign the roles to groups and users.
- Built-in roles / Standard roles:
  - Owner has full access to all resources including the right to delegate access to others.
  - Contributor can create and manage all types of Azure resources but can’t grant access to others.
  - Reader can view existing Azure resources.
- Access inheritance.
  - Azure subscription belongs to only one AAD.
  - Azure group belongs to only one subscription.
  - Resource belongs only to one group.

### Azure Data Storage
- Table Storage:
  - Structured
  - Schemaless
  - name-value pair storage (no RDBMS!)
  - Rows are called entities
  - PartionKey + RowKey are the primairy key.
  - Entities can have up to 255 properties (columns).
  - Timestamp is always included. 
- SQL Storage:
  - Relational SQL storage.
  - Elastic and auto-scales.
  - Azure hosted database.
  - Geo-replication (optional).
- DocumentDB Storage: 
  - NoSQL storage for JSON objects.
- Blob Storage:
  - Container model
  - Containers have security.
  - Blob = binary large object.
  - 500TB capacity.
  - Endpoint: http://myaccount.blob.core.windows.net/mycontainer/myblob
  - Block blobs:
    - Optimized for streaming and storing objects in the Cloud.
    - Block blobs are idea for large binary (or text) file storage that don't need to be frequently read from or accessed to.
    - Block blobs support up to 50,000 blocks of up to 100MB each, or approximately 4.75 TB.
    - Cool and Archive options:
      - Cool early deletion period of 30 days.
      - Archive early deletion period of 180 days.
  - Append blobs: Similar to block blobs but optimized for append only (log files).
  - Page blobs:
    - Good for random writes, represents harddisk, virtual machine storage.
    - Page blobs are better for Virtual machine VHD files.
- Queue Storage:
  - Max 64gb in size.
  - Invisibility behavior, read and then becomes invisible for a period of time.
- File Storage:
  - Works as a network file share (SMB 3.0).
  - Also a REST API.
  - Useful for legacy systems.
  - Can create directories.
  - Each file can be up to 1TB.
  - Files are addressable by a private URL.
- SQL Server VM:
  - IaaS model for SQL Server.
  - Needs a license.
- Securing SQL Server:
  - By default only the owner of the storage account has access.
  - Assign right roles.
  - Avoid anonymous access.
  - Whitelist IP's and use ACL (Access control lists).
  - Encryption (can use SSL as well).
  - Shared Access Signature (SAS) can give temporary access to storage.
  - Shared Access Policy (SAP) can be given to users and revoked as needed.
- Deciding when to use Azure Blobs, Azure Files, or Azure Disks
  - Azure Files:
    - Provides an SMB interface, client libraries, and a REST interface that allows access from anywhere to stored files.
	- You want to "lift and shift" an application to the cloud which already uses the native file system APIs to share data between it and other applications running in Azure.
	- You want to store development and debugging tools that need to be accessed from many virtual machines.
	- Endpoint: http://myaccount.file.core.windows.net/myshare/myfile.txt
	- 5 TiB file shares and Up to 1 TiB per file
  - Azure Blobs:
    - Provides client libraries and a REST interface that allows unstructured data to be stored and accessed at a massive scale in block blobs.
	- You want your application to support streaming and random access scenarios.
    - You want to be able to access application data from anywhere.
	- Endpoint: http://myaccount.blob.core.windows.net/mycontainer/myblob
	- Up to 500 TiB containers and Up to about 4.75 TiB per block blob
  - Azure Disks:
    - Exclusive to a single virtual machine. Files within the VHD cannot be accessed.
    - Provides client libraries and a REST interface that allows data to be persistently stored and accessed from an attached virtual hard disk.
	- You want to lift and shift applications that use native file system APIs to read and write data to persistent disks.
	- You want to store data that is not required to be accessed from outside the virtual machine to which the disk is attached.
	- 4 TiB disk
	
### Azure Mobile Apps
- Azure Mobile Apps: Provides capabilities (PaaS services) to make the development of Mobile Apps much more easy.
- Cross Platform Applications: Cross platform support (Windows, iOS, Andriod, HTML5, Xamarin, Apache Cordova.
- Offline Sync: allows to use the mobile app offline and sync mobile client data once it becomes online. Uses implicit push and incremental pull model.
- Extend Mobile Apps: Services to provide essential functions for mobile apps, such as custom authentication push, notifications, data storage.
- Security: Two levels: Infrastrcture and platform security, Application Security. Infrastrcture and platform security = managed by Microsoft. Application Security = responsibility of developer.

### Azure Notification Hub
- Service that takes care of all the platforms (iOS, Android, Windows Mobile, Kindle).
- Uses single API.
- Also supports .NET, PHP, Java, NodeJS.
- Can target single user, group or all users.
- Allows tagging for sementation.
- Can security notification, so the app pulls the data secure after receiving the notification.

### Web API
- Web API:
  - Web API is a way to communicate between to systems using API's.
  - Web API runs on the MVC model.
  - Verifies the request and populates a result based on the model.
  - Can be deployed to a VM running IIS (IaaS) or to the PaaS Web Application.
  - Integrated with Visual Studio.
- Scaling Web API:
  - For the vm you can either, upgrade to a higher hosting place, change the tier, instance size and instance count (horizontal scaling).
  - Can be used with traffic manager to grow across regions and geos.
- Web Jobs:
  - Continuously running jobs or predefined scheduled jobs.
  - Works with Azure Service Bus or Azure Storage.
  - Windows EXE, CMD, BAT, PS1, Shell, PHP, Python, NodeJS.
  - Can scale by having more instances. Uses the WebJobs SDK.
- Securing Web API's:
  - For corporate networks use Azure AD Service, AD Federation or Access Control Service.
  - Monitoring or tokens. Audit, logging, validation of inputs, SSL or cryptographic. 

### Hybrid Applications
- Azure Relay / Service Bus Relay:
  - Allows apps to connect to your on premises services.
  - The Service Bus Relay runs in the cloud accepts the request and securely passes on that request to the WCF service running indside the corp network.
  - Azure Relay has two features:
    - Hybrid Connections - Uses the open standard web sockets enabling multi-platform scenarios. Hybrid Connections can be used to access application resources in other networks. It provides access from your app to an application endpoint.
    - WCF Relays - Uses Windows Communication Foundation (WCF) to enable remote procedure calls. WCF Relay is the legacy relay offering that many customers already use with their WCF programming models.
- Self-hosted Integration Runtime / Data Management Gateway:
  - The Self-hosted Integration Runtime / Data management gateway is a client agent that you must install in your on-premises environment to copy data between cloud and on-premises data stores.
    - Data movement: Move data between data stores in public network and data stores in private network (on-premise or virtual private network). It provides support for built-in connectors, format conversion, column mapping, and performant and scalable data transfer.
    - Activity dispatch: Dispatch and monitor transformation activities running on a variety of compute services such as Azure HDInsight, Azure Machine Learning, Azure SQL Database, SQL Server, and more.
    - SSIS package execution: Natively execute SQL Server Integration Services (SSIS) packages in a managed Azure compute environment.
  - Data Management Gateway has now been rebranded as Self-hosted Integration Runtime.
- Azure On-premises Data Gateway
  - The on-premises data gateway acts as a bridge, providing secure data transfer between on-premises data sources and your Azure Analysis Services servers in the cloud.
  - Can be necessary for connecting to on-premises data sources only.
	
### Azure Media Services
- Azure Media Services:
  - Supports online streaming of video's for live and recorded.
  - PaaS model, so no access to underlying hardware.
  - Supports live encoding. Uses streaming endpoints.
  - Supports previews, Media indexer for closed captions and transcripts.
  - Checks quality of the stream. Archive and DRM (AES Clear Key dynamic encryption).
- Azure Media Analytics:
  - Media Analytics is a collection of speech and vision components 
  - Paas to derive actionable insights from video files.
  - Uses the following components: Indexer (make content searchable), Hyperlapse (video stabilization and time-lapse), Motion Detector, Face Detector, Video summarization (selecting interesting snippets), Optical character recognition (OCR), Scalable face redactio, Content Moderation

### Compute Intensive Applications
- For jobs that can not be handled sequential
- Managed, Scalable, Elastic and Pay for use model.
- HPC pack: on premises, hybrid or IaaS (spin up VM's) or PaaS (Azure Batch)
- Head node (controls the distribution of the jobs, can be on prem or cloud) and Compute nodes (execution of tasks)
- Azure Batch:
  - Use Azure Batch to run large-scale parallel and high-performance computing (HPC) batch jobs efficiently in Azure.
  - Azure Batch creates and manages a pool of compute nodes (virtual machines), installs the applications you want to run, and schedules jobs to run on the nodes. 
  - Includes job scheduling as a service, application lifecycle management, budget, quotas, users and limites.
  - There is no cluster or job scheduler software to install, manage, or scale. Instead, you use Batch APIs and tools, command-line scripts, or the Azure portal to configure, manage, and monitor your jobs.
  - Batch works well with intrinsically parallel (also known as "embarrassingly parallel") workloads.
  - Uses a JSON for deployment, not ARM templates
- Azure Batch AI:
  - Parallel exeucution of Deep Learning- and AI-models using GPU's.
  - Uses frameworks like CNTK, TensorFlow, Chainer, etc.
  - Contains Jupyter Notebooks

### Designing Long Running Applications
- Jobs that take days, weeks, with the risk of interupption.
- Availability: Multiple instances, stateless vs state-ful, avoid single point of failures, deploy across regions
- Reliability: Faulty domains, error handling and retry, loose coupling, monitoring
- Scaling: planned versus reactive. Scaling up versus out, breaking up into smaller parts
- Other considerations: partition the workload, Logging, Retry on errors, Gracefully restart, check points, single instances (is_singleton:true)

### Selecting Storage Options
- Traditional SQL have challenges around scaling, consider partioning and sharding.
- NoSQL options for very large applications.
- Read versus write, temporary storage versus persistent storage, consistency and locking, data sorting versus indexing, latency

### Integrating Azure Services with a Solution
- Azure Machine Learning:
  - Azure Machine Learning is an integrated, end-to-end data science and advanced analytics solution. It enables data scientists to prepare data, develop experiments, and deploy models at cloud scale.
  - Cloud service that allow to create jobs to predictive do analytics.
  - Examines large amounts of data to detect patterns, generate code to recognize those patterns.
  - Azure Machine Learning is built on top of the following open source technologies: Jupyter Notebook, Apache Spark, Docker, Kubernetes, Python, Conda
  - The main components of Azure Machine Learning are:
    - Azure Machine Learning Workbench
    - Azure Machine Learning Experimentation Service
    - Azure Machine Learning Model Management Service
    - Microsoft Machine Learning Libraries for Apache Spark (MMLSpark Library)
    - Visual Studio Code Tools for AI
- Big Data:
  - HDInsight, uses Apache Hadoop (HortonWorks).
  - Manage, Analyze and Report.
- Azure Search:
  - PaaS service, integrated with REST API's and .NET SDK.
  - Supports mulitple languages, simple query syntax, suggestions, highlight matches, facets and filters.
- Azure Time Series:
  - Time Series Insights is built for storing, visualizing, and querying large amounts of time series data, such as that generated by IoT devices.
- Cognitive Services:
  - Microsoft Cognitive Services (formerly Project Oxford) are a set of APIs, SDKs and services available to developers to make their applications more intelligent, engaging and discoverable.
- Azure Bot Service:
  - Azure Bot Service provides tools (SDK) to build, test, deploy, and manage intelligent bots all in one place.

### Scalability and Performance of Azure Web Apps
- Globally Scale Azure Web Apps:
  - CDN
  - Traffic Manager
  - Traffic Manager failover
  - Choosing the right database
  - Auto-Scale
  - Caching
  - Smart application architecture & design
- Develop Web Apps in .NET: Azure SDK for .NET
- Debugging Web Apps:
  - Turn off customerrors in web.config
  - Publish debug version with symbols
  - Attach IDE to Azure server
  - Use server explorer
  - Enable remote debugging (disabled after 48 hours)
  - Application logs, web server logs, detailed error logs, tracing logs.
- Language Options: .NET, Java, PHP, Ruby, NodeJS, Python, Powershell and other scripts.
- Difference Between Web Apps and VM and Cloud Services:
  - Web Apps (PaaS, Package code and config, Autoscale, Multiple instances)
  - Cloud Services (PaaS, VM based, Resizing causes downtime, remote desktop access, install software, Azure takes care of OS) and Virtual Machines (IaaS, do it yourself).

### Deploy Web Apps
- Azure Site Extensions:
  - Allow you to add custom blocks to the Azure management portal, e.g. sales figures, phpMyadmin, account usage, etc.
  - Can use pre-defined packages or create custom packages.
- Packages:
  - An way of deploying code from the development environment to Azure *publishing*.
  - Takes only the relevant code and pushes it to Azure.
  - Azure supports multiple options; FTP/SFTP, Project Kudo (Source Control Management, open source project similar to Git), Web Deploy (built into Visual Studio .NET, precompiles locally and then pushes).
- App Hosting Plans:
  - Web Apps are hosted on App Service Plans.
  - App Service Plans have limits and prices (# of apps, disk space, SLA's, Auto scaling).
  - Allows to group app together under one service plan.
  - Only apps in the same region can share a plan.
  - Create seperate plans for dev, test and production.
  - Apps and plans belong to a resource group.
  - Resource groups are restricted to a region.
- Deployment Slots:
  - Staging environment to be used before applications are deployed to production.
  - Allows testing before application is published.
  - There is a flip switch to do final deployment.
  - DNS level change.
  - Allows easy roll-back.
  - A/B testing.
  - You can decide to route a percentage of live traffic to the second slot.
  - Allows to see errors only on small percentage of the visitors. 

### Design for Business Continuity
- Scaling for Business Continuity:
  - allows to recover quickly from disasters.
  - Scaling up: larger hardware, more memory, bigger CPU.
  - Scaling out: multuple instances.
  - Auto-scaling is only supported on Standard and Premium.
  - Choose a metric and the app will scale based on the metric.
  - SQL Database also has a scaling feature, which is called Elastic Scale.
  - Allows database sharding.
  - CDN scaling and scaling across different regions.
  - Traffic Manager to direct to the quickest responding site.
  - Scaling-up an application involves moving to a higher pricing tier, which can be done in the Azure portal in a few minutes.
- Data Replication:
  - SQL databases distributed to other regions.
  - SQL Sync is not pure replication, but sync.
  - SQL Geo Replication.
  - Data Replication.
  - Premium tier = active geo-replication.
  - Up to 4 copies in the same region or across other regions.
  - Primary is always ahead. Secondary is sometimes behind.
- Disaster Recovery:
  - Use deployment slots for minimal downtime.
  - Use multiple instances for minimal downtime.
  - Thinks about backup strategy.
  - Backups are stored as blobs, Azure Storage Account is needed to store backups on.
  - High Availability consists of Availability, Scalability, Fault Tolerance.
  - Recovery Time Objective (RTO) = maximum amount of time to restore application functionality.
  - Recovery Point Objective (RPO) = acceptable time window of data loss.
  - Locally redundant storage (LRS) and Zone Redundant Storage (ZRS) storage stores 3 copies of your data in a single region, while Azure Geo Redundant Storage (GRS) and Read-access Azure Geo Redundant Storage (RA-GRS) stores 6 copies across two regions.
  - Azure Managed Disks
    - Premium Managed Disks are backed by Solid-State Drives (SSDs).
	  - By default, disk caching policy is Read-Only for all the Premium data disks, and Read-Write for the Premium operating system disk attached to the VM. 
	- Standard Managed Disks are backed by regular spinning disks.
	- You can upload either generalized and specialized VHDs.
      - Generalized VHD - has had all of your personal account information removed using Sysprep.
      - Specialized VHD - maintains the user accounts, applications, and other state data from your original VM.
    - Locally redundant storage (LRS)
      - Replicates your data three times within the region in which you created your storage account.
  - Storage account-based disks
    - Locally redundant storage (LRS)
      - Replicates your data three times within the region in which you created your storage account.
    - Zone redundant storage (ZRS)
      - Replicates your data three times across two to three facilities, either within a single region or across two regions.
    - Geo-redundant storage (GRS)
      - Replicates your data to a secondary region that is hundreds of miles away from the primary region.
    - Read-access geo-redundant storage (RA-GRS)
      - Replicates your data to a secondary region, as with GRS, but also then provides read-only access to the data in the secondary location.
	- Unmanaged disks:
      - Premium storage is backed by Solid-State Drives (SSDs) and is charged based on the capacity of the disk.
      - Standard storage is backed by regular spinning disks and is charged based on the in-use capacity and desired storage availability.
        - For RA-GRS, there is an additional Geo-Replication Data Transfer charge for the bandwidth of replicating that data to another Azure region.
- Azure Resource Manager (ARM) Templates:
  - Use Azure Resource Manager (ARM) Templates to create automated deployment of resources for highly available web apps.
  - You leverage from having the same configuration across different locations or regions.

### Microsoft System Center for Hybrid Model
- Microsoft System Center:
  - Software to manage infra and operations.
  - App Controller = spins up virtual machines on-prem Hyper-V or virtual machines on Azure
  - Data Protection Manager (DPM) = complete backup solution for servers
  - Server Manager (IT Service Manager) = library for ITIL
  - Configuration Manager (SCCM) = everything included to manage the infra
  - EndPoint Protection = firewall
  - Orchestrator (SCO) = manager for the runbooks and orchestration.
  - VM Manager (VMM) = manage the virtual machines, scaling scripting, etc.
  - Unified Installer = installer.
    - Hybrid model = mixture of cloud and on-prem.
    - Azure model = only Azure.
- Considerations for Choosing the Hybrid Model:
  - Authentication
  - Response time
  - Single points of failure
  - Data travelling over distances.
- When to Use a Hybrid Model:
  - Maintain control over the data
  - Web apps and mobile apps can access existing on-premises data and services securely
  - Flexbility between cloud providers.
  - On demand scalability.

### Monitoring
- Azure Monitoring:
  - minimal monitoring is enabled by default.
  - Turn on verbose monitoring for more details.
  - Verbose monitoring requires a storage account.
  - Up to 10 days.
  - Management portal has monitoring.
  - Azure Monitor and Diagnostic Service.
  - Azure Management API's can be used to retrieve monitoring details.
  - System Center Operations Manager (SCOM) is typically what is really used for detail insights.
- System Center Operations Manager (SCOM):
  - SCOM is the heart of operations.
  - Azure Management Pack fo Operations is needed to get details from Azure.
  - Allows also monitoring of SQL Server, Web Apps.
  - Requires to install an agent to be installed on server to do monitoring.
  - Requires PKI certificates between machines for trust and security.
- Monitoring Use Cases:
  - Global Service Monitor = Measure the experience of the end users (what is the delay of the website?)
  - Applications Insights = allows to monitor application at the code level, e.g. application or SQL exceptions.
    - Can be added during development (SDK).
  - System Center = Integrated monitoring with your onpremises and Azure applications.
    - Overall health of the system(s).
- Patching Strategy:
  - Fault Domain = everything that can be seen as a single point of failure.
    - A fault domain is a logical group of underlying hardware that share a common power source and network switch, similar to a rack within an on-premises datacenter. As you create VMs within an availability set, the Azure platform automatically distributes your VMs across these fault domains. This approach limits the impact of potential physical hardware failures, network outages, or power interruptions.
  - Availability set allows to run outside a single fault domain.
  - Should have at least 2 instances of each role.
  - Update Domain = For PaaS apps, Microsoft will try to update the apps for only a single Update Domain at a time.
    - An update domain is a logical group of underlying hardware that can undergo maintenance or be rebooted at the same time. As you create VMs within an availability set, the Azure platform automatically distributes your VMs across these update domains. This approach ensures that at least one instance of your application always remains running as the Azure platform undergoes periodic maintenance. The order of update domains being rebooted may not proceed sequentially during planned maintenance, but only one update domain is rebooted at a time.
  - Maintenance is planned.
  - Keep you applications across multiple update domains to avoid downtime.
  - Maximum of 20 update domains per role service.
  - Default = 5 update domains, 3 fault domains
  - Deployment Slots = staging copy, which allows to fast deployment. Easy roll-back.
- Security Center:
  - Single dashboard for overall security health of Azure resources.
  - Free and standard

### Business Continuity and Disaster Recovery
- Business Continuity / Disaster Recovery (BC/DR):
  - design applications for maximum availability.
  - Read Acces Geo Redudant guarantees 99.99%.
  - Think of tradeoffs, how much data loss is acceptable and how much downtime is acceptable.
  - The 99.95% SLA is available for all tiers (Standard, Premium and Basic) except Free and Shared.
- Hyper-V and Hyper-V Replica:
  - Hyper-V Replica allows to have a replicated version of the virtual machine.
  - Azure Site Recovery:
    - Azure Site Recovery protects your VMs from a major disaster scenario, when a whole region experiences an outage due to major natural disaster or widespread service interruption.
    - Allows the primary site to be the secondary if fail-over is required.
    - Operates in a hybrid model.
    - Between VMM and Azure.
    - Between Hyper-V and Azure.
    - Between on-premises sites.
    - Azure Site Recovery allows you to automate the replication of virtual machines data whether they are in Azure, or on prem.
- Azure Backup for VMs:
  - Use Azure Backup to backup Azure Resource Manager VM's.

### Azure Backup
- Overview of Azure Backup:
  - For backing up Azure VMs running production workloads, use Azure Backup. 
  - Azure Backup supports application-consistent backups for both Windows and Linux VMs. 
  - Azure Backup encrypts the backups in the cloud.
  - No cost for data transmission into Azure.
  - Full or incremental backups.
    - A managed snapshot is a read-only full copy of a managed disk. Snapshots exist independent of the source disk and can be used to create new managed disks for rebuilding a VM. 
  - File-level restore (instead of full restore).
  - Azure Backup Vault: Recovery Services creates a Backup Vault for backups.
  - Recommended to use different region.
  - Requires vault credentials certificates and a passphrase.
  - Limit of 1.7TB per volume can be backed up.
  - Data Protection Manager:
    - Is part of System Center.
    - More powerfull backup solution.
    - Supports bare meta recovery.
    - Requires agent to be installed.
    - Can be deployed on prem or in Azure.
    - Overall limit of 500TB in an Azure Storage account.
  - Azure Backup Components[^](https://docs.microsoft.com/en-us/azure/backup/backup-introduction-to-azure-backup#which-azure-backup-components-should-i-use)

||||||
|:--|:--|:--|:--|:--|
|**Component**|**Benefits**|**Limits**|**What is protected?**|**Where are backups stored?**|
|Azure Backup Agent (MARS)|<ul><li>Backup files and folders on Physical or virtual windows OS</li> <li> No backup server required </li> </ul>|<ul><li> Backup 3x per day</li><li>Not application aware; file, folder, and volume-level restore only</li> <li>**No support for Linux**</li> </ul>| <ul> <li>Files</li> <li> Folders </li> <li> System State</li></ul>| Recovery Services vault |
|System Center DPM|<ul><li>Application-aware snapshots (VSS)</li> <li>Linux support on Hyper-V and VMware VMs</li></ul>|<ul><li>Cannot back up Oracle workload</li></ul>|<ul><li>Files</li><li> Folders</li><li>Volumes</li><li>VMs</li><li>Applications</li><li>Workloads</li><li>System State</li></ul>| <ul><li> Recovery Services vault</li> <li>Locally attached disk</li><li>Tapes (on-prem)</li></ul>|
|Azure Backup Server | <ul> <li>Does not require System Center license</li></ul>|<ul><li>Cannot back up Oracle workload</li><li>Always requires live Azure subscription</li><li>No support for tape backup</li> </ul>|<ul><li>Files</li><li>Folders</li><li>Volumes</li><li>VMs</li><li>Applications</li><li>Workloads</li><li>System State</li></ul>| <ul><li> Recovery Services vault</li> <li>Locally attached disk</li></ul>|
|Azure IaaS VM Backup|<ul><li>Native backups for Windows/Linux</li><li>No specific agent installation required</li><li>Fabric-level backup with no backup infrastructure needed</li></ul>|<ul><li>Back up VMs once-a-day</li><li>Restore VMs only at disk level</li><li> **Cannot back up on-premises**</li></ul>|<ul><li>VMs</li><li>All disks (using PowerShell)</li></ul>|Recovery Services vault|
||||||


### Azure Automation
- Azure SDK for PowerShell, connect you azure account to PowerShell, PowerShell has more options than the portal, works with storage, vm's, etc.
- Workflows: runbooks, publish, running, scheduling, controlling VMs outside the VM

### Chef and Puppet
- Chef:
  - open source
  - cloud and infra automation
  - available for Azure, AWS, Google Cloud
  - Powerful but complex, recipes called cookbooks
- Puppet:
  - open source
  - cloud and infra automation
  - available for Azure, AWS, Google Cloud, VMWare
  - Powerful but complex, manifests and modules
- Azure Automation / previousely Desired State Configuration (DSC):
  - forcing a desired state
  - ensures that the state remains in predefined consistent setting, e.g. check the security settings, e.g. check modules installed, e.g. avoid software to be installed.
  - Process automation = automates frequent, time-consuming, and error-prone cloud management tasks
  - Configuration management = apply configurations to virtual or physical machines from a DSC Pull Server
  - Update management = Update Windows and Linux systems across hybrid environments
  - PowerShell can be used to enfore DSC.
  - Azure Automation can turn those scripts into scheduled tasks that run in the cloud.

### New services
- Azure functions:
  - Small functions. (Bash, Batch, C#, F#, javascript, PHP, Powershell, Python).
  - Consumption plan: When your function runs, Azure provides all of the necessary computational resources. You don't have to worry about resource management, and you only pay for the time that your code runs.
  - App Service plan: Run your functions just like your web, mobile, and API apps. When you are already using App Service for your other applications, you can run your functions on the same plan at no additional cost.
  - Functions provides templates to get you started with key scenarios, including the following:
    - HTTPTrigger - Trigger the execution of your code by using an HTTP request. For an example, see Create your first function.
    - TimerTrigger - Execute cleanup or other batch tasks on a predefined schedule. For an example, see Create a function triggered by a timer.
    - GitHub webhook - Respond to events that occur in your GitHub repositories. For an example, see Create a function triggered by a GitHub webhook.
    - Generic webhook - Process webhook HTTP requests from any service that supports webhooks. For an example, see Create a function triggered by a generic webhook.
    - CosmosDBTrigger - Process Azure Cosmos DB documents when they are added or updated in collections in a NoSQL database. For an example, see Create a function triggered by Azure Cosmos DB.
    - BlobTrigger - Process Azure Storage blobs when they are added to containers. You might use this function for image resizing. For more information, see Blob storage bindings.
    - QueueTrigger - Respond to messages as they arrive in an Azure Storage queue. For an example, see Create a function triggered by Azure Queue storage.
    - EventHubTrigger - Respond to events delivered to an Azure Event Hub. Particularly useful in application instrumentation, user experience or workflow processing, and Internet of Things (IoT) scenarios. For more information, see Event Hubs bindings.
    - ServiceBusQueueTrigger - Connect your code to other Azure services or on-premises services by listening to message queues. For more information, see Service Bus bindings.
    - ServiceBusTopicTrigger - Connect your code to other Azure services or on-premises services by subscribing to topics. For more information, see Service Bus bindings.
  - Integrates with:
    - Azure Cosmos DB
    - Azure Event Hubs
	  - Azure Event Hubs is a highly scalable data streaming platform and event ingestion service, capable of receiving and processing millions of events per second.
	  - Event Hubs can process and store events, data, or telemetry produced by distributed software and devices.
	  - Data sent to an event hub can be transformed and stored using any real-time analytics provider or batching/storage adapters. 
    - Azure Event Grid
	  - Azure Event Grid allows you to easily build applications with event-based architectures.
	  - Event Grid has built-in support for events coming from Azure services, like storage blobs and resource groups.
	  - Event Grid also has custom support for application and third-party events, using custom topics and custom webhooks.
    - Azure Mobile Apps (tables)
    - Azure Notification Hubs
	  - Azure Notification Hubs provide an easy-to-use, multi-platform, scaled-out push engine.
	  - With a single cross-platform API call, you can easily send targeted and personalized push notifications to any mobile platform from any cloud or on-premises backend.
    - Azure Service Bus (queues and topics)
    - Azure Storage (blob, queues, and tables)
    - GitHub (webhooks)
    - On-premises (using Service Bus)
    - Twilio (SMS messages)
  - If an application needs an HTTP endpoint for a potentially long running  process without getting into a elaborate programming model, Azure Functions is a good option. They can be developed as an extension of an existing application. They support routing based endpoints similar to an API. They support AAD and other authentication, SSL, Custom Domain, RBAC, etc. They have a good CI/CD support as well. They are in the process of integrating .Net Core support. So if you are looking to get a simple application model and don't want to get into setting up/managing underlying infrastructure, Azure Functions is good choice.
- Stream Analytics:
  - For streaming data.
  - Uses inputs (topics), queries (pattern matching) and outputs (subscribers)
- IoT Hub / Azure Events Hub:
  - allows 2 way communication between many devices and the back-end.
  - IoT Hub supports more protocols and supports 2 way communication.
  - State management
  - Events Hub = one way and not state management
- Service Fabric:
  - Microservices platform.
  - Microsofts' internal microservices platform.
  - Supports state management, monitoring, fail-over, load balancing, etc.
  - If an application must have its state saved locally, then use Service Fabric. It is also a good choice if you are looking to deploy application in Windows server ecosystem(Linux support is in the works as well!). Refer to common workloads on Service Fabric for more discussion on applications that can benefit from Service Fabric. Biggest benefit is that Service Fabric applications can run on-premise, on Azure or even in other cloud platforms also.
  - https://stackoverflow.com/questions/48415057/difference-between-kubernetes-and-service-fabric
- Azure Cloud Shell:
  - Alternative to Powershell.
  - Supports Bash environment and Powershell (windows) environment.
  - Webbased interface.
- Azure Container Services (AKS):
  - Kubernetes microservices platform.
  - Managed by Azure.
  - Developed by Google.
  - Uses containers.
  - Each VM has it's own operating system.
  - Runs on a Host OS / Hypervisor.
  - Containers virutalizes the application environment, allowing to move applications easily.
  - Application lives inside the container.
  - Containers are light weight.
  - Quick, auto scaling, enables multi-cloud, provides auto-restart.
  - Uses a manifest file (YAML - .yml).
    - Similar to ARM templates.
- Azure Container Instances:
  - Azure Container Instances offers the fastest and simplest way to run a container in Azure, without having to manage any virtual machines and without having to adopt a higher-level service.
    - Azure Container Instances is a great solution for any scenario that can operate in isolated containers, including simple applications, task automation, and build jobs.
	- Co-scheduled groups: Azure Container Instances supports scheduling of multi-container groups that share a host machine, local network, storage, and lifecycle. This enables you to combine your main application container with other supporting role containers, such as logging sidecars.
    - Azure Container Networking: https://github.com/Azure/azure-container-networking
  - If you are looking to deploy your application in Linux environment and are comfortable with an orchestrator such as Swarm, Kubernetes or DC/OS, use ACS. A typical 3 tier application (such as a web front end, a caching layer, a API layer and a database layer) can be easily container-ized with 1 single dockerfile (or docker-compose file). It can be continuously decomposed into smaller services gradually. This approach provides an immediate benefit of portability of such an application. Containers is Open technology and there is great community support around containers.
- API Management for API Apps and Serverless Apps:
  - API Management (APIM) helps organizations publish APIs to external, partner, and internal developers to unlock the potential of their data and services.
  - Contains an API Gateway service, Azure portal and Developer portal 
  - Has API Gateway features like API limits, caching, user and groups, policies, Open or Protected products, reporting, developer focussed portal and documentation, tokens, etc.
- Azure Key Vault:
  - Manage cryptographic keys, also third party keys.
  - By using Key Vault, you can encrypt keys and secrets (such as authentication keys, storage account keys, data encryption keys, .PFX files, and passwords) using keys protected by hardware security modules (HSMs).
  - Allows to no longer hard code the key inside the application. You retrieve the value by referencing the key vault and secret in your parameter file. The value is never exposed because you only reference its key vault ID. 
  - Has the option to use a hardware component.
  - ARM templates: When creating the key vault, set the enabledForTemplateDeployment property to true. By setting this value to true, you allow access from Resource Manager templates during deployment.
- Virtual Machine Scale Sets (VMSS):
  - Allows to deploy multiple virtual machines as a set. Automatically create from central configuration
  - True autoscaling. The number of VM instances can automatically increase or decrease in response to demand or a defined schedule.
  - Increases or decreases based on the performance (I/O or CPU) or fixed schedules.
  - Limit of 1000 generic.
  - 300 custom.
  - Recommended to use placement groups (above 100 use singlePlacementGroup=false).
  - Don't get public IP's by default.
  - Use a load balancer and jump boxes to connect.
  - There is no additional cost to scale sets. You only pay for the underlying compute resources such as the VM instances, load balancer, or Managed Disk storage. 
- Overview of Containers in Azure:
  - AKS
  - Container instances (isolated containers, windows or linux, public ip, custom size)
  - Container registry (store and manage docker images, works also with other container platforms)
  - Service Fabric
  - App Service (deploy code directly from desktop to PaaS)
  - Azure Batch (A service that allows large workloads to be deployed to).
- Network Watcher:
  - Monitoring and diagnosis tool for your network.
  - Topology (vm's, etc.).
  - Package sniffer.
  - Monitor network groups, Troubleshoot VPN problems.
  - Troubleshoot VM connectivity issues.
  - 1 network watcher per region.
- Data Lake Store:
  - Uses the HDFS standard to easy migration of Hadoop and Spark.
  - Blobs have limites (500TB, file 195GB-8TB)
  - Date Lake is optmized for throughput.
  - Blobs can be replicated.
  - Blobs can work with more languages.
- Storsimple:
  - Physical hardware device that is installed inside your on-prem network.
  - Cloud appliance.
  - Used for back-ups, archiving using secure channel.
  - StorSimple Virtual Array is a virtual machine. 
- SQL Server Stretch DB:
  - Allows to scale SQL Server 2016 'cold data' into the Cloud.
  - Uses your existing SQL Server instance.
  - Good alternative to buying more local storage. Works on table level.
- Azure Database for PostgreSQL and MySQL:
  - Fully managed PostgreSQL or MySQL.
  - Has high availability.
  - Auto backup and restore.
- Azure Monitor:
  - Allows collection of metrics, activity logs and diagnostic logs.
  - Can generate alerts and notifications based on all the resources.
  - Is a more basic monitoring.
- Azure Log Analytics:
  - Infrastructure monitoring tool for logging at infra level.
  - Can analysis on-prem and cloud log files and stores the result central.
  - Can connect to System Center Operations Manager (SCOM) or to a VM directly.
  - Comparable to Splunk. 
- Azure Advisor:
  - Azure Advisor is a tool that makes recommendations based on the usage and resources.
  - High availability, security, performance and costs.
- Azure Data Catalog:
  - Enterprise metadata catalog of all data sources.
  - contains definitions, locations, tags and other information.
  - Can be used to restrict access.
- Accelerated Networking:
  - 30Gbps networking throughput when VM's are running on the same virtual network
  - Only supports recent versions: Windows Server 2016 Datacenter, Windows 2016 R2 DataCenter, some of the latest linux versions (Ubutu, Centos, etc.)
  - Only computed uptimized instances are supports (D, E or F series with 2/4 CPU's)
  - Virtual switch has been taken out
- VM reserved instances:
  - one or three year contract with up to 72% cost savings
- Availability Zones
  - Currently 5 regions are supported (Central US, France Central, East US 2, West Europe, Southeast Asia)
  - If deployed on at least two AZ's the garanteed uptime is 99.99% for VM's
  - In the same Availability Set uptime is 99.95%
  - Single instance virtual machine using premium storage the uptime is 99.9%
- Virtual Network Service Endpoints
  - Security feature that allows to connect networks over the private network instead using the public network. Ties a virtual network to a specific Azure service.
  - Azure Storage, SQL Database, Cosmos DB, SQL Data Warehouse
- Azure Event Grid:
  - Azure Event Grid allows you to easily build applications with event-based architectures.
  - Event Grid has built-in support for events coming from Azure services, like storage blobs and resource groups.
  - Uses the following concepts: 
    - Events - What happened.
    - Event sources/publishers - Where the event took place.
    - Topics - The endpoint where publishers send events.
    - Event subscriptions - The endpoint or built-in mechanism to route events, sometimes to multiple handlers. Subscriptions are also used by handlers to intelligently filter incoming events.
    - Event handlers - The app or service reacting to the event.
  - Event sources: Azure Subscriptions (management operations), Custom Topics, Event Hubs, IoT Hub, Media Services, Resource Groups (management operations), Service Bus, Storage Blob, Storage General-purpose v2 (GPv2)
  - Event handlers: Azure Automation, Azure Functions, Event Hubs, Hybrid Connections, Logic Apps, Microsoft Flow, Queue Storage, WebHooks

### Big Data
- HDInsight for ETL:
  - Orchestration (Apache Oozie for scheduling, Azure Data Factory)
  - Data Sources (Azure Blob Storage, Azure Data Lake Store, SQL Data Warehouse, Apache HBase, SQL Sources)
  - Extraction (Apache Scoop for file transfer, Apache Flume for large amount of log data)
  - Transform (SQL Server Integration Services, Split a single line of data into tables, validate against business rules), Hive for relational storage, Pig for processing, Pig works with SSIS.
- SQL Data Warehouse:
  - Massively Parallel Processing (MPP)
  - Data is broken into shards
  - Control node (the brain of the Database)
  - Compute (computational power from 1 to 60 nodes)
  - Data Movement Services - DMS coordinates data movements between all the nodes.
- Azure HDInsight Cluster Types:
  - HDInsight = Apache Hortonworks Data Platform (HDP or Hadoop)
  - 99.9% availabbility
  - Hadoop, Spark, Hive, LLAP, Hbase, Storm, etc.
  - Works with HDFS (hadoop filesystem)
  - Yarn (job scheduling)
  - MapReduce (parallel processing)
  - Spark is an open source distributed compute engine.
  - Hive is data warehouse for querying structured data. Works with HiveQL. Translates SQL to MapReduce.
  - Hive LLAP is called interactive Query on Azure.
  - HBase = column oriented key value store.
  - Kafka to process streaming data, Storm for real-time analysis on streaming data.
- Azure Data Factory:
  - Fully managed ETL service in the Cloud.
  - Run SSIS packages in the Cloud.
  - Piplelines (locical group of activities), activity (processing step), dataset (source of the data), linked services (input and output services).
  - Azure Data Factory v2 is in preview.
- Azure Data Lake Analytics:
  - Azure Data Lake Analytics is an on-demand analytics job service that simplifies big data.
  - Instead of deploying, configuring, and tuning hardware, you write queries to transform your data and extract valuable insights.
  - Data Lake Analytics includes U-SQL, a query language that extends the familiar, simple, declarative nature of SQL with the expressive power of C#.
  - Data Lake Analytics deep integrates with Visual Studio.
- Azure Analysis Services:
  - Azure Analysis Services provides enterprise-grade data modeling in the cloud.
  - It is a fully managed platform as a service (PaaS), integrated with Azure data platform services.
  - With Analysis Services, you can mashup and combine data from multiple data sources, define metrics, and secure your data in a single, trusted semantic data model.

### Azure Serverless
- Serverless:
  - Function as a service, everything is managed, even the run-time.
  - Azure functions, Azure Logic Apps, Azure Bots, Machine Learning
  - Integration with Azure Storage and Azure Database via API's
  - Event-driven
  - No concept of continues running.
  - Code is expected to be small, just a function.
- Azure Functions Overview:
  - Respond to an event, e.g. call to url (http), timed events, Github webhook, Blob, etc.
  - Functions have a start and a finish
  - Funcntions have an input and output
  - Should be single purpose
  - Preferably a-synchronous
  - Languages supported: C#, F#, NodeJs, Java, PHP, Bash or anything executable.
  - Two models:
    - Paying for the consumption plan
    - Option to add a function to a App Service Plan, alongside with the web, mobile and API app.
  - Trigger: is somehting that can cause the function to run. 
    - Can be schedule, HTTP, Events from Event Hub, Storage events (Blovs, Queues), Queues and Topics from Service Bus, NoSQL DB from Cosmos DB, Microsoft Graph Events.
  - Binding: is an action that a function can use as input or ouput
    - Input bindings: Storage (Blobs and Tables), SQL Data, NoSQL data from Cosmos DB, Microsoft Graph (Excel, OneDrive, Events, Auth tokens, etc.)
    - Output bindings: HTTP output, Storage (Blobs, Queues, Tables), Events from Event Hub, Queues and topics from Service Bus, SQL Data, NoSQL Data from CosmosDB, Push Notifications, Twilio SMS, SendGrid Email, Microsoft Graph (Excel, Onedrive, Outlook email, Graph events)
- Logic Apps:
  - Workflow-as-a-Service
  - Very much like Windows Workflow Foundation
  - A task progresses through a series of steps
  - Triggers: time based/schedule, http, polling http, polling API, Webhook HTTP
  - Actions: Branching based on conditions (if-then), Looping, Calling an Azure API, Calling an external API, Invoke an Azure function, Wait, Manipulate data, Call other Logic App.
  - Logic App Builder:
    - Logic Apps can be built using a visual interface
    - Boxes and lines, drag and drop
    - Logic apps can be created without any coding
    - Dozen of connectors, Dropbox, Google Drive, Facebook, Twitter, Salesforce, etc.
  - ARM templates:
    - Logic Apps can be created as Azure Resource Manager templates
    - Same JSON template language used to create VMs or other Azure resources
    - Can use Powershell or CLI to deploy templates
    - Can be used for Desires State Configuration or keeping Logic App "code" in source control like Git
  - SDK: Vistual Studio 2015, Vistual Studio 2017
  
## Overview of all Azure Service plans, SLA's

### Virtual machines
- General purpose: A0-7, Av2, B, D, DS, Dv2, DSv2, Dv3, Dsv3
- Compute optimized: F, Fs, Fsv2
- Memory optimized: D, DS, Dv2, DSv2, Ev3, Esv3, G, GS, M
- Storage optimized: Ls
- GPU: NC, NCv2, NCv3, ND, NV
- High-performance: A8-11, H

### App Service Plans
- Free & Shared
  - 1 GB RAM
  - Custom domains
- Basic
  - 1.75, 3.5 or 7 GB RAM
  - 10 GB Storage
  - Custom domains
  - SSL
  - Up to 3 instances
- Standard:
  - 1.75, 3.5 or 7 GB RAM
  - 50 GB Storage
  - Custom domains
  - SSL
  - Daily backup
  - 5 deployment slots
  - Up to 10 instances
  - Traffic Manager
- Standard:
  - 1,2 or 4 cores
  - 1.75, 3.5 or 7 GB RAM
  - 250 GB Storage
  - Custom domains
  - SSL
  - 50 x Daily backup
  - 20 deployment slots
  - Up to 20 instances
  - Traffic Manager
- Isolated:
  - 1,2 or 4 cores
  - 3.5, 7 or 14 GB RAM
  - 250 GB Storage
  - Up to 100 instances
  - ASE, own VNET, Private app access

### Redis
- Basic: single node cache, NO SLA
- Standard: replicated between two nodes, SLA includes
- Premium: bigger workloads, better performance, disaster recovery, 53+ limit removed, persistence storage

### Azure Load Balancer
- Basic: free to use, supports monitoring and NAT
- Standard: scales up to 1000 VM's, diagnostics, NSG support, high reliability

### Azure VPN
- Basic: 10 site-to-site, 128 point-to-site, 100Mbps
- VPNGw1: 30 site-to-site, 128 point-to-site, 650Mbps
- VPNGw2: 30 site-to-site, 128 point-to-site, 1Gbps
- VPNGw3: 30 site-to-site, 128 point-to-site, 1.25Gbps

### Azure Search
- Free: Shared service, 50 MB, 3 indexes, 10.000 documents, no scale out
- Basic: 2 GB, 5 indexes, scale up to 3 units
- Standard S1: 25GB, 50 indexes, 36 units, 12 partitions and 12 replica's
- Standard S1: 200GB, 50 indexes, 36 units, 12 partitions and 12 replica's
- Standard S1: 200GB, 1000 indexes, 36 units, 12 partitions and 12 replica's

### Azure Service Catalog:
- Free: 5,000 data assets, all users
- Standard: 100.000 data assets, asset level security and visability

### SQL Server backup
- Basic: 7 days
- Standard: 35 days
- Premium: 35 days

### Azure Active Directory
- Free: basic features, 500.000 objects, SSO, support for Azure AD Connect
- Basic: no object limit, SLA 99.9%, Application proxy and self-service password support
- Premium P1: Advanced reporting, MFA, MDM auto-enrollment, cloud app discovery and azure ad connect health
- Premium P2: Identity protection and PIM

### Azure key vault
- Standard: offers geo scaling and availability
- Premium: adds additional support for Hardware security modules

### Azure security center
- Free: security policies, security assessments and recommendations
- Standard: Adds hybrid environment support, thread detection.

### Azure IoT Hub
- Free: 8.000 messages per unit a day
- Standard S1: 400.000 messages per unit a day
- Standard S2: 6.000.000 messages per unit a day
- Standard S3: 300.000.000 messages per unit a day

### Azure Event Hub
- Basic: 20 throughput units with 1 MB/s ingress and 2 MB/s egress. Message size = 256kb. Retention 1 day, 100 connections
- Standard: 20 throughput units with 1 MB/s ingress and 2 MB/s egress. Message size = 256kb. Retention 1 day, 1.000 connections
- Dedicated: 20 throughput units with 1 MB/s ingress and 2 MB/s egress. Message size = 1 MB. Retention 7 days, 25.000 connections

### Azure Service Bus
- Basic: 256kb messages
- Standard: de-duplication, topics, subscriptions, transactions, sessions
- Premium: 1 MB messages, de-duplication, topics, subscriptions, transactions, sessions

### Notification hubs
- Free: 1 million messages per month, 500 active devices, 100 hubs
- Basic: 10 million messages per month, 100 name spaces, 200.000 active devices, 100 hubs, SLA, limited telemetry
- Standard: 10 million messages per month, unlimited name spaces, 10.000.000 active devices, 100 hubs, SLA, rich telemetry, push, bulk import, multi-tendency
