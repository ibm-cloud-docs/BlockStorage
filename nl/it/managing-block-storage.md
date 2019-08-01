---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-22"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Gestione di {{site.data.keyword.blockstorageshort}}
{: #managingstorage}

Puoi gestire i tuoi volumi {{site.data.keyword.blockstoragefull}} tramite la [console {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external}. Dal **menu**, seleziona **Classic Infrastructure** per interagire con i servizi classici.

## Visualizzazione dei dettagli di un LUN {{site.data.keyword.blockstorageshort}}

Puoi visualizzare un riepilogo delle informazioni chiave per il LUN di archiviazione selezionato, comprese le capacità di istantanea e replica supplementari che sono state aggiunte all'archiviazione.

1. Fai clic su **Storage**, **{{site.data.keyword.blockstorageshort}}**.
2. Fai clic sul nome del volume appropriato dall'elenco.

In alternativa, puoi utilizzare il seguente comando nella SLCLI.
```
# slcli block volume-detail --help
Usage: slcli block volume-detail [OPTIONS] VOLUME_ID

Options:
  -h, --help  Show this message and exit.
```

## Autorizzazione degli host ad accedere a {{site.data.keyword.blockstorageshort}}

Gli host "autorizzati" sono host a cui è stato concesso l'accesso a uno specifico LUN. Senza l'autorizzazione host, non puoi accedere all'archiviazione dal tuo sistema né utilizzarla. Autorizzare un host ad accedere al tuo LUN genera il nome utente, la password e l'IQN (iSCSI qualified name) necessari per montare la connessione iSCSI MPIO (multipath I/O).

Puoi autorizzare e connettere gli host che si trovano nello stesso data center della tua archiviazione. Puoi avere più account ma non puoi autorizzare un host da un account ad accedere alla tua archiviazione su un altro account.
{:important}

2. Fai clic su **Storage** > **{{site.data.keyword.blockstorageshort}}**.
3. Individua il volume e fai clic su **...**.
4. Fai clic su **Authorize Host**.
5. Per visualizzare l'elenco di dispositivi o indirizzi IP disponibili, devi prima selezionare se vuoi autorizzare l'accesso basato sul tipo di dispositivo o sulle sottoreti.
   - se scegli Devices, puoi scegliere tra istanze del server bare metal o virtuali.
   - se scegli IP Address, devi prima selezionare la sottorete in cui risiede il tuo host.
6. Dall'elenco filtrato, seleziona uno o più host che possono accedere al volume e fai clic su **Save**.

In alternativa, puoi utilizzare il seguente comando nella SLCLI.
```
# slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of a hardware server to authorize.
  -v, --virtual-id TEXT     The ID of a virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of an IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  --help                    Show this message and exit.
```

## Visualizzazione dell'elenco di host che sono autorizzati ad accedere a un LUN {{site.data.keyword.blockstorageshort}}

1. Fai clic su **Storage** > **{{site.data.keyword.blockstorageshort}}** e fai clic sul tuo nome del volume.
2. Scorri verso il basso alla sezione **Authorized Hosts**.

Qui puoi vedere un elenco di host che sono attualmente autorizzati ad accedere al LUN. Puoi inoltre vedere le informazioni di autenticazione necessarie per stabilire una connessione – nome utente, password e IQN host. L'indirizzo di destinazione è elencato nella pagina **Storage Detail**. Per NFS, l'indirizzo di destinazione è descritto come un nome DNS e, per iSCSI, è l'indirizzo IP dell'individuazione del portale di destinazione.

In alternativa, puoi utilizzare il seguente comando nella SLCLI.
```
# slcli block access-list --help
Usage: slcli block access-list [OPTIONS] VOLUME_ID

Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: id, name, type,
                  private_ip_address, source_subnet, host_iqn, username,
                  password, allowed_host_id
  -h, --help      Show this message and exit.
```

## Visualizzazione del {{site.data.keyword.blockstorageshort}} a cui è autorizzato un host

Puoi visualizzare i LUN a cui un host ha accesso, comprese le informazioni necessarie per stabilire una connessione, ossia nome LUN, tipo di archiviazione, indirizzo di destinazione, capacità e ubicazione:

1. Fai clic su **Devices** -> **Device List** nella console [{{site.data.keyword.cloud}}](https://{DomainName}/classic){: external} e fai clic sul dispositivo appropriato.
2. Seleziona la scheda **Storage**.

Ti viene presentato un elenco di LUN di archiviazione a cui questo specifico host ha accesso. L'elenco è raggruppato in base al tipo di archiviazione (blocco, file, altro). Puoi autorizzare ulteriore archiviazione oppure rimuovere l'accesso facendo clic su **Actions**.

Un host non può essere autorizzato ad accedere a LUN di diversi tipi di SO contemporaneamente. Un host può soltanto essere autorizzato ad accedere a LUN di un solo tipo di SO. Se tenti di autorizzare l'accesso a più LUN con diversi tipi di SO, l'operazione genera un errore.
{:note}

## Montaggio e smontaggio di {{site.data.keyword.blockstorageshort}}

In base al sistema operativo del tuo host, segui le istruzioni appropriate.

- [Connessione ai LUN su Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connessione ai LUN su CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connessione ai LUN su Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Configurazione di Block Storage per il backup con cPanell](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configurazione di Block Storage per il backup con Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)


## Revoca dell'accesso di un host a {{site.data.keyword.blockstorageshort}}

Se vuoi arrestare l'accesso da un host a uno specifico LUN di archiviazione, puoi revocare l'accesso. Una volta revocato l'accesso, la connessione host viene eliminata dal LUN. Il sistema operativo e le applicazioni su tale host non possono comunicare con il LUN.

Per evitare problemi sul lato host, smonta il LUN di archiviazione dal tuo sistema operativo prima di revocare l'accesso per evitare che manchino delle unità o danneggiamenti dei dati.
{:important}

Puoi revocare l'accesso dall'elenco dispositivi (**Device List**) o dalla vista Archiviazione (**Storage**).

### Revoca dell'accesso dall'elenco dei dispositivi

1. Nella [console {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external}, fai clic sull'icona Classic Infrastructure. Quindi, fai clic su **Devices**, **Device List** e doppio clic sul dispositivo appropriato.
2. Seleziona la scheda **Storage**.
3. Ti viene presentato un elenco di LUN di archiviazione a cui questo specifico host ha accesso. L'elenco è raggruppato in base al tipo di archiviazione (blocco, file, altro). Accanto al nome del volume, fai clic su **Actions** e su **Revoke Access**.
4. Conferma che vuoi revocare l'accesso da un LUN perché l'azione non può essere annullata. Fai clic su **Yes** per revocare l'accesso a LUN oppure su **No** per annullare l'azione.

Se vuoi disconnettere più LUN da uno specifico host, devi ripetere l'azione Revoke Access per ciascun LUN.
{:tip}


### Revoca dell'accesso dalla vista Storage

1. Fai clic su **Storage**, **{{site.data.keyword.blockstorageshort}}** e seleziona il LUI da cui vuoi revocare l'accesso.
2. Scorri a **Authorized Hosts**.
3. Fai clic su **Actions** accanto all'host di cui deve essere revocato l'accesso e seleziona **Revoke Access**.
4. Conferma che vuoi revocare l'accesso da un LUN perché l'azione non può essere annullata. Fai clic su **Yes** per revocare l'accesso a LUN oppure su **No** per annullare l'azione.

Se vuoi disconnettere più host da uno specifico LUN, devi ripetere l'azione Revoke Access per ciascun host.
{:tip}

### Revoca dell'accesso tramite la SLCLI.

In alternativa, puoi utilizzare il seguente comando nella SLCLI.
```
# slcli block access-revoke --help
Usage: slcli block access-revoke [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of a hardware server to revoke authorization.
  -v, --virtual-id TEXT     The ID of a virtual server to revoke authorization.
  -i, --ip-address-id TEXT  The ID of an IP address to revoke authorization.
  -p, --ip-address TEXT     An IP address to revoke authorization.
  --help                    Show this message and exit.
```

## Annullamento di un LUN di archiviazione

Se non hai più bisogno di uno specifico LUN, puoi annullarlo in qualsiasi momento.

Per annullare un LUN di archiviazione, devi prima revocare l'accesso da eventuali host.
{:important}

1. Fai clic su **Storage**, **{{site.data.keyword.blockstorageshort}}**.
2. Seleziona il volume che deve essere annullato, fai clic su **Actions** e seleziona **Cancel {{site.data.keyword.blockstorageshort}}**.
3. Conferma che vuoi annullare il LUN immediatamente oppure alla data di anniversario di quando è stato eseguito il provisioning del LUN.

   Se selezioni l'opzione per annullare il LUN alla sua data di anniversario, puoi invalidare la richiesta di annullamento prima che tale data ricorra.
   {:tip}
4. Fai clic su **Continue** o su **Close**.
5. Fai clic sulla casella di spunta **Acknowledgment** e fai clic su **Confirm**.

In alternativa, puoi utilizzare il seguente comando nella SLCLI.
```
# slcli block volume-cancel --help
Usage: slcli block volume-cancel [OPTIONS] VOLUME_ID

Options:
  --reason TEXT  An optional reason for cancellation
  --immediate    Cancels the block storage volume immediately instead of on
                 the billing anniversary
  -h, --help     Show this message and exit.
```

Puoi aspettarti che il LUN rimanga visibile nel tuo elenco di archiviazione per almeno 24 ore (annullamento immediato) oppure fino alla data di anniversario. Alcune funzioni non saranno più disponibili, ma il volume rimane visibile fino a quando non viene recuperato. Tuttavia, la fatturazione viene arrestata immediatamente dopo aver fatto clic su Elimina/Annulla.

Le repliche attive possono bloccare il recupero del volume di archiviazione. Assicurati che il volume non sia più montato, che le autorizzazioni host siano state revocate e che la replica sia stata annullata prima di tentare di annullare il volume originale.
