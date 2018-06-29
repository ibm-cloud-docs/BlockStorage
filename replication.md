---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}

# Replicating Data

Replication uses one of your snapshot schedules to automatically copy snapshots to a destination volume in a remote data center. The copies can be recovered in the remote site if a catastrophic event occurs or your data becomes corrupted.

With replicas you can

- Recover from site failures and other disasters quickly by failing over to the destination volume,
- Fail over to a specific point-in-time in the DR copy.

Before you can replicate, you must create a snapshot schedule. When you fail over, you’re "flipping the switch" from your storage volume in your primary data center to the destination volume in your remote data center. For example, your primary data center is London and your secondary data center is Amsterdam. If a failure event occurs, you’d fail over to Amsterdam – connecting to the now-primary volume from a compute instance in Amsterdam. After your volume in London is repaired, a snapshot is taken of the Amsterdam volume to fail back to London and the once-again primary volume from a compute instance in London.


## Determining the remote data center for my replicated storage volume

{{site.data.keyword.BluSoftlayer_full}}'s data centers are paired into primary and remote combinations worldwide.
See Table 1 for the complete list of data center availability and replication targets.

<table>
	<caption style="text-align: left;"><p>Table 1 - This table shows the complete list of data centers with enhanced capabilities in each region. Every region is a separate column. Some cities, such as Dallas, San Jose, Washington DC, Amsterdam, Frankfurt, London, and Sydney have multiple data centers.</p>
		<p>&#42; Data centers in US 1 region do NOT have enhanced storage. Hosts in data centers with enhanced storage capabilities <strong>can't</strong> start replication with replica targets in US 1 data centers.</p>
</caption>
	<thead>
		<tr>
			<th>US 1 &#42;</th>
			<th>US 2</th>
			<th>Latin America</th>
			<th>Canada</th>
			<th>Europe</th>
			<th>Asia-Pacific</th>
			<th>Australia</strong></t>
		</tr>
	</thead>
	<tbody>
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
				<br />
				<br />
			</td>
			<td>SJC03<br />
			       SJC04<br />
			       WDC04<br />
			       WDC06<br />
			       WDC07<br />
			       DAL09<br />
				DAL10<br />
				DAL12<br />
				DAL13<br />
				<br /><br />
			</td>
			<td>MEX01<br />
				SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>
				AMS01<br />
				AMS03<br />
				FRA02<br />
				FRA04<br />
				FRA05<br />
				LON02<br />
				LON04<br />
				LON06<br />
				OSL01<br />
				PAR01<br />
				MIL01<br />
			</td>
			<td>HKG02<br />
				TOK02<br />
				SNG01<br />
				SEO01<br />
                                CHE01<br />
				<br />
				<br />
				<br />
				<br />
				<br />
				<br />
			</td>
			<td>
				SYD01<br />
				SYD04<br />
				MEL01<br />
				<br /><br /><br /><br /><br /><br /><br /><br />
			</td>
		</tr>
	</tbody>
</table>

## Creating the initial replica

Replications work based on a snapshot schedule. You must first have snapshot space and a snapshot schedule for the source volume before you can replicate. If you try to set up replication and one or the other isn't in place, you are going to be prompted to purchase more space or set up a schedule. Replications are managed under **Storage**, **{{site.data.keyword.blockstorageshort}}** in the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Click your storage volume.
2. Click **Replica** and click **Purchase a replication**.
Select the existing snapshot schedule that you want your replication to follow. The list contains all of your active snapshot schedules. <br />
  **Note:** You can select only one schedule even if you have a mix of hourly, daily, and weekly. All snapshots that were captured since the previous replication cycle, are replicated regardless of the schedule that originated them.<br />
  **Note:** If you don't have Snapshots set up, you are prompted to do so before you can order replication. See [Working with Snapshots](snapshots.html) for more details.
