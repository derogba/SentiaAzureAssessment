# Sentia Azure Assessment - Transformation and Migration to the Public Cloud

### Problem Statement
The customer is interested in leveraging the maximum potential availability within Azure to migrate her existing web application infrastructure to the Public Cloud. 
The solution needs to be:
* highly available, scalable, and flexible
* utilize only native Azure Services and Products.
* deployable completely using Infrastructure as Code (IaC) from a single codebase 

### Existing Infrastructure
The solution will assess the client strategy to migrate on-premises NodeJS customer facing web application behind a NGINX reverse proxy to the public cloud by addressing the following challenges.
* Consider OS level access in the developed solution  
* MongoDB is to be retained as the database technology
* Processes generating PDF files which are stored locally. 
* The finance team needs to have access to these files via FTP. 
* All services are hosted on several virtual machines.
* The 3 environments, namely Test, Acceptance and Production are to be kept in Azure, while following best practices in terms of workload isolation. 
* The region of choice is West Europe.
* They want to go live in Azure within 9 months.

### Deliverables
1.	An architectural design of the solution. 
2.	Short presentation describing the solution and mapping the choices made to the various requirements.
3.	ARM Templates MVP solution demo.
4.	Git repository of the solution portfolio


## Proposed Solution

### Description
The NodeJS application will be hosted in Azure App Service, which supports hosting Node.js apps in both Linux (Node versions 12, 14, and 16) and Windows (versions 12 and 14) server environments. The MongoDB database will be hosted in Azure Cosmos DB, a cloud native database offering a 100% MongoDB compatible API.

## Requirements Mapping

| Requirement  | Recommendation  |
|---|---|
| Consider OS level access in the developed solution   |Deploy Azure Wep App to leverage the maximum potential high availability, scalability, and performance within with Azure. Reduce administrative overhead of managing virtual machines   |
|Retaining MongoDB as the database technology.   | Deploy CosmosDB.
Azure Cosmos DB API for MongoDB makes it easy to use Cosmos DB as if it were a MongoDB database. You can leverage your MongoDB experience and continue to use your favorite MongoDB drivers, SDKs, and tools by pointing your application to the API for MongoDB account's connection string.  |
| Processes generating PDF files which are stored locally.  |  Azure Files take advantage of fully managed file shares in the cloud that are accessible via the industry-standard SMB and NFS protocols. Azure Files shares can be mounted concurrently by cloud or on-premises deployments of Windows, Linux, and macOS. Azure Files shares can also be cached on Windows Servers with Azure File Sync for fast access near where the data is being used. |
|The finance team needs to have access to these files via FTP.    | Azure Files take advantage of fully managed file shares in the cloud that are accessible via the industry-standard SMB and NFS protocols. Azure Files shares can be mounted concurrently by cloud or on-premises deployments of Windows, Linux, and macOS. Azure Files shares can also be cached on Windows Servers with Azure File Sync for fast access near where the data is being used.  |
| All services are hosted on several virtual machines.  |App Service plan provides the managed virtual machines (VMs) that host your app. All apps associated with a plan run on the same VM instances.   |
| The Test, Acceptance and Production environments are to be kept in Azure, while following best practices in terms of workload isolation.  | The Isolated App Service plan tier allows you to create separate deployments slots for the Test, Acceptance and Production environments.  |
| The region of choice is West Europe.  | The regional pairing of West Europe (Netherland) with North Europe (Ireland) will make the application highly scalable.  |
| They want to go live in Azure within 9 months.   | Go-live can occur within 3 months by leveraging the recommendations above.  |

## High Level Solution Architecture

 The proposed design architecture provides a strategy to run an Azure App Service application in multiple regions to achieve high availability, improve user experience and provide a business continuity and disaster recovery plan.
There are several general approaches to achieve high availability across regions:
* Active/Passive with hot standby: traffic goes to one region, while the other waits on hot standby. Hot standby means the VMs in the secondary region are allocated and running at all times.
* Active/Passive with cold standby: traffic goes to one region, while the other waits on cold standby. Cold standby means the VMs in the secondary region are not allocated until needed for failover. This approach costs less to run but will generally take longer to come online during a failure.
* Active/Active: both regions are active, and requests are load balanced between them. If one region becomes unavailable, it is taken out of rotation.
* The Active/passive with hot standby approach is recommended as it provides the best proven practices for improving scalability and performance in an Azure App Service web application.

