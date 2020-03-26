![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
SAP NetWeaver on Azure
</div>

<div class="MCWHeader2">
Before the hands-on lab setup guide
</div>

<div class="MCWHeader3">
March 2020
</div>


Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2019 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [SAP NetWeaver on Azure before the hands-on lab setup guide](#sap-netweaver-on-azure-before-the-hands-on-lab-setup-guide)
  - [Requirements](#requirements)
  - [Before the hands-on lab](#before-the-hands-on-lab)
    - [Task 1: Review the relevant Microsoft documentation](#task-1-review-the-relevant-microsoft-documentation)
    - [Task 2: Validate the owner role assignment in the Azure subscription](#task-2-validate-the-owner-role-assignment-in-the-azure-subscription)
    - [Task 3: Validate the Global Administrator role in the Azure AD tenant associated with the Azure subscription](#task-3-validate-the-global-administrator-role-in-the-azure-ad-tenant-associated-with-the-azure-subscription)
    - [Task 4: Validate sufficient number of vCPU cores](#task-4-validate-sufficient-number-of-vcpu-cores)
    - [Task 5: Identify SAP binaries](#task-5-identify-sap-binaries)

<!-- /TOC -->

# SAP NetWeaver on Azure before the hands-on lab setup guide

## Requirements

-   A Microsoft Azure subscription with at least 40 vCPU available in the Azure region where the Azure VMs deployed in this lab will reside.

    -   AD VMs -- 2 x D4s\_v3: 2 x 4 vCPUs = 8 vCPUs

    -   ASCS VMs -- 2 x D4s\_v3: 2 x 4 vCPUs = 8 vCPUs

    -   DB VMs -- 2 x D8s\_v3: 2 x 8 vCPUs = 16 vCPUs

    -   APP VMs -- 2 x D4s\_v3: 2 x 4 vCPUs = 8 vCPUs

   > **Note**: The Azure region you select must support Active Directory authentication for Azure File shares. For the listing of regions supporting this functionality, refer to <https://docs.microsoft.com/en-us/azure/storage/files/storage-files-identity-auth-active-directory-enable>.

   > **Note**: The lab VM does not require locally installed software. Az PowerShell tasks are run in Cloud Shell in the Azure portal and directly from Azure VMs deployed throughout the course of the lab. If you decide to run some of these tasks from the lab computer, install Az PowerShell (module version 2.1.0 or newer).

-   Access to SAP binaries described in task 5 of this document

-   Internet access from the lab VM

## Before the hands-on lab

Timeframe: 30 minutes

Review the relevant Microsoft documentation.

### Task 1: Review the relevant Microsoft documentation 

1.  Review to the documentation regarding highly available architecture and scenarios of SAP NetWeaver on Azure at <https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios>.

1.  Review to the documentation regarding Az PowerShell at <https://docs.microsoft.com/en-us/powershell/azure/new-azureps-module-az?view=azps-3.6.1>.

### Task 2: Validate the owner role assignment in the Azure subscription

1.  Login to the Azure portal at <http://portal.azure.com>, use the **Search resources, services, and docs** text box to search for **Subscriptions** and, in the list of results, select **Subscriptions**.

1.  On the **Subscriptions** blade, select the name of the subscription you intend to use for this lab.

1.  On the subscription blade, select **Access control (IAM)**.

1.  Switch to the **Role assignments** tab, review the list of role assignments, and verify that your user account has the Owner role assigned to it.

### Task 3: Validate the Global Administrator role in the Azure AD tenant associated with the Azure subscription

1.  In the Azure portal at <http://portal.azure.com>, use the **Search resources, services, and docs** text box to search for **Azure Active Directory** and, in the list of results, select  **Azure Active Directory**.

1.  On the Azure Active Directory blade, select **Users**.

1.  On the **Users | All Users** blade, select the entry representing your user account. 

1.  On the blade displaying properties of your user account, select **Assigned roles**.

1.  Verify that the list of the directory roles assigned to your user account includes the **Global administrator** entry. 

### Task 4: Validate sufficient number of vCPU cores

1.  In the Azure portal at <http://portal.azure.com>.

1.  In the Azure Portal, start a PowerShell session in Cloud Shell. 

    > **Note**: If this is the first time you are launching Cloud Shell in the current Azure subscription, you will be asked to create an Azure file share to persist content of your Cloud Shell home directory. If so, accept the defaults, which will result in creation of a storage account in an automatically generated resource group.

1.  In the Azure portal, in the **Cloud Shell**, at the PowerShell prompt, run the following: where `<Azure_region>` designates the target Azure region that you intend to use for this lab (e.g. `eastus`):

    ```powershell
    Get-AzVMUsage -Location <Azure_region> | Where-Object {$_.Name.Value -eq 'StandardDSv3Family'}
    ``` 

    > **Note**: To identify the names of Azure regions, in the **Cloud Shell**, at the PowerShell prompt, run `(Get-AzLocation).Location`.
   
1.  Review the output of the command executed in the previous step and ensure that you have at least 40 available vCPUs in the **Standard DSv3 Family** in the target Azure region.

1.  If the number of vCPUs is not sufficient, in the Azure portal, navigate back to the subscription blade, and select **Usage + quotas**. 

1.  On the subscription's **Usage + quotas** blade, select **Request Increase**.

1.  On the **Basic** blade, specify the following and select **Next: Solutions >**:

    -   Issue type: **Service and subscription limits (quotas)**

    -   Subscription: the name of the Azure subscription you will be using in this lab.

    -   Quota type: **Compute-VM (cores-vCPUs) subscription limit increases**

    -   Support plan: the name of the support plan associated with the target subscription.

1.  On the **Details** blade, select the **Provide details** link.

1.  On the **Quota details** blade, specify the following and select **Save and continue**:

    -   Deployment model: **Resource Manager**

    -   Location: the target Azure region you intend to use in this lab.

    -   Types: **Standard**

    -   Standard: **DSv3 Series** 
    
    -   New vCPU Limit: the new limit

1.  Back on the **Details** blade, specify the following and select **Next: Review + create >**:

    -   Severity: **C - Minimal impact**

    -   Preferred contact method: choose your preferred option and provide your contact details
    
1.  On the **Review + create** blade, select **Create**.

   > **Note**: It might take a few days for quota increase requests to be completed.

### Task 5: Identify SAP binaries

1.  From the lab computer, start a Web browser, navigate to **SAP Software Downloads** at https://launchpad.support.sap.com/#/softwarecenter/ and log on using your SAP Online Service System account.

1.  In the **SAP Software Download** portal, verify that you have access to the following software packages:

    -   Installation for SAP IGS integrated in SAP Kernel ; OS: Windows on x64 6 ; Support Package SAP IGS 7.53 Windows on x64 64bit(igsexe_9-80003241.sar)
    
    -   Kernel Part I (753) ; OS: Windows on x64 64bit ; DB: Database independent; Support Package SAP KERNEL 7.53 64-BIT UNICODE Windows on x64 64bit #Database independent (SAPEXE_401-80002612.SAR)

    -   Kernel Part II (753) ; OS: Windows on x64 64bit ; DB: MS SQL SERVER; Support Package SAP KERNEL 7.53 64-BIT UNICODE Windows on x64 64bit MS SQL SERVER (SAPEXEDB_401-80002611.SAR)

    -   SAP HOST AGENT 7.21 SP45 ; OS: Windows on x64 64bit (SAPHOSTAGENT45_45-20009417.SAR)

    -   SAP IGS Fonts and Textures; Support Package SAP IGS HELPER # OS independent (igshelper_17-10010245.sar)

    -   Support Package SAPCAR 7.21 Windows on x64 64bit (SAPCAR_1311-80000938.EXE)

    -   SOFTWARE PROVISIONING MGR SWPM 1.0 SP28 for NW higher than 7.0x (SWPM10SP28_3-20009707.SAR)

    -   Support Package SOFTWARE PROVISIONING MGR SP28 SWPM10_IM_WINDOWS_X86_64 (51054279_15.ZIP)

    -   SAP Kernel 7.45 Windows Server on x64 64bit - NW 7.5; SAP DC Kernel 7.45 Windows Server on x64 64bit (51051055_10.ZIP)

    -   NW 7.5 Installation Export (51050829_3.ZIP)
    
    > **Note**: The packages listed above might be superseded by newer versions. If so, ensure to adjust accordingly all references to the names of these packages in this task. To find appropriate packages, you can take advantage of the search functionality of the **SAP Software Downloads** portal. Use the first part of each package (up to the first hyphen) as the search criterion and, in the search results, identify the type that matches the intended platform (either Linux x86_64 or Windows 64-bit).
  
You should follow all steps provided *before* performing the Hands-on lab.