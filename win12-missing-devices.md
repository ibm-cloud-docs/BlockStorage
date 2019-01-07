---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-07"

---

{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Windows 2012 R2 - multiple iSCSI devices

If you use more than two iSCSI devices, you might find this procedure useful; especially if all 4 iSCSI allotments are from the same Storage device. If you see only two devices in Disk Manager, then you need to manually connect to each device in iSCSI Initiator on every server node.

1. Open the Windows iSCSI Initiator.
2. Click the **Targets** tab, then click **Devices**.

   ![iSCSI Initiator properties](/images/win12-ts1.png)
3. Confirm the number of devices that are shown. If you see 2 devices, instead of 4 that were authorized, continue to the next step.
4. Click **Targets**, then **Connect**.
5. Select **Multipath**, then **Advanced**.
6. Select Microsoft iSCSI Initiator as the Local Adapter. The Initiator IP belongs to your server.
7. Select the first of the IP addresses that are shown in the Target Portal IP list.

   ![Advanced Settings, IP addresses](/images/win12-ts3.png)

   You have to repeat this step for all the IP addresses that are listed.
   {:tip}

8. Select the **Enable CHAP** box, and enter the server's CHAP ID and Password.

   ![Advanced Settings, CHAP](/images/win12-ts4.png)
9. Click **OK**.
10. Repeat steps 5-9 for every IP that you entered in the iSCSI Initiator. When you're done, click the **Devices** tab and review the results. Expect to see every LUN that you set up listed twice.

    ![Devices tab](/images/win12-ts5.png)
11. Click **OK**.
12. Open Disk Manager, and verify that your drives are now showing.

    ![Device Manager](/images/win12-ts6.png)
