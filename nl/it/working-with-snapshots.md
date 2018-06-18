---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Gestione delle istantanee

## Come creo una pianificazione delle istantanee?

Con le pianificazioni delle istantanee decidi con che frequenza e quando vuoi creare un riferimento ad un punto temporale del tuo volume di archiviazione. Puoi avere un massimo di 50 istantanee per volume di archiviazione. Le pianificazioni vengono gestite tramite la scheda **Storage**, **{{site.data.keyword.blockstorageshort}}** del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

Prima di poter configurare la tua pianificazione iniziale, devi procedere all'acquisto di spazio di istantanea, se non lo hai fatto durante il provisioning iniziale del volume di archiviazione.

### Come aggiungo una pianificazione delle istantanee

Le pianificazioni delle istantanee possono essere configurate per intervalli orari, giornalieri e settimanali, ciascuno con un distinto ciclo di conservazione. C'è un massimo di 50 istantanee pianificate, che può essere una combinazione di pianificazioni orarie, giornaliere e settimanali e di istantanee manuali per volume di archiviazione.

1. Fai clic sul tuo volume di archiviazione, fai clic su **Actions** e fai clic su **Schedule Snapshot**.
2. Nella finestra New Schedule Snapshot ci sono tre diverse frequenze di istantanea da cui scegliere. Usa qualsiasi combinazione di queste tre frequenze per creare una pianificazione delle istantanee completa.
   - Hourly
      - Specifica il minuto di ciascuna ora in cui deve essere eseguita un'istantanea; il valore predefinito è il minuto corrente.
      - Specifica il numero di istantanee orarie da conservare prima di eliminare quella meno recente.
   - Daily
      - Specifica l'ora e il minuto in cui deve essere eseguita un'istantanea; il valore predefinito sono l'ora e il minuto correnti.
      - Seleziona il numero di istantanee orarie da conservare prima di eliminare quella meno recente.
   - Weekly
      - Specifica il giorno della settimana, l'ora e il minuto in cui deve essere eseguita un'istantanea; il valore predefinito sono il giorno, l'ora e il minuto correnti.
      - Seleziona il numero di istantanee settimanali da conservare prima di eliminare quella meno recente.
3. Fai clic su **Save** e crea un'altra pianificazione con una frequenza differente. Se il numero totale di istantanee pianificate è superiore e 50, riceverai un messaggio di avvertenza e non potrai eseguire il salvataggio.

L'elenco delle istantanee viene visualizzato man mano che vengono eseguite nella sezione **Snapshots** della pagina **Detail**.

## Come acquisisco un'istantanea manuale?

Le istantanee manuali possono essere eseguite a vari punti durante un upgrade o una manutenzione dell'applicazione. Puoi anche acquisire istantanee su più macchine che sono state temporaneamente disattivate a livello dell'applicazione.

C'è un massimo di 50 istantanee manuali per volume di archiviazione.

1. Fai clic sul tuo volume di archiviazione.
2. Fai clic su **Actions**.
3. Fai clic su **Take Manual Snapshot**.
L'istantanea viene acquisita e visualizzata nella sezione **Snapshots** della pagina **Detail**. La sua pianificazione sarà manuale (Manual).

## Come vedo un elenco di istantanee con lo spazio utilizzato e le funzioni di gestione?

Un elenco di istantanee conservate e spazio consumato può essere visualizzato nella pagina **Detail** (Storage, {{site.data.keyword.blockstorageshort}}). Le funzioni di gestione (modifica di pianificazioni e aggiunta di ulteriore spazio) vengono eseguite nella pagina Detail utilizzando il menu **Actions** o i link nelle diverse sezioni della pagina.

## Come visualizzo un elenco delle istantanee conservate?

Le istantanee conservate sono basate sul numero che hai immesso nel campo **Keep the last** quando configuri le tue pianificazioni. Puoi visualizzare le istantanee che hai acquisito nella sezione **Snapshot**. Le istantanee sono elencate in base alla pianificazione.

