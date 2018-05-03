---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# A propos de {{site.data.keyword.blockstorageshort}}

{{site.data.keyword.blockstoragefull}} est un stockage iSCSI persistant, haute performance, qui est mis à disposition et géré indépendamment des instances de calcul. Les numéros d'unité logique {{site.data.keyword.blockstorageshort}} basés sur iSCSI sont connectés à des périphériques autorisés via des connexions en E-S multi-accès (MPIO) redondantes.

{{site.data.keyword.blockstorageshort}} fournit les meilleurs niveaux de durabilité et de disponibilité grâce à un ensemble de fonctionnalités inégalé. Il a été construit dans le respect des normes de l'industrie et des meilleures pratiques, et conçu pour protéger l'intégrité des données et assurer la disponibilité pendant des événements de maintenance et des pannes inattendues, tout en offrant une base de performance cohérente.

## Fonctions principales

Profitez des fonctionnalités suivantes de {{site.data.keyword.blockstorageshort}} :

- **Base de performance cohérente**
   - Fournie grâce à l'allocation d'E-S/s de niveau protocole à des volumes individuels.
- **Haute durabilité et résilience**
   - Protège l'intégrité des données et assure la disponibilité pendant des événements de maintenance et des pannes inattendues, sans avoir besoin de créer et gérer des grappes RAID au niveau du système d'exploitation.
- **Chiffrement des données au repos** ([disponible dans certains centres de données](new-ibm-block-and-file-storage-location-and-features.html))
   - Chiffrement géré par le fournisseur pour les données au repos sans coût supplémentaire.
- **Stockage entièrement sécurisé par mémoire flash** ([disponible dans certains centres de données](new-ibm-block-and-file-storage-location-and-features.html))
   - Stockage entièrement sécurisé par mémoire flash pour les volumes mis à disposition avec Endurance ou Performance à 2 E-S/s/Go au minimum.
- **Instantanés** (en cas de mise à disposition avec Endurance ou Performance dans [certains centres de données](new-ibm-block-and-file-storage-location-and-features.html))
   - Capture des instantanés de données ponctuels de manière transparente.
- **Réplication** (en cas de mise à disposition avec Endurance ou Performance dans [certains centres de données](/new-ibm-block-and-file-storage-location-and-features.html))
   - Copie automatiquement des instantanés vers un centre de données {{site.data.keyword.BluSoftlayer_full}} partenaire.
- **Connectivité hautement disponible**
   - Utilise des connexions réseau redondantes pour optimiser la disponibilité - {{site.data.keyword.blockstorageshort}} basé sur iSCSI utilise l'E-S multi-accès (MPIO).
- **Accès simultané**
   - Permet à plusieurs hôtes d'accéder simultanément à des volumes de blocs (jusqu'à 8) pour les configurations en clusters.
- **Bases de données en clusters**
   - Prend en charge des cas d'utilisation avancés, tels que des bases de données en clusters.
     
## Facturation à l'heure/au mois

Vous pouvez sélectionner une facturation à l'heure ou au mois pour un numéro d'unité logique Block Storage. Le type de facturation sélectionné pour un numéro d'unité logique s'appliquera à son espace d'instantané et à ses répliques. Par exemple, si vous mettez à disposition un numéro d'unité logique avec une facturation à l'heure, les instantanés ou les frais de réplique seront facturés à l'heure. Si vous mettez à disposition un numéro d'unité logique avec une facturation au mois, les instantanés ou les frais de réplique seront facturés au mois. 

Avec une **facturation à l'heure**, le calcul du nombre d'heures pendant lesquelles le numéro d'unité logique du bloc a existé sur le compte s'effectue au moment de la suppression du numéro d'unité logique ou à la fin du cycle de facturation, selon l'événement qui se produit en premier.  La facturation à l'heure représente un bon choix pour un stockage qui est utilisé pendant quelques jours ou moins d'un mois entier. La facturation à l'heure est uniquement disponible pour un stockage mis à disposition dans [certains centres de données](new-ibm-block-and-file-storage-location-and-features.html). 

Avec une **facturation au mois**, le calcul du prix est proportionnel à la durée écoulée entre la date de création et la fin du cycle de facturation, et facturé immédiatement. Aucun remboursement n'est possible si un numéro d'unité logique est supprimé avant la fin du cycle de facturation.  La facturation au mois représente un bon choix pour un stockage utilisé dans des charges de travail de production qui utilisent des données devant être stockées et accessibles pendant de longues périodes (un mois ou plus). 

