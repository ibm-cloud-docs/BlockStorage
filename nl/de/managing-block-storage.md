---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.blockstorageshort}} verwalten

Sie können Ihre {{site.data.keyword.blockstoragefull}}-Datenträger über das [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} verwalten. In diesem Artikel finden Sie Anweisungen zu den häufigsten Aufgaben.

## Details zu bereitgestellter {{site.data.keyword.blockstorageshort}}-LUN anzeigen

Sie können eine Zusammenfassung der wichtigsten Informationen zu der ausgewählten Speicher-LUN einschließlich zusätzlicher Snapshot- und Replikationsfunktionen anzeigen, die zu dem Speicher hinzugefügt wurden.

1. Klicken Sie auf **Speicher**, **{{site.data.keyword.blockstorageshort}}**.
2. Klicken Sie in der Liste auf den entsprechenden LUN-Namen.

## Hosts für den Zugriff auf {{site.data.keyword.blockstorageshort}} autorisieren

'Autorisierte' Hosts sind Hosts, denen Zugriffsrechte für eine bestimmte LUN erteilt wurden. Ohne eine Hostberechtigung können Sie nicht auf Speicher Ihres Systems zugreifen oder diesen verwenden. Durch das Autorisieren eines Hosts für den Zugriff auf Ihre LUN werden der Benutzername, das Kennwort und der qualifizierte iSCSI-Name (IQN) generiert, die zum Anhängen der MPIO-iSCSI-Verbindung erforderlich sind.

**Hinweis**: Sie können nur solche Hosts autorisieren und verbinden, die sich in demselben Rechenzentrum wie Ihr Speicher befinden. Wenn Sie mehrere Konten haben, können Sie einen Host von einem Konto nicht für den Zugriff auf Ihren Speicher auf einem anderen Konto autorisieren.

1. Klicken Sie auf **Speicher** -> **{{site.data.keyword.blockstorageshort}}** und anschließend auf Ihren LUN-Namen.
2. Blättern Sie zum Bereich 'Autorisierte Hosts' der Seite.
3. Klicken Sie rechts auf der Seite auf den Link **Host autorisieren**. Wählen Sie die Hosts aus, die auf die entsprechende LUN zugreifen können.

 

## Liste der für eine {{site.data.keyword.blockstorageshort}}-LUN autorisierten Hosts anzeigen

Führen Sie die folgenden Schritte aus, um die Liste der für eine LUN autorisierten Hosts anzuzeigen.

1. Klicken Sie auf **Speicher** -> **{{site.data.keyword.blockstorageshort}}** und anschließend auf Ihren LUN-Namen.
2. Blättern Sie auf der Seite nach unten bis zum Bereich 'Autorisierte Hosts'.

Hier werden die Liste der Hosts, die gerade für den Zugriff auf die LUN autorisiert sind, und - speziell für iSCSI - die Authentifizierungsdaten angezeigt, die zum Herstellen einer Verbindung erforderlich sind: Benutzername, Kennwort und IQN-Host. Die Zieladresse befindet sich oben auf der Seite mit den Speicherdetails. Für NFS wird sie als DNS-Name beschrieben, für iSCSI als IP-Adresse des Erkennungszielportals.

 

## {{site.data.keyword.blockstorageshort}} anzeigen, für den ein Host autorisiert ist

Sie können die LUNs anzeigen, auf die ein Host Zugriff hat, sowie die Informationen, die zum Herstellen einer Verbindung erforderlich sind – LUN-Name, Speichertyp, Zieladresse, Kapazität und Position:

