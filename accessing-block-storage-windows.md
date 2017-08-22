---

copyright:
  years: 2014, 2017
lastupdated: "2017-08-22"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Connecting to MPIO iSCSI LUNS on Microsoft Windows
Before starting, make sure the host accessing the Block Storage volume has been authorized through the Portal:

1. From the Block Storage listing page, click the **Actions** button associated with the newly provisioned volume, and click **Authorize Host**.
2. Select the desired host(s) from the list and click **Submit**; this authorizes the host(s) to access the volume.

## Mounting Block Storage Volumes

Following are the steps required to connect a Windows-based Bluemix Infrastructure compute instance to a multipath input/output (MPIO) Internet Small Computer System Interface (iSCSI) logical unit number (LUN). The example is based on Windows Server 2012. The steps can be adjusted for other Windows versions according to the operating system (OS) vendor documentation.

## High-level steps

1. Configure the MPIO feature at the system level.
2. Configure block storage (iSCSI) by running the iSCSI Initiator.
3. Make sure the MPIO option is selected in the iSCSI Initiator.
4. Add multiple sessions and paths to the block storage.
5. Verify the provisioned input/output operations per second (IOPS).

## Detailed steps

### Configure the MPIO feature

1. Launch the Server Manager and navigate to **Manage**, **Add Roles and Features**.
2. Click **Next** to the Features menu.
3. Scroll down and click the **Multipath I/O** checkbox.
4. Click **Install** to install MPIO on the host server.
![Adding Roles and Features](/images/Roles_Features.png)

### Add iSCSI support for MPIO

1. Open the MPIO Properties. To open MPIO Properties, click **Start**, point to Administrative Tools, and then click **MPIO**.
2. Click the **Discover Multi-Paths** tab
3. Select the **Add support for iSCSI devices** check box, and then click **Add**. When prompted to restart the computer, click **Yes**.

### Configure the iSCSI Initiator

1. Launch iSCSI Initiator from the Server Manager and select **Tools**, **iSCSI Initiator**.
2. Click the **Configuration** tab.
    - The Initiator Name field may already be populated with an entry similar to iqn.1991-05.com.microsoft:.
    - Click **Change** to replace existing values with your iSCSI Qualified Name (IQN). The IQN name can be obtained from the Block Storage Details screen in the Bluemix Portal. 
    ![iSCSI Initiator Properties](/images/iSCSI.png)
    - Click the **Discovery** tab and click **Discover Portal**.
    - Input the IP address of your iSCSI target and leave Port at the default value of 3260. 
    - Click **Advanced** to launch the Advanced Settings window.
    - Select **Enable CHAP log on** to turn on CHAP authentication. 
    ![Enable CHAP login](/images/Advanced_0.png)
    **Note:** The Name and Target secret fields are case sensitive.
         - Delete any existing entries in the Name field and input the username from the portal.
         - Enter the password from the portal in the Target secret field.<br/>
         **Note:** The Name and Target secret values can be obtained from the Block Storage Details screen in the portal as Username and Password respectively.
    - Click **OK** on the Advanced Settings and Discover Target Portal windows to get back to the main iSCSI Initiator Properties screen. If you receive authentication errors, recheck the username and password entries. 
    ![Inactive Target](/images/Inactive_0.png)
    Note that the name of your target appears in the Discovered targets section with an Inactive status. 
    
    The following steps show how to activate the target.
    
### Activate Target
1. Click **Connect** to connect to the target.
2. Select **Enable multi-path** checkbox to enable multi-path IO to the target. 
![Enable Multi-path](/images/Connect_0.png)
3. Click **Advanced** and select **Enable CHAP log on**. 
![Enable CHAP](/images/chap_0.png)
4. Enter the username in the Name field, and enter the password in the Target secret field.<br/>
**Note:** The Name and Target secret field values can be obtained from the Block Storage Details screen in the portal as User name and Password respectively
5. Click **OK** until the iSCSI Initiator Properties window is displayed. The status of the target in the Discovered Targets section changes from Inactive to Connected. 
![Connected status](/images/Connected.png) 


### Configure MPIO in the iSCSI Initiator

1. Launch the iSCSI Initiator, and on the Targets tab, click **Properties**.
2. Click **Add Session** on the Properties window to launch the Connect To Target window.
3. Select **Enable multi-path** checkbox and click **Advanced...**.
  ![Target](/images/Target.png) 
  
4. In the Advanced Settings window
   - Leave Default as the value for the Local Adapter and Initiator IP fields. For host servers with multiple interfaces into iSCSI, you’ll need to choose the appropriate value for the Initiator IP field.
   - Select the IP of your iSCSI storage from the Target portal IP drop–down list.
   - Click **Enable CHAP Log on** checkbox
   - Enter the Name and Target secret values obtained from the portal and click **OK**.
   - Click **OK** on the Connect To Target window to go back to the Properties window. The Properties window should now display more than one session within the Identifier window. You now have more than one session into the iSCSI storage. 
   ![Settings](/images/Settings.png) 
   
5. In the Properties window, click **Devices** and launch the Devices window. The device interface name should have mpio at the beginning of the device name. <br/>
  ![Devices](/images/Devices.png) 
  
6. Click **MPIO** to launch the Device Details window. This window lets you to choose load balancing policies for MPIO and also shows you the paths to the iSCSI. In this example, two paths are shown as available for MPIO with a Round Robin With Subset load balancing policy.
  ![DeviceDetails](/images/DeviceDetails.png) 
  
7. Click **OK** several times to exit the iSCSI Initiator.

### Unmounting Block Storage volumes

Following are the steps required to disconnect a Windows-based Bluemix compute instance to an MPIO iSCSI LUN. The example is based on Windows Server 2012. The steps can be adjusted for other Wind
ows versions according to the OS vendor documentation.

#### Launch the iSCSI Initiator.

1. Click the **Targets** tab.
2. Select the targets you wish to remove and click **Disconnect**.

#### Optional if you will no longer need to access the iSCSI targets:

1. Click the **Discovery** tab in the iSCSI Initiator.
2. Highlight the target portal associated with your storage volume and click **Remove**.

## How to verify if MPIO is configured correctly in Windows OSes

To verify if Windows MPIO is configured, you must first ensure the MPIO Add-on is enabled and the requisite reboot has been completed:

![Roles_Features_0](/images/Roles_Features_0.png)

Once the reboot is complete and the Performance Storage Device is added, we can verify if MPIO is configured and working. To do so, look at **Target Device Details** and click **MPIO**:
![DeviceDetails_0](/images/DeviceDetails_0.png)

If MPIO has not been configured on your Performance Storage Device and the SoftLayer Teams performs Maintenance or a Network Outage occurs, your Storage Device will disconnect and become unavailable. MPIO will ensure an extra level of connecitivty during those events, and will keep an established session with active reads/writes going to the LUN.

