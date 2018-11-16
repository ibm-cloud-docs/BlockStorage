---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Duplikat eines Blockdatenträgers erstellen

Sie können ein Duplikat eines vorhandenen {{site.data.keyword.blockstoragefull}}s erstellen. Das Duplikat übernimmt standardmäßig die Kapazitäts- und Leistungsoptionen der Original-LUN bzw. des Originaldatenträgers und enthält bis zum Zeitpunkt eines Snapshots eine Kopie der Daten.   

Da das Duplikat auf den Daten eines Snapshots zu einem bestimmten Zeitpunkt basiert, wird ein Snapshotbereich auf dem Originaldatenträger benötigt, damit das Duplikat erstellt werden kann. Weitere Informationen zu Snapshots und zum Bestellen von Snapshotbereichen finden Sie in der [Snapshotdokumentation](snapshots.html).  

Duplikate können sowohl für **Primärdatenträger** als auch **Replikatdatenträger** erstellt werden. Das neue Duplikat wird in demselben Rechenzentrum wie der Originaldatenträger erstellt. Wenn Sie ein Duplikat aus einem Replikatdatenträger erstellen, wird der neue Datenträger in demselben Rechenzentrum erstellt wie der Replikatdatenträger.

Der Lese- und Schreibzugriff auf duplizierte Datenträger kann durch einen Host erfolgen, sobald der Speicher bereitgestellt wurde. Snapshots und Replikation sind dagegen erst zulässig, nachdem die Daten vollständig vom Original auf das Duplikat kopiert wurden.

Sobald die Datenkopie abgeschlossen ist, kann das Duplikat als komplett vom Original unabhängiger Datenträger verwaltet und verwendet werden.

Diese Funktion steht an den meisten Standorten zur Verfügung. Klicken Sie [hier](new-ibm-block-and-file-storage-location-and-features.html), um eine Liste mit den verfügbaren Rechenzentren anzuzeigen.

Als Benutzer mit einem dedizierten Konto für {{site.data.keyword.containerlong}} finden Sie in der [{{site.data.keyword.containerlong_notm}}-Dokumentation](/docs/containers/cs_storage_file.html#backup_restore) Informationen zu Optionen für die Duplizierung eines Datenträgers.
{:tip}

Nachstehend einige gängige Anwendungen für duplizierte Datenträger:
- **Disaster-Recovery-Tests:** Erstellen Sie ein Duplikat des Replikatdatenträgers, um zu überprüfen, ob die Daten intakt sind und im Fall einer Katastrophe ohne Unterbrechung der Replikation verwendet werden können.
- **Goldene Kopie:** Verwenden Sie einen Speicherdatenträger als goldene Kopie, um damit mehrere Instanzen für verschiedene Anwendungen zu erstellen.
- **Datenaktualisierung:** Erstellen Sie eine Kopie Ihrer Produktionsdaten, um diese zu Testzwecken an die nicht für die Produktion verwendete Umgebung anzuhängen.
- **Wiederherstellung aus Snapshot:** Stellen Sie Daten auf dem Originaldatenträger mit bestimmten Dateien/Datumsangaben aus einem Snapshot wieder her, ohne den gesamten Originaldatenträger mit einer Snapshotwiederherstellungsfunktion zu überschreiben.
- **Entwicklung/Test:** Erstellen Sie gleichzeitig bis zu vier simultane Duplikate eines Datenträgers, um duplizierte Daten zu Entwicklungs- und Testzwecken zu erstellen.
- **Größenänderung des Speichers:** Erstellen Sie einen Datenträger mit der neuen Größe und/oder den IOPS-Raten, ohne die Daten verschieben zu müssen.  

Sie können einen duplizierten Datenträger über das [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} erstellen:


## Duplikat von einem bestimmten Datenträger in der Speicherliste erstellen

1. Navigieren Sie zur {{site.data.keyword.blockstorageshort}}-Liste:
    - Klicken Sie im Kundenportal auf **Speicher** > **{{site.data.keyword.blockstorageshort}}**.
    - Klicken Sie im {{site.data.keyword.BluSoftlayer_full}}-Katalog auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.blockstorageshort}}**.
2. Wählen Sie in der Liste einen Datenträger aus und klicken Sie auf **Aktionen** > **Duplizierte LUN (Datenträger)**.
3. Wählen Sie Ihre Snapshotoption aus:
    - Wenn Sie von einem **Nicht-Replikat**-Datenträger bestellen:
      - Wählen Sie **Aus neuem Snapshot erstellen** aus: Damit wird ein neuer Snapshot für das Duplikat erstellt. Verwenden Sie diese Option, wenn der Datenträger keine aktuellen Snapshots aufweist oder wenn Sie zu diesem Zeitpunkt ein Duplikat erstellen wollen.<br/>
      - Wählen Sie **Aus letztem Snapshot erstellen** aus: Damit wird ein Duplikat aus dem letzten für diesen Datenträger vorhandenen Snapshot erstellt.
    - Bei einer Bestellung über einen **Replikat**-Datenträger: Die einzige Option für einen Snapshot ist die Verwendung des neuesten verfügbaren Snapshots.