1. Klicken Sie im [{{site.data.keyword.slportal}}](http://control.softlayer.com/){:new_window} auf **Geräte** -> **Geräteliste** und klicken Sie auf das entsprechende Gerät.
2. Wählen Sie die Registerkarte **Speicher** aus.

Dann wird Ihnen eine Liste der Speicher-LUNs angezeigt, auf die der entsprechende Host Zugriff hat, gruppiert nach Speichertyp (Block, Datei, andere). Sie können über die entsprechenden Aktionsmenüs zusätzlichen Speicher autorisieren oder den Zugriff entfernen.

 

## {{site.data.keyword.blockstorageshort}} anhängen und abhängen

In den folgenden Artikeln finden Sie Details zum zum Anhängen und Abhängen von {{site.data.keyword.blockstorageshort}} von einem Host.

- [{{site.data.keyword.blockstorageshort}} unter Linux](accessing_block_storage_linux.html)

- [{{site.data.keyword.blockstorageshort}} unter Microsoft Windows](accessing-block-storage-windows.html)

 

## Zugriff eines Hosts auf {{site.data.keyword.blockstorageshort}} widerrufen

Wenn Sie den Zugriff auf eine bestimmte Speicher-LUN über einen Host stoppen wollen, können Sie den Zugriff widerrufen. Beim Widerrufen des Zugriffs wird die Hostverbindung aus der LUN gelöscht und weder das Betriebssystem noch die Anwendungen können mit der LUN kommunizieren.

**Hinweis**: Hängen Sie zur Vermeidung von Problemen die Speicher-LUN von Ihrem Betriebssystem ab, bevor Sie den Zugriff widerrufen, um fehlende Laufwerke oder Datenbeschädigung zu vermeiden.

Sie können den Zugriff auf den Speicher über die Geräteliste oder die Speicheransichten widerrufen.

### Zugriff über die Geräteliste widerrufen

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Geräte** - **Geräteliste** und doppelklicken Sie auf das entsprechende Gerät.
2. Wählen Sie die Registerkarte **Speicher** aus.
3. Dann wird Ihnen eine Liste der Speicher-LUNs angezeigt, auf die der entsprechende Host Zugriff hat, gruppiert nach Speichertyp (Block, Datei, andere). Wählen Sie das entsprechende Aktionsmenü neben der LUN aus, von der Sie den Zugriff widerrufen wollen, und klicken Sie auf **Zugriff widerrufen**.
4. Sie werden gefragt, ob Sie den Zugriff auf eine LUN widerrufen wollen, weil die Aktion nicht rückgängig gemacht werden kann. Klicken Sie auf **Ja**, um den LUN-Zugriff zu widerrufen, oder auf **Nein**, um die Aktion abzubrechen.

**Hinweis**: Wenn Sie mehrere LUNs von einem bestimmten Host trennen wollen, müssen Sie die Aktion zum Widerrufen des Zugriffs für jede einzelne LUN wiederholen.


### Zugriff über die Speicheransicht widerrufen

1. Klicken Sie auf **Speicher**, **{{site.data.keyword.blockstorageshort}}** und wählen Sie die LUN aus, von der Sie den Zugriff widerrufen wollen.
2. Blättern Sie zum Bereich **Autorisierte Hosts** der Seite.
3. Klicken Sie neben dem Host, dessen Zugriff widerrufen werden soll, auf den Dropdown-Pfeil **Aktionen**, und wählen Sie **Zugriff widerrufen** aus.
4. Sie werden gefragt, ob Sie den Zugriff auf eine LUN widerrufen wollen, weil die Aktion nicht rückgängig gemacht werden kann. Klicken Sie auf **Ja**, um den LUN-Zugriff zu widerrufen, oder auf **Nein**, um die Aktion abzubrechen.

**Hinweis**: Wenn Sie mehrere Hosts von einer bestimmten LUN trennen wollen, müssen Sie die Aktion zum Widerrufen des Zugriffs für jeden einzelnen Host wiederholen.

 

## Speicher-LUN abbrechen

Wenn Sie eine bestimmte LUN nicht mehr brauchen, können Sie sie abbrechen. Zum Abbrechen einer Speicher-LUN müssen Sie zunächst den Zugriff von allen Hosts widerrufen.

1. Klicken Sie auf **Speicher**, **{{site.data.keyword.blockstorageshort}}**.
2. Klicken Sie auf den Dropdown-Pfeil **Aktionen** der LUN, die abgebrochen werden soll, und wählen Sie **{{site.data.keyword.blockstorageshort}} abbrechen** aus.
3. Sie werden gefragt, ob Sie die LUN sofort oder an dem Stichtag ihrer Bereitstellung löschen wollen. Klicken Sie auf **Weiter** oder **Schließen**.
**Hinweis**: Wenn Sie die Option zum Abbrechen der LUN an ihrem Stichtag auswählen, können Sie die Abbruchanforderung vor ihrem Stichtag annullieren.
4. Aktivieren Sie das Kontrollkästchen **Bestätigung** und klicken Sie auf **Bestätigen**.

 

