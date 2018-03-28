---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-23"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Regolazione di IOPS

Con questa nuova funzione, i nostri utenti di archiviazione {{site.data.keyword.blockstoragefull}} potranno regolare l'IOPS del loro {{site.data.keyword.blockstorageshort}} esistente senza causare alcuna interruzione, senza dover creare un duplicato o migrare manualmente i dati alla nuova archiviazione. L'utente non riscontrerà alcun tipo di interruzione o mancanza di accesso all'archiviazione mentre viene eseguita la regolazione. 

La fatturazione per l'archiviazione verrà aggiornata per aggiungere la differenza, calcolata proporzionalmente (pro-rata), del nuovo prezzo al ciclo di fatturazione corrente e l'intero nuovo ammontare verrà fatturato nel prossimo ciclo di fatturazione.

Questa funzione è disponibile solo in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html). 

## Perché avvantaggiarsi dell'IOPS regolabile?

- Gestione dei costi - alcuni dei nostri clienti potrebbero avere bisogno di un IOPS elevato solo durante i tempi di utilizzo di picco. Ad esempio, un grosso negozio al dettaglio ha un utilizzo di picco durante i periodi festivi e potrebbe avere bisogno di un IOPS sulla sua archiviazione più elevato rispetto a quello necessario nel bel mezzo dell'estate. Questa funzione consente loro di gestire i loro costi e pagare un IOPS più elevato solo quando ne hanno veramente bisogno.

## Ci sono delle limitazioni?

Questa funzione è disponibile solo per l'archiviazione di cui viene eseguito il provisioning nei [data center](new-ibm-block-and-file-storage-location-and-features.html) con funzionalità migliorate. 

I clienti non possono passare da Endurance a Performance e viceversa quando regolano il loro IOPS. Gli utenti possono specificare un nuovo livello IOPS o un livello IOPS per la loro archiviazione sulla base dei seguenti criteri e delle seguenti limitazioni: 

- Se il volume originale è un livello Endurance 0,25, il livello IOPS non può essere aggiornato.
- Se il volume originale è Performance con < 0,30 IOPS/GB, le opzioni disponibili dovrebbero includere solo le combinazioni di dimensione e IOPS che danno come risultato < 0,30 IOPS/GB. 
- Se il volume originale è Performance con >= 0,30 IOPS/GB, le opzioni disponibili dovrebbero includere solo le combinazioni di dimensione e IOPS che danno come risultato >= 0,30 IOPS/GB. Dimensione (superiore o uguale al volume originale)



## In che modo la regolazione dell'IOPS si ripercuote sulla replica?

Se per il volume è implementata la replica, quest'ultima verrà aggiornata automaticamente in modo che corrisponda alla selezione IOPS di quello primario. 

## Come regolo l'IOPS nella mia archiviazione?

1. Dal {{site.data.keyword.slportal}}, fai clic su **Storage** > **{{site.data.keyword.blockstorageshort}}** OPPURE, dal catalogo {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Seleziona il LUN dall'elenco e fai clic su **Actions** > **Modify LUN**
3. In **Storage IOPS Options**, effettua una nuova selezione.
    - Endurance (IOPS a livelli): seleziona un livello IOPS superiore a 0,25 IOPS/GB della tua archiviazione. Puoi aumentare il livello IOPS in qualsiasi momento. Tuttavia, in diminuzione è disponibile solo una volta al mese.
    - Performance (IOPS allocato): specifica la nuova opzione IOPS per la tua archiviazione immettendo un valore compreso tra 100 e 48.000 IOPS. (Assicurati di esaminare gli eventuali limiti specifici richiesti in base alla dimensione nel modulo dell'ordine).
4. Riesamina la tua selezione e il nuovo prezzo.
5. Fai clic sulla casella di spunta **I have read the Master Service Agreement...** e fai clic su **Place Order**.
6. La tua nuova allocazione di archiviazione dovrebbe essere disponibile in pochi minuti.
