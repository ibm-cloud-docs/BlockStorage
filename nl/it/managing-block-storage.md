---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gestione di {{site.data.keyword.blockstorageshort}}

Puoi gestire i tuoi volumi {{site.data.keyword.blockstoragefull}} tramite[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}. Questo articolo fornisce le istruzioni per le attività più comuni.

## Vedi i dettagli di un LUN {{site.data.keyword.blockstorageshort}} di cui è stato eseguito il provisioning

Puoi visualizzare un riepilogo delle informazioni chiave per il LUN di archiviazione selezionato, comprese le capacità di istantanea e replica aggiuntive che sono state aggiunte all'archiviazione.

1. Fai clic su **Storage**, **{{site.data.keyword.blockstorageshort}}**.
2. Fai clic sul nome LUN appropriato all'elenco.

## Autorizza gli host ad accedere a {{site.data.keyword.blockstorageshort}}

Gli host "autorizzati" sono host a cui sono stati concessi i diritti di accesso a uno specifico LUN. Senza l'autorizzazione host, non potrai accedere all'archiviazione dal tuo sistema né utilizzarla. Autorizzare un host ad accedere al tuo LUN genera il nome utente, la password e l'IQN (iSCSI qualified name) necessari per montare la connessione iSCSI MPIO (multipath I/O).

**Nota**: puoi autorizzare e connettere solo gli host che si trovano nello stesso data center della tua archiviazione. Se hai più account, non puoi autorizzare un host da un account ad accedere alla tua archiviazione su un altro.

1. Fai clic su **Storage** -> **{{site.data.keyword.blockstorageshort}}** e fai clic sul tuo nome LUN.
2. Scorri alla sezione Authorized Hosts della pagina.
3. Fai clic sul link **Authorize Host** sul lato destro della pagina. Seleziona gli host che possono accedere allo specifico LUN.

 

## Visualizza gli elenchi di host autorizzati a un LUN {{site.data.keyword.blockstorageshort}}

Per visualizzare l'elenco di host autorizzati a un LUN, attieniti alla seguente procedura.

1. Fai clic su **Storage** -> **{{site.data.keyword.blockstorageshort}}** e fai clic sul tuo nome LUN.
2. Scorri verso il basso al fondo alla pagina alla sezione Authorized Hosts.

Qui vedrai un elenco di host che sono attualmente autorizzati ad accedere al LUN e, specificamente per iSCSI, le informazioni di autenticazione necessarie per stabilire una connessione, ossia nome utente, password e IQN host. L'indirizzo di destinazione si trova nella parte superiore della pagina Storage Detail. Per NFS è descritto come un nome DNS e per iSCSI l'indirizzo IP dell'individuazione del portale di destinazione.

 

## Visualizza il {{site.data.keyword.blockstorageshort}} a cui è autorizzato un host

Puoi visualizzare i LUN a cui un host ha accesso, comprese le informazioni necessarie per stabilire una connessione, ossia nome LUN, tipo di archiviazione, indirizzo di destinazione, capacità e ubicazione:

1. Fai clic su **Devices** -> **Device List** dal[{{site.data.keyword.slportal}}](http://control.softlayer.com/){:new_window} e fai clic sul dispositivo appropriato.
2. Seleziona la scheda **Storage**.

Ti verrà quindi presentato un elenco di LUN di archiviazione a cui questo specifico host ha accesso, tutti raggruppati in base al tipo di archiviazione (blocco, file, altro). Di rispettivi menu Action, puoi autorizzare dell'ulteriore archiviazione oppure rimuovere l'accesso.

 

## Monta e smonta {{site.data.keyword.blockstorageshort}}

Fai riferimento ai seguenti articoli per i dettagli per montare e smontare {{site.data.keyword.blockstorageshort}} da un host.

- [{{site.data.keyword.blockstorageshort}} su Linux](accessing_block_storage_linux.html)

- [{{site.data.keyword.blockstorageshort}} su Microsoft Windows](accessing-block-storage-windows.html)

 

## Revoca l'accesso di un host a {{site.data.keyword.blockstorageshort}}

Se vuoi arrestare l'accesso da un host a uno specifico LUN di archiviazione, puoi revocare l'accesso. Una volta revocato l'accesso, la connessione host verrà eliminata dal LUN e né il sistema operativo né le applicazioni saranno in grado di comunicare con il LUN.

**Nota**: per evitare problemi sul lato host, smonta il LUN di archiviazione dal tuo sistema operativo prima di revocare l'accesso per evitare che manchino delle unità o dei danneggiamenti dei dati.

Puoi revocare l'accesso dall'uno o dall'altro Storage dalle viste Device List o Storage.

### Revoca l'accesso da Device List:

1. Fai clic su **Devices**, **Device List** dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} e fai doppio clic sul dispositivo appropriato.
2. Seleziona la scheda **Storage**.
3. Ti verrà quindi presentato un elenco di LUN di archiviazione a cui questo specifico host ha accesso, tutti raggruppati in base al tipo di archiviazione (blocco, file, altro). Seleziona il rispettivo menu Action accanto al LUN da cui vuoi revocare l'accesso e fai clic su **Revoke Access**.
4. Ti verrà chiesto se vuoi revocare l'accesso da un LUN perché l'azione non può essere annullata. Fai clic su **Yes** per revocare l'accesso a LUN oppure su **No** per annullare l'azione.

**Nota**: se vuoi disconnettere più LUN da uno specifico host, dovrai ripetere l'azione Revoke Access per ciascun LUN.


### Revoca l'accesso dalla vista Storage:

1. Fai clic su **Storage**, **{{site.data.keyword.blockstorageshort}}** e seleziona il LUN da cui vuoi revocare l'accesso.
2. Scorri verso il basso alla sezione ** Authorized Hosts** della pagina.
3. Fai clic sulla freccia a discesa **Actions** accanto all'host di cui deve essere revocato l'accesso e seleziona **Revoke Access**.
4. Ti verrà chiesto se vuoi revocare l'accesso da un LUN perché l'azione non può essere annullata. Fai clic su **Yes** per revocare l'accesso a LUN oppure su **No** per annullare l'azione.

**Nota**: se vuoi disconnettere più host da uno specifico LUN, dovrai ripetere l'azione Revoke Access per ciascun host.

 

## Annulla un LUN di archiviazione

Se non hai più bisogno di uno specifico LUN, puoi annullarlo. Per annullare un LUN di archiviazione, devi prima revocare l'accesso da eventuali host.

1. Fai clic su **Storage**, **{{site.data.keyword.blockstorageshort}}**.
2. Fai clic sulla freccia a discesa **Actions** per il LUN da annullare e seleziona **Cancel {{site.data.keyword.blockstorageshort}}**.
3. Ti verrà chiesto di confermare che vuoi annullare il LUN immediatamente oppure alla data di anniversario di quando è stato eseguito il provisioning del LUN. Fai clic su **Continue** o su **Close**.
**Nota**: se selezioni l'opzione per annullare il LUN alla sua data di anniversario, puoi invalidare la richiesta di annullamento prima che tale data ricorra.
4. Fai clic sulla casella di spunta **Acknowledgment** e fai clic su **Confirm**.

 

