---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Managing {{site.data.keyword.blockstorageshort}}

You can manage your {{site.data.keyword.blockstoragefull}} volumes through [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

## Viewing {{site.data.keyword.blockstorageshort}} LUN details

You can view a summary the key information for the selected storage LUN including extra snapshot and replication capabilities that were added to the storage.

1. Click **Storage**, **{{site.data.keyword.blockstorageshort}}**.
2. Click the appropriate LUN Name from the list.

## Authorizing hosts to access to {{site.data.keyword.blockstorageshort}}

"Authorized" hosts are hosts that were given access to a particular LUN. Without host authorization, you can't access or use the storage from your system. Authorizing a host to access your LUN generates the user name, password, and iSCSI qualified name (IQN), which are needed to mount the multipath I/O (MPIO) iSCSI connection.

You can authorize and connect hosts that are located in the same data center as your storage. You can have multiple accounts, but you can't authorize a host from one account to access your storage on another account.
{:important}

1. Click **Storage** -> **{{site.data.keyword.blockstorageshort}}**, and click your LUN Name.
2. Scroll to the **Authorized Hosts** section of the page.
3. On the right, click **Authorize Host**. Select the hosts that can access that particular LUN.



## Viewing the list of hosts that are authorized to access a {{site.data.keyword.blockstorageshort}} LUN

1. Click **Storage** -> **{{site.data.keyword.blockstorageshort}}**, and click your LUN Name.
2. Scroll down to the **Authorized Hosts** section.

There you can see the list of hosts, which are currently authorized to access the LUN. You can also see the authentication information that is needed to make a connection – user name, password, and IQN Host. The Target address is listed on the **Storage Detail** page. For NFS, the Target address is described as a DNS name, and for iSCSI, it's the IP address of the Discover Target Portal.



## Viewing the {{site.data.keyword.blockstorageshort}} to which a host is authorized

You can view the LUNs to which a host has access to, including information that is needed to make a connection – LUN Name, Storage Type, Target Address, capacity and location:

1. Click **Devices** -> **Device List** in the [{{site.data.keyword.slportal}}](http://control.softlayer.com/){:new_window} and click the appropriate device.
2. Select the **Storage** tab.

You're presented with a list of storage LUNs that this particular host has access to. The list is grouped by storage type (block, file, other). You can authorize more storage or remove access by clicking **Actions**.



## Mounting and unmounting {{site.data.keyword.blockstorageshort}}

Based on the Operating System of your host, follow the appropriate instructions.

- [Connecting to MPIO iSCSI LUNs on Linux](accessing_block_storage_linux.html)
- [Connecting to MPIO iSCSI LUNs on CloudLinux](configure-iscsi-cloudlinux.html)
- [Connecting to MPIO iSCSI LUNS on Microsoft Windows](accessing-block-storage-windows.html)
- [Configuring Block Storage for Backup with cPanel](configure-backup-cpanel.html)
- [Configuring Block Storage for Backup with Plesk](configure-backup-plesk.html)


## Revoking a host's access to {{site.data.keyword.blockstorageshort}}

If you want to stop the access from a host to a particular storage LUN, you can revoke the access. Upon revoking access, the host connection is dropped from the LUN. The operating system and applications on that host can't communicate with the LUN anymore.

To avoid host side issues, unmount the storage LUN from your operating system before you revoke the access to avoid missing drives or data corruption.
{:important}

You can revoke access from the **Device List** or the **Storage view**.

### Revoking access from the Device List

1. Click **Devices**, **Device List** from the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} and double-click the appropriate device.
2. Select the **Storage** tab.
3. You are presented with a list of storage LUNs that this particular host has access to. The list is grouped by storage type (block, file, other). Next to the LUN name, select **Action"" and click **Revoke Access**.
4. Confirm that you want to revoke the access for a LUN because the action can't be undone. Click **Yes** to revoke LUN access or **No** to cancel the action.

If you want to disconnect multiple LUNs from a specific host, you need to repeat the Revoke Access action for each LUN.
{:tip}


### Revoking access from the Storage View

1. Click **Storage**, **{{site.data.keyword.blockstorageshort}}**, and select the LUN from which you want to revoke access.
2. Scroll to ** Authorized Hosts**.
3. Click **Actions** next to the host whose access is to be revoked, and select **Revoke Access**.
4. Confirm that you want to revoke the access for a LUN because the action can't be undone. Click **Yes** to revoke LUN access or **No** to cancel the action.

If you want to disconnect multiple hosts from a specific LUN, you need to repeat the Revoke Access action for each host.
{:tip}



## Canceling a storage LUN

If you no longer need a specific LUN, you can cancel it.
To cancel a storage LUN, it's necessary to revoke access from any hosts first.
{:important}

1. Click **Storage**, **{{site.data.keyword.blockstorageshort}}**.
2. Click **Actions** for the LUN to be canceled and select **Cancel {{site.data.keyword.blockstorageshort}}**.
3. Confirm if want to cancel the LUN immediately or on the anniversary date of when the LUN was provisioned.

   If you select the option to cancel the LUN on its anniversary date, you can void the cancellation request before its anniversary date.
   {:tip}
4. Click **Continue** or **Close**.
5. Click the **Acknowledgment** check box and click **Confirm**.
