---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, new features, new locations, Block Storage, mount point changes, select data centers, ISCSI,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Nouveaux emplacements et fonctions
{: #news}

{{site.data.keyword.BluSoftlayer_full}} présente une nouvelle version d'{{site.data.keyword.blockstoragefull}} !

Le nouveau stockage est disponible dans certains centres de données et sécurisé par mémoire flash à des niveaux d'E-S/s supérieurs avec chiffrement des données au repos au niveau du disque. La totalité du stockage mis à disposition dans les centres de données mis à niveau est automatiquement créée avec la nouvelle version.

Le point de montage NFS des nouveaux volumes est différent du point de montage des volumes non chiffrés. Pour plus d'informations, voir la section [Nouveau point de montage des volumes {{site.data.keyword.blockstorageshort}} chiffrés](#mountpoints).
{:important}

## Nouveaux emplacements
{: #new-locations}

La nouvelle fonction {{site.data.keyword.blockstorageshort}} est disponible dans les régions et centres de données ci-dessous :
<table role="presentation">
  <tr>
    <td><strong>EU 2</strong></td>
    <td><strong>UE</strong></td>
    <td><strong>Australie</strong></td>
    <td><strong>Canada</strong></td>
    <td><strong>Amérique latine</strong></td>
    <td><strong>Asie Pacifique</strong></td>
  </tr>
  <tr>
    <td>DAL09<br />
	DAL10<br />
	DAL12<br />
	DAL13<br />
	SJC03<br />
        SJC04<br />
	WDC04<br />
	WDC06<br />
	WDC07<br />
	<br /><br /><br />
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
	MIL01<br />
	OSLO1<br />
	PAR01<br />
    </td>
    <td>MEL01<br />
        SYD01<br />
        SYD04<br />
        SYD05<br />
        <br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MON01<br />
        TOR01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MEX01<br />
        SAO01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>CHE01<br />
        HKG02<br />
	SEO01<br />
	SNG01<br />
        TOK02<br />
	TOK04<br />
	TOK05<br />
	<br /><br /><br /><br /><br />
    </td>
  </tr>
</table>

*Le tableau 1 répertorie la disponibilité de nos centres de données. Chaque région correspond à une colonne. Certaines villes, comme Dallas, San José, Washington DC, Amsterdam, Francfort, Londres et Sydney possèdent plusieurs centres de données.*

## Nouvelles fonctions et fonctionnalités
{: #features}

- **[Chiffrement géré par le fournisseur pour les données au repos](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption)**.
  La totalité du service {{site.data.keyword.blockstorageshort}} est automatiquement mise à disposition sous forme chiffrée sans frais supplémentaires.
- **Option de niveau 10 E-S/s par Go**.
  Un nouveau niveau est disponible avec le type {{site.data.keyword.blockstorageshort}} Endurance pour prendre en charge les charges de travail les plus exigeantes.
- **Stockage entièrement sécurisé par mémoire flash.**
  La totalité du service {{site.data.keyword.blockstorageshort}} mis à disposition avec le type Endurance ou Performance à un niveau de 2 E-S/s par Go ou supérieur est sauvegardée à l'aide d'un stockage entièrement sécurisé par mémoire flash.
- Prise en charge de l'**image instantanée et de la réplication** avec {{site.data.keyword.blockstorageshort}}
- L'option de **facturation à l'heure** est disponible pour un stockage dont l'utilisation prévue est inférieure à un mois.
- **Jusqu'à 48 000 E-S/s** pour le service {{site.data.keyword.blockstorageshort}} mis à disposition avec Performance.
- **Les débits d'E-S/s sont ajustables** pour améliorer les performances en cas de modifications de charge saisonnières. Pour en savoir plus sur cette fonction, cliquez [ici](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS).
- Créez un clone de vos données grâce à la **[fonction de duplication de volume {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)**.
- **Le stockage est extensible** par incréments en Go jusqu'à 12 To, sans avoir besoin de créer un doublon ni de déplacer manuellement les données vers un volume plus grand. Pour en savoir plus sur cette fonction, cliquez [ici](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity).

## Nouveau point de montage des volumes de stockage chiffrés
{: #mountpoints}

Tous les volumes de stockage amélioré qui sont mis à disposition dans ces centres de données possèdent un point de montage différent des volumes non chiffrés. Vérifiez les informations de point de montage sur la page **Volume Details** du portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} pour vous assurer que vous utilisez le point de montage approprié. Vous pouvez également obtenir les informations relatives au point de montage correct via un appel d'API : `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Pour pouvoir accéder à toutes les nouvelles fonctions, sélectionnez `Storage-as-a-Service Package 759` lorsque vous passez votre commande via l'API. Pour plus d'informations sur les commande {{site.data.keyword.blockstorageshort}} via l'API, voir [order_block_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.order_block_volume){: external}.
{:important}

Revenez ici pour savoir si d'autres centres de données ont été mis à niveau et si de nouvelles fonctions et capacités ont été ajoutées pour {{site.data.keyword.blockstorageshort}}.
{:tip}
