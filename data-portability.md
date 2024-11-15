---

copyright:
  years: 2024
lastupdated: "2024-11-15"

keywords: data portability, DORA, vpc, Block storage for VPC, File storage for VPC

subcollection: BlockStorage

---

{{site.data.keyword.attribute-definition-list}}

# Understanding data portability for Classic Block and File Storage services
{: #data-portability}

Data portability involves a set of tools and procedures that enable you to export the digital artifacts that are needed to implement similar workload and data processing on different service providers or on-premises software. It includes procedures for copying and storing the service customer content, including the related configuration that is used by the service to store and process the data, in your location.
{: shortdesc}

## Responsibilities
{: #data-portability-responsibilities}

{{site.data.keyword.cloud_notm}} services provide interfaces and instructions to guide you through the process of copying and storing service customer content, including the related configuration, in your selected location.

You're responsible for the use of the exported data and configuration for data portability to other infrastructures, which includes:

- The planning and execution for setting up alternative infrastructure on different cloud providers or on-premises software that provide similar capabilities to the {{site.data.keyword.IBM_notm}} services.
- The planning and execution for the porting of the required application code on the alternative infrastructure, including the adaptation of your application code, deployment automation, and so on.
- The conversion of the exported data and configuration to the format that's required by the alternative infrastructure and adapted applications.

To find out more about responsibility ownership for using {{site.data.keyword.cloud}} products between {{site.data.keyword.IBM_notm}} and the customer, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} products](/docs/overview?topic=overview-shared-responsibilities).

## Data export procedures
{: #data-portability-procedures}

When you create a strategy for migrating {{site.data.keyword.blockstorageshort}} or {{site.data.keyword.filestorage_short}} data to another target storage platform, whether that is on premises or another cloud provider, many factors must be considered to facilitate a successful outcome. 

Each migration scenario is different. Capture the requirements and any special considerations for your use case. Each migration scenario includes factors such as the type of data that are being migrated, the data availability, data compatibility, target storage environment requirements, and so on.
{: important}

1. Identify the type of data that you intend to migrate. You must ensure compatibility of the storage services that are available between the source and target storage environments to help ensure seamless workload transition upon completion of the migration process.
   -  If the targeted storage is block volume service data, make sure that the target cloud provider has equivalent block storage services available to map your existing data correctly.
   -  If target storage is file service data, confirm that the target cloud provider has equivalent NFS file services available to map your existing data correctly.

2. Identify the performance and capacity requirements for the data. Make sure that the target platform has compatible performance, and capacity profiles available to reduce any impact to workload behavior after the transition. It is recommended to conduct performance testing upfront within the new target environment to evaluate any workload performance impacts before you migrate your data.

3. Consider the data availability and downtime requirements of your services
   - Identify any specific requirements around data availability and outage windows.
   - Consider the strategies that you can incorporate to help ensure data consistency and accessibility across your workloads during the migration process.
   - Think about how long the migration can take. If you have a large data estate, you must develop a detailed migration plan that defines the timeline for migration execution. You must evaluate the size of the data sets to be transferred and consider expected data transfer speeds between the source and target environments.

4. Select the right tool. When you consider specific tools for data migration, it’s important to pick the right solution for the job.
   - Block storage – Select a block volume migration tool that maintains the native block format. Make sure that the data is transferred as raw blocks or chunks, maintaining the original file system structure and volume layout upon ingest into the target storage system. The specific tools that you choose depend on your unique migration requirements. Some important considerations to keep in mind during the evaluation process include the capacities that the provider has for data integrity, data monitoring, and data security during the migration process. In addition, it is important to evaluate any requirements that are dictated by the target storage vendor you are migrating data to. It is recommended that you review the target storage environment documentation to understand if they recommend processes or tools to simplify the migration.
   - File storage – Select an NFS migration tool that maintains file system hierarchy, directory structure, metadata, and permissions when the data is transferred to the new storage environment. When you evaluate NFS migration tools, make sure that the tool you use maintains data consistency and integrity during the migration. Make sure that the tool supports secure data transfer capabilities and has adequate monitoring facilities. Different tools each have their own strengths, so it's important to evaluate your specific requirements and choose the one that best fits your needs.

5. Develop a comprehensive test plan to validate the migration tools' capabilities against your requirements before you start your data migration plan. Make sure that the testing evaluates the following:
   - The data consistency and data integrity of the data during the migration process
   - The ability to monitor, detect, and react to issues during the migration process
   - The network bandwidth and network reliability between the source and target environments
   - The ability to verify that all data is intact on the target system after the migration is complete

Other Considerations:
   - It is recommended that snapshots are taken within {{site.data.keyword.cloud_notm}} before execution of any data migration activities. It is important to have a backup of all data to help ensure data retention during the process.
   - Update any applications or systems that rely on the migrated data to point to the new storage location.

## Data ownership
{: #data-ownership}

All exported data is classified as customer content. Apply the full customer ownership and licensing rights, as stated in the [IBM Cloud Service Agreement](https://www.ibm.com/support/customer/csol/terms/?id=Z126-6304_WS){: external}.
