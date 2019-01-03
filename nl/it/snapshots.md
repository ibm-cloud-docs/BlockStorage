---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-13"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Istantanee

Le istantanee sono una funzione di {{site.data.keyword.blockstoragefull}}. Un'istantanea rappresenta il contenuto di un volume a uno specifico punto temporale. Con le istantanee, puoi proteggere i tuoi dati senza alcun effetto sulle prestazioni e un utilizzo minimo di spazio. Le istantanee sono considerate la prima linea di difesa per la protezione dei dati. Se un utente modifica o elimina accidentalmente dei dati cruciali da un volume, può ripristinarli in modo facile e veloce da una copia istantanea.

{{site.data.keyword.blockstorageshort}} ti fornisce due modi per acquisire le tue istantanee.

* Il primo consiste in una pianificazione delle istantanee configurabile che crea ed elimina le copie istantanea automaticamente per ciascun volume di archiviazione. Puoi anche creare pianificazioni delle istantanee extra, eliminare manualmente delle copie e gestire le pianificazioni in base ai tuoi requisiti.
* Il secondo modo consiste nell'acquisire un'istantanea manuale.

Una copia istantanea è un'immagine di sola lettura di un LUN {{site.data.keyword.blockstorageshort}} che acquisisce lo stato del volume a un punto temporale. Le copie istantanea sono efficienti sia per quanto riguarda il tempo necessario per crearlo che per quanto riguarda lo spazio di archiviazione. La creazione di una copia istantanea di un {{site.data.keyword.blockstorageshort}} richiede solo pochi secondi. Di norma impiega meno di 1 secondo, indipendentemente dalla dimensione del volume o dal livello di attività sull'archiviazione. Dopo che una copia istantanea viene creata, le modifiche agli oggetti di dati sono riflesse negli aggiornamenti alla versione corrente degli oggetti, come se le copie istantanea non esistessero. Nel frattempo, la copia dei dati rimane stabile.

Una copia istantanea non comporta alcuna diminuzione delle prestazioni. Gli utenti possono facilmente archiviare fino a 50 istantanee pianificate e 50 istantanee manuali per volume {{site.data.keyword.blockstorageshort}}, tutte accessibili come versioni di sola lettura e online dei dati.

Con le istantanee puoi:

- Creare in modo non distruttivo dei punti di ripristino a un punto temporale.
- Ripristinare i volumi a punti temporali precedenti.

Devi acquistare dello spazio di istantanea per il tuo volume prima di poter acquisirne delle istantanee. Lo spazio di istantanea può essere aggiunto durante l'ordinazione iniziale o successivamente tramite la pagina **Volume Details**. Le istantanee pianificate e manuali condividono lo spazio dell'istantanea, quindi assicurati di ordinare abbastanza spazio istantanea. Per ulteriori informazioni, consulta [Ordinazione di istantanee](ordering-snapshots.html).

## Procedure ottimali per le istantanee

La progettazione dell'istantanea dipende dall'ambiente del cliente. Le seguenti considerazioni sulla progettazione può aiutarti a pianificare e implementare copie istantanea:
- Puoi creare fino a 50 istantanee tramite la pianificazione e fino a 50 manualmente su ciascun volume o LUN.
- Non acquisire troppe istantanee. Assicurati che la tua frequenza di istantanee pianificate soddisfi le tue esigenze di obiettivo del tempo di ripristino (RTO) e obiettivo del punto di ripristino (RPO) e i tuoi requisiti di business applicativi pianificando istantanee orarie, giornaliere o settimanali.
- La funzione di eliminazione automatica (AutoDelete) di istantanea può essere utilizzata per controllare la crescita del consumo dell'archiviazione. <br/>

  La soglia di AutoDelete è fissata al 95 percento.
  {:note}

Le istantanee non sono sostituzioni della replica di ripristino di emergenza (Disaster Recovery) offsite o di un backup a lunga conservazione effettivi.
{:important}

## Sicurezza

Per impostazione predefinita, vengono crittografate anche tutte le istantanee e le repliche di {{site.data.keyword.filestorage_short}}. Questa funzione non può essere disattivata in base ai singoli volumi. Per ulteriori informazioni sulla crittografia dei dati inattivi gestita dal provider, consulta [Come rendere sicuri i tuoi dati](block-file-storage-encryption-rest.html).

## Come le istantanee influiscono sullo spazio su disco

Le copie istantanea riducono al minimo l'utilizzo di disco preservando i singoli blocchi invece dei file interi. Le copie istantanea usano spazio extra solo quando i file nel file system attivo vengono modificati o eliminati. Quando i file vengono modificati o eliminati, i blocchi file originali continuano a essere conservati come parte di una o più copie di istantanee.

Nel file system attivo, i blocchi modificati vengono riscritti in ubicazioni differenti sul disco oppure completamente rimossi come blocchi file attivi. Di conseguenza, oltre allo spazio su disco utilizzato dai blocchi nel file system attivo modificato, lo spazio su disco utilizzato dai blocchi originali continua a essere conservato per riflettere lo stesso del file system attivo prima della modifica.

<table>
    <colgroup>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
    </colgroup>
      <tr>
        <th colspan="3" style="border: 0.0px;text-align: center;">Utilizzo dello spazio su disco prima e dopo la copia istantanea</th>
     </tr><tr>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle1.png" alt="Prima della copia istantanea"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle3.png" alt="Dopo la copia istantanea"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle2.png" alt="Modifiche dopo la copia istantanea"></td>
     </tr><tr>
        <td style="border: 0.0px;">Prima che venga creata qualsiasi copia istantanea, lo spazio su disco è utilizzato solo dal file system attivo.</td>
        <td style="border: 0.0px;">Dopo la creazione di una copia istantanea, il file system attivo e la copia istantanea puntano agli stessi blocchi disco. La copia istantanea non usa spazio su disco extra.</td>
        <td style="border: 0.0px;">Dopo che <i>myfile.txt</i> è stato eliminato dal file system attivo, la copia istantanea continua a includere il file e i riferimenti ai suoi blocchi disco. Per tale motivo l'eliminazione dei dati del file system attivo non sempre libera spazio su disco.</td>
      </tr>
</table>

Per ulteriori informazioni su come visualizzare quanto spazio di istantanea è utilizzato, consulta [Gestione delle istantanee](working-with-snapshots.html).