3. Click **Location**, and select the data center that is your DR site.
4. Click **Continue**.
5. Enter in a **Promo Code** if you have one, and click **Recalculate**. The other fields in the window are completed by default.
6. Click the **I have read the Master Service Agreement…** check box, and click **Place Order**.


## Editing an existing replication

You can edit your replication schedule and change your replication space from either the **Primary** or **Replica** tab under **Storage**, **{{site.data.keyword.blockstorageshort}}** in the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.



## Editing the replication schedule

The replication schedule is based on an existing snapshot schedule. To change the replica schedule, for example from Hourly to Weekly, you must cancel the replication schedule and set up a new one.

Changing the schedule can be done on the Primary or Replica tab.

1. Click **Actions** on either the **Primary** or **Replica** tab.
2. Select **Edit Snapshot Schedule**.
3. Look in the **Snapshot** frame under **Schedule** to determine which schedule you're using for replication. Change the schedule that you want. For example, if your replication schedule is **Daily**, you can change the time of day when replication is to take place.
4. Click **Save**.


## Changing the Replication space

Your primary snapshot space and your replica space must be the same. If you change the space on the **Primary** or **Replica** tab, automatically adds space to both your source and destination data centers. Increasing snapshot space triggers an immediate replication update also.

1. Click **Actions** on either the **Primary** or **Replica** tab.
2. Select **Add More Snapshot Space**.
3. Select the storage size from the list and click **Continue**.
4. Enter in a **Promo Code** if you have one and click **Recalculate**. The other fields in the dialog box are completed by default.
5. Click the **I have read the Master Service Agreement…** check box and click **Place Order**.


## Viewing the replica volumes in the Volume List

You can view your replication volumes on the {{site.data.keyword.blockstorageshort}} page under **Storage > {{site.data.keyword.blockstorageshort}}**. The **LUN Name** shows the primary volume's name followed by REP. The **Type** is Endurance or Performance – Replica. The **Target Address** is N/A because the replica volume isn't mounted at the replica data center, and the **Status** shows Inactive.



## Viewing a replicated volume's details at the replica data center

You can view the replica volume details on the **Replica** tab under **Storage**, **{{site.data.keyword.blockstorageshort}}**. Another option is to select the replica volume from the **{{site.data.keyword.blockstorageshort}}** page and click the **Replica** tab.



## Specifying host authorizations before the server fails over to the secondary data center

Authorized hosts and volumes must be in the same data center. You can't have a replica volume in London and the host in Amsterdam. Both must be in London or both must be in Amsterdam.

1. Click your source or destination volume from the **{{site.data.keyword.blockstorageshort}}** page.
2. Click **Replica**.
3. Scroll down to the **Authorize Hosts** frame and click **Authorize Hosts** on the right.
4. Highlight the host that is to be authorized for replications. To select multiple hosts, use the CTRL-key and click the applicable hosts.
5. Click **Submit**. If you have no hosts, you are prompted to purchase compute resources in the same data center.


## Increasing the Snapshot space in the replica data center when Snapshot space is increased in the primary data center

Your volume sizes must be the same for your primary and replica storage volumes. One can't be larger than the other. When you increase your snapshot space for your primary volume, the replica space is automatically increased. Be aware that increasing snapshot space triggers an immediate replication update. The increase to both volumes shows as line items on your invoice and is prorated as necessary.

Click [here](snapshots.html) to learn how to increase your snapshot space.



## Starting a failover from a volume to its replica

If a failure event occurs, you can start a **failover** to your destination, or target, volume. The target volume becomes active. The last successfully replicated snapshot is activated, and the volume is made available for mounting. Any data that was written to the source volume since the previous replication cycle is lost. Be aware that when a failover is started, the replication relationship is flipped. Your target volume becomes your source volume, and your former source volume becomes your target as indicated by the **LUN Name** followed by **REP**.

