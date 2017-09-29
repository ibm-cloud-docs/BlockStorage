---
 
copyright:
  years: 2015, 2017
lastupdated: "2017-09-28"
 
---

{:shortdesc: .shortdesc} 
{:new_window: target="_blank"}

# Working with Replication

Replication uses one of your snapshot schedules to automatically copy snapshots to a destination volume in a remote data center. The copies can be recovered in the remote site in the event of corrupted data or a catastrophic event.

Replicas let you

- Recover from site failures and other disasters quickly by failing over to the destination volume,
- Failover to a specific point in time in the DR copy.

Before you can replicate, you must create a snapshot schedule. When you failover, you’re “flipping the switch” from your storage volume in your primary data center to the destination volume in your remote data center. For example, your primary data center is London and your secondary data center is Amsterdam. In the case of a failure event, you’d fail over to Amsterdam – connecting to the now-primary volume from a compute instance in Amsterdam. After your volume in London has been repaired, a snapshot is taken of the Amsterdam volume in order to fall back to London and the once-again primary volume from a compute instance in London.

 
**Note:** Unless otherwise noted, the steps are the same for both {{site.data.keyword.blockstoragefull}} and File Storage.

## How Do I Determine the Remote Data Center for My Replicated Storage Volume?

{{site.data.keyword.BluSoftlayer_full}}'s worldwide data centers have been paired into primary and remote combinations.
See Table 1 for a complete list of data center availability and replication targets.
Note that some cities, such as Dallas, San Jose, Washington, D.C., and Amsterdam have multiple data centers.


<table cellpadding="1" cellspacing="1">
	<caption>Table 1</caption>
	<tbody>
		<tr>
			<td><strong>US 1</strong></td>
			<td><strong>US 2</strong><sup><img src="/images/numberone.png" alt="1" /></sup></td>
			<td><strong>Latin/South America</strong></td>
			<td><strong>Canada</strong><sup><img src="/images/numberone.png" alt="1" /></sup></td>
			<td><strong>Europe</strong></td>
			<td><strong>Asian Pacific</strong></td>
			<td><strong>Australias</strong><sup><img src="./images/numberone.png" alt="1" /></sup></td>
		</tr>
		<tr>
			<td>DAL01<br />
				DAL05<br />
				DAL06<br />
				HOU02<br />
				SJC01<br />
				WDC01<br />
				<br />
				<br />
				<br />
				<br /><br />
			</td>
			<td><p>SJC03<br />
			       SJC04<br />
			       WDC04<br />
			       WDC06<br />
			       WDC07<br />
				DAL09<br />
				DAL10<br />
				DAL12<br />
				DAL13<br /><br /></p>
			</td>
			<td>MEX01<sup><img src="/images/numberone.png" alt="1" /></sup><br />______<br />
				SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /></td>
			<td>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /></td>
			<td>
				AMS03<sup><img src="/images/numberone.png" alt="1" /></sup><br />
				FRA02<sup><img src="/images/numberone.png" alt="1" /></sup><br />
				LON02<sup><img src="/images/numberone.png" alt="1" /></sup><br />
				LON04<sup><img src="/images/numberone.png" alt="1" /></sup><br />
				LON06<sup><img src="/images/numberone.png" alt="1" /></sup><br />
				OSL01<sup><img src="/images/numberone.png" alt="1" /></sup><br />
				PAR01<sup><img src="/images/numberone.png" alt="1" /></sup><br />
				______<br />
				MIL01<br />
				AMS01<br />
			</td>
			<td>HKG02<sup><img src="/images/numberone.png" alt="1" /></sup><br />
				TOK02<sup><img src="/images/numberone.png" alt="1" /></sup><br />
				______<br />
				SEO01<br />
				SNG01<br />
				CHE01<br />
				<br />
				<br />
				<br />
				<br /><br />
				</td>
			<td>
				SYD01<br />
				SYD04<br />
				MEL01<br />
				<br /><br /><br /><br /><br /><br /><br /><br />
			</td>
		</tr>
		<tr>
			<td colspan="100%">
				<p><sup><img src="/images/numberone.png" alt="1" /></sup>Data centers in these regions or specifically noted within a region have encrypted storage.</p>
				<p>In EU region, PAR01, MIL01 and AMS01 are all replication partners, and can initiate
replication to data centers with encrypted storage as replicas (data centers above the line). <br/>
In Asia Pacific region, SEO01, SNG01 and CHE01 are all replication partners, and can initiate replication data centers with encrypted storage as replicas (data centers above the line). <br/>
In Latin America Region, MEX01 is enabled with encrypted storage. Replication not allowed from MEX01 to SAO01. But, replication can be established from SAO01 to MEX01. <br/>
<strong>Note</strong>: Data centers with encrypted storage <strong>cannot</strong> initiate replication with non-encrypted data centers as replica targets.
</p>
			</td>
		</tr>
	</tbody>
