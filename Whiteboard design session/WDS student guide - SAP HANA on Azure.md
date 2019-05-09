![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
SAP HANA on Azure
</div>

<div class="MCWHeader2">
 Whiteboard design session student guide
</div>

<div class="MCWHeader3">
March 2019
</div>


Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [SAP HANA on Azure whiteboard design session student guide](#sap-hana-on-azure-whiteboard-design-session-student-guide)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Step 1: Review the customer case study](#step-1-review-the-customer-case-study)
        - [Customer situation](#customer-situation)
        - [Customer needs](#customer-needs)
        - [Customer objections](#customer-objections)
        - [Infographic for common scenarios](#infographic-for-common-scenarios)
    - [Step 2: Design a proof of concept solution](#step-2-design-a-proof-of-concept-solution)
    - [Step 3: Present the solution](#step-3-present-the-solution)
    - [Wrap-up](#wrap-up)
    - [Additional references](#additional-references)

<!-- /TOC -->

# SAP HANA on Azure whiteboard design session student guide

## Abstract and learning objectives 

In this Whiteboard Design Session, you will look at what is involved in deploying SAP HANA on Azure with the goals of designing for high availability, disaster recovery as well as supportability.

At the end of this whiteboard design session you will be able to better design and deploy SAP HANA on Azure.

## Step 1: Review the customer case study 

**Outcome**

Analyze your customer's needs.

Timeframe: 15 minutes

Directions: With all participants in the session, the facilitator/SME presents an overview of the customer case study along with technical tips.

1.  Meet your table participants and trainer.

2.  Read all of the directions for steps 1-3 in the student guide.

3.  As a table team, review the following customer case study.

### Customer situation

Contoso Group is a global pharmaceutical company with its headquarters based in Boston, US.

Contoso has been using SAP ERP and BW on HANA for its Finance/Logistics/Analytics systems on the HP-UX/Oracle platform. 

Contoso Leadership and Planning Groups wants to significantly reduce server and storage hardware in their own datacenters to minimize IT related costs. The leadership asked Contoso IT to look into the possibility of migrating its SAP HANA environment to cloud. Contoso is also considering transitioning to SAP HANA 2.0 with a longer term objective of deploying S/4 HANA and BW/4 HANA.

Contoso has already a number of their non-SAP systems migrated to Azure. Contoso IT decided to leverage its knowledge of the Microsoft cloud platform and existing ExpressRoute connectivity and host its SAP landscape in Azure. The intention is to migrate the BW system first (go live in March CY20), and migrate ECC in Q4 of CY20. The multi-stage approach is supposed to minimize potential migration risks.

Considering that Contoso management team often uses BW to support their management decisions, the systems should be highly available, and their performance must be predictable and consistent. In addition, the management team wants to leverage disaster recovery capabilities offered by Azure in order to ensure resiliency of the migrated environment in case the primary region hosting the new deployment becomes unavailable.

As Andrew Cross, CIO of Contoso Group emphasized this point by stating "Our operational dependencies on SAP applications force us to seek reasonably priced availability and disaster recovery capabilities for our production SAP HANA deployments."

Contoso also wants to dramatically simplify the process of provisioning Azure resources, as well as installation and configuration of SAP HANA software components. Contoso IT has been relying on Ansible to manage its on-premises environment and, with the cloud migration plans, it started evaluating the use of Terraform to automate on-premises and cloud-based deployments. 

Before migrating the production environment, Contoso wants to test its new deployment approach by provisioning training, development, test, and UAT environments in Azure.

### Customer needs 

-   Highly responsive systems with low network latency

-   In-memory database performance

-   High availability & disaster recovery

-   Enterprise data protection & security

-   Safe migration with downtime minimized

-   Access from HANA-based applications

-   Automated provisioning

-   Minimized cost

1.  Design scope:

    -   BW migration to HANA in Azure VMs

        -   Go-live date: March 2020

        -   Current BW (ABAP Unicode) on-premises with HP-UX/Oracle and application layer on Linux

        -   Customer requests flexible VM solution within Cloud to accommodate the BW workloads

            -   Use 1-year Reserved VM Instance option for Production VMs

    -   ERP is kept on-premises (with HP-UX/Oracle) until December 2020

        -   Data is transferred from ERP (on-premises) to BW (in Cloud) every hour

    -   (Option) Need to start to prepare for ERP migration to Cloud

1.  Target environment:

    -   Sizing

        -   Production (3-tier) with latest OS/DB fully certified and supported by SAP

            -   HANA sizing memory requirement 1.2 TB, estimate 1.9 TB in 3 years

            -   Throughput DB files at least 400MB/s \[/hana/data\]

            -   Throughput DB Log files at least 250MB/s \[/hana/log\]

            -   BW application servers: 15K SAPS

        -   Certification is NOT required for non-Prod

            -   QA (2-tier) HANA database server: 800 GB

            -   Dev, Test (both 2-tier) HANA database server(s): 256 GB

    -   Uptime -- Prod: 24x7, 744 hours/month, QA - 50 hours/month, DEV/Test - 200 hours/month

1.  High availability and disaster recovery:

    -   Availability

        -   Both HA only and HA with DR options need to be proposed.

        -   In regard to HA, in case of server/storage issues, auto failover to complete within a few minutes, in case of a disaster recovery within 1 day.

    -   Backup

        -   Long term backup -- use backup storage in Cloud

        -   Data loss not allowed

        -   HANA DB log backup taken every 30 minutes

        -   DB log backup to be kept for 1 day (DB restore to be fast)

        -   HANA DB full backup every night

        -   Daily HANA DB full backup to be retained for 1 month

        -   Monthly HANA DB full backup for 1 year, annual for 3 years

1.  End user access:

    -   User locations -- 300 from US, 50 LATAM, 50 Europe, 30 Asia - all intranet

    -   Currently ExpressRoute is set up to Azure East US 2

    -   Response time needs to be minimized

### Customer objections 

1.  ECC remains on-premises until Dec CY20. How can we maintain integrations between ECC and BW?

1.  How much does Azure cost? Give us two options (HA only and HA with DR).

1.  Do I have to pay for virtual machines when they are stopped?

1.  Can I automate the shutdown of virtual machines at specific times of day?

### Infographic for common scenarios

![Common solutions for the case study are represented by icons of virtual machines, virtual networks, VPN gateway, hybrid connectivity, load balancers, and storage, as well as SAP on Azure, with more information available via https://aka.ms/sapazure](images/CommonScenarios.png "Common Scenarios")

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

*High-level solution architecture:*

1.  What should be the Azure region(s) where the solution will be deployed?

1.  Should the customer use a 2-tier or 3-tier architecture for its SAP deployment?

1.  How would you ensure that the high-availability and disaster recovery requirements are satisfied?

*Network design:*

1.  What should be the hybrid connectivity option?

1.  What should be the Azure virtual network design in order to maximize security?

*SAP deployment architecture and methodology:*

1.  What will be the configuration of the configuration of the application and database components of your solution?

1.  What Azure VM sizes do you intend to use?

1.  What other Azure resources will be part of your solution?

1.  What should be the SAP deployment methodology?

*Solution cost:*

1.  What is the estimated cost of your solution with HA?

1.  What is the estimated cost of your solution with HA/DR?

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

##  Additional references

|    |            |
|----------|:-------------:|
| **Description** | **Links** |
| High availability of SAP HANA on Azure VMs on SUSE Linux Enterprise Server | <https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-high-availability/> |
| High availability for SAP NetWeaver on Azure VMs on SUSE Linux Enterprise Server for SAP applications | <https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/high-availability-guide-suse> |
| Automated SAP Deployments in Azure Cloud | <https://github.com/Azure/sap-hana> |
| 1928533 - SAP Applications on Azure: Supported Products and Azure VM types | <https://launchpad.support.sap.com/#/notes/1928533> |
