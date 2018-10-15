---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-15"

---
{:new_window: target="_blank"}

# Replica dei dati

La replica usa una delle tue pianificazioni delle istantanee per copiare automaticamente le istantanee su un volume di destinazione in un data center remoto. Le copie possono essere ripristinate nel sito remoto se si verifica un evento catastrofico o un danneggiamento dei dati.

Con le repliche, puoi

- eseguire il ripristino da malfunzionamento del sito e altre situazioni critiche in modo rapido eseguendo il failover al volume di destinazione;
- eseguire il failover a uno specifico punto temporale nella copia di ripristino di emergenza (DR, disaster recovery).

Prima di poter eseguire la replica, è necessario creare una pianificazione delle istantanee. Quando esegui il failover, stai passando dal tuo volume di archiviazione nel tuo data center primario al volume di destinazione nel tuo data center remoto. Ad esempio, il tuo data center primario si trova a Londra e il tuo data center secondario si trova ad Amsterdam. Se si verifica un evento di malfunzionamento, eseguirai il failover ad Amsterdam, stabilendo una connessione al volume che ora è quello primario da un'istanza di elaborazione ad Amsterdam. Dopo che il tuo volume a Londra sarà stato riparato, verrà acquisita un'istantanea del volume che si trova ad Amsterdam per eseguire il failback a Londra e al volume che ora è nuovamente quello primario da un'istanza di elaborazione a Londra.


## Determinazione del data center remoto per il mio volume di archiviazione replicato

I data center {{site.data.keyword.BluSoftlayer_full}} sono accoppiati in combinazioni di primario e remoto in tutto il mondo.
Vedi la Tabella 1 per l'elenco completo della disponibilità dei data center e delle destinazioni di replica.

<table>
  <caption style="text-align: left;"><p>Tabella 1 - questa tabella mostra l'elenco completo di data center con funzionalità migliorate in ciascuna regione. Ogni regione è una colonna separata. Alcune città, come Dallas, San Jose, Washington DC, Amsterdam, Francoforte, Londra e Sydney hanno più data center.</p>
  <p>&#42; I data center nella regione US 1 NON hanno l'archiviazione migliorata. Gli host nei data center con funzionalità di archiviazione migliorate <strong>non possono</strong> avviare la replica con le destinazioni di replica nei data center US 1.</p>
  </caption>
    <thead>
    <tr>
      <th>US 1 &#42;</th>
      <th>US 2</th>
      <th>America Latina</th>
      <th>Canada</th>
      <th>Europa</th>
      <th>Asia-Pacifico</th>
      <th>Australia</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>DAL01<br />
          DAL05<br />
	  DAL06<br />
	  HOU02<br />
	  SJC01<br />
	  WDC01<br />
	  <br /><br /><br /><br /><br /><br />
      </td>
      <td>SJC03<br />
	  SJC04<br />
	  WDC04<br />
	  WDC06<br />
	  WDC07<br />
	  DAL09<br />
	  DAL10<br />
	  DAL12<br />
	  DAL13<br />
	  <br /><br /><br />
      </td>
      <td>MEX01<br />
	  SAO01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>TOR01<br />
          MON01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>AMS01<br />
	  AMS03<br />
	  FRA02<br />
	  FRA04<br />
	  FRA05<br />
	  LON02<br />
	  LON04<br />
	  LON05<br />
	  LON06<br />
	  OSL01<br />
	  PAR01<br />
	  MIL01<br />
      </td>
      <td>HKG02<br />
          TOK02<br />
	  TOK04<br />
	  TOK05<br />
	  SNG01<br />
	  SEO01<br />
          CHE01<br />
	  <br /><br /><br /><br /><br />
      </td>
      <td>SYD01<br />
          SYD04<br />
	  MEL01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
    </tr>
  </tbody>
</table>

## Creazione della replica iniziale

Le repliche funzionano in base a una pianificazione delle istantanee. Prima di poter eseguire la replica, devi già avere lo spazio di istantanea e una pianificazione delle istantanee per il volume di origine. Se tenti di impostare la replica e non disponi di uno o l'altro, ti viene richiesto di acquistare più spazio o di impostare una pianificazione. Le repliche sono gestite in **Storage**, **{{site.data.keyword.blockstorageshort}}** nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Fai clic sul tuo volume di archiviazione.
2. Fai clic su **Replica** e fai clic su **Purchase a replication**.
3. Seleziona la pianificazione delle istantanee esistente che vuoi venga seguita dalla tua replica. L'elenco contiene tutte le pianificazioni delle istantanee attive. <br />
   >**Nota:** - Puoi selezionare solo una pianificazione, anche se hai una combinazione di orarie, giornaliere e settimanali. Tutte le istantanee acquisite a partire dal ciclo di replica precedente vengono replicate indipendentemente dalla pianificazione che ha dato loro origine.<br />Se non hai delle istantanee configurate, ti viene richiesto di farlo prima che tu possa ordinare la replica. Vedi il documento relativo alla [gestione delle istanze](snapshots.html) per ulteriori dettagli.