</table>
 

## How Do I Create an Initial Replication?

Replications work off of a snapshot schedule. You must first have snapshot space and a snapshot schedule set up for the source volume before you can replicate. You’ll receive prompts letting you know space needs to be purchased or a schedule set up if you try to set up replication and one or the other is not in place. Replications are managed under Storage, {{site.data.keyword.blockstorageshort}} or File Storage from the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Click on your storage volume.
2. Click on the **Replica** tab and click the **Purchase a replication** link.
Select an existing snapshot schedule that you want your replications to follow. The list will contain all of your active snapshot schedules. 
  **Note:** You can only select one schedule even if you have a mix of hourly, daily, and weekly.  All snapshots captured since the previous replication cycle will be replicated regardless of the schedule that originated.
3. Click the **Location** drop-down arrow and select the data center that will be your DR site.
4. Click **Continue**.
5. Enter in a **Promo Code** if you have one and click **Recalculate**. The other fields in the dialog box will default.
6. Click the **I have read the Master Service Agreement…** check box and click **Place Order**.
 

## How Do I Edit an Existing Replication?

You can edit your replication schedule and change your replication space from either the **Primary** or **Replica** tab under **Storage**, **{{site.data.keyword.blockstorageshort}}** from the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

 

## How Do I Edit a Replication Schedule?

You are actually changing a snapshot schedule because your replication schedule is based on an existing snapshot schedule. To change the replica schedule, for example from Hourly to Weekly, you must cancel the replication schedule and set up a new one.

Changing the schedule can be done on the Primary or Replica tab.

1. Click on the **Actions** drop-down menu from either the **Primary** or **Replica** tab.
2. Select **Edit Snapshot Schedule**.
3. Look in the Snapshot frame under Schedule to determine which schedule you’re using for replication. Make the changes to the schedule that is being used for replication; e.g., if your replication schedule is **Daily**, you can change the time of day replication is to take place.
4. Click **Save**.
 

## How Do I Change Replication space?

You primary snapshot space and your replica space must be the same. If you change the space on the **Primary** or **Replica** tab, it will automatically add space to both your source and destination data centers. Be aware that increasing snapshot space will trigger an immediate replication update.

Click on the **Actions** drop-down menu from either the **Primary** or **Replica** tab.
Select **Add More Snapshot Space**.
Select the storage size from the list and click **Continue**.
Enter in a **Promo Code** if you have one and click **Recalculate**. The other fields in the dialog box will default.
Click the I have read the Master Service Agreement… check box and click the Place Order button.
 

## How Do I See My Replica Volumes in the Volume List?

You can view your replication volumes from the {{site.data.keyword.blockstorageshort}} page under **Storage > {{site.data.keyword.blockstorageshort}}**. The LUN Name will have the primary volume’s name followed by REP. The Type is Endurance(Performance) – Replica, the Target Address is N/A because the replica volume is not mounted at the replica data center, and the Status is Inactive.

 

## How Do I View a Replicated Volume's Details at the Replica Data Center?

You can view the replica volume details on the **Replica** tab under **Storage**, **{{site.data.keyword.blockstorageshort}}**. Another option is to select the replica volume from the **{{site.data.keyword.blockstorageshort}}** page and click on the **Replica** tab.

 

## How Do I Specify Host Authorizations Prior to Failing over to the Secondary Data Center?

Authorized hosts and volumes must be in the same data center. You can’t have a replica volume in London and the host in Amsterdam; both have to be London or both have to be Amsterdam.

1. Click on your source or destination volume from either the **{{site.data.keyword.blockstorageshort}}** page.
2. Click on the **Replica** tab.
3. Scroll down to the **Authorize Hosts** frame and click on the **Authorize Hosts** link on the right side of the screen.
4. Highlight the host that is to be authorized for replications. To select multiple hosts, use the CTRL-key and click on the applicable hosts.
5. Click **Submit**. If you have no hosts, the dialog box will offer you the option of purchasing compute resources in the same data center or you can click **Close**.
 

## How Do I Increase My Snapshot Space in My Replica Data Center When I Increase Space in My Primary Data Center?

Your volume sizes must be the same for your primary and replica storage volumes; one cannot be larger than the other. When you increase your snapshot space for your primary volume, the replica space is automatically increased. Be aware that increasing snapshot space will trigger an immediate replication update. The increase to both volumes will show as line items on your invoice and will be prorated as necessary.

Click [here](snapshots.html) to learn how to increase your snapshot space.

 

## How Do I Initiate a Failover from a Volume to Its Replica?

