---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Protezione dei tuoi dati - crittografia dei dati inattivi gestita dal provider

## Crittografia dei dati inattivi {{site.data.keyword.blockstorageshort}}

{{site.data.keyword.BluSoftlayer_full}} prende sul serio la sicurezza e comprende l'importanza di poter crittografare i dati per tenerli al sicuro. Con la crittografia gestita dal provider, il {{site.data.keyword.blockstoragefull}} di cui è stato eseguito il provisioning con Endurance o Performance è crittografato per impostazione predefinita senza costi aggiuntivi e ripercussioni sulle prestazioni.

La funzione di crittografia dei dati inattivi gestita dal provider utilizza i seguenti protocolli standard del settore:

* Crittografia AES-256 standard del settore
* Le chiavi sono gestite internamente con il KMIP (Key Management Interoperability Protocol) standard del settore
* L'archiviazione è convalidata per FIPS PUB (Federal Information Processing Standard Publication) 140-2, FISMA (Federal Information Security Management Act) e HIPAA (Health Insurance Portability and Accountability Act). L'archiviazione viene anche convalidata per la conformità a PCI (Payment Card Industry), Basel II, California Security Breach Information Act (SB 1386) e alla direttiva sulla protezione dei dati dell'Unione europea 95/46/EC.

## Fornire la crittografia dei dati inattivi per istantanee o archiviazione replicata  

Per impostazione predefinita, vengono crittografate anche tutte le istantanee e le repliche di {{site.data.keyword.blockstorageshort}}. Questa funzione non può essere disattivata in base ai singoli volumi.

## Provisioning di archiviazione con la crittografia

La funzione di crittografia dei dati inattivi gestita dal provider è disponibile per il {{site.data.keyword.blockstorageshort}} di cui viene eseguito il provisioning in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html). Tutta l'archiviazione ordinata in questi data center è automaticamente dotata della crittografia.

Quando ordini {{site.data.keyword.blockstorageshort}}, seleziona un data center indicato con un asterisco (`*`). Vedi un'icona di blocco a destra del campo LUN/Volume Name che indica che il volume è crittografato.

![L'icona di blocco indica che il LUN è crittografato](/images/encryptedstorage.png)
<caption>Figura 1. Esempio di icona di blocco che indica che il LUN è crittografato.</caption>



L'archiviazione non crittografata di cui viene eseguito il provisioning prima dell'upgrade del data center **non viene** crittografata automaticamente. Se hai la tua archiviazione non crittografata in un data center di cui è stato eseguito l'upgrade e vuoi che venga crittografata, devi creare un nuovo volume e migrare i tuoi dati. Per ulteriori informazioni, vedi [Migrazione di {{site.data.keyword.blockstorageshort}} in data center di cui è stato eseguito l'upgrade](migrate-block-storage-encrypted-block-storage.html).
{:important}
