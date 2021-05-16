||
|:-:|
| Disclaimer: This guide is being updated as I am preparing for the Exam 70-535. All the content referred here are available in public domain and not my creation. This is just a collection for easy reference. Use it with your own discretion. |
||

# Architecting Microsoft Azure Solutions (70-535)[^](https://www.microsoft.com/en-us/learning/exam-70-535.aspx)
## 1. Design Compute Infrastructure (20-25%)[^](https://docs.microsoft.com/en-us/azure/#pivot=products&panel=Compute)
### 1.1 Design solutions using virtual machines[^](notes/Compute.md#virtual-machines) 
* Design VM deployments by leveraging availability sets, fault domains, and update domains in Azure[^](notes/Compute.md#Compute.md#availability-sets-fault-domains-and-update-domains)
* Use web app for containers[^](notes/Compute.md#web-app-for-containers)
* Design VM Scale Sets[^](notes/Compute.md#vm-scale-sets)
* Design for compute-intensive tasks using Azure Batch[^](notes/Compute.md#compute-intensive-tasks-using-azure-batch)
* Define a migration strategy from cloud services[^](notes/Compute.md##migration-strategy-from-cloud-services)
* Recommend use of Azure Backup and Azure Site Recovery[^](notes/Compute.md#azure-backup)
 
### 1.2 Design solutions for serverless computing[^](https://azure.microsoft.com/en-in/overview/serverless-computing/) 
* Use Azure Functions to implement event-driven actions[^](notes/Compute.md#use-azure-functions-)
* Design for serverless computing using Azure Container Instances[^](notes/Compute.md#azure-container-instances)
* Design application solutions by using Azure Logic Apps, Azure Functions, or both[^](notes/Compute.md#design-application-solutions-by-using-azure-logic-apps-)[^](https://docs.microsoft.com/en-in/azure/azure-functions/functions-create-serverless-api)[^](https://docs.microsoft.com/en-in/azure/azure-functions/functions-compare-logic-apps-ms-flow-webjobs)
* Determine when to use API management service[^](notes/Compute.md#api-management-service)

### 1.3 Design microservices-based solutions[^](https://docs.microsoft.com/en-us/azure/#pivot=products&panel=containers)[^](notes/Compute.md#microservices)   
* Determine when a container-based solution is appropriate[^](notes/Compute.md#azure-container-instances)
* Determine when container-orchestration is appropriate[^](notes/Compute.md#container-orchestration)
* Determine when Azure Service Fabric (ASF) is appropriate[^](notes/Compute.md#azure-service-fabric-asf)
* Determine when Azure Functions is appropriate[^](https://docs.microsoft.com/en-in/azure/azure-functions/functions-overview)
* Determine when to use API management service[^](notes/Compute.md#api-management-service)
* Determine when Web API is appropriate[^](notes/Compute.md#custom-web-api)
* Determine which platform is appropriate for container orchestration[^](notes/Compute.md#container-orchestration)
* Consider migrating existing assets versus cloud native deployment[^](https://docs.microsoft.com/en-us/dotnet/standard/modernize-with-azure-and-containers/)
* Design lifecycle management strategies

### 1.4 Design web applications [^](https://docs.microsoft.com/en-us/azure/#pivot=products&panel=web)[^](notes/Compute.md#web-applications)
* Design Azure App Service Web Apps[^](https://docs.microsoft.com/en-us/azure/app-service/)[^](notes/Compute.md#azure-app-service-)
* Design custom web API [^](notes/Compute.md#custom-web-api)
* Secure Web API[^](notes/Compute.md#secure-web-api)
* Design Web Apps for scalability and performance[^](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/app-service-web-app/scalable-web-app)
* Design for high availability using Azure Web Apps in multiple regions[^](notes/Compute.md#multi-region-)
* Determine which App service plan to use[^](notes/Compute.md#app-service-plans)
* Design Web Apps for business continuity[^](notes/Compute.md#business-continuity)
* Determine when to use Azure App Service Environment (ASE)[^](notes/Compute.md#azure-app-service-environment-ase)
* Design for API apps[^](notes/Compute.md#api-apps)
* Determine when to use API management service[^](notes/Compute.md#api-management-service)
* Determine when to use Web Apps on Linux[^](notes/Compute.md#web-apps-on-linux)
* Determine when to use a CDN[^](notes/Compute.md#cdn)
* Determine when to use a cache, including Azure Redis cache[^](notes/Compute.md#using-cache)

### 1.5 Create compute-intensive application 
* Design high-performance computing (HPC) and other compute-intensive applications using Azure Services[^](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/high-performance-computing)[^](https://azure.microsoft.com/en-in/solutions/high-performance-computing/#references)
* Determine when to use Azure Batch[^](https://docs.microsoft.com/en-us/azure/batch/batch-technical-overview)
* Design stateless components to accommodate scale[^](https://blogs.msdn.microsoft.com/microsoft_press/2015/05/04/from-the-mvps-application-design-going-stateless-on-azure/)
* Design lifecycle strategy for Azure Batch[^](https://docs.microsoft.com/en-us/azure/batch/batch-api-basics)

#### Notes:
 * [Azure Compute](notes/Compute.md)

#### Useful links:
[Microsoft Azure Storage Performance and Scalability Checklist](https://docs.microsoft.com/en-in/azure/storage/storage-performance-checklist)

## 2. Design Data Implementation (15-20%)[^](https://docs.microsoft.com/en-us/azure/#pivot=products&panel=storage)[^](https://docs.microsoft.com/en-us/azure/#pivot=products&panel=databases)
### 2.1 Design for Azure Storage solutions
Determine when to use 
* Azure Blob Storage[^](https://docs.microsoft.com/en-in/azure/storage/blobs/storage-blobs-introduction) [^](https://docs.microsoft.com/en-us/azure/storage/common/storage-decide-blobs-files-disks)
* Blob tiers [^](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-storage-tiers)
* Azure Files [^](https://docs.microsoft.com/en-in/azure/storage/files/storage-files-introduction)
* Disks [^](https://docs.microsoft.com/en-in/azure/virtual-machines/windows/about-disks-and-vhds)
* StorSimple [^](https://docs.microsoft.com/en-us/azure/storsimple/storsimple-ova-overview)

#### Notes:
 * [Azure Storage](notes/DataStorage.md)
 * [Queues Comparision](notes/QueuesComparision.md)

### 2.2 Design for Azure Data Services
Determine when to use 
* Data Catalog[^](https://docs.microsoft.com/en-us/azure/data-catalog/data-catalog-what-is-data-catalog) [^](notes/Database.md#Azure-Data-Catalog)
* Azure Data Factory[^](https://azure.microsoft.com/en-us/services/data-factory/) [^](notes/Database.md#Azure-Data-Factory)
* SQL Data Warehouse[^](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/) [^](notes/Database.md#sql-data-warehouse)
* Azure Data Lake Analytics[^](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-overview) [^](notes/Database.md#Azure-Data-Lake-Analytics)
* Azure Analysis Services[^](https://docs.microsoft.com/en-us/azure/analysis-services/analysis-services-overview) [^](notes/Database.md#Azure-Analysis-Services) and
* Azure HDInsight[^](https://docs.microsoft.com/en-us/azure/hdinsight/) [^](notes/Database.md#azure-hdinsight)

### 2.3 Design for relational database storage
* Determine when to use
    * Azure SQL Database[^](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-paas-vs-sql-server-iaas)
    * SQL Server Stretch Database[^](notes/Database.md#stretch-database)
* Design for scalability[^](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-elastic-scale-introduction) and features[^](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-features)
* Determine when to use 
    * Azure Database for MySQL[^](https://docs.microsoft.com/en-us/azure/mysql/overview) 
    * Azure Database for PostgreSQL[^](https://docs.microsoft.com/en-us/azure/postgresql/overview)
* Design for HA/DR, geo-replication; design a backup and recovery strategy[^](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool)

#### Useful links:
* [Database Notes](notes/Database.md)
* [SQL Database Automated Backups](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-automated-backups)
* [Long-term backup retension upto 10 years using Azure Recovery Services vault](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-long-term-retention)
* [Business continuity overview](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-business-continuity)
* [Designing highly available services using Azure SQL Database](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery)
* [Disaster recovery strategies for applications using SQL Database elastic pools](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool)


### 2.4 Design for NoSQL storage
Determine when to use
* Azure Redis Cache[^](https://azure.microsoft.com/en-in/services/cache/)[^](notes/Database.md#azure-redis-cache),
* Azure Table Storage[^](https://azure.microsoft.com/en-in/services/storage/tables/) [^](notes/Database.md#azure-table-storage),
* Azure Data Lake[^](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-overview) [^](notes/Database.md#azure-data-lake-store),
* Azure Search[^](https://docs.microsoft.com/en-us/azure/search/search-what-is-azure-search)[^](notes/Database.md#azure-search),
* Time Series Insights[^](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-overview) [^](notes/Database.md#azure-time-series-insights)

### 2.5 Design for CosmosDB storage [^](https://azure.microsoft.com/en-us/services/cosmos-db/)
* Determine when to use 
    * MongoDB API[^](https://docs.microsoft.com/en-us/azure/cosmos-db/mongodb-introduction),
    * DocumentDB API[^](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction),
    * Graph API[^](https://docs.microsoft.com/en-us/azure/cosmos-db/graph-introduction),
    * Azure Tables API[^](https://docs.microsoft.com/en-us/azure/cosmos-db/table-introduction)
* Design for cost [^](https://docs.microsoft.com/en-us/azure/cosmos-db/key-value-store-cost), performance [^](https://docs.microsoft.com/en-us/azure/cosmos-db/partition-data), data consistency [^](https://docs.microsoft.com/en-us/azure/cosmos-db/consistency-levels), availability [^](https://docs.microsoft.com/en-us/azure/cosmos-db/online-backup-and-restore), and business continuity [^](https://docs.microsoft.com/en-us/azure/cosmos-db/regional-failover)

## 3. Design Networking Implementation (15-20%)[^](https://docs.microsoft.com/en-us/azure/#pivot=products&panel=network)
### 3.1 Design Azure virtual networks
* Design solutions that use Azure networking services[^](https://docs.microsoft.com/en-us/azure/networking/networking-overview)
* Design for load balancing using Azure Load Balancer[^](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-overview) and Azure Traffic Manager[^](https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-overview)
* Define DNS[^](https://docs.microsoft.com/en-us/azure/dns/dns-overview), DHCP, and IP strategies
* Determine when to use Azure Application Gateway[^](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-introduction)
* Determine when to use multi-node application gateways, Traffic Manager and load balancers

### 3.2 Design external connectivity for Azure Virtual Networks
* Determine when to use Azure VPN, ExpressRoute[^](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-introduction) and Virtual Network Peering architecture and design
* Determine when to use User Defined Routes (UDRs)[^](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-udr-overview)
* Determine when to use VPN gateway site-to-site failover for ExpressRoute[^](https://docs.microsoft.com/en-in/azure/architecture/reference-architectures/hybrid-networking/expressroute-vpn-failover)

### 3.3 Design security strategies
* Determine when to use network virtual appliances[^](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-scenario-udr-gw-nva)[^](https://docs.microsoft.com/en-in/azure/architecture/reference-architectures/dmz/nva-ha)[^](https://docs.microsoft.com/en-in/azure/architecture/reference-architectures/hybrid-networking/considerations)
* Design a perimeter network (DMZ)[^](https://docs.microsoft.com/en-in/azure/architecture/reference-architectures/dmz/secure-vnet-dmz)[^](https://docs.microsoft.com/en-in/azure/architecture/reference-architectures/dmz/)[^](https://docs.microsoft.com/en-us/azure/best-practices-network-security)
* Determine when to use a Web Application Firewall (WAF)[^](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-introduction#web-application-firewall), Network Security Group (NSG)[^](https://docs.microsoft.com/en-us/azure/virtual-network/security-overview#network-security-groups), and virtual network service tunneling[^](https://docs.microsoft.com/en-us/azure/security/azure-security-network-security-best-practices)

### 3.4 Design connectivity for hybrid applications
* Design connectivity to on-premises data from Azure applications using Azure Relay Service[^](https://docs.microsoft.com/en-us/azure/app-service/app-service-hybrid-connections),
 Azure Data Management Gateway for Data Factory[^](https://docs.microsoft.com/en-in/azure/data-factory/v1/data-factory-data-management-gateway), Azure On-Premises Data Gateway[^](https://docs.microsoft.com/en-us/azure/analysis-services/analysis-services-gateway), Hybrid Connections[^](https://docs.microsoft.com/en-in/azure/app-service/app-service-hybrid-connections), or Azure Web App’s virtual private network (VPN) capability[^](https://docs.microsoft.com/en-us/azure/app-service/environment/network-info)
* Identify constraints for connectivity with VPN[^](https://docs.microsoft.com/en-in/azure/architecture/reference-architectures/hybrid-networking/vpn)
* Identify options for joining VMs to domains[^](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/active-directory-ds-networking)

#### Notes:
 * [Networking](notes/Networking.md)

## 4. Design Security and Identity Solutions (20-25%)[^](https://docs.microsoft.com/en-us/azure/#pivot=products&panel=security)
### 4.1 Design an identity solution
* Design AD Connect synchronization[^](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect); 
* Design federated identities using Active Directory Federation Services (AD FS)[^](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-azure-adfs);
* Design solutions for Multi-Factor Authentication (MFA)[^](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/); 
* Design an architecture using Active Directory on-premises and Azure Active Directory (AAD)[^](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect); 
* Determine when to use Azure AD Domain Services[^](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/active-directory-ds-overview); 
* Design security for Mobile Apps using AAD[^](https://docs.microsoft.com/en-us/azure/app-service/app-service-authentication-overview)[^](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-authentication-scenarios)

#### Notes:
* [Azure AD Notes](notes/AzureAD.md)
* [Introduction to Azure Security](https://docs.microsoft.com/en-us/azure/security/azure-security)

### 4.2 Secure resources by using identity providers
* Design solutions that use external or consumer identity providers such as Microsoft account, Facebook, Google, and Yahoo;[^](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-overview) 
* Determine when to use Azure AD B2C and Azure AD B2B[^](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-compare-b2c); 
*  Design mobile apps using AAD B2C or AAD B2B [^](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-reference-oauth-code)
### 4.3 Design a data security solution
* Design data security solutions for Azure services[^](https://docs.microsoft.com/en-us/azure/security/security-azure-encryption-overview); 
* Determine when to use Azure Storage encryption[^](https://docs.microsoft.com/en-us/azure/storage/common/storage-service-encryption), Azure Disk Encryption[^](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption), Azure SQL Database security capabilities[^](https://docs.microsoft.com/en-us/azure/security/azure-database-security-overview), and Azure Key Vault[^](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis); 
* Design for protecting secrets in ARM templates using Azure Key Vault[^](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-keyvault-parameter); 
* Design for protecting application secrets using Azure Key Vault[^](https://docs.microsoft.com/en-us/azure/architecture/multitenant-identity/key-vault);
* Design a solution for managing certificates using Azure Key Vault[^](https://blogs.technet.microsoft.com/kv/2016/09/26/get-started-with-azure-key-vault-certificates/)[^](http://www.rahulpnath.com/blog/manage-certificates-in-azure-key-vault/); 
* Design solutions that use Azure AD Managed Service Identity[^](https://docs.microsoft.com/en-us/azure/active-directory/managed-service-identity/overview)

#### Note: The Azure SQL Database service is only available through TCP port 1433.

#### Useful Links :
* [Azure Database Security Best Practices](https://docs.microsoft.com/azure/security/azure-database-security-best-practices)
* [Determine hybrid identity lifecycle adoption strategy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy)

### 4.4 Design a mechanism of governance and policies for administering Azure resources
* Determine when to use Azure RBAC standard roles and custom roles[^](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-what-is); 
* Define an Azure RBAC strategy[^](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-configure);
* Determine when to use Azure resource policies[^](https://docs.microsoft.com/en-us/azure/azure-policy/azure-policy-introduction); 
* Determine when to use Azure AD Privileged Identity Management[^](https://docs.microsoft.com/en-us/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac); 
* Design solutions that use Azure AD Managed Service Identity[^](https://docs.microsoft.com/en-us/azure/app-service/app-service-managed-service-identity); 
* Determine when to use HSM-backed keys[^](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-hsm-protected-keys)[^](https://blogs.technet.microsoft.com/kv/2015/01/08/azure-key-vault-making-the-cloud-safer/)
### 4.5 Manage security risks by using an appropriate security solution
* Identify, assess, and mitigate security risks by using Azure Security Center[^](https://docs.microsoft.com/en-us/azure/security-center/security-center-get-started), Operations Management Suite Security and Audit solutions, and other services[^](https://docs.microsoft.com/en-us/azure/operations-management-suite/oms-security-getting-started); 
* Determine when to use Azure AD Identity Protection[^](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-identityprotection); 
* Determine when to use Advanced Threat Detection[^](https://docs.microsoft.com/en-us/azure/security/azure-threat-detection); 
* Determine an appropriate endpoint protection strategy[^](https://docs.microsoft.com/en-us/azure/security/azure-security-antimalware)[^](https://docs.microsoft.com/en-us/azure/security-center/security-center-install-endpoint-protection)

## 5. Design Solutions by using Platform Services (10-15%)[^](https://docs.microsoft.com/en-us/azure/#pivot=products&panel=ai)[^](https://docs.microsoft.com/en-us/azure/#pivot=products&panel=iot)[^](https://docs.microsoft.com/en-us/azure/#pivot=products&panel=integration)
### 5.1 Design for Artificial Intelligence Services
Determine when to use the appropriate
* Cognitive Services[^](https://azure.microsoft.com/en-us/services/cognitive-services/)[^](notes/PlatformServices.md#cognitive-services)
* Azure Bot Service[^](https://docs.microsoft.com/en-in/azure/bot-service/)[^](notes/PlatformServices.md#azure-bot-service)
* Azure Machine Learning[^](https://docs.microsoft.com/en-us/azure/machine-learning/)[^](notes/PlatformServices.md#azure-machine-learning)

and other categories that fall under cognitive AI

### 5.2 Design for IoT[^](https://docs.microsoft.com/en-in/azure/iot-fundamentals/iot-introduction)
Determine when to use 
* Stream Analytics[^](https://azure.microsoft.com/en-us/services/stream-analytics/)[^](notes/PlatformServices.md#azure-stream-analytics)
* IoT Hubs[^](https://azure.microsoft.com/en-us/services/iot-hub/)[^](notes/PlatformServices.md#iot-hub)
* Event Hubs[^](https://azure.microsoft.com/en-us/services/event-hubs/)[^](notes/PlatformServices.md#event-hub)
* real-time analytics[^]()
* Time Series Insights[^](https://azure.microsoft.com/en-us/services/time-series-insights/)[^](notes/Database.md#azure-time-series-insights)
* IoT Edge[^](https://azure.microsoft.com/en-us/services/iot-edge/)[^](notes/PlatformServices.md#iot-edge)
* Notification Hubs[^](https://docs.microsoft.com/en-in/azure/notification-hubs/)[^](notes/PlatformServices.md#notification-hub)
* Event Grid[^](https://azure.microsoft.com/en-us/services/event-grid/)[^](notes/PlatformServices.md#event-grid)

and other categories that fall under IoT[^](https://azure.microsoft.com/en-us/product-categories/iot/)
### 5.3 Design messaging solution architectures
* Design a messaging architecture [^](https://docs.microsoft.com/en-us/azure/architecture/patterns/category/messaging)
* determine when to use 
    * Azure Storage Queues[^](https://docs.microsoft.com/en-in/azure/service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted)
    * Azure Service Bus[^](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview)[^](https://docs.microsoft.com/en-us/azure/service-bus-messaging/)
    * Azure Event Hubs[^](notes/PlatformServices.md#event-hub)[^](https://docs.microsoft.com/en-us/azure/event-hubs/)
    * Event Grid[^](notes/PlatformServices.md#event-grid)[^](https://docs.microsoft.com/en-us/azure/event-grid/)
    * Azure Relay[^](notes/PlatformServices.md#azure-relay)
    * Azure Functions[^](https://azure.microsoft.com/en-in/services/functions/)[^](https://docs.microsoft.com/en-in/azure/azure-functions/functions-twitter-email)
    * Azure Logic Apps[^](https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services)[^](https://azure.microsoft.com/en-in/blog/events-data-points-and-messages-choosing-the-right-azure-messaging-service-for-your-data/)[^](notes/QueuesComparsion.md)
* design a push notification strategy for Mobile Apps [^](https://docs.microsoft.com/en-us/azure/notification-hubs/notification-hubs-enterprise-push-notification-architecture)
* design for performance[^](https://docs.microsoft.com/en-in/azure/service-bus-messaging/service-bus-performance-improvements) and scale[^](https://docs.microsoft.com/en-us/azure/architecture/patterns/category/performance-scalability)
### 5.4 Design for media service solutions
Define solutions using Azure Media Services[^](https://docs.microsoft.com/en-in/azure/media-services/media-services-overview), video indexer[^](https://docs.microsoft.com/en-in/azure/cognitive-services/video-indexer/video-indexer-overview), video API, computer vision API[^](https://azure.microsoft.com/en-in/services/cognitive-services/computer-vision/), preview, and other media related services

#### Notes:
* [Azure Platform Services](notes/PlatformServices.md)

## 6. Design for Operations (10-15%)[^](https://docs.microsoft.com/en-us/azure/#pivot=products&panel=mgmt)
### 6.1 Design an application monitoring and alerting strategy
* Determine the appropriate Microsoft products and services for monitoring applications on Azure [^](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview)
* Define solutions for analyzing logs and enabling alerts using Azure Log Analytics[^](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-overview)
* Define solutions for analyzing performance metrics and enabling alerts using Azure Monitor[^](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor/) 
* Define a solution for monitoring applications and enabling alerts using Application Insights[^](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-overview)

### 6.2 Design a platform monitoring and alerting strategy
* Determine the appropriate Microsoft products and services for monitoring Azure platform solutions; define a monitoring solution using Azure Health[^](https://docs.microsoft.com/en-us/azure/service-health/), Azure Advisor[^](https://azure.microsoft.com/en-us/services/advisor/), and Activity Log[^](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)
* Define a monitoring solution for Azure Networks using Log Analytics[^](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-networking-analytics) and Network Watcher service[^](https://azure.microsoft.com/en-us/services/network-watcher/)
* Monitor security with Azure Security Center[^](https://docs.microsoft.com/en-us/azure/security-center/)
### 6.3 Design an operations automation strategy
* Determine when to use Azure Automation[^](https://azure.microsoft.com/en-us/services/automation/)[^](notes/Operations.md#azure-automation), Chef[^](notes/Operations.md#chef), Puppet[^](https://puppet.com/products/managed-technology/microsoft-windows-azure)[^](notes/Operations.md#puppet), PowerShell[^](https://docs.microsoft.com/en-in/powershell/dsc/overview)[^](notes/Operations.md#powershell), Desired State Configuration (DSC)[^](https://docs.microsoft.com/en-us/azure/automation/automation-dsc-overview)[^](notes/Operations.md#desired-state-configuration-), Event Grid[^](https://azure.microsoft.com/en-us/services/event-grid/)[^](notes/Operations.md#azure-event-grid), and Azure Logic Apps[^](https://azure.microsoft.com/en-us/services/logic-apps/)[^](notes/Operations.md#azure-logic-apps);
* Define a strategy for auto-scaling[^](https://azure.microsoft.com/en-us/features/autoscale/)[^](notes/Operations.md#azure-autoscale);
* Define a strategy for enabling periodic processes and tasks[^](https://docs.microsoft.com/en-us/azure/scheduler/scheduler-intro)[^](notes/Operations.md#azure-scheduler)

#### Notes:
* [Monitoring and Alerting, Operations Automation](notes/Operations.md)

#### Useful links
* [Avail free training](https://azure.microsoft.com/en-us/training/learning-paths/azure-solution-architect/)
* [Books to study](https://buildazure.com/2018/02/01/book-exam-ref-70-535-architecting-microsoft-azure-solutions/) [^](http://amzn.to/2Fsg5IG)
* [Azure Services](https://azure.microsoft.com/en-in/services/)


## Azure Active Directory [^](https://azure.microsoft.com/documentation/articles/active-directory-whatis/)

* Azure AD is a directory service in Azure
* Azure AD Service is not a replacement for Windows Server Active Directory
* **Azure AD Connect** synchronization services used to synch on-prem AD groups and users to Azure AD, which is called **Hybrid Identity**
* AD Connect is successor to DirSync, AzureADSync, Forefront Identity Manager.
* Azure AD can be associated with an on-premises Active Directory to support single sign-on (SSO).
* True SSO using Active Directory Federation Services (AD FS) or shared sign-on, in which Azure AD Connect is used to sync a password hash between Active Directory and Azure AD
* Azure AD is accessible via a modern REST API, Graph REST APIs.

Azure AD serves as key component for identity management in MS cloud and has the following capabilities:
* Multi-Factor Authentication,
* Device registration,
* Role Based Access Control (RBAC),
* Application usage monitoring,
* Security Monitoring and Alerting,
* Self-serivce password management

Important Azure AD features:
* Azure AD B2C (business to consumer)[^](https://azure.microsoft.com/documentation/articles/active-directory-b2c-overview/)

    - Leverages existing social accounts(Facebook, MS, Google, Amazon, LinkedIn) or custom accounts
    - It is the evolution of Azure AD Access Control Sevice - classic service (ACS) 

* Azure AD B2B (business to business)[^](https://azure.microsoft.com/documentation/articles/active-directory-b2b-collaboration-overview/)

    - enable access to your organization’s applications from external business partner identities
    - allows your business partners to use their own authentication credentials
* Azure AD Application Proxy[^](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/)
    - enables users to leverage SSO to securely access on-premises web applications such as SharePoint sites and Outlook Web Access—without the need for a VPN.
    - Application Proxy is available for the Basic and Premium editions of Azure AD

* Azure AD Directory Join [^](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-windows10-devices-overview/)

    - Directory Join enables Windows 10 devices to connect with Azure AD.
    - Allows users to sign-in to Windows using Azure AD accounts, will enable SSO to Azure resources, Windows Store, device access restriction using group policy.
    - Suitable for devices that cannot domain join

* Azure AD Domain Services [^](https://azure.microsoft.com/documentation/articles/active-directory-ds-overview/)
    - Domain Services provide fully managed domain services such as domain join, group policy, LDAP, Kerberos/NTLM, and so on that are compatible with Windows Server Active Directory.

* Azure AD Device Registration [^](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access-device-registration-overview/)
    - enables mobile devices (such as iOS, Android, and Windows devices) to be registered in Azure AD
   - enable conditional access to on-premises or Office 365 applications

* Azure AD Cloud App Discovery[^](https://azure.microsoft.com/documentation/articles/active-directory-cloudappdiscovery-whatis/)

    - finds the applications being used (usage metrics), identifies users, and enables offline data analysis.
    - enables IT departments to discover cloud applications used in their organization
    - allows the applications to be brought under IT control to help mitigate risk of potential data leakage or other security threats

* Azure AD Connect Health [^](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-health/)

    - enables you to monitor and gain insights into the overall health of the integration between your on-premises Windows Server Active Directory/Active Directory Federation Service and Azure AD (or Office 365).

* Azure AD Identity Protection [^](https://azure.microsoft.com/documentation/articles/active-directory-identityprotection/) 

    - is a security service that enables you to gain insights into potential security vulnerabilities affecting users in your organization (more specifically, their identities)

### Active Directory editions [^](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-whatis#choose-an-edition) [^](https://azure.microsoft.com/en-us/pricing/details/active-directory/)

* **Free** - manage users, synchronize with on-premises Active Directory, establish SSO across Azure and Office 365, and access SaaS applications in the Azure AD application gallery

* **Basic** - Free tier, plus self-service password resets, group-based application access, customizable branding, Azure AD Application Proxy, and a 99.9 percent availability service level agreement (SLA)

* **Premium (P1, P2)** - Free and Basic tiers, plus self-service group management, advanced security reports and alerts, Multi-Factor Authentication, and licenses for Microsoft Identity Manager. P2: provides Identity Protection and Privileged Identity Management.

| **Basic** | **Premium (P1 & P2)**|
|---:|:---|
|99.9% uptime SLA | Includes Microsoft Identity Manager (MIM) 2016 (an on-premises identity and access management suite)|
| Self-service password reset |  Self-service password reset with write-back |
| Azure AD Join for Windows 10 | Multi-facator authentication (MFA) |
| SSO for 10 apps / user |  No SSO app limit |
| Azure AD Application Proxy | MDM auto-enrollment|
| | P2: Identity Protection and Privileged Identity Management|

### Azure Backup[^](https://docs.microsoft.com/en-us/azure/backup/backup-introduction-to-azure-backup)

Key features:
* Automatic Storage Management (Pay-as-you-use)
* Unlimited Scaling
* Multiple storage options (LRS and GRS)
* Unlimited data transfer (no limit and charge for inbound and outbound data transfers)
* Data encryption
* Application-consistent backup
* Long-term retention

### Azure Backup Components[^](https://docs.microsoft.com/en-us/azure/backup/backup-introduction-to-azure-backup#which-azure-backup-components-should-i-use)


||||||
|:--|:--|:--|:--|:--|
|**Component**|**Benefits**|**Limits**|**What is protected?**|**Where are backups stored?**|
|Azure Backup Agent (MARS)|<ul><li>Backup files and folders on Physical or virtual windows OS</li> <li> No backup server required </li> </ul>|<ul><li> Backup 3x per day</li><li>Not application aware; file, folder, and volume-level restore only</li> <li>**No support for Linux**</li> </ul>| <ul> <li>Files</li> <li> Folders </li> <li> System State</li></ul>| Recovery Services vault |
|System Center DPM|<ul><li>Application-aware snapshots (VSS)</li> <li>Linux support on Hyper-V and VMware VMs</li></ul>|<ul><li>Cannot back up Oracle workload</li></ul>|<ul><li>Files</li><li> Folders</li><li>Volumes</li><li>VMs</li><li>Applications</li><li>Workloads</li><li>System State</li></ul>| <ul><li> Recovery Services vault</li> <li>Locally attached disk</li><li>Tapes (on-prem)</li></ul>|
|Azure Backup Server | <ul> <li>Does not require System Center license</li></ul>|<ul><li>Cannot back up Oracle workload</li><li>Always requires live Azure subscription</li><li>No support for tape backup</li> </ul>|<ul><li>Files</li><li>Folders</li><li>Volumes</li><li>VMs</li><li>Applications</li><li>Workloads</li><li>System State</li></ul>| <ul><li> Recovery Services vault</li> <li>Locally attached disk</li></ul>|
|Azure IaaS VM Backup|<ul><li>Native backups for Windows/Linux</li><li>No specific agent installation required</li><li>Fabric-level backup with no backup infrastructure needed</li></ul>|<ul><li>Back up VMs once-a-day</li><li>Restore VMs only at disk level</li><li> **Cannot back up on-premises**</li></ul>|<ul><li>VMs</li><li>All disks (using PowerShell)</li></ul>|Recovery Services vault|
||||||


### Concepts that can help you make important decisions around backup and disaster recovery[^](https://docs.microsoft.com/en-in/azure/backup/backup-introduction-to-azure-backup#how-does-azure-backup-differ-from-azure-site-recovery)
|||||
|:--|:--|:--|:--|
| **Concept** | **Details** | **Backup** | **Disaster recovery (DR)** |
| Recovery point objective (RPO) | The amount of acceptable data loss if a recovery needs to be done. | Backup solutions have wide variability in their acceptable RPO. Virtual machine backups usually have an RPO of one day, while database backups have RPOs as low as 15 minutes. | Disaster recovery solutions have low RPOs. The DR copy can be behind by a few seconds or a few minutes.|
| Recovery time objective (RTO) |The amount of time that it takes to complete a recovery or restore. | Because of the larger RPO, the amount of data that a backup solution needs to process is typically much higher, which leads to longer RTOs. For example, it can take days to restore data from tapes, depending on the time it takes to transport the tape from an off-site location. | Disaster recovery solutions have smaller RTOs because they are more in sync with the source. Fewer changes need to be processed.|
| Retention | How long data needs to be stored | For scenarios that require operational recovery (data corruption, inadvertent file deletion, OS failure), backup data is typically retained for 30 days or less. From a compliance standpoint, data might need to be stored for months or even years. Backup data is ideally suited for archiving in such cases. | Disaster recovery needs only operational recovery data, which typically takes a few hours or up to a day. Because of the fine-grained data capture used in DR solutions, using DR data for long-term retention is not recommended.|
|||||


# Compute[^](https://azure.microsoft.com/en-us/product-categories/compute/)

|||
|:--|:--|
| **If you want to...** | **Use this** |
|Provision Linux and Windows virtual machines in seconds with the configurations of your choice | Virtual Machines |
|Achieve high availability by autoscaling to create thousands of VMs in minutes | Virtual Machine Scale Sets |
|Simplify the deployment, management, and operations of Kubernetes with a fully managed service| Azure Container Service (AKS) |
|Accelerate app development using an event-driven, serverless architecture| Functions|
|Develop microservices and orchestrate containers on Windows and Linux| Service Fabric|
|Quickly create cloud apps for web and mobile with fully managed platform| App Service |
|Containerize apps and easily run containers with a single command| Container Instances |
|Cloud-scale job scheduling and compute management with the ability to scale to tens, hundreds, or thousands of virtual machines | Batch |
|Create highly available, scalable cloud applications and APIs that help you focus on apps instead of hardware | Cloud Services |
|||


## Virtual Machines[^](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal) 
* Azure Virtual Machines (VM) is one of several types of on-demand, scalable computing resources that Azure offers

||||
|:--|:--|:--|
| VM Type | Sizes | Description|
| General Purpose | B, Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7 | Balanced CPU-to-memory ratio. Ideal for testing and development, small to medium databases, and low to medium traffic web servers.|
| Compute Optimized | Fsv2, Fs, F |High CPU-to-memory ratio. Good for medium traffic web servers, network appliances, batch processes, and application servers.|
| Memory Optimized | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D | High memory-to-CPU ratio. Great for relational database servers, medium to large caches, and in-memory analytics.|
| Storage Optimized | Ls |  High disk throughput and IO. Ideal for Big Data, SQL, and NoSQL databases.|
| GPU | NV, NC, NCv2, NCv3, ND |Specialized virtual machines targeted for heavy graphic rendering and video editing. Available with single or multiple GPUs.|
| High Performance Compute | H, A8-11 | Our fastest and most powerful CPU virtual machines with optional high-throughput network interfaces (RDMA).
||||

### Availability sets, fault domains, and update domains[^](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/tutorial-availability-sets)[^](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/regions-and-availability#availability-sets)[^](https://docs.microsoft.com/en-in/azure/virtual-machines/windows/manage-availability)
* An Availability Set is a logical grouping capability that Azure ensures VM resources are isolated. The VMs you place within an Availability Set run across multiple physical servers, compute racks, storage units, and network switches.
* An update domain is a group of VMs and underlying physical hardware that can be rebooted at the same time
* VMs in the same fault domain share common storage as well as a common power source and network switch.
* You can't add an existing VM to an availability set after it is created.
* A fault domain is a logical group of underlying hardware that share a common power source and network switch, similar to a rack within an on-premises datacenter.
* An update domain is a logical group of underlying hardware that can undergo maintenance or be rebooted at the same time. 

### Web app for containers[^](https://azure.microsoft.com/en-in/services/app-service/containers/)
* Deploy container-based web apps by just pulling container images from Docker Hub or a private Azure Container Registry
* Web App for Containers will deploy the containerised app with your preferred dependencies to production in seconds
* The platform automatically takes care of OS patching, capacity provisioning and load balancing.
* Streamline CI/CD with Docker Hub, Azure Container Registry and GitHub
* App Service on Linux is only supported with Basic and Standard app service plans and does not have a Free or Shared tier.

### VM Scale Sets[^](https://azure.microsoft.com/en-in/services/virtual-machine-scale-sets/)[^](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview)
* Azure virtual machine scale sets let you create and manage a group of  **identical**, load balanced VMs. 
* Azure virtual machine scale sets provide the management capabilities for applications that run across many VMs, automatic scaling of resources, and load balancing of traffic.
* VM scale sets are designed to support true auto-scale – no pre-provisioning of VMs is required – and as such makes it easier to build large-scale services targeting big compute, big data, and containerized workloads.
* You can set the maximum, minimum and default number of VMs, and define triggers – action rules based on resource consumption.
* Differences between virtual machines and scale sets[^](https://docs.microsoft.com/en-in/azure/virtual-machine-scale-sets/overview#differences-between-virtual-machines-and-scale-sets)
### Compute-intensive tasks using Azure Batch[^](https://docs.microsoft.com/en-us/azure/batch/)[^](#azure-batch)
### Migration strategy from Cloud Services[^](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cloud-services-migration-differences)[^](https://docs.microsoft.com/en-us/azure/architecture/service-fabric/migrate-from-cloud-services)[^](https://azure.microsoft.com/en-us/documentation/learning-paths/cloud-services/)[^](https://docs.microsoft.com/en-in/azure/cloud-services/cloud-services-choose-me)
* There are two types of Azure Cloud Services roles:
    * Web role: Automatically deploys and hosts your app through IIS.
    * Worker role: Does not use IIS, and runs your app standalone.
### Azure Backup[^](https://docs.microsoft.com/en-in/azure/backup/backup-introduction-to-azure-backup)[^](AzureBackup.md)
Components:[^](https://docs.microsoft.com/en-in/azure/backup/backup-introduction-to-azure-backup#which-azure-backup-components-should-i-use)
* Azure Backup (MARS) agent - Back up files and folders on physical or virtual Windows OS, supports on-prem VMs, No Linux support. Stores backup in Recovery Services Vault (RSV).
* System Center DPM - Application-aware snapshots, Cannot back up Oracle workload. RSV, Local disk, Tape (on-prem)
* Azure Backup Server - Application-aware snapshots, Connot back up Oracle, No Tape support. RSV, Local Disk
* Azure IaaS VM Backup - Native backups for Windows/Linux, Fabric-level backup with no backup infrastructure needed, Back up VMs once-a-day, Restore VMs at disk level, Cannot back up on-premises. RSV

### Azure Site Recovery[^](https://docs.microsoft.com/en-in/azure/site-recovery/site-recovery-overview)
* Azure Recovery Services contribute to your BCDR strategy:
    * Site Recovery service: Site Recovery replicates workloads running on physical and virtual machines (VMs) from a primary site to a secondary location during outages
    * Backup Service: The Azure Backup service keeps your data safe and recoverable by backing it up to Azure.
* Azure VMs replicating between Azure regions.
* On-premises VMs and physical servers replicating to Azure, or to a secondary site.

#### Useful Links:
1. [Manage Availability sets](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability) 
2. [Azure Exam Prep – Fault Domains and Update Domains](https://blogs.msdn.microsoft.com/plankytronixx/2015/05/01/azure-exam-prep-fault-domains-and-update-domains/) 
3. [Use Infrastructure Automation Tools with VMs in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/infrastructure-automation) 
4. [Use a custom Docker image for Web App for Containers](https://docs.microsoft.com/en-us/azure/app-service/containers/tutorial-custom-docker-image) 
5. [Run intrinsically parallel workloads with Batch](https://docs.microsoft.com/en-us/azure/batch/batch-technical-overview) 
6. [About Azure Migrate](https://docs.microsoft.com/en-us/azure/migrate/migrate-overview) 
7. [Back up Azure virtual machines to a Recovery Services vault](https://docs.microsoft.com/en-us/azure/backup/backup-azure-arm-vms)

#### Recommended high availability "best practices" for virtual machines deployment:
- Configure multiple virtual machines in an availability set - for redundancy
- Use managed disks for VMs in an availability set
- Use Scheduled Events to proactively response to VM impacting events
- Configure each application tier into separate availability sets
- Combine a Load Balancer with availability sets
- Use availability zones to protect from datacenter level failures

## Serverless Computing[^](https://azure.microsoft.com/en-in/overview/serverless-computing/)
* Serverless computing is the abstraction of servers, infrastructure and operating systems.
* Serverless computing is driven by the reaction to events and triggers happening in near-real-time—in the cloud

### Web Jobs[^](https://docs.microsoft.com/en-in/azure/app-service/web-sites-create-web-jobs)

### Use Azure Functions to implement event-driven actions[^](https://docs.microsoft.com/en-in/azure/azure-functions/functions-overview)
* Azure Functions is a serverless compute service that enables you to run code on-demand without having to explicitly provision or manage infrastructure
* Azure Functions is a solution for easily running small pieces of code, or "functions," in the cloud
* Key features of Functions are:
    * Choice of language
    * Pay-per-use pricing model
    * Bring your own dependecies
    * Integrated Security
    * Simplified integration
    * Flexible development
    * Open-source
* Functions is a great solution for processing data, integrating systems, working with the internet-of-things (IoT), and building simple APIs and microservices
* Functions provides templates to get you started with key scenarios, including the following:
    * HTTPTrigger
    * TimerTrigger
    * GitHub webhook
    * Generic webhook
    * CosmosDBTrigger
    * BlobTrigger
    * QueueTrigger
    * EventHubTrigger
    * ServiceBusQueueTrigger
    * ServiceBusTopicTrigger
* A trigger defines how a function is invoked. A function must have exactly one trigger. Triggers have associated data, which is usually the payload that triggered the function.
* Input and output bindings provide a declarative way to connect to data from within your code. Bindings are optional and a function can have multiple input and output bindings.

### Azure Container Instances
* Azure Container Instances is a great solution for any scenario that can operate in isolated containers, including simple applications, task automation, and build jobs.
* For scenarios where you need full container orchestration, including service discovery across multiple containers, automatic scaling, and coordinated application upgrades, use Azure Container Service (AKS).
* Benifits of Containers:
    * Fast startup times
    * Public IP connectivity and DNS name
    * Hypervisor-level security
    * Custom Sizes
    * Linux and Windows Containers
    * Co-scheduled groups
* Because of their small size and application orientation, containers are well suited for agile delivery environments and microservice-based architectures. 

###  Design application solutions by using Azure Logic Apps, Azure Functions, or both[^](https://docs.microsoft.com/en-in/azure/logic-apps/logic-apps-serverless-overview)
* Serverless applications offer benefits of an increase in speed of development, reduction in required code, and simplicity with scale.
* The core services in Azure around Serverless are Azure Functions and Azure Logic Apps. 
* Azure Functions is a solution for easily running small pieces of code, or "functions," in the cloud.
* Azure Logic Apps provides a way to simplify and implement scalable integrations and workflows in the cloud.

### Determine when to use API management service[^](#api-management-service)

Serverless Comparision[^](https://docs.microsoft.com/en-in/azure/azure-functions/functions-compare-logic-apps-ms-flow-webjobs)

## Microservices[^](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-overview-microservices)[^](https://azure.microsoft.com/en-in/blog/microservices-an-application-revolution-powered-by-the-cloud/)
### when to use Container-based solution

### Container-orchestration[^](https://docs.microsoft.com/en-us/azure/aks/intro-kubernetes)
*  The task of automating and managing a large number of containers and how they interact is known as orchestration. Popular container orchestrators include Kubernetes, DC/OS, and Docker Swarm.
* Azure Container Service (AKS) makes it simple to create, configure, and manage a cluster of virtual machines that are preconfigured to run containerized applications.
* As a managed Kubernetes service, AKS provides:
    * Automated Kubernetes version upgrades and patching
    * Easy cluster scaling
    * Self-healing hosted control plane (masters)
    * Cost savings - pay only for running agent pool nodes
* Kubernetes provides a distributed platform for containerized applications. With AKS, provisioning of a production ready Kubernetes cluster is simple and quick.

### Azure Service Fabric (ASF)[^](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-overview)
* Azure Service Fabric is a distributed systems platform that makes it easy to package, deploy, and manage scalable and reliable microservices and containers.
* Service Fabric provides three broad areas to help you build applications that use a microservices approach:
    * A platform that provides system services to deploy, upgrade, detect, and restart failed services, discover services, route messages, manage state, and monitor health. 
    * Ability to deploy applications either running in containers or as processes. Service Fabric is a container and process orchestrator.
    * Productive programming APIs, to help you build applications as microservices: ASP.NET Core, Reliable Actors, and Reliable Services.
* A Service Fabric cluster is a network-connected set of virtual or physical machines into which your microservices are deployed and managed. Clusters can scale to thousands of machines.
* A stateful service uses Service Fabric to manage your service's state via its Reliable Collections or Reliable Actors programming models.
* A key differentation with Service Fabric is its strong focus on building stateful services, either with the built in programming models or with containerized stateful services
* Service Fabric supported programming models:[^](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-choose-framework)
    * Containers
    * Reliable Services
    * Reliable Actors - framework uses independent units of compute and state with single-threaded execution called actors
    * ASP.NET Core
    * Guest executables

* Service Fabric Application Scenarios[^](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-application-scenarios) - The Service Fabric platform in Azure is ideal for the following categories of applications:
    * Highly available services - provide fast failover
    * Scalable services - services can be partitioned, allowing for state to be scaled out across the cluster
    * Computation on nonstatic data - allows the collocation of processing (computation) and data in applications
    * Session-based interactive applications - such as online gaming or instant messaging that require low latency reads and writes
    * Data analytics and workflows - transactional and financial systems - handles large scale and has low latency through its stateful services

#### Comparision of Azure App Service, Virtual Machines, Service Fabric, and Cloud Services [^](https://docs.microsoft.com/en-in/azure/app-service/choose-web-site-cloud-service-vm)
#### When to use what?
|||||
|:--|:--|:--|:--|
||Azure Container Services|Azure Container Instances|Azure Service Fabric|
|production deployments of complex systems (with a container orchestrator)|X|||
|For running simple configurations (possibly without orchestrator)||X||
|For long-running workloads on containers|X|
|For short-running workloads on containers||X||
|For orchestrating a system based on containers|X||X|
|Orchestrating with open-source orchestrators (DC/OS, Docker Swarm, Kubernetes)|X|||
|Orchestrating with built-in orchestrator|||X|
|||||

### Determine when Azure Functions is appropriate[^](#use-azure-function-)[^](https://docs.microsoft.com/en-us/azure/azure-functions/functions-compare-logic-apps-ms-flow-webjobs#compare-azure-functions-and-azure-logic-apps)
### Determine when Web API is appropriate[^](https://azure.microsoft.com/en-in/services/app-service/api/)
* App Service has built-in support for Cross-Origin Resource Sharing (CORS) for RESTful APIs.

### Platform for container orchestration[^](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-orchestrator-relationship)
### Migrating existing assets versus cloud native deployment[^](https://docs.microsoft.com/en-us/dotnet/standard/modernize-with-azure-and-containers/)
### Lifecycle management strategies[^](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-package-apps)

## Web Applications
[ASP.NET Overview](https://docs.microsoft.com/en-us/aspnet/overview)

### Azure App Service Web Apps[^](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-overview)
Azure App Service Web Apps (or just Web Apps) is a service for hosting web applications, REST APIs, and mobile back ends.

Key features of App Service Web Apps:
* Multiple languages and frameworks support
* DevOps optimization - CI/CD, staging, PowerShell, CLI
* Global scale with high availability - Scale up or Out
* Connections to SaaS platforms and on-premises data - more than 50 connetors, Hybrid connections, and Azure VNets
* Security and Compliance - ISO, SOC and PCI compliant, Authenticate using AAD, or social login. IP restrictions (Firewall), manage service Identities
* Application templates
* Visual Studio Integration
* API and mobile features
* Serverless code

### Custom Web API[^](https://docs.microsoft.com/en-in/azure/logic-apps/logic-apps-create-api-app)
* Custom APIs and custom connectors are web APIs that use REST for pluggable interfaces, Swagger metadata format for documentation, and JSON as their data exchange format. 
* A custom API works best with logic apps when the API also has a Swagger document that describes the API's operations and parameters.
* Custom API should provide actions. A basic action is a controller that accepts HTTP requests and returns HTTP responses.
* Action Patterns
    * Perform long-running tasks with the **polling action pattern**
    * Perform long-running tasks with the **webhook action pattern**
* Trigger patterns
    * Check for new data or events regularly with the **polling trigger pattern**
    * Wait and listen for new data or events with the **webhook trigger pattern**

#### [On-Premises PowerBI Gateway](https://docs.microsoft.com/en-in/power-bi/service-gateway-getting-started)
A gateway is software that facilitates access to data that resides on a private, on-premises network, for subsequent use in a cloud service like Power BI.
#### [App Service Hybrid Connections](https://docs.microsoft.com/en-in/azure/app-service/app-service-hybrid-connections)

### Secure Web API [^](https://docs.microsoft.com/en-us/aspnet/web-api/overview/security/)[^](https://docs.microsoft.com/en-in/azure/logic-apps/logic-apps-custom-api-authentication)
* Authentication and Authorization in Web API
    * *Authentication* is knowing the identity of the user
    * *Authorization* is deciding whether a user is allowed to perform an action
    * Web API assumes that authentication happens in the host(IIS), IIS uses HTTP modules for authentication.
    * Alernatively, authentication logic can be put into an HTTP message handler
    * Authorization happens later in the pipeline, closer to the controller.
    * Authorization filters run before the controller action
    * Web API provides a built-in authorization filter, **AuthorizeAttribute**.
    
* Secure a Web API with Individual Accounts in Web API 2.2
    * Secure a web API using OAuth2 to authenticate against a membership database
    * Web API project template gives you three options for authentication:
        * Individual accounts. The app uses a membership database.
        * Organizational accounts. Users sign in with their Azure Active Directory, Office 365, or on-premise Active Directory credentials.
        * Windows authentication. This option is intended for Intranet applications, and uses the Windows Authentication IIS module.
    * Individual accounts provide two ways for a user to log in:
        * Local Login - The user registers at the site
        * Social Login - The user signs in with external social media account
    * For both local and social login, Web API uses OAuth2 to authenticate requests
* External Authentication Services with Web API (C#)
    * to integrate with external authentication services, which include several OAuth/OpenID and social media authentication services: Microsoft Accounts, Twitter, Facebook, and Google.

* Preventing Cross-Site Request Forgery (CSRF) Attacks in Web API
    * Cross-Site Request Forgery (CSRF) is an attack where a malicious site sends a request to a vulnerable site where the user is currently logged in
    * Using SSL does not prevent a CSRF attack, because the malicious site can send an "https://" request.
    * CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site. Basic and Digest authentication are also vulnerable
    * To help prevent CSRF attacks, ASP.NET MVC uses anti-forgery tokens, also called request verification tokens.
    * Same-origin policies prevent documents hosted on two different sites from accessing each other's content
* Enabling Cross-Origin Requests in Web API 2
    * Browser security prevents a web page from making AJAX requests to another domain. This restriction is called the same-origin policy, and prevents a malicious site from reading sensitive data from another site. 
    * Cross Origin Resource Sharing (CORS) is a W3C standard that allows a server to relax the same-origin policy. 
    * Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.
* Authentication Filters in Web API 2
    * An authentication filter is a component that authenticates an HTTP request
    * Authentication filters let you set an authentication scheme for individual controllers or actions.
    * authentication filters can be applied per-controller, per-action, or globally to all Web API controllers.
    * "Host-level authentication" is authentication performed by the host (such as IIS), before the request reaches the Web API framework.
    * [Web API Security Filter](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)
* Basic Authentication in Web API
    * Basic authentication is performed within the context of a "realm."
    * The credentials are sent unencrypted, Basic authentication is only secure over HTTPS.
    * Basic authentication is also vulnerable to CSRF attacks
* Forms Authentication in Web API
    * Forms authentication uses an HTML form to send the user's credentials to the server.
    * Forms authentication is only appropriate for web APIs that are called from a web application, so that the user can interact with the HTML form.
    * Advantages:
        * Easy to implement: Built into ASP.NET.
        * Uses ASP.NET membership provider, which makes it easy to manage user accounts
    * Disadvantages:
        * Not a standard HTTP authentication mechanism;
        * uses HTTP cookies instead of the standard Authorization header.
        * Requires a browser client.
        * Credentials are sent as plaintext.
        * Vulnerable to cross-site request forgery (CSRF).
        * Requires anti-CSRF measures.
        * Difficult to use from nonbrowser clients.
        * Login requires a browser.
        * User credentials are sent in the request. Some users disable cookies.
* Integrated Windows Authentication
    * Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.
    * The client sends credentials in the Authorization header.
    * Windows authentication is best suited for an intranet environment
    * Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks
    * Advantages:
        * Built into IIS.
        * Does not send the user credentials in the request.
        * If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials
    * Disadvantages:
        * Not recommended for Internet applications.
        * Requires Kerberos or NTLM support in the client.
        * Client must be in the Active Directory domain.
* Working with SSL
    * Basic authentication and forms authentication send unencrypted credentials. To be secure, these authentication schemes must use SSL. 
    * In addition, SSL client certificates can be used to authenticate clients.
    * SSL provides authentication by using Public Key Infrastructure certificates
    * Advantages:
        * Certificate credentials are stronger than username/password.
        * SSL provides a complete secure channel, with authentication, message integrity, and message encryption.
    * Disadvantages:
        * must obtain and manage PKI certificates.
        * The client platform must support SSL client certificates.


### Web Apps for scalability and performance[^](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/app-service-web-app/scalable-web-app)

#### Azure App Service Local Cache[^](https://docs.microsoft.com/en-in/azure/app-service/app-service-local-cache-overview#best-practices-for-using-app-service-local-cache)
* Provides a web role view of your content. Web apps that run on Local Cache have the following benefits:
    * They are immune to latencies that occur when they access content on Azure Storage.
    * They are immune to the planned upgrades or unplanned downtimes and any other disruptions with Azure Storage that occur on servers that serve the content share.
    * They have fewer app restarts due to storage share changes.
* The local cache is a copy of the /site and /siteextensions folders of the web app.
* Use Local Cache if your web app needs a high-performance, reliable content store, that does not use the content store to write critical data at runtime, and is less than 2 GB in total size 

#### SignalR[^](https://docs.microsoft.com/en-us/aspnet/signalr/overview/getting-started/introduction-to-signalr)
* ASP.NET SignalR is a library for ASP.NET developers that simplifies the process of adding real-time web functionality to applications.
* Real-time web functionality is the ability to have server code push content to connected clients instantly as it becomes available, rather than having the server wait for a client to request new data.
* SignalR can be used to add any sort of "real-time" web functionality to your ASP.NET application.
* Examples include Chats, dashboards and monitoring applications, collaborative applications (such as simultaneous editing of documents), job progress updates, and real-time forms.
* The SignalR API contains two models for communicating between clients and servers: Persistent Connections and Hubs.

### Multi region, high availability Azure web apps[^](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/app-service-web-app/multi-region)

Azure App Service application in multiple regions to achieve high availability![](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/app-service-web-app/images/multi-region-web-app-diagram.png)
* A multi-region architecture can provide higher availability than deploying to a single region. 
* If a regional outage affects the primary region, you can use Traffic Manager to fail over to the secondary region.
*  General approaches to achieving high availability across regions:
    * Active/passive with hot standby - Hot standby means the VMs in the secondary region are allocated and running at all times.
    * Active/passive with cold standby - Cold standby means the VMs in the secondary region are not allocated until needed for failover. This approach costs less to run, but will generally take longer to come online during a failure.
    * Active/active -  Both regions are active, and requests are load balanced between them

### App Service plans[^](https://docs.microsoft.com/en-in/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)
* An App Service plan defines a set of compute resources for a web app to run. These compute resources are analogous to the server farm in conventional web hosting. 
*  categories of pricing tiers:
    * Shared compute: Free and Shared -  runs an app on the same Azure VM as other App Service apps, including apps of other customers. The resources cannot scale out. Use only for development and testing purposes.
    * Dedicated compute: The Basic, Standard, Premium, and PremiumV2 tiers -run apps on dedicated Azure VMs. Only apps in the same App Service plan share the same compute resources.
    * Isolated: runs dedicated Azure VMs on dedicated Azure VNets, which provides network isolation on top of compute isolation. It provides the maximum scale-out capabilities.
    * Consumption: only available to function apps. It scales the functions dynamically depending on workload.
* The new PremiumV2 pricing tier provides Dv2-series VMs with faster processors, SSD storage, and double memory-to-core ratio compared to Standard tier. 
### Business Continuity[^](https://docs.microsoft.com/en-us/azure/architecture/resiliency/disaster-recovery-azure-applications)[^](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions)[^](https://docs.microsoft.com/en-in/azure/architecture/resiliency/index)
* Business continuity (BC), is the ability to perform essential business functions during and after adverse conditions, such as a natural disaster or a downed service. BC covers the entire operation of the business, including physical facilities, people, communications, transportation, and IT.
* Disaster recovery (DR) is focused on recovering from a catastrophic loss of application functionality.
* Paired regions - Azure region is paired with another region within the same geography, together making a regional pair
* Paired region benifits - Physical Isolation, Platform provided replication, region recovery order, sequential updates, Data residency.
* Azure Site Recovery provides a simple way to replicate Azure VMs between regions
* Azure Traffic Manager[^](https://docs.microsoft.com/en-in/azure/traffic-manager/traffic-manager-overview) - uses the Domain Name System (DNS) to direct client requests to the most appropriate endpoint based on a traffic-routing method and the health of the endpoints.
* Azure disaster scenarios - Application failure, Data corruption, Network outage, Failure of a dependent service, Region-wide service disruption, Azure-wide service disruption, Reduced application functionality
### Azure App Service Environment (ASE)[^](https://docs.microsoft.com/en-us/azure/app-service/environment/intro)
* The Azure App Service Environment is an Azure App Service feature that provides a fully isolated and dedicated environment for securely running App Service apps at high scale.
* App Service environments (ASEs) are appropriate for application workloads that require:
    * Very high scale.
    * Isolation and secure network access.
    * High memory utilization.

### API apps[^](https://azure.microsoft.com/en-in/services/app-service/api/)[^](https://docs.microsoft.com/en-in/azure/app-service/app-service-web-tutorial-rest-api)
### API management service[^](https://docs.microsoft.com/en-us/azure/api-management/api-management-key-concepts)
* API Management (APIM) helps organizations publish APIs to external, partner, and internal developers to unlock the potential of their data and services.
* The **API gateway** is the endpoint that accepts API calls and routes them to your backends.
* The **Azure portal** is the administrative interface where you set up your API program
* The **Developer portal** serves as the main web presence for developers

### Web Apps on Linux[^](https://docs.microsoft.com/en-in/azure/app-service/containers/app-service-linux-intro)
* Customers can use App Service on Linux to host web apps natively on Linux for supported application stacks.
* App Service on Linux supports a number of Built-in images
* App Service on Linux is only supported with Basic and Standard app service plans and does not have a Free or Shared tier
* You cannot create Web App for Containers in an App Service plan already hosting non-Linux Web Apps.
* App Service on Linux offers two different paths to getting your application published to the web:
    * Custom image deployment: "Dockerize" your app into a Docker image that contains all of your files and dependencies in a ready-to-run package.
    * App deployment with a built-in platform image: Our built-in platform images contain common web app runtimes and dependencies, such as Node and PHP. 

### CDN[^](https://docs.microsoft.com/en-us/azure/cdn/cdn-overview)
* A content delivery network (CDN) is a distributed network of servers that can efficiently deliver web content to users.
* CDNs store cached content on edge servers in point-of-presence (POP) locations that are close to end users, to minimize latency. 
* Benefits of using Azure CDN to deliver web site assets include:
    * Better performance and improved user experience for end users, especially when using applications in which multiple round-trips are required to load content.
    * Large scaling to better handle instantaneous high loads, such as the start of a product launch event.
    * Distribution of user requests and serving of content directly from edge servers so that less traffic is sent to the origin server.
* The file remains cached on the edge server in the POP until the time-to-live (TTL) specified by its HTTP headers expires
* The default TTL is seven days.
* Azure CDN offers the following key features:
    * Dynamic site acceleration
    * CDN caching rules
    * HTTPS custom domain support
    * Azure diagnostics logs
    * File compression
    * Geo-filtering

### Using Cache [^](https://docs.microsoft.com/en-us/azure/cdn/cdn-how-caching-works), Azure Redis Cache[^](https://azure.microsoft.com/en-in/services/cache/)

[Azure App Service, Virtual Machines, Service Fabric, and Cloud Services comparison](https://docs.microsoft.com/en-us/azure/app-service/choose-web-site-cloud-service-vm?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)

## High Performance Computing, Compute-intensive Applications[^](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/high-performance-computing)
Azure provides flexible solutions to distribute work and scale to thousands of VMs or cores and then scale down when you need fewer resources. 

### Design High-performance computing (HPC) and other Compute-intensive applications using Azure Services

### Azure Batch
* Azure Batch creates and manages a pool of compute nodes (virtual machines), installs the applications you want to run, and schedules jobs to run on the nodes.
* There is no cluster or job scheduler software to install, manage, or scale. Instead, you use Batch APIs and tools, command-line scripts, or the Azure portal to configure, manage, and monitor your jobs.
* Intrinsically parallel workloads are those where the applications can run independently, and each instance completes part of the work
* Examples of intrinsically parallel workloads you can bring to Batch:
    * Financial risk modeling using Monte Carlo simulations
    * VFX and 3D image rendering
    * Image analysis and processing
    * Media transcoding
    * Genetic sequence analysis
    * Optical character recognition (OCR)
    * Data ingestion, processing, and ETL operations
    * Software test execution
* Tightly coupled workloads need to communicate with each other, as opposed to run independently. Tightly coupled applications normally use the Message Passing Interface (MPI) API.
* Examples of tightly coupled workloads:
    * Finite element analysis
    * Fluid dynamics
    * Multi-node AI training
* Batch supports large-scale rendering workloads with rendering tools including Autodesk Maya, 3ds Max, Arnold, and V-Ray. 
* R users can install the doAzureParallel R package to easily scale out the execution of R algorithms on Batch pools.
* Run Batch jobs as part of a larger Azure workflow to transform data, managed by tools such as Azure Data Factory.
* Use Low priority VMs with Batch[^](https://docs.microsoft.com/en-us/azure/batch/batch-low-pri-vms) for Development and Testing, Suplimenting on-demand capacity, and for jobs with flexible execution time.
* Long-running MPI jobs that utilize multiple VMs are not well suited to use low-priority VMs

Batch Service Resources:[^](https://docs.microsoft.com/en-in/azure/batch/batch-api-basics#batch-service-resources)

* Account
* Compute node - VM that is dedicated to processing a portion of your application's workload
* Pool - collection of nodes that your application runs on
* Job - A job is a collection of tasks
    * Job schedules - create recurring jobs
* Task - unit of computation that is associated with a job and runs on a node
    * Start task
    * Job manager task - control and/or monitor job execution
    * Job preparation and release tasks
    * Multi-instance task (MPI)
    * Task dependencies
* Application packages - provide simplified deployment and versioning of the applications that your tasks run

### Design stateless components to accommodate scale [^](https://docs.microsoft.com/en-us/azure/architecture/checklist/scalability)

* Scalability is the ability of a system to handle increased load.
* Application design:
    * Partition the workload
    * Design for scaling - the application and the services it uses must be stateless, to allow requests to be routed to any instance. 
    * Scale as a unit
    * Avoid client affinity
    * Autoscaling capability
    * Offload intensive CPU/IO tasks as background tasks
    * Distribute the workload for background tasks
    * shared-nothing architecture
* Data management
    * Use data partitioning
    * Design for eventual consistency
    * Reduce chatty interactions between components and services
    * Use queues to level the load for high velocity data writes
    * Minimize the load on the data store
    * Minimize the volume of data retrieved
    * Aggressively use caching
    * Handle data growth and retention
    * Optimize Data Transfer Objects (DTOs) using an efficient binary format
    * Set cache control
    * Enable client side caching.
    * Use Azure blob storage and the Azure Content Delivery Network to reduce the load on the application
    * Optimize and tune SQL queries and indexes
    * Consider de-normalizing data.
* Implementation[^](https://docs.microsoft.com/en-us/azure/architecture/checklist/scalability#implementation)


### Design lifecycle strategy for Azure Batch[^](https://docs.microsoft.com/en-in/azure/batch/batch-api-basics)

## Auzre SQL Database Service

* SQL Database is a genreal purpose relational database managed service in Azure.
* Supports structres such as relational data, JSON, spatial, and XML
* Dynamimcally Scalable performance
* Columnstore indexes for extream analytic analysis and reporting (Data Warehousing) and real time operational analytics

SQL Database types:
* managed Single SQL databases
* managed SQL databases in an elastic pool
* managed SQL Instances (SQL Database Managed Instance - preview) with near 100% compatibility with SQL server on-prem

Availability Capabilities
* 99.99% availability SLA
* Automatic Backups
* Point-in-time restores
* Active geo-replication
* Failover groups
* Zone redundant databases (preview)

Built-in Intelligence
* Automatic Performance monitoring and tuning - performance tuning recomendations and Intelligent Insights
* Automatic tuning - Azure SQL Database identifies CREATE INDEX, DROP INDEX, and FORCE LAST GOOD PLAN recommendations that can optimize the database and shows them in Azure portal

### Scaling out with Azure SQL Database[^](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-elastic-scale-introduction)
* Elastic Database tools
    * Elastic Database client library [^](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-elastic-database-client-library) -  is used to manage a shard set.
    * Elastic Database split-merge tool [^](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-elastic-scale-overview-split-and-merge) - is used to move data from one shard to another
    * Elastic Database jobs (preview) [^](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-elastic-jobs-overview) - runs scheduled or ad hoc T-SQL scripts against all databases
    * Elastic Database query (preview) [^](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-elastic-query-overview) - allows you to write a query that spans all databases in the shard set
    * Elastic transactions [^](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-elastic-transactions-overview) - allow you to run transactions that span several databases.

* Horizontal scaling / "scaling out" - using sharding,  managed using Elastic Database client library
* Verticle scaling / "scaling up"  - using Azure powershell to change serivce tier or placing in elastic pool

* **Sharding** is a technique to distribute large amounts of identically structured data across a number of independent databases.

* Multi-tenant and Single-tenant sharding patterns.

### SQL Database Security

Protect Data:

* Data in motion - Transport layer security
* Data at rest - Tranparent data encryption
* Data in use - Always encripted
* column-level encryption, or cell-level encryption. 

Data Discovery & Classification (preview) [^](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-data-discovery-and-classification) - provides capabilities for discovering, classifying, labeling and protecting the sensitive data.

Control Access
* Firewall and Firewall rules
* Authentication ( SQL Authentication and Azure active directory authentication)
* Authorization - database role memberships and object-level permissions
* Row-level security (RLS) [^](https://docs.microsoft.com/en-us/sql/relational-databases/security/row-level-security) based on group membership or execution context
* Dynamic data masking limits sensitive data exposure by masking it to non-privileged users

Proactive monitoring
* Auditing - records database events to Azure storage account
* Threat Detection - detects unusual and potentially harmful attempts to access or exploit databases

SQL Vulnerability Assessment[^](https://docs.microsoft.com/en-us/azure/sql-database/sql-vulnerability-assessment)
* SQL Vulnerability Assessment (VA) is a service that provides visibility into your security state, and includes actionable steps to resolve security issues, and enhance your database security
* Compliance reports generated by VA scans

Business Continuity Features[^](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-business-continuity)
* Recovery Time Objective (RTO) - the maximum acceptable time before the application fully recovers after the disruptive event.
* Recovery point objective (RPO) - the maximum amount of recent data updates (time interval) the application can tolerate losing when recovering after the disruptive event

Comparision of ERT and RTO for the common scenarios

|||||
|:--|:--|:--|:--|
| Capability | Basic tier | Standard tier | Premium tier |
|Point in Time Restore from backup|Any restore point within 7 days|Any restore point within 35 days|Any restore point within 35 days|
|Geo-restore from geo-replicated backups|ERT < 12h, RPO < 1h|ERT < 12h, RPO < 1h|ERT < 12h, RPO < 1h|
|Restore from Azure Backup Vault|ERT < 12h, RPO < 1 wk|ERT < 12h, RPO < 1 wk|ERT < 12h, RPO < 1 wk|
|Active geo-replication|ERT < 30s, RPO < 5s|ERT < 30s, RPO < 5s|ERT < 30s, RPO < 5s|
|||||


Database Backups
* SQL Database automatically performs a combination of full database backups weekly, differential database backups hourly, and transaction log backups every five - ten minutes to protect your business from data loss
*  Backups are stored in geo-redundant storage for 35 days for databases in the Standard and Premium service tiers and 7 days for databases in the Basic service tier
* The full and differential database backups are also replicated to a paired data center for protection against a data center outage.
* Long term retention (LTR policy) upto 10 years using Azure Recovery Services vault [^](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-long-term-retention)

Frequency of data backups
* Full database backups happen weekly
* Differential database backups generally happen every few hours
* Transaction log backups generally happen every 5 - 10 minutes

Backup retention period
* Basic service tier is 7 days.
* Standard service tier is 35 days.
* Premium service tier is 35 days.

Data Encryption at rest
* Transparent data encryption with Bring your own key support (TDE with BYOK) [^](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql)

### Stretch Database[^](https://docs.microsoft.com/en-us/sql/sql-server/stretch-database/stretch-database)

Stretch Database migrates your cold data transparently and securely to the Microsoft Azure cloud

Benifits of Stretch Database:
* Provides cost effective cold data storage - Stretch warm and cold transactional data dynamically from SQL Server to Microsoft Azure with SQL Server Stretch Database
* Doesn’t require changes to queries or applications - access data seamlessly whether on-prem SQL Server or stretched to the cloud 
* Streamlines on-premises data maintenance - Reduce on-premises maintenance and storage for your data
* Keeps your data secure even during migration - RLS, SQL Always Encryption for data in motion and all security features apply to Stretch Database

Candidates for Stretch Database
* Stretch Database targets transactional databases with large amounts of cold data, typically stored in a small number of tables
* Use Microsoft Data Migration Assistant tool [^](https://docs.microsoft.com/en-us/sql/sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor)
* Data Migration Assistant replaces and extends Stretch Database Advisor

### SQL Data Warehouse[^](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/)

* SQL Data Warehouse is a relational database with Massively Parallel Processing (MPP) for big data analytics.
* Polybase T-SQL queries imports big data into SQL Warehouse
* MPP provides high-performance analytics.
* SQL Data Warehouse stores data into relational tables with columnar storage

Azure SQL Data Warehouse performance tiers
* Optimized for Elasticity performance tier - lowest entry price and scale to support majority workloads
* Optimized for Compute performance tier - NVMe SSD cache, for workloads of continuous, blazing fast performance.

SQL Data Warehouse supports these sharding patterns:
* Hash distributed tables - highest query performance for joins and aggregations on large tables
* Round Robin distributed tables - used as staging tables
* Replicated tables - for small tables

### Azure Database for MySQL[^](https://docs.microsoft.com/en-us/azure/mysql/overview)

Azure Database for MySQL is a relational database service in the Microsoft cloud based on the MySQL Community Edition database engine.
* High availability built-in, 99.99% SLA
* Dynamic scalability using tiers (Basic, General Purpose, and Memory Optimized)
* Built-in performance monitoring and alerting to emails and webhooks
* Automatic backups and point-in-time-restore for up to 35 days
* Storage encryption (AES) for data at-rest and SSL connection for Data in-motion
* Connection Security using SSL settings and firewall rules

### Azure Database for PostgreSQL[^](https://docs.microsoft.com/en-us/azure/postgresql/overview)
Azure Database for PostgreSQL is a relational database service based on the open source Postgres database engine. 
All features are same as MySQL

## Azure NoSQL Storage

### Azure Redis Cache[^](https://azure.microsoft.com/en-in/services/cache/)
Azure Redis Cache, a secure data cache and messaging broker that provides high throughput and low-latency access to data for applications. In-Memory data store.

Azure Redis Cache Tiers:
* Basic - Single node, multiple sizes, ideal for development/test and non-critical workloads. No SLA. Size up to 53GB
* Standard - Two-node Primary/Replica, with 99.9% SLA. Size up to 53GB. 
* Premium - Two-node Primary/Replica with up to 10 shards, 99.9% SLA, DR, enhanced security, Redis Persistance, Redis Cluster (Shards data across multiple nodes), VNet deployment for security and isolation. For bigger workloads Sizes 6GB up to 530GB.

### Azure Table Storage[^](https://azure.microsoft.com/en-in/services/storage/tables/)
* A NoSQL key-value storage service, schemaless semi-structured datasets. Ideal for storing structured, non-relational data.

Common uses of Table Storage

* Storing TBs of structured data capable of serving web scale applications
* Storing datasets that don't require complex joins, foreign keys, or stored procedures and can be denormalized for fast access
* Quickly querying data using a clustered index
* Accessing data using the OData protocol and LINQ queries with WCF Data Service .NET Libraries

Table Storage Components
* Storage Account - globally unique, part of URI
* Table - collection of entities
* Entity - similar to rows, a primary key and set of properties
* Property - name, typed-value pair, similar to a column 

An entity can have up to 252 properties and 3 system properties (Partition Key, Row Key, Timestamp)


### Azure Cosmos DB[^](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction)
Azure Cosmos DB is a globally distributed, multi-model database service that supports  document, key-value, wide-column, and graph databases.

Key Capabilities:
* Multi-homing APIs, distribute data across Azure regions, ensures  lowest possible latency.
* Multiple data models and popular APIs for accessing and querying data
    * The atom-record-sequence (ARS) based data model supports multiple data models
    * Data model APIS supported:
        * SQL API : A schema-less JSON database engine with rich SQL querying capabilities
        * MongoDB API : A massively scalable MongoDB-as-a-Service powered by Azure Cosmos DB platform
        * Cassandra API : A globally distributed Cassandra-as-a-Service powered by Azure Cosmos DB platform
        * Graph (Gremlin) API: A fully managed, horizontally scalable graph database service that makes it easy to build and run applications that work with highly connected datasets supporting Open Graph APIs (based on the Apache TinkerPop specification, Apache Gremlin).
        * Table API : A key-value database service built to provide premium capabilities (such as, automatic indexing, guaranteed low latency, global distribution) to existing Azure Table storage applications without making any app changes.
        * Additional data models will be added soon
* Elastically and independently scale throughput and storage on demand and worldwide
    * Easily scale database throughput at a per-second granularity
    * Scale storage size transparently and automatically to handle your size requirements 
* Build highly responsive and mission-critical applications
    *  guarantees end-to-end low latency at the 99th percentile
* Ensure "always on" availability
    * 99.99% availability SLA for all single region database accounts, and all 99.999% read availability on all multi-region database accounts
    * Deploy to any number of Azure regions for higher availability and better performance.
    * Dynamically set priorities to regions and simulate a failure of one or more regions with zero-data loss guarantees to test the end-to-end availability for the entire app 
* Write globally distributed applications, the right way
    * Five well-defined, practical, and intuitive consistency models
* Money back guarantees
    * Industry-leading, financially backed, comprehensive service level agreements for availability, latency, throughput, and consistency for your mission-critical data
* No database schema/index management
    * Rapidly iterate the schema of your application without worrying about database schema and/or index management.
    * Azure Cosmos DB’s database engine is fully schema-agnostic – it automatically indexes all the data it ingests without requiring any schema or indexes and serves blazing fast queries
* Low cost of ownership
    * Five to ten times more cost effective than a non-managed solution or an on-prem NoSQL solution
    * Three times cheaper than AWS DynamoDB or Google Spanne

#### Concepts
* **Serverless** database
    * Serverless computing with Azure CosmosDB and Azure Functions
        * create and deploy event-driven serverless apps with low-latency access to rich data for a global user base.
    * Ways to integrate databases and serverless apps
        * Azure Cosmos DB trigger
        * Input binding
        * Output binding
        * works with SQL API and Graph API accounts only
* Global distribution
* Partitioning [^](https://docs.microsoft.com/en-us/azure/cosmos-db/partition-data)
* Unique keys[^](https://docs.microsoft.com/en-us/azure/cosmos-db/unique-keys)
* Consistency[^](https://docs.microsoft.com/en-us/azure/cosmos-db/consistency-levels)
    * Strong
    * Bounded Staleness
    * Session
    * Consistent Prefix
    * Eventual
* Throughput [^](https://docs.microsoft.com/en-us/azure/cosmos-db/request-units)
* Multi-Model APIs
    * SQL API (DocumentDB API) [^](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-introduction)
    * MangoDB API [^](https://docs.microsoft.com/en-us/azure/cosmos-db/mongodb-introduction)
    * Graph API[^](https://docs.microsoft.com/en-us/azure/cosmos-db/graph-introduction),
    * Azure Tables API[^](https://docs.microsoft.com/en-us/azure/cosmos-db/table-introduction)
    * Cassandra API [^](https://docs.microsoft.com/en-us/azure/cosmos-db/cassandra-introduction)
* Security : Encryption at rest, Firewall support, Securing access to data using Master Key and Resource Tokens
* TCO

### Azure Data Lake Store[^](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-overview)
* Azure Data Lake Store is an enterprise-wide hyper-scale repository for big data analytic workloads.
* Azure Data Lake enables you to capture data of any size, type, and ingestion speed in one single place for operational and exploratory analytics
* Use Data Lake Store to create a hyper-scale, Hadoop-compatible repository for analytics on data of any size, type, and ingestion speed.
* Specifically designed to enable analytics on the stored data and is tuned for performance for data analytics scenarios
* Built for Hadoop - compatible with HDFS, WebHDFS API
* Unlimited storage, petabyte files
* Performance-tuned for big data analytics
* Enterprise-ready: Highly-available and secure
* Supports all Data - can store any data in their native format, as is, without requiring any prior transformations.

Securing Data Lake
* Authentication - Azure Active Directory (AAD), OAuth /OpenID, MFA, Federation
* Access Control - POSIX ACL by WebHDFS, RBAC
* Encryption - supports "on by default," transparent encryption
* Network Isolation - Establish Firewalls
* Data Protection - uses TLS / HTTPS for data in transit

The two modes for managing the master encryption key (MEKs) are as follows:
* Service managed keys
* Customer managed keys - Key rotation

Three types of Keys for design of data encryption:
* Master Encryption Key (MEK)
* Data Encryption Key (DEK)
* Block Encryption Key (BEK)

### Azure Search[^](https://docs.microsoft.com/en-us/azure/search/search-what-is-azure-search)
* Azure Search is a search-as-a-service cloud solution that gives developers APIs and tools for adding a rich search experience over your content in web, mobile, and enterprise applications
* Features:
    * Full text search and text analysis, Simple query and Lucene query syntax
    * functionality accessed using REST API / .Net SDK
    * Data integration - any source, JSON data structure
    * Linguistic analysis
    * Geo-search
    * Core features common to search-centric apps: scoring, search suggestions, faceting, synonyms, geo-search, filters, hit highlighting, sorting, paging
    * Relevance - simple scoring
    * Monitoring and reporting
    * Tools - Import data wizard, Search explorer
    * Highly available, fully managed and scalable
    * Supports OData version 4

### Azure Time Series Insights[^](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-overview)
* Time Series Insights is built for storing, visualizing, and querying large amounts of time series data, such as that generated by IoT devices.
* Four Key Jobs
    * Fully integrated with cloud. It joins metadata with telemetry and indexes your data in a columnar store.
    * manages storage of data, stores your data in memory and SSD’s for up to 400 days
    * provides out-of-the-box visualization via the TSI explorer
    * provides a query service that are easy to integrate for embedding your time series data into custom applications.

## Azure Data Services

### Azure Data Catalog[^](https://docs.microsoft.com/en-us/azure/data-catalog/data-catalog-what-is-data-catalog)
Azure Data Catalog is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources. The enterprise users can discover the data sources they need and understand the data sources they find and it helps organizations to get more value from their existing data.

Capabilities of Data Catalog
* Register Data sources
* Discover Data sources
* Annotate Data sources
* Document Data sources
* Connect to Data sources
* Register Big data sources - Hadoop HDFS
* Data profiling - provides statistics and info about regiestered data assets
* Manage ownership of data assets (standard edition, not in Free edition)
* Save searches and pin data assets
* Setup Business glossary , governed tagging
* Secure Access (standard edition, not in Free edition)
* View related data assets 

### Azure Data Factory[^](https://azure.microsoft.com/en-us/services/data-factory/)

*  Azure Data Factory is a managed cloud service that's built for the complex hybrid extract-transform-load (ETL), extract-load-transform (ELT), and data integration projects
* It's a cloud-based data integration service that allows you to create data-driven workflows in the cloud for orchestrating and automating data movement and data transformation
* Using Azure Data Factory, you can create and schedule data-driven workflows (called pipelines) that can ingest data from disparate data stores.
* The pipelines (data-driven workflows) in Azure Data Factory typically perform the following four steps:
    * Connect and Collect
    * Transform and enrich
    * Publish
    * Monitor
* Azure Data Factory is composed of four key components:
    * Pipeline - A data factory can have one or more pipelines. A pipeline is a group of activities. Together, the activities in a pipeline perform a task
    * Activity - A pipeline can have one or more activities. Activities define the actions to perform on your data. Activities represent a processing step in a pipeline.
        * Data movement activities - Copy Activity in Data Factory copies data from a source data store to a sink data store.
        * Data transformation activities - transformation activities that can be added to pipelines either individually or chained with another activity.
        * Control activities
    * Datasets - Datasets represent data structures within the data stores
    * Linked services - Linked services are much like connection strings, which define the connection information that's needed for Data Factory to connect to external resources. Represents Data Store and Compuete resource.
* Triggers - Triggers represent the unit of processing that determines when a pipeline execution needs to be kicked off
* Pipeline runs - A pipeline run is an instance of the pipeline execution
* Parameters - Parameters are key-value pairs of read-only configuration
* Control flow - Control flow is an orchestration of pipeline activities that includes chaining activities in a sequence, branching, defining parameters at the pipeline level, and passing arguments while invoking the pipeline on-demand or from a trigger


### Azure Data Lake Analytics[^](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-overview)
* Azure Data Lake Analytics (ADLA) is an on-demand analytics job service that simplifies big data
* U-SQL lets you analyze data across Data Lake Store, SQL Server in Azure, Azure SQL Database, and Azure SQL Data Warehouse
* U-SQL is a query language that extends the familiar, simple, declarative nature of SQL with the expressive power of C#
* Use Data Lake Analytics to run big data analysis jobs that scale to massive data sets
* Diagnostic logging allows you to collect data access audit trails.
* ADLA Quota Limits
    * Max ADLA accounts per subscription: 5
    * Max Analytics Units (AUs) per account: 250
    * Max concurrent U-SQL jobs per account: 20

### Azure Analysis Services[^](https://docs.microsoft.com/en-us/azure/analysis-services/analysis-services-overview)
* Azure Analysis Services provides enterprise-grade data modeling in the cloud
* full-managed PaaS service
* you can mashup and combine data from multiple sources, define metrics, and secure your data in a single, trusted semantic data model
* The data model provides an easier and faster way for your users to browse massive amounts of data with client applications like Power BI, Excel, Reporting Services, third-party, and custom apps.
* built on SQL Server Analysis Services
* supports tabular models at the 1200 and 1400 compatibility levels.
* The on-premises data gateway acts as a bridge, providing secure data transfer between on-premises data sources and your Azure Analysis Services servers in the cloud.
* Azure Analysis Services uses Azure Active Directory (Azure AD) for identity management and user authentication
* Tabular Model Scripting Language (TMSL) is the command and object model definition syntax for Analysis Services tabular model databases at compatibility level 1200 or higher

### Azure HDInsight[^](https://azure.microsoft.com/en-in/services/hdinsight/)
Azure HDInsight is a cloud distribution of the Hadoop components from HDP. Azure HDInsight is a fully-managed cloud service that makes it easy, fast, and cost-effective to process massive amounts of data. Use popular open-source frameworks such as Hadoop, Spark, Hive, LLAP, Kafka, Storm, R & more. Azure HDInsight enables a broad range of scenarios such as ETL, Data Warehousing, Machine Learning, IoT and more.

Azure HDInsight can be used for a variety of scenarios in big data processing. It can be historical data (data that's already collected and stored) or real-time data (data that's directly streamed from the source). The scenarios for processing such data can be summarized in the following categories: 

* Batch processing (ETL) : unstructured or structured data is extracted from heterogeneous data sources, transformed into a structured format and loaded into a data store for data science or data warehousing
* Internet of Things (IoT) : process streaming data that's received in real time from a variety of devices
* Data science : build applications that extract critical insights from data, use ML on top to predict future trends
* Data warehousing : perform interactive queries at petabyte scales over structured or unstructured data in any format
* Hybrid: extend your existing on-premises big data infrastructure to Azure to leverage the advanced analytics capabilities of the cloud

* HDInsight supports Java and Python as default programming languages. JVM languages (Clojure, Jython, Scala), Hadoop-specific
languages (Pig Latin and HiveQL)

### Azure Databricks[^](https://docs.microsoft.com/en-us/azure/azure-databricks/what-is-azure-databricks)
* Azure Databricks is an Apache Spark-based analytics platform
* Spark in Azure Databricks includes the following components:
    * Spark SQL and DataFrames
    * Streaming
    * MLib
    * GraphX
    * Spark Core API:  R, SQL, Python, Scala, and Java
	
	
### Azure Storage Types:

* Blob Storage
  * Types - Block Blobs, Page Blobs, Append Blobs
  * Supports streaming and random access scenarios
  * REST APIs
  * LRS, ZRS, GRS, RA-GRS
  * Capacity - up to 500 TB containers
  * Object Size - Up to 200 GB/block blob
  
* Azure Files
  * Network file share using SMB
  * REST APIs, SMB 2.1 (Within region) and SMB 3.0 (Worldwide)
  * AD ACLs are not supported
  * Lift and Shift applications to cloud
  * LRS, ZRS, GRS
  * Capacity - up to 1TB/file 
  * Object Size - Up to 1TB/file
  * Shared accross multiple VMs
  * 1000 IOps
  * Max Size - 5 TB File Share and 1 TB file within share

* Disk Storage
  * provides managed and unmanage disk capabilities
  * persistant data accessible from within the VM (VHDs)
  * Scope - single VM
  * Provides Snapshots and Copy
  * Built-in authentication
  * Files within VHD cannot be accessed
  * 500 IOps
  * Max Size - 4 TB disk
  
* Queue Storage
  * Messages can be up to 64 KB in size
  
* Table Storage
  * stores structured NoSQL data in the cloud, providing a key/attribute store with a schemaless design

### Types of Storage accounts

* General Purpose Storage accounts - 
  * Standard - uses Magnetic media to store data
  * Premium - uses SSD to store data
* Blob Storage accounts
  * Stores Block blobs and Append blobs. Page blobs cannot be stored
  * Access tiers - Hot or Cool
  * Hot for frequent data access Higher cost for storage but cost of access is low
  * Cool access tier - Cost of storage is less and higher cost for accessing blobs

### Securing access to Storage
* controlling access to storage account key using RBAC in Azure AD
* using shared access signatures.
* pulic access to blobs for anonymous read
  
#### SAS (Shared Access Signature)[^](https://docs.microsoft.com/en-us/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
* Provides access to Storage account resources to clients without sharing storage account keys
* Types of Access Control
  * Time Interval
  * Permissions (read, write , delete)
  * IP address
  * Protocol
* When to used SAS?
  * When Clients upload data via a front-end proxy
  * Access Storage directly with permissions defined by SAS
  * When copying Blobs to Blobs, Files to Files and Blobs to Files
* Types of SAS
  * Service SAS
  * Account SAS
* SAS is a signed URI that has Resource URI and SAS token

### Encryption

* Encryption at rest - Azure Storage Service Encryption (SSE) at rest
  * automatically encrypts data prior to persisting to storage and decrypts prior to retrieval.
  * Standard and Premium tiers
* Client-side encryption
  * Storage client libraries
  
### Replication
* Locally-redundant storage (LRS)
  *  99.999999999% (11 9's) durability
* Zone-redundant storage (ZRS) (Preview)
  * at least 99.9999999999% (12 9's) durability
  * synchronous data replication across multiple availability zones
  * ZRS Classic available for block blobs in gen purpose v1 account for asynchronous replication across one or two regions.
  
* Geo-redundant storage (GRS)
  * provide 99.99999999999999% (16 9's) durability
  * local copies of data in primary region and copies in secondary region
  
* Read-access geo-redundant storage (RA-GRS)
  * provides read access to data in secondary region

### Transfering Data to and from storage
* AZCopy tool for Windows and Linux
* Azure Import/Export services - for large amounts of blob data using hard drives

### StorSimple
* Microsoft Azure StorSimple device -  an on-premises hybrid storage array that contains SSDs and HDDs, together with redundant controllers and automatic failover capabilities.
* StorSimple Cloud Appliance (Virtual Appliance) - is a software version of the StorSimple device that replicates the architecture and most capabilities of the physical hybrid storage device
* The Microsoft Azure StorSimple Virtual Array is an integrated storage solution that manages storage tasks between an on-premises virtual array running in a hypervisor and Microsoft Azure cloud storage.
* The virtual array is particularly well-suited for the storage of infrequently accessed archival data.
* The virtual array has a maximum capacity of 6.4 TB on the device (with an underlying storage requirement of 8 TB) and 64 TB including cloud storage.

### Storage Analytics
* Azure Storage Analytics performs logging and provides metrics data for a storage account
* Storage Analytics metrics are available for the Blob, Queue, Table, and File services
* Storage Analytics logging is available for the Blob, Queue, and Table services
* Storage account with ZRS do not have metrics and logging capabilities
* Storage Analytics has a 20TB limit independent of storage account limit

### CORS (Cross-Origin Resource Sharing) Support for Azure Services
* CORS is an HTTP feature that enables a web application running under one domain to access resources in another domain.
* Web browsers implement a security restriction known as same-origin policy that prevents a web page from calling APIs in a different domain
* CORS provides a secure way to allow one domain (the origin domain) to call APIs in another domain.
* CORS is not supported for Premium Storage accounts
* CORS to be enabled for each service (Blob, File, Queue and Table) separately
* By default, CORS is disabled for each service

Useful Link:
	* Storage Concpets in Windows Server[^](https://docs.microsoft.com/en-us/windows-server/storage/storage)


## Key to achieve Goals
What separates the people who achieve their goals and realize their dreams from those who don't?

**Self discipline** is the center of all material success. You cannot win the war against the world if you cannot win the war against your own mind.

* **Eliminate choices whenever possible**
    We all have a limited amount of mental energy for exercising self-control. The more choices you have to make throughout the day, the harder it is to stay on course -- and the easiest it is to give in to temptation.

* **Do the tough stuff first**
    We all have greater mental energy -- and therefore greater self-discipline -- early in the day. The best time to stay on course is to tackle the tough stuff early in the day.  

* **Focus not just on doing, but on becoming**
    Becoming what happens when you put in the time and effort to gain a certain level of skill
	
	
## Networking[^](https://docs.microsoft.com/en-us/azure/networking/networking-overview)


### Virtual Networks[^](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-vnet-plan-design-arm)
* Azure Virtual Network enables many types of Azure resources, such as Azure Virtual Machines (VM), to securely communicate with each other, the internet, and on-premises networks.
* All resources in a virtual network can communicate outbound to the internet, by default.
* Communicate between Azure resources:
    * Through a virtual network
    * Through a virtual network service endpoint: Virtual Network (VNet) service endpoints extend your virtual network private address space and the identity of your VNet to the Azure services(Storage, SQL DB, SQL DW), over a direct connection.[^](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-service-endpoints-overview)
* Communicate with on-premises resources:
    * Point-to-site virtual private network (VPN) - communication between a single computer in your network and a virtual network
    * Site-to-site VPN - communication between your on-premises VPN device and an Azure VPN gateway
    * Azure ExpressRoute - private connection between your network and Azure, through an ExpressRoute partner
* Filter network traffic between subnets using
    * Network security groups
    * Network virtual appliances - a VM that performs a network function, such as a firewall, WAN optimization.
* Routes traffic between subnets, connected VNets, on-prem networks and Internet[^](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-udr-overview)
    * Route tables: control where traffic is routed to for each subnet
    * Border gateway protocol (BGP) routes: you can propagate your on-premises BGP routes to your virtual networks
* Connect virtual networks using Virtual network peering (local or global)[^](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview)


### Load Balancer[^](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-overview)
* With Azure Load Balancer you can scale your applications and create high availability for your services.
* You can use Azure Load Balancer to:
    * Load-balance incoming internet traffic to your VMs (Public load balancer
)
    * Load-balance traffic across VMs inside a virtual network (Internal load balancer)
    * Port forward traffic to a specific port on specific VMs with inbound network address translation (NAT) rules.
    * Provide outbound connectivity for VMs inside your virtual network by using a public load balancer.
* Use Application Gateway for Transport Layer Security (TLS) protocol termination ("SSL offload") or per-HTTP/HTTPS request, application-layer processing.
* Use Traffic Manager for global DNS load balancing.
* Load Balancer features:
    * Load balancing - can create a load-balancing rule to distribute traffic that arrives at front-end to back-end pool instances.Load Balancer uses a hash-based algorithm (5 tuple hash by default)
    * Port forwarding - create an inbound NAT rule to port forward traffic from a specific port of a specific front-end IP address to a specific port of a specific back-end instance inside the virtual network. Remote Desktop Protocol (RDP) or Secure Shell (SSH) sessions.
    * Application agnostic and transparent - does not directly interact with TCP or UDP or the application layer
    * Automatic reconfiguration - reconfigures itself when you scale instances up or down
    * Health probes - stops sending new connections to the unhealthy instances. (HTTP custom probe, TCP custom probe, Guest agent probe)
    * Outbound connections (source NAT / SNAT)
* Supports both Basic and Standard SKUs.

### Application Gateway[^](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-introduction)
* Azure Application Gateway is a web traffic load balancer that enables you to manage traffic to your web applications. 
* application layer (OSI layer 7) load balancing - can route traffic based on the incoming URL (/images, /video).
* URL-based routing - route traffic to back-end server pools based on URL Paths of the request
* Redirection - automatic HTTP to HTTPS redirection to ensure all communication between an application and its users occurs over an encrypted path. (Global redirection from one port to another port, Path-based redirection, Redirect to an external site)
* Multiple-site hosting -  configure more than one web site on the same application gateway instance
* Session affinity - cookie-based session affinity keeps a user session on the same server
* Secure Sockets Layer (SSL) termination - saves web servers from costly encryption and decryption overhead. Also supports end to end SSL encryption.
* Web application firewall (WAF) - provides centralized protection of your web applications from common exploits and vulnerabilities ( SQL injection attacks, cross site scripting attacks)
* Websocket and HTTP/2 traffic - enable full duplex communication between a server and a client over a long running TCP connection. 

### Traffic Manager[^](https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-overview)
* Azure Traffic Manager allows you to control the distribution of user traffic for service endpoints in different datacenters.
* Traffic Manager uses the Domain Name System (DNS) to direct client requests to the most appropriate endpoint based on a traffic-routing method and the health of the endpoints.
* Traffic Manager benefits:
    * Improve availability of critical applications - automatic failover 
    * Improve responsiveness for high-performance applications - by directing traffic to the endpoint with the lowest network latency
    * Perform service maintenance without downtime - directs traffic to alternative endpoints while the maintenance is in progress
    * Combine on-premises and Cloud-based applications - supports external, non-Azure endpoints
    * Distribute traffic for large, complex deployments - use nested Traffic Manager profiles
* Traffic Manager routing methods - Priority, Weighted, Performance, Geographic

### Azure DNS[^](https://docs.microsoft.com/en-us/azure/dns/dns-overview)
* The Domain Name System, or DNS, is responsible for translating (or resolving) a website or service name to its IP address. 
* Reliability and performance - uses Anycast networking so that each DNS query is answered by the closest available DNS server.
* Seamless integration - to manage DNS records for Azure services and external resources as well
* Securtiy - ARM provides role-based access control, audit logs, and resource locking
* Azure DNS does not currently support purchasing of domain names

### NSG[^](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg)
* A network security group (NSG) contains a list of security rules that allow or deny network traffic to resources connected to Azure Virtual Networks (VNet)
* NSGs can be associated to subnets, individual VMs (classic), or individual network interfaces (NIC) attached to VMs (Resource Manager)
* Endpoint-based access control lists (ACL) and NSGs are not supported on the same VM instance

### User Defined Routes (UDRs)[^](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-udr-overview)[^](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-scenario-udr-gw-nva)
* You can create custom, or user-defined, routes in Azure to override Azure's default system routes, or to add additional routes to a subnet's route table.
* Each subnet in Azure can be linked to a UDR table used to define how traffic initiated in that subnet is routed. If no UDRs are defined, Azure uses default routes to allow traffic to flow from one subnet to another
* The following next hop types can be specified when creating a user-defined route:
    * Virtual appliance - next hop IP address (private IP of NIC, private IP of internal load balancer)
    * Virtual network gateway - traffic destined for specific address prefixes routed to a virtual network gateway
    * None - Specify when you want to drop traffic to an address prefix
    * Virtual network
    * Internet
* A route with the 0.0.0.0/0 address prefix instructs Azure how to route traffic destined for an IP address that is not within the address prefix of any other route in a subnet's route table.
* Azure creates a default route to the 0.0.0.0/0 address prefix, with the Internet next hop type.
* You cannot specify VNet peering or VirtualNetworkServiceEndpoint as the next hop type in user-defined routes. 

### VNet Peering[^](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview)
* Virtual network peering enables you to seamlessly connect two Azure virtual networks.
* VNet peering - connecting VNets within the same Azure region
* Global VNet peering - connecting VNets across Azure regions
* Service chaining enables you to direct traffic from one virtual network to a virtual appliance, or virtual network gateway, in a peered virtual network, through user-defined routes.
*  A virtual network can have only one gateway.
* You can deploy hub-and-spoke networks, where the hub virtual network can host infrastructure components such as a network virtual appliance or VPN gateway.[^](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)
* Each virtual network can have its own gateway and use it to connect to an on-premises network

### VNet Service Endpoints[^](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-service-endpoints-overview)
* Virtual Network (VNet) service endpoints extend your virtual network private address space and the identity of your VNet to the Azure services, over a direct connection.
* Traffic from your VNet to the Azure service always remains on the Microsoft Azure backbone network.
* Azure services available:
    * Azure Storage
    * Azure SQL Database
    * Azure SQL Data Warehouse
	
	
## Designing Security and Identity

Key Security areas
* Identity and Access (Identity for users and resources)
* Network security (secure access, isolation and publishing)
* Data Protection (Encryption of data at rest and in transit) 
* Protecting Secrets (Keys, Certificates, credentials)
* System Integrity (Patched, Protected)
* Insight (auditing, system state, Health)

Azure AD(AD) and On-Prem Windows Server Active Directory (WSAD)
Azure AD does not support Windows Authentication, Group Policies as supported by Windows Server AD.

Authentication Options for AAD
* Identity sync with Password hash.
* Identity sync with ADFS
* Identity sync with Pass-through authentication agent

### Azure AD Domain Services:
Azure AD Domain Services provides managed domain services such as domain join, group policy, LDAP, Kerberos/NTLM authentication that are fully compatible with Windows Server Active Directory. 

Azure ADDS Managed domain features:
* The managed domain is stand-alone domain and not an extension of on-premisis domain
* The domain controllers in managed domain does not need to be managed, patched, or monitored
* No need to manage AD replication to managed domain. All User accounts, group memberships and credentials are automatically available with managed domain. The synchronization from on-prem to Azure AD is done using AD Connect.
* The Domain administrator or Enterprise administrator privileges are not avaialble in managed domain.

MFA is available in AD premium, but can also be obtained by pay per use basis

### Azure Key Vault
* On-Prem solution is HSM (Hardware Security Module)
* HSM protected and Software protected
* Azure Key Vault can store Secrets, Keys and Certificates(x509 compliant)
* Authentication is via Azure AD OAuth2 token
* Authorization is via Access Control List (ACL) on the key vault
* Roles include - Key Vault Owner, Key/Secret Owner, App Operator, Auditor

### Network Security
* OS - Patched, Anti-virus, Managed Policy, Managed Firewall
* Virtual Subnet - RBAC, NSG, Virtual Appliance
* Virtual Network - RBAC, NSG, App Gateway, S2S VPN / Express Routes, Forced Tunneling
* Virtual Network is tied to a Region and in turn to a Subscription
* Virtual Appliances- A VM with preconfigured software such as Firewalls, load balancers
* Azure Application Gateway - A layer 7 reverse proxy, SSL termination
* Web Application Firewall - Optional to App Gateway, Implements CRS3.O

### Azure Resource Manager
* Resource, Resource Groups, Subscription
* Reosource Group is not an access boundary.

### Role Based Access Control
* RBAC enables assignment of roles at various levels: Subscription, Resource Group, Individual resource.
* Assigned rights are inherited by chlid objects

Three basic roles:
* **Owner** has full access to all resources including the right to delegate access to others.
* **Contributor** can create and manage all types of Azure resources but can’t grant access to others.
* **Reader** can view existing Azure resources.

### Azure AD P2 features

* Previliged Identity Management - Just in time privilege elevation
* Identity Protection

Azure Security Center gives overview of overall security


## Application Monitoring and Alerting strategy
### Monitoring Azure applications and resources

Components that work together to provide monitoring of Azure resources:
* Deep Application Monitoring - Application Insights
* Deep Infrastructure Monitoring 
    * Log Analytics
    * Management Solutions
    * Network Monitoring
    * Service Map
* Core Monitoring
    * Azure Monitor
    * Advisor
    * Service Health
    * Activity Log
* Shared Capabilities
    * Alerts
    * Dashboards
    * Metrics Explorer

### Azure Monitor[^](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor)

* Azure Monitor helps you track performance, maintain security, and identify trends.
* Azure Monitor provides base-level infrastructure metrics and logs for most services in Microsoft Azure
* Monitor Page shows the following:
    * Triggered alerts and alert sources
    * Activity Log Errors
    * Azure Service Health
    * Application Insights
* Route - stream monitoring data to Application Insight or Event Hubs
* Store and Archive
    * Metrics - 90 days
    * Activity Log - 90 days
    * Diagnostics logs are not stored.

### Azure Log Analytics[^](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-overview)
Log Analytics plays a central role in Azure management by collecting telemetry and other data from a variety of sources and providing a query language and analytics engine that gives you insights into the operation of your applications and resources.

* Log Analytics includes a rich query language to quickly retrieve, consolidate, and analyze collected data
* Views in Log Analytics visually present data from log searches.

### Application Insights[^](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-overview)
* Application Insights is an extensible Application Performance Management (APM) service for web developers on multiple platforms.
* It can monitor and analyze telemetry from mobile apps by integrating with Visual Studio App Center and HockeyApp.

## Monitoring Azure Platform and Alerting

Azure Monitor enables core monitoring for Azure services by allowing the collection of metrics, activity logs, and diagnostic logs. 

### Azure Service Health[^](https://docs.microsoft.com/en-us/azure/service-health/)

* Azure Service Health can notify you, help you understand the impact of issues, and keep you updated as the issue resolves.
* Prepare for planned maintenance and changes that could affect the availability of your resources.
* Components of Azure Health:
    * Azure Status - a global view of health of Azure services, real time status updates and history pages
    * Service Health - a personalized view of health of Azure services, provides a customizable dashboard. Tracks 3 types of health events:
        * Serviec issues (any ongoing problems)
        * Planned maintenance
        * Health advisories
    * Resource Health - a deeper view of the health of the individual resources provisioned to you by your Azure services, provides technical support to help you mitigate problems. Resource Health statuses are:
        * Available
        * Unavailable
        * Non-platform events - triggered by users' actions
        * Unknown - no update since 10 mins
        * Degraded - detected a loss in performance

### Azure Advisor[^](https://docs.microsoft.com/en-us/azure/advisor/advisor-overview)
* Advisor is a personalized cloud consultant that helps to follow best practices to optimize Azure deployments.
* constantly monitors your resource configuration and usage telemetry.
* Get proactive, actionable, and personalized best practices recommendations
* Improve the performance, security, and high availability of your resources, as you identify opportunities to reduce your overall Azure spend
* Get recommendations with proposed actions inline. 
* Categories of recomendation:
    * High Availability
    * Security
    * Performance
    * Cost
* Advisor provides recommendations for virtual machines, availability sets, application gateways, App Services, SQL servers, and Redis Cache.

### Activity Log[^](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)
* Provides data about the operation of an Azure resource
* Is a subscription log that provides insight into subscription-level events that have occurred in Azure
* Primarily for ARM activities, does not track resources using the Classic/RDFE model
* The Activity Log was previously known as “Audit Logs” or “Operational Logs
* Categories of data in Activity Log: 
    * Administrative - Resource Manager operations
    * Service Health - events: Action Required, Assisted Recovery, Incident, Maintenance, Information, or Security
    * Alert - record of activation of Azure alerts
    * Autoscale - events related to the operation of the autoscale engine
    * Recommendation - events offer recommendations for how to better utilize your resources
    * Security - alerts generated by Azure Security Center
    * Policy and Resource Health - reserved for future use

### Diagnostics Logs[^](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* Resource-level diagnostic logs provide insight into operations that were performed within that resource itself.
* Resource-level diagnostic logs differ from the Activity Log and guest OS-level diagnostic logs
* Activity Log provides insight into the operations that were performed on resources.
* Guest OS diagnostic logs are those collected by an agent running inside of a virtual machine or other supported resource type.
* Resource-level diagnostic logs require no agent and capture resource-specific data from the Azure platform itself, while guest OS-level diagnostic logs capture data from the operating system and applications running on a virtual machine.

### Monitoring Azure Networks

Log Analytics offers the following solutions for monitoring your networks:
* Network Perfomance Monitor (NPM) - to monitor health of your network
* Azure Application Getway analytics to review
    * Azure Application Gateway logs
    * Azure Application Gateway metrics
* Solutions to monitor and audit network activity on your cloud network
* Traffic Analytics[^](https://docs.microsoft.com/azure/networking/network-monitoring-overview#traffic-analytics)
* Azure Network Security Group Analytics

Network Performance Monitor (NPM) [^](https://docs.microsoft.com/en-in/azure/networking/network-monitoring-overview)
* NPM management solution is a network monitoring solution, that monitors the health, availability and reachability of networks.
* It is used to monitor connectivity between:
    * Public cloud and on-premises
    * Data centers and user locations (branch offices)
    * Subnets hosting various tiers of a multi-tiered application.
* Network Performance Monitor detects network issues like traffic blackholing, routing errors, and issues that conventional network monitoring methods aren't able to detect
* NPM is a cloud-based hybrid network monitoring solution that helps you monitor network performance between various points in your network infrastructure
* Network Performance Monitor offers three broad capabilities:
    * Performance Monitor[^](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-network-performance-monitor-performance-monitor)
    * Service Endpoint Monitor[^](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-network-performance-monitor-service-endpoint)
    * ExpressRoute Monitor[^](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-network-performance-monitor-expressroute)

Azure Application Gateway and Network Security Group analytics  management solutions collect diagnostics logs directly from Azure Application Gateways and Network Security Groups.

Network Watcher Service[^](https://azure.microsoft.com/en-us/services/network-watcher/)
* Network Watcher is a regional service that enables you to monitor and diagnose conditions at a network scenario level in, to, and from Azure.
* Automate remote network monitoring with packet capture
* Gain insight into your network traffic using flow logs
* Diagnose VPN connectivity issues
* Capabilities:
    * Topology
    * Variable Packet Capture
    * IP flow verify
    * Next Hop
    * Security group view
    * NSG flow logging
    * VNet Gateway and Connection troubleshooting
    * Network subscription limits
    * Configuring Diagnostics log
    * Connection troubleshoot
    * Connection Monitor
* Network Watcher uses RBAC model

### Monitor security with Azure Security Center[^](.\SecurityCenter.md)[^](https://docs.microsoft.com/en-us/azure/security-center/security-center-network-recommendations)
* Azure Security Center provides unified security management and advanced threat protection across hybrid cloud workloads.
* Security Center identifies potential security vulnerabilities, it creates recommendations that guide you through the process of configuring the needed controls
* Available network recommendations:
    * Add a Next Generation Firewall (NGFW)
    * Route traffic through NGFW only
    * Enable Network Security Groups on subnets or virtual machines
    * Restrict access through Internet facing endpoint
* Integrated Azure Security solutions
    * Endpoint protection (Trend Micro, Symantec, Windows Defender, and System Center Endpoint Protection (SCEP))
    * Web application firewall (Barracuda, F5, Imperva, Fortinet, and Azure Application Gateway)
    * Next-generation firewall (Check Point, Barracuda, Fortinet, and Cisco)
    * Vulnerability assessment (Qualys) 

## Operations Automation Strategy

### Azure Automation[^](https://docs.microsoft.com/en-us/azure/automation/automation-intro)
* Azure Automation delivers a cloud-based automation and configuration service that provides consistent management across your Azure and non-Azure environments
* It consists of process automation, update management, and configuration features.
* Azure Automation provides complete control during deployment, operations, and decommissioning of workloads and resources
* Azure Automation capabilities:
    * Process Automation: Orchestrate processes using graphical, Powershell, and Python runbooks
    * Configuration management: Collect inventory, track changes, configure desired state
    * Update management: Assess compliance, Schedule update installation across hybrid environments
    * Heterogeneous: Windows & Linux, Azure and on-premises
    * Shared Capabilities: RBAC, Secure, global store for variables, credentials, certificates,connections. Flexible Scheduling, Shared modules, Source Control support, Auditing, Tags
* When to use Azure Automation:
    * if you do not already have a Configuration Management Solution, or not deeply embedded
    * If you want to significantly expand your configuration management without significant expense
    * If you already own OMS
    * If you already have PowerShell expertise

### Chef[^](https://docs.microsoft.com/en-in/azure/virtual-machines/windows/chef-automation?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json)
* Chef has three main architectural components: Chef Server, Chef Client (node), and Chef Workstation.
* Cross-OS systems management, automation and analytics output
* Ruby and Git are required plus agent is on target machine
* Good development focused teams (code driven approach to configuration)
* Leverage Chef in Azure when already using it.
* When to use Chef:
    * If you already have a Chef management infrastructure
    * If your primary expertise is managing Linux machines

### Puppet[^](https://blogs.technet.microsoft.com/lukeb/2016/02/05/azure-puppet-on-premises-deploying-to-azure/)[^](https://azure.microsoft.com/en-us/blog/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)
* Puppet - is an Open Source configuration management system that allows you to define the state of your IT infrastructure, then automatically enforces the correct state.
* Stable and mature so good for managing large, heterogeneous enterprise environments
* Automate systems configuration and enforce consitency
* Large Open Source catalog of modules and runs on nearly every OS (cross platform)
* When to use Puppet:
    * If you already have a Puppet management infrastructure
    * If your primary expertise is managing Linux machines

### PowerShell[^](https://docs.microsoft.com/en-in/powershell/azure/overview?view=azurermps-5.7.0)[^](https://docs.microsoft.com/en-in/powershell/dsc/overview)
* Azure PowerShell provides a set of cmdlets that use the Azure Resource Manager model for managing your Azure resources.
* Use the Cloud Shell to run the Azure PowerShell in your browser or install it on own computer
* PowerShell supports asynchronous action with PowerShell Jobs.
* PowerShell background jobs run a command or expression in the background without interacting with the current session.
* Azure PowerShell also provides an -AsJob switch on some long-running cmdlets
* DSC is a management platform in PowerShell that enables you to manage your IT and development infrastructure with configuration as code
* DSC is a declarative platform used for configuration, deployment, and management of systems.
* Three primary components:
    * Configurations - are declarative PowerShell scripts which define and configure instances of resources
    * Resources - A resource exposes properties that can be configured (schema) and contains the PowerShell script functions that LCM calls
    * Local Configuration Manager (LCM) - is the engine by which DSC facilitates the interaction between resources and configurations.

### Desired State Configuration (DSC)[^](https://docs.microsoft.com/en-us/azure/automation/automation-dsc-overview)
* Azure Automation DSC is an Azure service that allows you to write, manage, and compile PowerShell Desired State Configuration (DSC) configurations, import DSC Resources, and assign configurations to target nodes, all in the cloud.
* Azure Automation DSC provides several advantages over using DSC outside of Azure:
    * Built-in pull server - target nodes automatically receive configurations, conform to the desired state, and report back on their compliance.
    * Management of all your DSC artifacts
    * Import reporting data into Log Analytics
* Install or remove windows roles and features
* Running Windows PowerShell scripts
* Managing registry settings
* Managing files and directories
* Starting, stopping and managing processes and services
* Managing groups and user accounts
* Deploying new software
* Manging environment variables
* Discovering the actual configuration state on a given node
* Fixing a configuration that has drifted away from the desired state
* When to use DSC:
    * If you do not already have a Configuration Mangement Solution
    * If your primary experience is in managing Windows machines
    * Uses vendor-neutral configuration files (MOF)
    * If you already have PowerShell expertise

### Azure Event Grid[^](./PlatformServices.md#event-grid)
* Fully managed event routing
* Near real-time event delivery at scale
* Broad coverage within Azure and beyond
* backbone of event-driven computing
* Manage all events at one place
* Scenarios
    * Serverless apps
    * Ops Automation - notify Azure automation when underlying infrastructure is provisioned
    * Application integration

### Azure Logic Apps[^](https://docs.microsoft.com/en-us/azure/logic-apps/)
* Azure Logic Apps simplifies how you build automated scalable workflows that integrate apps and data across cloud services and on-premises systems.
* Logic Apps helps you build, schedule, and automate processes as workflows so you can integrate apps, data, systems, and services across enterprises or organizations
* Every logic app workflow starts with a trigger, which fires when a specific event happens, or when new available data meets specific criteria.

### Azure Autoscale[^](https://azure.microsoft.com/en-us/features/autoscale/)[^](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-autoscale)
* Autoscale is a built-in feature of Cloud Services, Mobile Services, Virtual Machines, and Websites that helps applications perform their best when demand changes
* Autoscale allows you to have the right amount of resources running to handle the load on your application.
*  It allows you to add resources to handle increases in load and also save money by removing resources that are sitting idle.

### Azure Scheduler[^](https://docs.microsoft.com/en-us/azure/scheduler/scheduler-intro)
* Azure Scheduler allows you to declaratively describe actions to run in the cloud. It then schedules and runs those actions automatically.
* It invokes via HTTP, HTTPS, a storage queue, a service bus queue, or a service bus topic.
* Recurring application actions - Periodically gathering data from Twitter into a feed
* Daily maintenance - Daily pruning of logs, performing backups, and other maintenance tasks
* Scheduler allows you to create, update, delete, view, and manage jobs and job collections programmatically, by using scripts, and in the portal.


## Artificial Intelligence Services

### Azure Machine Learning[^](https://docs.microsoft.com/en-in/azure/machine-learning/preview/overview-what-is-azure-ml)
* Azure Machine Learning services (preview) enable building, deploying, and managing machine learning and AI models using any Python tools and libraries.
* Machine learning is a data science technique that allows computers to use existing data to forecast future behaviors, outcomes, and trends
* Azure Machine Learning Components:
    * Azure Machine Learning Workbench
    * Azure Machine Learning Experimentation Service
    * Azure Machine Learning Model Management Service
    * Microsoft Machine Learning Libraries for Apache Spark (MMLSpark Library)
    * Visual Studio Code Tools for AI
    * Other ML products [^](https://docs.microsoft.com/en-in/azure/machine-learning/preview/overview-more-machine-learning)
        * SQL Server Machine Learning Services
        * Microsoft Machine Learning Server
        * Azure Machine Learning Services
        * Azure Machine Learning Studio
        * Data Science Virtual Machine
        * Spark MLLib in HDInsight
        * Batch AI Training Service
        * Microsoft Cognitive Toolkit
        * Microsoft Cognitive Services
    * Azure Machine Learning has two types of web services:
        * Request-Response Service (RRS): A low latency, highly scalable service that provides an interface to the stateless models created and deployed by using Machine Learning Studio.
        * Batch Execution Service (BES): An asynchronous service that scores a batch for data records. 

### Azure Bot Service[^](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-overview-introduction)

* Bot Service provides the core components for creating bots, including the Bot Builder SDK for developing bots and the Bot Framework for connecting bots to channels.
* Two hosting plans
    * App Service plan - standard Azure web app, predictable costs and scaling
    * Consumption plan - serverless bot that runs on Azure Functions, pay-per-run pricing
* Bot Service Features:
    * Multiple language support - .NET and Node.js
    * Bot templates - Basic bot, a Forms bot, LUIS, QnA bot, Proactive bot 
    * Support dependencies - NuGet and NPM support
    * Flexible development - Azure portal, GitHub, VSTS, Visual Studio 
    * Connect to channels - Skype, Facebook, Teams, Slack, SMS etc.
    * Tools and services - Bot Framework Emulator, Channel Inspector
    * Open source - Bot Builder SDK is open-source
* Bot State service
    * Enables your bot to store and retrieve state data that is associated with a user, a conversation, or a specific user within the context of a specific conversation.
    * REST and JSON over HTTPS
    * authentication with JWT Bearer tokens

### Cognitive Services[^](https://azure.microsoft.com/en-us/services/cognitive-services/)
Infuse your apps, websites and bots with intelligent algorithms to see, hear, speak, understand and interpret your user needs through natural methods of communication

* Vision
    * Computer Vision API
    * Face API
    * Content Moderator
    * Emotion API
    * Custom Vision Service 
    * Video Indexer
* Speech
    * Translator Speech API 
    * Speaker Recognition API 
    * Bing Speech API
    * Custom Speech Service
* Knowledge
    * QnA Maker API
    * Custom Decision Service
* Language
    * Language Understanding (LUIS)
    * Text Analytics API 
    * Bing Spell Check API 
    * Translator Text API
    * Web Language Model API
    * Linguistic Analysis API
* Search
    * Bing Autosuggest API
    * Bing Image Search API
    * Bing News Search API
    * Bing Video Search API
    * Bing Web Search API
    * Bing Custom Search API
    * Bing Entity Search API

## Azure IoT

### Azure Stream Analytics[^](https://docs.microsoft.com/en-us/azure/stream-analytics/)
* Azure Stream Analytics is a managed event-processing engine set up real-time analytic computations on streaming data
* The data can come from devices, sensors, web sites, social media feeds, applications, infrastructure systems, and more
* Use Stream Analytics to examine high volumes of data streaming from devices or processes, extract information from that data stream, identify patterns, trends, and relationships.
* The data can be ingested into Azure from a device using an Azure event hub or IoT hub. The data can also be pulled from a data store like Azure Blob Storage.
* Stream Analytics can handle up to 1 GB of incoming data per second
* Response to analysis:
    * Send a command to change device settings. 
    * Send data to a monitored queue for further action based on findings (such as event hubs, Azure Service Bus, queues)
    * Send data to a Power BI dashboard.
    * Send data to storage like Data Lake Store, Azure SQL Database, or Azure Blob storage.
* Stream Analytics provides a SQL-like language for creating transformations (SQL DSL)
* Azure provides multiple solutions for analyzing streaming data: Azure Streaming Analytics and Apache Storm on Azure HDInsight
* Stream Analytics pricing is based on volume of data processed and the number of streaming units required per hour that the job is running
* Apache Storm on HDInsight pricing is cluster-based, and is charged based on the time the cluster is running, independent of jobs deployed
* Stream Analytics enables developers to use Tumbling, Hopping and Sliding windows to perform temporal operations on streaming data

### IoT Hub[^](https://docs.microsoft.com/en-us/azure/iot-hub/)[^](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-what-is-iot-hub)
Azure IoT Hub is a fully managed service that enables reliable and secure bidirectional communications between millions of IoT devices and a solution back end

Comparision between IoT Hub and Event Hub[^](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-compare-event-hubs)

|||||
|:--|:--|:--|:--|
| IoT Capability | IoT Hub standard tier | IoT Hub basic tier | Event Hubs |
| Device-to-cloud messaging | Yes | Yes | Yes |
| Protocols: HTTPS, AMQP, AMQP over websockets | Yes | Yes | Yes |
| Protocols: MQTT, MQTT over websockets | Yes | Yes | |
| Per-device identity | Yes | Yes | |
| File upload from devices | Yes | Yes | |
| Device Provisioning Service | Yes | Yes | |
| Cloud-to-device messaging | Yes | | |
| Device twin and device management | Yes | | |
| IoT Edge | Yes | | |
|||||

IoT Hub communication options: 
* Device Twins
* Per-device authentication and secure connectivity
* Route device-to-cloud messages to Azure services based on declarative rules
* Integrate IoT Hub events into your business applications: IoT Hub integrates with Azure Event Grid
* Monitoring of device connectivity operations
* An extensive set of device libraries
* IoT protocols and extensibility: enables devices to natively use the MQTT v3.1.1, HTTPS 1.1, or AMQP 1.0 protocols
* Scale
* Device provisioning

Gateways:
* A protocol gateway performs protocol translation, for example MQTT to AMQP
* A field gateway can:
    * Run analytics on the edge.
    * Make time-sensitive decisions to reduce latency.
    * Provide device management services.
    * Enforce security and privacy constraints.
    * Perform protocol translation.
* A field gateway differs from a simple traffic routing device (NAT / firewall)

### Event Hub[^](https://docs.microsoft.com/en-in/azure/event-hubs/event-hubs-what-is-event-hubs)
* Azure Event Hubs is a highly scalable data streaming platform and event ingestion service, capable of receiving and processing millions of events per second.
* Data sent to an event hub can be transformed and stored using any real-time analytics provider or batching/storage adapters
* Event Hub is different from Azure Service Bus messaging, and does not implement some of the capabilities that are available for Service Bus messaging entities, such as topics.

Event Hubs features:
* Event producers/publishers: An event is published via AMQP 1.0 or HTTPS
* Capture
* Partitions: Enables each consumer to only read a specific subset, or partition, of the event stream
* Event consumers
* Consumer groups
* Throughput units: Throughput units are pre-purchased units of capacity
* Billing plan: Basic, Standard, Dedicated

### Time Series Insights[^](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-overview)[^](notes/Database.md#azure-time-series-insights)

### IoT Edge[^]()
* Azure IoT Edge moves cloud analytics and custom business logic to devices
* Azure IoT Edge is only available in the standard tier of IoT Hub
* Azure IoT Edge is made up of three components:
    * IoT Edge modules - Docker compatible containers that run Azure services, 3rd party services, code
    * IoT Edge runtime - runs on IoT edge device and manages modules deployed, Supports both Linux and Windows OS
    * IoT Edge cloud interface - to configure workloads, monitor and manage IoT Edge devices

### Notification Hub[^](https://docs.microsoft.com/en-in/azure/notification-hubs/notification-hubs-push-notification-overview)
* Azure Notification Hubs provide an easy-to-use, multi-platform, scaled-out push engine.
* a single cross-platform API call to send targeted and personalized push notifications to any mobile platform from any cloud or on-premises backend
* Push notifications is a form of app-to-user communication where users of mobile apps are notified of certain desired information, usually in a pop-up or dialog box
* Push notifications are delivered through platform-specific infrastructures called Platform Notification Systems (PNSes)
* Notification Hubs is offered in three tiers—free, basic and standard. Base charge and quotas are applied at the namespace level[^](https://azure.microsoft.com/en-in/pricing/details/notification-hubs/)

Challenges of Push Notifications:
* Platform dependency
* Scale - device tokens must be refreshed upon every app launch, broadcast support
* Routing - backend must maintain a registry to associate devices with interest groups, users, properties, etc.

Notification Hub advantages:
* Cross platforms - iOS, Android, Windows, and Kindle and Baidu
* Cross backends - Cloud or on-premises, .NET, Node.js, Java, etc
* Rich set of delivery patterns:
    * Broadcast to one or multiple platforms
    * Push to device
    * Push to user
    * Push to segment with dynamic tags
    * Localized push
    * Silent push
    * Scheduled push
    * Direct push
    * Personalized push
* Rich telemetry
* Scalability
* Security - SAS or federated authentication

 App Service Mobile Apps has built-in support for push notifications using Notification Hubs.

### Event Grid[^](https://docs.microsoft.com/en-in/azure/event-grid/overview)
* Event Grid is a fully managed event routing service which provides reliable message delivery at massive scale
* Azure Event Grid is a fully-managed intelligent event routing service that allows for uniform event consumption using a publish-subscribe mode
* Azure Event Grid allows you to easily build applications with event-based architectures
* Event Grid has built-in support for events coming from Azure services, like storage blobs and resource groups
* Event sources
    * Azure Subscriptions (management operations)
    * Custom Topics
    * Event Hubs
    * IoT Hub
    * Resource Groups (management operations)
    * Service Bus
    * Storage Blob
    * Storage General-purpose v2 (GPv2)
* Event handlers
    * Azure Automation
    * Azure Functions
    * Event Hubs
    * Logic Apps
    * Microsoft Flow
    * WebHooks
* When using Azure Functions as the handler, use the Event Grid trigger instead of generic HTTP triggers. Event Grid automatically validates Event Grid Function triggers. With generic HTTP triggers, you must implement the validation response.
* Five concepts in Azure Event Grid:
    * Events
    * Event sources/publishers
    * Topics
    * Event subscriptions
    * Event handlers
* Event Grid Usage scenarios
    * Serverless application architectures
    * Ops Automation
    * Application integration

### Azure IoT Central[^](https://docs.microsoft.com/en-us/azure/iot-central/overview-iot-central)
* A software as a service (SaaS) offering, to deploy a fully managed, end-to-end solution that enables powerful IoT scenarios without requiring cloud-solution expertise.

### Azure IoT Suite[^](https://docs.microsoft.com/en-us/azure/iot-suite/)
* A platform as a service (PaaS) offering, fully customizable solutions for common scenarios to accelerate your IoT project.

### Azure Relay[^](https://docs.microsoft.com/en-us/azure/service-bus-relay/relay-what-is-it)
* Hybrid connections and WCF Relay are part of Azure Relay service
* Comminicate with on-premises resources without opening a firewall port
* Hybrid connections support multi-platform scenarios
* Hybrid connections can be used to connect to any TCP port including SQL DB's and Web API's 
* WCF Relay can communicate with WCF services and .Net Framework only
* Connect an Azure Web App to on-presmises via VPN
* VNet integration gives a multi tenant Web App Access to resources accessible by an Azure VNet.
* App Service Environment (ASE) can be deployed into an Azure VNet for bidirectional access.

### Useful links
* Compare Queues: [^](https://docs.microsoft.com/en-in/azure/service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted)

* Performance:[^](https://docs.microsoft.com/en-in/azure/service-bus-messaging/service-bus-performance-improvements)

* High-Availability:[^](https://docs.microsoft.com/en-in/azure/service-bus-messaging/service-bus-async-messaging)

* Premium messaging: [^](https://docs.microsoft.com/en-in/azure/service-bus-messaging/service-bus-premium-messaging)

* Asynchronous Messaging Primer: [^](https://msdn.microsoft.com/library/dn589781.aspx)

* Compare Azure IoT options[^](https://docs.microsoft.com/en-us/azure/iot-suite/iot-suite-options)

### Azure Media Services[^](https://docs.microsoft.com/en-in/azure/media-services/media-services-overview)
* Microsoft Azure Media Services is an extensible cloud-based platform that enables developers to build scalable media management and delivery applications
* Azure Media Services concepts[^](https://docs.microsoft.com/en-in/azure/media-services/media-services-concepts)
* Media Services Applications
    * Live event Streaming service with CDN capabilities
    * Studio grade encoding
    * Content protection and encryption
    * Distribute content across multiple channels and devices
	

## Storage Queues vs. Service Bus Queues [^](https://docs.microsoft.com/en-in/azure/service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted)


|||
|:---|:---|
| **Storage Queues** |  **Service Bus Queues** |
| Arbitrary ordering |  FIFO guaranteed ordering |
| Delivery at least once, possible multiple times|  Delivery at least once and at most once |
| 30 second default locks can be extended to 7 days | 60 seconds default locks can be renewed |
| Supports in-place updates of the message content | Messages are finalized once consumed |
| Can integrate with WF through a custom activity | Native integration with WCF and WF|
| | |

|||
|:--|:--|
| **Azure Service Bus Queues** |  **Azure Storage Queues** |
| Message lifetime >7 days | Message lifetime <7days |
| Guaranteed (first in–first out) ordered |  Queue size >80 GB |
| Duplicate detection | Transaction logs |
| Message size ≤1 MB | Message size ≤64 KB |
|||


### When to use which Azure service for messages or events [^](https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services)

||||||||
|:--|:--|:--|:--|:--|:--|:--|
|  | Event Grid | Event Hubs | IoT Hub | Topics |Service Bus Queues | Storage Queues |
|Event ingestion | X | X | X |
|Device management| | | X | | | |
|Messaging|X|X|X|X|X|X|
|Multiple consumers|X| X| X| X| | |
|Multiple senders |X |X |X |X |X |X |
|Use for decoupling| |X |X |X |X |X |
|Use for publish/subscribe| X |
|Max message size| 64 KB |256 KB| 256 KB| 1 MB| 1 MB| 64 KB |
||||||||


## Azure Security Center[^](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro)

Why use Security Center:
* Centralized policy management:
    - A security policy defines the desired configuration of the workloads
* Continuous security assessment: 
    - Security Center analyzes the security state of compute resources, virtual networks, storage and data services, and applications.
* Actionable recommendations
* Advanced cloud defenses
* Prioritized alerts and incidents
* Integrated security solutions

Security Center Pricing Tiers[^](https://docs.microsoft.com/en-us/azure/security-center/security-center-pricing):
* Free tier - provides security policy, continuous security assessment, and actionable security recommendations.
* Standard tier - Hybrid security (on prem and Cloud), Advanced threat detection, Access and application controls, OS security configuration customization.

Note: To customize OS security configurations, you must be assigned the role of Subscription Owner, Subscription Contributor, or Security Administrator

The Azure Security Center dashboard has the following sections:
* Overview - lists all the Recomendations, Security Solutions, Alerts and Incidents, Events
* Prevention - Shows resources in Compute, Networking, Storage & Data, Applications
* Detection (Advanced Threat Detection feature)- Security Alerts, Most Attacked reources


# 4. Design Security and Identity Solutions (20-25%)
* Design AD Connect synchronization[^](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect)
* Design federated identities using Active Directory Federation Services (AD FS) [^](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-azure-adfs)
* Design solutions for Multi-Factor Authentication (MFA)
* Design an architecture using Active Directory on-premises and Azure Active Directory (AAD)
* Determine when to use Azure AD Domain Services
* Design security for Mobile Apps using AAD