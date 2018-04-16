---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Nouveaux emplacements et fonctions de {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}}

{{site.data.keyword.BluSoftlayer_full}} présente une nouvelle version d'{{site.data.keyword.blockstoragefull}} !  

Le nouveau stockage est disponible dans certains centres de données et sécurisé par mémoire flash à des niveaux d'E-S/s supérieurs avec chiffrement des données au repos au niveau du disque. La totalité du stockage mis à disposition dans les centres de données sélectionnés sera automatiquement mise à disposition avec la nouvelle version de {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_full}}. 

**Remarque :** Le point de montage NFS pour les nouveaux volumes a changé. Consultez la section **Nouveau point de montage pour les volumes {{site.data.keyword.filestorage_short}} chiffrés** ci-dessous pour plus de détails. 

Les nouvelles versions de {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}} sont actuellement disponibles dans les régions/centres de données suivants, mais cette liste va bientôt être enrichie ! 
<table style="width:100%;">
	<caption>Disponibilité des centres de données</caption>
	<tbody>
		<tr>
			<td><strong>Etats-Unis 2</strong></td>
			<td><strong>UE</strong></td>
			<td><strong>Australie</strong></td>
			<td><strong>Canada</strong></td>
			<td><strong>Amérique latine</strong></td>
			<td><strong>Asie-Pacifique</strong></td>
		</tr>
		<tr>
			<td>
				<p>SJC03<br />
				SJC04<br />
				WDC04<br />
				WDC06<br />
				WDC07<br />
				DAL09<br />
				DAL10<br />
				DAL12<br />
				DAL13</p>
			</td>
			<td>
				<p>LON02<br />
				LON04<br />
				LON06<br />
				FRA02<br />
				AMS01<br />
				AMS03<br />
				OSLO1<br />
				PAR01<br />
				MIL01<br /></p>
			</td>
			<td>
				<p>SYD01<br />
				SYD04<br />
				MEL01<br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>MEX01<br />
				SAO01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>TOK02<br />
				HKG02<br />
			        SEO01<br />
				SNG01<br />
				CHE01<br /><br /><br /><br /><br /></p>
			</td>
			</tr>
	</tbody>
</table>


Le nouveau stockage comporte les fonctionnalités suivantes : 

- [Chiffrement géré par le fournisseur pour les données au repos](block-file-storage-encryption-rest.html). Tous les services {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}} seront automatiquement mis à disposition sous forme chiffrée sans frais supplémentaires. 
- Option de niveau 10 E-S/s par Go. Un nouveau niveau a été ajouté au type {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}} Endurance pour prendre en charge les charges de travail les plus exigeantes. 
- Stockage entièrement sécurisé par mémoire flash. Les services {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}} sont mis à disposition avec Endurance ou Performance à un niveau de 2 E-S/s par Go ou supérieur avec stockage entièrement sécurisé par mémoire flash. 
- Prise en charge des instantanés et de la réplication avec des services {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}} mis à disposition avec Endurance ou Performance. 
- Option de facturation à l'heure ajoutée pour un stockage dont l'utilisation prévue est inférieure à un mois.  
- Jusqu'à 48 000 E-S/s pour les services {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}} mis à disposition avec Performance. 
- Les débits d'E-S/s sont ajustables pour améliorer les performances en cas de modifications de charge saisonnières. Découvrez plus de détails sur cette fonctionnalité [ici](adjustable-iops.html). 
- Créez un clone de vos données avec la fonction de duplication de volume de [{{site.data.keyword.blockstorageshort}}](how-to-create-duplicate-volume.html). 
- Le stockage est extensible instantanément par incréments en Go jusqu'à 12 To, sans avoir besoin de créer un doublon ni de migrer manuellement les données vers un volume plus grand. Découvrez plus de détails sur cette fonctionnalité [ici](expandable_block_storage.html). 

## Nouveau point de montage pour les volumes de stockage chiffrés

Tous les volumes de stockage chiffrés mis à disposition dans ces centres de données possèdent un point de montage différent des volumes non chiffrés. Pour vous assurer que vous utilisez le point de montage correct pour vos volumes de stockage chiffrés comme non chiffrés, vous pouvez afficher les informations relatives au point de montage sur la page Détails du volume dans l'interface utilisateur et y accéder par l'appel d'API : `SoftLayer_Network_Storage::getNetworkMountAddress()`. 

Vérifiez à nouveau ici la date à laquelle des centres de données supplémentaires ont été mis à niveau et recherchez les fonctionnalités qui ont été ajoutées récemment pour {{site.data.keyword.blockstorageshort}}. 
