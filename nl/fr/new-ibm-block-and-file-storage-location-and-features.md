---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-29"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Nouveaux emplacements et fonctions de {{site.data.keyword.blockstorageshort}}

{{site.data.keyword.BluSoftlayer_full}} présente une nouvelle version d'{{site.data.keyword.blockstoragefull}} !

Le nouveau stockage est disponible dans certains centres de données et sécurisé par mémoire flash à des niveaux d'E-S/s supérieurs avec chiffrement des données au repos au niveau du disque. La totalité du stockage mis à disposition dans les centres de données sélectionnés est automatiquement créée avec la nouvelle version. 

**Remarque :** Le point de montage NFS pour les nouveaux volumes a changé. Consultez la section **Nouveau point de montage pour les volumes {{site.data.keyword.filestorage_short}} chiffrés** ci-dessous pour plus de détails.

La nouvelle version de {{site.data.keyword.blockstorageshort}} est actuellement disponible dans les régions/centres de données suivants, mais cette liste va bientôt être enrichie !
<table style="width:100%;">
 <caption>Le Tableau 1 répertorie la disponibilité de nos centres de données. Chaque région dispose de sa propre colonne. Certaines villes, comme Dallas, San José, Washington, Amsterdam, Francfort, Londres et Sydney possèdent plusieurs centres de données.</caption>
	 <tr>
	   <td><strong>Etats-Unis 2</strong></td>
	   <td><strong>UE</strong></td>
	   <td><strong>Australie</strong></td>
	   <td><strong>Canada</strong></td>
	   <td><strong>Amérique latine</strong></td>
	   <td><strong>Asie-Pacifique</strong></td>
	</tr>
	<tr>
	   <td><p>SJC03<br />
		SJC04<br />
		WDC04<br />
		WDC06<br />
		WDC07<br />
		DAL09<br />
		DAL10<br />
		DAL12<br />
		DAL13<br /><br /><br /></p>
	   </td>
	   <td><p>LON02<br />
		LON04<br />
		LON06<br />
		FRA02<br />
		FRA04<br />
		FRA05<br />
		AMS01<br />
		AMS03<br />
		OSLO1<br />
		PAR01<br />
		MIL01</p>
            </td>
	    <td><p>SYD01<br />
		SYD04<br />
		MEL01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
	    </td>
	    <td><p>TOR01<br />
		MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
	    </td>
	    <td><p>MEX01<br />SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
	    </td>
	    <td><p>TOK02<br />
		HKG02<br />
		SEO01<br />
		SNG01<br />
		CHE01<br /><br /><br /><br /><br /><br /><br /></p>
	   </td>
	</tr>
</table>


Le nouveau stockage comporte les fonctionnalités suivantes :

- **[Chiffrement géré par le fournisseur pour les données au repos](block-file-storage-encryption-rest.html)**. La totalité du service {{site.data.keyword.blockstorageshort}} est automatiquement mise à disposition sous forme chiffrée sans frais supplémentaires.
- **Option de niveau 10 E-S/s par Go**.
  Un nouveau niveau est disponible avec le type {{site.data.keyword.blockstorageshort}} Endurance pour prendre en charge les charges de travail les plus exigeantes.
- **Stockage entièrement sécurisé par mémoire flash.**
  La totalité du service {{site.data.keyword.blockstorageshort}} mis à disposition avec le type Endurance ou Performance à un niveau de 2 E-S/s par Go ou supérieur est sauvegardée à l'aide d'un stockage entièrement sécurisé par mémoire flash.
- Prise en charge de l'**image instantanée et de la réplication** avec {{site.data.keyword.blockstorageshort}}
- L'option de **facturation à l'heure** est disponible pour un stockage dont l'utilisation prévue est inférieure à un mois.
- **Jusqu'à 48 000 E-S/s** pour le service {{site.data.keyword.blockstorageshort}} mis à disposition avec Performance.
- **Les débits d'E-S/s sont ajustables** pour améliorer les performances en cas de modifications de charge saisonnières. Découvrez plus de détails sur cette fonctionnalité [ici](adjustable-iops.html).
- Créez un clone de vos données avec la fonction de duplication de volume de **[{{site.data.keyword.blockstorageshort}}](how-to-create-duplicate-volume.html)**.
- **Le stockage est extensible** par incréments en Go jusqu'à 12 To, sans avoir besoin de créer un doublon ni de déplacer manuellement les données vers un volume plus grand. Découvrez plus de détails sur cette fonctionnalité [ici](expandable_block_storage.html).

## Nouveau point de montage pour les volumes de stockage chiffrés

Tous les volumes de stockage amélioré qui sont mis à disposition dans ces centres de données possèdent un point de montage différent des volumes non chiffrés. Pour vous assurer que vous utilisez le point de montage correct pour vos volumes de stockage, vous pouvez afficher les informations relatives au point de montage sur la page **Détails du volume** dans l'interface utilisateur. Vous pouvez également accéder au point de montage correct via un appel API : `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Vérifiez à nouveau ici la date à laquelle des centres de données supplémentaires ont été mis à niveau et recherchez les fonctionnalités qui ont été ajoutées récemment pour {{site.data.keyword.blockstorageshort}}.