In the case of a failure event, the **Failover** action lets you initiate a failover to your destination, or target, volume. The target volume will become active, the last successfully replicated snapshot is activated, and the volume is made active for mounting. Any data written to the source volume since the previous replication cycle will be destroyed. Be aware that once a failover is initiated, the **replication relationship flipped**. Your target volume is now your source volume, and your former source volume becomes your target as indicated by the **LUN Name** followed by **REP**.

Failovers are initiated under **Storage**, **{{site.data.keyword.blockstorageshort}}** from the [[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

**Before proceeding with this process, it is recommended to disconnect the volume. Failure to do so, will end with corruption and/or data loss.**

1. Click on your active LUN (“source”).
2. Click on the **Replica** tab and click the **Actions** link in the upper-right corner.
3. Select Failover.
   You will receive a message across the top of the page stating the failover is in progress. Additionally, an icon will appear next to your volume on the **{{site.data.keyword.blockstorageshort}}** indicating that an active transaction is occurring. Hovering over the icon produces a dialog indicating the transaction. The icon will disappear once the transaction is complete. During the failover process, configuration-related actions are read only; you cannot edit any snapshot schedule, change snapshot space, and so on. The event is logged in replication history.
   Another message will let you know when your target volume is live. Your original source volume’s LUN Name will be followed by REP and its Status will be Inactive.
4. Click the **View All** (**{{site.data.keyword.blockstorageshort}}**) Storage link in the top right corner.
5. Click on your active LUN (formerly your target volume). This volume will now have an **Active** status.
6. Mount and attach your storage volume to the host. Click [here](provisioning-block_storage.html) for instructions.
 

## How Do I Initiate a Fallback from a Volume to Its Replica?

Once your original source volume has been repaired, the **Fallback** action lets you initiate a controlled fallback to your original source volume. In a controlled fallback,

- The acting source volume is taken offline;
- A snapshot taken;
- The replication cycle completed;
- The just-taken data snapshot activated;
- And, the source volume made active for mounting.

Be aware that once a fallback is initiated, the **replication relationship is once again flipped**. Your source volume is restored as your source volume, and your target volume once again is the target volume as indicated by the **LUN Name** followed by **REP**.

Fallbacks are initiated under **Storage**, **{{site.data.keyword.blockstorageshort}}** from the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Click on your active Endurance LUN (“target”).
2. Click on the **Replica** tab and click the **Actions** link in the upper-right corner.
3. Select **Fallback**.
   You will receive a message across the top of the page stating the failover is in progress. Additionally, an icon will appear next to your volume on the **{{site.data.keyword.blockstorageshort}}** indicating that an active transaction is occurring. Hovering over the icon produces a dialog indicating the transaction. The icon will disappear once the transaction is complete. During the fallback process, configuration-related actions are read only; you cannot edit any snapshot schedule, change snapshot space, and so on. The event is logged in replication history.
   Another message will let you know when your source volume is live. Your target volume will now have an Inactive status.
4. Click the **View All** (**{{site.data.keyword.blockstorageshort}}**) link in the top right corner.
5. Click on your active Endurance LUN (source). This volume will now have an **Active** status.
6. Mount and attach your storage volume to the host. Click [here](provisioning-block_storage.html) for instructions.
 

## How Do I See My Replication History?

Replication history is viewed on the **Audit Log** via the **Account** tab under **Manage** . Both the primary and replica volumes will display identical replication history, which includes:

- Type for replication (failover or fallback)
- When it was initiated,
- Snapshot used for the replication
- Size of the replication
- When it completed
 

## How Do I Cancel an Existing Replication?

Cancelation can be performed either immediately or on the anniversary date, which causes billing to terminate. Replication can be canceled from either the Primary or Replica tabs.

1. Click on the volume from the **{{site.data.keyword.blockstorageshort}}** page.
2. Click on the **Actions** drop-down on either the **Primary** or **Replica** tab.
3. Select **Cancel Replica**.
4. Select when to cancel – **Immediately** or **Anniversary Date** and click **Continue**.
5. Click the **I acknowledge that due to cancellation, data loss may occur** checkbox and click **Cancel Replica**.
 

## How Do I Cancel Replication When the Primary Volume Is Cancelled?

When a primary volume is canceled, the replication schedule and the volume in the replica data center are deleted. Replicas are canceled from the {{site.data.keyword.blockstorageshort}} page.

 1. Highlight your volume on the **{{site.data.keyword.blockstorageshort}}** page.
 2. Click the **Actions** drop-down menu and select **Cancel {{site.data.keyword.blockstorageshort}}**.
 3. Select when to cancel the volume – Immediately or Anniversary Date and click **Continue**.
 4. Click the **I acknowledge that due to cancellation, data loss may occur** checkbox and click **Cancel**.