### Performance :
<table>
 <tbody>
  <tr>
   <th>Tarif mensuel</th>
   <td>0,10 $/Go + 0,07 $/E-S</td>
  </tr>
  <tr>
   <th>Tarif horaire</th>
   <td>0,0001 $/Go + 0,0002 $/E-S</td>
  </tr>
  </tbody>
</table>
 
### Endurance :
<table>
 <tbody>
  <tr>
   <th>Niveau d'E-S/s</th>
   <th>0,25 E-S/s/Go</th>
   <th>2 E-S/s/Go</th>
   <th>4 E-S/s/Go</th>
   <th>10 E-S/s/Go</th>
  </tr>
  <tr>
   <th>Tarif mensuel</th>
   <td>0,10 $/Go</td>
   <td>0,20 $/Go</td>
   <td>0,35 $/Go</td>
   <td>0,58 $/Go</td>
  </tr>
  <tr>
   <th>Tarif horaire</th>
   <td>0,0002 $/Go</td>
   <td>0,0003 $/Go</td>
   <td>0,0005 $/Go</td>
   <td>0,0009 $/Go</td>
  </tr>
  </tbody>
</table>



## Mise à disposition

Des numéros d'unité logique {{site.data.keyword.blockstorageshort}} peuvent être mis à disposition de 20 Go à 12 To avec deux options : <br/>
- Mise à disposition de niveaux **Endurance** avec des niveaux de performance prédéfinis et des fonctions telles que les instantanés et la réplication.
- Construction d'un environnement **Performance** haute puissance avec allocation d'opérations d'E-S par seconde (E-S/s). 

### Niveaux Endurance

Endurance est disponible dans trois niveaux de performance d'E-S/s pour prendre en charge les divers besoins des applications. <br />

- **0,25 E-S/s par Go** est adapté aux charges de travail avec une faible intensité d'E-S. Ces charges de travail se caractérisent généralement par un fort pourcentage de données inactives à un moment donné. Exemples d'applications : stockage de boîtes aux lettres ou partages de fichiers au niveau d'un service dans une entreprise.
- **2 E-S/s par Go** est adapté à des usages plus généraux. Exemples d'applications : hébergement de petites bases de données utilisées par des applications Web ou d'images de disques de machine virtuelle pour un hyperviseur.
- **4 E-S/s par Go** est adapté aux charges de travail de forte intensité. Ces charges de travail se caractérisent généralement par un pourcentage élevé de données actives à un moment donné. Exemples d'applications : bases de données transactionnelles, bases de données sensibles aux performances.
- **10 E-S/s par Go** est adapté aux charges de travail les plus exigeantes telles que celles créées par les bases de données NoSQL, et au traitement de données pour analyse.  Ce niveau est disponible pour le stockage mis à disposition d'une taille maximale de 4 To dans [certains centres de données](new-ibm-block-and-file-storage-location-and-features.html).

Jusqu'à 48 000 E-S/s sont disponibles dans un volume Endurance de 12 To.
 

Bien sûr, il est primordial de choisir le niveau adéquat d'Endurance {{site.data.keyword.blockstorageshort}} pour votre charge de travail, mais il est également important d'utiliser la taille de bloc, la vitesse de connexion Ethernet et le nombre d'hôtes nécessaires pour optimiser les performances. La non-concordance de l'un de ces éléments peut avoir un impact important sur le débit généré.

 
### Performance

Performance est une classe de {{site.data.keyword.blockstorageshort}} conçue pour prendre en charge des applications à forte entrée-sortie nécessitant des performances prédéfinies qui ne sont pas bien adaptées à un niveau Endurance. Les performances prévisibles sont obtenues par l'allocation d'E-S/s de niveau protocole à des volumes individuels. Les E-S/s comprises entre 100 et 48 000 peuvent être mises à disposition avec des tailles de stockage allant de 20 Go à 12 To. 

Performance pour {{site.data.keyword.blockstorageshort}} est accessible et monté via une connexion iSCSI d'E-S multi-accès. {{site.data.keyword.blockstorageshort}} est généralement utilisé lorsqu'une seule machine doit accéder au volume. Plusieurs volumes peuvent être montés sur un hôte et segmentés pour atteindre des volumes plus importants et un nombre d'E-S/s plus élevé. Des volumes de performance peuvent être commandés selon les tailles et les E-S/s figurant dans le Tableau 1 pour les systèmes d'exploitation Linux, XEN, VMware et Windows.


