---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Gestione delle istantanee

## Come creo una pianificazione delle istantanee?

Le pianificazioni delle istantanee ti consentono di decidere con che frequenza e quando vuoi creare un riferimento ad un punto temporale del tuo volume di archiviazione. Puoi avere un massimo di 50 istantanee per volume di archiviazione. Le pianificazioni vengono gestite tramite la scheda **Storage**, **{{site.data.keyword.blockstorageshort}}** del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

Prima di poter configurare la tua pianificazione iniziale, devi procedere all'acquisto di spazio di istantanea, se non lo hai fatto durante il provisioning iniziale del volume di archiviazione.

### Come aggiungo una pianificazione delle istantanee

Le pianificazioni delle istantanee possono essere configurate per intervalli orari, giornalieri e settimanali, ciascuno con un distinto ciclo di conservazione. C'è un massimo di 50 istantanee pianificate, che può essere una combinazione di pianificazioni orarie, giornaliere e settimanali e di istantanee manuali per volume di archiviazione.

1. Fai clic sul tuo volume di archiviazione, fai clic sulla casella a discesa **Actions** e fai clic su **Schedule Snapshot**.
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
3. Fai clic su **Save** e crea un'altra pianificazione con una frequenza differente. Nota: riceverai un messaggio di avvertenza e non potrai eseguire il salvataggio se il numero totale di istantanee pianificate è superiore a 50.

Un elenco delle istantanee verrà visualizzato man mano che vengono eseguite nella sezione Snapshots della pagina Detail.

## Come eseguo un'istantanea manuale?

Le istantanee manuali possono essere eseguite a vari punti durante un upgrade o una manutenzione dell'applicazione. Ti consentono anche di eseguire delle istantanee su più macchine che sono state temporaneamente disattivate a livello dell'applicazione.

C'è un massimo di 50 istantanee manuali per volume di archiviazione.

1. Fai clic sul volume di archiviazione.
2. Fai clic sulla casella a discesa Actions.
3. Fai clic su **Take Manual Snapshot**.
L'istantanea verrà acquisita e verrà visualizzata nella sezione Snapshots della pagina Detail. La sua pianificazione sarà manuale (Manual).

## Come vedo un elenco di istantanee con lo spazio consumato e le funzioni di gestione?

Un elenco di istantanee conservate e spazio consumato può essere visualizzato nella pagina **Detail** (Storage, {{site.data.keyword.blockstorageshort}}). Le funzioni di gestione (modifica di pianificazioni e aggiunta di ulteriore spazio) vengono controllate nella pagina Detail utilizzando il menu a discesa **Actions** o i link nelle diverse sezioni della pagina.

## Come visualizzo un elenco di istantanee conservate?

Le istantanee conservate sono basate sul numero che hai immesso nel campo Keep the last quando configuri le tue pianificazioni. Puoi visualizzare le istantanee che hai acquisito nella sezione Snapshot. Le istantanee sono elencate in base alla pianificazione.

## Come visualizzo quanto spazio di istantanea è stato utilizzato?

Il grafico a torta nella parte superiore della pagina Details visualizza quanto spazio è stato utilizzato e quanto spazio è rimasto. Riceverai delle notifiche quando inizi a raggiungere le soglie di spazio – 75%, 90%, 95%.

## Come modifico la quantità di spazio di istantanea per il mio volume?

Potresti aver bisogno di aggiungere dello spazio di istantanea a un volume che in precedenza non ne aveva o che potrebbe averne bisogno. Puoi aggiungere tra i 5 GB e i 4.000 GB, a seconda delle tue esigenze. 

**Nota**: lo spazio di istantanea può essere solo aumentato e non ridotto. Dovresti selezionare una quantità di spazio più piccola finché non determini di quanto spazio hai effettivamente bisogno. Ricordati che le istantanee automatizzate e quelle manuali condividono lo stesso spazio.

Lo spazio di istantanea viene modificato tramite **Storage, {{site.data.keyword.blockstorageshort}}**.

1. Fai clic sui tuoi volumi di archiviazione, fai clic sulla casella a discesa **Actions** e fai clic su **Add More Snapshot Space**.
2. Seleziona un intervallo di dimensioni dal prompt. Le dimensioni di norma vanno da 0 alla dimensione del tuo volume.
3. Fai clic su **Continue** per eseguire il provisioning dello spazio aggiuntivo.
4. Immetti l'eventuale codice promozionale (Promo Code) a tua disposizione e fai clic su **Recalculate**. Gli addebiti (Charges) per questo ordine e il riesame dell'ordine (Order Review) avranno i valori predefiniti.
5. Fai clic sulla casella di spunta **I have read the Master Service Agreement…** e fai clic su **Place Order**. Nel giro di pochi minuti, verrà eseguito il provisioning del tuo spazio di istantanea aggiuntivo.

## Come ricevo delle notifiche quando sono prossimo al mio limite di spazio di istantanea e quando vengono eliminate le istantanee?

