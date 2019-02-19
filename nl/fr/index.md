---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Initiation à {{site.data.keyword.blockstorageshort}}
{: #GettingStarted}

{{site.data.keyword.blockstoragefull}} est un stockage iSCSI persistant, haute performance, qui est mis à disposition et géré indépendamment des instances de calcul. Les numéros d'unité logique {{site.data.keyword.blockstorageshort}} basés sur iSCSI sont connectés à des périphériques autorisés via des connexions en E-S multi-accès (MPIO) redondantes.

{{site.data.keyword.blockstorageshort}} fournit les meilleurs niveaux de durabilité et de disponibilité du marché grâce à un ensemble de fonctionnalités hors pair. Il a été construit dans le respect des normes de l'industrie et des meilleures pratiques. {{site.data.keyword.blockstorageshort}} est conçu pour protéger l'intégrité des données et assurer la disponibilité pendant des événements de maintenance et des pannes inattendues et offrir une base de référence cohérente pour les performances.

## Fonctions principales

Profitez des fonctionnalités suivantes de {{site.data.keyword.blockstorageshort}} :

- **Base de référence cohérente pour les performances**
   - Fournie grâce à l'allocation d'E-S/s de niveau protocole à des volumes individuels.
- **Durabilité et résilience élevées**
   - Protège l'intégrité des données et assure la disponibilité pendant des événements de maintenance et des pannes inattendues, sans avoir besoin de créer et gérer des grappes RAID au niveau du système d'exploitation.
- **Chiffrement des données au repos** ([disponible dans certains centres de données](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Chiffrement géré par le fournisseur pour les données au repos sans coût supplémentaire
- **Stockage entièrement sécurisé par mémoire flash** ([disponible dans certains centres de données](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Stockage entièrement sécurisé par mémoire flash pour les volumes mis à disposition avec Endurance ou Performance à 2 E-S/s/Go au minimum
- **Instantanés** ([disponibles dans certains centres de données](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Capture des instantanés de données ponctuels de manière transparente.
- **Réplication** ([disponible dans certains centres de données](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Copie automatiquement des instantanés vers un centre de données {{site.data.keyword.BluSoftlayer_full}} partenaire.
- **Connectivité hautement disponible**
   - Utilise des connexions réseau redondantes pour accroître la disponibilité
   - {{site.data.keyword.blockstorageshort}} basé sur iSCSI utilise l'E-S multi-accès (MPIO)
- **Accès simultané**
   - Permet à plusieurs hôtes d'accéder simultanément à des volumes de blocs (jusqu'à huit) pour les configurations en clusters.
- **Cluster de bases de données**
   - Prend en charge des cas d'utilisation avancés, tels que des bases de données en clusters.

## Facturation

Vous pouvez sélectionner une facturation à l'heure ou au mois pour un numéro d'unité logique Block Storage. Le type de facturation sélectionné pour un numéro d'unité logique s'applique à son espace d'image instantanée et à ses répliques. Par exemple, si vous mettez à disposition un numéro d'unité logique avec une facturation horaire, tous les frais liés aux instantanés ou aux répliques seront facturés à l'heure. Si vous mettez à disposition un numéro d'unité logique avec une facturation mensuelle, tous les frais liés aux instantanés ou aux répliques sont facturés au mois.

Avec la **facturation horaire**, le nombre d'heures d'existence du numéro d'unité logique de bloc sur le compte est calculé lors de la suppression du numéro d'unité logique ou à la fin du cycle de facturation, selon l'événement qui se produit en premier. La facturation horaire est un bon choix si vous avez besoin d'un stockage pour quelques jours ou pour moins d'un mois complet. La facturation horaire est disponible uniquement pour le stockage qui est mis à disposition dans des [centres de données sélectionnés](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations).

Avec la **facturation mensuelle**, le calcul du prix est calculé au prorata depuis la date de création jusqu'à la fin du cycle de facturation et la facturation est immédiate. Aucun remboursement n'est possible si un numéro d'unité logique est supprimé avant la fin du cycle de facturation. La facturation mensuelle convient si vous avez besoin d'un stockage pour des charges de travail qui utilisent des données devant être stockées et rester accessibles pour de longues périodes (un mois ou plus).

**Performance**
<table>
  <caption>Le tableau 1 contient les prix du stockage Performance avec une facturation mensuelle et horaire.</caption>
  <tr>
   <th>Prix mensuel</th>
   <td>0,10 $/Go + 0,07 $/IOP</td>
  </tr>
  <tr>
   <th>Prix horaire</th>
   <td>0,0001 $/Go + 0,0002 $/IOP</td>
  </tr>
</table>

**Endurance**
<table>
  <caption>Le tableau 2 contient les prix du stockage Endurance avec chaque niveau avec des options de facturation mensuelle et horaire.</caption>
  <tr>
   <th>Niveau d'IOPS</th>
   <th>0,25 IOPS/Go</th>
   <th>2 IOPS/Go</th>
   <th>4 IOPS/Go</th>
   <th>10 IOPS/Go</th>
  </tr>
  <tr>
   <th>Prix mensuel</th>
   <td>0,06 $/Go</td>
   <td>0,15 $/Go</td>
   <td>0,20 $/Go</td>
   <td>0,58 $/Go</td>
  </tr>
  <tr>
   <th>Prix horaire</th>
   <td>0,0001 $/Go</td>
   <td>0,0002 $/Go</td>
   <td>0,0003 $/Go</td>
   <td>0,0009 $/Go</td>
  </tr>
</table>



## Mise à disposition

Des numéros d'unité logique {{site.data.keyword.blockstorageshort}} peuvent être mis à disposition de 20 Go à 12 To avec deux options : <br/>
- Effectuez la mise à disposition avec des niveaux **Endurance** offrant des niveaux de performance prédéfinis et d'autres fonctionnalités telles que les instantanés et la réplication.
- Créez un environnement de **Performance** haute puissance avec des opérations d'entrée-sortie par seconde (IOPS) allouées.

### Mise à disposition avec des niveaux Endurance

{{site.data.keyword.blockstorageshort}} Endurance est disponible avec quatre niveaux de performance d'IOPS permettant de prendre en charge divers besoins d'application. <br />

- L'option **0,25 IOPS par Go** est adaptée aux charges de travail peu exigeantes en E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données inactives à un moment donné. Exemples d'applications : stockage de boîtes aux lettres ou partages de fichiers au niveau d'un service dans une entreprise.

- L'option **2 IOPS par Go** est adaptée à des usages plus généraux. Exemples d'applications : hébergement de petites bases de données qui sauvegardent des applications Web ou des images de disques de machine virtuelle pour un hyperviseur.

- L'option **4 IOPS par Go** est adaptée aux charges de travail plus exigeantes en E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données actives à un moment donné. Exemples d'applications : bases de données transactionnelles, bases de données sensibles aux performances.

- L'option **10 IOPS par Go** est adaptée aux charges de travail les plus intensives, telles que celles créées par les bases de données NoSQL et le traitement de données pour Analytics. Ce niveau est disponible pour le stockage mis à disposition jusqu'à 4 To uniquement dans des [centres de données sélectionnés](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations).

Un volume de type Endurance de 12 To comporte un maximum de 48 000 IOPS disponibles.

Il est essentiel de choisir le niveau d'endurance adapté pour votre charge de travail. Il est également important d'utiliser la taille de bloc, la vitesse de connexion Ethernet et le nombre d'hôtes appropriés afin d'atteindre des performances maximales. La non-concordance de l'un de ces éléments peut avoir un impact important sur le débit généré.


### Mise à disposition avec Performance

Performance est une classe de {{site.data.keyword.blockstorageshort}} conçue pour prendre en charge des applications avec un niveau élevé d'entrée/sortie et nécessitant un niveau de performance bien établi qui ne correspond pas à un niveau Endurance. Pour atteindre la performance prévue, il suffit d'allouer les IOPS au niveau du protocole à des volumes individuels. Des IOPS allant de 100 à 48 000 peuvent être mis à disposition avec des tailles de stockage de 20 Go à 12 To.

Le niveau Performance pour {{site.data.keyword.blockstorageshort}} est accessible et monté via une connexion iSCSI d'E-S multi-accès. {{site.data.keyword.blockstorageshort}} est généralement utilisé lorsqu'un seul serveur doit accéder au volume. Plusieurs volumes peuvent être montés sur un hôte et segmentés pour atteindre des volumes plus importants et un nombre d'E-S/s plus élevé. Des volumes de performance peuvent être commandés selon les tailles et les E-S/s figurant dans le tableau 3 pour les systèmes d'exploitation Linux, XEN et Windows.


<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>Le tableau 3 présente les combinaisons de taille et d'IOPS possibles pour le stockage Performance.<br/><sup><img src="/images/numberone.png" alt="Note de bas de page" /></sup> Vous pouvez opter pour un nombre d'IOPS supérieur à 6 000 dans des centres de données sélectionnés.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
          <tr>
            <th>Taille (Go)</th>
            <th>Nb min d'IOPS</th>
            <th>Nb max d'IOPS</th>
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
            <td>6 000 ou 20 000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>2 000-3 000</td>
            <td>200</td>
            <td>6 000 ou 40 000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>4 000-7 000</td>
            <td>300</td>
            <td>6 000 ou 48 000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>8 000-9 000</td>
            <td>500</td>
            <td>6 000 ou 48 000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>10 000-12 000</td>
            <td>1 000</td>
            <td>6 000 ou 48 000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
</table>


Les volumes Performance sont conçus pour fonctionner d'une manière cohérente proche du niveau d'IOPS mis à disposition. La cohérence facilite le dimensionnement et la mise à l'échelle des environnements d'application avec un niveau de performance donné. De plus, il est possible d'optimiser un environnement en créant un volume avec le rapport idéal prix/performance.

### Remarques sur la mise à disposition

**Taille de bloc**

Les IOPS pour les niveaux Endurance et Performance se fondent sur une taille de bloc de 16 Ko avec une charge de travail aléatoire de 50 % de lectures/écritures. Un bloc de 16 Ko équivaut à une écriture sur le volume.
{:important}

La taille de bloc utilisée par votre application a une incidence directe sur les performances de stockage. Si la taille de bloc employée par votre application est inférieure à 16 Ko, la limite des opérations d'entrée-sortie par seconde est atteinte avant la limite de débit. A l'inverse, si la taille de bloc qui est utilisée par votre application est supérieure à 16 Ko, la limite de débit est atteinte avant la limite des opérations d'entrée-sortie par seconde.

<table>
  <caption>Le tableau 4 présente des exemples de l'impact de la taille de bloc et des opérations d'entrée-sortie par seconde sur le débit.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>Taille de bloc (ko)</th>
            <th>IOPS</th>
            <th>Débit (Mo/s)</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>4 (standard pour Linux)</td>
            <td>1 000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8 (standard pour Oracle)</td>
            <td>1 000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1 000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32 (standard pour SQL Server)</td>
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

**Hôtes autorisés**

Un autre facteur à prendre en compte est le nombre d'hôtes qui utilisent votre volume. Si un seul hôte accède au volume, il peut s'avérer difficile de réaliser le nombre maximal d'IOPS disponible, surtout avec des nombres d'IOPS extrêmes (10 000). Si votre charge de travail requiert un débit élevé, il est préférable de configurer au moins deux serveurs pour accéder à votre volume afin d'éviter un goulot d'étranglement dû à un seul serveur.

**Connexion réseau**

La vitesse de votre connexion Ethernet doit être supérieure au débit maximal attendu de votre volume. En règle générale, vous ne devriez pas saturer votre connexion Ethernet au-delà de 70 % de la bande passante disponible. Par exemple, si vous disposez de 6 000 IOPS et que vous utilisez une taille de bloc de 16 Ko, le volume peut traiter un débit d'environ 94 Mo par seconde. Si vous disposez d'une connexion Ethernet de 1 Gbit/s
vers votre numéro d'unité logique, vous rencontrez un goulot d'étranglement lorsque vos serveurs tentent d'utiliser le débit maximal disponible. Cela est dû au fait que 70 % de la limite théorique d'une connexion Ethernet de 1 Gbit/s (125 Mo par seconde) n'autorisent que 88 Mo par seconde.

Pour atteindre le nombre maximal d'IOPS, vous devez mettre en place les ressources réseau adéquates. Vous devez également tenir compte de l'utilisation du réseau privé en dehors du stockage, ainsi que des réglages côté hôte et spécifiques aux applications (pile IP ou [nombre de lignes de file d'attente](/docs/infrastructure/BlockStorage?topic=BlockStorage-hostqueuesettings), etc.).

Le trafic de stockage est inclus dans l'utilisation réseau totale des serveurs virtuels publics. Pour plus d'informations sur les limites que peut imposer le service, voir la [documentation sur les serveurs virtuels](/docs/vsi?topic=virtual-servers-public-virtual-servers).
{:tip}

## Soumission de votre commande

Lorsque vous êtes prêt à passer votre commande, utilisez la [console](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole) ou [SLCLI](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI).

## Connexion de votre nouveau stockage

Lorsque votre demande de mise à disposition est terminée, autorisez vos hôtes à accéder au nouveau stockage et configurez votre connexion. Suivez le lien approprié en fonction du système d'exploitation de votre hôte.
- [Connexion à des numéros d'unité logique (LUN) sous Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connexion à des numéros d'unité logique (LUN) sous CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connexion à des numéros d'unité logique (LUN) sous Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Configuration de Block Storage pour une sauvegarde avec cPanel](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configuration de Block Storage pour une sauvegarde avec Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)
