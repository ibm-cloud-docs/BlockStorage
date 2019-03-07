---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 复制数据
{: #replication}

复制使用其中一个快照安排自动将快照复制到远程数据中心内的目标卷。如果发生灾难性事件，或者数据变得损坏，那么可以在远程站点中恢复副本。

复制可以使两个不同位置的数据保持同步。如果想要克隆卷并独立于原始卷来使用该卷，请参阅[创建复制块卷](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)。
{:tip}

必须创建快照安排后，才能进行复制。
{:important}


## 确定复制存储卷的远程数据中心

{{site.data.keyword.BluSoftlayer_full}} 在世界各地的数据中心已成对编成主数据中心和远程数据中心组合。
请参阅表 1，以获取数据中心可用性和复制目标的完整列表。

<table>
  <caption style="text-align: left;"><p>表 1 - 下表显示每个区域中具有增强功能的数据中心的完整列表。每个区域单独一列。某些城市（例如，达拉斯、圣何塞、华盛顿特区、阿姆斯特丹、法兰克福、伦敦和悉尼）有多个数据中心。</p>
  <p>&#42; “美国 1”区域中的数据中心没有增强型存储器。具有增强存储功能的数据中心内的主机<strong>无法</strong>启动与“美国 1”数据中心内副本目标的复制。</p>
  </caption>
  <thead>
    <tr>
      <th>美国 1 &#42;</th>
      <th>美国 2</th>
      <th>拉丁美洲</th>
      <th>加拿大</th>
      <th>欧洲</th>
      <th>亚太地区</th>
      <th>澳大利亚</th>
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
	  <br /><br /><br /><br /><br /><br />
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
	  <br /><br /><br />
      </td>
      <td>MEX01<br />
	  SAO01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>TOR01<br />
MON01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>AMS01<br />
	  AMS03<br />
	  FRA02<br />
	  FRA04<br />
	  FRA05<br />
	  LON02<br />
	  LON04<br />
	  LON05<br />
	  LON06<br />
	  OSL01<br />
	  PAR01<br />
	  MIL01<br />
      </td>
      <td>HKG02<br />
TOK02<br />
TOK04<br />
TOK05<br />
SNG01<br />
SEO01<br />
                                CHE01<br />
	        <br /><br /><br /><br /><br />
      </td>
      <td>SYD01<br />
SYD04<br />
          SYD05<br />
MEL01<br />
          <br /><br /><br /><br /><br /><br /><br /><br />
      </td>
    </tr>
  </tbody>
</table>

## 创建初始副本

复制将根据快照安排来执行。必须首先具有用于源卷的快照空间和快照安排，然后才能进行复制。如果尝试设置复制，但未设置源卷的快照空间或快照安排，那么系统将提示您购买更多空间或设置安排。复制在 [{{site.data.keyword.slportal}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){:new_window} 中的**存储** > **{{site.data.keyword.blockstorageshort}}** 下进行管理。

1. 单击存储卷。
2. 单击**副本**，然后单击**购买复制**。
3. 选择希望复制遵循的现有快照安排。此列表包含所有有效的快照安排。<br />
只能选择一个安排，即便有每小时、每天和每周的混合安排也是如此。将复制自上一个复制周期以来捕获到的所有快照，而不管这些快照源自哪个安排。<br />如果没有设置“快照”，系统将提示您进行设置，然后才能订购复制。有关更多详细信息，请参阅[使用快照](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots)。
   {:important}
3. 单击**位置**，然后选择将作为 DR 站点的数据中心。
4. 单击**继续**。
5. 如果您有促销码，请在**促销码**中进行输入，然后单击**重新计算**。缺省情况下，已填写窗口中的其他字段。

   处理订单时会应用折扣。
   {:note}
6. 单击**我已阅读主服务协议...** 复选框，然后单击**下订单**。


## 编辑现有复制

您可以在 [{{site.data.keyword.slportal}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){:new_window} 中的**存储** > **{{site.data.keyword.blockstorageshort}}** 下的**主**或**副本**选项卡中，编辑复制安排并更改复制空间。



## 编辑复制安排

复制安排基于现有快照安排。要更改复制安排（例如，从每小时更改为每周），必须取消该复制安排，然后设置新的复制安排。

更改安排可以在“主”或“副本”选项卡上执行。

1. 单击**主**或**副本**选项卡上的**操作**。
2. 选择**编辑快照安排**。
3. 查看**安排**下的**快照**框架，以确定要用于复制的安排。更改所需的安排。例如，如果复制安排为**每天**，那么可以更改执行复制的时刻。
4. 单击**保存**。


## 更改复制空间

主快照空间和副本空间必须相同。如果在**主**或**副本**选项卡上更改空间，那么会自动将该空间添加到源和目标数据中心。增大快照空间还会触发立即复制更新。

1. 单击**主**或**副本**选项卡上的**操作**。
2. 选择**添加更多快照空间**。

3. 从列表中选择存储器大小，然后单击**继续**。

4. 如果您有促销码，请在**促销码**中进行输入，然后单击**重新计算**。缺省情况下，已填写对话框中的其他字段。
5. 单击**我已阅读主服务协议...** 复选框，然后单击**下订单**。


## 在卷列表中查看副本卷

您可以在**存储 > {{site.data.keyword.blockstorageshort}}** 下的 {{site.data.keyword.blockstorageshort}} 页面中查看复制卷。**LUN 名称**显示为主卷名称后跟 REP。**类型**为“耐久性 - 副本”或“性能 - 副本”。**目标地址**为“不适用”（因为副本卷未安装在副本数据中心上），**状态**显示为“不活动”。


## 在副本数据中心查看复制卷的详细信息

您可以在**存储** > **{{site.data.keyword.blockstorageshort}}** 下的**副本**选项卡中查看副本卷详细信息。另一个选择是在 **{{site.data.keyword.blockstorageshort}}** 页面中选择副本卷，然后单击**副本**选项卡。


## 在主数据中心内增大快照空间时，也增大副本数据中心内的快照空间

主存储卷和副本存储卷的卷大小必须相同。不能其中一个卷大于另一个卷。增大主卷的快照空间时，副本空间也会自动增加。增大快照空间会触发立即复制更新。这两个卷的增大将在发票上显示为行项，并且将根据需要分派。

有关增加快照空间的更多信息，请参阅[订购快照](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots)。
{:tip}


## 查看复制历史记录

复制历史记录可在**管理**下**帐户**选项卡上的**审计日志**中进行查看。主卷和副本卷将显示完全相同的复制历史记录。历史记录包含以下项。

- 复制的类型（故障转移或故障恢复）。
- 启动时间。
- 用于复制的快照。
- 复制的大小。
- 完成复制的时间。


## 创建副本的复制项

您可以创建现有 {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.blockstoragefull}} 的复制项。缺省情况下，复制卷将继承原始卷的容量和性能选项，并且会包含截至快照时间点的数据的副本。

复制项可以基于主卷和副本卷创建。新复制项会在原始卷所在的数据中心内创建。如果基于副本卷创建复制项，那么新卷将在副本卷所在的数据中心内创建。

供应存储器后，主机即可以访问复制卷以进行读/写。但是，在完成从原始项到复制项的数据复制之后，才允许使用快照和复制。

有关更多信息，请参阅[创建复制块卷](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)。

## 在灾难发生时使用副本进行故障转移

进行故障转移时，将“翻转开关”从主数据中心的存储卷切换到远程数据中心的目标卷。例如，主数据中心是伦敦，辅助数据中心是阿姆斯特丹。如果发生故障事件，将故障转移到阿姆斯特丹 - 从阿姆斯特丹的计算实例连接到现在的主卷。修复伦敦的卷之后，会生成阿姆斯特丹卷的快照，以便故障恢复到伦敦，并从伦敦的计算实例连接到原先的主卷。

* 如果主位置即将面临危险或受到严重影响，请参阅[通过可访问的主卷进行故障转移](/docs/infrastructure/BlockStorage?topic=BlockStorage-dr-accessible)。
* 如果主位置停止运行，请参阅[通过不可访问的主卷进行故障转移](/docs/infrastructure/BlockStorage?topic=BlockStorage-dr-inaccessible)。


## 取消现有复制

您可以立即取消复制，也可以在周年日期取消复制；取消复制将导致记帐结束。可以在**主**或**副本**选项卡中取消复制。

1. 在 **{{site.data.keyword.blockstorageshort}}** 页面上，单击卷。
2. 单击**主**或**副本**选项卡上的**操作**。
3. 选择**取消复制**。
4. 选择取消时间。选择**立即**或**周年日期**，然后单击**继续**。
5. 单击**我确认取消操作可能会导致数据丢失**，然后单击**取消复制**。


## 取消主卷时取消复制

取消主卷后，将删除副本数据中心内的复制安排和卷。副本将在 {{site.data.keyword.blockstorageshort}} 页面中取消。

 1. 在 **{{site.data.keyword.blockstorageshort}}** 页面上，突出显示卷。
 2. 单击**操作**，然后选择**取消 {{site.data.keyword.blockstorageshort}}**。
 3. 选择取消时间。选择**立即**或**周年日期**，然后单击**继续**。
 4. 单击**我确认取消操作可能会导致数据丢失**，然后单击**取消**。

## SLCLI 中与复制相关的命令
{: #clicommands}

* 列出特定卷的适用复制数据中心。
  ```
  # slcli block replica-locations --help
  用法：slcli block replica-locations [OPTIONS] VOLUME_ID

  选项：
  --sortby TEXT   要作为排序依据的列
  --columns TEXT  要显示的列。选项：标识、长名称、短名称
  -h, --help      显示此消息并退出。
  ```

* 订购块存储器副本卷。
  ```
  # slcli block replica-order --help
  用法：slcli block replica-order [OPTIONS] VOLUME_ID

  选项：
  -s, --snapshot-schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                                  用于复制的快照安排，
                                  (INTERVAL | HOURLY | DAILY | WEEKLY)
                                  [必需]
  -l, --location TEXT             用于副本卷的数据中心的短名称
                                  （例如：dal09）[必需]
  --tier [0.25|2|4|10]            为其订购了副本卷的主卷的耐久性存储器层 (IOPS/GB) [可选]

  --os-type [HYPER_V|LINUX|VMWARE|WINDOWS_2008|WINDOWS_GPT|WINDOWS|XEN]
                                  为其订购了副本卷的主卷的操作系统类型（例如：LINUX）[可选]

  -h, --help                      显示此消息并退出。
  ```

* 列出块卷的现有副本卷。
  ```
  # slcli block replica-partners --help
  用法：slcli block replica-partners [OPTIONS] VOLUME_ID

  选项：
  --sortby TEXT   要作为排序依据的列
  --columns TEXT  要显示的列。选项：标识、用户名、帐户标识、
                  容量 (GB)、硬件标识、访客标识、主机标识
  -h, --help      显示此消息并退出。
```

* 将块卷故障转移到特定副本卷。
  ```
  # slcli block replica-failover --help
  用法：slcli block replica-failover [OPTIONS] VOLUME_ID

  选项：
  --replicant-id TEXT 副本卷的标识
  --immediate         立即故障转移到副本卷。
  -h, --help      显示此消息并退出。
```

* 从特定副本卷故障恢复块卷。
  ```
  # slcli block replica-failback --help
  用法：slcli block replica-failback [OPTIONS] VOLUME_ID

  选项：
  --replicant-id TEXT  副本卷的标识
  -h, --help           显示此消息并退出。
  ```
