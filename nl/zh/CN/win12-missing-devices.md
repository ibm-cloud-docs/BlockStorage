---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: Block storage, auxiliary storage, missing routes, mpio, multipath, windows, troubleshooting

subcollection: BlockStorage

---
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# Windows 2012 R2 - 多个 iSCSI 设备
{: #troubleshootingWin12}

如果使用两个以上具有相同主机的 iSCSI 设备，您可能会发现此过程很有用；尤其是，当所有 iSCSI 连接都来自相同存储设备的时候。如果在“磁盘管理器”中只能看到两个设备，那么需要手动连接到每个服务器节点上 iSCSI 启动器中的每个设备。
{:tsSymptoms}
{:tsResolve}


1. 打开 Windows iSCSI 启动器。
2. 在**目标**选项卡上，单击**设备**。

   ![iSCSI 启动器属性](/images/win12-ts1.png)
3. 确认显示的设备的数量。如果看到两个设备，而不是授权的四个，那么继续下一步。
4. 单击**目标**，然后单击**连接**。
5. 选择**多路径**，然后单击**高级**。
6. 选择 Microsoft iSCSI 启动器作为“本地适配器”。启动器 IP 属于您的服务器。
7. 选择“目标门户网站 IP”列表中显示的第一个 IP 地址。

   ![高级设置 - IP 地址](/images/win12-ts3.png)

   您必须针对列出的所有 IP 地址重复此步骤。
   {:tip}

8. 选择**启用 CHAP** 框，输入服务器的 CHAP 标识和密码。

   ![高级设置 - CHAP](/images/win12-ts4.png)
9. 单击**确定**。
10. 针对在 iSCSI 启动器中输入的每个 IP 重复步骤 5-9。在完成时，单击**设备**选项卡，然后复审结果。应该会看到您设置的每个 LUN 列出两次。

    ![“设备”选项卡](/images/win12-ts5.png)
11. 单击**确定**。
12. 打开“磁盘管理器”，并验证您的驱动器现在是否显示。

    ![设备管理器](/images/win12-ts6.png)
