---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-05"

---

{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Windows 2012 R2 - più dispositivi iSCSI
{: #troubleshootingWin12}

Se utilizzi più di due dispositivi iSCSI, potresti trovare utile questa procedura; specialmente se tutte le 4 assegnazioni iSCSI provengono dallo stesso dispositivo di archiviazione. Se visualizzi solo due dispositivi in Gestione del disco, devi collegare manualmente ogni dispositivo nell'iniziatore iSCSI su ogni nodo del server.

1. Apri l'iniziatore iSCSI di Windows.
2. Fai clic sulla scheda **Destinazioni** e poi su **Dispositivi**.

   ![Proprietà iniziatore iSCSI](/images/win12-ts1.png)
3. Conferma il numero di dispositivi mostrato. Se vedi 2 dispositivi, invece dei 4 che sono stati autorizzati, continua al passo successivo.
4. Fai clic su **Destinazioni** e quindi su **Connetti**.
5. Seleziona **Multipath** e quindi **Avanzate**.
6. Seleziona l'iniziatore iSCSI di Microsoft come adattatore locale. L'IP dell'iniziatore appartiene al tuo server.
7. Seleziona il primo indirizzo IP mostrato nell'elenco IP portale destinazione.

   ![Impostazioni avanzate, indirizzi IP](/images/win12-ts3.png)

   Devi ripetere questo passo per tutti gli indirizzi IP elencati.
   {:tip}

8. Seleziona la casella **Attiva CHAP** e immetti la password e l'ID CHAP del server.

   ![Impostazioni avanzate, CHAP](/images/win12-ts4.png)
9. Fai clic su **OK**.
10. Ripeti i passi da 5 a 9 per ogni IP che hai immesso nell'iniziatore iSCSI. Quando hai finito, fai clic sulla scheda **Dispositivi** e controlla i risultati. Aspettati di vedere ogni LUN che hai configurato elencato due volte.

    ![Scheda Dispositivi](/images/win12-ts5.png)
11. Fai clic su **OK**.
12. Apri la Gestione del disco e verifica che le tue unità vengano ora visualizzate.

    ![Gestione dispositivi](/images/win12-ts6.png)
