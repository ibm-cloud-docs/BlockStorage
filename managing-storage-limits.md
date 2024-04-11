---

copyright:
  years: 2014, 2024
lastupdated: "2024-02-14"

keywords: Block Storage for Classic, limit increase, global quota, quota increase

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Managing storage limits
{: #managingstoragelimits}
{: help}
{: support}

By default, you can provision a combined total of 700 {{site.data.keyword.blockstorageshort}} and {{site.data.keyword.filestorage_short}} volumes globally. By following this process, you can increase the number of volumes that you can provision.

For more information about increasing your storage volume capacity beyond 12 TB, see [expanding {{site.data.keyword.blockstorageshort}} capacity](/docs/BlockStorage?topic=BlockStorage-expandingcapacity#increasecapacityover12TB).

## Confirming your current limit and provisioning count from the CLI
{: #confirmblocklimitscli}
{: cli}

Before you begin, decide on the CLI client that you want to use.

* You can either install the [IBM Cloud CLI](/docs/cli){: external} and install the SL plug-in with `ibmcloud plugin install sl`. For more information, see [Extending IBM Cloud CLI with plug-ins](/docs/cli?topic=cli-plug-ins).
* Or, you can install the [SLCLI](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.

### IBMCLOUD CLI
{: #ibmcloudcli1}

Use the `ibmcloud sl block volume-limits` command to display what is available and what is provisioned.

```sh
ibmcloud sl block volume-limits
Datacenter   MaximumAvailableCount   ProvisionedCount
global       700                    99
```
{: codeblock}

For more information about all of the parameters that are available for this command, see [ibmcloud sl block volume-limits](/docs/cli?topic=cli-sl-block-storage#sl_block_volume_limits){: external}.

### SLCLI
{: #slcli1}

You can list the number of your volumes by using the [`volume-limits`](https://softlayer-python.readthedocs.io/en/latest/cli/block/#block-volume-limits){: external} command in SLCLI (version 5.8.5 or higher).
```sh
slcli block volume-limits
```
{: pre}

The output looks similar to the following example.
```sh
[{'datacenterName': 'global', 'maximumAvailableCount': 700, 'provisioned Count':117}]
:............:.......................:..................:
: Datacenter : maximumAvailableCount : ProvisionedCount :
:............:.......................:..................:
:   global   :           700        :         117      :
:............:.......................:..................:
```
{: screen}

## Confirming your current limit and provisioning count with the API
{: #confirmblocklimitsAPI}
{: api}

### REST API
{: #restAPIBlock}

To directly get this information from the API, use the following method: [`SoftLayer_Network_Storage/getVolumeCountLimits`](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Storage/getVolumeCountLimits/){: external}.

```curl
curl -u $SL_USER:$SL_APIKEY 'https://api.softlayer.com/rest/v3.1/SoftLayer_Network_Storage/getVolumeCountLimits.json'

SoftLayer_Container_Network_Storage_DataCenterLimits_VolumeCountLimitContainer[{"datacenterName":"global","maximumAvailableCount":700,"provisionedCount":99}]
```

The API call shows the combined number of {{site.data.keyword.blockstorageshort}} and {{site.data.keyword.filestorage_short}}.
{: tip}

## Requesting volume limit increase
{: #increaseblocklimits}

You can request a provisioning limit increase by submitting a support case in the [console](/unifiedsupport/cases/add){: external}. When the request is approved, you get a volume limit that is set for a specific data center.
{: shortdesc}

In the case, provide the following information:

- **Ticket Subject**: 
   > Request to Increase Data Center Volume Count Storage Limit

- **What is the use case for the additional volumes request?**
   >*For example, your answer might be something similar to a "new VMware&reg; datastore", "a new development and testing (`dev/test`) environment", "an SQL database", or "logging".*

- **How many extra Block volumes are needed by type, size, IOPS, and location?**
   >*For example, "25x Endurance 2 TB @ 4 IOPS in DAL09" or "25x Performance 4 TB @ 2 IOPS in WDC04."*

- **How many extra File volumes are needed by type, size, IOPS, and location?**
   >*For example, "25x Performance 20 GB @ 10 IOPS in DAL09" or "50x Endurance 2 TB @ 0.25 IOPS in SJC03."*

- **Provide an estimate of when you expect or plan to provision all of the requested volume increase.**
   >*For example, "90 days."*

- **Provide a 90-day forecast of expected average capacity usage of these volumes.**
   >*"I expect 25 percent to be used in 30 days, 50 percent to be used in 60 days and 75 percent to be used in 90 days."*

Respond to all questions and statements in your request. They are required for processing and approval.
{: important}

You're going to be notified of the update to your limits through the case process.
