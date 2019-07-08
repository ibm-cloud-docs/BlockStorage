---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords:  Block Storage, block storage, snapshot, snapshot space, snapshot schedule, create snapshot schedule, manual snapshot, view snapshot space, modify snapshot space, SLCLI, API, restore from snapshot

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}

# Gestion des instantanés
{: #managingSnapshots}

## Création d'un planning d'instantané

Les plannings d'instantané vous permettent de choisir la fréquence et le moment de création d'une référence ponctuelle de votre volume de stockage. Vous disposez d'un maximum de 50 instantanés par volume de stockage. Les plannings sont gérés via l'onglet **Stockage** > **{{site.data.keyword.blockstorageshort}}** de la [console {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external}.

Avant de pouvoir configurer votre planning initial, vous devez d'abord acheter de l'espace d'image instantanée si vous ne l'avez pas fait lors de la mise à disposition initiale du volume de stockage. Pour plus d'informations, voir [Commande d'instantanés](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
{:important}

### Ajout d'un planning d'instantané
{: #addingschedule}

Vous pouvez configurer les plannings d'instantané à une fréquence horaire, quotidienne ou hebdomadaire, avec un cycle de conservation distinct. La limite maximale d'instantanés est de 50 par volume de stockage, avec différents plannings horaires, quotidiens et hebdomadaires, et instantanés manuels.

1. Cliquez sur votre volume de stockage, cliquez sur **Actions**, puis sur **Planifier un échantillonnage**.
2. Dans la fenêtre Nouveau planning d'instantané, vous avez le choix entre trois fréquences d'instantané. Vous pouvez utiliser n'importe quelle combinaison de ces trois fréquences pour créer un planning d'instantané complet.
   - Horaire
      - Indiquez la minute de chaque heure à laquelle un instantané doit être pris. La minute en cours est indiquée par défaut.
      - Indiquez le nombre d'instantanés horaires à conserver avant la suppression du plus ancien d'entre eux.
   - Quotidienne
      - Indiquez l'heure et la minute auxquelles un instantané doit être pris. L'heure et la minute en cours sont indiquées par défaut.
      - Sélectionnez le nombre d'instantanés horaires à conserver avant la suppression du plus ancien d'entre eux.
   - Hebdomadaire
      - Indiquez le jour de la semaine, l'heure et la minute de la prise d'un instantané. Le jour, l'heure et la minute en cours sont indiqués par défaut.
      - Sélectionnez le nombre d'instantanés hebdomadaires à conserver avant la suppression du plus ancien d'entre eux.
3. Cliquez sur **Enregistrer**. Vous pouvez ensuite créer un autre planning avec une fréquence différente. Si le nombre total d'instantanés planifiés est supérieur à 50, vous recevez un message d'avertissement et la sauvegarde est impossible.

La liste des instantanés s'affiche lors de leur prise dans la section **Instantanés** de la page **Détails**.

Pour obtenir la liste de vos plannings d'instantanés via l'interface SLCLI, exécutez la commande suivante.
```
# slcli block snapshot-schedule-list --help
Usage: slcli block snapshot-schedule-list [OPTIONS] VOLUME_ID

Options:
  -h, --help  Show this message and exit.
```
{:codeblock}

## Prise d'un instantané manuel

Il est possible de prendre des instantanés manuels à différents points d'une opération de mise à niveau ou de maintenance d'une application. Vous pouvez également prendre des instantanés sur plusieurs serveurs temporairement désactivés au niveau de l'application.

Vous disposez d'un maximum de 50 instantanés par volume de stockage.

1. Cliquez sur votre volume de stockage.
2. Cliquez sur **Actions**.
3. Sélectionnez **Prendre un instantané manuel**.
L'instantané est pris et affiché dans la section **Instantanés** de la page **Détails**. Son planning apparaît comme Manuel.

Vous pouvez également utiliser la commande suivante pour créer un instantané via l'interface SLCLI.
```
# slcli block snapshot-create --help
Usage: slcli block snapshot-create [OPTIONS] VOLUME_ID

Options:
  -n, --notes TEXT  Notes to set on the new snapshot
  -h, --help        Show this message and exit.
```
{:codeblock}

## Affichage de la liste de tous les instantanés avec les informations relatives à l'espace utilisé et les fonctions de gestion

Vous pouvez afficher la liste des instantanés conservés ainsi que l'espace utilisé sur la page **Détails**.  Les fonctions de gestion (édition des plannings et ajout d'espace supplémentaire) sont réalisées sur la page Détail à l'aide du menu **Actions** ou des liens qui figurent dans les différentes sections de la page.

## Affichage de la liste des instantanés conservés

Les instantanés conservés dépendent du nombre que vous avez saisi dans la zone **Conserve les n derniers** lors de la configuration de vos plannings. Vous pouvez afficher les instantanés pris dans la section **Instantané**. Les instantanés sont indiqués par planning.

Vous pouvez également utiliser la commande suivante dans l'interface SLCLI pour afficher les instantanés disponibles.
```
# slcli block snapshot-list --help
Usage: slcli block snapshot-list [OPTIONS] VOLUME_ID

Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: id, name, created, size_bytes
  -h, --help      Show this message and exit.
```
{:codeblock}

## Affichage de la quantité d'espace d'image instantanée utilisé

Le graphique circulaire sur la page **Détails** indique la quantité d'espace utilisé et la quantité d'espace restant. Vous recevez des notifications lorsque vous atteignez les seuils d'espace suivants : 75 %, 90 % et 95 %.

## Modification de la quantité d'espace d'image instantanée pour un volume

Il se peut que vous deviez ajouter de l'espace d'image instantanée à un volume qui n'en disposait pas auparavant ou qui en requiert davantage. Vous pouvez ajouter entre 5 et 4 000 Go, selon vos besoins.

L'espace d'image instantanée peut uniquement être augmenté. Il est impossible de le réduire. Vous pouvez sélectionner une quantité moins élevée d'espace jusqu'à ce que vous déterminiez vos besoins. Gardez à l'esprit que les instantanés automatisés et manuels partagent le même espace.
{:note}

L'espace d'instantané est modifié via **Stockage** > **{{site.data.keyword.blockstorageshort}}**.

1. Cliquez sur vos volumes de stockage, cliquez sur **Actions**, puis sur **Ajouter de l'espace d'instantané supplémentaire**.
2. Effectuez votre choix dans la plage de tailles présentée par l'invite. Les tailles vont généralement de 0 à la taille de votre volume.
3. Cliquez sur **Continuer**.
4. Entrez un code promo le cas échéant et cliquez sur **Recalculer**. Les zones Prix pour cette commande et Vérification de la commande sont renseignées par défaut.
5. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service...** et cliquez sur **Valider la commande**. Votre espace d'image instantanée supplémentaire est mis à disposition en quelques minutes.

## Réception de notifications lorsque la limite d'espace d'image instantanée est atteinte et que des instantanés sont supprimés

Des notifications sont envoyées via les cas de support à l'Utilisateur maître de votre compte lorsque vous atteignez trois seuils d'espace différents – 75 %, 90 % et 95 %.

- A **75 % de la capacité **, un avertissement indiquant que l'utilisation de l'espace d'image instantanée a dépassé 75 % de la capacité est envoyé. Si vous tenez compte de l'avertissement et que vous procédez manuellement à l'ajout d'espace ou à la suppression d'images instantanés conservées et inutiles, l'action est notée et le ticket est fermé. Si vous ne faites rien, vous devez accuser manuellement réception du cas et il est fermé.
- A **90 % de la capacité**, un second avertissement indiquant que l'utilisation de l'espace d'image instantanée a dépassé 90 % de la capacité est envoyé. Comme lors du dépassement de 75 % de la capacité, si vous effectuez les actions nécessaires pour réduire l'espace qui est utilisé, l'action est notée et le ticket est fermé. Si vous ne faites rien, vous devez accuser manuellement réception du cas et il est fermé.
- A **95 % de la capacité**, un dernier avertissement est envoyé. Si vous n'intervenez pas pour ramener votre utilisation d'espace sous le seuil, une notification est générée et une suppression automatique est instaurée empêchant la création de futurs instantanés. Les instantanés planifiés sont supprimés, en commençant par le plus ancien, jusqu'à ce que l'utilisation passe au-dessous de 95 %. Les instantanés continuent d'être supprimés à chaque fois que l'utilisation dépasse 95 % de la capacité jusqu'à ce qu'elle repasse sous le seuil. Si l'espace est augmenté manuellement ou que des instantanés sont supprimés, l'avertissement est réinitialisé et émis à nouveau en cas de nouveau dépassement du seuil. Si aucune action n'est effectuée, il s'agit du seul avertissement que vous recevez.

## Suppression d'un planning d'instantané

Les plannings d'instantané peuvent être annulés via **Stockage** > **{{site.data.keyword.blockstorageshort}}**.

1. Cliquez sur le planning à supprimer dans le cadre **Plannings d'échantillonnage** sur la page **Détails**.
2. Cochez la case en regard du planning à supprimer et cliquez sur **Enregistrer**.<br />

Si vous utilisez la fonctionnalité de réplication, vérifiez que le planning que vous supprimez n'est pas celui qui est employé par la réplication. Pour plus d'informations sur la suppression d'un planning de réplication, voir [Réplication de données](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication).
{:important}

## Suppression d'un instantané

Il est possible de supprimer manuellement des instantanés inutiles afin de libérer de l'espace pour de futurs instantanés. La suppression s'effectue via **Stockage** > **{{site.data.keyword.blockstorageshort}}**.

1. Cliquez sur votre volume de stockage et faites défiler l'écran jusqu'à la section **Instantané** pour afficher la liste des instantanés existants.
2. Cliquez sur **Actions** en regard d'un instantané spécifique, puis cliquez sur **Supprimer** pour supprimer l'instantané. Cette suppression n'affecte pas les instantanés futurs ou passés du même planning puisqu'il n'existe pas de dépendance entre les instantanés.

Les instantanés manuels qui ne sont pas supprimés manuellement dans le portail sont automatiquement supprimés lorsque vous atteignez les limites en termes d'espace (le plus ancien d'abord).

Vous pouvez utiliser la commande suivante pour supprimer un volume via l'interface SLCLI.
```
# slcli block snapshot-delete
Usage: slcli block snapshot-delete [OPTIONS] SNAPSHOT_ID

Options:
  -h, --help  Show this message and exit.
```
{:codeblock}


## Restauration de volume de stockage à un point de cohérence spécifique à l'aide d'un instantané

Il se peut que vous deviez ramener votre volume de stockage à un point de cohérence spécifique en raison d'une erreur d'utilisateur ou d'une altération des données.

La restauration d'un volume entraîne la suppression de tous les instantanés qui ont été pris après celui utilisé pour la restauration.
{:important}

1. Démontez et déconnectez le volume de stockage de l'hôte.
   - [Connexion à des numéros d'unité logique (LUN) sous Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#unmounting)
   - [Connexion à des numéros d'unité logique (LUN) sous Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows#unmounting)
2. Cliquez sur **Stockage**, **{{site.data.keyword.blockstorageshort}}** dans la [console {{site.data.keyword.cloud_notm}}](https://{DomainName}/){: external}.
3. Faites défiler l'écran et cliquez sur le volume à restaurer. La section **Instantanés** de la page **Détails** affiche la liste de tous les instantanés sauvegardés, ainsi que leur taille et leur date de création.
4. Cliquez sur **Actions** en regard de l'instantané à utiliser, puis cliquez sur **Restaurer**. <br/>

   L'opération de restauration entraîne la perte des données qui ont été créées ou modifiées après la prise de l'instantané. Cette perte de données se produit car votre volume de stockage reprend le même état que celui qui était le sien au moment de la prise de l'instantané.
   {:note}
5. Cliquez sur **Oui** pour lancer la restauration.

   Un message doit s'afficher sur la page pour vous indiquer que le volume est restauré à l'aide de l'instantané sélectionné. En outre, une icône apparaît en regard de votre volume sur {{site.data.keyword.blockstorageshort}} pour indiquer qu'une transaction active est en cours. Survolez cette icône pour ouvrir une boîte de dialogue affichant la transaction. L'icône disparaît une fois la transaction terminée.
   {:note}
6. Montez et reconnectez le volume de stockage à l'hôte.
   - [Connexion à des numéros d'unité logique (LUN) sous Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
   - [Connexion à des numéros d'unité logique (LUN) sous CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
   - [Connexion à des numéros d'unité logique (LUN) sous Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)

Une fois le volume déconnecté de l'hôte, vous pouvez également utiliser la commande suivante dans l'interface SLCLI pour démarrer une restauration.
```
# slcli block snapshot-restore --help
Usage: slcli block snapshot-restore [OPTIONS] VOLUME_ID

Options:
  -s, --snapshot-id TEXT  The id of the snapshot which is to be used to restore
                          the block volume
  -h, --help              Show this message and exit.
```
{:codeblock}  

Une fois la restauration terminée, montez votre volume de stockage et reconnectez-le à l'hôte.
