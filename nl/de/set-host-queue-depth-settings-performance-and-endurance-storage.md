---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Empfehlung für Einstellungen der Hostwarteschlangenlänge

{{site.data.keyword.BluSoftlayer_full}} empfiehlt für die einzelnen Performance-Tiers eine maximale Warteschlangenlänge für die Ein-/Ausgabe bei Hosts und Anwendungen. Die Hosteinstellung hat keine Auswirkung auf die Datenträger- und Controllerlatenz, nur auf die vom Host und der Anwendung beobachtete Latenz.

Eine Warteschlangenlänge über der empfohlenen Anzahl kann die Ein-/Ausgabelatenz des Hosts erhöhen, während eine Warteschlangenlänge unter der empfohlenen Anzahl möglicherweise die Ein-/Ausgabeleistung des Hosts verringert. Da jede Anwendung anders ist, sind Anpassung und Beobachtung zur Erreichung der maximalen Speicherleistung erforderlich.

Die Warteschlangenlänge des Hosts wird in der Regel im Treiber des Hostbusadapters oder im Hypervisor angepasst, manchmal in der Anwendung. Standardwerte wie 32 oder 64 führen unter Umständen zu einer übermäßigen Host- oder Anwendungslatenz.

Verwenden Sie, wenn ein Host oder Hypervisor mehrere Performance-Tiers verwendet, die Warteschlangenlänge des schnellste Tiers und beobachten Sie die Latenz des langsamsten Performance-Tiers. Passen Sie die Warteschlangenlänge an, wenn die Latenz des niedrigsten Tiers nicht akzeptabel ist, bis ein Ausgleich zwischen der Latenz und der Leistung aller Tiers erreicht ist.

<table align="center">
	<tbody>
		<tr>
			<td><strong>Performance-Tier</strong></td>
			<td style="text-align: center; vertical-align: middle;">0,25 IOPS pro GB</td>
			<td style="text-align: center; vertical-align: middle;">2 IOPS pro GB</td>
			<td style="text-align: center; vertical-align: middle;">4 IOPS pro GB</td>
		</tr>
		<tr>
			<td><strong>Maximale Hostwarteschlangenlänge</strong></td>
			<td style="text-align: center; vertical-align: middle;">8</td>
			<td style="text-align: center; vertical-align: middle;">24</td>
			<td style="text-align: center; vertical-align: middle;">56</td>
		</tr>
	</tbody>
</table>
