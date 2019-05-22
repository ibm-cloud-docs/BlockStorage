---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, inaccessible Primary volume, duplicate of a replica volume, Disaster Recovery, volume duplication, replication, failover, failback

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Reprise après incident - Basculement avec un volume principal inaccessible
{: #dr-inaccessible}

En cas de défaillance catastrophique ou d'un incident entraînant une indisponibilité sur le site principal, les clients peuvent effectuer les actions suivantes pour accéder rapidement à leurs données sur le site secondaire.

## Basculement avec un doublon d'un volume de réplique sur le site secondaire

1. Connectez-vous à la [console IBM Cloud](https://{DomainName}/){: external}, puis cliquez sur l'icône **Menu** dans l'angle supérieur gauche. Sélectionnez **Infrastructure classique**.


   Vous pouvez également vous connecter au portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.
2. Cliquez sur **Stockage** > **{{site.data.keyword.blockstorageshort}}**.
3. Cliquez sur la réplique du numéro d'unité logique dans la liste pour afficher la page **Détails** correspondante.
4. Sur la page **Détails**, faites défiler l'écran et sélectionnez un instantané existant, puis cliquez sur **Actions** > **Dupliquer**.
5. Apportez les mises à jour nécessaires à la capacité (pour augmenter la taille) ou aux opérations d'entrée-sortie par seconde pour le nouveau volume.
6. Mettez à jour l'espace d'instantané pour le nouveau volume, si besoin.
7. Cliquez sur **Continuer** pour passer votre commande du doublon.

Dès que le volume est créé, il peut être associé à un hôte et effectuer des opérations d'écriture/de lecture. Pendant que les données sont copiées depuis le volume d'origine vers le la page des détails indique que la duplication est en cours. Une fois le processus de duplication terminé, le nouveau volume devient indépendant du volume d'origine ; il peut être géré avec des instantanés et des réplications comme un volume normal.

## Reprise par restauration sur le site principal d'origine

Si vous souhaitez renvoyer la production au site principal d'origine, vous devez effectuer les étapes ci-après.

1. Connectez-vous à la [console IBM Cloud](https://
{DomainName}/){: external}, puis cliquez sur l'icône **Menu** dans l'angle supérieur gauche. Sélectionnez **Infrastructure classique**.


   Vous pouvez également vous connecter au portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.
2. Cliquez sur **Stockage** > **{{site.data.keyword.blockstorageshort}}**.
3. Cliquez sur le nom du numéro d'unité logique et créez un planning d'instantané (le cas échéant).

   Pour plus d'informations sur les plannings d'instantané, voir [Gestion des instantanés](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingSnapshots#addingschedule).
   {:tip}
4. Cliquez sur **Réplique**, puis sur **Acheter une réplication**.
5. Sélectionnez le planning d'instantané existant que vous souhaitez que votre réplication suive. La liste contient tous les plannings d'instantané actifs.
6. Cliquez sur **Emplacement** et sélectionnez le centre de données qui était votre site de production d'origine.
7. Cliquez sur **Continuer**.
8. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**.

Une fois la réplication terminée, vous devez créer un volume dupliqué de la nouvelle réplique.
{:important}

1. Revenez dans **Stockage** > **{{site.data.keyword.blockstorageshort}}**.
2. Cliquez sur la réplique du numéro d'unité logique dans la liste pour afficher la page **Détails** correspondante.
3. Sur la page **Détails**, faites défiler l'écran et sélectionnez un instantané existant, puis cliquez sur **Actions** > **Dupliquer**.
4. Apportez les mises à jour nécessaires à la capacité (pour augmenter la taille) ou aux opérations d'entrée-sortie par seconde pour le nouveau volume.
5. Mettez à jour l'espace d'instantané pour le nouveau volume, si besoin.
6. Cliquez sur **Continuer** pour passer votre commande du doublon.

Lorsque le processus de duplication est terminé, vous pouvez annuler la réplication et les volumes qui étaient utilisés pour que les données soient de nouveau placées sur le site principal d'origine. Le doublon devient le site de stockage principal, et la réplication vers le site secondaire d'origine peut être réétablie.
