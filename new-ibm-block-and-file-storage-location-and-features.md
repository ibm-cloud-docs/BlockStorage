---

copyright:
  years: 2014, 2017
lastupdated: "2017-08-31"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# New IBM Block and File Storage Location and Features

IBM Bluemix Infrastructure is introducing a new version of IBM Block and File storage!  The new storage is available in select data centers, and is backed by flash storage at higher IOPS levels with disk level encryption for data-at-rest.  All storage provisioned in the select data centers will automatically be provisioned with the new version of Block and File storage.

**Note:** The NFS mount point for new volumes has changed. See **New Mount Point for encrypted File Storage Volumes** below for details.

The new Block and File storage is currently available in following regions/data centers with additional data center availability coming soon!
<table style="width:100%;">
	<caption>Data Center Availability</caption>
	<tbody>
		<tr>
			<td><strong>US 2</strong></td>
			<td><strong>EU</strong><img src="./images/numberone.png" alt="1" /></td>
			<td><strong>Australia</strong></td>
			<td><strong>Canada</strong></td>
			<td><strong>Latin America</strong></td>
			<td><strong>Asia Pacific</strong><img src="/images/numberone.png" alt="1" /></td>
		</tr>
		<tr>
			<td>
				<p>SJC03<br />
				   SJC04<br />
					WDC04<br />
					WDC06<br />
					WDC07<br />
					DAL09<br />
					DAL10<br />
					DAL12<br />
					DAL13</p>
			</td>
			<td>
				<p>LON02<br />
				LON04<br />
				LON06<br />
					FRA02<br />
					AMS03<br />
					OSLO1<br />PAR01<br /><br /><br /></p>
			</td>
			<td>
				<p>SYD01<br />
				SYD04<br />
				MEL01<br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>TOR01<br />
					MON01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>MEX01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
						<td>
				<p>TOK02<br />
					HKG02<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			</tr>
	</tbody>
</table>
 

<sup>![1](/images/numberone.png)</sup> Support for encrypted storage in Paris, Milan and Seoul will be coming soon. In the meantime, replication is only allowed to the above EU data centers from Paris, Milan and Seoul, but not TO Paris, Milan or Seoul. 

The new storage has the following features and capabilities:

- [Provider Managed encryption for data-at-rest](block-file-storage-encryption-rest.html). All Block and File storage will automatically be provisioned as encrypted at no additional charge.
- 10 IOPS per GB tier option.  A new tier has been added to the Endurance type Block and File storage to support the most demanding workloads.
- All flash-backed storage.  Block and File storage provisioned with either Endurance or Performance at 2 IOPS per GB or higher with backed by all-flash storage.
- Snapshot and Replication support with Block and File storage provisioned with Performance.
- Up to 48,000 IOPS for Block and File storage provisioned with Performance.
- Create a new clone of your data with the [Block Storage Volume Duplication feature](how-to-create-duplicate-volume.html).  

## New Mount Point for Encrypted File Storage Volumes

All encrypted File storage volumes provisioned in these data centers have a different mount point than non-encrypted volumes.  To ensure you are using the correct mount point for both your encrypted and non-encrypted file storage volumes you can view the mount point information in the Volume Details page in the UI as well as access the correct mountpoint via an API call:  `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Check back here to see when additional data centers have been upgraded and for newly available features and capabilities that are being added for Block and File storage.