3. Fai clic su **Location** e seleziona il data center che è il tuo sito di ripristino di emergenza (DR, disaster recovery).
4. Fai clic su **Continue**.
5. Immetti un codice promozionale (**Promo Code**), se ne hai uno, e fai clic su **Recalculate**. Gli altri campi nella finestra sono completati per impostazione predefinita.
6. Fai clic sulla casella di spunta **I have read the Master Service Agreement…** e su **Place Order**.


## Modifica di una replica esistente

Puoi modificare la tua pianificazione replica e modificare il tuo spazio di replica dalla scheda **Primary** o da quella **Replica** in **Storage**, **{{site.data.keyword.blockstorageshort}}** nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.



## Modifica della pianificazione replica

La pianificazione replica è basata su una pianificazione delle istantanee esistente. Per modificare la pianificazione replica, ad esempio da Hourly a Weekly, devi annullare la pianificazione replica e configurarne una nuova.

La modifica della pianificazione può essere eseguita nella scheda Primary o in quella Replica.

1. Fai clic su **Actions** nella scheda **Primary** o in quella **Replica**.
2. Seleziona **Edit Snapshot Schedule**.
3. Controlla nel frame **Snapshot** in **Schedule** per determinare quale pianificazione stai usando per la replica. Modifica la pianificazione che desideri. Ad esempio, se la tua pianificazione replica è **Daily**, puoi modificare l'ora del giorno in cui deve essere eseguita la replica.
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


## Specifica delle autorizzazioni host prima che il server riscontri un failover nel data center secondario

Gli host autorizzati e i volumi si devono trovare nello stesso data center. Non puoi avere un volume di replica a Londra e l'host di Amsterdam. Entrambi devono essere a Londra o entrambi devono essere ad Amsterdam.

1. Fai clic sul tuo volume di origine o di destinazione dall'una o dall'altra pagina **{{site.data.keyword.blockstorageshort}}**-
2. Fai clic su **Replica**.
3. Scorri giù al frame **Authorize Hosts** e fai clic su **Authorize Hosts** sulla destra.
4. Evidenzia l'host che deve essere autorizzato per le repliche. Per selezionare più host, utilizza il tasto CTRL e fai clic sugli host applicabili.
5. Fai clic su **Submit**. Se non hai alcun host, ti viene richiesto di acquistare delle risorse di calcolo nello stesso data center.


## Aumenta lo spazio dell'istantanea nel data center di replica quando lo spazio dell'istantanea viene aumentato nel data center primario

Le dimensioni dei tuoi volumi devono essere le stesse per i volumi di archiviazione primario e di replica. L'uno non può essere più grande dell'altro. Quando aumenti il tuo spazio di istantanea per il tuo volume primario, lo spazio di replica viene aumentato automaticamente. L'aumento dello spazio dell'istantanea attiva un aggiornamento di replica immediato. L'aumento per entrambi i volumi viene visualizzato come delle voci di riga nella tua fattura ed è a base proporzionale come necessario.

Fai clic [qui](snapshots.html) per informazioni su come aumentare il tuo spazio di istantanea.


## Avvio di un failover da un volume alla sua replica

Se si verifica un evento di errore, puoi avviare un **failover** al tuo volume di destinazione. Il volume di destinazione diventa attivo. L'ultima istantanea replicata correttamente viene attivata e il volume viene reso disponibile per il montaggio. Tutti i dati che erano stati scritti nel volume di origine a partire dal ciclo di replica precedente vengono perduti. Una volta avviato un failover, la relazione di replica viene invertita. Il tuo volume di destinazione diventa il tuo volume di origine e il tuo precedente volume di origine diventa la tua destinazione, come indicato dal nome LUN (**LUN Name**) seguito da **REP**.

