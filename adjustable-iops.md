---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: Block storage, new feature, adjusting IOPS, modify IOPS, increase IOPS, decrease IOPS,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Adjusting IOPS
{: #adjustingIOPS}

With this feature, {{site.data.keyword.blockstoragefull}} storage users can adjust the IOPS of their existing {{site.data.keyword.blockstorageshort}} immediately. They don't need to create a duplicate or manually copy data to new storage. Users don't experience any kind of outage or lack of access to the storage while the adjustment is taking place.

Billing for the storage is updated to add the pro-rated difference of the new price to the current billing cycle. The full new amount is billed in the next billing cycle.


## Advantages of Adjustable IOPS

- Cost management – Some clients might need high IOPS only during peak usage times. For example, a large retail store has peak usage during the holidays and might need higher IOPS rate on their storage then. However, they don't need higher IOPS in the middle of the summer. This feature allows them to manage their costs and pay for higher IOPS when they need it.

## Limitations
{: #limitsofIOPSadjustment}

This feature is available in [select data centers](/docs/infrastructure/BlockStorage?topic=BlockStorage-news) only.

Clients can't switch between Endurance and Performance when they adjust their IOPS. However, they can specify a new IOPS tier or IOPS level for their storage based on the following criteria and restrictions:

- If original volume is Endurance 0.25 tier, IOPS tier can’t be updated.
- If original volume is Performance with less than or equal to 0.30 IOPS/GB, options available include only the size and IOPS combinations that result in less than or equal to 0.30 IOPS/GB.
- If original volume is Performance with more than 0.30 IOPS/GB, options available include only the size and IOPS combinations that result in more than 0.30 IOPS/GB.

## Effect of IOPS adjustment on replication

If the volume has replication in place, the replica is automatically updated to match the IOPS selection of the primary.

## Adjusting the IOPS on your Storage
{: #adjustingsteps}

1. Go to your list of {{site.data.keyword.blockstorageshort}}. From the {{site.data.keyword.cloud}} console, click on the **menu** icon, then click **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Select the LUN from the list and click **Actions** > **Modify LUN**
3. Under **Storage IOPS Options**, make a new selection:
    - For Endurance (Tiered IOPS), select an IOPS Tier greater than 0.25 IOPS/GB of your storage. You can increase the IOPS tier at any time. However, decreasing is available only once a month.
    - For Performance (Allocated IOPS), specify new IOPS option for your storage by entering a value in the range 100-48,000 IOPS.

    Be sure to look at any specific boundaries that are required by size in the order form.
    {:tip}
4. Review your selection and the new pricing.
5. Click the **I have read the Master Service Agreement...** check box and click **Place Order**.
6. Your new storage allocation is available in a few minutes.


Alternatively, you can adjust the IOPS through the SLCLI.
```
# slcli block volume-modify --help
Usage: slcli block volume-modify [OPTIONS] VOLUME_ID

Options:
  -c, --new-size INTEGER        New Size of block volume in GB. ***If no size
                                is given, the original size of volume is
                                used.***
                                Potential Sizes: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [the original size of the volume]
  -i, --new-iops INTEGER        Performance Storage IOPS, between 100 and 6000
                                in multiples of 100 [only for performance
                                volumes] ***If no IOPS value is specified, the
                                original IOPS value of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is less than 0.3, new IOPS/GB
                                must also be less than 0.3. If original
                                IOPS/GB for the volume is greater than or
                                equal to 0.3, new IOPS/GB for the volume must
                                also be greater than or equal to 0.3.]
  -t, --new-tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) [only for
                                endurance volumes] ***If no tier is specified,
                                the original tier of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is 0.25, new IOPS/GB for the
                                volume must also be 0.25. If original IOPS/GB
                                for the volume is greater than 0.25, new
                                IOPS/GB for the volume must also be greater
                                than 0.25.]
  -h, --help                    Show this message and exit.
```
{:codeblock}
