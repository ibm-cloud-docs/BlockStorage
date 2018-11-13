---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# New Locations and Features of {{site.data.keyword.blockstorageshort}}

{{site.data.keyword.BluSoftlayer_full}} is introducing a new version of {{site.data.keyword.blockstoragefull}}!

The new storage is available in select data centers, and is backed by flash storage at higher IOPS levels with disk level encryption for data-at-rest. All storage that is provisioned in the upgraded data centers is automatically created with the new version.

The NFS mount point for new volumes differs from the mount point of non-encrypted volumes. For more information, see the [New mount point for encrypted {{site.data.keyword.blockstorageshort}} volumes](#new-mount-point-for-encrypted-storage-volumes) section.
{:important}

## New locations

The new {{site.data.keyword.blockstorageshort}} is available in the following regions and data centers.
<table role="presentation">
  <tr>
    <td><strong>US 2</strong></td>
    <td><strong>EU</strong></td>
    <td><strong>Australia</strong></td>
    <td><strong>Canada</strong></td>
    <td><strong>Latin America</strong></td>
    <td><strong>Asia Pacific</strong></td>
  </tr>
  <tr>
    <td>DAL09<br />
	DAL10<br />
	DAL12<br />
	DAL13<br />
	SJC03<br />
        SJC04<br />
	WDC04<br />
	WDC06<br />
	WDC07<br />
	<br /><br /><br />
    </td>
    <td>AMS01<br />
        AMS03<br />
	FRA02<br />
	FRA04<br />
	FRA05<br />
	LON02<br />
	LON04<br />
	LON05<br />
	LON06<br />
	MIL01<br />
	OSLO1<br />
	PAR01<br />
    </td>
    <td>MEL01<br />
        SYD01<br />
        SYD04<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MON01<br />
        TOR01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MEX01<br />
        SAO01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>CHE01<br />
        HKG02<br />
	SEO01<br />
	SNG01<br />
        TOK02<br />
	TOK04<br />
	TOK05<br />
	<br /><br /><br /><br /><br />
    </td>
  </tr>
</table>

*Table 1 shows our Data Center Availability. Each region has its own column. Some cities, such as Dallas, San Jose, Washington DC, Amsterdam, Frankfurt, London, and Sydney have multiple data centers.*

## New features and capabilities

- **[Provider-managed encryption for data-at-rest](block-file-storage-encryption-rest.html)**.
  All {{site.data.keyword.blockstorageshort}} is automatically provisioned as encrypted at no extra charge.
- **10 IOPS per GB tier option**.
  A new tier is available with the Endurance type {{site.data.keyword.blockstorageshort}} to support the most demanding workloads.
- **All flash-backed storage.**
  All {{site.data.keyword.blockstorageshort}} that is provisioned with either Endurance or Performance type at 2 IOPS per GB or higher are backed by all-flash storage.
- **Snapshot and Replication** support with {{site.data.keyword.blockstorageshort}}
- **Hourly Billing** option is available for storage that is planned to be used for less than a full month.
- **Up to 48,000 IOPS** for {{site.data.keyword.blockstorageshort}} that is provisioned with Performance.
- **IOPS rates are adjustable** to improve performance during seasonal load changes. Read more about this feature [here](adjustable-iops.html).
- Create a clone of your data with the **[{{site.data.keyword.blockstorageshort}} Volume Duplication feature](how-to-create-duplicate-volume.html)**.
- **Storage is expandable** in GB increments up to 12 TB, without the need to create a duplicate or manually moving data to a larger volume. Read more about this feature [here](expandable_block_storage.html).

## New mount point for encrypted storage volumes

All enhanced storage volumes that are provisioned in these data centers have a different mount point than non-encrypted volumes. Check the mount point information on the **Volume Details** page in the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} to ensure that you're using the correct mount point. You can also get the correct mount point information through an API call: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Check back here to see when more data centers are upgraded and for new features and capabilities that are being added for {{site.data.keyword.blockstorageshort}}.
{:tip}
