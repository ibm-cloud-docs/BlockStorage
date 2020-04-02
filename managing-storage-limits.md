---

copyright:
  years: 2014, 2020
lastupdated: "2020-04-02"

keywords: Block Storage, limit increase, global quota, quota increase

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}

# Managing storage limits
{: #managingstoragelimits}
{: help}
{: support}

By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} and {{site.data.keyword.filestorage_short}} volumes globally.

## Confirming your current limit and provisioning count.

If you're unsure how many volumes you have, you can confirm the numbers by using multiple methods.

### SLCLI

You can list the number of your volumes by using the [volume-limits](https://softlayer-python.readthedocs.io/en/latest/cli/block/#block-volume-limits){: external} command in `slcli` (version 5.8.5 or higher).

```
# slcli block volume-limits
```
Example output:
```
[{'datacenterName': 'global', 'maximumAvailableCount': 250, 'provisioned Count':117}]
:............:.......................:..................:
: Datacenter : maximumAvailableCount : ProvisionedCount :
:............:.......................:..................:
:   global   :           250         :         117      :
:............:.......................:..................:
```

### IBMCLOUD CLI

The volume-limits command is also available in the `sl` plugin for ibmcloud cli (v1.0 or higher).

```
# ibmcloud sl block volume-limits
Datacenter   MaximumAvailableCount   ProvisionedCount
global       300                     99
```

### REST API CALL

To directly get this information from the API, use the following method: [SoftLayer_Network_Storage/getVolumeCountLimits](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Storage/getVolumeCountLimits/){: external}.

```
curl -u $SL_USER:$SL_APIKEY 'https://api.softlayer.com/rest/v3.1/SoftLayer_Network_Storage/getVolumeCountLimits.json'
[{"datacenterName":"global","maximumAvailableCount":300,"provisionedCount":99}]
```

The API call shows the combined number of {{site.data.keyword.blockstorageshort}} and {{site.data.keyword.filestorage_short}}.
{:tip}

## Requesting limit increase

You can request a limit increase by submitting a support case in the [console](https://{DomainName}/unifiedsupport/cases/add){: external}. When the request is approved, you get a volume limit that is set for a specific data center.
{:shortdesc}

To request a limit increase, open a case and direct it to your sales representative.

In the case, provide the following information:

- **Ticket Subject**: Request to Increase Data Center Volume Count Storage Limit

- **What is the use case for the additional volumes request?** <br />
*For example, your answer might be something similar to a new VMware datastore, a new development and testing (dev/test) environment, an SQL database, or logging.*

- **How many extra Block volumes are needed by type, size, IOPS, and location?** <br />
*For example, your answer might be something similar to "25x Endurance 2 TB @ 4 IOPS in DAL09" or "25x Performance 4 TB @ 2 IOPS in WDC04".*

- **How many extra File volumes are needed by type, size, IOPS, and location?** <br />
*For example, your answer might be something similar to "25x Performance 20 GB @ 10 IOPS in DAL09" or "50x Endurance 2 TB @ 0.25 IOPS in SJC03".*

- **Provide an estimate of when you expect or plan to provision all of the requested volume increase.** <br />
*For example, your answer might be something similar to "90 days".*

- **Provide a 90-day forecast of expected average capacity usage of these volumes.** <br />
*For example, your answer might be something similar to "expect 25 percent to be used in 30 days, 50 percent to be used in 60 days and 75 percent to be used in 90 days".*

Respond to all questions and statements in your request. They are required for processing and approval.
{:important}

You're going to be notified of the update to your limits through the case process.