<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
          <tr>
            <th>Taille (Go)</th>
            <th>Nb min d'E-S/s</th>
            <th>Nb max d'E-S/s</th>
          </tr>
          <tr>
            <td>20</td>
            <td>100</td>
            <td>1 000</td>
          </tr>
          <tr>
            <td>40</td>
            <td>100</td>
            <td>2 000</td>
          </tr>
          <tr>
            <td>80</td>
            <td>100</td>
            <td>4 000</td>
          </tr>
          <tr>
            <td>100</td>
            <td>100</td>
            <td>6 000</td>
          </tr>
          <tr>
            <td>250</td>
            <td>100</td>
            <td>6 000</td>
          </tr>
          <tr>
            <td>500</td>
            <td>100</td>
            <td>6 000 ou 10 000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>1 000</td>
            <td>100</td>
            <td>6 000 ou 20 000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>2 000 à 3 000</td>
            <td>200</td>
            <td>6 000 ou 40 000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>4 000 à 7 000</td>
            <td>300</td>
            <td>6 000 ou 48 000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>8 000 à 9 000</td>
            <td>500</td>
            <td>6 000 ou 48 000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>10 000 à 12 000</td>
            <td>1 000</td>
            <td>6 000 ou 48 000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
        </tbody>
</table>

<sup>![footnote](/images/numberone.png)</sup> La limite d'E-S/s supérieure à 6 000 est disponible dans [certains centres de données](new-ibm-block-and-file-storage-location-and-features.html).


Les volumes de performance sont conçus pour fermer de façon cohérente le niveau d'E-S/s mis à disposition. La cohérence facilite le dimensionnement et la mise à l'échelle d'environnements d'application avec un niveau de performance donné. En outre, en fonction de la plage de tailles de volume et du nombre d'E-S/s, il devient possible d'optimiser un environnement en créant un volume avec le rapport prix-performance idéal.

### Conseils pour la mise à disposition d'E-S/s pour {{site.data.keyword.blockstorageshort}}

Les E-S/s pour les niveaux Endurance et Performance se fondent sur une taille de bloc de 16 ko avec une charge de travail aléatoire de 50 % de lectures/écritures. Un bloc de 16 ko équivaut à une écriture sur le volume.

La taille de bloc utilisée par votre application aura un impact direct sur les performances de stockage.  Si la taille de bloc utilisée par votre application est inférieure à 16 ko, la limite des E-S/s sera réalisée avant celle du débit.  A l'inverse, si la taille de bloc utilisée par votre application est supérieure à 16 ko, la limite de débit sera réalisée avant celle des E-S/s.

La modification de la taille de bloc affectera les performances comme suit :

<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
          <tr>
            <th>Taille de bloc (ko)</th>
            <th>E-S/s</th>
            <th>Débit (Mo/s)</th>
          </tr>
          <tr>
            <td>4 (classique pour Linux)</td>
            <td>1 000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8 (classique pour Oracle)</td>
            <td>1 000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1 000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32 (classique pour SQLServer)</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

Le choix de l'option {{site.data.keyword.blockstorageshort}} adaptée à votre charge de travail est important, tout comme la façon d'éviter les goulots d'étranglement. La vitesse de votre connexion Ethernet doit être plus élevée que le débit maximal attendu de votre volume. En règle générale, vous ne devez pas saturer votre connexion Ethernet au-delà de 70 % de la bande passante disponible. Par exemple, si vous disposez de 6 000 E-S/s et que vous utilisez une taille de bloc de 16 ko, le volume est capable de traiter environ 94 Mo par seconde. Si vous disposez d'une connexion Ethernet de 1 Gbit/s sur votre numéro d'unité logique, elle constituera un goulot d'étranglement lorsque vos serveurs tenteront d'utiliser le débit maximal disponible car 70 % de la limite théorique d'une connexion Ethernet de 1 Gbit/s (125 Mo par seconde) n'autorisera que 88 Mo par seconde.


Un autre facteur à prendre en compte est le nombre d'hôtes qui utilisent votre volume. Si un seul hôte accède au volume, il peut être difficile de réaliser le nombre maximal d'E-S/s disponible, en particulier sur des nombres d'E-S/s extrêmes (10 000 s). Si votre charge de travail nécessite un débit élevé, il est préférable de configurer au moins deux ou trois serveurs pour accéder à votre volume afin d'éviter un goulot d'étranglement sur un serveur unique.


Pour atteindre le nombre maximal d'E-S/s, vous devez mettre en place les ressources réseau adéquates. Les autres considérations incluent l'utilisation d'un réseau privé en dehors du stockage, ainsi que des optimisations spécifiques en matière d'application et d'hôte (pile IP, nombre de lignes de la file d'attente, etc.).
