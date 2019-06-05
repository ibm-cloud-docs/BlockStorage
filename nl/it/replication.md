---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-11"

keywords: Block Storage, secondary storage, replication, duplicate volume, synchronized volumes, primary volume, secondary volume, DR, disaster recovery

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Replica dei dati
{: #replication}

La replica usa una delle tue pianificazioni delle istantanee per copiare automaticamente le istantanee su un volume di destinazione in un data center remoto. Le copie possono essere ripristinate nel sito remoto se si verifica un evento catastrofico o un danneggiamento dei dati.

La replica mantiene i tuoi dati sincronizzati in due diverse ubicazioni. Se vuoi clonare il tuo volume e utilizzarlo in modo indipendente dal volume originale, consulta [Creazione di un volume di blocco duplicato](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume).
{:tip}

Prima di poter eseguire la replica, è necessario creare una pianificazione delle istantanee.
{:important}


## Determinazione del data center remoto per il mio volume di archiviazione replicato

I data center {{site.data.keyword.cloud}} sono accoppiati in combinazioni di primario e remoto in tutto il mondo.
Vedi la Tabella 1 per l'elenco completo della disponibilità dei data center e delle destinazioni di replica.

| US 1 | US 2 | America Latina | Canada  | Europa  | Asia-Pacifico  | Australia  |
|-----|-----|-----|-----|-----|-----|-----|
| DAL01<br />DAL05<br />DAL06<br />HOU02<br />SJC01<br />WDC01 | SJC03<br />SJC04<br />WDC04<br />WDC06<br />WDC07<br />DAL09<br />DAL10<br />DAL12<br />DAL13 | MEX01<br />SAO01 | TOR01<br />MON01 | AMS01<br />AMS03<br />FRA02<br />FRA04<br />FRA05<br />LON02<br />LON04<br />LON05<br />LON06<br />OSL01<br />PAR01<br />MIL01 | HKG02<br />TOK02<br />TOK04<br />TOK05<br />SNG01<br />SEO01<br />CHE01 | SYD01<br />SYD04<br />SYD05<br />MEL01 |
{: caption="Tabella 1 - questa tabella mostra l'elenco completo di data center con funzionalità migliorate in ciascuna regione. Ogni regione è una colonna separata. Alcune città, come Dallas, San Jose, Washington DC, Amsterdam, Francoforte, Londra e Sydney hanno più data center. I data center nella regione US 1 NON hanno l'archiviazione migliorata. Gli host nei data center con funzionalità di archiviazione migliorate non possono avviare la replica con le destinazioni di replica nei data center US 1." caption-side="top"}

## Creazione della replica iniziale

Le repliche funzionano in base a una pianificazione delle istantanee. Prima di poter eseguire la replica, devi già avere lo spazio di istantanea e una pianificazione delle istantanee per il volume di origine. Se tenti di impostare la replica e non disponi di uno o l'altro, ti viene richiesto di acquistare più spazio o di impostare una pianificazione. Le repliche sono gestite in **Storage**, **{{site.data.keyword.blockstorageshort}}** nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.

1. Fai clic sul tuo volume di archiviazione.
2. Fai clic su **Replica** e fai clic su **Purchase a replication**.
3. Seleziona la pianificazione delle istantanee esistente che vuoi venga seguita dalla tua replica. L'elenco contiene tutte le tue pianificazioni delle istantanee attive. <br />
   Puoi selezionare solo una pianificazione, anche se hai una combinazione di orarie, giornaliere e settimanali. Tutte le istantanee acquisite a partire dal ciclo di replica precedente vengono replicate indipendentemente dalla pianificazione che ha dato loro origine.<br />Se non hai delle istantanee configurate, ti viene richiesto di farlo prima che tu possa ordinare la replica. Vedi il documento relativo alla [gestione delle istanze](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots) per ulteriori dettagli.
   {:important}
3. Fai clic su **Location** e seleziona il data center che è il tuo sito di ripristino di emergenza (DR, disaster recovery).
4. Fai clic su **Continue**.
5. Immetti un codice promozionale (**Promo Code**), se ne hai uno, e fai clic su **Recalculate**. Gli altri campi nella finestra sono completati per impostazione predefinita.

   Gli sconti vengono applicati quando l'ordine viene elaborato.
   {:note}
6. Fai clic sulla casella di spunta **I have read the Master Service Agreement…** e su **Place Order**.


## Modifica di una replica esistente

Puoi modificare la tua pianificazione replica e modificare il tuo spazio di replica dalla scheda **Primary** o da quella **Replica** in **Storage**, **{{site.data.keyword.blockstorageshort}}** nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.


## Modifica della pianificazione replica

La pianificazione replica è basata su una pianificazione delle istantanee esistente. Per modificare la pianificazione della replica da Hourly a Daily o Weekly o viceversa, devi annullare il volume di replica e configurarne uno nuovo.

Tuttavia, se vuoi modificare l'ora del giorno di quando si verifica la replica **Daily**, puoi modificare la pianificazione esistente sulla scheda Primary o Replica.

1. Fai clic su **Actions** nella scheda **Primary** o in quella **Replica**.
2. Seleziona **Edit Snapshot Schedule**.
3. Controlla nel frame **Snapshot** in **Schedule** per determinare quale pianificazione stai usando per la replica. Modifica la pianificazione che desideri.
4. Fai clic su **Save**.


## Modifica dello spazio di replica

Il tuo spazio di istantanea primario e il tuo spazio di replica devono essere uguali. Se modifichi lo spazio nella scheda **Primary** o in quella **Replica**, viene automaticamente aggiunto dello spazio sia al data center di origine sia a quello di destinazione. L'aumento dello spazio dell'istantanea attiva anche un aggiornamento di replica immediato.

1. Fai clic su **Actions** nella scheda **Primary** o in quella **Replica**.
2. Seleziona **Add More Snapshot Space**.
3. Seleziona la dimensione di archiviazione dall'elenco e fai clic su **Continue**.
4. Immetti un codice promozionale (**Promo Code**), se ne hai uno, e fai clic su **Recalculate**. Gli altri campi nella finestra di dialogo sono completati per impostazione predefinita.
5. Fai clic sulla casella di spunta **I have read the Master Service Agreement…** e fai clic su **Place Order**.


## Visualizzazione dei volumi di replica nell'elenco volumi

Puoi visualizzare i tuoi volumi di replica nella pagina {{site.data.keyword.blockstorageshort}} in **Storage > {{site.data.keyword.blockstorageshort}}**. Il nome LUN (**LUN Name**) mostra il nome del volume primario seguito da REP. Il tipo (**Type**) è Endurance o Performance – Replica. L'indirizzo di destinazione (**Target Address**) non è disponibile (N/A) perché il volume di replica non è montato nel data center di replica e lo stato (**Status**) mostra inattivo (Inactive).


## Visualizzazione dei dettagli di un volume replicato nel data center di replica

Puoi visualizzare i dettagli del volume di replica nella scheda **Replica** in **Storage**, **{{site.data.keyword.blockstorageshort}}**. Un'altra opzione consiste nel selezionare il volume di replica dalla pagina **{{site.data.keyword.blockstorageshort}}** e fare clic sulla scheda **Replica**.


## Aumenta lo spazio dell'istantanea nel data center di replica quando lo spazio dell'istantanea viene aumentato nel data center primario

Le dimensioni dei tuoi volumi devono essere le stesse per i volumi di archiviazione primario e di replica. L'uno non può essere più grande dell'altro. Quando aumenti il tuo spazio di istantanea per il tuo volume primario, lo spazio di replica viene aumentato automaticamente. L'aumento dello spazio dell'istantanea attiva un aggiornamento di replica immediato. L'aumento per entrambi i volumi viene visualizzato come delle voci di riga nella tua fattura ed è a base proporzionale come necessario.

Per ulteriori informazioni sulle considerazioni sullo spazio dell'istantanea, consulta [Ordinazione di istantanee](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
{:tip}


## Visualizzazione della cronologia replica

La cronologia replica può essere visualizzata in **Audit Log** tramite la scheda **Account** in **Manage**. Entrambi i volumi primario e di replica visualizzano cronologie di replica identiche. La cronologia include i seguenti elementi:

- Il tipo per la replica (failover o failback).
- Quando è stata avviata.
- L'istantanea utilizzata per la replica.
- La dimensione della replica.
- L'ora in cui è stata completata la replica.


## Creazione di un duplicato di una replica

Puoi creare un duplicato di un {{site.data.keyword.cloud}} {{site.data.keyword.blockstoragefull}} esistente. Il volume duplicato eredita per impostazione predefinita le opzioni di capacità e prestazioni del volume originale e ha una copia dei dati che arriva fino al punto temporale di un'istantanea.

I duplicati possono essere creati sia dal volume primario che da quello di replica. Il nuovo duplicato viene creato nello stesso data center del volume originale. Se crei un duplicato da un volume di replica, il nuovo volume viene creato nello stesso data center del volume di replica.

I volumi duplicati solo accessibili da un host per la lettura/scrittura non appena viene seguito il provisioning dell'archiviazione. Tuttavia, le istantanee e le repliche sono consentite solo dopo il completamento della copia dei dati dall'originale al duplicato.

Per ulteriori informazioni, vedi [Creazione di un volume di blocco duplicato](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume).

## Utilizzo delle repliche per eseguire il failover quando si verifica un'emergenza

Quando esegui il failover, stai passando dal tuo volume di archiviazione nel tuo data center primario al volume di destinazione nel tuo data center remoto. Ad esempio, il tuo data center primario si trova a Londra e il tuo data center secondario si trova ad Amsterdam. Se si verifica un evento di malfunzionamento, eseguirai il failover ad Amsterdam, stabilendo una connessione al volume che ora è quello primario da un'istanza di elaborazione ad Amsterdam. Dopo che il tuo volume a Londra sarà stato riparato, verrà acquisita un'istantanea del volume che si trova ad Amsterdam per eseguire il failback a Londra e al volume che ora è nuovamente quello primario da un'istanza di elaborazione a Londra.

* Se l'ubicazione primaria non è in pericolo imminente o non sta subendo gravi ripercussioni, consulta [Failover con un volume primario accessibile](/docs/infrastructure/BlockStorage?topic=BlockStorage-dr-accessible).
* Se l'ubicazione primaria non è attiva, consulta [Failover con un volume primario non accessibile](/docs/infrastructure/BlockStorage?topic=BlockStorage-dr-inaccessible).


## Annullamento di una replica esistente

Puoi annullare la replica immediatamente o alla data dell'anniversario, che causa la terminazione della fatturazione. La replica può essere annullata dalla scheda **Primary** o da quella **Replica**.

1. Fai clic sul volume nella pagina **{{site.data.keyword.blockstorageshort}}**.
2. Fai clic su **Actions** nella scheda **Primary** o in quella **Replica**.
3. Seleziona **Cancel Replica**.
4. Seleziona quando annullare. Scegli **Immediately** o **Anniversary Date** e fai clic su **Continue**.
5. Fai clic su **I acknowledge that due to cancellation, data loss may occur** e fai clic su **Cancel Replica**.


## Annullamento della replica quando il volume primario viene annullato

Quando un volume primario viene annullato, la pianificazione replica e il volume nel data center di replica vengono eliminati. Le repliche vengono annullate dalla pagina {{site.data.keyword.blockstorageshort}}.

 1. Evidenzia il tuo volume nella pagina **{{site.data.keyword.blockstorageshort}}**.
 2. Fai clic su **Actions** e seleziona **Cancel {{site.data.keyword.blockstorageshort}}**.
 3. Seleziona quando annullare. Scegli **Immediately** o **Anniversary Date** e fai clic su **Continue**.
 4. Fai clic su **I acknowledge that due to cancellation, data loss may occur** e fai clic su **Cancel**.

## Comandi correlati alla replica nella SLCLI
{: #clicommands}

* Elenca i datacenter di replica appropriati per un volume specifico.
  ```
  # slcli block replica-locations --help
  Usage: slcli block replica-locations [OPTIONS] VOLUME_ID

  Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: ID, Long Name, Short Name
  -h, --help      Show this message and exit.
  ```

* Ordina un volume di replica dell'archiviazione blocchi.
  ```
  # slcli block replica-order --help
  Usage: slcli block replica-order [OPTIONS] VOLUME_ID

  Options:
  -s, --snapshot-schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                                  Snapshot schedule to use for replication,
                                  (INTERVAL | HOURLY | DAILY | WEEKLY)
                                  [required]
  -l, --location TEXT             Short name of the data center for the
                                  replicant (e.g.: dal09)  [required]
  --tier [0.25|2|4|10]            Endurance Storage Tier (IOPS per GB) of the
                                  primary volume for which a replicant is
                                  ordered [optional]
  --os-type [HYPER_V|LINUX|VMWARE|WINDOWS_2008|WINDOWS_GPT|WINDOWS|XEN]
                                  Operating System Type (e.g.: LINUX) of the
                                  primary volume for which a replica is
                                  ordered [optional]
  -h, --help                      Show this message and exit.
  ```

* Elenca i volumi di replica esistenti per un volume di blocchi.
  ```
  # slcli block replica-partners --help
  Usage: slcli block replica-partners [OPTIONS] VOLUME_ID

  Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: ID, Username, Account ID,
                  Capacity (GB), Hardware ID, Guest ID, Host ID
  -h, --help      Show this message and exit.
  ```

* Failover di un volume di blocchi in un volume di replica specifico.
  ```
  # slcli block replica-failover --help
  Usage: slcli block replica-failover [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  --immediate          Failover to replicant immediately.
  -h, --help      Show this message and exit.
  ```

* Failback di un volume di blocchi da un volume di replica specifico.
  ```
  # slcli block replica-failback --help
  Usage: slcli block replica-failback [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  -h, --help           Show this message and exit.
  ```
