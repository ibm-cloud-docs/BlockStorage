---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-15"

---
{:new_window: target="_blank"}

# Réplication de données

La réplication utilise l'un de vos plannings d'instantané pour copier automatiquement des instantanés sur un volume de destination dans un centre de données distant. Les copies peuvent être récupérées sur le site distant en cas de données endommagées ou de catastrophe.

Les répliques vous permettent :

- d'effectuer rapidement une reprise après un échec du site et d'autres incidents en basculant sur le volume de destination,
- d'effectuer un basculement vers un point de cohérence précis dans la copie de reprise après incident.

Avant d'effectuer une réplication, vous devez créer un planning d'instantané. Lorsque vous effectuez un basculement, vous "basculez l'interrupteur" depuis votre volume de stockage du centre de données principal vers le volume de destination du centre de données distant. Par exemple, votre centre de données principal peut se situer à Londres et votre centre de données secondaire à Amsterdam. Dans le cas d'un événement d'échec, vous basculez vers Amsterdam, en vous connectant au volume qui est désormais devenu principal à partir d'une instance de calcul à Amsterdam. Une fois votre volume de Londres réparé, un instantané du volume d'Amsterdam est pris afin de permettre le retour à Londres avec le volume de Londres à nouveau considéré comme le volume principal à partir d'une instance de traitement située à Londres.


## Comment déterminer le centre de données distant de mon volume de stockage répliqué ?

Les centres de données d'{{site.data.keyword.BluSoftlayer_full}} sont appariés en combinaisons principal-distant dans le monde entier.
Pour obtenir la liste complète de la disponibilité des centres de données et des cibles de réplication, reportez-vous au Tableau 1.

<table>
  <caption style="text-align: left;"><p>Le tableau 1 répertorie l'ensemble des centres de données avec les fonctionnalités améliorées dans chaque région. Chaque région correspond à une colonne. Certaines villes, comme Dallas, San Jose, Washington DC, Amsterdam, Francfort, Londres et Sydney disposent de plusieurs centres de données.</p>
  <p>&#42; Les centres de données de la région EU 1 ne comportent PAS de stockage amélioré. Les hôtes des centres de données comportant des fonctionnalités de stockage améliorées <strong>ne peuvent pas</strong> démarrer la réplication avec des cibles de réplique dans les centres de données de la région EU 1.</p>
  </caption>
    <thead>
    <tr>
      <th>EU 1 &#42;</th>
      <th>EUS 2</th>
      <th>Amérique latine</th>
      <th>Canada</th>
      <th>Europe</th>
      <th>Asie-Pacifique</th>
      <th>Australie</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>DAL01<br />
          DAL05<br />
	  DAL06<br />
	  HOU02<br />
	  SJC01<br />
	  WDC01<br />
	  <br /><br /><br /><br /><br /><br />
      </td>
      <td>SJC03<br />
	  SJC04<br />
	  WDC04<br />
	  WDC06<br />
	  WDC07<br />
	  DAL09<br />
	  DAL10<br />
	  DAL12<br />
	  DAL13<br />
	  <br /><br /><br />
      </td>
      <td>MEX01<br />
	  SAO01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>TOR01<br />
          MON01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>AMS01<br />
	  AMS03<br />
	  FRA02<br />
	  FRA04<br />
	  FRA05<br />
	  LON02<br />
	  LON04<br />
	  LON05<br />
	  LON06<br />
	  OSL01<br />
	  PAR01<br />
	  MIL01<br />
      </td>
      <td>HKG02<br />
          TOK02<br />
	  TOK04<br />
	  TOK05<br />
	  SNG01<br />
	  SEO01<br />
          CHE01<br />
	  <br /><br /><br /><br /><br />
      </td>
      <td>SYD01<br />
          SYD04<br />
	  MEL01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
    </tr>
  </tbody>
</table>

## Création de la réplique initiale

