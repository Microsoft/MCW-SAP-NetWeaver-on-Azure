![Microsoft Cloud Workshops icon](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
SAP NetWeaver on Azure
</div>

<div class="MCWHeader2">
Hands-on lab unguided
</div>

<div class="MCWHeader3">
June 2019
</div>


Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**
<!-- TOC -->

- [SAP NetWeaver on Azure hands-on lab step-by-step](#sap-netweaver-on-azure-hands-on-lab-step-by-step)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Overview](#overview)
    - [Solution architecture](#solution-architecture)
    - [Exercise 1: Deploy the SAP on Azure infrastructure components](#exercise-1-deploy-the-sap-on-azure-infrastructure-components)
        - [Task 1: Implement Azure virtual network](#task-1-implement-azure-virtual-network)
        - [Task 2: Deploy SAP NetWeaver application and database tier VMs](#task-2-deploy-sap-netweaver-application-and-database-tier-vms)
        - [Task 3: Deploy Windows Server 2019 jumpbox Azure VM](#task-3-deploy-windows-server-2019-jumpbox-azure-vm)
        - [Task 4: Implement Azure Active Directory Domain Services (Azure AD DS)](#task-4-implement-azure-active-directory-domain-services)
        - [Task 5: Create Azure AD DS user accounts to facilitate implementation of SAP NetWeaver](#task-5-create-azure-ad-acounts-to-facilitate-implementation-of-sap-netweaver)
        - [Task 6: Join Windows Sever 2019 Azure VMs to the Azure AD DS domain](#task-6-join-windows-server-2019-azure-vms-to-the-azure-ad-ds-domain)
        - [Task 7: Install Remote Server Administration Tools on all Azure VMs](#task-7-install-remote-server-administration-tools-on-all-azure-vms)
        - [Task 8: Prepare the Azure AD DS for deployment of SAP NetWeaver application and database tier VMs](#task-8-prepare-the-azure-ad-ds-for-deployment-of-sap-netweaver-application-and-database-tier-vms)
        - [Task 9: Configure IP addresses of Azure VMs and internal load balancers](#task-9-configure-ip-addresses-of-azure-vms-and-internal-load-balancers)
        - [Task 10: Create and configure the sapmnt share](#task-10-create-and-configure-the-sapmnt-share)
        - [Task 11: Configure User Account Control on application and the database VMs](#task-11-configure-user-account-control-on-application-and-the-database-vms)
        - [Summary](#summary)
    - [Exercise 2: Configure the SAP NetWeaver ASCS servers](#exercise-2-configure-the-sap-netweaver-ascs-servers)
        - [Overview](#overview-1)
        - [Task 1: Set up Failover Clustering on the ASCS VMs](#task-1-set-up-failover-clustering-on-the-ascs-vms)
        - [Task 2: Install the SAP ASCS components on s03-ascs-0](#task-2-install-the-sap-ascs-components-on-s03-ascs-0)
        - [Task 3: Install the SAP ASCS components on s03-ascs-1](#task-3-install-the-sap-ascs-components-on-s03-ascs-1)
        - [Task 4: Configure SAP ASCS cluster components on s03-ascs-0 and s03-ascs-1](#task-4-configure-sap-ascs-cluster-components-on-s03-ascs-0-and-s03-ascs-1)
        - [Task 5: Install the Enqueue Repliation Server instance on both nodes of the cluster](#task-5-install-the-enqueue-replication-server-instance-on-both-nodes-of-the-cluster)
        - [Summary](#summary-1)
    - [Exercise 3: Configure SAP NetWeaver database servers](#exercise-3-configure-sap-netweaver-database-servers)
        - [Overview](#overview-2)
        - [Task 1: Configure storage of the SAP database layer.](#task-1-configure-storage-of-the-sap-database-layer)
        - [Task 2: Grant elevated user rights to the SQL Server service account](#task-2-grant-elevated-user-rights-to-the-sql-server-service-account)
        - [Task 3: Install SQL Server 2017 with the SQL\_Latin1\_General\_CP850\_BIN2 collation](#task-3-install-sql-server-2017-with-the-sql\_latin1\_general\_cp850\_bin2-collation)
        - [Task 4: Install the SAP database layer](#task-4-install-the-sap-database-layer)
        - [Task 5: Implement Always-On Availability Group](#task-5-implement-always-on-availability-group)
        - [Task 6: Modify the SAP Default Profile](#task-6-modify-the-sap-default-profile)
        - [Summary](#summary-2)
    - [Exercise 4: Configure SAP NetWeaver Application servers](#exercise-4-configure-sap-netweaver-application-servers)
        - [Overview](#overview-3)
        - [Task 1: Configure storage and software prerequisites of the SAP Application layer](#task-1-configure-storage-and-software-prerequisites-of-the-sap-application-layer)
        - [Task 2: Install the SAP Primary Application Server (PAS) layer](#task-2-install-the-sap-primary-application-server-pas-layer)
        - [Task 3: Install the SAP Additional Application Server (AAS) layer](#task-3-install-the-sap-additional-application-server-aas-layer)
        - [Task 4: Install the SAP Enhanced Monitoring Extension for SAP](#task-4-install-the-sap-enhanced-monitoring-extension-for-sap)
        - [Summary](#summary-3)
    - [Lab summary](#lab-summary)
    - [After the hands-on lab](#after-the-hands-on-lab)
        - [Task 1: Delete resources](#task-1-delete-resources)

<!-- /TOC -->


# SAP on Azure hands-on lab unguided 

## Abstract and learning objectives 

In this hands-on lab you will setup and configure SAP NetWeaver by using a combination of Azure Infrastructure as a Service (IaaS) and Platform as a Service (PaaS) components. You will setup identity management by using Azure Active Directory Domain Services (Azure AD DS), file sharing by using Azure Files service, NetWeaver application and database tiers by using Azure virtual machines (VMs) and integrate them with Active Directory. You will then setup high-availability for SAP ASCS components hosted on Azure VMs by configuring failover clustering. Finally, you will configure the database tier with Always-On Availability Groups and configure the NetWeaver application servers for redundancy.

As you step through the hands-on-lab, you will learn how to optimize deployment of SAP on Azure, including configuration of SAP NetWeaver ASCS servers, SAP NetWeaver database servers, and SAP NetWeaver Application servers.

## Overview

Contoso has asked you to develop a process of provisioning a highly available, Windows Server 2019-based SAP NetWeaver deployment on Azure, with SAP Advanced Business Application Programming (ABAP) stack and SQL Server 2017 as the database tier. To provide high-availability of the ABAP SAP Central Services (ASCS) components, you will implement an instance of a failover cluster. The ASCS component will leverage Azue Files to host highly-available sapmnt share. To provide high-availability of the database tier, you will implement an instance of SQL Server Always-On Availability Group. In both cases, you will use a Cloud Witness quorum, introduced in Windows Server 2016 Failover Clustering. All Azure VMs will be using managed disks. To streamline deployment, you will take advantage of Azure Resource Manager templates and PowerShell scripts.

## Solution architecture

From the architectural standpoint, the deployment will consist of the following layers:

-   Authentication layer -- two Active Directory domain controllers

-   Shared storage -- Azure Files

-   SAP ASCS layer -- two servers configured as members of a two-node Windows Failover Clustering cluster

    -   Failover Clustering will rely on Cloud Witness-based quorum

    -   An Azure Standard Internal Load Balancer will provide a front-end IP address of the ASCS Failover Clustering role

-   SAP Application instances layer -- two SAP application servers:

    -   Primary Application Server (PAS)

    -   Additional Application Server (AAS)

-   SAP Database layer -- two servers hosting separate instances of SQL Server 2017 configured as nodes of a SQL Server Always-On Availability Group cluster

    -   Failover Clustering will rely on Cloud Witness-based quorum

    -   An Azure Standard Internal Load Balancer will provide a front-end IP address of the Always-On Availability Group listener

To facilitate high-availability on the platform level, each pair of Azure virtual machines (VMs) in each layer will belong to the same managed availability set.

The solution will also include a Windows Server 2019 Azure VM that will serve the role of a jumpbox server.

![Solution architecture to setup SAP NetWeaver on Azure consisting of the authentication layer (with two managed Azure Active Directory domain controllers), shared storage (an Azure Storage account with Azure Files hosting the sapmnt share), SAP ASCS layer (with two servers configured as members of a two-node Windows Failover Clustering cluster), SAP Application instances layer (with two SAP application servers), SAP Database layer (two servers hosting separate instances of SQL Server 2017 configured as nodes of a SQL Server Always-On Availability Group cluster).](images/Hands-onlabunguided-SAPonAzureimages/media/image1.png  "Solution architecture diagram")

## Help references

|    |            |
|----------|:-------------:|
| **Description** | **Links** |
| SAP NetWeaver 3-tier compatible template using a Marketplace image   | <https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-md>   |
| SAP Highly Available ASCS Now Supports File Share UNC Source   | <https://blogs.msdn.microsoft.com/saponsqlserver/2017/08/10/high-available-ascs-for-windows-on-file-share-shared-disk-no-longer-required/>      |
| Configure Always-On Availability Group in Azure VM manually  |  <https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial>  |


## Exercise 1: Deploy the SAP on Azure infrastructure components 

Duration: 150 minutes

### Overview

In this exercise, you will deploy Azure infrastructure prerequisites for implementing SAP NetWeaver on Azure Virtual Machines (VMs). This will include creating such resources as an Azure Virtual Network, Azure VMs and Azure load balancers. In addition, you will implement Azure AD DS, and subsequently, join all Azure VMs to the same Active Directory domain.

### Tasks to Complete:

### Task 1: Implement Azure virtual network

In this task, you will create an Azure virtual network which will host the SAP implementation. You will use the following settings during deployment:

-   Resource group name: **s03-RG**

-   Virtual network name: **s03-RG-vnet**

-   Virtual network IP address space: **10.0.0.0/22**

-   Subnets: 

    -   Subnet name: **dmzSubnet**

    -   Subnet IP address range: **10.0.0.0/24**

    -   Subnet name: **sapSubnet**

    -   Subnet IP address range: **10.0.1.0/24**

    -   Subnet name: **adSubnet**

    -   Subnet IP address range: **10.0.2.0/24**

-   DDoS protection: **Basic**

-   Service endpoints: **Disabled**

-   Firewall: **Disabled**


### Task 2: Deploy SAP NetWeaver application and database tier VMs 

In this task, you will deploy Azure VMs that will be hosting your SAP implementation by using an Azure Resource Manager QuickStart template from GitHub available at <https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-md>. You will use the following settings during deployment:

-   Resource group: **s03-RG**

-   Sap System Id: **s03**

-   Stack Type: **ABAP**

-   OS Type: **Windows Server 2019 Datacenter**

-   Sap System Size: **Demo**

-   System Availability: **HA**

-   vmSizes:

    -   dbserversize: **Standard\_D8s\_v3**

    -   ascsserversize: **Standard\_D4s\_v3**

    -   diserversize: **Standard\_D4s\_v3**

-   Admin Username: **demouser**

-   Admin Password: **demo\@pass123**

-   New Or Existing Subnet: **existing**

-   Subnet Id: **Id of the sapSubnet***


### Task 3: Deploy Windows Server 2019 jumpbox Azure VM 

In this task, you will deploy Azure VMs that will serve as a jumpbox server for your SAP implementation by using the Azure portal. You will use the following settings during deployment:

-   Subscription: the name of the Azure subscription you are using for this lab

-   Resource group: **s03-RG**

-   Virtual machine name: **s03-adm-0**

-   Region: the name of the same Azure region which you used earlier in this exercise

-   Availability options: **No infrastructure redundancy required**

-   Image: **Windows Server 2019 Datacenter**

-   Size: **Standard\_D2s\_v3**

-   Username: **demouser**

-   Password: **demo\@pass123**

-   Public inbound ports: **RDP**

-   Already have a Windows license?: **No**

-   OS disk type: **Standard HDD**

-   Virtual network: **s03-RG-vnet**

-   Subnet name: **dmzSubnet**

-   Public IP: the name of a new public IP address **az1000301-vm0-ip**

-   Network security group: **Basic**

-   Public inbound ports: **Allow selected ports**

-   Select inbound ports: **RDP**

-   Accelerated networking: **Off**

-   Place this virtual machine behind an existing load balancing solution?: **No**

-   Enable basic plan for free: **Off**

-   Boot diagnostics: **Off**

-   OS guest diagnostics: **Off**

-   System assigned managed identity: **Off**

-   Enable auto-shutdown: **Off**


### Task 4: Implement Azure Active Directory Domain Services (Azure AD DS)

In this task, you will implement Azure AD DS domain that will be hosting managed Active Directory domain controller providing authentication and authorization services for your SAP implementation. You will use the following settings during deployment:

-   Directory name: the default value

-   DNS domain name: **contoso.com**

-   Subscription: the name of the Azure subscription you are using for this lab

-   Resource group: the name of a new resource group **s03-ad-RG**

-   Region: the name of the same Azure region which you used earlier in this exercise

-   Virtual network: **s03-RG-vnet**

-   Subnet: use existing **adSubnet**

-   Administrator group: the default setting

-   Synchronization: **All**


### Task 5: Create Azure AD DS user accounts to facilitate implementation of SAP NetWeaver

In this task, you will create the following user accounts with non-expiring password set to **demo\@pass123** in the managed Azure AD DS instance to facilitate your SAP implementation:

-   the account to perform all operating system level configuration tasks, including installation of all SAP components)

    -   Name: **s03-su** 

    -   Member of:

        -   **AAD DC Administrators**

-   the SAP service accounts:

    -   Name: **s03adm** 

    -   Member of:

        -   **AAD DC Administrators**

-   the SAP service accounts:

    -   Name: **SAPService03** 

    -   Member of:

        -   **AAD DC Administrators**

-   the account to configure SQL Server 2017 services on the first SQL Server VM.

    -   Name: **s03-db-0-sqlsvc**

-   the account to configure SQL Server 2017 services on the second SQL Server VM.

    -   Name: **s03-db-1-sqlsvc**


### Task 6: Join Windows Sever 2019 Azure VMs to the Azure AD DS domain

In this task, you will join all of the Windows Server 2019 Azure VMs you deployed earlier in this exercise to the Active Directory domain contoso.com.


### Task 7: Install Remote Server Administration Tools on all Azure VMs

In this task, you will install Remote Server Administration Tools all of the Windows Server 2019 Azure VMs you deployed earlier in this exercise. 


### Task 8: Prepare the Azure AD DS for deployment of SAP NetWeaver application and database tier VMs 

In this task, you will configure the managed Azure AD DS instance and its DNS services to prepare for deployment of your SAP implementation. You will configure the following settings:

-   In the contoso.com Active Directory:

    -   Create an organizational unit in the root of the contoso.com domain named **S03** that will host SAP-specific users and groups.

-   In the contoso.com DNS zone:

    -   Create the following DNS Host (A) records:

        -   **s03-ascs-v0 10.0.1.16** (this IP address must match the IP address that will be assigned to the **frontendascs** front end IP configuration of the ASCS-layer internal load balancer)

        -   **s03-ascs-ers-v0 10.0.1.17** (this IP address must match the IP address that will be assigned to the **frontendaers** front end IP configuration of the ASCS-layer internal load balancer)

        -   **s03-ascs-ers0 10.0.1.18** (this IP address must match the IP address that will be assigned to the **s03-ascs-0**)

        -   **s03-ascs-ers1 10.0.1.19** (this IP address must match the IP address that will be assigned to the **s03-ascs-1**)

        -   **s03-di-v0 10.0.1.38** (this IP address must match the IP address that will be assigned to the SAP Primary Application Server s0-di-0)

        -   **s03-di-v1 10.0.1.39** (this IP address must match the IP address that will be assigned to the SAP Additional Application Server s0-di-1)

    -   Enable non-secure DNS updates.

-   In the contoso.com Active Directory:

    -   Create an organizational unit in the root of the contoso.com domain named **S03** that will host SAP-specific computer, users, and group objects.

    -   Move the following computer objects to the **S03** organizational unit: **s03-ascs-0**, **s03-ascs-1**, **s03-db-0**, **s03-db-1**, **s03-di-0**, and **s03-di-1** 

    -   Create the following SAP-specific user and group objects in the **S03** organizational unit:

        -   Full name: **s03-db-0-sqlsvc**

            -   User UPN logon: [s03-db-0-sqlsvc\@contoso.com](mailto:s03-su@contoso.com)

            -   User SamAccountName: **contoso\\s03-db-0-sqlsvc**

            -   Password: **demo\@pass123** 

            -   Password options:

                -   Password never expires

        -   Full name: **s03-db-1-sqlsvc**

            -   User UPN logon: [s03-db-1-sqlsvc\@contoso.com](mailto:%20s03-db-1-sqlsvc@contoso.com)

            -   User SamAccountName: **contoso\\s03-db-1-sqlsvc**

            -   Password: **demo\@pass123**

            -   Password options:

                -   Password never expires

        -   Full name: **s03adm**

            -   User UPN logon: [s03-db-1-sqlsvc\@contoso.com](mailto:%20s03-db-1-sqlsvc@contoso.com)

            -   User SamAccountName: **contoso\\s03-db-1-sqlsvc**

            -   Password: **demo\@pass123**

            -   Password options:

                -   Password never expires

            -   Member of:

                -   **SAP_S03_GlobalAdmin**

        -   Full name: **SAPServiceS03**

            -   User UPN logon: [SAPServiceS03\@contoso.com](mailto:%SAPServiceS03@contoso.com)

            -   User SamAccountName: **contoso\\SAPServiceS03**

            -   Password: **demo\@pass123**

            -   Password options:

                -   Password never expires

            -   Member of:

                -   **SAP_S03_GlobalAdmin**


### Task 9: Configure IP addresses of Azure VMs and internal load balancers

In this exercise, you will configure private IP addresses assigned to the Azure VMs and front end IP configuration of load balancers deployed in the previous task by using an Azure Resource Manager template. 

>**Note:** At this point, each of the Azure VMs we have deployed in the previous task has a dynamically assigned Private IP address.

Configure the private IP addresses of Azure VMs and frontend IP configuration of load balancers in the following manner:

-   s03-nic-ascs-0: 10.0.1.18

-   s03-nic-ascs-1: 10.0.1.19

-   s03-nic-db-0: 10.0.1.28

-   s03-nic-db-1: 10.0.1.29

-   s03-nic-di-0: 10.0.1.38

-   s03-nic-di-1: 10.0.1.39

-   s03-lb-db:

    -   frontendsql: 10.0.1.25

    -   frontendsqlcl: 10.0.1.26

-   s03-lb-ascs:

    -   frontendascscl: 10.0.1.15

    -   frontendascs: 10.0.1.16

    -   frontendaers: 10.0.1.17


### Task 10: Create and configure the sapmnt share

In this task, you will create and configure the **sapmnt** share by using Azure Files


### Task 11: Configure User Account Control on application and the database VMs

In this task, you will disable User Account Control on the Windows Server 2019 Azure VMs that will be hosting your SAP implementation.

> **Note**: This is a temporary measure. You should re-enable User Account Control once you complete the setup.

### Exit criteria

-   You have implemented Azure AD DS domain named **contoso.com**. 

-   You have deployed the ARM template <https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image> into the same virtual network as the first template.

-   You have implemented an Azure Files share named **sapmnt** with permission settings integrated with Azure AD DS.

-   You have joined all VMs to the contoso.com Active Directory domain.


### Summary

In this exercise, you deployed Azure infrastructure prerequisites for implementing SAP NetWeaver on Azure IaaS virtual machines (VMs). This included creation of such resources as an Azure Virtual Network, Azure VMs, Azure load balancers and Azure Storage accounts. You also used managed PaaS services, including Azure Active Directory Domain Services and Azure Files to provide authentication, authorization, and highly available file sharing capabilities.


## Exercise 2: Configure the SAP NetWeaver ASCS servers

Duration: 90 minutes

### Overview

In this exercise, you will configure the Azure VMs that constitute the SAP ASCS layer. This will include installing Failover Clustering on s03-ascs-0 and s03-ascs-1. You will also run the SAP Software Provisioning Manager to perform highly available installation of the ASCS components on the s03-ascs-0 and s03-ascs-1 VMs. 

### Tasks to complete:

### Task 1: Set up Failover Clustering on the ASCS VMs

In this task, you will start by configuring operating system on s03-ascs-0 and s03-ascs-1. On both VMs, mount the 128 GB data disk as ReFS-formatted U: drive with the disk label **SAP Local FS**. On both VMs, disable Windows firewall for the domain profile. Next, implement a Failover Clustering-based cluster named **s03-ascs-cl0** with the IP address of **10.0.1.11** consisting of **s03-ascs-0** and **s03-ascs-1** Azure VMs. Set up the cluster with a Cloud Witness quorum by using the storage account that was provisioned in the previous exercise when you implemented Azure Files. Finally, grant permissions to the **s03-ascs-cl0** cluster computer account to create computer objects in the **S03** organizational unit.


### Task 2: Install the SAP ASCS components on s03-ascs-0

In this task, you will use SAP Software Provisioning Manager to carry out the distributed installation of ASCS components on the first node of ASCS cluster of MS SQL Server-based SAP NetWeaver 7.5 deployment. Run the installation as the **CONTOSO\\s03-su** account you created in the first exercise. During the installation, address all necessary prerequisites, and specify the following parameters:

-   Destination Drive for Local Instances: **U:**

-   SAP System ID (SAPSID): **S03**

-   DNS Domain Name for SAP Systems: **contoso.com**

-   Windows Domain: **Domain of Current User**

-   Windows Domain for SAP Host Agent: **Local Domain**

-   ASCS Instance Number: **00**

-   ABAP Message Server Port: **3600**

-   Internal ABAP Message Server Port: **3900**

-   Install an SAP Web Dispatcher integrated in the ASCS instance: **Yes**

-   Install a Gateway integrated in the ASCS instance: **Yes**

-   Set SAP Web Dispatcher Encryption Mode: **Never**

-   Set all passwords to **demo\@pass123**

Account for the fact you will be using the virtual names, and configure the following registry entries on both VMs:

-   HKLM\\SYSTEM\\CurrentControlSet\\Control\\LSA

    -   Set DisableLoopbackCheck to 1

-   HKLM:\\SYSTEM\\CurrentControlSet\\Services\\lanmanserver\\parameters

    -   Set DisableStrictNameChecking to 1


### Task 3: Install the SAP ASCS components on s03-ascs-1

In this task, you will use SAP Software Provisioning Manager to carry out the distributed installation of ASCS components on the second node of ASCS cluster of MS SQL Server-based SAP NetWeaver 7.5 deployment. Run the installation as the **CONTOSO\\s03-su** account you created in the first exercise. During the installation, address all necessary prerequisites, and specify the following parameters:

-   Destination Drive for Local Instances: **U:**

-   SAP System ID (SAPSID): **S03**

-   DNS Domain Name for SAP Systems: **contoso.com**

-   Windows Domain: **Domain of Current User**

-   Windows Domain for SAP Host Agent: **Local Domain**

-   ASCS Instance Number: **00**

-   ABAP Message Server Port: **3600**

-   Internal ABAP Message Server Port: **3900**

-   Install an SAP Web Dispatcher integrated in the ASCS instance: **Yes**

-   Install a Gateway integrated in the ASCS instance: **Yes**

-   Set SAP Web Dispatcher Encryption Mode: **Never**

-   Set all passwords to **demo\@pass123**


### Task 4: Configure SAP ASCS cluster components on s03-ascs-0 and s03-ascs-1

In this task, you will configure SAP ASCS cluster components, including Azure Files to host sapmnt share. These extra steps are necessary since, at the time of authoring this content, Software Provisioning Manager does not yet support clustered installation of ASCS components with Azure Files-based sapmnt share.


### Task 5: Install the Enqueue Repliation Server instance on both nodes of the cluster

In this task, you will install the Enqueue Repliation Server (ERS) instance on both nodes of the cluster


### Exit criteria

-   You have installed the HA ASCS component on the s03-ascs-0 and s03-ascs-1 VMs.

    ![Screenshot of the SAP Management Console. Under SAP Systems, S03 is selected.](images/Hands-onlabunguided-SAPonAzureimages/media/image2.png)

### Summary

In this exercise, you configured the Azure VMs that constitute the SAP ASCS layer. This included installing and configuring Failover Clustering on both VMs and running the Software Provisioning Manager on each.


## Exercise 3: Configure SAP NetWeaver database servers

Duration: 120 minutes

### Overview

In this exercise, you will configure the SAP NetWeaver database servers. You will start by installing a separate, stand-alone instance of SQL Server 2017 on each VM. Next, you will run SAP Software Provisioning Manager to install HA DB component on the s03-db-0 VM. Afterwards, you will implement high-availability by setting both SQL Server instances as members of the same Always-On Availability Group. Just as in the previous exercise, you will use Cloud Witness to provide the quorum for the Failover Cluster. You will also copy SQL Server logins from the instance hosted on s03-db-0 to the instance hosted on s03-db-1. Finally, you will update the SAP default profile to point to the Always-On Availability Group listener, rather than to an individual SQL Server 2017 instance.

### Tasks to complete:

### Task 1: Configure storage of the SAP database layer.

In this task, you will configure the Azure VMs for the database layer by attaching an extra Premium 512 GB data disk to each of them (the template-based deployment resulted in one disk of 128 GB in size already attached to every Azure VM within that deployment). Set the host caching on the 128 GB disk to **None** (since this disk will be used to host logs) and on the 512 GB disk to **Read-only** (since this disk will be used to host data files). To configure a Cloud Witness for the database cluster, you will use the same storage account you created in the second exercise when deploying the SAP Azure infrastructure components, so there is no need to create another storage account for this purpose.


### Task 2: Grant elevated user rights to the SQL Server service account

In this task, you will use Group Policy to grant the following user rights to the SQL Server service account (**s03-db-0-sqlsvc** and **s03-db-1-sqlsvc**) on the Window Server 2016 computers hosting their respective SQL Server instances.

-   Perform volume maintenance tasks

-   Lock pages in memory

These options provide potential performance improvements, as documented in <https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-instant-file-initialization?view=sql-server-2016> and <https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/enable-the-lock-pages-in-memory-option-windows?view=sql-server-2016> .

In a production deployment, you should consider using domain-based Group Policy Objects rather than local Group Policy. This approach is used in this lab strictly for simplicity.


### Task 3: Install SQL Server 2017 with the SQL\_Latin1\_General\_CP850\_BIN2 collation

In this task, you will configure operating system on s03-db-0 and s03-db-1, and install SQL Server 2017 on s03-db-0 and s03-db-1 Azure VMs. Run the installation as the **CONTOSO\\s03-su** account you created in the first exercise. Start by disabling the Windows firewall for the domain profile and mounting the 128 GB data disk as ReFS-formatted L: drive with the disk label **Logs Disk** and the 512 GB data disk as ReFS-formatted M: drive with the disk label **Data Disk**. Perform the installation of SQL Server 2017 on both s03-db-0 and s03-db-1 with the following settings:

-   Use Microsoft Update to check for updates (recommended): **disabled**

-   Feature Selection:

    -   **Database Engine Services**

    -   **Client Tools Connectivity**

-   Instance Configuration: **Default instance**

-   Service Accounts:

    -   s03-db-0

        -   SQL Server: **contoso\\s03-db-0-sqlsvc**

        -   SQL Server Agent: **contoso\\s03-db-0-sqlsvc**

    -   s03-db-1

        -   SQL Server: **contoso\\s03-db-1-sqlsvc**

        -   SQL Server Agent: **contoso\\s03-db-1-sqlsvc**

-   Collation: **SQL\_Latin1\_General\_CP850\_BIN2**

-   Windows authentication mode:

    -   SQL Server Administrators

        -   **contoso\\s03-su**

        -   **s03-db-0\\Administrators**

-   Data Directories:

    -   **Data root directory: C:\\Program Files\\Microsoft SQL Server**

    -   **User database directory: M:\\Program Files\\Microsoft SQL Server\\MSSQL13.MSSQLSERVER\\MSSQL\\Data**

    -   **User database log directory: L:\\Program Files\\Microsoft SQL Server\\MSSQL13.MSSQLSERVER\\MSSQL\\Data**

    -   **Backup directory: M:\\Program Files\\Microsoft SQL Server\\MSSQL13.MSSQLSERVER\\MSSQL\\Backup**

-   TempDB:

    -   **Data directories: M:\\Program Files\\Microsoft SQL Server\\MSSQL13.MSSQLSERVER\\MSSQL\\Data (note that you'll need to create this directory)**.

    -   **Log directory: L:\\Program Files\\Microsoft SQL Server\\MSSQL13.MSSQLSERVER\\MSSQL\\Data**

    -   **On both s03-db-0 and s03-db-1, download and install SQL Server Management Studio with the default settings**

    -   **Copy SAP specific logins from s03-db-0 to s03-db-1 with their existing settings, including the default database and the server role**


### Task 4: Install the SAP database layer

In this task, you will install the SAP database instance on s03-db-0 Azure VM by using the SAP Software Provisioning Manager. Run the installation as the **CONTOSO\\s03-su** account you created in the first exercise. During the installation, address all necessary prerequisites, and specify the following parameters:

-   Profile Directory: **\\\\<storage_account_name>\\sapmnt\\S03\\SYS\\profile**

-   SAP System User Domain: **Domain of Current User**

-   OU Path: **S03**

-   Database instance: **S03-DB-0**

-   SAP Host Agent User Domain: **Local Domain**

-   MS SQL Server Data Files: **4 for small systems**

-   MS SQL Server Database Files:

    -   **data: M:\\S03DATA1**

    -   **data: M:\\S03DATA2**

    -   **data: M:\\S03DATA3**

    -   **data: M:\\S03DATA4**

    -   **log: L:\\S03LOG1**

-   SAP System Database Import:

    -   **Number of Parallel Jobs: 3**

-   Set all passwords to **demo\@pass123**


### Task 5: Implement Always-On Availability Group

### Task 5: Implement Always-On Availability Group

In this task, you will implement a Failover Clustering-based cluster named **s03-db-cl0** with the IP address of **10.0.1.17** on s03-db0 and s03-db-1 Azure VMs with Always-On Availability Group hosting the SAP database. Set up the cluster with a Cloud Witness quorum by using the storage account you created in the second exercise. To prepare for setting up Always-On Availability Group for the SAP database you installed in the previous task, grant the **Create Computer Objects** permissions on the **S03** organizational unit in the contoso.com Active Directory to the newly created **contoso\\s03-db-cl0** account. In addition, create a directory structure on s03-db-1 that matches the directory structure on the M: and L: drives of s03-db-0.

Next, from SQL Server Configuration Manager, enable Always-On Availability Groups, and restart the SQL Server (MSSQLSERVER) service on both s03-db-0 and s03-db-01. On s03-db-0, create a new share **C:\\Backup** named **Backup**. On the share level and on the ReFS level, grant **Read/Write** permissions to **CONTOSO\\s03-db-0-sqlsvc** account and **Read** permissions to the **CONTOSO\\s03-db-1-sqlsvc** account.

From SQL Server Management Studio on s03-db-0, use the New Availability Group Wizard to create a new Availability Group with the following settings:

-   Name: **s03-db-ag0**

-   User database for availability group: **S03**

-   Replicas: **S03-DB-0 and S03-DB-1**

    -   Automatic Failover: **Yes**

    -   Synchronous Commit: **Yes**

    -   Readable Secondary: **No**

    -   Initial Data Synchronization: **Full**

    -   A shared network location accessible by all replicas: [**\\\\S03-DB-0\\Backup**](file://S03-DB-0/Backup)

>**Note:** The majority of customers choose to use Transparent Data Encryption (TDE) when deploying SQL Server databases to Azure. If you use TDE, then you cannot use the Always-On Availability Group wizard. Instead, you have to follow the instructions described in <https://blogs.msdn.microsoft.com/saponsqlserver/2017/04/04/more-questions-from-customers-about-sql-server-transparent-data-encryption-tde-azure-key-vault/> . Alternatively, you can enable TDE after establishing the Always On Availability Group, however this is a tedious and slow process. For the purpose of this lab, you will not enable TDE and use the Always-On Availability Group wizard when setting up the Always-On Availability Group.

In the Failover Cluster Manager, create a new Client Access Point with the following settings:

-   Name: **s03-db-agl0**

-   IP address: **10.0.1.16 (static) --** *make sure it matches the IP address assigned to the s03-lb-db load balancer*

Set the dependency of **s03-db-ag0** on **s03-db-agl0**.

Use Windows PowerShell to set the properties of the IP address resource of the newly created Client Access Point as follows:

-   Address: **10.0.1.16**

-   ProbePort: **62500**

-   SubnetMask: **255.255.255.255**

-   Network: **Cluster Network 1**

-   EnableDhcp: **0**

In the SQL Server Management Studio, set the port of the **s03-db-agl0** listener to **1433**.

Bring the Always-On Availability Group clustered role online.


### Task 6: Modify the SAP Default Profile

In this task, you will modify the SAP default profile in order to ensure the application tier communicates with the database tier via the newly created Always-On Availability Group listener. To accomplish this, set the value of the **dbs/mss/server** parameter to the name of the newly created Availability Group listener (**s03-db-agl0**) in the default profile of the S03 SAP system (hosted on the shared disk of the ASCS layer). In order for the change to take effect, restart the VMs that constitute the SAP ASCS layer.


### Exit criteria

-   You have installed the HA DB component on the s03-db-0 VM.

-   You have implemented Always-On Availability Group on s3-db0 and s03-db-1 VMs.

    ![Screenshot of the s03-db-ag0 tab.](images/Hands-onlabunguided-SAPonAzureimages/media/image3.png)

### Summary

In this exercise, you have configured the SAP NetWeaver database servers. You started by first installing a separate, stand-alone instance of SQL Server 2017 on each VM. Next, you ran SAP Software Provisioning Manager to install HA DB component on the s03-db-0 VM. Afterwards, you implemented high-availability by setting both SQL Server instances as members of the same Always-On Availability Group. Just as in the previous exercise, you used Cloud Witness to provide the quorum for the Failover Cluster. You also copied SQL Server logins from the instance hosted on s03-db-0 to the instance hosted on s03-db-1. Finally, you updated the SAP default profile to point to the Always-On Availability Group listener, rather than to an individual SQL Server 2017 instance.


## Exercise 4: Configure SAP NetWeaver Application servers

Duration: 50 minutes

### Overview

In this exercise, you will configure the SAP NetWeaver application servers. You will start by installing Microsoft ODBC Driver for SQL Server on s03-di-0 and s03-di-1 Azure VMs. Next, you will use SAP Software Provisioning Manager to install the Primary Application Server (PAS) instance on s03-di-0 and follow with the installation of an Additional Application Server (AAS) instance on s03-di-1. In addition, you will install SAP Enhanced Monitoring Extension for SAP on Azure VMs included in your SAP deployment. Optionally, you might want to also consider disabling Internet Explorer by following these instructions: <https://support.microsoft.com/en-us/help/4013567/how-to-disable-internet-explorer-on-windows>.

### Tasks to complete:

### Task 1: Configure storage and software prerequisites of the SAP Application layer

In this task, you will configure the Azure VMs in the Application layer by mounting the 128 GB data disk as ReFS-formatted U: drive with the disk label **SAP Local FS**. On both VMs, disable Windows firewall for the domain profile. You will also install on both Microsoft ODBC Driver for SQL Server (available from <https://www.microsoft.com/en-us/download/confirmation.aspx?id=53339> ) to facilitate connectivity to the database tier.


### Task 2: Install the SAP Primary Application Server (PAS) layer

In this task, you will install the SAP PAS of HA deployment of MS SQL Server-based SAP NetWeaver 7.5 HA deployment on s03-di-0 Azure VM by using the SAP Software Provisioning Manager. Run the installation as the **CONTOSO\\s03-su** account you created in the first exercise. During the installation, address all necessary prerequisites, and specify the following parameters:

-   Profile Directory: **\\\\<storage_account_name>.file.core.windows.net\\sapmnt\\S03\\SYS\\profile** (where **<storage_account_name>** designates the name of the storage account you created earlier in this lab)

-   Drive for Local Instances: **U:**

-   Windows Domain: **Domain of current user**

-   Database instance: **S03-DB-AGL0**

-   Existing MS SQL Server Database: **S03**

-   Windows Domain for SAP Host Agent: **Local Domain**

-   PAS Instance Host Name: **s03-di-v0**

-   ABAP Message Server Ports: **3600**

-   Internal ABAP Message Server Ports: **3900**

-   SLD Destination for the SAP System OS Level: **No SLD destination**

-   Message Server Access Control List: **Do not create Message Server Access Control List**

-   Secure Storage Key Generation: **Default Key**

-   Set all passwords to **demo\@pass123**

Account for the fact you are using the virtual names and configure the following registry entries on both VMs:

-   HKLM\\SYSTEM\\CurrentControlSet\\Control\\LSA

    -   Set DisableLoopbackCheck to 1

-   HKLM:\\SYSTEM\\CurrentControlSet\\Services\\lanmanserver\\parameters

    -   Set DisableStrictNameChecking to 1


### Task 3: Install the SAP Additional Application Server (AAS) layer

In this task, you will install the SAP AAS of HA deployment of MS SQL Server-based SAP NetWeaver 7.5 HA deployment on s03-di-1 Azure VM by using the SAP Software Provisioning Manager. Run the installation as the **CONTOSO\\s03-su** account you created in the first exercise. During the installation, address all necessary prerequisites and specify the following parameters:

-   Profile Directory: **\\\\<storage_account_name>.file.core.windows.net\\sapmnt\\S03\\SYS\\profile** (where **<storage_account_name>** designates the name of the storage account you created earlier in this lab)

-   Drive for Local Instances: **U:**

-   Windows Domain: **Domain of current user**

-   Database instance: **S03-DB-AGL0**

-   Existing MS SQL Server Database: **S03**

-   Windows Domain for SAP Host Agent: **Local Domain**

-   AAS Instance Host Name: **s03-di-v1**

-   Message Server Access Control List: **Do not create Message Server Access Control List**

-   Set all passwords to **demo\@pass123**


### Task 4: Install the SAP Enhanced Monitoring Extension for SAP

In this task, you will install SAP Enhanced Monitoring Extension for SAP on Azure VMs included in your SAP deployment.

### Exit criteria

-   You have installed the SAP PAS on s03-di-0 and SAP AAS on s03-di-1.

    ![Screenshot of the SAP Management Console. Under SAP Systems, S03 is selected.](images/Hands-onlabunguided-SAPonAzureimages/media/image4.png)

### Summary

In this exercise, you configured the SAP NetWeaver application servers. You started by installing Microsoft ODBC Driver for SQL Server on the s03-di-0 and s03-di-1 Azure VMs. Next, you used SAP Software Provisioning Manager to install the Primary Application Server (PAS) instance on s03-di-0 and followed with the installation of an Additional Application Server (AAS) instance on s03-di-1.


## Lab summary

In this lab, you stepped through a process of provisioning a highly available, Windows Server 2017-based SAP NetWeaver deployment on Azure, with SAP ABAP stack and SQL Server 2017 as the database tier. To provide high-availability of the ABAP SAP Central Services (ASCS) components, you implemented an instance of a Failover Cluster that leveraged a Storage Spaces Direct (S2D) cluster, hosting highly-available shared storage hosting the sapmnt share. To provide high-availability of the database tier, you implemented an instance of SQL Server Always-On Availability Group. In both cases, you will use a Cloud Witness quorum introduced in Windows Server 2017 Failover Clustering. To provide resiliency of the SAP application server instances, you deployed the Primary Application Server (PAS) and an Additional Application Server (AAS) in the same availability set.


## After the hands-on lab 

Duration: 20 Minutes

### Task 1: Delete resources

1.  Now that the HOL is complete, go ahead and delete all of the Resource Groups created for this HOL. You will no longer need those resources, and it will be beneficial to clean up your Azure Subscription.

You should follow all steps provided *after* attending the hands-on lab.