## Come posso vedere quanto spazio di istantanea è stato utilizzato?

Il grafico a torta nella parte superiore della pagina **Details** visualizza quanto spazio è stato utilizzato e quanto spazio è rimasto. Riceverai delle notifiche quando raggiungerai le soglie di spazio – 75 percento, 90 percento e 95 percento.

## Come modifico la quantità di spazio di istantanea per il mio volume?

Potresti aver bisogno di aggiungere dello spazio di istantanea a un volume che in precedenza non ne aveva o che potrebbe averne bisogno. Puoi aggiungere da 5 GB a 4.000 GB, a seconda delle tue esigenze. 

**Nota**: lo spazio di istantanea può essere solo aumentato e non ridotto. Dovresti selezionare una quantità di spazio più piccola finché non determini di quanto spazio hai effettivamente bisogno. Ricordati che le istantanee automatizzate e quelle manuali condividono lo spazio.

Lo spazio di istantanea viene modificato tramite **Storage** > **{{site.data.keyword.blockstorageshort}}**.

1. Fai clic sui tuoi volumi di archiviazione, fai clic su **Actions** e fai clic su **Add More Snapshot Space**.
2. Seleziona un intervallo di dimensioni dal prompt. Le dimensioni di norma vanno da 0 alla dimensione del tuo volume.
3. Fai clic su **Continue**.
4. Immetti l'eventuale codice promozionale (Promo Code) a tua disposizione e fai clic su **Recalculate**. Gli addebiti (Charges) per questo ordine e il riesame dell'ordine (Order Review) vengono completati per impostazione predefinita.
5. Fai clic sulla casella di spunta **I have read the Master Service Agreement…** e fai clic su **Place Order**. Nel giro di pochi minuti, verrà eseguito il provisioning del tuo spazio di istantanea aggiuntivo.

## Come ricevo delle notifiche quando sono prossimo al mio limite di spazio di istantanea e quando vengono eliminate le istantanee?

Le notifiche vengono inviate tramite i ticket di supporto all'utente master sul tuo account quando raggiungi tre diverse soglie di spazio – 75 percento, 90 percento e 95 percento.

- **Capacità al 75 percento **: viene inviata un'avvertenza che l'utilizzo dello spazio di istantanea ha superato il 75 percento. Se presti attenzione all'avvertenza e aggiungi dello spazio manualmente oppure elimini delle istantanee conservate e non necessarie, l'azione viene annotata e il ticket viene chiuso. Se non fai niente, devi confermare manualmente il ticket che viene quindi chiuso.
- **Capacità al 90 percento **: viene inviata una seconda avvertenza quando l'utilizzo dello spazio di istantanea ha superato il 90 percento. In modo analogo al raggiungimento della capacità al 75 percento, se esegui le azioni necessarie per ridurre lo spazio utilizzato, l'azione viene annotata e il ticket viene chiuso. Se non fai niente, devi confermare manualmente il ticket che viene quindi chiuso.
- **Capacità al 95 percento **: viene inviata un'avvertenza finale. Se non viene eseguita alcuna azione per portare il tuo spazio al di sotto della soglia, viene generata una notifica e si verifica un'eliminazione automatica in modo che sia possibile creare delle future istantanee. Le istantanee pianificate vengono eliminate, a partire da quella meno recente, finché l'utilizzo non scende al di sotto del 95 percento.Le istantanee continueranno a essere eliminate ogni volta che l'utilizzo supera il 95 percento finché non scende al di sotto della soglia. Se lo spazio viene aumentato manualmente oppure se le istantanee vengono eliminate, l'avvertenza verrà reimpostata ed emessa nuovamente se la soglia viene superata nuovamente. Se non viene eseguita alcuna azione, questa sarà la sola avvertenza che verrà ricevuta.

## Come elimino una pianificazione delle istantanee?

