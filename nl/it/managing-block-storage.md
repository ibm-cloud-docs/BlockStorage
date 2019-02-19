---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Gestione di {{site.data.keyword.blockstorageshort}}
{: #managingstorage}

Puoi gestire i tuoi volumi {{site.data.keyword.blockstoragefull}} tramite il [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window}.

## Visualizzazione dei dettagli di un LUN {{site.data.keyword.blockstorageshort}}

Puoi visualizzare un riepilogo delle informazioni chiave per il LUN di archiviazione selezionato, comprese le capacità di istantanea e replica supplementari che sono state aggiunte all'archiviazione.

1. Fai clic su **Storage**, **{{site.data.keyword.blockstorageshort}}**.
2. Fai clic sul nome LUN appropriato all'elenco.

In alternativa, puoi utilizzare il seguente comando nella CLI SL.
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

1. Fai clic su **Storage** -> **{{site.data.keyword.blockstorageshort}}** e fai clic sul tuo nome LUN.
2. Scorri alla sezione **Authorized Hosts** della pagina
3. Sulla destra, fai clic su **Authorize Host**. Seleziona gli host che possono accedere allo specifico LUN.

In alternativa, puoi utilizzare il seguente comando nella CLI SL.
```
# slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  --help                    Show this message and exit.
```

## Visualizzazione dell'elenco di host che sono autorizzati ad accedere a un LUN {{site.data.keyword.blockstorageshort}}

1. Fai clic su **Storage** -> **{{site.data.keyword.blockstorageshort}}** e fai clic sul tuo nome LUN.
2. Scorri verso il basso alla sezione **Authorized Hosts**.

Qui puoi vedere un elenco di host che sono attualmente autorizzati ad accedere al LUN. Puoi inoltre vedere le informazioni di autenticazione necessarie per stabilire una connessione – nome utente, password e IQN host. L'indirizzo di destinazione è elencato nella pagina **Storage Detail**. Per NFS, l'indirizzo di destinazione è descritto come un nome DNS e, per iSCSI, è l'indirizzo IP dell'individuazione del portale di destinazione.

In alternativa, puoi utilizzare il seguente comando nella CLI SL.
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

1. Fai clic su **Devices** -> **Device List** nel [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://control.softlayer.com/){:new_window} e fai clic sul dispositivo appropriato.
2. Seleziona la scheda **Storage**.

Ti viene presentato un elenco di LUN di archiviazione a cui questo specifico host ha accesso. L'elenco è raggruppato in base al tipo di archiviazione (blocco, file, altro). Puoi autorizzare ulteriore archiviazione oppure rimuovere l'accesso facendo clic su **Actions**.

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

1. Fai clic su **Devices**, **Device List** dal [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window} e fai doppio clic sul dispositivo appropriato.
2. Seleziona la scheda **Storage**.
3. Ti viene presentato un elenco di LUN di archiviazione a cui questo specifico host ha accesso. L'elenco è raggruppato in base al tipo di archiviazione (blocco, file, altro). Accanto al nome LUN, seleziona **Action"" e fai clic su **Revoke Access**.
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

### Revoca dell'accesso tramite la CLI SL.

In alternativa, puoi utilizzare il seguente comando nella CLI SL.
```
# slcli block access-revoke --help
Usage: slcli block access-revoke [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to revoke
                            authorization
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to revoke
                            authorization
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to revoke authorization
  --ip-address TEXT         An IP address to revoke authorization
  --help                    Show this message and exit.
```

## Annullamento di un LUN di archiviazione

Se non hai più bisogno di uno specifico LUN, puoi annullarlo in qualsiasi momento.

Per annullare un LUN di archiviazione, devi prima revocare l'accesso da eventuali host.
{:important}

1. Fai clic su **Storage**, **{{site.data.keyword.blockstorageshort}}**.
2. Fai clic su **Actions** per il LUN da annullare e seleziona **Cancel {{site.data.keyword.blockstorageshort}}**.
3. Conferma che vuoi annullare il LUN immediatamente oppure alla data di anniversario di quando è stato eseguito il provisioning del LUN.

   Se selezioni l'opzione per annullare il LUN alla sua data di anniversario, puoi invalidare la richiesta di annullamento prima che tale data ricorra.
   {:tip}
4. Fai clic su **Continue** o su **Close**.
5. Fai clic sulla casella di spunta **Acknowledgment** e fai clic su **Confirm**.

In alternativa, puoi utilizzare il seguente comando nella CLI SL.
```
# slcli block volume-cancel --help
Usage: slcli block volume-cancel [OPTIONS] VOLUME_ID

Options:
  --reason TEXT  An optional reason for cancellation
  --immediate    Cancels the block storage volume immediately instead of on
                 the billing anniversary
  -h, --help     Show this message and exit.
```