Failovers are started under **Storage**, **{{site.data.keyword.blockstorageshort}}** in the [[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

**Before you proceed with these steps, disconnect the volume. Failure to do so, results in corruption and data loss.**

1. Click your active LUN (“source”).
2. Click **Replica** and click the **Actions** link in the upper-right corner.
3. Select Failover.
   Expect a message across the top of the page that states that the failover is in progress. Additionally, an icon appears next to your volume on the **{{site.data.keyword.blockstorageshort}}** that indicates that an active transaction is occurring. Hovering over the icon produces a window that shows the transaction. The icon disappears when the transaction is complete. During the failover process, configuration-related actions are read-only. You can't edit any snapshot schedule or change snapshot space. The event is logged in replication history.
   When your target volume is live you get another message. Your original source volume's LUN Name updates to end in "REP" and its Status becomes Inactive.
4. Click **View All ({{site.data.keyword.blockstorageshort}})**.
5. Click your active LUN (formerly your target volume). This volume now has an **Active** status.
6. Mount and attach your storage volume to the host. Click [here](provisioning-block_storage.html) for instructions.


## Starting a failback from a volume to its replica

When your original source volume is repaired, you can start a controlled Failback to your original source volume. In a controlled Failback,

- The acting source volume is taken offline,
- A snapshot is taken
- The replication cycle is completed,
- The just-taken data snapshot is activated,
- And the source volume becomes active for mounting.

Be aware that when a Failback is started, the replication relationship is flipped again. Your source volume is restored as your source volume, and your target volume is the target volume again as indicated by the **LUN Name** followed by **REP**.

Failbacks are started under **Storage**, **{{site.data.keyword.blockstorageshort}}** in the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Click your active Endurance LUN ("target").
2. Click **Replica** and click **Actions** in the upper-right corner.
3. Select **Failback**.
   Expect a message across the top of the page that shows the failover is in progress. Additionally, an icon appears next to your volume on the **{{site.data.keyword.blockstorageshort}}** indicating that an active transaction is occurring. Hovering over the icon produces a window that shows the transaction. The icon disappears when the transaction is complete. During the Failback process, configuration-related actions are read-only. You can't edit any snapshot schedule or change snapshot space. The event is logged in replication history.
   Another message informs you when your source volume is live. Your target volume has an Inactive status.
4. In the upper right corner, click **View All {{site.data.keyword.blockstorageshort}}** link.
5. Click your active Endurance LUN (source). This volume has an **Active** status now.
6. Mount and attach your storage volume to the host. Click [here](provisioning-block_storage.html) for instructions.


## Viewing replication history

Replication history can be viewed in the **Audit Log** on the **Account** tab under **Manage**. Both the primary and replica volumes display identical replication history. The history includes:

- Type for replication (failover or failback)
- When it was started
- Snapshot that was used for the replication
- Size of the replication
- When it completed


## Cancelling an existing replication

You can cancel replication either immediately or on the anniversary date, which causes billing to end. Replication can be canceled from either the **Primary** or the **Replica** tabs.

1. Click the volume on the **{{site.data.keyword.blockstorageshort}}** page.
2. Click **Actions** on either the **Primary** or **Replica** tab.
3. Select **Cancel Replica**.
4. Select when to cancel. – **Immediately** or **Anniversary Date** and click **Continue**.
5. Click the **I acknowledge that due to cancellation, data loss may occur** check box and click **Cancel Replica**.


## Cancelling replication when the primary volume is canceled

When a primary volume is canceled, the replication schedule and the volume in the replica data center are deleted. Replicas are canceled from the {{site.data.keyword.blockstorageshort}} page.

 1. Highlight your volume on the **{{site.data.keyword.blockstorageshort}}** page.
 2. Click **Actions** and select **Cancel {{site.data.keyword.blockstorageshort}}**.
 3. Select when to cancel. Choose **Immediately** or **Anniversary Date** and click **Continue**.
 4. Click the **I acknowledge that due to cancellation, data loss may occur** check box and click **Cancel**.
