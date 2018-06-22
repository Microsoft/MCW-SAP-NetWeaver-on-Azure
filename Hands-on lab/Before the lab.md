
## Requirements

-   A Microsoft Azure subscription with at least 36 vCPU available in the Azure region where the lab Azure VMs will reside.

    -   DC VMs -- 2 x DS2\_v2: 2 x 2 vCPUs = 4 vCPUs

    -   S2D VMs -- 2 x DS3\_v2: 2 x 4 vCPUs = 8 vCPUs

    -   ASCS VMs -- 2 x DS2\_v2: 2 x 2 vCPUs = 4 vCPUs

    -   DB VMs -- 2 x DS4\_v2: 2 x 8 vCPUs = 16 vCPUs

    -   APP VMs -- 2 x DS2\_v2: 2 x 2 vCPUs = 4 vCPUs

-   A lab computer running Windows 7 or newer and configured with:

    -   Azure PowerShell (AzureRM module version 5.7.0 or newer)

-   Access to SAP Software Provisioning Manager (SWPM) version SPS21 or newer (requires an SAP Online Service System account)

-   Access to NTCLUST.SAR including the current SAP cluster resource DLL (requires an SAP Online Service System account)

-   Access to Internet from the lab VMs


## Before the hands-on lab

Timeframe: 30 minutes

Review the relevant Microsoft documentation.

### Task 1: Review the relevant Microsoft documentation 

1.  Review to the documentation regarding highly available deployments of SAP NetWeaver on Azure at <https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios>

2.  Review to the documentation regarding Azure PowerShell at <https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps?view=azurermps-5.7.0>

You should follow all steps provided *before* attending the hands-on lab.
