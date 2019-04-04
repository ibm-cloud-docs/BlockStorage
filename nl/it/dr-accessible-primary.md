---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, accessible Primary volume, duplicate of a replica volume, Disaster Recovery, volume duplication, replication, failover, failback

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Ripristino di emergenza - Failover con un volume primario accessibile
{: #dr-accessible}

Se si verifica un errore catastrofico o un'emergenza sul sito primario e l'archiviazione primaria è ancora accessibile, i clienti possono eseguire le seguenti azioni per accedere rapidamente ai loro dati sul sito secondario.

Prima di avviare il failover, assicurati che sia in vigore tutta l'autorizzazione host.

Gli host autorizzati e i volumi si devono trovare nello stesso data center. Ad esempio, non puoi avere un volume di replica a Londra e l'host ad Amsterdam. Entrambi devono essere a Londra o entrambi devono essere ad Amsterdam.
{:note}

1. Accedi alla [Console {{site.data.keyword.cloud}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/catalog){:new_window} e fai clic sull'icona **menu** in alto a sinistra. Seleziona **Classic Infrastructure**.


   In alternativa, puoi accedere al [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window}.
2. Fai clic sul tuo volume di origine o di destinazione dall'una o dall'altra pagina **{{site.data.keyword.blockstorageshort}}**-
3. Fai clic su **Replica**.
4. Scorri giù al frame **Authorize Hosts** e fai clic su **Authorize Hosts** sulla destra.
5. Evidenzia l'host che deve essere autorizzato per le repliche. Per selezionare più host, utilizza il tasto CTRL e fai clic sugli host applicabili.
6. Fai clic su **Submit**. Se non hai alcun host disponibile, ti viene richiesto di acquistare delle risorse di calcolo nello stesso data center.


## Avvio di un failover da un volume alla sua replica

Se è imminente un evento di errore, puoi avviare un **failover** al tuo volume di destinazione. Il volume di destinazione diventa attivo. L'ultima istantanea replicata correttamente viene attivata e il volume viene reso disponibile per il montaggio. Tutti i dati che erano stati scritti nel volume di origine a partire dal ciclo di replica precedente vengono perduti. Una volta avviato un failover, la relazione di replica viene invertita. Il tuo volume di destinazione diventa il tuo volume di origine e il tuo precedente volume di origine diventa la tua destinazione, come indicato dal nome LUN (**LUN Name**) seguito da **REP**.

I failover vengono avviati in **Storage**, **{{site.data.keyword.blockstorageshort}}** nel [[{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window}.

**Prima di procedere con questa procedura, disconnetti il volume. In caso contrario, si verifica un danneggiamento e una perdita di dati.**

1. Fai clic sul tuo LUN attivo ("origine").
2. Fai clic su **Replica** e fai clic su **Actions**.
3. Seleziona **Failover**.

   Prevedi un messaggio nella pagina che indica che il failover è in corso. Compare inoltre un'icona accanto al tuo volume in **{{site.data.keyword.blockstorageshort}}** che indica che si sta verificando una transazione attiva. Se passi il puntatore del mouse sull'icona, verrà visualizzata una finestra che indica la transazione. Una volta completata la transazione, l'icona scompare. Durante il processo di failover, le azioni correlate alla configurazione sono di sola lettura. Non puoi modificare le pianificazioni delle istantanee o modificare lo spazio di istantanea. L'evento viene registrato nella cronologia replica.<br/> Una volta che il tuo volume di destinazione è operativo, ricevi un altro messaggio. Il nome LUN del tuo volume di origine iniziale si aggiorna in "REP" e il suo stato diventa Inattivo.
   {:note}
4. Fai clic su **View All ({{site.data.keyword.blockstorageshort}})**.
5. Fai clic sul tuo LUN attivo (in precedenza il tuo volume di destinazione).
6. Monta o collega il tuo volume di archiviazione all'host. Fai clic [qui](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole) per le istruzioni.


## Avvio di un failback da un volume alla sua replica

Dopo che il tuo volume di origine originale viene riparato, puoi avviare un failback controllato al tuo volume di origine originale. In un failback controllato,

- il volume di origine operativo viene portato offline,
- viene acquisita un'istantanea,
- il ciclo di replica viene completato,
- l'istantanea di dati appena acquisita viene attivata,
- e il volume di origine diventa attivo per il montaggio.

Una volta avviato un failback, la relazione di replica viene nuovamente invertita. Il tuo volume di origine viene ripristinato come tuo volume di origine e il tuo volume di destinazione è nuovamente il volume di destinazione, come indicato dal nome LUN (**LUN Name**) seguito da **REP**.

I failback vengono avviati in **Storage**, **{{site.data.keyword.blockstorageshort}}** nel [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window}.

1. Fai clic sul tuo LUN attivo ("destinazione").
2. n alto a destra, fai clic su **Replica** e su **Actions**.
3. Seleziona **Failback**.

   Prevedi un messaggio nella pagina che mostra che il failover è in corso. Compare inoltre un'icona accanto al tuo volume in **{{site.data.keyword.blockstorageshort}}** che indica che si sta verificando una transazione attiva. Se passi il puntatore del mouse sull'icona, verrà visualizzata una finestra che indica la transazione. Una volta completata la transazione, l'icona scompare. Durante il processo di failback, le azioni correlate alla configurazione sono di sola lettura. Non puoi modificare le pianificazioni delle istantanee o modificare lo spazio di istantanea. L'evento viene registrato nella cronologia replica.
   {:note}
4. In alto a destra, fai clic sul link **View All {{site.data.keyword.blockstorageshort}}**.
5. Fai clic sul tuo LUN attivo ("origine").
6. Monta o collega il tuo volume di archiviazione all'host. Fai clic [qui](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole) per le istruzioni.
