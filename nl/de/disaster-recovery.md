---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: Block Storage, inaccessible Primary volume, duplicate of a replica volume, Disaster Recovery, volume duplication, replication, failover, failback

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Disaster-Recovery - Failover mit einem nicht zugänglichen Primärdatenträger
{: #dr-inaccessible}

Wenn eine Betriebsunterbrechung oder eine Katastrophe einen Ausfall am primären Standort verursachen, können Kunden die folgenden Aktionen ausführen, um schnell am sekundären Standort auf ihre Daten zuzugreifen.

## Failover mit dem Duplikat eines Replikatdatenträgers am sekundären Standort

1. Melden Sie sich an der [{{site.data.keyword.cloud_notm}}-Konsole](https://{DomainName}/){: external} an und klicken Sie auf das **Menüsymbol** oben links. Wählen Sie **Klassische Infrastruktur** aus.
2. Klicken Sie auf **Speicher** > **{{site.data.keyword.blockstorageshort}}**.
3. Klicken Sie auf das Replikat der LUN in der Liste, um die zugehörige Seite **Details** anzuzeigen.
4. Blättern Sie auf der Seite **Details** nach unten und wählen Sie einen vorhandenen Snapshot aus. Klicken Sie anschießend auf **Aktionen** > **Duplikat**.
5. Nehmen Sie die erforderlichen Aktualisierungen für die Kapazität (Größe erhöhen) oder die IOPs für den neuen Datenträger vor.
6. Aktualisieren Sie bei Bedarf den Snapshotbereich für den neuen Datenträger.
7. Klicken Sie auf **Weiter**, um die Bestellung für das Duplikat abzuschicken.

Sobald der Datenträger erstellt wurde, kann er einem Host zugeordnet werden und Sie können Lese- und Schreiboperationen auf dem Datenträger ausführen. Während Daten vom ursprünglichen Datenträger auf den Duplikatdatenträger kopiert werden, wird auf der Detailseite ein Status mit der Information angezeigt, dass die Duplizierung in Bearbeitung ist. Wenn der Duplizierungsprozess abgeschlossen ist, ist der neue Datenträger unabhängig vom Original und kann mit Snapshots und Replikation normal verwaltet werden.

## Rückübertragung an den ursprünglichen primären Standort

Wenn Sie die Produktion an den ursprünglichen primären Standort zurückgeben möchten, müssen Sie die folgenden Schritte ausführen.

1. Melden Sie sich an der [{{site.data.keyword.cloud_notm}}-Konsole](https://{DomainName}/){: external} an und klicken Sie auf das **Menüsymbol** oben links. Wählen Sie **Klassische Infrastruktur** aus.
2. Klicken Sie auf **Speicher** > **{{site.data.keyword.blockstorageshort}}**.
3. Klicken Sie auf den LUN-Namen und erstellen Sie einen Snapshotplan (falls noch keiner vorhanden ist).

   Weitere Informationen zu Snapshotplänen finden Sie unter [Snapshots verwalten](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingSnapshots#addingschedule).
   {:tip}
4. Klicken Sie auf die Registerkarte **Replikat** und klicken Sie auf **Replikation kaufen**.
5. Wählen Sie einen vorhandenen Snapshotplan aus, den die Replikation befolgen soll. Die Liste enthält alle aktiven Snapshotpläne.
6. Klicken Sie auf **Position** und wählen Sie das Rechenzentrum aus, das der ursprüngliche Produktionsstandort war.
7. Klicken Sie auf **Weiter**.
8. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung aufgeben**.

Nach Abschluss der Replikation müssen Sie ein Duplikat des Datenträgers des neuen Replikats erstellen.
{:important}

1. Gehen Sie zurück zu **Speicher** > **{{site.data.keyword.blockstorageshort}}**.
2. Klicken Sie auf das Replikat der LUN in der Liste, um die zugehörige Seite **Details** anzuzeigen.
3. Blättern Sie auf der Seite **Details** nach unten und wählen Sie einen vorhandenen Snapshot aus. Klicken Sie anschießend auf **Aktionen** > **Duplikat**.
4. Nehmen Sie die erforderlichen Aktualisierungen für die Kapazität (Größe erhöhen) oder die IOPs für den neuen Datenträger vor.
5. Aktualisieren Sie bei Bedarf den Snapshotbereich für den neuen Datenträger.
6. Klicken Sie auf **Weiter**, um Ihre Bestellung des Duplikats abzusetzen.

Wenn der Duplizierungsprozess abgeschlossen ist, können Sie die Replikation und die Datenträger stornieren, die verwendet wurden, um die Daten wieder an den ursprünglichen primären Standort zu bringen. Das Duplikat wird zum primären Speicher, und die Replikation auf den ursprünglichen sekundären Standort kann wieder eingerichtet werden.
