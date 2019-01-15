---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Regolazione di IOPS

Con questa nuova funzione, gli utenti di archiviazione {{site.data.keyword.blockstoragefull}} possono regolare l'IOPS del loro {{site.data.keyword.blockstorageshort}} esistente immediatamente. Non devono creare un duplicato o copiare manualmente i dati nella nuova archiviazione. Gli utenti non riscontreranno alcun tipo di interruzione o mancanza di accesso all'archiviazione mentre viene eseguita la regolazione.

La fatturazione per l'archiviazione viene aggiornata per aggiungere la differenza calcolata proporzionalmente del nuovo prezzo al ciclo di fatturazione corrente. L'intero nuovo ammontare verrà fatturato nel prossimo ciclo di fatturazione.


## Vantaggi dell'IOPS regolabile

- Gestione dei costi - alcuni dei clienti potrebbero avere bisogno di un IOPS elevato solo durante i tempi di utilizzo di picco. Ad esempio, un grosso negozio al dettaglio ha un utilizzo di picco durante i periodi festivi e potrebbe avere bisogno di un tasso di IOPS sulla sua archiviazione più elevato. Tuttavia, non hanno bisogno di un IOPS più elevato nel bel mezzo dell'estate. Questa funzione consente loro di gestire i loro costi e pagare un IOPS più elevato quando ne hanno bisogno.

## Limitazioni

Questa funzione è disponibile solo in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html).

I clienti non possono passare da Endurance a Performance e viceversa quando regolano il loro IOPS. Tuttavia, possono specificare un nuovo livello IOPS o un livello IOPS per la loro archiviazione sulla base dei seguenti criteri e delle seguenti limitazioni:

- Se il volume originale è un livello Endurance 0,25, il livello IOPS non può essere aggiornato.
- Se il volume originale è Performance con un numero inferiore o uguale a 0,30 IOPS/GB, le opzioni disponibili includono solo le combinazioni di dimensione e IOPS che danno un risultato inferiore o uguale a 0,30 IOPS/GB.
- Se il volume originale è Performance con più di 0,30 IOPS/GB, le opzioni disponibili includono solo le combinazioni di dimensione e IOPS che danno un risultato superiore a 0,30 IOPS/GB.

## Effetto della modifica dell'IOPS sulla replica

Se per il volume è implementata la replica, quest'ultima viene aggiornata automaticamente in modo che corrisponda alla selezione IOPS di quello primario.

## Modifica dell'IOPS sulla tua archiviazione

1. Vai al tuo elenco di {{site.data.keyword.blockstorageshort}}
   - Da {{site.data.keyword.slportal}}, fai clic su **Storage** > **{{site.data.keyword.blockstorageshort}}**
   - Dalla console {{site.data.keyword.BluSoftlayer_full}} fai clic su **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Seleziona il LUN dall'elenco e fai clic su **Actions** > **Modify LUN**
3. In **Storage IOPS Options**, effettua una nuova selezione.
    - Per Endurance (IOPS a livelli), seleziona un livello IOPS superiore a 0,25 IOPS/GB della tua archiviazione. Puoi aumentare il livello IOPS in qualsiasi momento. Tuttavia, in diminuzione è disponibile solo una volta al mese.
    - Per Performance (IOPS allocato), specifica la nuova opzione IOPS per la tua archiviazione immettendo un valore compreso tra 100 e 48.000 IOPS.
    
    Assicurati di esaminare gli eventuali limiti specifici richiesti in base alla dimensione nel modulo dell'ordine.
    {:tip}
4. Riesamina la tua selezione e la nuova determinazione del prezzo.
5. Fai clic sulla casella di spunta **I have read the Master Service Agreement...** e fai clic su **Place Order**.
6. La tua nuova allocazione di archiviazione è disponibile in pochi minuti.
