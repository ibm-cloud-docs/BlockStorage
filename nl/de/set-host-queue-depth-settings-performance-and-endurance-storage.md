---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Empfehlung für Einstellungen der Hostwarteschlangenlänge

Für {{site.data.keyword.BluSoftlayer_full}} wird für jedes Leistungstier ein maximaler Wert für die Ein-/Ausgabe-Warteschlangenlänge für Host und Anwendungen empfohlen. Die Hosteinstellung hat keine Auswirkung auf die Datenträger- und Controllerlatenz, nur auf die vom Host und der Anwendung beobachtete Latenz.

<table align="center">
  <caption>Empfohlene Warteschlangenlänge für jedes IOPS-Tier</caption>
        <thead>
	    <tr>
		<th>Performance-Tier</th>
		<th>Maximale Hostwarteschlangenlänge</th>
	    </tr>
	</thead>
	<tbody>
   	    <tr>
		<td style="text-align: center; vertical-align: middle;">0,25 IOPS pro GB</td>
		<td style="text-align: center; vertical-align: middle;">8</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">2 IOPS pro GB</td>
		<td style="text-align: center; vertical-align: middle;">24</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">4 IOPS pro GB</td>
		<td style="text-align: center; vertical-align: middle;">56</td>
            </tr>
         </tbody>
</table>

Eine Warteschlangenlänge über der empfohlenen Anzahl kann die Ein-/Ausgabelatenz des Hosts erhöhen, während eine Warteschlangenlänge unter der empfohlenen Anzahl möglicherweise die Ein-/Ausgabeleistung des Hosts verringert. Da jede Anwendung unterschiedlich ist, sind zum Erreichen der maximalen Speicherleistung Anpassungen und Beobachtungen erforderlich.

Die Hostwarteschlangenlänge wird in der Regel im Hostbusadaptertreiber oder Hypervisor angepasst, manchmal in der Anwendung. Standardwerte wie 32 oder 64 führen unter Umständen zu einer übermäßigen Host- oder Anwendungslatenz.

Verwenden Sie, wenn ein Host oder Hypervisor mehrere Performance-Tiers verwendet, die Warteschlangenlänge des schnellste Tiers und beobachten Sie die Latenz des langsamsten Performance-Tiers. Falls die Latenz für das niedrigste Tier nicht mehr akzeptabel ist, passen Sie die Warteschlangenlänge so an, bis ein Ausgleich zwischen Latenz und Leistung für alle Tier erreicht ist.
