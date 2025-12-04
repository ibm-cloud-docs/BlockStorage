---

copyright:
  years: 2014, 2025
lastupdated: "2025-12-03"

keywords: MPIO iSCSI LUNS, iSCSI Target, MPIO, multipath, block storage, LUN, mounting, mapping secondary storage

subcollection: BlockStorage

content-type: tutorial
services:
account-plan: paid
completion-time: 1h

---
{{site.data.keyword.attribute-definition-list}}

# Mount iSCSI volumes on Microsoft Windows
{: #mountingWindows}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="1h"}

This tutorial guides you through how to mount an {{site.data.keyword.blockstoragefull}} volume on a server with a Windows 2019 or Windows 2022 operating system. You're going to create two connections from one network interface of your host to two target IP addresses of the storage array.
{: shortdesc}

## Before you begin
{: #authhostwin}
{: step}

1. [Create a virtual server for Classic in the console](/docs/virtual-servers?group=provisioning-ht), from the CLI, with the API, or [Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-sample_infrastructure_config).

1. [Order a block storage volume](/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage) in the same data center.

1. Make sure that the host is authorized to access the {{site.data.keyword.blockstorageshort}} volume. For more information, see [Authorizing the host in the console](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=ui#authhostUI){: ui}[Authorizing the host from the CLI](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=cli#authhostCLI){: cli}[Authorizing the host with Terraform](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=terraform#authhostTerraform){: terraform}. After the authorization is complete, note the username, password, and host IQN information.

If multiple hosts mount the same {{site.data.keyword.blockstorageshort}} volume without being cooperatively managed, your data is at risk for corruption. Volume corruption can occur if changes are made to the volume by multiple hosts at the same time. You need a cluster-aware, shared-disk file system to prevent data loss such as Microsoft Cluster Shared Volumes (CSV), Red Hat Global File System (GFS2), VMware&reg; VMFS, and others. For more information, see your OS Documentation.

The following activities are prerequisites on the iSCSI client:
- Installation of Multipath-IO services 
- Setting the iSCSI initiator service to start automatically
- Enabling support for multipath MPIO to iSCSI
- Enabling automatic claiming of all iSCSI volumes

Restart the Windows client after installation of these prerequisites. The MPIO load-balancing policy requires a restart so that it can be set.
{: important}

## Installing the MPIO feature
{: #installMPIOWin}
{: step}

1. Establish an RDP connection to your server with the Windows App.
1. Start the Server Manager and browse to **Manage**, **Add Features**.
2. Click **Next** until you get to the Features menu.
3. Scroll down and check **Multipath I/O**.
4. Click **Next** and **Install** to install MPIO on the host server.

   ![The image shows the Select features window of the Add Roles and Features Wizard in Server Manager. The MPIO option is selected in the Features list. The Next button is highlighted with a blue outline.](images/1-EnableMultipath.svg){: caption="Install MPIO on the host server." caption-side="bottom"}
5. Restart the server.

## Adding iSCSI support for MPIO devices
{: #addISCSIWim}
{: step}

1. Open the MPIO Properties window by clicking **Start**, pointing to **Administrative Tools**, and clicking **MPIO**.
2. Click **Discover Multi-Paths**.
3. Select **Add support for iSCSI devices**, and click **Add**.
   
   ![The image shows the MPIO properties screen. The Discover Multi-paths tab is selected. The box that is next to Add support for iSCSI devices option is checked. The Add and OK buttons are also visible and active.](images/2-AddMPIOsupport4iSCSIdevices.svg){: caption="Enable MPIO support for ISCSI devices." caption-side="bottom"}
1. Close the window by clicking **OK**.

## Configuring the iSCSI Initiator to discover the Target
{: #configISCSIWin}
{: step}

1. From the Server Manager, start iSCSI Initiator by selecting **Tools** > **iSCSI Initiator**.
   1. If the iSCSI service is not running yet, the server prompts you to click **Yes** to start the service. Your server must be rebooted for the setting to take effect.
   1. Return to the iSCSI Initiator properties screen.
1. Click the **Configuration** tab.
   1. The Initiator Name field might already be populated with an `iqn` entry.
   1. Click **Change** to replace existing values with your iSCSI Qualified Name (IQN)[^IQN] from the console.
     
      ![The image shows the iSCSI Initiator Properties screen with the Initiator Name field that is prepopulated. The Change button is highlighted with a blue outline. ](images/3-ChangeIQN.svg){: caption="ISCSI Initiator Properties" caption-side="bottom"}
      [^IQN]: The IQN name can be obtained from the **{{site.data.keyword.blockstorageshort}} Detail** screen in the [{{site.data.keyword.cloud_notm}} console](/login){: external}.
1. Click **Discovery**, and click **Discover Portal**.
     
      ![The image shows the Discovery tab in the iSCSI Initiator Properties screen. The Discover portal button is highlighted with a light blue background.](images/4-DiscoveryPortal.svg){: caption="ISCSI Initiator Properties, Discovery tab" caption-side="bottom"}
   1. Input the IP address of your iSCSI target and keep the default value of 3260 for the Port.
   1. Click **Advanced** to open the Advanced Settings window.
   1. On the Local adapter list, select Microsoft iSCSI Initiator.
   1. On the Initiator IP list, select the IP address of the host.
   1. On the Target Portal IP list, select the IP of the block volume.
   1. Select **Enable CHAP log-on** to turn on CHAP authentication.

      ![The image shows the General tab of the Advanced Settings screen. The Enable CHAP log-on option is selected. The Name field contains an IBM Cloud volume name, and the Target secret field is active.](images/5-AdvancedCHAPLogOn.svg){: caption="Enable CHAP Login in Advanced Settings." caption-side="bottom"}
   1. In the **Name** field, delete any existing entries and input the username from the [{{site.data.keyword.cloud_notm}} console](/login){: external}. This field is case-sensitive.
   1. In the **Target secret** field, enter the password from the [{{site.data.keyword.cloud_notm}} console](/login){: external}. This field is case-sensitive.
   1. Click **OK** on **Advanced Settings** and **Discover Target Portal** windows to get back to the main iSCSI Initiator Properties screen. If you receive authentication errors, check the username and password entries.
1. On the Targets screen, the name of your target appears in the Discovered targets section with an `Inactive` status. Click **Connect** to connect to the target.

   ![The image shows the Target tab of the iSCSI Initiator Properties screen. The discovered target is in inactive status.](images/6-ConnectDiscoveredTarget.svg){: caption="Discovered Target in the ISCSI Initiator Properties window." caption-side="bottom"}
1. Select **Enable multi-path** checkbox to enable multi-path IO to the target.

   ![The image shows the Connect to Target screen, the Enable Multi-path option is selected. Advanced and OK buttons are highlighted with a blue outline.](images/7-CHAPLogon4Connection.svg){: caption="Enable multi-path IO on the Connect to Target screen." caption-side="bottom"}
5. Click **Advanced**, and select **Enable CHAP log-on**.

   ![The image shows the General tab of the Advanced Settings screen. The Enable CHAP log-on option is selected. The Name field contains an IBM Cloud volume name, and the Target secret field is active.](images/8-InputCHAPcredentials.svg){: caption="CHAP logon and credentials." caption-side="bottom"}
6. Enter the username in the Name[^username] field, and enter the password in the Target secret[^password] field.
   [^username]: The Name and Target secret field values can be obtained from the **{{site.data.keyword.blockstorageshort}} Detail** screen.
   [^password]: The Name and Target secret field values can be obtained from the **{{site.data.keyword.blockstorageshort}} Detail** screen.

7. Click **OK** until the **iSCSI Initiator Properties** window is displayed. The status of the target in the **Discovered Targets** section changes from **Inactive** to **Connected**.

   ![The image shows the Target tab of the iSCSI Initiator Properties screen. The discovered target is in Connected status.](images/9-Target1Connected.svg){: caption="The first discovered target is shown as connected." caption-side="bottom"}

### Adding and configuring multiple MPIO sessions
{: #configmultiMPIOsessions}

1. Start the iSCSI Initiator, and on the Targets tab, click **Properties**.
2. Click **Add Session** on the Properties window.
3. In the Connect to Target dialog box, select **Enable multi-path** checkbox, and click **Advanced**.

   ![The image shows the Properties screen where you can click Add session to initiate a connection to the second Target. The image also shows the Connect to Target dialog where the new target name is entered and the Enable multi-path option is selected.](images/10-OneActiveSession.svg){: caption="Adding a second MPIO path." caption-side="bottom"}
4. In the Advanced Settings window, update the following fields.
   1. On the Local adapter list, select Microsoft iSCSI Initiator.
   1. On the Initiator IP list, select the IP address of the host.
   1. On the Target Portal IP list, select the IP of one of the storage interfaces.
   1. Click **Enable CHAP log-on** checkbox.
   1. Enter the Name and Target secret values that were obtained from the console and click **OK**.
   1. Click **OK** on the Connect To Target window to go back to the Properties window.
5. Click **Properties**. In the Properties dialog box, click **Add Session** again to add the second path.
6. In the Connect to Target window, select the checkbox to **Enable multi-path**. Click **Advanced**.
7. In the Advanced Settings window,
   1. On the Local adapter list, select Microsoft iSCSI Initiator.
   1. On the Initiator IP list, select the IP address that corresponds to the host. In this case, you are connecting two network interfaces on the storage array to a single network interface on the host. Therefore, this interface is the same as the one that was provided for the first session.
   1. On the Target Portal IP list, select the IP address for the second interface[^SecondIP] that is enabled on the storage array.
     [^SecondIP]: You can find the second IP address in the **{{site.data.keyword.blockstorageshort}} Detail** screen in the [{{site.data.keyword.cloud_notm}} console](/login){: external}.
   1. Click **Enable CHAP log-on** checkbox.
   1. Enter the Name and Target secret values that were obtained from the console and click **OK**.
   1. Click **OK** on the Connect To Target window to go back to the Properties window.
     
   ![The image shows the General tab of the Advanced Settings screen. The Enable CHAP log-on option is selected for adding the credentials of the 2nd target.](images/11-InputCHAPcredentials2ndTarget.svg){: caption="Adding CHAP credentials for the 2nd target in Advanced Settings." caption-side="bottom"}
8. Now the Properties window displays more than one session within the Identifier pane. It means that you have more than one session into the iSCSI storage.
    
   ![The image shows the Properties window, and the Sessions screen. Two connected sessions are displayed in the list.](images/12-Confirm2ConnectedSessions.svg){: caption="Two connected sessions are displayed." caption-side="bottom"}

   If your host has multiple interfaces that you want to connect to the storage, you can set up another connection with the address of the second NIC in the Initiator IP field. However, be sure to authorize the second initiator IP address in the [{{site.data.keyword.cloud}} console](/login){: external} before you attempt to make the connection.
   {: note}

9. In the Properties window, click **Devices** to open the Devices window. The device interface name start with `mpio`.
   
   ![Devices window](images/13-ConfigureMPIO.svg){: caption="Devices window displays the iSCSI target." caption-side="bottom"}
10. Click **MPIO** to open the **Device Details** window. You can choose load balance policies for MPIO in this window and it shows you the paths to the iSCSI. In this example, two paths are shown as available for MPIO.
   
   ![The Device Details window shows two paths available for MPIO with a Round Robin With Subset load balance policy.](images/14-DeviceDetails2paths.svg){: caption="Multipath can be validated on the Device Details window." caption-side="bottom"}

11. Click **OK** several times to exit the iSCSI Initiator.

## Initializing and formatting the {{site.data.keyword.blockstorageshort}} volume
{: #formatLUNonWIn}
{: step}

1. Press the Windows Logo key + X, and then click **Run**.
2. In the Run dialog box, type `Diskmgmt.msc`. Click **OK**, and the Disk Management dialog box appears. The side pane shows the drives that are attached to your host.
3. In the Disk Management window, right-click the discovered volume's name, and then click **online**.
4. Right-click and select **Initialize Disk**.
5. In the dialog box, select the disk to initialize, and then click **OK**.
6. The New Simple Volume wizard starts. Select a disk size, and then click **Next**.
7. Assign a drive letter to the volume, and then click **Next**.
8. Enter the parameters to format the volume.
    - On a Windows Server, only NTFS is supported.
    - Set the allocation unit size to 64 K.
    - Provide a label for your Storage volume.
9. Click **Next**.
10. Check the values for your volume, and then click **Finish**. On the Disk Management page, the volume now appears as online.

## Verifying whether MPIO is configured correctly
{: #verifyMPIOWindows}
{: step}

It's possible to attach a volume with a single path, but it is important that connections are established on both paths to ward against disruption of service. To verify whether Windows MPIO is configured, you must first make sure that the MPIO Add-on is enabled and then restart the server.

![The image shows the Select features window of the Add Roles and Features Wizard in Server Manager. The MPIO option is selected in the Features list.](images/Roles_Features_0.svg){: caption="Multipath I/O is shown as checked." caption-side="bottom"}

After the restart is complete, take the following steps to view all of the active paths.
1. On the desktop, click **Start**.
2. In the Start Search field, type `diskmgmt.msc`.
3. In the Programs list, click `diskmgmt`.
4. Right-click each disk for which you want to verify the multiple paths and then click **Properties**.
5. On the MPIO tab, in the Select the MPIO policy list, click all the paths that are active.
   
   ![The Device Details screen is shown with 2 active paths on the MPIO tab.](images/DeviceDetails_0.svg){: caption="Several paths that are leading to the target are shown." caption-side="bottom"}

To verify multipathing by using the command line, complete the following steps.

1. Open the command prompt.
2. Run `mpclaim.exe –v c:\multipathconfig.txt` to capture the multipath configuration.
3. Review the contents of the `multipathconfig.txt`. Confirm that each of the two paths that are listed for the volume contain distinct TPG_Id values.

If MPIO isn't configured correctly, your storage device might disconnect and appear offline when a network outage occurs or when {{site.data.keyword.cloud}} teams perform maintenance. MPIO provides an extra level of connectivity during those events, and keeps an established session to the volume with active read/write operations.

On rare occasions, a volume is provisioned and attached while the second path is down. In such instances, the host might see one single path when the discovery scan is run. If you encounter this phenomenon, check the [{{site.data.keyword.cloud}} status page](/status?component=block-storage&selected=status){: external} to see whether a current event might impact your host's ability to access the storage. If no events are reported, perform the discovery scan again to make sure that all paths are properly discovered. If an event is in progress, the storage can be attached with a single path. However, it's essential that paths are rescanned after the event is completed. If both paths are not discovered after the rescan, [create a support case](/unifiedsupport/cases/add){: external} so it can be properly investigated.

## Unmounting {{site.data.keyword.blockstorageshort}} volumes
{: #unmountingWin}
{: step}

To disconnect an iSCSI volume from a Windows-based {{site.data.keyword.cloud}} Compute instance, complete the following steps.

### Disconnect the volume from the iSCSI Initiator
{: #startISCSIwin}

1. In Server Manager, click **Storage** > **iSCSI**. 
1. Right-click the volume and take it **Offline**.
1. In iSCSI Initiator, click **Targets**.
2. Select the target that you want to remove and click **Disconnect**.

### Removing targets
{: #removetargetoptional}

This step is optional, for when you no longer need to access the iSCSI targets.

1. Click **Discovery** in the iSCSI Initiator.
2. Highlight the target portal that is associated with your storage volume and click **Remove**.
