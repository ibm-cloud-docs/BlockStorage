---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:  Block Storage, block storage, snapshot, snapshot space, snapshot schedule, create snapshot schedule, manual snapshot, view snapshot space, modify snapshot space, SLCLI, API, restore from snapshot

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}

# Gestione delle istantanee
{: #managingSnapshots}

## Creazione di una pianificazione dell'istantanea

Con le pianificazioni delle istantanee decidi con che frequenza e quando vuoi creare un riferimento ad un punto temporale del tuo volume di archiviazione. Puoi avere un massimo di 50 istantanee per volume di archiviazione. Le pianificazioni vengono gestite tramite la scheda **Storage** > **{{site.data.keyword.blockstorageshort}}** del [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window}.

Prima di poter configurare la tua pianificazione iniziale, devi procedere all'acquisto di spazio di istantanea, se non lo hai fatto durante il provisioning iniziale del volume di archiviazione. Per ulteriori informazioni, consulta [Ordinazione di istantanee](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
{:important}

### Aggiunta di una pianificazione dell'istantanea
{: #addingschedule}

Le pianificazioni delle istantanee possono essere configurate per intervalli orari, giornalieri e settimanali, ciascuno con un distinto ciclo di conservazione. Il limite massimo di istantanee è di 50 per volume di archiviazione, che può essere una combinazione di pianificazioni orarie, giornaliere e settimanali e di istantanee manuali.

1. Fai clic sul tuo volume di archiviazione, fai clic su **Actions** e fai clic su **Schedule Snapshot**.
2. Nella finestra New Schedule Snapshot, puoi scegliere tra tre diverse frequenze di istantanea. Usa qualsiasi combinazione di queste tre frequenze per creare una pianificazione delle istantanee completa.
   - Hourly
      - Specifica il minuto di ciascuna ora in cui deve essere eseguita un'istantanea. Il valore predefinito è il minuto corrente.
      - Specifica il numero di istantanee orarie da conservare prima di eliminare quella meno recente.
   - Daily
      - Specifica l'ora e il minuto in cui deve essere eseguita un'istantanea. Il valore predefinito è il minuto e l'ora correnti.
      - Seleziona il numero di istantanee orarie da conservare prima di eliminare quella meno recente.
   - Weekly
      - Specifica il giorno della settimana, l'ora e il minuto in cui deve essere eseguita un'istantanea. Il valore predefinito è il minuto, l'ora e il giorno correnti.
      - Seleziona il numero di istantanee settimanali da conservare prima di eliminare quella meno recente.
3. Fai clic su **Save** e crea un'altra pianificazione con una frequenza differente. Se il numero totale di istantanee pianificate è superiore a 50, ricevi un messaggio di avvertenza e non puoi eseguire il salvataggio.

L'elenco delle istantanee viene visualizzato man mano che vengono eseguite nella sezione **Snapshots** della pagina **Detail**.

Puoi anche visualizzare l'elenco delle tue pianificazioni delle istantanee tramite la CLI SL con il seguente comando.
```
# slcli block snapshot-schedule-list --help
Usage: slcli block snapshot-schedule-list [OPTIONS] VOLUME_ID

Options:
  -h, --help  Show this message and exit.
```
{:codeblock}

## Acquisizione di un'istantanea manuale

Le istantanee manuali possono essere eseguite a vari punti durante un upgrade o una manutenzione dell'applicazione. Puoi anche acquisire istantanee su più server che sono stati temporaneamente disattivati a livello dell'applicazione.

Il limite massimo di istantanee per volume di archiviazione è di 50.

1. Fai clic sul tuo volume di archiviazione.
2. Fai clic su **Actions**.
3. Fai clic su **Take Manual Snapshot**.
L'istantanea viene acquisita e visualizzata nella sezione **Snapshots** della pagina **Detail**. La sua pianificazione è manuale (Manual).

In alternativa, puoi utilizzare il seguente comando per creare un'istantanea tramite la CLI SL.
```
# slcli block snapshot-create --help
Usage: slcli block snapshot-create [OPTIONS] VOLUME_ID

Options:
  -n, --notes TEXT  Notes to set on the new snapshot
  -h, --help        Show this message and exit.
```
{:codeblock}

## Elenco di tutte le istantanee con funzioni di gestione e informazione dello spazio utilizzato

Un elenco di istantanee conservate e spazio utilizzato può essere visualizzato nella pagina **Detail**.  Le funzioni di gestione (modifica di pianificazioni e aggiunta di ulteriore spazio) vengono eseguite nella pagina Detail utilizzando il menu **Actions** o i link nelle diverse sezioni della pagina.

## Visualizzazione dell'elenco delle istantanee conservate

Le istantanee conservate sono basate sul numero che hai immesso nel campo **Keep the last** quando configuri le tue pianificazioni. Puoi visualizzare le istantanee che hai acquisito nella sezione **Snapshot**. Le istantanee sono elencate in base alla pianificazione.

In alternativa, puoi utilizzare il seguente comando nella CLI SL per visualizzare le istantanee disponibili.
```
# slcli block snapshot-list --help
Usage: slcli block snapshot-list [OPTIONS] VOLUME_ID

Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: id, name, created, size_bytes
  -h, --help      Show this message and exit.
```
{:codeblock}

## Visualizzazione della quantità di spazio istantanea utilizzato.

Il grafico a torta nella pagina **Details** visualizza quanto spazio è utilizzato e quanto spazio è rimasto. Ricevi delle notifiche quando raggiungerai le soglie di spazio – 75 percento, 90 percento e 95 percento.

## Modifica della quantità di spazio di istantanea per un volume

Potresti aver bisogno di aggiungere dello spazio di istantanea a un volume che in precedenza non ne aveva o che potrebbe averne bisogno. Puoi aggiungere da 5 a 4.000 GB, a seconda delle tue esigenze.

Lo spazio di istantanea può essere solo aumentato. Non può essere ridotto. Puoi selezionare una quantità di spazio più piccola finché non determini di quanto spazio hai bisogno. Ricordati che le istantanee automatizzate e quelle manuali condividono lo spazio.
{:note}

Lo spazio di istantanea viene modificato tramite **Storage** > **{{site.data.keyword.blockstorageshort}}**.

1. Fai clic sui tuoi volumi di archiviazione, fai clic su **Actions** e fai clic su **Add More Snapshot Space**.
2. Seleziona un intervallo di dimensioni dal prompt. Le dimensioni di norma vanno da 0 alla dimensione del tuo volume.
3. Fai clic su **Continue**.
4. Immetti l'eventuale codice promozionale a tua disposizione e fai clic su **Recalculate**. Gli addebiti (Charges) per questo ordine e il riesame dell'ordine (Order Review) vengono completati per impostazione predefinita.
5. Fai clic sulla casella di spunta **I have read the Master Service Agreement…** e fai clic su **Place Order**. Nel giro di pochi minuti, viene eseguito il provisioning del tuo spazio di istantanea aggiuntivo.

## Ricezione delle notifiche quando viene raggiunto il limite dello spazio di istantanea e le istantanee vengono eliminate

Le notifiche vengono inviate tramite i ticket di supporto all'utente master sul tuo account quando raggiungi tre diverse soglie di spazio – 75 percento, 90 percento e 95 percento.

- Al raggiungimento del **75 percento della capacità**, viene inviata un'avvertenza che l'utilizzo dello spazio di istantanea ha superato il 75 percento. Se presti attenzione all'avvertenza e aggiungi dello spazio manualmente oppure elimini delle istantanee conservate e non necessarie, l'azione viene annotata e il ticket viene chiuso. Se non fai niente, devi confermare manualmente il ticket che viene quindi chiuso.
- Al raggiungimento del **90 percento della capacità**, viene inviata una seconda avvertenza che l'utilizzo dello spazio di istantanea ha superato il 90 percento. In modo analogo al raggiungimento della capacità al 75 percento, se esegui le azioni necessarie per ridurre lo spazio utilizzato, l'azione viene annotata e il ticket viene chiuso. Se non fai niente, devi confermare manualmente il ticket che viene quindi chiuso.
- Al raggiungimento del **95 percento della capacità**, viene inviato un'avvertenza finale. Se non viene eseguita alcuna azione per portare il tuo utilizzo dello spazio al di sotto della soglia, viene generata una notifica e si verifica un'eliminazione automatica in modo che sia possibile creare delle future istantanee. Le istantanee pianificate vengono eliminate, a partire da quella meno recente, finché l'utilizzo non scende al di sotto del 95 percento. Le istantanee continuano a essere eliminate ogni volta che l'utilizzo supera il 95 percento finché non scende al di sotto della soglia. Se lo spazio viene aumentato manualmente oppure se le istantanee vengono eliminate, l'avvertenza viene reimpostata ed emessa nuovamente se la soglia viene superata nuovamente. Se non viene eseguita alcuna azione, questa notifica è la sola avvertenza che viene ricevuta.

## Eliminazione di una pianificazione dell'istantanea

Le pianificazioni delle istantanee possono essere eliminate tramite **Storage** > **{{site.data.keyword.blockstorageshort}}**.

1. Fai clic sulla pianificazione da eliminare nel frame **Snapshot Schedules** nella pagina **Details**.
2. Fai clic sulla casella di spunta accanto alla pianificazione da eliminare e fai clic su **Save**.<br />

Se stai utilizzando la funzione di replica, assicurati che la pianificazione che stai eliminando non sia la pianificazione utilizzata dalla replica. Per ulteriori informazioni sull'eliminazione di una pianificazione della replica, consulta [Replica dei dati](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication).
{:important}

## Eliminazione di un'istantanea

Le istantanee che non sono più necessarie possono essere rimosse manualmente per liberare spazio per le future istantanee. L'eliminazione viene eseguita tramite **Storage** > **{{site.data.keyword.blockstorageshort}}**.

1. Fai clic sul tuo volume di archiviazione e scorri alla sezione **Snapshot** per visualizzare l'elenco delle istantanee esistenti.
2. Fai clic su **Actions** accanto a una specifica istantanea e fai clic su **Delete** per eliminare l'istantanea. Questa eliminazione non ha alcuna ripercussione sulle eventuali istantanee passate o future nella stessa pianificazione poiché le istantanee non hanno alcuna interdipendenza.

Le istantanee manuali che non vengono eliminate nel portale manualmente, vengono eliminate automaticamente quando raggiungi le limitazioni dello spazio (prima le ultime).

Puoi utilizzare il seguente comando per eliminare un volume tramite la CLI SL.
```
# slcli block snapshot-delete
Usage: slcli block snapshot-delete [OPTIONS] SNAPSHOT_ID

Options:
  -h, --help  Show this message and exit.
```
{:codeblock}


## Ripristino del volume di archiviazione a uno specifico punto temporale utilizzando un'istantanea

Potresti dover riportare il tuo volume di archiviazione a uno specifico punto temporale a causa di un errore utente o di un danneggiamento dei dati.

Il ripristino di un volume determina l'eliminazione di tutte le istantanee acquisite successivamente all'istantanea utilizzata per il ripristino.
{:important}

1. Smonta e scollega il tuo volume di archiviazione dall'host.
   - [Connessione ai LUN iSCSI su Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#unmounting)
   - [Connessione ai LUN iSCSI su Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows#unmounting)
2. Fai clic su **Storage**, **{{site.data.keyword.blockstorageshort}}** nel [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window}.
3. Scorri verso il basso e fai clic sul tuo volume da ripristinare. la sezione **Snapshots** della pagina **Detail** visualizza l'elenco di tutte le istantanee selezionate insieme alle relative dimensione e data di creazione.
4. Fai clic su **Actions** accanto all'istantanea da utilizzare e fai clic su **Restore**. <br/>

   Il completamento del ripristino comporta la perdita dei dati creati o modificati dopo l'acquisizione dell'istantanea. Questa perdita di dati si verifica perché il tuo volume di archiviazione restituisce lo stesso stato in cui si trovava al momento dell'istantanea.
   {:note}
5. Fai clic su **Sì** per avviare il ripristino.

   Aspettati un messaggio nella pagina che indica che il volume è stato ripristinato utilizzando l'istantanea selezionata. Compare inoltre un'icona accanto al tuo volume in {{site.data.keyword.blockstorageshort}} che indica che è in corso una transazione attiva. Se passi il puntatore del mouse sull'icona, verrà visualizzata una finestra che indica la transazione. Una volta completata la transazione, l'icona scompare.
   {:note}
6. Monta e ricollega il tuo volume di archiviazione all'host.
   - [Connessione ai LUN su Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
   - [Connessione ai LUN su CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
   - [Connessione ai LUN su Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)

In alternativa, una volta che il volume è stato scollegato dall'host, puoi utilizzare il seguente comando nella CLI SL per avviare un ripristino.
```
# slcli block snapshot-restore --help
Usage: slcli block snapshot-restore [OPTIONS] VOLUME_ID

Options:
  -s, --snapshot-id TEXT  The id of the snapshot which is to be used to restore
                          the block volume
  -h, --help              Show this message and exit.
```
{:codeblock}  

Al termine del ripristino, monta e ricollega il tuo volume di archiviazione all'host.