Le pianificazioni delle istantanee possono essere eliminate tramite **Storage** > **{{site.data.keyword.blockstorageshort}}**.

1. Fai clic sulla pianificazione da eliminare nel frame **Snapshot Schedules** nella pagina **Details**.
2. Fai clic sulla casella di spunta accanto alla pianificazione da eliminare e fai clic su **Save**.<br />
**Attenzione**: se stai utilizzando la funzione di replica, assicurati che la pianificazione che stai eliminando non sia la pianificazione utilizzata dalla replica. Fai clic [qui](replication.html) per ulteriori informazioni sull'eliminazione di una pianificazione replica.

## Come elimino un'istantanea?

Le istantanee che non sono più necessarie possono essere rimosse manualmente per liberare spazio per le future istantanee. L'eliminazione viene eseguita tramite **Storage** > **{{site.data.keyword.blockstorageshort}}**.

1. Fai clic sul tuo volume di archiviazione e scorri alla sezione **Snapshot** per visualizzare l'elenco delle istantanee esistenti.
2. Fai clic su **Actions** accanto a una specifica istantanea e fai clic su **Delete** per eliminare l'istantanea. Ciò non avrà alcuna ripercussione sulle eventuali istantanee passate o future nella stessa pianificazione poiché le istantanee non hanno alcuna interdipendenza.

Le istantanee manuali che non sono eliminate nel modo descritto in precedenza sono eliminate automaticamente (prima quella meno recente) quando raggiungi le limitazioni di spazio.

## Come ripristino il mio volume di archiviazione a uno specifico punto temporale utilizzando un'istantanea?

Potresti dover riportare il tuo volume di archiviazione a uno specifico punto temporale a causa di un errore utente o di un danneggiamento dei dati.

1. Smonta e scollega il tuo volume di archiviazione dall'host.
   - Fai clic [qui](accessing_block_storage_linux.html) per le istruzioni di {{site.data.keyword.blockstorageshort}} su Linux.
   - Fai clic [qui](accessing-block-storage-windows.html) per le istruzioni di {{site.data.keyword.blockstorageshort}} su Microsoft Windows.
2. Fai clic su **Storage**, **{{site.data.keyword.blockstorageshort}}** nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
3. Scorri verso il basso e fai clic sul tuo volume da ripristinare. la sezione **Snapshots** della pagina **Detail** visualizza l'elenco di tutte le istantanee selezionate insieme alle relative dimensione e data di creazione.
4. Fai clic su **Actions** accanto all'istantanea da utilizzare e fai clic su **Restore**. <br/>
  **Nota**: l'esecuzione di un ripristino produrrà una perdita dei dati creati o modificati a partire da quando era stata acquisita l'istantanea che si sta utilizzando. Una volta completata l'operazione, il tuo volume di archiviazione verrà ripristinato allo stesso stato in cui si trovava quando era stata acquisita l'istantanea.  
5. Fai clic su **Sì** per avviare il ripristino. Prevedi un messaggio nella parte superiore della pagina che indica che il volume è stato ripristinato utilizzando l'istantanea selezionata. Compare inoltre un'icona accanto al tuo volume in {{site.data.keyword.blockstorageshort}} che indica che è in corso una transazione attiva. Se passi il puntatore del mouse sull'icona, verrà visualizzata una finestra di dialogo che indica la transazione. Una volta completata la transazione, l'icona scomparirà.
6. Monta e ricollega il tuo volume di archiviazione all'host.
   - Fai clic [qui](accessing_block_storage_linux.html) per le istruzioni di {{site.data.keyword.blockstorageshort}} su Linux.
   - Fai clic [qui](accessing-block-storage-windows.html) per le istruzioni di {{site.data.keyword.blockstorageshort}} su Microsoft Windows.
   
**Nota**: il ripristino di un volume comporterà l'eliminazione di tutte le istantanee antecedenti all'istantanea ripristinata.
