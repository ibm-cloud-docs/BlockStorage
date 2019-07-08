---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# En savoir plus sur {{site.data.keyword.blockstorageshort}}
{: #About}

{{site.data.keyword.blockstoragefull}} est un stockage iSCSI persistant, haute performance, qui est mis à disposition et géré indépendamment des instances de calcul. Les numéros d'unité logique {{site.data.keyword.blockstorageshort}} basés sur iSCSI sont connectés à des périphériques autorisés via des connexions en E-S multi-accès (MPIO) redondantes.

{{site.data.keyword.blockstorageshort}} fournit les meilleurs niveaux de durabilité et de disponibilité du marché grâce à un ensemble de fonctionnalités hors pair. Il a été construit dans le respect des normes de l'industrie et des meilleures pratiques. {{site.data.keyword.blockstorageshort}} est conçu pour protéger l'intégrité des données et assurer la disponibilité pendant des événements de maintenance et des pannes inattendues et offrir une base de référence cohérente pour les performances.

## Fonctions principales
{: #corefeatures}

Profitez des fonctionnalités suivantes de {{site.data.keyword.blockstorageshort}} :

- **Base de référence cohérente pour les performances**
   - Fournie grâce à l'allocation d'IOPS de niveau protocole à des volumes individuels.
- **Durabilité et résilience élevées**
   - Protège l'intégrité des données et assure la disponibilité pendant des événements de maintenance et des pannes inattendues, sans avoir besoin de créer et gérer des grappes RAID au niveau du système d'exploitation.
- **Chiffrement des données au repos** ([Disponible dans la plupart des centres de données](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC))
   - Chiffrement géré par le fournisseur pour les données au repos sans coût supplémentaire
- **Stockage entièrement sécurisé par mémoire flash** ([Disponible dans la plupart des centres de données](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC))
   - Stockage entièrement sécurisé par mémoire flash pour les volumes mis à disposition avec Endurance ou Performance à 2 IOPS/Go au minimum
- **Instantanés** ([Disponible dans la plupart des centres de données](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC))
   - Capture des instantanés de données ponctuels de manière transparente.
- **Réplication** ([Disponible dans la plupart des centres de données](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC))
   - Copie automatiquement des instantanés vers un centre de données {{site.data.keyword.cloud}} partenaire.
- **Connectivité hautement disponible**
   - Utilise des connexions réseau redondantes pour accroître la disponibilité
   - {{site.data.keyword.blockstorageshort}} basé sur iSCSI utilise l'E-S multi-accès (MPIO)
- **Accès simultané**
   - Permet à plusieurs hôtes d'accéder simultanément à des volumes de blocs (jusqu'à huit) pour les configurations en clusters.
- **Cluster de bases de données**
   - Prend en charge des cas d'utilisation avancés, tels que des bases de données en clusters.


## Mise à disposition
{: #provisioning}

Des numéros d'unité logique {{site.data.keyword.blockstorageshort}} peuvent être mis à disposition de 20 Go à 12 To avec deux options : <br/>
- Effectuez la mise à disposition avec des niveaux **Endurance** offrant des niveaux de performance prédéfinis et d'autres fonctionnalités telles que les instantanés et la réplication.
- Créez un environnement de **Performance** haute puissance avec des opérations d'entrée-sortie par seconde (IOPS) allouées.


### Mise à disposition avec des niveaux Endurance
{: #provendurance}

{{site.data.keyword.blockstorageshort}} Endurance est disponible avec quatre niveaux de performance d'IOPS permettant de prendre en charge divers besoins d'application. <br />

- L'option **0,25 IOPS par Go** est adaptée aux charges de travail peu exigeantes en E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données inactives à un moment donné. Exemples d'applications : stockage de boîtes aux lettres ou partages de fichiers au niveau d'un service dans une entreprise.

- L'option **2 IOPS par Go** est adaptée à des usages plus généraux. Exemples d'applications : hébergement de petites bases de données qui sauvegardent des applications Web ou des images de disques de machine virtuelle pour un hyperviseur.

- L'option **4 IOPS par Go** est adaptée aux charges de travail plus exigeantes en E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données actives à un moment donné. Exemples d'applications : bases de données transactionnelles, bases de données sensibles aux performances.

- L'option **10 IOPS par Go** est adaptée aux charges de travail les plus intensives, telles que celles créées par les bases de données NoSQL et le traitement de données pour Analytics. Ce niveau est disponible pour du stockage mis à disposition jusqu'à 4 To dans [la plupart des centres de données](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC).

Un volume de type Endurance de 12 To comporte un maximum de 48 000 IOPS disponibles.

Il est essentiel de choisir le niveau d'endurance adapté pour votre charge de travail. Il est également important d'utiliser la taille de bloc, la vitesse de connexion Ethernet et le nombre d'hôtes appropriés afin d'atteindre des performances maximales. La non-concordance de l'un de ces éléments peut avoir un impact important sur le débit généré.


### Mise à disposition avec Performance
{: #provperformance}

Performance est une classe de {{site.data.keyword.blockstorageshort}} conçue pour prendre en charge des applications avec un niveau élevé d'entrée/sortie et nécessitant un niveau de performance bien établi qui ne correspond pas à un niveau Endurance. Pour atteindre la performance prévue, il suffit d'allouer les IOPS au niveau du protocole à des volumes individuels. Des IOPS allant de 100 à 48 000 peuvent être mis à disposition avec des tailles de stockage de 20 Go à 12 To.

Le niveau Performance pour {{site.data.keyword.blockstorageshort}} est accessible et monté via une connexion iSCSI d'E-S multi-accès. {{site.data.keyword.blockstorageshort}} est généralement utilisé lorsqu'un seul serveur doit accéder au volume. Plusieurs volumes peuvent être montés sur un hôte et segmentés pour atteindre des volumes plus importants et un nombre d'IOPS plus élevé. Des volumes de performance peuvent être commandés selon les tailles et les IOPS figurant dans le tableau 3 pour les systèmes d'exploitation Linux, XEN et Windows.

| Taille (Go) | Nb min d'IOPS | Nb max d'IOPS
|-----|-----|-----|
| 20 | 100 | 1 000 |
| 40 | 100 | 2 000  |
| 80 | 100 | 4 000 |
| 100 | 100 | 6 000 |
| 250 | 100 | 6 000 |
| 500 | 100  | 6 000 ou 10 000 |
| 1 000 | 100 | 6 000 ou 20 000 ![note de bas de page](/images/numberone.png) |
| 2 000 | 200 | 6 000 ou 40 000 ![note de bas de page](/images/numberone.png) |
| 3 000 à 7 000 | 300 | 6 000 ou 48 000 ![note de bas de page](/images/numberone.png) |
| 8 000 à 9 000 | 500 | 6 000 ou 48 000 ![note de bas de page](/images/numberone.png) |
| 10 000 à 12 000 | 1 000 | 6 000 ou 48 000 ![note de bas de page](/images/numberone.png) |
{: row-headers}
{: class="comparison-table"}
{: caption="Comparaison de tableaux" caption-side="top"}
{: summary="Table 1 is showing the possible minimum and maximum IOPS rates based of the volume size. This table has row and column headers. The row headers identify the volume size range. The column headers identify the minimum and maximum IOPS levels. To understand what IOPS rates you can expect from your Storage, navigate to the row and review the two options."}

![Note de bas de page](/images/numberone.png) *Vous pouvez opter pour un nombre d'IOPS supérieur à 6 000 dans la plupart des centres de données.*

Les volumes Performance sont conçus pour fonctionner d'une manière cohérente proche du niveau d'IOPS mis à disposition. La cohérence facilite le dimensionnement et la mise à l'échelle des environnements d'application avec un niveau de performance donné. De plus, il est possible d'optimiser un environnement en créant un volume avec le rapport idéal prix/performance.

## Facturation
{: #billing}

Vous pouvez sélectionner une facturation à l'heure ou au mois pour un numéro d'unité logique Block Storage. Le type de facturation sélectionné pour un numéro d'unité logique s'applique à son espace d'instantané et à ses répliques. Par exemple, si vous mettez à disposition un numéro d'unité logique avec une facturation horaire, tous les frais liés aux instantanés ou aux répliques seront facturés à l'heure. Si vous mettez à disposition un numéro d'unité logique avec une facturation mensuelle, tous les frais liés aux instantanés ou aux répliques sont facturés au mois.

 * Avec la **facturation horaire**, le nombre d'heures d'existence du numéro d'unité logique de bloc sur le compte est calculé lors de la suppression du numéro d'unité logique ou à la fin du cycle de facturation, selon l'événement qui se produit en premier. La facturation horaire est un bon choix si vous avez besoin d'un stockage pour quelques jours ou pour moins d'un mois complet. La fonctionnalité de facturation à l'heure est disponible dans [la plupart des centre de données](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC).

 * Avec la **facturation mensuelle**, le calcul du prix est calculé au prorata depuis la date de création jusqu'à la fin du cycle de facturation et la facturation est immédiate. Aucun remboursement n'est possible si un numéro d'unité logique est supprimé avant la fin du cycle de facturation. La facturation mensuelle convient si vous avez besoin d'un stockage pour des charges de travail qui utilisent des données devant être stockées et rester accessibles pour de longues périodes (un mois ou plus).

### Endurance
{: #pricing-comparison-endurance}

| Options de tarification pour niveaux d'IOPS prédéfinis | 0,25 IOPS | 2 IOPS/Go | 4 IOPS/Go | 10 IOPS/Go |
|-----|-----|-----|-----|-----|
| Prix mensuel | 0,06 $/Go | 0,15 $/Go | 0,20 $/Go | 0,58 $/Go |
| Prix horaire | 0,0001 $/Go | 0,0002 $/Go | 0,0003 $/Go | 0,0009 $/Go |
{: row-headers}
{: class="comparison-table"}
{: caption="Comparaison de tableaux" caption-side="top"}
{: summary="Table 2 is showing the prices for Endurance Storage for each tier with monthly and hourly billing options. This table has row and column headers. The row headers identify the billing options. The column headers identify the IOPS level that is chosen for the service. To understand what your price is located in the table, navigate to the column and review the two different billing options for that IOPS tier."}

### Performances
{: #pricing-comparison-performance}

| Options de tarification pour IOPS personnalisé | Calcul de tarification |
|-----|-----|
| Prix mensuel | 0,10 $/Go + 0,07 $/IOP |
| Prix horaire | 0,0001 $/Go + 0,0002 $/IOP |
{: row-headers}
{: class="comparison-table"}
{: caption="Comparaison de tableaux" caption-side="top"}
{: summary="Table 3 is showing the prices for Performance Storage with monthly and hourly billing. This table has row and column headers. The row headers identify the billing options. To see what your cost for Storage is, navigate to the row of the billing option you are interested in."}
