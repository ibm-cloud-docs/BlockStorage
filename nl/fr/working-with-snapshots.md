---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Gestion des instantanés

## Création d'un planning d'instantané

Les plannings d'instantané vous permettent de choisir la fréquence et le moment de création d'une référence ponctuelle de votre volume de stockage. Vous disposez d'un maximum de 50 instantanés par volume de stockage. Les plannings sont gérés via l'onglet **Stockage** > **{{site.data.keyword.blockstorageshort}}** du portail [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.

Avant de pouvoir configurer votre planning initial, vous devez d'abord acheter de l'espace d'image instantanée si vous ne l'avez pas fait lors de la mise à disposition initiale du volume de stockage.
{:important}

### Ajout d'un planning d'instantané

Vous pouvez configurer les plannings d'instantané à une fréquence horaire, quotidienne ou hebdomadaire, avec un cycle de conservation distinct. Vous disposez d'un maximum de 50 instantanés planifiés, avec différents plannings horaires, quotidiens et hebdomadaires, et instantanés manuels par volume de stockage.

1. Cliquez sur votre volume de stockage, cliquez sur **Actions**, puis sur **Planifier un échantillonnage**.
2. Dans la fenêtre Nouveau planning d'instantané, trois fréquences d'instantané vous sont proposées. Vous pouvez utiliser n'importe quelle combinaison de ces trois fréquences pour créer un planning d'instantané complet.
   - Horaire
      - Indiquez la minute de chaque heure à laquelle un instantané doit être pris. La minute en cours est indiquée par défaut.
      - Indiquez le nombre d'instantanés horaires à conserver avant la suppression du plus ancien d'entre eux.
   - Quotidienne
      - Indiquez l'heure et la minute auxquelles un instantané doit être pris. L'heure et la minute en cours sont indiquées par défaut.
      - Sélectionnez le nombre d'instantanés horaires à conserver avant la suppression du plus ancien d'entre eux.
   - Hebdomadaire
      - Indiquez le jour de la semaine, l'heure et la minute de la prise d'un instantané. Le jour, l'heure et la minute en cours sont indiqués par défaut.
      - Sélectionnez le nombre d'instantanés hebdomadaires à conserver avant la suppression du plus ancien d'entre eux.
3. Cliquez sur **Enregistrer** et créez un autre planning avec une fréquence différente. Si le nombre total d'instantanés planifiés est supérieur à 50, vous recevez un message d'avertissement et la sauvegarde est impossible.

La liste des instantanés s'affiche lors de leur prise dans la section **Instantanés** de la page **Détails**.

## Prise d'un instantané manuel

Il est possible de prendre des instantanés manuels à différents points d'une opération de mise à niveau ou de maintenance d'une application. Vous pouvez également prendre des instantanés sur plusieurs serveurs temporairement désactivés au niveau de l'application.

Vous disposez d'un maximum de 50 instantanés manuels par volume de stockage.

1. Cliquez sur votre volume de stockage.
2. Cliquez sur **Actions**.
3. Sélectionnez **Prendre un instantané manuel**.
L'instantané est pris et affiché dans la section **Instantanés** de la page **Détails**. Son planning apparaît comme Manuel.

## Affichage de la liste de tous les instantanés avec les informations relatives à l'espace utilisé et les fonctions de gestion

Vous pouvez afficher la liste des instantanés conservés ainsi que l'espace utilisé sur la page **Détails**. Les fonctions de gestion (édition des plannings et ajout d'espace supplémentaire) sont réalisées sur la page Détail à l'aide du menu **Actions** ou des liens qui figurent dans les différentes sections de la page.

## Affichage de la liste des instantanés conservés

Les instantanés conservés dépendent du nombre que vous avez saisi dans la zone **Conserve les n derniers** lors de la configuration de vos plannings. Vous pouvez afficher les instantanés pris dans la section **Instantané**. Les instantanés sont indiqués par planning.

## Affichage de la quantité d'espace d'image instantanée utilisé

Le graphique circulaire en haut de la page **Détails** indique la quantité d'espace utilisé et la quantité d'espace restant. Vous recevez des notifications lorsque vous atteignez les seuils d'espace suivants : 75 %, 90 % et 95 %.

## Modification de la quantité d'espace d'image instantanée pour un volume

Il se peut que vous deviez ajouter de l'espace d'image instantanée à un volume qui n'en disposait pas auparavant ou qui en requiert davantage. Vous pouvez ajouter entre 5 et 4 000 Go, selon vos besoins.

L'espace d'image instantanée peut uniquement être augmenté. Il est impossible de le réduire. Vous pouvez sélectionner une quantité moins élevée d'espace jusqu'à ce que vous déterminiez vos besoins réels. Gardez à l'esprit que les instantanés automatisés et manuels partagent le même espace.
{:note}

L'espace d'instantané est modifié via **Stockage** > **{{site.data.keyword.blockstorageshort}}**.

1. Cliquez sur vos volumes de stockage, cliquez sur **Actions**, puis sur **Ajouter de l'espace d'instantané supplémentaire**.
2. Effectuez votre choix dans la plage de tailles présentée par l'invite. Les tailles vont généralement de 0 à la taille de votre volume.
3. Cliquez sur **Continuer**.
4. Entrez un code promo le cas échéant et cliquez sur **Recalculer**. Les zones Prix pour cette commande et Vérification de la commande sont renseignées par défaut.
5. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service...** et cliquez sur **Valider la commande**. Votre espace d'image instantanée supplémentaire est mis à disposition en quelques minutes.

## Réception de notifications lorsque la limite d'espace d'image instantanée est atteinte et que des instantanés sont supprimés

