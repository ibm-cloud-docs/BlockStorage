---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, limit increase, global quota, quota increase

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Gestion des limites de stockage
{: #managingstoragelimits}

Par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}} globalement.

Si vous ne savez pas combien de volumes vous possédez, vous pouvez afficher une liste de vos volumes pour chaque centre de données à l'aide de la commande `slcli` suivante.
```
# slcli block volume-count --help
Usage: slcli block volume-count [OPTIONS]

Options:
  -d, --datacenter TEXT  Datacenter shortname
  --sortby TEXT          Column to sort by
  -h, --help             Show this message and exit.
```

Vous pouvez demander une augmentation de cette limite en soumettant un ticket dans le portail [{{site.data.keyword.slportal}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/){:new_window}. Lorsque la demande est approuvée, une limite de volume est définie pour un centre de données spécifique.  

Pour demander une augmentation de la limite, ouvrez un ticket et envoyez-le à votre ingénieur commercial.

Dans le ticket, indiquez les informations suivantes :

- **Sujet du ticket** : demande d'augmentation de la limite de stockage en nombre de volumes pour un centre de données

- **Quel cas d'utilisation la demande de volumes supplémentaires concerne-t-elle ?** <br />
*Exemple de réponse : un nouveau magasin de données VMware, un nouvel environnement de développement/test, une base de données SQL ou la consignation.*

- **Combien de volumes de bloc supplémentaires sont-ils nécessaires par type, taille, IOPS et emplacement ? ** <br />
*Exemple de réponse : "25x Endurance 2 To @ 4 IOPS dans DAL09" ou "25x Performance 4 To @ 2 IOPS dans WDC04".*

- **Combien de volumes de fichier supplémentaires sont-ils nécessaires par type, taille, IOPS et emplacement ? ** <br />
*Exemple de réponse : "25x Performance 20 Go @ 10 IOPS dans DAL09" ou "50x Endurance 2 To @ 0,25 IOPS dans SJC03".*

- **Indiquez une estimation du délai au terme duquel vous escomptez ou planifiez que la totalité de l'augmentation de volume demandée soit mise à disposition.** <br />
 *Exemple de réponse : "90 jours".*

- **Indiquez une prévision à 90 jours de l'utilisation moyenne de la capacité attendue de ces volumes.** <br />
*Exemple de réponse : "prévision de 25 % d'utilisation dans 30 jours, de 50 % d'utilisation dans 60 jours et de 75 % d'utilisation dans 90 jours".*

Répondez à toutes les questions et instructions dans votre demande. Elles sont nécessaires pour traiter et approuver la demande.
{:important}

Vous serez notifié de la mise à jour de vos limites pendant le processus de traitement du ticket.
