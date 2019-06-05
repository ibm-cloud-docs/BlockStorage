---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: MPIO iSCSI LUNS, iSCSI Target, MPIO, multipath, block storage, LUN, mounting, mapping secondary storage

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}

# Connessione ai LUN iSCSI su Microsoft Windows
{: #mountingWindows}

Prima di iniziare, assicurarti che l'host che sta accedendo al volume {{site.data.keyword.blockstoragefull}} sia stato autorizzato tramite [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.

1. Dalla pagina di elenco {{site.data.keyword.blockstorageshort}}, individua il nuovo volume e fai clic su **Actions**. Fai clic su **Authorize Host**.
2. Dall'elenco, seleziona l'host o gli host che devono accedere al volume e fai clic su **Submit**.

In alternativa, puoi autorizzare l'host tramite la CLI SL.
```
# slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  --help                    Show this message and exit.
```
{:codeblock}

## Montaggio di volumi {{site.data.keyword.blockstorageshort}}
{: #mountWin}

Viene qui di seguito indicata la procedura necessaria per connettere un'istanza di elaborazione {{site.data.keyword.cloud}} basata su Windows a un LUN (logical unit number) iCSCI (internet Small Computer System Interface) MPIO (multipath input/output). L'esempio è basato su Windows Server 2012. La procedura può essere regolata per altre versioni di Windows in base alla documentazione del fornitore del sistema operativo.

### Configurazione della funzione MPIO

1. Avvia Gestione server e passa a **Gestisci**, **Aggiungi ruoli e funzionalità**.
2. Fai clic su **Avanti** al menu Funzionalità.
3. Scorri verso il basso e seleziona **Multipath I/O**.
4. Fai clic su **Installa** per installare MPIO sul server host.
![Aggiunta di ruoli e funzionalità in Gestione server](/images/Roles_Features.png)

### Aggiunta del supporto iSCSI per MPIO

1. Apri la finestra Proprietà MPIO facendo clic su **Start**, puntando a **Strumenti di amministrazione** e facendo clic su **MPIO**.
2. Fai clic su **Individua percorsi multipli**.
3. Seleziona la casella di spunta **Aggiungi supporto per dispositivi iSCSI** e fai quindi clic su **Aggiungi**. Quando ti viene richiesto di riavviare il computer, fai clic su **Sì**.

In Windows Server 2008, l'aggiunta del supporto per iSCSI consente a un Modulo specifico dispositivo Microsoft (MSDSM, Microsoft Device Specific Module) di richiedere i diritti su tutti i dispositivi iSCSI per MPIO, operazione che richiede preliminarmente una connessione a una Destinazione iSCSI.
{:note}

### Configurazione dell'iniziatore iSCSI

1. Avvia l'iniziatore iSCSI da Gestione server e seleziona **Strumenti**, **Iniziatore iSCSI**.
2. Fai clic sulla scheda **Configurazione**.
    - Il campo Nome iniziatore potrebbe già essere compilato con una voce simile a `iqn.1991-05.com.microsoft:`.
    - Fai clic su **Modifica** per sostituire i valori esistenti con il tuo nome qualificato iSCSI (IQN, iSCSI Qualified Name).
    ![Proprietà iniziatore iSCSI](/images/iSCSI.png)

      Il nome IQN può essere ottenuto dalla schermata Dettagli di {{site.data.keyword.blockstorageshort}} nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.
      {: tip}

    - Fai clic sulla scheda **Individuazione** e fai clic su **Individua portale**.
    - Immetti l'indirizzo IP della tua destinazione iSCSI e lascia la porta al valore predefinito di 3260.
    - Fai clic su **Avanzate** per aprire la finestra Impostazioni avanzate.
    - Seleziona **Attiva accesso CHAP** per attivare l'autenticazione CHAP.
    ![Attiva accesso CHAP](/images/Advanced_0.png)

    I campi Nome e Segreto destinazione sono sensibili a maiuscole/minuscole.
    {:important}
         - Nel campo **Nome**, elimina qualsiasi voce esistente e immetti il nome utente dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.
         - Nel campo **Segreto destinazione**, immetti la password dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.
    - Fai clic su **OK** nelle finestre **Impostazioni avanzate** e **Individua portale destinazione** per tornare alla schermata Proprietà iniziatore iSCSI principale. Se ricevi degli errori di autenticazione, controlla le voci di nome utente e password.
    ![Destinazione inattiva](/images/Inactive_0.png)

    Il nome della tua destinazione appare nella sezione Destinazioni individuate con uno stato `Inactive`.
    {:note}


### Attivazione della destinazione

1. Fai clic su **Connetti** per stabilire una connessione alla destinazione.
2. Seleziona la casella di spunta **Consenti percorsi multipli** per abilitare il Multipath I/O alla destinazione.
<br/>
   ![Consenti percorsi multipli](/images/Connect_0.png)
3. Fai clic su **Avanzate** e seleziona **Attiva accesso CHAP**.
</br>
   ![Attiva CHAP](/images/chap_0.png)
4. Immetti il nome utente nel campo Nome e immetti la password nel campo Segreto destinazione.

   I valori dei campi Nome e Segreto destinazione possono essere ottenuti dalla schermata Dettagli di {{site.data.keyword.blockstorageshort}}.
   {:tip}
5. Fai clic su **OK** finché non viene visualizzata la finestra **Proprietà iniziatore iSCSI**. Lo stato della destinazione nella sezione **Destinazioni individuate** cambia da **Inattivo** a **Connesso**.
![Stato Connesso](/images/Connected.png)


### Configurazione di MPIO nell'iniziatore iSCSI

1. Avvia l'iniziatore iSCSI e, nella scheda Destinazioni, fai clic su **Proprietà**.
2. Fai clic su **Aggiungi sessione** nella finestra Proprietà per aprire la finestra Connessione alla destinazione.
3. Nella finestra di dialogo Connessione alla destinazione, seleziona la casella di spunta **Consenti percorsi multipli** e fai clic su **Avanzate**.
  ![Destinazione](/images/Target.png)

4. Nella finestra Impostazioni avanzate ![Impostazioni](/images/Settings.png)
   - Nell'elenco Adattatore locale, seleziona Iniziatore iSCSI Microsoft.
   - Nell'elenco IP iniziatore, seleziona l'indirizzo IP dell'host.
   - Nell'elenco IP portale di destinazione, seleziona l'IP dell'interfaccia del dispositivo.
   - Fai clic sulla casella di spunta **Attiva accesso CHAP**
   - Immetti i valori Nome e Segreto destinazione ottenuti dal portale e fai clic su **OK**.
   - Fai clic su **OK** nella finestra Connessione alla destinazione per tornare alla finestra Proprietà.

5. Fai clic su **Proprietà**. Nella finestra di dialogo Proprietà, fai nuovamente clic su **Aggiungi sessione** per aggiungere il secondo percorso.
6. Nella finestra di dialogo Connessione alla destinazione, seleziona la casella di spunta **Consenti percorsi multipli**. Fai clic su **Avanzate**.
7. Nella finestra Impostazioni avanzate,
   - Nell'elenco Adattatore locale, seleziona Iniziatore iSCSI Microsoft.
   - Nell'elenco IP iniziatore, seleziona l'indirizzo IP corrispondente all'host. In questo caso, stai connettendo due interfacce di rete sul dispositivo di archiviazione a una singola interfaccia di rete sull'host. Di conseguenza, questa interfaccia è la stessa di quella fornita per la prima sessione.
   - Nell'elenco IP portale di destinazione, seleziona l'indirizzo IP della seconda interfaccia dati abilitata sul dispositivo di archiviazione.

     Puoi trovare il secondo indirizzo IP nella schermata Dettagli di {{site.data.keyword.blockstorageshort}} nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.
      {: tip}
   - Fai clic sulla casella di spunta **Attiva accesso CHAP**
   - Immetti i valori Nome e Segreto destinazione ottenuti dal portale e fai clic su **OK**.
   - Fai clic su **OK** nella finestra Connessione alla destinazione per tornare alla finestra Proprietà.
8. La finestra Proprietà ora visualizza più di una sessione nella finestra Identificatore. Hai più di una sessione nell'archiviazione iSCSI.

   Se il tuo host dispone di più interfacce che vuoi collegare all'archivio ISCSI, puoi configurare un'altra connessione con l'indirizzo IP dell'altro NIC nel campo IP iniziatore. Tuttavia, assicurati di autorizzare il secondo indirizzo IP dell'iniziatore nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} prima di tentare di effettuare la connessione.
   {:note}
9. Nella finestra Proprietà, fai clic su **Dispositivi** e apri la finestra Dispositivi. Il nome dell'interfaccia del dispositivo inizia con `mpio`. <br/>
  ![Dispositivi](/images/Devices.png)

10. Fai clic su **MPIO** per aprire la finestra **Dettagli dispositivo**. Questa finestra ti consente di scegliere le politiche di bilanciamento del carico per MPIO e ti mostra i percorsi all'iSCSI. In questo esempio, due percorsi sono visualizzati come disponibili per MPIO con una politica di bilanciamento del carico Round robin con sottoinsieme.
  ![La finestra Dettagli dispositivo mostra due percorsi disponibili per MPIO con una politica di bilanciamento del carico Round robin con sottoinsieme](/images/DeviceDetails.png)

11. Fai clic su **OK** diverse volte per uscire dall'iniziatore iSCSI.



## Verifica se MPIO è configurato correttamente nei sistemi operativi Windows
{: #verifyMPIOWindows}

Per verificare se MPIO Windows è configurato, devi prima assicurarti che il componente aggiuntivo MPIO sia abilitato e riavviare il server.

![Roles_Features_0](/images/Roles_Features_0.png)

Dopo il completamento del riavvio e l'aggiunta del dispositivo di archiviazione, puoi verificare se MPIO è configurato e funzionante. Per farlo, osserva **Dettagli dispositivo di destinazione** e fai clic su **MPIO**:
![DeviceDetails_0](/images/DeviceDetails_0.png)

Se MPIO non è stato configurato correttamente, il tuo dispositivo di archiviazione può venire disconnesso e essere visualizzato come non disponibile quando si verifica un'interruzione di rete o quando i team di {{site.data.keyword.cloud}} eseguono la manutenzione. MPIO garantisce un livello supplementare di connettività durante tali eventi e mantiene una sessione stabilita con le operazioni di lettura/scrittura attive che vanno al LUN.

## Smontaggio dei volumi {{site.data.keyword.blockstorageshort}}
{: #unmountingWin}

Viene qui di seguito riportata la procedura necessaria per disconnettere un'istanza di elaborazione {{site.data.keyword.Bluemix_short}} basata su Windows a un LUN iSCSI MPIO. L'esempio è basato su Windows Server 2012. La procedura può essere regolata per altre versioni di Windows in base alla documentazione del fornitore del sistema operativo.

### Avvio dell'iniziatore iSCSI

1. Fai clic sulla scheda **Destinazioni**.
2. Seleziona le destinazioni che vuoi rimuovere e fai clic su **Disconnetti**.

### Rimozione delle destinazioni
Questo passo è facoltativo, per quando non hai più bisogno di accedere alle destinazioni iSCSI.

1. Fai clic su **Individuazione** nell'iniziatore iSCSI.
2. Evidenzia il portale di destinazione associato al tuo volume di archiviazione e fai clic su **Rimuovi**.
