---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords: Block Storage, use of a Block Storage volume, LUN, Block Storage

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}

# FAQ (Foire aux questions)
{: #block-storage-faqs}

## Combien d'instances peuvent partager l'utilisation d'un volume {{site.data.keyword.blockstorageshort}} ?
{: faq}

Le nombre d'autorisations par volume de blocs est limité par défaut à 8. Cela signifie que jusqu'à 8 hôtes peuvent être autorisés à accéder au numéro d'unité logique Block Storage. Les clients qui utilisent {{site.data.keyword.blockstorageshort}} dans leur déploiement VMware peuvent demander à ce que la limite d'autorisation soit augmentée jusqu'à 64. Pour demander une augmentation de limite, contactez votre ingénieur commercial ou créez un [cas de support](https://{DomainName}/unifiedsupport/cases/add){: external}.

## Combien de volumes peuvent être commandés ?
{: faq}

Par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes de bloc et de stockage de fichier. Pour augmenter votre limite, contactez votre commercial. Pour plus d'informations, voir [Gestion des limites de stockage](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits).

## Combien de volumes {{site.data.keyword.blockstorageshort}} peuvent être montés sur un hôte ?
{: faq}

Cela dépend de ce que peut gérer le système d'exploitation de l'hôte. Ce n'est pas {{site.data.keyword.cloud}} qui fixe une limite. Consultez la documentation de votre système d'exploitation pour connaître les limites fixées pour le nombre de volumes pouvant être montés.

## Puis-je joindre plusieurs numéros d'unité logique (LUN) à des paramètres de système d'exploitation différents ?
{: faq}

Non. Un hôte ne peut pas être autorisé à accéder en même temps à des numéros d'unité logique de différents types de système d'exploitation. Un hôte peut uniquement être autorisé à accéder à des numéros d'unité logique d'un type unique de système d'exploitation. Si vous tentez d'autoriser l'accès à plusieurs numéros d'unité logique de types de système d'exploitation différents, l'opération se traduit par une erreur.

## Quelle version de Windows dois-je choisir pour mon numéro d'unité logique Block Storage ?
{: #windowsOStypes}
{: faq}

Lorsque vous créez un numéro d'unité logique, vous devez spécifier le type de système d'exploitation. Le type de système d'exploitation doit être basé sur le système d'exploitation qui est utilisé par les hôtes qui accèdent au numéro d'unité logique. Le type de système d'exploitation ne peut pas être modifié une fois que le numéro d'unité logique est créé. La taille réelle du numéro d'unité logique peut varier légèrement en fonction du type de système d'exploitation du numéro d'unité logique.

** Windows 2008+**
- Le numéro d'unité logique stocke des données Windows pour Windows 2008 et versions ultérieures. Utilisez cette option de système d'exploitation si votre système d'exploitation hôte est Windows Server 2008, Windows Server 2012, Windows Server 2016. Les méthodes de partitionnement MBR et GPT sont toutes les deux prises en charge.

**Windows 2003**
- Le numéro d'unité logique stocke un type de disque brut dans un disque Windows à une partition qui utilise le style de partitionnement MBR (Master Boot Record). Utilisez cette option uniquement si votre système d'exploitation hôte est Windows 2000 Server, Windows XP ou Windows Server 2003 qui utilise la méthode de partitionnement MBR.

**Windows GPT**
-  Le numéro d'unité logique stocke des données Windows en utilisant le style de partitionnement GPT (GUID Partition Type). Utilisez cette option si vous voulez adopter la méthode de partitionnement GPT et que votre hôte est peut l'utiliser. Windows Server 2003, Service Pack 1 et les niveaux ultérieurs peuvent utiliser la méthode de partitionnement GPT, et toutes les versions 64 bits de Windows la prenne en charge.

## La limite du nombre d'IOPS est-elle imposée par instance ou par volume ?
{: faq}

Les IOPS sont imposées au niveau du volume. En d'autres termes, deux hôtes connectés à un volume avec 6 000 IOPS partagent ces 6 000 IOPS.

## Mesure des IOPS
{: faq}

Les IOPS sont mesurées en fonction d'un profil de chargement de blocs de 16 Ko avec une répartition aléatoire de 50 % de lectures et 50 % d'écritures. Les charges de travail qui diffèrent de ce profil sont susceptibles de connaître une baisse des performances.

## Que se passe-t-il lorsqu'une taille de bloc inférieure est utilisée pour mesurer les performances ?
{: faq}

Le nombre maximal d'IOPS peut être obtenu même si vous utilisez des tailles de bloc plus petites. Toutefois, le débit devient plus lent. Par exemple, un volume doté de 6 000 IOPS présente les débits suivants en fonction des tailles de bloc :

- 16 ko * 6 000 IOPS == ~93,75 Mo/sec
- 8 ko * 6 000 IOPS == ~46,88 Mo/sec
- 4 ko * 6 000 IOPS == ~23,44 Mo/sec

## Le volume doit-il être préchauffé pour obtenir le débit prévu ?
{: faq}

Aucun préchauffage n'est nécessaire. Le débit indiqué peut être observé immédiatement après la mise à disposition du volume.

## Est-il possible d'atteindre un débit plus élevé en utilisant une connexion Ethernet plus rapide ?
{: faq}

Les limites de débit sont configurées par numéro d'unité logique. Par conséquent, une connexion Ethernet plus rapide ne permet pas d'augmenter la limite définie. Toutefois, avec une connexion Ethernet plus lente, votre bande passante peut éventuellement créer un goulot d'étranglement.

## Les pare-feu et groupes de sécurité ont-ils un impact sur les performances ?
{: #isolatedstoragetraffic}
{: faq}

Il est recommandé d'exécuter le trafic de stockage sur un réseau local virtuel qui ignore le pare-feu. L'exécution du trafic de stockage via des pare-feu logiciels augmente le temps d'attente et a un impact négatif sur les performances de stockage.

## Quel temps d'attente lié aux performances peut-on attendre du stockage {{site.data.keyword.blockstorageshort}} ?   
{: faq}

Le temps d'attente cible dans le stockage est < 1 ms. Le stockage est connecté à des instances de traitement sur un réseau partagé ; le temps d'attente exact des performances dépend donc du trafic réseau sur une période donnée.

## Pourquoi {{site.data.keyword.blockstorageshort}} avec un niveau Endurance de 10 IOPS peut-il être commandé dans certains centres de données et pas dans d'autres ?
{: faq}

Le niveau 10 IOPS/Go de {{site.data.keyword.blockstorageshort}} de type Endurance est disponible dans la plupart des [centre de données](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC).

## Comment savoir quels volumes {{site.data.keyword.blockstorageshort}} sont chiffrés ?
{: faq}

Lorsque vous consultez votre liste de services {{site.data.keyword.blockstorageshort}} sur la [console {{site.data.keyword.cloud}}](https://{DomainName}/classic/storage){: external}, une icône de verrouillage s'affiche à droite du nom du volume pour les numéros d'unité logique qui sont chiffrés.

## Comment savoir si l'ont met à disposition un stockage {{site.data.keyword.blockstorageshort}} dans un centre de données mis à niveau ?
{: faq}

Lorsque vous commandez {{site.data.keyword.blockstorageshort}}, tous les centres de données mis à niveau sont signalés par un astérisque (`*`) dans le formulaire de commande, ainsi que par un message indiquant que vous êtes sur le point de mettre à disposition un stockage avec chiffrement. Une fois le stockage mis à disposition, une icône apparaît dans la liste de stockage pour indiquer que le stockage est chiffré. Tous les volumes et numéros d'unité logique chiffrés sont mis à disposition uniquement dans des centres de données mis à niveau. Vous trouverez la liste complète des centres de données mis à niveau et des fonctions disponibles [ici](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC).

## Si nous possédons un stockage {{site.data.keyword.blockstorageshort}} non chiffré dans un centre de données qui a été récemment mis à jour, pouvons-nous chiffrer ce stockage {{site.data.keyword.blockstorageshort}} ?
{: faq}

Un service {{site.data.keyword.blockstorageshort}} qui est mis à disposition avant la mise à niveau du centre de données ne peut pas être chiffré.
Un nouveau stockage {{site.data.keyword.blockstorageshort}} mis à disposition dans des centres de données mis à niveau est automatiquement chiffré. Vous n'avez pas à choisir de paramètre de chiffrement, car la procédure est automatique.
Les données sur du stockage non chiffré dans un centre de données mis à niveau peuvent être chiffrées via la création d'un numéro d'unité logique (LUN), puis via la copie des données sur le nouveau LUN chiffré avec migration basée sur l'hôte. Pour plus d'informations, voir [Mise à niveau d'un service {{site.data.keyword.blockstorageshort}} vers un service {{site.data.keyword.blockstorageshort}} amélioré](/docs/infrastructure/BlockStorage?topic=BlockStorage-migratestorage#migratestorage).

## {{site.data.keyword.blockstorageshort}} prend-il en charge la réservation persistante SCSI-3 pour implémenter la protection d'E-S pour Db2 pureScale ?
{: faq}

Oui, {{site.data.keyword.blockstorageshort}} prend en charge les réservations persistantes SCSI-2 et SCSI-3.

## Qu'advient-il des données lors de la suppression des numéros d'unité logique {{site.data.keyword.blockstorageshort}} ?
{: faq}

{{site.data.keyword.blockstoragefull}} propose aux clients les volumes de blocs sur un espace de stockage physique qui est nettoyé avant toute réutilisation. Les clients ayant des exigences particulières de conformité (telles que celles définies dans le document 800-88 du National Institute of Standards and Technology intitulé Guidelines for Media Sanitization) doivent effectuer la procédure d'expurgation des données avant de supprimer leur stockage.

## Que deviennent les unités qui sont déclassées du centre de données cloud ?
{: faq}

Lorsque des unités sont déclassées, IBM les détruit avant de les supprimer. Elles sont ainsi inutilisables. Toutes les données qui étaient écrites sur ces unités deviennent inaccessibles.
