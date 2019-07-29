![Microsoft Cloud Workshops icon](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
SAP NetWeaver on Azure
</div>

<div class="MCWHeader2">
Whiteboard design session student guide
</div>

<div class="MCWHeader3">
June 2019
</div>

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

©  2019 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [SAP on Azure whiteboard design session student guide](#sap-on-azure-whiteboard-design-session-student-guide)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Step 1: Review the customer case study](#step-1-review-the-customer-case-study)
        - [Customer situation](#customer-situation)
        - [Customer needs](#customer-needs)
        - [Technical requirements](#technical-requirements)
        - [Customer objections](#customer-objections)
        - [Infographic for common scenarios](#infographic-for-common-scenarios)
    - [Step 2: Design a proof of concept solution](#step-2-design-a-proof-of-concept-solution)
        - [Architecting network connectivity](#architecting-network-connectivity)
        - [Architecting (ASCS) components](#architecting-ascs-components)
        - [Architecting application components](#architecting-application-components)
        - [Architecting database components](#architecting-database-components)
        - [Determining estimated cost of the solution](#determining-estimated-cost-of-the-solution)
    - [Step 3: Present the solution](#step-3-present-the-solution)
    - [Wrap-up](#wrap-up)
    - [Additional references](#additional-references)

<!-- /TOC -->

#  SAP on Azure whiteboard design session student guide

## Abstract and learning objectives 

In this whiteboard design session, you will learn how to deploy SAP on Azure infrastructure components as well as perform an SAP on Azure need assessment, take part in an SAP on Azure architecture design discussion, and respond to an RFI/RFP for SAP on Cloud opportunity.

At the end of this whiteboard design session, you will be better able to architect for SAP running on Azure infrastructure as a service components and understand proper configuration of SAP NetWeaver ASCS, database and application servers in Azure.

## Step 1: Review the customer case study 

**Outcome**

Analyze your customer's needs.

Timeframe: 15 minutes

Directions: With all participants in the session, the facilitator/SME presents an overview of the customer case study along with technical tips.

1.  Meet your table participants and trainer.

2.  Read all of the directions for steps 1-3 in the student guide.

3.  As a table team, review the following customer case study.

### Customer situation

Contoso Group is a US-based telecommunications company with headquarters in NY.  

Contoso Group maintains on-premises deployments of SAP Business Suite on Windows Server 2008 R2 with a SQL Server 2008 R2 in its US-based data center.

The IT organization finds it challenging to expand the SAP landscape to accommodate growing number of SAP-related business requests. This is related to the time and resources required to move through development and testing phases and implement new business processes into production. In addition, IT finds it difficult to adjust the size of the production environment in order to accommodate varying level of workloads. IT is also concerned about lack of high availability and disaster recovery capabilities in the existing deployment. The estimates of providing such capabilities for production workloads exceed the budget allocated for this purpose by far.

As the result, agility of the organization is hampered by capital expenditures and the time necessary to acquire and provision project-related SAP development and testing environments. Representatives of central IT management, including the company CIO Andrew Cross and its VP of IT Operations are concerned about managing test/dev infrastructure costs. There are also concerns regarding aging data center and hardware. Business Development Manager (BDM) and the company CFO would like to lower TCO for the SAP landscape, including the production ECC environment. 

Contoso Leadership and Planning Groups introduced a mandate to drastically reduce server and storage hardware in their own datacenters to minimize IT related costs. Contoso has already a number of their non-SAP systems migrated to Azure. The leadership asked Contoso IT to look into the possibility to migrate its existing SAP Business Suite to cloud. 

Contoso IT decided to leverage its knowledge of the Microsoft cloud platform and existing ExpressRoute connectivity to host its SAP landscape in Azure. Considering that Contoso finance and supply chain teams strongly rely on SAP Business Suite, the systems should be highly available, and their performance must be predictable and consistent.  In addition, the management team wants to leverage disaster recovery capabilities offered by Azure in order to ensure resiliency in case the primary region hosting the new deployment becomes unavailable. 

Before migrating the production environment, Contoso wants to test its new deployment approach by provisioning development, and UAT environments in Azure.

**Existing Contoso SAP environment**

Contoso Group has an existing on-premises deployment of SAP ECC (SAP ERP Central Component) 6.0 on Windows Server 2008 R2 with a SQL Server 2008 R2 database in its US-based data center near New York City. The deployment consists of the development, test, quality assurance, and production systems. 

The production app tier consists of three physical servers, each with 8 CPU cores and 32GB RAM. The production database tier is hosted on a single physical server with the same hardware configuration. The average performance of the application tier and the database tier is about 30,000 SAPS and 15,000 SAPS, respectively. The production SAP database workload generates about 20,000 IOPS. The current production DB size is 750GB. According to the estimates of the Contoso SAP team, the database is bound to grow to about 1TB in the next few years. In case of a disaster affecting the production instance, the business expects the RPO and RTO of 15 minutes and 2 hours, respectively. 

![A Customer environment diagram has a box with Test/Dev, Production, SAN storage, Tape devices, End users, and Legacy systems. A callout with a list of technical requirements points to Production. An arrow labeled Azure Migration points from the box to to a cloud with Dev/Test, DR and Production on Azure. Questions on the bottom are Compute/storage sizing, High availability and DR, and Cloud migration.](images/Whiteboarddesignsessiontrainerguide-SAPonAzureimages/media/image2.png)

**Customer objectives**

As Andrew Cross, the company's CIO wants to significantly improve times it takes to provision development and test SAP environments. At the same time, he stresses the importance of minimizing their cost. He also points out the criticality of the SAP production workloads and the need of protecting them against unplanned downtime. As he stated, "The existing on-premises SAP development and test environments no longer meet the agility required by the fast pace of changes in our business. In addition, our operational dependencies on SAP applications force us to seek reasonably priced availability and disaster recovery capabilities for our production SAP deployments."

Contoso CIO and the company CFO, along with Business Development Manager (BDM), Director of SAP Business Analysts, and Director of SAP Operations are all open to taking advantage of Azure as a platform and want their respective teams to be able to leverage the scalability and resiliency provided by the cloud platform.

### Customer needs 

-   Reduce time to deploy new business processes to have competitive customer offers.

-   Reduce time required to provision development and test environments.

-   Lower costs incurred by the development, test, and production environments.

-   Identify an estimated monthly infrastructure cost for operating your solution.

### Technical requirements

-   SAP ERP/ECC 6.0 NetWeaver is the application tier.

-   SQL Server is the data tier.

-   Application tier on the production system requires total of 30,000 SAPS.

-   Database tier on the production system requires total of 15,000 SAPS.

-   Database tier on the production system needs 20,000 IOPS.

-   The size of database files on the production system is 750GB, with the expected growth to 1TB.

-   RPO=15 min, RTO=6 hours

-   SLA : Availability 99.9%, RPO=15min, RTO = 2hr

### Customer objections 

-   If I have already paid for my hardware to run my dev/test environment, how am I getting any cost savings by moving it to Azure? How much more is your cloud solution costing me?

-   What if I need my cloud resources to access on-premises resources?

-   Will Azure meet our security and compliance requirements?

-   Do I have to pay for resources when they are stopped?

-   For my dev environment, can I automate stopping VMs during idle times?

### Key design considerations

1.  Two primary HA/DR options

    -   HA in an Availability Set and DR across regions (DR replica can coexist with QA in the second region)

        This option represents the traditional design, which relies on well-established concept of availability sets and leverages a pair of Azure regions in the same geography. An Azure Availability Set is a logical grouping capability that helps ensure that the VM resources that you place within the Availability Set are failure-isolated from each other when they are deployed within an Azure datacenter. Azure ensures that the VMs you place within an Availability Set run across multiple physical servers, compute racks, storage units, and network switches. This configuration is referred to as placements in different update and fault domains. These placements usually are within an Azure datacenter. Assuming that power source and network issues would affect the datacenter that you are deploying, all your capacity in one Azure region would be affected. 

        Note that it is important to consider high availability and disaster recovery provisions separately for each component of this design:

        -   HANA Database layer - both high availability and disaster recovery is implemented by using HANA System Replication (synchronous or asynchronous, depending on the option). In case of synchronous replication, the high availability functionality relies additionally on Linux-based Pacemaker clustering.

        -   SAP Central Services (ASCS) layer - high availability is implemented by using operating system-level clustering (either Windows Server Failover Clustering or Linux-based Pacemaker clustering, depending on the underlying operating system). Disaster recovery relies across Azure regions on Azure Site Recovery, which replicates virtual disks of cluster members to the secondary site. Azure Site Recovery is also used to orchestrate failover between sites.

        -   SAP file share layer - high availability of the sapmnt share is implemented by using either Windows-based file share (based on Windows Storage Spaces Direct cluster hosting Scale-Out File Server) or Linux-based NFS mount (based on Azure NetApp Files or Linux Distributed Replicated Block Device). Disaster recovery relies on Azure Site Recovery, which replicates the file sharing infrastructure to the secondary site. When using Azure NetApp Files NFS-based approach, NetApp offers a SaaS based solution called NetApp Cloud Sync. You can use any file based-copy tool (such as rsync) to replicate data to the secondary site (including file or folder permissions).      

        -   SAP Application Server layer - high availability is implemented by simply deploying multiple application servers. Disaster recovery across Azure regions relies on Azure Site Recovery, which replicates virtual disks of application servers to the secondary site. Azure Site Recovery is also used to orchestrate failover between sites.

        ![A diagram representing the HA in an Availability Set and DR across regions design option.](images/Whiteboarddesignsessiontrainerguide-SAPonAzureimages/media/image17.png)

    -   HA/DR across Availability Zones

        This option represents a different design that takes advantage of availability zones, representing separate, physically isolated data centers within the same region. The placement of datacenters that represent Azure Availability Zones constitutes a compromise between delivering acceptable network latency to services deployed in different zones, and a distance between datacenters. The distance matters because even though majority of destructive events are not likely to affect the infrastructure of all Availability Zones in the same region, history offers examples of larger scale disasters that could have such impact. To account for such occurrences, organizations might have the minimum distance requirement between the location of their production and disaster recovery sites. For most Azure customers, the minimum distance definition necessitates the use of two or more Azure regions.

        Note that it is important to consider high availability and disaster recovery provisions separately for each component of this design. In this case, the highly available production components are distributed across Availability Zone 1 and Availability Zone 2:

        -   HANA Database layer is hosted by a Linux Pacemaker cluster containing an active VM instance residing in Availability Zone 1 and standby VM instance residing in Availability Zone 2, with HANA Synchronous replication between nodes of the cluster. Clustering is implemented by using Pacemaker.

        -   ASCS layer hosted by a Windows cluster contains an active VM instance residing in Availability Zone 1 and a standby VM instance residing in Availability Zone 2. 

        -   SAP file share layer on Windows cluster contains a one VM node residing in Availability Zone 1 and another VM node residing in Availability Zone 2. 

        -   SAP Application Server layer consists of a PAS server VM in Availability Zone 1 and AAS in Availability Zone 2. Clustering is not used in this case since high availability is built-into the PAS and AAS functionality.  

        ![A diagram representing the HA/DR across Availability Zones design option.](images/Whiteboarddesignsessiontrainerguide-SAPonAzureimages/media/image18.png)

    In addition, when choosing the DR approach for your solutions, consider:

    -   Requirements, which can be divided into two categories:

        -   Business requirements:

            -   Minimum data loss in case of a disaster

            -   Failover completed within hours (including SAP application layer)

            -   Clear failover process

            -   Failover process is abstracted from users and subsystems

            -   Minimum cost

        -   IT requirements:

            -   Production environment is virtualized, on bare metal, or in the cloud

            -   Minimum overhead on production environment (including networking)

            -   No need for traditional SAN/tape backup solutions

    -   Disaster recovery mechanism for each of the three primary components:

        -   Database replication (e.g. SQL Server AlwaysOn)

        -   File replication (e.g. file copy)

        -   VM replication (Azure Site Recovery)

        ![DR solutions for SAP on Azure.](images/Whiteboarddesignsessiontrainerguide-SAPonAzureimages/media/image19.png)

1.  Azure VM compute & storage sizing principles for SAP workloads

    -   Start by identifying customer requirements, including: 

        -   Type of environment: production, or non-production (dev and test)

        -   SAPS required for the SAP application and database tiers

        -   IOPS and storage throughput required for the database tier

        -   System activity period (consider Azure automation to stop non-production VMs during idle times)

    -   Use Premium storage for SAP databases (both database files and log files).

    -   SAP application servers are usually deployed on Standard storage, but can be deployed on small Premium storage disks (P4, P6) if using a single server in order to benefit from single instance SLA.

    -   For database servers, use Windows storage spaces or LINUX volume managers to stripe disks and aggregate IOPS, throughput and storage capacity

    -   Next, use select a compute SKU based on SAPS required for database tier

    -   Finally, select compute SKUs based on SAPS required for application tier

    In general, there are two ways to determine the proper size of SAP systems to be implemented in Azure – by using reference sizing or by employing SAP Quick Sizer.

    For existing on-premises systems, you should reference system configuration and resource utilization data.  The system utilization information is collected by the SAP OS Collector and can be reported via transaction OS07N as well as the Early Watch Report.  Similar information can be retrieved by leveraging any system performance and statistics gathering tools.

    The SAP EarlyWatch Alert is a diagnostic service that delivers monitoring of both SAP and non-SAP systems in the SAP Solution Manager. The service captures variety of performance-related data, including, for example, general component status, system configuration, average response times, system load, critical error messages and process interruptions. 

    For new systems, you should use SAP quick sizer, which can be accessed at https://service.sap.com/quicksizer. Note that access to it requires an Online Service System account. Online Service System (or, in short, OSS) is used to provide online access to SAP users and partners to SAP applications and data
Quick Sizer offers two independent sizing approaches: user-based and throughput-based. For smaller environments, the user-based sizing approach might be sufficient. Business requirements are determined by the number of users performing business specific tasks. Users are grouped per business task according to their estimated usage activity. For larger and more complex environments, it is recommended to use throughput-based sizing. This takes into account not only the number of users performing business specific tasks but also an information about business processes and business objects.

    For the purpose of this design session, you will assume that the relevant sizing data has already been collected. In order to identify the configuration of our Azure-based deployment, you will perform three tasks:

    -   Choose VM type of SAP ASCS, application, and database layers based on the provided SAPS requirements

    -   Choose storage type and the number of disks for SAP database layer based on the provided IOPS, throughput, and latency requirements

    -   Identify the number of VMs for SAP ASCS, application, and database layers based on the provided resiliency requirements

    ![A table of customer requirements and sizing for Azure has rows labeled SOFS, ASCS, Application, and Database. Customer requirement columns are SAPS needed, IOPS needed, and Current DB Volume. Nine sizing for Azure columns include items such as Azure VM type, number of vms and whether they are active, SAPS provided, Database Files Premium Storage SKU and number of disks, IOPS for Database Files provided, Storage size allocated for DB files, Database Log Premium Storage SKU and number of disks, and Latency for Log. The Sizing for Azure columns have question marks.](images/Whiteboarddesignsessiontrainerguide-SAPonAzureimages/media/image13.png "How to size SAP on Azure VMs")

    In order to identify the VM types, you will use the information provided in the following table:

    ![A table containing the listing of performance characteristics of SAP-certified Azure VM sizes.](images/Whiteboarddesignsessiontrainerguide-SAPonAzureimages/media/image14.png)

    In order to identify disks for SAP database layer, you will use the information provided in the following table:

    ![A table containing the listing of performance characteristics of Premium Storage disks.](images/Whiteboarddesignsessiontrainerguide-SAPonAzureimages/media/image15.png)

    Premium Storage offers the following characteristics:

    -   Consistent low latency SSD with predictable throughput

    -   Intended for high-performance IO-intensive DB workloads

    -   Single digit ms latencies

    -   Supports up to 32 TB blob/disk size

    -   Stripe (to aggregate IOPS, throughput, and capacity) up to 64 disks for a total of 2,048 TB

    -   Disk performance is dependent on the disk size, with up to 20,000 IOPS per disk 

    -   Ensure to account for Azure VMs level IOPS limits

    -   Pay for what you provision (cost per GB differs by region)

1.  When designing the architecture of your solution, consider the following factors: 

    -   the choice of the Azure region(s)

    -   the hybrid connectivity method (ExpressRoute and/or Site to Site VPN)

    -   the choice of the virtual gateway SKUs

    -   the method of implementing high-availability of the database layer

    -   the method of implementing high-availability of the ASCS and application servers

    -   the role of Azure services in your solution

    -   the method of implementing disaster recovery in your solution

    ![A diagram of Subnet business suite, NetWeaver with HA and DR has two Azure regions. The first region includes a GW Subnet, subnet #1 - AP Prod, Subnet #2 - DB Prod, and subnet #3 - Mgmt. The second region has GW subnet, subnet #4  - AP DR, and subnet #5 - DB DR, and Subnet #6 Mgmt. Both regions are connected via ExpressRoute to a building icon. There are collouts referencing these components labeled, respectively, VPN/ExpressRoute Gateway SKU?, How to design highly available SQL Server VMs?, What other Azure services can be used? Domain controller and other VMs? ExpressRoute or/and S2S VPN?, and What are DR Solutions? ](images/Whiteboarddesignsessiontrainerguide-SAPonAzureimages/media/image16.png "How to build high availability across regions")

1.  Account for backup requirements by incorporating some or all of the following capabilities into your solution:

    -   Online backup of SQL Server databases, files and OS system state with minimum overhead

    -   Backup to Azure Blob Storage (Azure Backup Vault)

    -   Point-in-time restore

    -   Minimizing backup storage (via incremental backups)

    -   Centralized console for monitoring and alert capabilities

    ![Enterprise Backup Solutions for LOB applications/SAP on Azure](images/Whiteboarddesignsessiontrainerguide-SAPonAzureimages/media/image20.png "Enterprise Backup Solutions for LOB applications/SAP on Azure")

1.  When evaluating the cost of your solution: 

    -   Use Azure Pricing Calculator

    -   Consider best OS licensing option(s):

        -   It is common NOT to include Windows license costs because of Azure Hybrid Use Benefits

        -   Linux OS subscription costs can be based on Azure Marketplace

    -   Provide assumptions for ExpressRoute bandwidth

1.  To summarize, when designing your solution, account for the following requirements:

    -   problem statement and customer benefits

    -   secure and low latency network connectivity

    -   Windows-based application tier

    -   SQL Server-based database tier

    -   Specific sizing requirements (SAPS) for the application tier

    -   Specific sizing requirements (SAPS) for the database tier

    -   Specific IO requirements

    -   HA capabilities (as part of 3-tier deployment)

    -   Load-balanced front-end for the database tier

    -   Backup solution and storage for data retention

    -   Monitoring solution

    -   Security solution

    -   DR capabilities (with multi-region deployment)

    -   Monthly cost estimate

    -   Migration methodology

    ![Requirements](images/Whiteboarddesignsessiontrainerguide-SAPonAzureimages/media/image21.png "Requirements")


### Infographic for common scenarios

![SAP on Azure - a wide varity of compute instances](images/SoA-WideVerietyofCompute.png "Wide variety of Compute instances")

![How to Size SAP Any DB (SQL Server, Oracle, DB2, ASE) on Azure VM ](images/SizeSAPonAnyDB.png "How to Size SAP Any DB (SQL Server, Oracle, DB2, ASE) on Azure VM")

![SAP on Azure Backup Solutions](images/SAPonAzureBackupSolutions.png)

## Step 2: Design a proof of concept solution

**Outcome**

Design a solution and prepare to present the solution to the target customer audience in a 15-minute chalk-talk format.

Timeframe: 60 minutes

**Business needs**

Directions:  With all participants at your table, answer the following questions and list the answers on a flip chart:

1.  Who should you present this solution to? Who is your target customer audience? Who are the decision makers?

2.  What customer business needs do you need to address with your solution?

**Design**

Directions: With all participants at your table, respond to the following questions on a flip chart:

*Architecting a multi-tier SAP solution*

Using the features of Azure and the requirements from the customer, design the application infrastructure and network for Contoso SAP deployment in Azure. Consider such factors as cost of ownership of entire SAP landscape as well as high availability and disaster recovery capabilities for the production environment.

![Benefits of migration to Azure. Three empty current state boxes point to three empty future state boxes.](images/Whiteboarddesignsessiontrainerguide-SAPonAzureimages/media/image4.png)

Make sure that your design covers the following items:

*High-level architecture*

**Task:** Identifying the target Azure region(s) and the number of tiers.

-   Identify the optimal choices for Azure region(s) where the solution should be deployed.

-   Choose between the 2-tier and 3-tier architecture.

-   Ensure that high-availability and disaster recovery requirements are satisfied.

### Architecting network connectivity

**Task:** Design a hybrid network (at a high-level) that will allow you to meet all the customer requirements and support your design for moving Contoso SAP landscape to Azure.

-   Design virtual networking in Azure and suggest the hybrid connectivity configuration between Contoso on-premises environment and Azure.

### Architecting (ASCS) components

**Task:** Design a highly available and geo-redundant solution for hosting the (A)SCS components of the SAP production environment.

-   Design the architecture that will provide high availability and disaster recovery capabilities for the (A)SCS components of the SAP production environment.

-   Provide the sizing information for your design.

-   Describe what technologies you will use to implement high availability.

-   Describe the disaster recovery approach.

### Architecting application components

**Task:** Design a scalable, highly available, and geo-redundant solution for hosting the SAP application components of the SAP production environment.

-   Design the architecture that will provide high availability and disaster recovery capabilities for the application components of the SAP production environment.

-   Provide the sizing information for your design.

-   Describe what technologies you will use to implement high availability.

-   Describe the disaster recovery approach.

### Architecting database components

**Task:** Design a resilient and scalable SQL Server backend configured to span geographically distributed datacenters.

-   Design the architecture that will provide high availability and disaster recovery capabilities for the database components of the SAP production environment.

-   Provide the sizing information for your design.

-   Describe what technologies you will use to implement high availability.

-   Describe the disaster recovery approach.

### Determining estimated cost of the solution

**Task:** Based on the proposed design, determine the estimated cost of the entire solution.

-   Provide the pricing of the components of your solution.

-   Exclude cost of hybrid connectivity, licensing, and support.

-   Consider cost impact of leveraging Azure VM reserved instances (RI) and Azure Hybrid Benefit (AHUB).

**Prepare**

Directions: With all participants at your table:

1.  Identify any customer needs that are not addressed with the proposed solution.

2.  Identify the benefits of your solution.

3.  Determine how you will respond to the customer's objections.

Prepare a 15-minute chalk-talk style presentation to the customer.

## Step 3: Present the solution

**Outcome**

Present a solution to the target customer audience in a 15-minute chalk-talk format.

Timeframe: 30 minutes

**Presentation**

Directions:

1.  Pair with another table.

2.  One table is the Microsoft team and the other table is the customer.

3.  The Microsoft team presents their proposed solution to the customer.

4.  The customer makes one of the objections from the list of objections.

5.  The Microsoft team responds to the objection.

6.  The customer team gives feedback to the Microsoft team.

7.  Tables switch roles and repeat Steps 2-6.

##  Wrap-up 

Timeframe: 15 minutes

Directions: Tables reconvene with the larger group to hear the facilitator/SME share the preferred solution for the case study.

### Additional references 

|    |            |
|----------|:-------------:|
| **Description** | **Links** |
| Using Azure for hosting and running SAP workload scenarios | <https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/get-started>  |
| Deploy SAP NetWeaver (Windows) for AnyDB on Azure virtual machines |  <https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/sap/> |
| SAP on MS Platform blog   | <https://blogs.msdn.microsoft.com/saponsqlserver/>  |
| SAP on Azure -- SAP Community Wiki | <https://wiki.scn.sap.com/wiki/display/VIRTUALIZATION/SAP+on+Microsoft+Azure>  |
| SAP Note 1928533 (Supported Products and Azure VM types) | <http://launchpad.support.sap.com/#/notes/1928533>  |