Les réplications fonctionnent selon un planning d'instantané. Vous devez d'abord configurer un espace d'instantané et un planning d'instantané pour le volume source avant de pouvoir répliquer. Si vous tentez de configurer la réplication alors que l'espace d'instantané ou le planning d'instantané n'existe pas, vous serez invité à acheter davantage d'espace ou à configurer un planning. Les réplications sont gérées sous **Stockage**, **{{site.data.keyword.blockstorageshort}}** dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Cliquez sur votre volume de stockage.
2. Cliquez sur **Réplique**, puis sur **Acheter une réplication**.
3. Sélectionnez le planning d'instantané existant que vous souhaitez que votre réplication suive. La liste contient tous vos plannings d'instantané actifs. <br />
   >**Remarque :** vous ne pouvez sélectionner qu'une seul planning, même si vous combinez des réplications horaires, quotidiennes et hebdomadaires. Tous les instantanés qui ont été capturés depuis le cycle de réplication précédent sont répliqués quel que soit leur planning d'origine.<br />Si vous n'avez pas configuré d'instantanés, vous êtes invité à le faire avant de pouvoir commander la réplication. Pour plus de détails, voir [Utilisation d'instantanés](snapshots.html).
3. Cliquez sur **Emplacement** et sélectionnez le centre de données qui est votre site de reprise après incident.
4. Cliquez sur **Continuer**.
5. Entrez un **Code promo** le cas échéant et cliquez sur **Recalculer**. Les autres zones de la boîte de dialogue contiennent les valeurs par défaut.
6. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**.


## Edition d'une réplication existante

Vous pouvez éditer votre planning de réplication et modifier votre espace de réplication à partir de l'onglet **Principal** ou **Réplique** sous **Stockage**, **{{site.data.keyword.blockstorageshort}}** dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.



## Edition du planning de réplication

Le planning de réplication est basé sur un planning d'instantané existant. Pour modifier le planning de réplication, par exemple d'Horaire en Hebdomadaire, vous devez annuler le planning de réplication et en configurer un nouveau.

La modification du planning peut s'effectuer sur l'onglet Principal ou Réplique.

1. Cliquez sur **Actions** sur l'onglet **Principal** ou **Réplique**.
2. Sélectionnez **Modifier le planning d'instantané**.
3. Regardez dans le cadre **Instantané** sous **Planning** pour déterminer le planning que vous utilisez pour la réplication. Modifiez le planning de votre choix. Par exemple, si votre planning de réplication est **Quotidien**, vous pouvez modifier l'heure de la journée à laquelle la réplication doit avoir lieu.
4. Cliquez sur **Enregistrer**.


## Modification de l'espace de réplication

Votre espace d'image instantanée principal et votre espace de réplique doivent être identiques. Si vous modifiez l'espace sur l'onglet **Principal** ou **Réplique**, l'espace est automatiquement ajouté à vos centres de données source et de destination. L'augmentation de l'espace d'image instantanée déclenche également une mise à jour immédiate de la réplication.

1. Cliquez sur **Actions** sur l'onglet **Principal** ou **Réplique**.
2. Sélectionnez **Ajouter de l'espace d'image instantanée supplémentaire**.
3. Sélectionnez la taille de stockage dans la liste, puis cliquez sur **Continuer**.
4. Entrez un **Code Promo** le cas échéant et cliquez sur **Recalculer**. Les autres zones de la boîte de dialogue contiennent les valeurs par défaut.
5. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**.


## Affichage des volumes de réplique dans la liste de volumes

Vous pouvez afficher vos volumes de réplication sur la page {{site.data.keyword.blockstorageshort}} sous **Stockage > {{site.data.keyword.blockstorageshort}}**. Le **Nom LUN** contient le nom du volume principal suivi de REP. Le **Type** est Endurance ou Performance – Réplique. L'**Adresse cible** est Sans objet car le volume de réplique n'est pas monté sur le centre de données de réplique, et le **Statut** indique Inactif.


## Affichage des détails d'un volume répliqué dans le centre de données de la réplique

Vous pouvez afficher les détails du volume de réplique sur l'onglet **Réplique** sous **Stockage**, **{{site.data.keyword.blockstorageshort}}**. Une autre option consiste à sélectionner le volume de réplique à partir de la page **{{site.data.keyword.blockstorageshort}}** et à cliquer sur l'onglet **Réplique**.


## Spécification des autorisations de l'hôte avant le basculement du serveur vers le centre de données secondaire

Les hôtes et les volumes autorisés doivent figurer dans le même centre de données. Vous ne pouvez pas avoir un volume de réplique à Londres et un hôte à Amsterdam. ils doivent se trouver tous les deux à Londres ou à Amsterdam.

1. Cliquez sur votre volume source ou cible à partir de la page **{{site.data.keyword.blockstorageshort}}**.
2. Cliquez sur **Réplique**.
3. Faites défiler l'écran vers le bas jusqu'au cadre **Autoriser les hôtes** et cliquez sur **Autoriser les hôtes** à droite.
4. Mettez en évidence l'hôte qui doit être autorisé pour les réplications. Pour sélectionner plusieurs hôtes, utilisez la touche ctrl et cliquez sur les hôtes concernés.
5. Cliquez sur **Soumettre**. En l'absence d'hôte, vous êtes invité à acheter des ressources de traitement dans le même centre de données.


## Augmentation de l'espace d'image instantanée dans le centre de données de réplique lorsque l'espace d'image instantanée est augmenté dans le centre de données principal.

Les tailles des volumes de stockage principal et de réplique doivent être identiques. Il n'est pas possible que l'un soit plus grand que l'autre. Lorsque vous augmentez votre espace d'image instantanée dans le volume principal, l'espace de réplique est automatiquement augmenté. L'augmentation de l'espace d'image instantanée déclenche une mise à jour immédiate de la réplication. L'augmentation des deux volumes apparaît sous forme de lignes d'article dans votre facture et est calculée au prorata si nécessaire.

Cliquez [ici](snapshots.html) pour savoir comment augmenter votre espace d'instantané.


## Démarrage d'un basculement depuis un volume vers sa réplique

Dans le cas d'un événement d'échec, vous pouvez initier un **basculement** vers votre volume de destination, ou volume cible. Le volume cible devient actif. Le dernier instantané répliqué avec succès est activé et le volume est alors disponible pour le montage. Toutes les données écrites sur le volume source depuis le cycle de réplication précédent sont perdues. Une fois le basculement démarré, la relation de réplication est inversée. Votre volume cible devient votre volume source, et le volume source précédent devient votre cible, comme indiqué par le **Nom LUN** suivi de **REP**.

Les basculements sont lancés sous **Stockage**, **{{site.data.keyword.blockstorageshort}}** dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

**Avant d'exécuter ces étapes, déconnectez le volume. Si vous omettez cette étape, des données seront endommagées et perdues.**

1. Cliquez sur votre numéro d'unité logique actif ("source").
2. Cliquez sur **Réplique**, puis sur le lien **Actions** dans l'angle supérieur droit.
3. Sélectionnez **Basculement**.
   >Vous devez voir s'afficher un message dans la partie supérieure de la page, indiquant le basculement est en cours. En outre, une icône apparaît en regard de votre volume sur **{{site.data.keyword.blockstorageshort}}** pour indiquer qu'une transaction active est en cours. Survolez cette icône pour ouvrir une boîte de dialogue affichant la transaction. L'icône disparaît une fois la transaction terminée. Durant le processus de basculement, les actions liées à la configuration sont accessibles en lecture seule. Vous ne pouvez pas éditer de planning d'instantané, ni modifier l'espace d'image instantanée. L'événement est consigné dans l'historique des réplications.<br/> Lorsque le volume cible est opérationnel, vous obtenez un autre message. Le nom LUN de votre volume source d'origine est mis à jour afin de se terminer par "REP" et il devient inactif.
4. Cliquez sur **Tout afficher ({{site.data.keyword.blockstorageshort}})**.
5. Cliquez sur votre numéro d'unité logique actif (anciennement votre volume cible).
6. Montez votre volume de stockage sur l'hôte et associez-les. Cliquez [ici](provisioning-block_storage.html) pour obtenir des instructions.


## Démarrage d'une reprise par restauration depuis un volume vers sa réplique

Une fois votre volume source d'origine réparé, vous pouvez démarrer une reprise par restauration contrôlée vers le volume source d'origine. Dans une reprise par restauration contrôlée,

- le volume source actif est mis hors ligne ;
- un instantané est pris ;
- le cycle de réplication est mené à bien ;
- l'instantané de données tout juste pris est activé ;
- et le volume source est activé pour le montage.

Une fois la reprise par restauration démarrée, la relation de réplication est inversée. Votre volume source est restauré en tant que volume source, et votre volume cible redevient le volume cible, comme indiqué par le **Nom LUN** suivi de **REP**.

Les reprises par restauration sont lancées sous **Stockage**, **{{site.data.keyword.blockstorageshort}}** dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Cliquez sur votre numéro d'unité logique actif ("cible").
2. Dans l'angle supérieur droit, cliquez sur **Réplique**, puis sur **Actions**.
3. Sélectionnez **Reprise par restauration**. Vous devez voir s'afficher un message dans la partie supérieure de la page, indiquant le basculement est en cours. En outre, une icône apparaît en regard de votre volume sur **{{site.data.keyword.blockstorageshort}}** pour indiquer qu'une transaction active est en cours. Survolez cette icône pour ouvrir une boîte de dialogue affichant la transaction. L'icône disparaît une fois la transaction terminée. Durant le processus de reprise par restauration, les actions liées à la configuration sont accessibles en lecture seule. Vous ne pouvez pas éditer de planning d'instantané, ni modifier l'espace d'image instantanée. L'événement est consigné dans l'historique des réplications.
4. Dans l'angle supérieur droit, cliquez sur le lien **Afficher tout {{site.data.keyword.blockstorageshort}}**.
5. Cliquez sur votre numéro d'unité logique actif ("source").
6. Montez votre volume de stockage sur l'hôte et associez-les. Cliquez [ici](provisioning-block_storage.html) pour obtenir des instructions.


## Affichage de l'historique des réplications

L'historique des réplications s'affiche dans le **journal d'audit** sur l'onglet **Compte** sous **Gérer**. L'historique des réplications est le même pour le volume principal et le volume de réplique. Il comprend les informations suivantes :

- Type de réplication (basculement ou reprise par restauration)
- Date/heure de début
- Instantané utilisé pour la réplication
- Taille de la réplication
- Date/heure de fin


## Création d'un doublon d'un volume de réplique

Vous pouvez créer un doublon d'un {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.blockstoragefull}} existant. Le volume en double hérite par défaut des options de capacité et de performance du numéro d'unité logique/volume d'origine et contient une copie des données jusqu'au moment de la prise d'un instantané.

Vous pouvez créer des doublons à partir de volumes principaux et de volumes de réplique. Le nouveau doublon est créé dans le même centre de données que le volume d'origine. Si vous créez un doublon à partir d'un volume de réplique, le nouveau volume est créé dans le même centre de données que le volume de réplique.

Les volumes dupliqués sont accessibles par un hôte en lecture/écriture dès la mise à disposition du stockage. Toutefois, les instantanés et la réplication ne sont pas autorisés tant que la copie des données depuis le volume d'origine vers le doublon n'est pas terminée.

Pour plus d'informations, voir [Création d'un volume de blocs en double](how-to-create-duplicate-volume.html)


