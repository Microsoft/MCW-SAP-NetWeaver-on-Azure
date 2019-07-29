![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
SAP NetWeaver on Azure
</div>

<div class="MCWHeader2">
Before the hands-on lab setup guide
</div>

<div class="MCWHeader3">
June 2019
</div>


Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2019 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [SAP NetWeaver on Azure before the hands-on lab setup guide](#\sap-netweaver-on-azure-before-the-hands-on-lab-setup-guide)
    - [Requirements](#requirements)
    - [Before the hands-on lab](#before-the-hands-on-lab)
        - [Task 1: Review the relevant Microsoft documentation](#task-1-review-the-relevant-microsoft-documentation)

<!-- /TOC -->

# SAP NetWeaver on Azure before the hands-on lab setup guide

## Requirements

-   A Microsoft Azure subscription with at least 34 vCPU available in the Azure region where the Azure VMs deployed in this lab will reside.

    -   ASCS VMs -- 2 x D4s\_v3: 2 x 4 vCPUs = 8 vCPUs

    -   DB VMs -- 2 x D8s\_v3: 2 x 8 vCPUs = 16 vCPUs

    -   APP VMs -- 2 x D4s\_v3: 2 x 4 vCPUs = 8 vCPUs

    -   Jumpbox VMs -- 1 x D2s\_v3: 1 x 2 vCPUs = 2 vCPUs

   > **Note**: The lab VM does not require locally installed software. Az PowerShell tasks are performed by using Cloud Shell in the Azure portal. If you decide to run these tasks from the lab computer, install Az PowerShell (module version 2.1.0 or newer).

-   Access to SAP Software Provisioning Manager (SWPM) version SPS24 or newer (requires an SAP Online Service System account)

-   Access to NTCLUST.SAR including the current SAP cluster resource DLL (requires an SAP Online Service System account)

-   Internet access from the lab VM

## Before the hands-on lab

Timeframe: 30 minutes

Review the relevant Microsoft documentation.

### Task 1: Review the relevant Microsoft documentation 

1.  Review to the documentation regarding highly available architecture and scenarios of SAP NetWeaver on Azure at <https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios>.

1.  Review to the documentation regarding Az PowerShell at <https://docs.microsoft.com/en-us/powershell/azure/new-azureps-module-az?view=azps-2.1.0>.

### Task 2: Validate the owner role membership in the Azure subscription

1.  Login to the Azure portal at <http://portal.azure.com>, click on **All services** and, in the list of services, click **Subscriptions**.

1.  On the **Subscriptions** blade, click the name of the subscription you intend to use for this lab.

1.  On the subscription blade, click **Access control (IAM)**.

1.  Review the list of user accounts and verify that your user account has the Owner role assigned to it.

### Task 3: Validate the Global Administrator role in the Azure AD tenant associated with the Azure subscription

1.  In the Azure portal at <http://portal.azure.com>, in the hub menu, click **Azure Active Directory**.

1.  On the Azure Active Directory blade, click **Users**.

1.  On the **Users - All Users** blade, click the entry representing your user account. 

1.  On the blade displaying properties of your user account, click **Directory role**.

1.  Verify that the blade displaying the directory roles of your user account includes the **Global administrator** entry. 

### Task 4: Validate sufficient number of vCPU cores

1.  In the Azure portal at <http://portal.azure.com>.

1.  In the Azure Portal, start a PowerShell session in Cloud Shell. 

    > **Note**: If this is the first time you are launching Cloud Shell in the current Azure subscription, you will be asked to create an Azure file share to persist Cloud Shell files. If so, accept the defaults, which will result in creation of a storage account in an automatically generated resource group.

1.  In the Azure portal, in the **Cloud Shell**, at the PowerShell prompt, run the following: where `<Azure_region>` designates the target Azure region that you intend to use for this lab (e.g. `eastus`):

    ```
    Get-AzVMUsage -Location eastus | Where-Object {$_.Name.Value -eq 'StandardDSv3Family'}
    ``` 

    > **Note**: To identify the names of Azure regions, in the **Cloud Shell**, at the Bash prompt, run `(Get-AzLocation).Location`.
   
1.  Review the output of the command executed in the previous step and ensure that you have at least 18 available vCPUs in the **Standard DSv3 Family** in the target Azure region.

1.  If the number of vCPUs is not sufficient, in the Azure portal, navigate back to the subscription blade, and click **Usage + quotas**. 

1.  On the subscription's **Usage + quotas** blade, click **Request Increase**.

1.  On the **Basic** blade, specify the following and click **Next: Solutions >>**:

    -   Issue type: **Service and subscription limits (quotas)**

    -   Subscription: the name of the Azure subscription you will be using in this lab.

    -   Quota type: **Compute/VM (cores/vCPUs) subscription limit increases**

    -   Support plan: the name of the support plan associated with the target subscription.

1.  On the **Details** blade, click the **Provide details** link.

1.  On the **Quota details** blade, specify the following and click **Save and continue**:

    -   Deployment model: **Resource Manager**

    -   Location: the target Azure region you intend to use in this lab.

    -   SKU family: **DSv2 Series** and **ESv3 Series** (for each SKU series, provide the new limit)

1.  Back on the **Details** blade, specify the following and click **Next: Review + create >>**:

    -   Severity: **C - Minimal impact**

    -   Preferred contact method: choose your preferred option and provide your contact details
    
1.  On the **Review + create** blade, click **Create**.

   > **Note**: Quota increase requests are typically completed during the same business day.

### Task 5: Download SAP binaries

1.  From the lab computer, start a Web browser, and navigate to **SAP Software Downloads** at https://launchpad.support.sap.com/#/softwarecenter/ (requires an SAP Online Service System account).

1.  From the **SAP Software Download**, download the following software packages to the lab computer:

    -   Installation for SAP IGS integrated in SAP Kernel ; OS: Windows on x64 6 (igsexe_9-80003241.sar)
    
    -   Kernel Part I (753) ; OS: Windows on x64 64bit ; DB: Database independent (SAPEXE_401-80002612.SAR)

    -   Kernel Part II (753) ; OS: Windows on x64 64bit ; DB: MS SQL SERVER (SAPEXEDB_401-80002611.SAR)

    -   SAP HOST AGENT 7.21 SP42 ; OS: Windows on x64 64bit (SAPHOSTAGENT42_42-20009417.SAR)

    -   SAP IGS Fonts and Textures (igshelper_17-10010245.sar)

    -   Support Package SAPCAR 7.21 Windows on x64 64bit (SAPCAR_1211-80000938.EXE)

    -   Support Package SOFTWARE PROVISIONING MGR 1.0 Windows on x64 64bit (SWPM10SP26_1-20009707.SAR)

    -   SAP Kernel 7.45 Windows Server on x64 64bit - NW 7.5 (51050826_10.ZIP)

    -   NW 7.5 Installation Export (51050829_3.ZIP)
    
    > **Note**: The packages names (listed above in parenthesis) might be superseded by newer versions. If so, ensure to adjust accordingly all references to the names of these packages in this task. To find appropriate packages, you can take advantage of the search functionality of the **SAP Software Downloads** portal.
    
You should follow all steps provided *before* performing the Hands-on lab.

