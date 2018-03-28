---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Sécurisation des données - Chiffrement au repos géré par le fournisseur

## Chiffrement au repos de {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_full_notm}} 

{{site.data.keyword.BluSoftlayer_full}} prend très au sérieux la question de la sécurité et comprend l'importance du chiffrement des données pour assurer leur sécurité. Avec le chiffrement géré par le fournisseur, {{site.data.keyword.blockstoragefull}} et {{site.data.keyword.filestorage_full}} mis à disposition avec Endurance ou Performance sont chiffrés par défaut sans coût supplémentaire et sans aucun impact sur les performances. 

La fonction de chiffrement au repos géré par le fournisseur utilise les protocoles conformes aux normes de l'industrie suivants : 

* Chiffrement conforme à la norme de l'industrie AES-256
* Les clés sont gérées en interne avec le protocole conforme à la norme de l'industrie KMIP (Key Management Improbability Protocol) 
* Le stockage est conforme à la norme FIPS (Federal Information Processing Standard) Publication 140-2 validée pour la loi Federal Information Security Management Act (FISMA), la loi Health Insurance Portability and Accountability Act (HIPAA), la norme PCI (Payment Card Industry), Bâle II, la loi California Security Breach Information Act (SB 1386) et la directive européenne relative à la protection des données 95/46/EC

## Chiffrement au repos pour les instantanés ou le stockage répliqué  

Tous les instantanés et répliques de données chiffrées {{site.data.keyword.blockstorageshort}} sont également chiffrés par défaut. Cette fonction ne peut pas être désactivée par volume.

## Mise à disposition de stockage avec chiffrement

La fonction de chiffrement au repos géré par le fournisseur est uniquement disponible pour le service {{site.data.keyword.blockstorageshort}} qui est mis à disposition dans certains centres de données, sachant que de nouveaux centres de données sont régulièrement ajoutés à cette liste. L'ensemble du stockage mis à disposition dans ces centres de données est automatiquement fourni avec le chiffrement des données au repos. Cliquez [ici](new-ibm-block-and-file-storage-location-and-features.html) pour afficher la liste actuelle des centres de données dans lesquels le chiffrement des données au repos {{site.data.keyword.blockstorageshort}} est disponible. 

Lorsque vous commandez votre service {{site.data.keyword.blockstorageshort}}, sélectionnez un centre de données signalé par un astérisque (*) et un message indiquant que le chiffrement est disponible. Vous verrez s'afficher une icône de verrouillage à droite de la zone LUN/Nom de volume indiquant qu'elle est chiffrée. Voir la Figure 1.

![L'icône de verrouillage indique que le numéro d'unité logique est chiffré](/images/encryptedstorage.png)
<caption>Figure 1. Exemple d'icône de verrouillage indiquant que le numéro d'unité logique est chiffré.</caption>



**Notez** que le stockage non chiffré mis à disposition avant la mise à niveau d'un centre de données **ne sera pas** chiffré automatiquement. Si vous disposez d'un stockage non chiffré dans un centre de données mis à niveau, vous devrez créer un nouveau numéro d'unité logique ou volume et effectuer une migration de données. Les articles suivants peuvent vous fournir des conseils. 

* [Migration de {{site.data.keyword.blockstorageshort}} dans des centres de données mis à niveau](migrate-block-storage-encrypted-block-storage.html)
