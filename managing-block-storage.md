---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Managing {{site.data.keyword.blockstorageshort}}

You can manager your {{site.data.keyword.blockstoragefull}} volumes through [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}. This article provides instructions for the most common tasks.

## See a provisioned {{site.data.keyword.blockstorageshort}} LUN details

You can view a summary the key information for the selected storage LUN including additional snapshot and replication capabilities that have been added to the storage.

1. Click **Storage**, **{{site.data.keyword.blockstorageshort}}**.
2. Click the appropriate LUN Name from the list.

## Authorize hosts to access to {{site.data.keyword.blockstorageshort}}

“Authorized” hosts are hosts that have been given access rights to a particular LUN. Without host authorization you won’t be able to access or use the storage from your system. Authorizing a host to access your LUN generates the Username, Password, and iSCSI qualified name (IQN), which is needed to mount the multipath I/O (MPIO) iSCSI connection.

**Note**: You can only authorize and connect hosts that reside in the same datacenter as your storage. If you have multiple accounts, you cannot authorize a host from one account to access your storage on another.

1. Click **Storage** -> **{{site.data.keyword.blockstorageshort}}**, and click on your LUN Name.
2. Scroll to the Authorized Hosts section of the page.
3. Click the **Authorize Host** link on the right side of the page. Select the hosts that can access that particular LUN.

 

## View the list of hosts authorized to a {{site.data.keyword.blockstorageshort}} LUN

Use the following steps to view the list of hosts authorized to a LUN.

1. Click **Storage** -> **{{site.data.keyword.blockstorageshort}}**, and click on your LUN Name.
2. Scroll down to the bottom of the page to the Authorized Hosts section.

Here you will see the list of hosts which are currently authorized to access the LUN and, specifically for iSCSI, the authentication information needed to make a connection – Username, Password, and IQN Host. The Target address is at the top of the Storage Detail page. For NFS it’s described as a DNS name and for iSCSI the IP address of the Discover Target Portal.

 

## View the {{site.data.keyword.blockstorageshort}} to which a host is authorized

You can view the LUNs to which a host has access to, including information needed to make a connection – LUN Name, Storage Type, Target Address, capacity and location:

1. Click **Devices** -> **Device List** from the [{{site.data.keyword.slportal}}](http://control.softlayer.com/){:new_window} and click the appropriate device.
2. Select the **Storage** tab.

You will then be presented with a list of storage LUNs that this particular host has access to, all grouped by storage type (block, file, other). From the respective Action menus you can authorize additional storage or remove access.

 

## Mount and Unmount {{site.data.keyword.blockstorageshort}}

Refer to the following articles with details for mounting and unmounting {{site.data.keyword.blockstorageshort}} from a host.

- [{{site.data.keyword.blockstorageshort}} on Linux](accessing_block_storage_linux.html)

- [{{site.data.keyword.blockstorageshort}} on Microsoft Windows](accessing-block-storage-windows.html)

 

## Revoke a host's access to {{site.data.keyword.blockstorageshort}}

If you want to stop the access from a host to a particular storage LUN you can revoke the access. Upon revoking access, the host connection will be dropped from the LUN and neither operating system nor applications will be able to communicate with the LUN.

**Note**: To avoid host side issues, unmount the storage LUN from your operating system before revoking access to avoid missing drives or data corruption.

You can revoke access from either Storage from the Device List or the Storage views.

### Revoke access from the Device List:

1. Click **Devices**, **Device List** from the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} and double-click the appropriate device.
2. Select the **Storage** tab.
3. You will then be presented with a list of storage LUNs that this particular host has access to, all grouped by storage type (block, file, other). Select the respective Action menu next to the LUN that you want to revoke access from and click **Revoke Access**.
4. You will be asked if you want to revoke the access for a LUN because the action cannot be undone. Click **Yes** to revoke LUN access or **No** to cancel the action.

**Note**: If you want to disconnect multiple LUNs from a specific host, you will need to repeat the Revoke Access action for each LUN.


### Revoke access from the Storage View:

1. Click **Storage**, **{{site.data.keyword.blockstorageshort}}**, and select the LUN from which you want to revoke access.
2. Scroll down to the** Authorized Hosts** section of the page.
3. Click the **Actions** drop-down arrow next to the host whose access is to be revoked, and select **Revoke Access**.
4. You will be asked if you want to revoke the access for a LUN because the action cannot be undone. Click **Yes** to revoke LUN access or **No** to cancel the action.

**Note**: If you want to disconnect multiple hosts from a specific LUN, you will need to repeat the Revoke Access action for each host.

 

## Cancel a storage LUN

If you no longer need a specific LUN, you can cancel it. In order to cancel a storage LUN, it's first necessary to revoke access from any hosts.

1. Click **Storage**, **{{site.data.keyword.blockstorageshort}}**.
2. Click the **Actions** drop-down arrow for the LUN to be cancelled and select **Cancel {{site.data.keyword.blockstorageshort}}**.
3. You will be asked to confirm if want to cancel the LUN immediately or on the anniversary date of when the LUN was provisioned. Click **Continue** or **Close**.
**Note**: If you select the option to cancel the LUN on its anniversary date, you can void the cancellation request prior to its anniversary date.
4. Click the **Acknowledgment** checkbox and click **Confirm**.

 