## Workflow
A multi-region architecture can provide higher availability than deploying to a single region. If a regional outage affects the primary region, you can use Front Door to fail over to the secondary region. This architecture can also help if an individual subsystem of the application fails.
* Primary and secondary regions. This architecture uses two regions to achieve higher availability. The application is deployed to each region. During normal operations, network traffic is routed to the primary region. If the primary region becomes unavailable, traffic is routed to the secondary region.
* Front Door. Front Door routes incoming requests to the primary region. If the application running that region becomes unavailable, Front Door fails over to the secondary region.
* Geo-replication of Cosmos DB.

## Components

Key technologies used to implement the proposed design architecture:
on
<span style="color:blue">Azure Active Directory (Azure AD)</span> is an enterprise identity service that provides single sign-on, multifactor authenticati, and conditional access to guard against 99.9 percent of cybersecurity attacks.

<span style="color:blue">App Service plan</span> provides the managed virtual machines (VMs) that host your app. All apps associated with a plan run on the same VM instances.

<span style="color:blue">Web app</span> is a typical modern application that contains both a website and one or more RESTful web APIs. A web API might be consumed by browser clients through AJAX, by native client applications, or by server-side applications. 

<span style="color:blue">Deployment slots</span> lets you stage a deployment and then swap it with the production deployment. That way, you avoid deploying directly into production

<span style="color:blue">Azure DNS</span> Use Azure DNS to host your Domain Name System (DNS) domains in Azure. Manage your DNS records using the same credentials, and billing and support contract, as your other Azure services. Seamlessly integrate Azure-based services with corresponding DNS updates and streamline your end-to-end deployment process.

<span style="color:blue">Azure Content Delivery Network</span> offers a global solution for rapidly delivering content. Save bandwidth and improve responsiveness when encoding or distributing gaming software, firmware updates, and IoT endpoints. Reduce load times for websites, mobile apps, and streaming media to increase user satisfaction globally

<span style="color:blue">Azure Front Door</span> is a layer 7 load balancer. In this architecture, it routes HTTP requests to the web front end. Front Door also provides a web application firewall (WAF) that protects the application from common exploits and vulnerabilities.

<span style="color:blue">some *blue* Azure Function</span>.are used to run background tasks. Functions are invoked by a trigger, such as a timer event or a message being placed on queue.

<span style="color:blue">Queue</span> the application queues background tasks by putting a message onto an Azure Service Bus queue. The message triggers a function app.

<span style="color:blue">Azure Cache for Redis</span> is used to store semi-static data.

<span style="color:blue">Azure Content Delivery Network (CDN)</span> is used to cache publicly available content for lower latency and faster delivery of content.

<span style="color:blue">Azure Files</span> Take advantage of fully managed file shares in the cloud that are accessible via the industry-standard SMB and NFS protocols. Azure Files shares can be mounted concurrently by cloud or on-premises deployments of Windows, Linux, and macOS. Azure Files shares can also be cached on Windows Servers with Azure File Sync for fast access near where the data is being used

<span style="color:blue">Azure Cosmos DB</span> Azure Cosmos DB is a fully managed NoSQL database service for modern app development. Get guaranteed single-digit millisecond response times and 99.999-percent availability, backed by SLAs, automatic and instant scalability, and open-source APIs for MongoDB. 

<span style="color:blue">Azure Search</span> adds search functionality such as search suggestions, fuzzy search, and language-specific search.

### Recommendations

**Regional pairing.**Each Azure region is paired with another region within the same geography. In this case West Europe (Netherland) is paired with North Europe (Ireland).

Benefits of doing so include:
* If there is a broad outage, recovery of at least one region out of every pair is prioritized.
* Planned Azure system updates are rolled out to paired regions sequentially to minimize possible downtime.
* In most cases, regional pairs reside within the same geography to meet data residency requirements.

**Resource groups.**The primary region, secondary region, and Traffic Manager will be placed in separate resource groups. This will ensure that the resources deployed to each region are managed as a single collection.

## Front Door configuration

**Routing.**Front Door supports several routing mechanisms. For our design, the priority routing setting will be used as it enables Front Door to send all requests to the primary region unless the endpoint for that region becomes unreachable. At that point, it automatically fails over to the secondary region.

**Health probe.**Front Door uses an HTTP (or HTTPS) probe to monitor the availability of each back end. The probe gives Front Door a pass/fail test for failing over to the secondary region. 

As a best practice, create a health probe path in your application backend that reports the overall health of the application. This health probe should check critical dependencies such as the App Service apps, storage queue, and SQL Database. 

**Cosmos DB.**Cosmos DB supports geo-replication across regions in active-active pattern with multiple write regions. 
 
## Note
All the replicas belong to the same resource group.

**Storage.**For Azure Storage, read-access geo-redundant storage (RA-GRS) is recommended. With RA-GRS storage, data is replicated to a secondary region. If there is a regional outage or disaster, the Storage administrator can decide to perform a geo-failover to the secondary region. 

