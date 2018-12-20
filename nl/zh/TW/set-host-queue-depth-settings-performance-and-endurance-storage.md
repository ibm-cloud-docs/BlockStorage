---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}

# 調整主機佇列深度設定

{{site.data.keyword.BluSoftlayer_full}} 建議針對每一個效能層級使用最大主機及應用程式輸入/輸出 (I/O) 佇列深度。

<table align="center">
  <caption>每一個 IOPS 層級的建議佇列深度</caption>
        <thead>
	    <tr>
		<th>效能層級</th>
		<th>最大主機佇列深度</th>
	    </tr>
	</thead>
	<tbody>
   	    <tr>
		<td style="text-align: center; vertical-align: middle;">每 GB 0.25 IOPS</td>
		<td style="text-align: center; vertical-align: middle;">8</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">每 GB 2 IOPS</td>
		<td style="text-align: center; vertical-align: middle;">24</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">每 GB 4 IOPS</td>
		<td style="text-align: center; vertical-align: middle;">56</td>
            </tr>
         </tbody>
</table>

主機設定不會影響磁碟及控制器延遲。它只會影響主機及應用程式所觀察到的延遲。

超過所列數目的佇列深度可能會增加主機 I/O 延遲，而少於所列數目的佇列深度可能會降低主機 I/O 效能。因為每一個應用程式都不同，所以需要調整及觀察才能達到最大儲存空間效能。

主機佇列深度通常是在主機匯流排配接卡驅動程式或 Hypervisor、而有時是在應用程式中進行調整。標準預設值（例如 32 或 64）可能會導致過多的主機或應用程式延遲。

如果有一台主機或 Hypervisor 正在使用多個效能層級，請在速度最快的層級使用佇列深度，然後在最慢的效能層級觀察延遲。

如果無法接受最低層級的延遲，則請調整佇列深度，直到達到所有層級的延遲與效能平衡為止。
