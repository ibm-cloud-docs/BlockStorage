---

copyright:
  years: 2014, 2019
lastupdated: "2019-11-14"

keywords: Block storage, auxiliary storage, missing routes, mpio, multipath, windows, troubleshooting

subcollection: BlockStorage

---
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:help: data-hd-content-type='help'}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}

# Windows 2012 R2 - multiple iSCSI devices
{: #troubleshootingWin12}

If you use more than two iSCSI devices with the same host, you might find this procedure useful; especially if all the iSCSI connections are from the same Storage device. When you use more than two devices, but can see only two devices in Disk Manager, then you need to manually connect to each device in iSCSI Initiator on every server node.
{: tsSymptoms}

## Manually connecting storage devices
{: #manualconnect}
{: tsResolve}


1. Open the Windows iSCSI Initiator.
2. On the **Targets** tab, click **Devices**.

   ![iSCSI Initiator properties](/images/win12-ts1.png)
3. Confirm the number of devices that are shown. If you see two devices, instead of the four that were authorized, continue to the next step.
4. Click **Targets**, then **Connect**.
5. Select **Multipath**, then **Advanced**.
6. Select Microsoft iSCSI Initiator as the Local adapter. The Initiator IP belongs to your server.
7. Select the first of the IP addresses that are shown in the Target Portal IP list.

   ![Advanced Settings, IP addresses](/images/win12-ts3.png)

   You have to repeat this step for all the IP addresses that are listed.
   {:tip}

8. Select the **Enable CHAP** box, and enter the server's CHAP ID and Password.

   ![Advanced Settings, CHAP](/images/win12-ts4.png)
9. Click **OK**.
10. Repeat steps 5-9 for every IP that you entered in the iSCSI Initiator. When you're done, click the **Devices** tab and review the results. Expect to see every LUN that you set up listed twice.

    ![Devices Tab](/images/win12-ts5.png)
11. Click **OK**.
12. Open Disk Manager, and verify that your drives are now showing.

    ![Device Manager](/images/win12-ts6.png)