For Queue storage, create a backup queue in the secondary region. During failover, the app can use the backup queue until the primary region becomes available again. That way, the application can still process new requests.

### Considerations

## High Availability – across regions

<span style="color:blue">Azure Front Door</span> automatically fails over if the primary region becomes unavailable. When Front Door fails over, there is a period of time (usually about 20-60 seconds) when clients cannot reach the application. 

<span style="color:blue">CosmosDB</span> RPO and recovery time objective (RTO) for Cosmos DB provide trade-offs between availability, data durability, and throughput. Cosmos DB provides a minimum RTO of 0 for a relaxed consistency level with multi-master or an RPO of 0 for strong consistency with single-master.

<span style="color:blue">Storage.</span> RA-GRS storage provides durable storage, but it's important to understand what can happen during an outage:

* If a storage outage occurs, there will be a period of time when you don't have write-access to the data. You can still read from the secondary endpoint during the outage.
* If a regional outage or disaster affects the primary location and the data there cannot be recovered, the Azure Storage team may decide to perform a geo-failover to the secondary region.
* Data replication to the secondary region is performed asynchronously. Therefore, if a geo-failover is performed, some data loss is possible if the data can't be recovered from the primary region.
* Transient failures, such as a network outage, will not trigger a storage failover. Design your application to be resilient to transient failures. Mitigation options include:

    * Read from the secondary region.
    * Temporarily switch to another storage account for new write operations (for example, to queue messages).
    * Copy data from the secondary region to another storage account.
    * Provide reduced functionality until the system fails back.

## Manageability
If the primary database fails, perform a manual failover to the secondary database. The secondary database remains read-only until you fail over.

## DevOps
This architecture follows the multi region deployment recommendation, described in the DevOps section of the Azure Well Architected Framework.
This architecture builds on the one shown in Improve scalability in a web application, see DevOps considerations section.

## Monitoring
A recommended practice is adding Application Insights to your code during development using the Application Insights SDKs and customizing per application. These open-source SDKs are available for most application frameworks. To enrich and control the data you collect, incorporate the use of the SDKs both for testing and production deployments into your development process. 
Like Application Insights, Log Analytics provides tools for analyzing data across sources, creating complex queries, and sending proactive alerts on specified conditions. You can also view telemetry in the Azure portal. Log Analytics adds value to existing monitoring services such as Azure Monitor and can also monitor on-premises environments.
Both Application Insights and Log Analytics use Azure Log Analytics Query Language. You can also use cross-resource queries to analyze the telemetry gathered by Application Insights and Log Analytics in a single query.
Security
Sensitive information and compliance requirements affect data collection, retention, and storage. 
The following security considerations may also apply:
•	Develop a plan to handle personal information if developers are allowed to collect their own data or enrich existing telemetry.
•	Consider data retention. For example, Application Insights retains telemetry data for 90 days. Archive data you want access to for longer periods using Microsoft Power BI, Continuous Export, or the REST API. Storage rates apply.
•	Limit access to Azure resources to control access to data and who can view telemetry from a specific application. 
•	Consider whether to control read/write access in application code to prevent users from adding version or tag markers that limit data ingestion from the application.
•	Add governance mechanisms to enforce policy or cost controls over Azure resources if needed. For example, use Log Analytics for security-related monitoring such as policies and role-based access control, or use Azure Policy to create, assign and manage policy definitions.
•	To monitor potential security issues and get a central view of the security state of your Azure resources, consider using Microsoft Defender for Cloud
Pricing
Use the pricing calculator to estimate costs. The recommendations below may help reduce cost.
Azure Front Door billing has three pricing tiers: outbound data transfers, inbound data transfers, and routing rules. For more info See Azure Front Door Pricing. The pricing chart does not include the cost of accessing data from the backend services and transferring to Front Door. Those costs are billed based on data transfer charges, described in Bandwidth Pricing Details.

Azure Cosmos DB. There are two factors that determine Azure Cosmos DB pricing:
•	The provisioned throughput or Request Units per second (RU/s).
There are two types of throughput that can be provisioned in Cosmos DB, standard and autoscale. Standard throughput is billed for the throughput provisioned hourly. Autoscale throughput is billed for the maximum throughput consumed hourly.
•	Consumed storage. You are billed a flat rate for the total amount of storage (GBs) consumed for data and the indexes for a given hour.


Deployment
Deployment Plan
1.	Clone the Repository. Clone the on-premises node.js wep app repository in advance of the migration. 
2.	Create the Azure App Service
3.	Connect your App Service to your Cosmos DB
4.	Deploy application code to Azure
5.	Browse to the application
6.	Configure and view application logs
7.	Inspect deployed files using Kudu
