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

# Windows 2012 R2 - più dispositivi iSCSI
{: #troubleshootingWin12}

Se utilizzi più di due dispositivi iSCSI con lo stesso host, potresti trovare utile questa procedura; specialmente se tutte le connessioni iSCSI provengono dallo stesso dispositivo di archiviazione.
Se visualizzi solo due dispositivi in Gestione del disco, devi collegare manualmente ogni dispositivo nell'iniziatore iSCSI su ogni nodo del server.
{:tsSymptoms}
{:tsResolve}


1. Apri l'iniziatore iSCSI di Windows.
2. Nella scheda **Destinazioni**, fai clic su **Dispositivi**.

   ![Proprietà iniziatore iSCSI](/images/win12-ts1.png)
3. Conferma il numero di dispositivi mostrato. Se vedi due dispositivi, invece dei quattro che sono stati autorizzati, continua al passo successivo.
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