4. Der Speichertyp und der Speicherort bleiben mit dem ursprünglichen Datenträger identisch.
5. Stündliche oder monatliche Abrechnung: Sie können wählen, ob die duplizierte LUN mit stündlicher oder monatlicher Abrechnung bereitgestellt wird. Der Abrechnungstyp für den Originaldatenträger wird automatisch ausgewählt. Wenn Sie jedoch für Ihren neuen duplizierten Speicher einen anderen Abrechnungstyp wählen möchten, können Sie das hier tun.
5. Bei Bedarf können Sie IOPS oder ein IOPS-Tier für den neuen Datenträger angeben. Die IOPS-Bezeichnung wird für den Originaldatenträger standardmäßig festgelegt. Die verfügbaren Kombinationen aus Leistung und Größe werden angezeigt.
    - Wenn Ihr Originaldatenträger ein Endurance-Tier mit 0,25 IOPS ist, können Sie keine neue Auswahl treffen.
    - Wenn Ihr Originaldatenträger ein Endurance-Tier mit 2, 4 oder 10 IOPS ist, können Sie für den neuen Datenträger einen beliebigen Wert zwischen diesen Tiers auswählen.
6. Bei Bedarf können Sie die Größe des neuen Datenträgers aktualisieren, sodass er größer als der Originaldatenträger ist. Die Größe des Originaldatenträgers wird standardmäßig festgelegt.

   {{site.data.keyword.blockstorageshort}} kann bis auf das Zehnfache der ursprünglichen Größe des Datenträgers erhöht werden.{:tip}
7. Bei Bedarf können Sie den Snapshotbereich für den neuen Datenträger aktualisieren und mehr, weniger oder keinen Snapshotbereich hinzufügen. Der Snapshotbereich wird für den Originaldatenträger standardmäßig festgelegt.
8. Klicken Sie auf **Weiter**, um Ihre Bestellung abzusetzen.



## Duplikat aus einem bestimmten Snapshot erstellen

1. Navigieren Sie zur {{site.data.keyword.blockstorageshort}}-Liste:
2. Klicken Sie in der Liste auf **eine LUN / einen Datenträger**, um die Detailseite anzuzeigen. (Es kann sich um einen Replikatdatenträger oder einen Nichtreplikatdatenträger handeln.)
3. Blättern Sie abwärts, wählen Sie in der Liste auf der Detailseite einen vorhandenen Snapshot aus und klicken Sie auf **Aktionen** > **Duplikate**.   
4. Speichertyp (Endurance oder Performance) und Position bleiben mit dem Originaldatenträger identisch.
5. Die verfügbaren Kombinationen aus Leistung und Größe werden angezeigt. Die IOPS-Bezeichnung wird für den Originaldatenträger standardmäßig festgelegt. Sie können IOPS oder ein IOPS-Tier für den neuen Datenträger angeben.
    - Wenn Ihr Originaldatenträger ein Endurance-Tier mit 0,25 IOPS ist, können Sie keine neue Auswahl treffen.
    - Wenn Ihr Originaldatenträger ein Endurance-Tier mit 2, 4 oder 10 IOPS ist, können Sie für den neuen Datenträger einen beliebigen Wert zwischen diesen Tiers auswählen.
6. Bei Bedarf können Sie die Größe des neuen Datenträgers aktualisieren, sodass er größer als der Originaldatenträger ist. Die Größe des Originaldatenträgers wird standardmäßig festgelegt.

   {{site.data.keyword.blockstorageshort}} kann bis auf das Zehnfache der ursprünglichen Größe des Datenträgers erhöht werden
. {:tip}
7. Bei Bedarf können Sie den Snapshotbereich für den neuen Datenträger aktualisieren und mehr, weniger oder keinen Snapshotbereich hinzufügen. Der Snapshotbereich wird für den Originaldatenträger standardmäßig festgelegt.
8. Klicken Sie auf **Weiter**, um Ihre Bestellung des Duplikats abzusetzen.


## Duplizierten Datenträger verwalten

Während die Daten vom Originaldatenträger auf das Duplikat kopiert werden, wird auf der Detailseite der Status angezeigt, der angibt, dass die Duplizierung in Bearbeitung ist. In dieser Zeit können Sie eine Verbindung zu einem Host herstellen, von dem Datenträger lesen und auf ihn schreiben, jedoch keine Snapshotpläne erstellen. Wenn der Duplizierungsprozess abgeschlossen ist, ist der neue Datenträger unabhängig vom Original und kann mit Snapshots und Replikation normal verwaltet werden.
