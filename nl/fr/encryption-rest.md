---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords: Block Storage Encryption, industry standard protocols, IBM Block Storage, LUN, provider-managed encryption

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Chiffrement au repos géré par le fournisseur
{: #encryption}

## Chiffrement au repos de {{site.data.keyword.blockstorageshort}}

{{site.data.keyword.cloud}} prend très au sérieux la question de la sécurité et comprend l'importance du chiffrement des données pour assurer leur sécurité. Avec le chiffrement géré par le fournisseur, {{site.data.keyword.blockstoragefull}}, qui est mis à disposition avec Endurance ou Performance, est chiffré par défaut sans coût supplémentaire et sans aucun impact sur les performances.

La fonction de chiffrement au repos géré par le fournisseur utilise les protocoles conformes aux normes de l'industrie suivants :

* Chiffrement AES-256 conforme aux normes de l'industrie
* Gestion en interne des clés via le protocole conforme aux normes de l'industrie KMIP (Key Management Interoperability Protocol)
* Le stockage est validé pour la norme FIPS (Federal Information Processing Standard) Publication 140-2, la loi Federal Information Security Management Act (FISMA) et la loi Health Insurance Portability and Accountability Act (HIPAA). Il est également validé pour la norme PCI (Payment Card Industry), Bâle II, la loi California Security Breach Information Act (SB 1386) et la directive européenne relative à la protection des données 95/46/EC.

## Fourniture du chiffrement au repos pour les instantanés ou le stockage répliqué  

Tous les instantanés et répliques de données chiffrées {{site.data.keyword.blockstorageshort}} sont également chiffrés par défaut. Cette fonction ne peut pas être désactivée par volume.

## Mise à disposition du stockage avec chiffrement

La fonction de chiffrement au repos géré par le fournisseur est disponible pour le service {{site.data.keyword.blockstorageshort}} mis à disposition dans [la plupart des centre de données](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC). La totalité du stockage qui est commandé dans ces centres de données est automatiquement doté du chiffrement.

Lorsque vous commandez {{site.data.keyword.blockstorageshort}}, sélectionnez un centre de données signalé par un astérisque (`*`). Une icône en forme de verrou figure à droite de la zone Numéro d'unité logique/nom de volume pour indiquer que le volume est chiffré.

![L'icône de verrouillage indique que le numéro d'unité logique est chiffré](/images/encryptedstorage.png)
<caption>Figure 1. Exemple d'icône en forme de verrou indiquant que le numéro d'unité logique est chiffré.</caption>



Un stockage non chiffré qui a été mis à disposition avant la mise à niveau du centre de données **n'est pas** automatiquement chiffré. Si vous disposez d'un stockage non chiffré dans un centre de données mis à niveau et que vous souhaitez le chiffrer, vous devrez créer un nouveau numéro volume et faire migrer vos données. Pour plus d'informations, voir [{{site.data.keyword.blockstorageshort}} Migration dans des centres de données mis à niveau](/docs/infrastructure/BlockStorage?topic=BlockStorage-migratestorage).
{:important}
