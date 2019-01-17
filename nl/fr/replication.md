---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-08"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Réplication de données

La réplication utilise l'un de vos plannings d'instantané pour copier automatiquement des instantanés sur un volume de destination dans un centre de données distant. Les copies peuvent être récupérées sur le site distant en cas de données endommagées ou de catastrophe.

La réplication permet de synchroniser vos données entre deux emplacements différents. Si vous voulez cloner votre volume et l'utiliser indépendamment du volume d'origine, voir [Création d'un volume de blocs en double](how-to-create-duplicate-volume.html).
{:tip}

Avant d'effectuer une réplication, vous devez créer un planning d'instantané.
{:important}


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
          SYD05<br />
          MEL01<br />
          <br /><br /><br /><br /><br /><br /><br /><br />
      </td>
    </tr>
  </tbody>
</table>

## Création de la réplique initiale

Les réplications fonctionnent selon un planning d'instantané. Vous devez d'abord configurer un espace d'instantané et un planning d'instantané pour le volume source avant de pouvoir répliquer. Si vous tentez de configurer la réplication alors que l'espace d'instantané ou le planning d'instantané n'existe pas, vous serez invité à acheter davantage d'espace ou à configurer un planning. Les réplications sont gérées sous **Stockage**, **{{site.data.keyword.blockstorageshort}}** dans le portail [{{site.data.keyword.slportal}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/){:new_window}.

1. Cliquez sur votre volume de stockage.
2. Cliquez sur **Réplique**, puis sur **Acheter une réplication**.
3. Sélectionnez le planning d'instantané existant que vous souhaitez que votre réplication suive. La liste contient tous vos plannings d'instantané actifs. <br />
   Vous ne pouvez sélectionner qu'une seul planning, même si vous combinez des réplications horaires, quotidiennes et hebdomadaires. Tous les instantanés qui ont été capturés depuis le cycle de réplication précédent sont répliqués quel que soit leur planning d'origine.<br />Si vous n'avez pas configuré d'instantanés, vous êtes invité à le faire avant de pouvoir commander la réplication. Pour plus de détails, voir [Utilisation d'instantanés](snapshots.html).
   {:important}
3. Cliquez sur **Emplacement** et sélectionnez le centre de données qui est votre site de reprise après incident.
4. Cliquez sur **Continuer**.
5. Entrez un **Code promo** le cas échéant et cliquez sur **Recalculer**. Les autres zones de la boîte de dialogue contiennent les valeurs par défaut.

   Les remises sont appliquées lors du traitement de la commande.
   {:note}
6. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**.


## Edition d'une réplication existante

Vous pouvez éditer votre planning de réplication et modifier votre espace de réplication à partir de l'onglet **Principal** ou **Réplique** sous **Stockage**, **{{site.data.keyword.blockstorageshort}}** dans le portail [{{site.data.keyword.slportal}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/){:new_window}.



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


## Augmentation de l'espace d'image instantanée dans le centre de données de réplique lorsque l'espace d'image instantanée est augmenté dans le centre de données principal.

Les tailles des volumes de stockage principal et de réplique doivent être identiques. Il n'est pas possible que l'un soit plus grand que l'autre. Lorsque vous augmentez votre espace d'image instantanée dans le volume principal, l'espace de réplique est automatiquement augmenté. L'augmentation de l'espace d'image instantanée déclenche une mise à jour immédiate de la réplication. L'augmentation des deux volumes apparaît sous forme de lignes d'article dans votre facture et est calculée au prorata si nécessaire.

Pour plus d'informations sur l'augmentation de l'espace d'image instantanée, voir [Commande d'instantanés](ordering-snapshots.html).
{:tip}


## Affichage de l'historique des réplications

L'historique des réplications s'affiche dans le **journal d'audit** sur l'onglet **Compte** sous **Gérer**. Les historiques des réplications sont les mêmes pour le volume principal et le volume de réplique. Ces historiques incluent les éléments suivants :

- le type de réplication (basculement ou reprise par restauration) ;
- l'heure à laquelle la réplication a commencé ;
- l'instantané qui a été utilisé pour la réplication ;
- la taille de la réplication ;
- l'heure à laquelle la réplication s'est terminée.


## Création d'un doublon d'un volume de réplique

Vous pouvez créer un doublon d'un {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.blockstoragefull}} existant. Le volume en double hérite par défaut des options de capacité et de performance du volume d'origine et contient une copie des données jusqu'au point de cohérence d'un instantané.

Vous pouvez créer des doublons à partir de volumes principaux et de volumes de réplique. Le nouveau doublon est créé dans le même centre de données que le volume d'origine. Si vous créez un doublon à partir d'un volume de réplique, le nouveau volume est créé dans le même centre de données que le volume de réplique.

Les volumes dupliqués sont accessibles par un hôte en lecture/écriture dès la mise à disposition du stockage. Toutefois, les instantanés et la réplication ne sont pas autorisés tant que la copie des données depuis le volume d'origine vers le doublon n'est pas terminée.

Pour plus d'informations, voir [Création d'un volume de blocs en double](how-to-create-duplicate-volume.html).

## Utilisation de répliques afin d'effectuer un basculement en cas de sinistre

Lorsque vous effectuez un basculement, vous "basculez l'interrupteur" depuis votre volume de stockage du centre de données principal vers le volume de destination du centre de données distant. Par exemple, votre centre de données principal peut se situer à Londres et votre centre de données secondaire à Amsterdam. Dans le cas d'un événement d'échec, vous basculez vers Amsterdam, en vous connectant au volume qui est désormais devenu principal à partir d'une instance de calcul à Amsterdam. Une fois votre volume de Londres réparé, un instantané du volume d'Amsterdam est pris afin de permettre le retour à Londres avec le volume de Londres à nouveau considéré comme le volume principal à partir d'une instance de traitement située à Londres.

* Si l'emplacement principal est en danger imminent ou gravement impacté, voir [Basculement avec un volume principal accessible](dr-accessible-primary.html).
* Si l'emplacement principal est arrêté, voir [Basculement avec un volume principal inaccessible](disaster-recovery.html).


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
