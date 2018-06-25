---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-25"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Connecting to MPIO iSCSI LUNS on Microsoft Windows

Before you start, make sure the host that is accessing the {{site.data.keyword.blockstoragefull}} volume was authorized through the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}:

1. From the {{site.data.keyword.blockstorageshort}} listing page, locate the new volume and click **Actions**. Click **Authorize Host**.
2. From the list, select the host or hosts that are to access the volume and click **Submit**.

## Mounting {{site.data.keyword.blockstorageshort}} Volumes

Following are the steps that are required to connect a Windows-based {{site.data.keyword.BluSoftlayer_full}} Compute instance to a multipath input/output (MPIO) internet Small Computer System Interface (iSCSI) logical unit number (LUN). The example is based on Windows Server 2012. The steps can be adjusted for other Windows versions according to the operating system's (OS) vendor documentation.

### Configuring the MPIO feature

1. Start the Server Manager and browse to **Manage**, **Add Roles and Features**.
2. Click **Next** to the Features menu.
3. Scroll down and check **Multipath I/O**.
4. Click **Install** to install MPIO on the host server.
![Adding Roles and Features in Server Manager](/images/Roles_Features.png)

### Adding iSCSI support for MPIO

1. Open the MPIO Properties window by clicking **Start**, pointing to **Administrative Tools**, and clicking **MPIO**.
2. Click **Discover Multi-Paths**.
3. Check mark **Add support for iSCSI devices**, and then click **Add**. When prompted to restart the computer, click **Yes**.

**Note**: In Windows Server 2008, adding support for iSCSI allows the Microsoft Device Specific Module (MSDSM) to claim all iSCSI devices for MPIO, which requires a connection to an iSCSI Target first.

### Configuring the iSCSI Initiator

1. Start iSCSI Initiator from the Server Manager and select **Tools**, **iSCSI Initiator**.
2. Click the **Configuration** tab.
    - The Initiator Name field might already be populated with an entry similar to `iqn.1991-05.com.microsoft:`.
    - Click **Change** to replace existing values with your iSCSI Qualified Name (IQN). The IQN name can be obtained from the {{site.data.keyword.blockstorageshort}} Details screen in the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}. 
    ![iSCSI Initiator Properties](/images/iSCSI.png)
    - Click the **Discovery** tab and click **Discover Portal**.
    - Input the IP address of your iSCSI target and leave Port at the default value of 3260. 
    - Click **Advanced** to open the Advanced Settings window.
    - Select **Enable CHAP log on** to turn on CHAP authentication. 
    ![Enable CHAP login](/images/Advanced_0.png)
    **Note:** The Name and Target secret fields are case-sensitive.
         - In the **Name** field, delete any existing entries and input the user name from the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
         - In the **Target secret** field, enter the password from the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
    - Click **OK** on **Advanced Settings** and **Discover Target Portal** windows to get back to the main iSCSI Initiator Properties screen. If you receive authentication errors, check the user name and password entries. 
    ![Inactive Target](/images/Inactive_0.png)
    **Note**: the name of your target appears in the Discovered targets section with an Inactive status. 

    
### Activating Target

1. Click **Connect** to connect to the target.
2. Select **Enable multi-path** check box to enable multi-path IO to the target. 
![Enable Multi-path](/images/Connect_0.png)
3. Click **Advanced** and select **Enable CHAP log on**. 
![Enable CHAP](/images/chap_0.png)
4. Enter the user name in the Name field, and enter the password in the Target secret field.<br/>
**Note:** The Name and Target secret field values can be obtained from the {{site.data.keyword.blockstorageshort}} Details screen.
5. Click **OK** until the **iSCSI Initiator Properties** window is displayed. The status of the target in the **Discovered Targets** section changes from **Inactive** to **Connected**. 
![Connected status](/images/Connected.png) 


### Configuring MPIO in the iSCSI Initiator

1. Start the iSCSI Initiator, and on the Targets tab, click **Properties**.
2. Click **Add Session** on the Properties window to open the Connect To Target window.
3. Select **Enable multi-path** check box and click **Advanced...**.
  ![Target](/images/Target.png) 
  
4. In the Advanced Settings window:
   - Leave Default as the value for the Local Adapter and Initiator IP fields. For host servers with multiple interfaces into iSCSI, you need to choose the appropriate value for the Initiator IP field.
   - Select the IP of your iSCSI storage from the **Target portal IP** drop–down list.
   - Click **Enable CHAP log on** check box
   - Enter the Name and Target secret values that were obtained from the portal and click **OK**.
   - Click **OK** on the Connect To Target window to go back to the Properties window. Now the Properties window displays more than one session within the Identifier pane. You now have more than one session into the iSCSI storage. 
   ![Settings](/images/Settings.png) 
   
5. In the Properties window, click **Devices** to open the Devices window. The device interface name start with `mpio`. <br/>
  ![Devices](/images/Devices.png) 
  
6. Click **MPIO** to open the **Device Details** window. You can choose load balance policies for MPIO in this window and it shows you the paths to the iSCSI. In this example, two paths are shown as available for MPIO with a Round Robin With Subset load balance policy.
  ![Device Details window shows two paths available for MPIO with a Round Robin With Subset load balance policy](/images/DeviceDetails.png) 
  
7. Click **OK** several times to exit the iSCSI Initiator.



## Verifying whether MPIO is configured correctly in Windows Operating systems

To verify if Windows MPIO is configured, you must first ensure that the MPIO Add-on is enabled and restart the server.

![Roles_Features_0](/images/Roles_Features_0.png)

When the reboot is complete and the Storage Device is added, you can verify if MPIO is configured and working. To do so, look at **Target Device Details** and click **MPIO**:
![DeviceDetails_0](/images/DeviceDetails_0.png)

If MPIO wasn't configured correctly, your storage device disconnects and becomes unavailable when a network outage occurs or when {{site.data.keyword.BluSoftlayer_full}} Teams perform maintenance. MPIO ensures an extra level of connectivity during those events, and keeps an established session with active reads/writes going to the LUN.

## Unmounting {{site.data.keyword.blockstorageshort}} volumes

Following are the steps that are required to disconnect a Windows-based {{site.data.keyword.Bluemix_short}} compute instance to an MPIO iSCSI LUN. The example is based on Windows Server 2012. The steps can be adjusted for other Windows versions according to the OS vendor documentation.

### Starting the iSCSI Initiator

1. Click the **Targets** tab.
2. Select the targets that you want to remove and click **Disconnect**.

### Removing targets (Optional, if you no longer need to access the iSCSI targets)

1. Click **Discovery** in the iSCSI Initiator.
2. Highlight the target portal that is associated with your storage volume and click **Remove**.
