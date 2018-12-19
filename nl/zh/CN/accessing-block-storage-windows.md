---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 在 Microsoft Windows 上连接到 MPIO iSCSI LUN

开始之前，请确保正在访问 {{site.data.keyword.blockstoragefull}} 卷的主机已通过 [{{site.data.keyword.slportal}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){:new_window} 授权。


1. 在 {{site.data.keyword.blockstorageshort}} 列表页面中，找到新卷，然后单击**操作**。单击**授权主机**。
2. 从列表中选择将访问该卷的一个或多个主机，然后单击**提交**。

## 安装 {{site.data.keyword.blockstorageshort}} 卷

下面是将基于 Windows 的 {{site.data.keyword.BluSoftlayer_full}} 计算实例连接到多路径输入/输出 (MPIO) 因特网小型计算机系统接口 (iSCSI) 逻辑单元号 (LUN) 所需的步骤。示例基于 Windows Server 2012。对于其他 Windows 版本，可以根据相应操作系统 (OS) 供应商文档来调整这些步骤。

### 配置 MPIO 功能

1. 启动服务器管理器，并浏览至**管理** > **添加角色和功能**。
2. 单击**下一步**以转至“功能”菜单。
3. 向下滚动并选中**多路径 I/O**。
4. 单击**安装**以在主机服务器上安装 MPIO。
![在服务器管理器中添加角色和功能](/images/Roles_Features.png)

### 添加对 iSCSI 的 MPIO 支持

1. 通过单击**启动**，指向**管理工具**并单击 **MPIO**，打开“MPIO 属性”窗口。
2. 单击**发现多路径**。
3. 选中**添加对 iSCSI 设备的支持**，然后单击**添加**。系统提示重新启动计算机时，单击**是**。

在 Windows Server 2008 中，添加对 iSCSI 的支持后，Microsoft 设备特定模块 (MSDSM) 就可以声明所有要进行 MPIO 的 iSCSI 设备，但在此之前，需要先连接到 iSCSI 目标。
{:note}

### 配置 iSCSI 启动器

1. 从服务器管理器启动 iSCSI 启动器，然后选择**工具** > **iSCSI 启动器**。
2. 单击**配置**选项卡。
    - “发起方名称”字段可能已使用类似于 i`iqn.1991-05.com.microsoft:` 的条目填充。
    - 单击**更改**以将现有值替换为 iSCSI 限定名 (IQN)。
    ![iSCSI 启动器属性](/images/iSCSI.png)

      IQN 名称可以从 {{site.data.keyword.blockstorageshort}}[{{site.data.keyword.slportal}} 中的“详细信息”屏幕 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){:new_window} 中获取。
      {: tip}

    - 单击**发现**选项卡，然后单击**发现门户网站**。
    - 输入 iSCSI 目标的 IP 地址，并使“端口”保留为缺省值 3260。
    - 单击**高级**以打开“高级设置”窗口。
    - 选中**启用 CHAP 登录**以启用 CHAP 认证。
    ![启用 CHAP 登录](/images/Advanced_0.png)
        “名称”和“目标私钥”字段区分大小写。
    {:important}
         - 在**名称**字段中，删除任何现有条目，然后输入 [{{site.data.keyword.slportal}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){:new_window} 的用户名。
         - 在**目标密钥**字段中，输入 [{{site.data.keyword.slportal}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){:new_window} 的密码。
    - 单击**高级设置**和**发现目标门户网站**窗口中的**确定**，以返回到“iSCSI 启动器属性”主屏幕。如果收到认证错误，请检查用户名和密码条目。
    ![不活动的目标](/images/Inactive_0.png)
        目标的名称显示在“发现的目标”部分中，状态为`不活动`。
    {:note}


### 激活目标

1. 单击**连接**以连接到目标。
2. 选中**启用多路径**复选框以启用到目标的多路径 IO。
</br>
   ![启用多路径](/images/Connect_0.png)
3. 单击**高级**，然后选择**启用 CHAP 登录**。
</br>
   ![启用 CHAP](/images/chap_0.png)
4. 在“名称”字段中输入用户名，然后在“目标私钥”字段中输入密码。

   “名称”和“目标私钥”字段值可以从“{{site.data.keyword.blockstorageshort}} 详细信息”屏幕中获取。
   {:tip}
5. 单击**确定**，直至显示 **iSCSI 启动器属性**窗口。**发现的目标**部分中目标的状态已从**不活动**变为**已连接**。
</br>
   ![“已连接”状态](/images/Connected.png)


### 在 iSCSI 启动器中配置 MPIO

1. 启动 iSCSI 启动器，然后在“目标”选项卡上，单击**属性**。
2. 在“属性”窗口中，单击**添加会话**以打开“连接到目标”窗口。
3. 在“连接到目标”对话框中，选中**启用多路径**复选框，然后单击**高级**。
  ![目标](/images/Target.png)

4. 在“高级设置”窗口中 ![设置](/images/Settings.png)：
   - 在“本地适配器”列表中，选择“Microsoft iSCSI 启动器”。
   - 在“启动器 IP”列表中，选择主机的 IP 地址。
   - 在“目标门户网站 IP”列表中，选择设备接口的 IP。
   - 单击**启用 CHAP 登录**复选框。
   - 输入从门户网站中获取的“名称”和“目标私钥”值，然后单击**确定**。
   - 在“连接到目标”窗口上，单击**确定**以返回到“属性”窗口。

5. 单击**属性**。在“属性”对话框中，再次单击**添加会话**以添加第二个路径。
6. 在“连接到目标”窗口中，选中**启用多路径**复选框。单击**高级**。
7. 在“高级设置”窗口中：
   - 在“本地适配器”列表中，选择“Microsoft iSCSI 启动器”。
   - 在“启动器 IP”列表中，选择与主机对应的 IP 地址。在此情况下，您要将设备上的两个网络接口连接到主机上的单个网络接口。因此，此接口与为第一个会话提供的接口相同。
   - 在“目标门户网站 IP”列表中，为设备上启用的第二个数据接口选择 IP 地址。
   - 单击**启用 CHAP 登录**复选框。
   - 输入从门户网站中获取的“名称”和“目标私钥”值，然后单击**确定**。
   - 在“连接到目标”窗口上，单击**确定**以返回到“属性”窗口。
8. “属性”窗口的“标识”窗格中现在会显示多个会话。iSCSI 存储器中有多个会话。

   如果您的主机有多个要连接到 iSCSI 存储器的接口，那么可以在“启动器 IP”字段中使用其他 NIC 的 IP 地址再设置一个连接。但是，在尝试建立连接之前，确保在 [{{site.data.keyword.slportal}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){:new_window} 中对第二个启动器 IP 地址进行授权。
   {:note}
9. 在“属性”窗口中，单击**设备**以打开“设备”窗口。设备接口名称以 `mpio` 开头。<br/>
  ![设备](/images/Devices.png)

10. 单击 **MPIO** 以打开**设备详细信息**窗口。在此窗口中，可以为 MPIO 选择负载均衡策略，并且还会显示 iSCSI 的路径。在此示例中，显示了两条可用于 MPIO 的路径，并且负载均衡策略为“带子集的循环法”。
  ![“设备详细信息”窗口显示可用于 MPIO 的两条路径以及“带子集的循环法”负载均衡策略](/images/DeviceDetails.png)

11. 单击**确定**多次以退出 iSCSI 启动器。



## 验证是否在 Windows 操作系统中正确配置了 MPIO

要验证是否已配置 Windows MPIO，必须先确保已启用 MPIO 附加组件，然后重新启动服务器。

![Roles_Features_0](/images/Roles_Features_0.png)

完成了重新启动并且添加了“存储设备”后，就可以验证 MPIO 是否已配置且生效。要执行此操作，请查看**目标设备详细信息**，然后单击 **MPIO**：
![DeviceDetails_0](/images/DeviceDetails_0.png)

如果未正确配置 MPIO，那么当发生网络中断或 {{site.data.keyword.BluSoftlayer_full}} 团队执行维护时，存储设备可能断开连接并显示为已禁用。MPIO 将确保在发生这些事件期间获得额外级别的连接，并且会保留已建立的会话，使活动读/写操作转至 LUN。

## 卸装 {{site.data.keyword.blockstorageshort}} 卷

下面是断开基于 Windows 的 {{site.data.keyword.Bluemix_short}} 计算实例与 MPIO iSCSI LUN 的连接所需的步骤。示例基于 Windows Server 2012。对于其他 Windows 版本，可以根据相应操作系统供应商文档来调整这些步骤。

### 启动 iSCSI 启动器

1. 单击**目标**选项卡。
2. 选择要除去的目标，然后单击**断开连接**。

### 除去目标
不再需要访问 iSCSI 目标时，此步骤为可选。

1. 单击 iSCSI 启动器中的**发现**。
2. 突出显示与存储卷关联的目标门户网站，然后单击**除去**。
