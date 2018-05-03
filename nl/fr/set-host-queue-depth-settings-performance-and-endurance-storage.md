---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Recommandation relative aux paramètres de profondeur de la file d'attente hôte

{{site.data.keyword.BluSoftlayer_full}} recommande un nombre de lignes maximal pour la file d'attente d'entrée/sortie (E-S) de l'hôte et de l'application pour chaque niveau de performance. Le paramètre de l'hôte n'affecte pas le temps d'attente du disque et du contrôleur, uniquement celui de l'hôte et de l'application.

Une profondeur de file d'attente supérieure au nombre de lignes recommandé peut augmenter le temps d'attente d'E-S de l'hôte, tandis qu'une profondeur inférieure peut réduire les performances d'E-S de l'hôte. Chaque application étant différente, un ajustement et une phase d'observation sont nécessaires pour optimiser les performances de stockage.

La profondeur de la file d'attente hôte est généralement ajustée dans le pilote d'adaptateur de bus hôte ou l'hyperviseur, et parfois dans l'application. Les valeurs par défaut standard telles que 32 ou 64 peuvent provoquer un temps d'attente excessif pour l'hôte ou l'application.

Si un hôte ou un hyperviseur utilise plusieurs niveaux de performance, servez-vous de la profondeur de la file d'attente pour le niveau le plus rapide et respectez le temps d'attente sur le niveau de performance le plus lent. Si le temps d'attente sur le niveau le plus bas est inacceptable, ajustez le nombre de lignes de la file d'attente jusqu'à atteindre un équilibre entre le temps d'attente et les performances pour tous les niveaux.

<table align="center">
	<tbody>
		<tr>
			<td><strong>Niveau de performance</strong></td>
			<td style="text-align: center; vertical-align: middle;">0,25 E-S/s par Go</td>
			<td style="text-align: center; vertical-align: middle;">2 E-S/s par Go</td>
			<td style="text-align: center; vertical-align: middle;">4 E-S/s par Go</td>
		</tr>
		<tr>
			<td><strong>Profondeur maximale de la file d'attente hôte</strong></td>
			<td style="text-align: center; vertical-align: middle;">8</td>
			<td style="text-align: center; vertical-align: middle;">24</td>
			<td style="text-align: center; vertical-align: middle;">56</td>
		</tr>
	</tbody>
</table>
