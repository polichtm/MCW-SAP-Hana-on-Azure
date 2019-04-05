
![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
SAP HANA on Azure
</div>

<div class="MCWHeader2">
Before the hands-on lab setup guide
</div>

<div class="MCWHeader3">
March 2019
</div>


Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2019 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [SAP HANA on Azure before the hands-on-lab setup guide](#sap-hana-on-azure-before-the-hands-on-lab-setup-guide)
    - [Requirements](#requirements)
    - [Before the hands-on lab](#before-the-hands-on-lab)
        - [Task 1: Validate the owner role membership in the target Azure subscription](#task-1-validate-the-owner-role-membership-in-the-target-azure-subscription)
        - [Task 2: Validate availability of the SUSE Linux Enterprise Server image](#task-2-validate-availability-of-the-suse-linux-enterprise-server-image)
        - [Task 3: Validate sufficient number of vCPU cores](#task-3-validate-sufficient-number-of-vcpu-cores)

<!-- /TOC -->

# SAP HANA on Azure before the hands-on-lab setup guide

## Requirements

-   A Microsoft Azure subscription

-   A lab computer running Windows 10 or Windows Server 2016 with:

    -   Access to Microsoft Azure

    -   Access to the SAP HANA installation media (requires an SAP Online Service System account)

   > **Note**: The lab does not require locally installed software. Azure CLI and Terraform tasks are performed by using Cloud Shell in the Azure portal.

## Before the hands-on lab

Duration: 15 minutes

To complete this lab, you must verify your account has sufficient permissions to the Azure subscription that you intend to use to deploy Azure VMs. You also need to identify the availability of the SUSE Linux Enterprise Server image that you will use to deploy Azure VMs.

### Task 1: Validate the owner role membership in the Azure subscription

1.  Login to <http://portal.azure.com>, click on **All services** and, in the list of services, click **Subscriptions**.

1.  On the **Subscriptions** blade, click the name of the subscription you intend to use for this lab.

1.  On the subscription blade, click **Access control (IAM)**.

1.  Review the list of user accounts, and verify that your user account has the Owner role assigned to it.

### Task 2: Validate availability of the SUSE Linux Enterprise Server image 

1.  In the Azure portal at <http://portal.azure.com>, click the **Cloud Shell** icon.

    ![In the Azure Portal, the Cloud Shell icon is selected.](images/Setup/image2.png "Azure Portal")

1.  If prompted, in the **Welcome to Azure Cloud Shell** window, click **Bash (Linux)**.

1.  If prompted, in the **You have no storage mounted** window, click **Create storage**.

1.  Once the storage account gets provisioned, at the Bash prompt, run the following: where `<Azure_region>` designates the target Azure region that you intend to use for this lab (e.g. `eastus`), and verify the output includes an existing image:

    ```
    az vm image list --location <Azure_region> --publisher SUSE --offer SLES-SAP --sku 12-SP3 --all --output table
    ``` 
     
### Task 3: Validate sufficient number of vCPU cores

1.  In the Azure portal at <http://portal.azure.com>, in the **Cloud Shell**, at the Bash prompt, run the following: where `<Azure_region>` designates the target Azure region that you intend to use for this lab (e.g. `eastus`):

    ```
    az vm list-usage --location <Azure_region> --query "[?localName == 'Standard DSv2 Family vCPUs' || localName == 'Standard ESv3 Family vCPUs'].{VMFamily:localName, currentValue:currentValue, Limit:limit}" --output table
    ``` 

   > **Note**: To identify the names of Azure regions, in the **Cloud Shell**, at the Bash prompt, run `az account list-locations --query '[].name' --output tsv`
   
1.  Review the output of the command executed in the previous step and ensure that you have at least 6 available vCPUs in the **Standard DSv2 Family** VM family and at least 24 available vCPUs in the **Standard ESv3 Family** in the target Azure region.

1.  If the number of vCPUs is not sufficient, navigate back to the subscription blade, and click **Usage + quotas**. 

1.  On the subscription's **Usage + quotas** blade, click **Request Increase**.

1.  On the **Basic** blade, specify the following and click **Next**:

    -   Issue type: **Service and subscription limits (quotas)**

    -   Subscription: the name of the Azure subscription you will be using in this lab

    -   Quota type: **Compute/VM (cores/vCPUs) subscription limit increases**

    -   Support plan: the name of the support plan associated with the target subscription

1.  On the **Quota details** blade, specify the following and click **Save and continue**:

    -   Deployment model: **Resource Manager**

    -   Location: the target Azure region you intend to use in this lab

    -   SKU family: **DSv2 Series** and **ESv3 Series**

1.  On the **Problem** blade, specify the following and click **Next**:

    -   Severity: **C - Minimal impact**

1.  On the **Contact Information** blade, provide your contact details and click **Create**

   > **Note**: Quota increase requests are typically completed during the same business day.


You should follow all steps provided *before* performing the Hands-on lab.
