---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: BlockStorage

---
{:new_window: target="_blank"}_
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}

# 管理快照
{: #managingSnapshots}

## 创建快照安排

通过快照安排，您可以决定创建存储卷的时间点引用的频率和时间。每个存储卷最多可以有 50 个快照。安排通过 [{{site.data.keyword.slportal}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){:new_window} 的**存储** > **{{site.data.keyword.blockstorageshort}}** 选项卡进行管理。

如果在初始供应存储卷期间未购买快照空间，那么必须首先购买快照空间，然后才能设置初始安排。
有关更多信息，请参阅[订购快照](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots)。
{:important}

### 添加快照安排
{: #addingschedule}

可以将快照安排的时间间隔设置为每小时、每天和每周，每种时间间隔使用不同的保留周期。快照的最大限制为每个存储卷 50，可以是每小时、每天和每周安排以及手动快照的混合。

1. 单击存储卷，单击**操作**，然后单击**安排快照**。
2. 在“新建安排快照”窗口中，您可以从三个不同的快照频率中进行选择。使用这三项的任意组合来创建综合快照安排。
   - 每小时
      - 指定每小时内应生成快照的时刻（分钟）。缺省值为当前分钟。
      - 指定在丢弃最旧快照之前要保留的每小时快照数。
   - 每天
      - 指定要生成快照的小时和分钟。缺省值为当前小时和分钟。
      - 选择在丢弃最旧快照之前要保留的每天快照数。
   - 每周
      - 指定应生成快照的星期几、小时和分钟。缺省值为当天、当前小时和分钟。
      - 选择在丢弃最旧快照之前要保留的每周快照数。
3. 单击**保存**，然后创建具有不同频率的其他安排。如果安排的快照总数超过 50 个，那么您将收到一条警告消息，并且无法保存该安排。

在生成快照时，会在**详细信息**页面的**快照**部分中显示快照列表。

您还可以通过 SLCLI 使用以下命令来查看快照安排的列表。
```
# slcli block snapshot-schedule-list --help
用法：slcli block snapshot-schedule-list [OPTIONS] VOLUME_ID

选项：
  -h, --help  显示此消息并退出。
```
{:codeblock}

## 生成手动快照

手动快照可以在应用程序升级或维护期间在各种时间点生成。您还可以针对在应用程序级别暂时停用的多台服务器生成快照。

每个存储卷的最大快照数限制为 50。

1. 单击存储卷。
2. 单击**操作**。
3. 单击**生成手动快照**。
这将生成快照，并且快照会显示在**详细信息**页面的**快照**部分中。其安排会显示为“手动”。

或者，您可以通过 SLCLI 使用以下命令来创建快照。
```
# slcli block snapshot-create --help
用法：slcli block snapshot-create [OPTIONS] VOLUME_ID

选项：
  -n, --notes TEXT  要对新快照设置的注释。
  -h, --help        显示此消息并退出。
```
{:codeblock}

## 列出所有带“已用空间信息”和“管理”功能的快照

在**详细信息**页面上，可以查看保留的快照和已使用空间量的列表。管理功能（编辑安排和添加更多空间）是在“详细信息”页面上使用该页面上各部分中的**操作**菜单或链接来执行的。

## 查看保留快照的列表

保留的快照数基于您设置安排时在**保留最后一项**字段中输入的数字。您可以在**快照**部分下查看已生成的快照。快照按安排列出。

或者，您可以在 SLCLI 中使用以下命令来显示可用的快照。
```
# slcli block snapshot-list --help
用法：slcli block snapshot-list [OPTIONS] VOLUME_ID

选项：
  --sortby TEXT   要作为排序依据的列
  --columns TEXT  要显示的列。选项：id、name、created、size_bytes
  -h, --help      显示此消息并退出。
```
{:codeblock}

## 查看已使用的快照空间量

**详细信息**页面上的饼图显示已使用的空间量以及剩余的空间量。在达到空间阈值（75%、90% 和 95%）时，您会收到通知。

## 更改卷的快照空间量

您可能需要向先前没有任何快照空间或者可能需要额外快照空间的卷添加快照空间。根据需要，可以添加 5 GB 到 4,000 GB。

只能增大快照空间。不能减小快照空间。因此，在确定需要的空间量之前，您可以选择较小的空间量。请记住，自动快照和手动快照共享该空间。
{:note}

通过**存储** > **{{site.data.keyword.blockstorageshort}}**，可更改快照空间。

1. 单击存储卷，单击**操作**，然后单击**添加更多快照空间**。
2. 从提示中的各种大小中进行选择。大小范围通常从 0 到卷大小。
3. 单击**继续**。
4. 输入您拥有的任何促销码，然后单击**重新计算**。缺省情况下，已填写“此订单的费用”和“订单复查”字段。
5. 单击**我已阅读主服务协议...** 复选框，然后单击**下订单**。更多快照空间将在几分钟后供应。

## 在达到快照空间限制以及删除快照时接收通知

达到三个不同的空间阈值（75%、90% 和 95%）时，会通过支持凭单向帐户的主用户发送通知。

- 达到 **75% 容量**时，会发送警告来指示快照空间使用率已超过 75%。如果您留意到警告并手动添加空间或删除保留的不需要的快照，那么将记录相应操作并关闭该凭单。如果不执行任何操作，那么必须手动确认该凭单，然后该凭单将关闭。
- 达到 **90% 容量**时，会发送第二个警告来指示快照空间使用率已超过 90%。与达到 75% 容量时一样，如果执行必要的操作来减少使用的空间，那么将记录相应操作并关闭该凭单。如果不执行任何操作，那么必须手动确认该凭单，然后该凭单将关闭。
- 达到 **95% 容量**时，会发送最后警告。如果未执行任何操作来使空间使用率低于该阈值，那么将生成通知并执行自动删除，以便未来可以创建快照。这将删除安排的快照，从最旧的快照开始删除，直到使用率低于 95% 为止。每次使用率超过 95% 时，都会继续删除快照，直到使用率低于该阈值为止。如果以手动方式增大空间或删除快照，那么将重置警告，如果再次超过阈值，将重新发出警告。如果不执行任何操作，那么此通知是您收到的唯一警告。

## 删除快照安排

通过**存储** > **{{site.data.keyword.blockstorageshort}}**，可取消快照安排。

1. 单击**详细信息**页面上**快照安排**框架中要删除的安排。
2. 单击要删除的安排旁边的复选框，然后单击**保存**。<br />

如果要使用复制功能，请确保要删除的安排不是复制所使用的安排。有关删除复制安排的更多信息，请参阅[复制数据](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication)。
{:important}

## 删除快照

可以手动除去不再需要的快照，以释放空间供未来快照使用。通过**存储** > **{{site.data.keyword.blockstorageshort}}**，可执行删除。

1. 单击存储卷，然后向下滚动到**快照**部分以查看现有快照的列表。
2. 单击特定快照旁边的**操作**，然后单击**删除**以删除快照。此删除操作不会影响同一安排上的任何未来或过去的快照，因为快照之间不存在依赖关系。

未在门户网站中手动删除的手动快照会在达到空间限制时自动删除（先删除最旧的快照）。

您可以通过 SLCLI 使用以下命令来删除卷。
```
# slcli block snapshot-delete
用法：slcli block snapshot-delete [OPTIONS] SNAPSHOT_ID

选项：
  -h, --help  显示此消息并退出。
```
{:codeblock}


## 使用快照将存储卷复原到特定时间点

由于用户错误或数据损坏，您可能需要将存储卷恢复到特定时间点。

复原卷会导致所有在用于复原的快照之后拍摄的快照均会被删除。
{:important}

1. 从主机卸装并拆离存储卷。
   - [在 Linux 上连接到 iSCSI LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#unmounting)
   - [在 Microsoft Windows 上连接到 iSCSI LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows#unmounting)
2. 单击 [{{site.data.keyword.slportal}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){:new_window} 中的**存储** > **{{site.data.keyword.blockstorageshort}}**。
3. 向下滚动并单击要复原的卷。**详细信息**页面的**快照**部分将显示所有保存的快照及其大小和创建日期的列表。
4. 单击要使用的快照旁边的**操作**，然后单击**复原**。<br/>

   完成复原会导致那些在生成快照后创建或修改的数据丢失。发生这种数据丢失的原因是，存储卷会恢复到在生成快照时所处的状态。
   {:note}
5. 单击**是**以启动复原。

   应该会在页面中收到一条消息，声明正在使用所选快照复原卷。此外，{{site.data.keyword.blockstorageshort}} 上的相应卷旁边会显示一个图标，指示正在执行活动事务。将鼠标悬停在该图标上将生成一个用于显示事务的窗口。事务完成后，该图标会消失。
   {:note}
6. 安装存储卷并将其重新连接到主机。
   - [在 Linux 上连接到 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
   - [在 CloudLinux 上连接到 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
   - [在 Microsoft Windows 上连接到 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)

或者，在从该主机拆离卷后，您可以在 SLCLI 中使用以下命令来启动复原。
```
# slcli block snapshot-restore --help
用法：slcli block snapshot-restore [OPTIONS] VOLUME_ID

选项：
  -s, --snapshot-id TEXT  要用于复原块卷的快照的标识
  -h, --help              显示此消息并退出。
```
{:codeblock}  

复原完成后，请安装存储卷并将其重新连接到该主机。