## Annulation d'une réplication existante

Vous pouvez annuler la réplication immédiatement ou à la date anniversaire, ce qui met fin à la facturation. La réplication peut être annulée à partir de l'onglet **Principal** ou **Réplique**.

1. Cliquez sur le volume dans la page **{{site.data.keyword.blockstorageshort}}**.
2. Cliquez sur **Actions** sur l'onglet **Principal** ou **Réplique**.
3. Sélectionnez **Annuler la réplique**.
4. Choisissez le moment de l'annulation du volume, Choisissez **Immédiatement** ou **Date anniversaire**, puis cliquez sur **Continuer**.
5. Cliquez sur **Je comprends les risques de perte de données liés à cette annulation** puis sur **Annuler la réplique**.


## Annulation de la réplication lorsque le volume principal est annulé

Lorsqu'un volume principal est annulé, le planning de réplication et le volume figurant dans le centre de données de réplique sont supprimés. Les répliques sont annulées à partir de la page {{site.data.keyword.blockstorageshort}}.

 1. Mettez en évidence votre volume sur la page **{{site.data.keyword.blockstorageshort}}**.
 2. Cliquez sur **Actions** et sélectionnez **Annuler stockage par bloc**.
 3. Choisissez le moment de l'annulation du volume, Choisissez **Immédiatement** ou **Date anniversaire**, puis cliquez sur **Continuer**.
 4. Cliquez sur **Je comprends les risques de perte de données liés à cette annulation** puis sur **Annuler**.
