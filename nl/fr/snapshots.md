---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-01"

---
{:new_window: target="_blank"}

# Instantanés

Les instantanés sont une fonctionnalité d'{{site.data.keyword.blockstoragefull}}. Un instantané représente le contenu d'un volume à un point précis dans le temps. Les instantanés vous permettent de protéger vos données sans impact sur les performances et avec une consommation minimale de l'espace. Ils sont considérés comme votre première ligne de défense dans le cadre de la protection des données. Si un utilisateur modifie ou supprime par erreur des données critiques d'un volume, les données peuvent être facilement et rapidement restaurées à partir d'une copie d'instantané.

{{site.data.keyword.blockstorageshort}} vous propose deux façons de prendre des instantanés.

– La première via un planning d'instantané configurable qui crée et supprime automatiquement des copies d'instantané pour chaque volume de stockage. Vous pouvez également créer des plannings d'instantané supplémentaires, supprimer des copies manuellement et gérer les plannings en fonction de vos besoins. 
- La seconde méthode consiste à prendre un instantané manuel.

Une copie d'instantané est une image en lecture seule d'un numéro d'unité logique {{site.data.keyword.blockstorageshort}}, qui capture l'état du volume à un moment donné. Les copies d'image instantanée sont efficaces en termes de durée de création et d'espace de stockage. La création d'une copie d'instantané {{site.data.keyword.blockstorageshort}} ne prend que quelques secondes, en général moins d'une, quelle que soit la taille du volume ou le niveau d'activité sur le stockage. Une fois une copie d'image instantanée créée, les modifications apportées aux objets de données sont reflétées dans les mises à jour de la version en cours des objets, comme s'il n'existait pas de copies d'image instantanée. Pendant ce temps, la copie des données reste stable. 

Une copie d'image instantanée n'entraîne aucune baisse de performances. Les utilisateurs peuvent facilement stocker jusqu'à 50 instantanés planifiés et 50 instantanés manuels par volume {{site.data.keyword.blockstorageshort}}, qui sont tous accessibles en tant que versions en ligne et en lecture seule des données.

Les images instantanées vous permettent : 

- de créer de manière transparente des points de récupération à un point de cohérence,
- de restaurer des volumes ponctuels antérieurs.

Vous devez d'abord acheter une certaine quantité d'espace d'image instantanée pour votre volume avant de pouvoir prendre des instantanés de celui-ci. Il est possible d'ajouter de l'espace d'image instantanée lors de la commande initiale ou après via la page des **détails sur le volume**. Les images instantanées planifiées et manuelles partagent l'espace d'image instantanée ; veillez à commander l'espace en conséquence. Pour plus de détails et pour obtenir de l'aide, voir [Commande d'instantanés](ordering-snapshots.html).

**Meilleures pratiques concernant les instantanés**

La conception des instantanés dépend de l'environnement du client. Prenez en compte les remarques de conception suivantes pour planifier et implémenter les copies d'image instantanée : 
- Vous pouvez créer jusqu'à 50 instantanés grâce à la planification et jusqu'à 50 instantanés manuellement sur chaque volume ou numéro d'unité logique. 
- Ne prenez pas trop d'instantanés. Veillez à ce que la fréquence des images instantanés planifiées corresponde à vos besoins en termes d'objectif de temps de reprise et d'objectif de point de reprise, ainsi qu'à vos exigences professionnelles liées aux applications et planifiez des images instantanées à un rythme horaire, quotidien ou hebdomadaire. 
- La fonction de suppression automatique des instantanés permet de contrôler la croissance de la consommation d'espace de stockage. <br/>
  >**Remarque** - le seuil de suppression automatique est fixé à 95 %.
    
Les instantanés ne se substituent pas à la réplication de reprise après incident hors site ou à la sauvegarde à long terme.
    
**Comment les instantanés affectent-ils l'espace disque ?**

Les copies d'image instantanée minimisent l'utilisation de l'espace disque en conservant des blocs individuels plutôt que des fichiers entiers. Les copies d'instantané n'utilisent de l'espace supplémentaire qu'en cas de modification ou de suppression des fichiers situés dans le système de fichiers actif. Dans ce cas, les blocs de fichier d'origine sont toujours conservés dans une ou plusieurs copies d'image instantanée.

Dans le système de fichiers actif, les blocs modifiés sont réécrits à des emplacements différents sur le disque ou retirés sous la forme de blocs de fichier complets. Ainsi, outre l'espace disque employé par les blocs dans le système de fichiers actif modifié, l'espace disque qui est utilisé par les blocs d'origine est toujours conservé pour refléter le statut du système de fichiers actif avant la modification.

<table>
    <colgroup>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
    </colgroup>
      <tr>
        <th colspan="3" style="border: 0.0px;text-align: center;">Utilisation de l'espace disque avant et après une copie d'image instantanée</th>
     </tr><tr>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle1.png" alt="Avant une copie d'image instantanée"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle3.png" alt="Après une copie d'image instantanée"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle2.png" alt="Modifications après une copie d'image instantanée"></td>
     </tr><tr>
        <td style="border: 0.0px;">Avant la création d'une copie d'image instantanée, l'espace disque est utilisé uniquement par le système de fichiers actif.</td>
        <td style="border: 0.0px;">Après la création d'une copie d'image instantanée, le système de fichiers actif et la copie d'image instantanée pointent vers les mêmes blocs disque. La copie d'instantané n'utilise pas d'espace disque supplémentaire.</td>
        <td style="border: 0.0px;">Même après la suppression de <i>myfile.txt</i> du système de fichiers actif, la copie d'image instantanée inclut toujours le fichier et fait référence à ses blocs de disque. C'est la raison pour laquelle la suppression des données du système de fichiers actif ne libère pas toujours de l'espace disque.</td>
      </tr>
</table>

Pour voir quelle quantité d'espace d'instantané est utilisée, suivez les instructions de l'article [Gestion des instantanés](working-with-snapshots.html).
