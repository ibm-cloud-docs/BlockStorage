---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-26"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Connessione ai LUN iSCSI MPIO su Microsoft Windows

Prima di iniziare, assicurarti che l'host che sta accedendo al volume {{site.data.keyword.blockstoragefull}} sia stato autorizzato tramite [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Dalla pagina di elenco {{site.data.keyword.blockstorageshort}}, individua il nuovo volume e fai clic su **Actions**.Fai clic su **Authorize Host**.
2. Dall'elenco, seleziona l'host o gli host che può accedere al volume e fai clic su **Submit**.

## Montaggio di volumi {{site.data.keyword.blockstorageshort}}

Viene qui di seguito indicata la procedura necessaria per connettere un'istanza di elaborazione {{site.data.keyword.BluSoftlayer_full}} basata su Windows a un LUN (logical unit number) iCSCI (internet Small Computer System Interface) MPIO (multipath input/output). L'esempio è basato su Windows Server 2012. La procedura può essere regolata per altre versioni di Windows in base alla documentazione del fornitore del sistema operativo.

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

**Nota**: in Windows Server 2008, l'aggiunta del supporto per iSCSI consente a un Modulo specifico dispositivo Microsoft (MSDSM, Microsoft Device Specific Module) di richiedere i diritti su tutti i dispositivi iSCSI per MPIO, operazione che richiede preliminarmente una connessione a una Destinazione iSCSI.

### Configurazione dell'iniziatore iSCSI

