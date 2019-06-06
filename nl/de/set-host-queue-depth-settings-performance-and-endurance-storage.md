---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, performance tuning, host performance improvement,

subcollection: BlockStorage

---
{:external: target="_blank" .external}

# Einstellungen der Hostwarteschlangenlänge anpassen
{: #hostqueuesettings}

Für {{site.data.keyword.cloud}} wird für jedes Leistungstier ein maximaler Wert für die Ein-/Ausgabe-Warteschlangenlänge für Host und Anwendungen empfohlen.

| Performance-Tier | Maximale Hostwarteschlangenlänge |
|:------:|:------:|
| 0,25 IOPS pro GB | 8 |
| 2 IOPS pro GB | 24 |
| 4 IOPS pro GB | 56 |
{: caption="Empfohlene Warteschlangenlänge für jedes IOPS-Tier" caption-side="top"}

Die Hosteinstellung hat keine Auswirkung auf die Platten- und Controllerlatenz. Sie beeinflusst nur die vom Host und der Anwendung überwachte Latenz.

Eine Warteschlangenlänge über die ausgeführten Werte hinaus kann die E/A-Latenz erhöhen. Eine Warteschlangenlänge unter dem ausgeführten Wert kann die E/A-Leistung des Hosts verringern. Da jede Anwendung unterschiedlich ist, sind zum Erreichen der maximalen Speicherleistung Anpassungen und Beobachtungen erforderlich.

Die Hostwarteschlangenlänge wird in der Regel im Hostbusadaptertreiber oder Hypervisor angepasst, manchmal in der Anwendung. Standardwerte wie 32 oder 64 führen unter Umständen zu einer übermäßigen Host- oder Anwendungslatenz.

Verwenden Sie, wenn ein Host oder Hypervisor mehrere Performance-Tiers verwendet, die Warteschlangenlänge des schnellste Tiers und beobachten Sie die Latenz des langsamsten Performance-Tiers.

Falls die Latenz für das niedrigste Tier nicht mehr akzeptabel ist, passen Sie die Warteschlangenlänge so an, bis ein Ausgleich zwischen Latenz und Leistung für alle Tier erreicht ist.