Le notifiche vengono inviate tramite i ticket di supporto da Support al Master User sul tuo account quando raggiungi tre diverse soglie di spazio– 75%, 90%, 95%. 
- **Capacità al 75% **: viene inviata un'avvertenza che l'utilizzo dello spazio di istantanea ha superato il 75%. Se presti attenzione all'avvertenza e aggiungi dello spazio manualmente oppure elimini delle istantanee conservate e non necessarie, l'azione viene annotata e il ticket viene chiuso. Se non fai niente, devi confermare manualmente il ticket che viene quindi chiuso.
- **Capacità al 90% **: una seconda avvertenza viene inviata quando l'utilizzo dello spazio di istantanea ha superato il 90%. In modo analogo al raggiungimento della capacità al 75% , se esegui le azioni necessarie per ridurre lo spazio utilizzato, l'azione viene annotata e il ticket viene chiuso. Se non fai niente, devi confermare manualmente il ticket che viene quindi chiuso.
- **Capacità al 95% **: viene inviata un'avvertenza finale. Se non viene eseguita alcuna azione per portare il tuo spazio al di sotto della soglia, viene generata una notifica e si verifica un'eliminazione automatica in modo che sia possibile creare delle future istantanee. Le istantanee pianificate vengono eliminate, a partire da quella meno recente, finché l'utilizzo non scende al di sotto del 95%, e continueranno a essere eliminate ogni volta che l'utilizzo supera il 95% finché non scende al di sotto della soglia. Se lo spazio viene aumentato manualmente oppure se le istantanee vengono eliminate, l'avvertenza verrà reimpostata e generata nuovamente se la soglia viene superata nuovamente. Se non viene eseguita alcuna azione, questa sarà la sola avvertenza che verrà ricevuta.

## Come elimino una pianificazione delle istantanee?

Le pianificazioni delle istantanee possono essere annullate tramite **Storage, {{site.data.keyword.blockstorageshort}}**.

1. Fai clic sulla pianificazione da eliminare nel frame **Snapshot Schedules** nella pagina **Details**.
2. Fai clic nella casella di spunta accanto alla pianificazione da eliminare e fai clic su **Save**.<br />
**Attenzione**: se stai utilizzando la funzione di replica, assicurati che la pianificazione che stai eliminando non sia la pianificazione utilizzata dalla replica. Fai clic [qui](replication.html) per ulteriori informazioni sull'eliminazione di una pianificazione replica.

## Come elimino un'istantanea?

Le istantanee che non sono più necessarie possono essere rimosse manualmente per liberare spazio per le future istantanee. L'eliminazione viene eseguita tramite **Storage, {{site.data.keyword.blockstorageshort}}**.

1. Fai clic sul tuo volume di archiviazione e scorri verso il basso alla sezione Snapshot per visualizzare un elenco delle istantanee esistenti.
2. Fai clic sull'elenco a discesa **Actions** accanto a una specifica istantanea e fai clic su **Delete** per eliminare l'istantanea. Ciò non avrà alcuna ripercussione sulle eventuali istantanee passate o future nella stessa pianificazione poiché le istantanee non hanno alcuna interdipendenza.

Le istantanee manuali che non sono eliminate nel modo sopra descritto sono eliminate automaticamente (prima quella meno recente) quando raggiungi le limitazioni di spazio.

## Come ripristino il mio volume di archiviazione a uno specifico punto temporale utilizzando un'istantanea?

Potresti dover riportare il tuo volume di archiviazione a uno specifico punto temporale a causa di un errore utente o di un danneggiamento dei dati.

1. Smonta e scollega il tuo volume di archiviazione dall'host.
   - Fai clic [qui](accessing_block_storage_linux.html) per le istruzioni di {{site.data.keyword.blockstorageshort}} su Linux.
   - Fai clic [qui](accessing-block-storage-windows.html) per le istruzioni di {{site.data.keyword.blockstorageshort}} su Microsoft Windows.
2. Fai clic su **Storage**, **{{site.data.keyword.blockstorageshort}}** nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
3. Scorri verso il basso e fai clic sul tuo volume da ripristinare. La sezione **Snapshots** della pagina **Detail** visualizzerà un elenco di tutte le istantanee salvate insieme alla loro dimensione e alla loro data di creazione.
4. Fai clic sul pulsante **Actions** per l'istantanea da utilizzare e fai clic su **Restore**. <br/>
  **Nota**: l'esecuzione di un ripristino produrrà una perdita dei dati creati o modificati a partire da quando era stata acquisita l'istantanea che si sta utilizzando. Una volta completata l'operazione, il tuo volume di archiviazione verrà ripristinato allo stesso stato in cui si trovava quando era stata acquisita l'istantanea. Verrà visualizzato un prompt che ti informerà della cosa.
5. Fai clic su **Sì** per avviare il ripristino. Riceverai un messaggio nella parte superiore della pagina che indica che il volume è stato ripristinato utilizzando l'istantanea selezionata. Comparirà inoltre un'icona accanto al tuo volume in {{site.data.keyword.blockstorageshort}} che indica che è in corso una transazione attiva. Se passi il puntatore del mouse sull'icona, verrà visualizzata una finestra di dialogo che indica la transazione. Una volta completata la transazione, l'icona scomparirà.
6. Monta e ricollega il tuo volume di archiviazione all'host.
   - Fai clic [qui](accessing_block_storage_linux.html) per le istruzioni di {{site.data.keyword.blockstorageshort}} su Linux.
   - Fai clic [qui](accessing-block-storage-windows.html) per le istruzioni di {{site.data.keyword.blockstorageshort}} su Microsoft Windows.
   
**Nota**: il ripristino di un volume comporterà l'eliminazione di tutte le istantanee antecedenti all'istantanea ripristinata.