Des notifications sont envoyées via les tickets de demande de service à l'utilisateur maître sur votre compte lorsque vous atteignez trois seuils d'espace différents : 75 %, 90 % et 95 %.

- **75 % de la capacité ** : un avertissement indiquant que l'utilisation de l'espace d'image instantanée a dépassé 75 % de la capacité est envoyé. Si vous tenez compte de l'avertissement et que vous procédez manuellement à l'ajout d'espace ou à la suppression d'images instantanés conservées et inutiles, l'action est notée et le ticket est fermé. Si vous ne faites rien, vous devez manuellement accuser réception du ticket, qui est ensuite fermé.
- **90 % de la capacité** : un second avertissement indiquant que l'utilisation de l'espace d'image instantanée a dépassé 90 % de la capacité est envoyé. Comme lors du dépassement de 75 % de la capacité, si vous effectuez les actions nécessaires pour réduire l'espace qui est utilisé, l'action est notée et le ticket est fermé. Si vous ne faites rien, vous devez manuellement accuser réception du ticket, qui est ensuite fermé.
- **Capacité de 95 % **: Un dernier avertissement est envoyé. Si vous ne faites rien pour ramener votre utilisation d'espace sous le seuil, une notification est générée et une suppression automatique est instaurée empêchant la création de futurs instantanés. Les instantanés planifiés sont supprimés, en commençant par le plus ancien, jusqu'à ce que l'utilisation se situe en dessous de 95 %. Les instantanés continuent d'être supprimés à chaque fois que l'utilisation dépasse 95 % de la capacité jusqu'à ce qu'elle retombe sous le seuil. Si l'espace est augmenté manuellement ou que des instantanés sont supprimés, l'avertissement est réinitialisé et émis à nouveau en cas de nouveau dépassement du seuil. Si aucune action n'est effectuée, il s'agit du seul avertissement que vous recevez.

## Suppression d'un planning d'instantané

Les plannings d'instantané peuvent être annulés via **Stockage** > **{{site.data.keyword.blockstorageshort}}**.

1. Cliquez sur le planning à supprimer dans le cadre **Plannings d'échantillonnage** sur la page **Détails**.
2. Cochez la case en regard du planning à supprimer et cliquez sur **Enregistrer**.<br />

Si vous utilisez la fonctionnalité de réplication, vérifiez que le planning que vous supprimez n'est pas celui qui est employé par la réplication. Pour plus d'informations sur la suppression d'un planning de réplication, voir [Réplication de données](replication.html).
{:important}

## Suppression d'un instantané

Il est possible de supprimer manuellement des instantanés inutiles afin de libérer de l'espace pour de futurs instantanés. La suppression s'effectue via **Stockage** > **{{site.data.keyword.blockstorageshort}}**.

1. Cliquez sur votre volume de stockage et faites défiler l'écran jusqu'à la section **Instantané** pour afficher la liste des instantanés existants.
2. Cliquez sur **Actions** en regard d'un instantané spécifique, puis cliquez sur **Supprimer** pour supprimer l'instantané. Cela n'affecte pas les instantanés futurs ou passés du même planning puisqu'il n'existe pas de dépendance entre les instantanés.

Les instantanés manuels qui ne sont pas supprimés comme indiqué précédemment sont automatiquement supprimés lorsque vous atteignez les limites en termes d'espace (le plus ancien d'abord).

## Restauration de volume de stockage à un point de cohérence spécifique à l'aide d'un instantané

Il se peut que vous deviez ramener votre volume de stockage à un point de cohérence spécifique en raison d'une erreur d'utilisateur ou d'une altération des données.

1. Démontez et déconnectez le volume de stockage de l'hôte.
   - [Connexion à des numéros d'unité logique (LUN) MPIO iSCSI sous Linux](accessing_block_storage_linux.html#un-mounting-block-storage-volumes)
   - [Connexion à des numéros d'unité logique (LUN) MPIO iSCSI sous Microsoft Windows](accessing-block-storage-windows.html#unmounting-block-storage-volumes)
2. Cliquez sur **Stockage**, **{{site.data.keyword.blockstorageshort}}** dans le portail [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.
3. Faites défiler l'écran et cliquez sur le volume à restaurer. La section **Instantanés** de la page **Détails** affiche la liste de tous les instantanés sauvegardés, ainsi que leur taille et leur date de création.
4. Cliquez sur **Actions** en regard de l'instantané à utiliser, puis cliquez sur **Restaurer**. <br/>

   L'opération de restauration entraîne la perte des données qui ont été créées ou modifiées après la prise de l'instantané. Cette perte de données se produit car votre volume de stockage reprend le même état que celui qui était le sien au moment de la prise de l'instantané.
   {:note}
5. Cliquez sur **Oui** pour lancer la restauration. Un message doit s'afficher en haut de la page pour vous indiquer que le volume est restauré à l'aide de l'instantané sélectionné. En outre, une icône apparaît en regard de votre volume sur {{site.data.keyword.blockstorageshort}} pour indiquer qu'une transaction active est en cours. Survolez cette icône pour ouvrir une boîte de dialogue affichant la transaction. L'icône disparaît une fois la transaction terminée.
6. Montez et reconnectez le volume de stockage à l'hôte.
   - [Connexion à des numéros d'unité logique (LUN) MPIO iSCSI sous Linux](accessing_block_storage_linux.html)
   - [Connexion à des numéros d'unité logique MPIO iSCSI sous CloudLinux](configure-iscsi-cloudlinux.html)
   - [Connexion à des numéros d'unité logique (LUN) MPIO iSCSI sous Microsoft Windows](accessing-block-storage-windows.html)

La restauration d'un volume entraîne la suppression de tous les instantanés qui ont été pris après celui utilisé pour la restauration.{:important}