1. Avvia l'iniziatore iSCSI da Gestione server e seleziona **Strumenti**, **Iniziatore iSCSI**.
2. Fai clic sulla scheda **Configurazione**.
    - Il campo Nome iniziatore potrebbe già essere compilato con una voce simile a `iqn.1991-05.com.microsoft:`.
    - Fai clic su **Modifica** per sostituire i valori esistenti con il tuo nome qualificato iSCSI (IQN, iSCSI Qualified Name). Il nome IQN può essere ottenuto dalla schermata Dettagli di {{site.data.keyword.blockstorageshort}} nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
    ![Proprietà dell'iniziatore iSCSI](/images/iSCSI.png)
    - Fai clic sulla scheda **Individuazione** e fai clic su **Individua portale**.
    - Immetti l'indirizzo IP della tua destinazione iSCSI e lascia la porta al valore predefinito di 3260. 
    - Fai clic su **Avanzate** per aprire la finestra Impostazioni avanzate.
    - Seleziona **Attiva accesso CHAP** per attivare l'autenticazione CHAP.
    ![Attiva accesso CHAP](/images/Advanced_0.png)
    **Nota:** i campi Nome e Segreto destinazione sono sensibili a maiuscole/minuscole.
         - Nel campo **Nome**, elimina qualsiasi voce esistente e immetti il nome utente dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
         - Nel campo **Segreto destinazione**, immetti la password dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
    - Fai clic su **OK** nelle finestre **Impostazioni avanzate** e **Individua portale destinazione** per tornare alla schermata Proprietà iniziatore iSCSI principale. Se ricevi degli errori di autenticazione, controlla le voci di nome utente e password.
    ![Destinazione inattiva](/images/Inactive_0.png)
    **Nota**: il nome della destinazione appare nella sezione Destinazioni individuate con uno stato Inattivo. 

    
### Attivazione della destinazione

1. Fai clic su **Connetti** per stabilire una connessione alla destinazione.
2. Seleziona la casella di spunta **Consenti percorsi multipli** per abilitare il Multipath I/O alla destinazione.
![Consenti percorsi multipli](/images/Connect_0.png)
3. Fai clic su **Avanzate** e seleziona **Attiva accesso CHAP**.
![Attiva CHAP](/images/chap_0.png)
4. Immetti il nome utente nel campo Nome e immetti la password nel campo Segreto destinazione.<br/>
**Nota:** i valori dei campi Nome e Segreto destinazione possono essere ottenuti dalla schermata Dettagli di {{site.data.keyword.blockstorageshort}}.
5. Fai clic su **OK** finché non viene visualizzata la finestra **Proprietà iniziatore iSCSI**. Lo stato della destinazione nella sezione **Destinazioni individuate** cambia da **Inattivo** a **Connesso**.
![Stato Connesso](/images/Connected.png) 


### Configurazione di MPIO nell'iniziatore iSCSI

1. Avvia l'iniziatore iSCSI e, nella scheda Destinazioni, fai clic su **Proprietà**.
2. Fai clic su **Aggiungi sessione** nella finestra Proprietà per aprire la finestra Connessione alla destinazione.
3. Seleziona la casella di spunta **Consenti percorsi multipli** e fai clic su **Avanzate...**.
  ![Destinazione](/images/Target.png) 
  
4. Nella finestra Impostazioni avanzate
   - Lascia Predefinito come valore per i campi Scheda locale e IP iniziatore. Per i server host con più interfacce in iSCSI, devi scegliere il valore appropriato per il campo IP iniziatore.
   - Seleziona l'IP della tua archiviazione iSCSI dall'elenco a discesa **IP portale destinazione**.
   - Fai clic sulla casella di spunta **Attiva accesso CHAP**
   - Immetti i valori Nome e Segreto destinazione ottenuti dal portale e fai clic su **OK**.
   - Fai clic su **OK** nella finestra Connessione alla destinazione per tornare alla finestra Proprietà. La finestra Proprietà ora visualizza più di una sessione nella finestra Identificatore. Ora hai più di una sessione nell'archiviazione iSCSI.
   ![Impostazioni](/images/Settings.png) 
   
5. Nella finestra Proprietà, fai clic su **Dispositivi** e apri la finestra Dispositivi. Il nome dell'interfaccia del dispositivo inizia con `mpio`. <br/>
  ![Dispositivi](/images/Devices.png) 
  
6. Fai clic su **MPIO** per aprire la finestra **Dettagli dispositivo**. Questa finestra ti consente di scegliere le politiche di bilanciamento del carico per MPIO e ti mostra i percorsi all'iSCSI. In questo esempio, due percorsi sono visualizzati come disponibili per MPIO con una politica di bilanciamento del carico Round robin con sottoinsieme.
  ![La finestra Dettagli dispositivo mostra due percorsi disponibili per MPIO con una politica di bilanciamento del carico Round robin con sottoinsieme](/images/DeviceDetails.png) 
  
7. Fai clic su **OK** diverse volte per uscire dall'iniziatore iSCSI.



## Verifica se MPIO è configurato correttamente nei sistemi operativi Windows

Per verificare se MPIO Windows è configurato, devi prima assicurarti che il componente aggiuntivo MPIO sia abilitato e riavviare il server.

![Roles_Features_0](/images/Roles_Features_0.png)

Dopo il completamento del riavvio e l'aggiunta del dispositivo di archiviazione, puoi verificare se MPIO è configurato e funzionante. Per farlo, osserva **Dettagli dispositivo di destinazione** e fai clic su **MPIO**:
![DeviceDetails_0](/images/DeviceDetails_0.png)

Se MPIO non è stato configurato correttamente, il tuo dispositivo di archiviazione viene disconnesso e diventa non disponibile quando si verifica un'interruzione di rete o quando i team di {{site.data.keyword.BluSoftlayer_full}} eseguono la manutenzione. MPIO garantisce un livello supplementare di connettività durante tali eventi e mantiene una connessione stabilita con le letture/scritture attive che vanno al LUN.

## Smontaggio dei volumi {{site.data.keyword.blockstorageshort}}

Viene qui di seguito riportata la procedura necessaria per disconnettere un'istanza di elaborazione {{site.data.keyword.Bluemix_short}} basata su Windows a un LUN iSCSI MPIO. L'esempio è basato su Windows Server 2012. La procedura può essere regolata per altre versioni di Windows in base alla documentazione del fornitore del sistema operativo.

### Avvio dell'iniziatore iSCSI

1. Fai clic sulla scheda **Destinazioni**.
2. Seleziona le destinazioni che vuoi rimuovere e fai clic su **Disconnetti**.

### Rimozione delle destinazioni
Facoltativo, per quando non hai più bisogno di accedere alle destinazioni iSCSI.

1. Fai clic su **Individuazione** nell'iniziatore iSCSI.
2. Evidenzia il portale di destinazione associato al tuo volume di archiviazione e fai clic su **Rimuovi**.
