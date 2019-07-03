---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: Block storage, auxiliary storage, missing routes, mpio, multipath, windows, troubleshooting

subcollection: BlockStorage

---
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# Windows 2012 R2 - mehrere iSCSI-Geräte
{: #troubleshootingWin12}

Wenn Sie mehr als zwei iSCSI-Geräte mit demselben Host verwenden, finden Sie diese Prozedur möglicherweise nützlich. Dies gilt insbesondere dann, wenn alle iSCSI-Verbindungen vom selben Speichermedium stammen.
Wenn in Disk Manager nur zwei Geräte angezeigt werden, müssen Sie auf jedem Serverknoten manuell eine Verbindung zu jedem Gerät im iSCSI-Initiator herstellen.
{:tsSymptoms}
{:tsResolve}


1. Öffnen Sie den Windows iSCSI-Initiator.
2. Klicken Sie auf der Registerkarte **Ziele** auf **Geräte**.

   ![Eigenschaften des iSCSI-Initiators](/images/win12-ts1.png)
3. Bestätigen Sie die Anzahl der Geräte, die angezeigt werden. Wenn Sie zwei Geräte statt vier sehen, die berechtigt wurden, fahren Sie mit dem nächsten Schritt fort.
4. Klicken Sie auf **Ziele** und dann auf **Verbinden**.
5. Wählen Sie **Multipath** und dann **Erweitert** aus.
6. Wählen Sie Microsoft iSCSI-Initiator als lokalen Adapter aus. Die Initiator-IP gehört zu Ihrem Server.
7. Wählen Sie die erste IP-Adresse aus, die in der IP-Zielliste des Zielportals angezeigt wird.

   ![Erweiterte EinstellungAdvanced Settings, IP-Adressen](/images/win12-ts3.png)

   Sie müssen diesen Schritt für alle aufgelisteten IP-Adressen wiederholen.
   {:tip}

8. Wählen Sie das Feld **CHAP aktivieren** aus und geben Sie die CHAP-ID und das Kennwort des Servers ein.

   ![Erweiterte Einstellungen, CHAP](/images/win12-ts4.png)
9. Klicken Sie auf **OK**.
10. Wiederholen Sie die Schritte 5 bis 9 für jede IP, die Sie in den iSCSI-Initiator eingegeben haben. Wenn Sie fertig sind, klicken Sie auf die Registerkarte **Geräte** und überprüfen Sie die Ergebnisse. Jede LUN, die Sie eingerichtet haben, sollte zwei Mal angezeigt werden.

    ![Registerkarte für Geräte](/images/win12-ts5.png)
11. Klicken Sie auf **OK**.
12. Öffnen Sie Disk Manager, und stellen Sie sicher, dass Ihre Laufwerke jetzt angezeigt werden.

    ![Gerätemanager](/images/win12-ts6.png)
