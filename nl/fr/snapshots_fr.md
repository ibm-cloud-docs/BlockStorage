---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Instantanés

Les instantanés sont une fonctionnalité d'{{site.data.keyword.blockstoragefull}}. Un instantané représente le contenu d'un volume à un moment donné. Les instantanés vous permettent de protéger vos données sans impact sur les performances, en consommant un minimum d'espace, et sont considérés comme la première ligne de défense pour la protection des données. Les données peuvent être facilement et rapidement restaurées à partir d'une copie d'instantané si un utilisateur modifie ou supprime par erreur des données critiques d'un volume. 

{{site.data.keyword.blockstorageshort}} vous propose deux façons de prendre des instantanés : via un planning d'instantané configurable qui crée et supprime automatiquement des copies d'instantané pour chaque volume de stockage. Vous pouvez également créer des plannings d'instantané supplémentaires, supprimer manuellement des copies et gérer des plannings en fonction de vos besoins. La seconde méthode consiste à prendre un instantané manuellement. 

Une copie d'instantané est une image en lecture seule d'un numéro d'unité logique {{site.data.keyword.blockstorageshort}}, qui capture l'état du volume à un moment donné. Les copies d'instantané sont extrêmement efficaces à la fois en ce qui concerne le temps nécessaire à leur création et en matière d'occupation de l'espace de stockage. La création d'une copie d'instantané {{site.data.keyword.blockstorageshort}} ne prend que quelques secondes, en général moins d'une, quelle que soit la taille du volume ou le niveau d'activité sur le stockage. Après la création d'une copie d'instantané, les modifications apportées aux objets de données sont reflétées dans les mises à jour de la version en cours des objets, comme s'il n'existait pas de copies d'instantané. Pendant ce temps, la copie des données reste complètement stable.  

Une copie d'instantané n'a aucun impact sur les performances ; les utilisateurs peuvent facilement stocker jusqu'à 50 instantanés planifiés et 50 instantanés manuels par volume {{site.data.keyword.blockstorageshort}}, qui sont tous accessibles en tant que versions en ligne et en lecture seule des données. 


Les instantanés permettent aux utilisateurs : 

- de créer de manière transparente des points de récupération à un point de cohérence,
- de restaurer des volumes ponctuels antérieurs.

Vous devez acheter une certaine quantité d'espace d'instantané pour votre volume afin de prendre des instantanés de ce dernier. L'espace d'instantané peut être ajouté lors de la première commande de volume ou après sa mise à disposition initiale via la page **Détails du volume** et en cliquant sur le bouton de menu déroulant **Actions**, puis en sélectionnant **Ajouter de l'espace d'instantané**. Etant donné que les instantanés planifiés et manuels partagent l'espace d'instantané, commandez votre espace en conséquence. Consultez l'article [Commande d'instantanés](ordering-snapshots.html) pour plus de détails et conseils. 

## Meilleures pratiques relatives aux instantanés

La conception d'instantanés dépend de l'environnement du client. Les considérations suivantes relatives à la conception vous aident à planifier et implémenter des copies d'instantané :  
- 	Vous pouvez créer jusqu'à 50 instantanés grâce à la planification et jusqu'à 50 instantanés manuellement sur chaque volume ou numéro d'unité logique.  
- 	Ne prenez pas trop d'instantanés. Vérifiez que la fréquence planifiée pour vos instantanés répond à vos besoins en matière d'objectif de temps de reprise et d'objectif de point de reprise, ainsi qu'à vos exigences métier en matière d'application, en planifiant des instantanés sur une base horaire, quotidienne ou hebdomadaire.  
- 	La suppression automatique des instantanés doit être utilisée pour contrôler l'accroissement de la consommation d'espace de stockage. <br/>
    **Remarque** : Le seuil de suppression automatique est fixé à 95 %.
    
Veuillez garder à l'esprit que les instantanés ne remplacent pas la réplication réelle des données hors site, ni leur conservation pendant une longue période. 
    
## Comment les copies d'instantané consomment-elles l'espace disque ? 

Les copies d'instantané réduisent la consommation du disque en conservant des blocs individuels plutôt que des fichiers entiers. Les copies d'instantané ne commencent à consommer de l'espace supplémentaire qu'en cas de modification ou de suppression des fichiers situés dans le système de fichiers actif. Dans ce cas, les blocs de fichier d'origine sont toujours conservés dans le cadre d'une ou plusieurs copies d'instantané.
Dans le système de fichiers actif, les blocs modifiés sont réécrits dans des emplacements différents sur le disque ou supprimés complètement en tant que blocs de fichier actifs. Ainsi, outre l'espace disque utilisé par les blocs dans le système de fichiers actif modifié, l'espace disque utilisé par les blocs d'origine est toujours réservé pour refléter le statut du système de fichiers actif avant la modification. 

<table>
    <colgroup>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
    </colgroup>
    <tbody>
      <tr>
        <th colspan="3" style="border: 0.0px;text-align: center;">Utilisation de l'espace disque avant et après la copie d'instantané</th>
     </tr><tr>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle1.png" alt="Avant la copie d'instantané"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle3.png" alt="Après la copie d'instantané"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle2.png" alt="Modifications après la copie d'instantané"></td>
     </tr><tr>
        <td style="border: 0.0px;">Avant la création d'une copie d'instantané, l'espace disque est consommé uniquement par le système de fichiers actif. </td>
        <td style="border: 0.0px;">Après la création d'une copie d'instantané, le système de fichiers actif et la copie d'instantané pointent vers les mêmes blocs de disque. La copie d'instantané n'utilise pas d'espace disque supplémentaire. </td>
        <td style="border: 0.0px;">Après la suppression du fichier <i>myfile.txt</i> du système de fichiers actif, la copie d'instantané inclut toujours le fichier et référence ses blocs de disque. C'est la raison pour laquelle la suppression des données du système de fichiers actif ne libère pas toujours de l'espace disque. </td>
      </tr>
    </tbody>
</table>

Pour voir quelle quantité d'espace d'instantané est utilisée, suivez les instructions du document [Gestion des instantanés](working-with-snapshots.html). 