I failover vengono avviati in **Storage**, **{{site.data.keyword.blockstorageshort}}** nel [[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

**Prima di procedere con questa procedura, disconnetti il volume. In caso contrario, si verifica un danneggiamento e una perdita di dati.**

1. Fai clic sul tuo LUN attivo ("origine").
2. Fai clic su **Replica** e fai clic sul link **Actions** nell'angolo superiore destro.
3. Seleziona **Failover**.
   >Prevedi un messaggio nella parte superiore della pagina che indica che il failover è in corso. Compare inoltre un'icona accanto al tuo volume in **{{site.data.keyword.blockstorageshort}}** che indica che si sta verificando una transazione attiva. Se passi il puntatore del mouse sull'icona, verrà visualizzata una finestra che indica la transazione. Una volta completata la transazione, l'icona scompare. Durante il processo di failover, le azioni correlate alla configurazione sono di sola lettura. Non puoi modificare le pianificazioni delle istantanee o modificare lo spazio di istantanea. L'evento viene registrato nella cronologia replica.<br/> Una volta che il tuo volume di destinazione è operativo ricevi un altro messaggio. Il nome LUN del tuo volume di origine iniziale si aggiorna in "REP" e il suo stato diventa Inattivo.
4. Fai clic su **View All ({{site.data.keyword.blockstorageshort}})**.
5. Fai clic sul tuo LUN attivo (in precedenza il tuo volume di destinazione).
6. Monta o collega il tuo volume di archiviazione all'host. Fai clic [qui](provisioning-block_storage.html) per le istruzioni.


## Avvio di un failback da un volume alla sua replica

Dopo che il tuo volume di origine originale viene riparato, puoi avviare un failback controllato al tuo volume di origine originale. In un failback controllato,

- il volume di origine operativo viene portato offline,
- viene acquisita un'istantanea,
- il ciclo di replica viene completato,
- l'istantanea di dati appena acquisita viene attivata,
- e il volume di origine diventa attivo per il montaggio.

Una volta avviato un Failback, la relazione di replica viene nuovamente invertita. Il tuo volume di origine viene ripristinato come tuo volume di origine e il tuo volume di destinazione è nuovamente il volume di destinazione, come indicato dal nome LUN (**LUN Name**) seguito da **REP**.

I failback vengono avviati in **Storage**, **{{site.data.keyword.blockstorageshort}}** nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Fai clic sul tuo LUN attivo ("destinazione").
2. n alto a destra, fai clic su **Replica** e su **Actions**.
3. Seleziona **Failback**. >Prevedi un messaggio nella parte superiore della pagina che mostra che il failover è in corso. Compare inoltre un'icona accanto al tuo volume in **{{site.data.keyword.blockstorageshort}}** che indica che si sta verificando una transazione attiva. Se passi il puntatore del mouse sull'icona, verrà visualizzata una finestra che indica la transazione. Una volta completata la transazione, l'icona scompare. Durante il processo di failback, le azioni correlate alla configurazione sono di sola lettura. Non puoi modificare le pianificazioni delle istantanee o modificare lo spazio di istantanea. L'evento viene registrato nella cronologia replica.
4. In alto a destra, fai clic sul link **View All {{site.data.keyword.blockstorageshort}}**.
5. Fai clic sul tuo LUN attivo ("origine").
6. Monta o collega il tuo volume di archiviazione all'host. Fai clic [qui](provisioning-block_storage.html) per le istruzioni.


## Visualizzazione della cronologia replica

La cronologia replica può essere visualizzata in **Audit Log** tramite la scheda **Account** in **Manage**. Entrambi i volumi primario e di replica visualizzano una cronologia replica identica. La cronologia include:

- il tipo per la replica (failover o failback);
- quando è stata avviata
- l'istantanea utilizzata per la replica;
- la dimensione della replica;
- quando è stata completata.


## Creazione di un duplicato di una replica

Puoi creare un duplicato di un {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.blockstoragefull}} esistente. Il volume duplicato eredita per impostazione predefinita le opzioni di capacità e prestazioni del LUN/volume originale e ha una copia dei dati che arriva fino al punto temporale di un'istantanea.

I duplicati possono essere creati sia dal volume primario che da quello di replica. Il nuovo duplicato viene creato nello stesso data center del volume originale. Se crei un duplicato da un volume di replica, il nuovo volume viene creato nello stesso data center del volume di replica.

I volumi duplicati solo accessibili da un host per la lettura/scrittura non appena viene seguito il provisioning dell'archiviazione. Tuttavia, le istantanee e le repliche sono consentite solo dopo il completamento della copia dei dati dall'originale al duplicato.

Per ulteriori informazioni, vedi [Creazione di un volume di blocco duplicato](how-to-create-duplicate-volume.html)


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
