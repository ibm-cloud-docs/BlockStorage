---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: Block storage, new feature, adjusting IOPS, modify IOPS, increase IOPS, decrease IOPS,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Regolazione di IOPS
{: #adjustingIOPS}

Con questa nuova funzione, gli utenti di archiviazione {{site.data.keyword.blockstoragefull}} possono regolare l'IOPS del loro {{site.data.keyword.blockstorageshort}} esistente immediatamente. Non devono creare un duplicato o copiare manualmente i dati nella nuova archiviazione. Gli utenti non riscontreranno alcun tipo di interruzione o mancanza di accesso all'archiviazione mentre viene eseguita la regolazione.

La fatturazione per l'archiviazione viene aggiornata per aggiungere la differenza calcolata proporzionalmente del nuovo prezzo al ciclo di fatturazione corrente. L'intero nuovo ammontare verrà fatturato nel prossimo ciclo di fatturazione.


## Vantaggi dell'IOPS regolabile

- Gestione dei costi - alcuni dei clienti potrebbero avere bisogno di un IOPS elevato solo durante i tempi di utilizzo di picco. Ad esempio, un grosso negozio al dettaglio ha un utilizzo di picco durante i periodi festivi e potrebbe avere bisogno di un tasso di IOPS sulla sua archiviazione più elevato. Tuttavia, non hanno bisogno di un IOPS più elevato nel bel mezzo dell'estate. Questa funzione consente loro di gestire i loro costi e pagare un IOPS più elevato quando ne hanno bisogno.

## Limitazioni
{: #limitsofIOPSadjustment}

Questa funzione è disponibile solo in [data center selezionati](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).

I clienti non possono passare da Endurance a Performance e viceversa quando regolano il loro IOPS. Tuttavia, possono specificare un nuovo livello IOPS o un livello IOPS per la loro archiviazione sulla base dei seguenti criteri e delle seguenti limitazioni:

- Se il volume originale è un livello Endurance 0,25, il livello IOPS non può essere aggiornato.
- Se il volume originale è Performance con un numero inferiore o uguale a 0,30 IOPS/GB, le opzioni disponibili includono solo le combinazioni di dimensione e IOPS che danno un risultato inferiore o uguale a 0,30 IOPS/GB.
- Se il volume originale è Performance con più di 0,30 IOPS/GB, le opzioni disponibili includono solo le combinazioni di dimensione e IOPS che danno un risultato superiore a 0,30 IOPS/GB.

## Effetto della modifica dell'IOPS sulla replica

Se per il volume è implementata la replica, quest'ultima viene aggiornata automaticamente in modo che corrisponda alla selezione IOPS di quello primario.

## Modifica dell'IOPS sulla tua archiviazione
{: #adjustingsteps}

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


In alternativa, puoi regolare l'IOPS tramite la CLI SL.
```
# slcli block volume-modify --help
Usage: slcli block volume-modify [OPTIONS] VOLUME_ID

Options:
  -c, --new-size INTEGER        New Size of block volume in GB. ***If no size
                                is given, the original size of volume is
                                used.***
                                Potential Sizes: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [the original size of the volume]
  -i, --new-iops INTEGER        Performance Storage IOPS, between 100 and 6000
                                in multiples of 100 [only for performance
                                volumes] ***If no IOPS value is specified, the
                                original IOPS value of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is less than 0.3, new IOPS/GB
                                must also be less than 0.3. If original
                                IOPS/GB for the volume is greater than or
                                equal to 0.3, new IOPS/GB for the volume must
                                also be greater than or equal to 0.3.]
  -t, --new-tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) [only for
                                endurance volumes] ***If no tier is specified,
                                the original tier of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is 0.25, new IOPS/GB for the
                                volume must also be 0.25. If original IOPS/GB
                                for the volume is greater than 0.25, new
                                IOPS/GB for the volume must also be greater
                                than 0.25.]
  -h, --help      Show this message and exit.
```
{:codeblock}
