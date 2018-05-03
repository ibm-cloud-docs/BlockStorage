---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Initiation à {{site.data.keyword.blockstorageshort}}

La section contenant une brève description doit inclure de une à deux phrases décrivant la raison pour laquelle un administrateur système ou un ingénieur développement et opérations souhaite utiliser ce service ou cette offre d'infrastructure.
Mentionnez rapidement l'objectif d'apprentissage de l'utilisateur et incluez les mots-clés d'optimisation pour les moteurs de recherche suivants dans le titre et/ou la brève description : IBM Cloud, ServiceName. Assurez-vous d'utiliser un style conversationnel. Pour plus de détails, consultez les conseils relatifs au style conversationnel dans Carbon Design System à l'adresse http://www.carbondesignsystem.com/guidelines/content/general.

Exemple :
Lorsque les demandes de ressources atteignent un pic, vous avez besoin de services d'infrastructure cloud qui puissent évoluer de façon contrôlée de manière à répondre immédiatement à ces nouvelles exigences. Les serveurs virtuels {{site.data.keyword.Bluemix}} peuvent être déployés en quelques minutes à partir des images de serveur virtuel de votre choix, dans la région géographique pertinente pour vos charges de travail. Dès que vos charges de travail diminuent, ces serveurs virtuels peuvent être suspendus ou mis hors tension afin que votre environnement de cloud s'adapte parfaitement à vos besoins d'infrastructure.

La section relative aux tâches inclut les étapes requises pour le fonctionnement d'un périphérique, d'un stockage ou d'un réseau.
- Grâce à des informations techniques basées sur les tâches, réduisez le style conversationnel en faveur d'instructions directes et succinctes.
- INCLUEZ les étapes du scénario de base les plus fréquentes pour utiliser le service d'infrastructure.
- N'INCLUEZ PAS les étapes d'ajout du service provenant du catalogue Bluemix ; nous supposons que l'utilisateur a déjà effectué la procédure de l'interface utilisateur pour ajouter le service.
- INCLUEZ des fragments de code dans tous les langages qui peuvent être copiés, ainsi que les informations de service VCAP. Vous trouverez des informations sur le service VCAP ici : https://console.ng.bluemix.net/docs/cli/vcapsvc.html
- Pour les tâches supplémentaires telles que la configuration, la gestion, etc., ajoutez une section relative à la tâche (## Gerund_task_title) au-dessous de la section relative à la tâche ou de la section "About", le cas échéant. Utilisez un titre de tâche tel que "Configuration de x", "Administration de y", "Gestion de z". -->

## Configuration préalable requise
Pour qu'un administrateur puisse mettre à disposition ou gérer ses offres d'infrastructure, il doit disposer d'un compte {{site.data.keyword.Bluemix}} mis à niveau. Pour plus d'informations, voir [Mise à niveau et unification des comptes de facturation {{site.data.keyword.Bluemix_notm}} et SoftLayer](../docs/admin/softlayerlink.html).

## Titre et description orientés tâche
Pour rendre ce service d'infrastructure rapidement fonctionnel, procédez comme suit : -OU-
Exécutez ces étapes pour démarrer le service Block Storage :

<!-- Use ordered list markup for the step section. For code examples:
- use three backticks ahead of and after the example (```)
- For copyable code snippet, multi-line, include {: codeblock} following the last set of backticks. A copy button will display in framework in output.
- For copyable command, single line, include {: pre} following the last set of backticks. When displayed, it will show "$" at the beginning of the command example and a copy button, but the copy button will include just the command example.
- For non-copyable output snippet, include {: screen} following the last set of backticks.
 -->

1. Etape 1 pour configurer le service.
2. Etape 2 pour configurer le service.

	```
	Copyable example for this step.
	This example might be multiline code
	to copy into a file.
	When displayed in the doc framework,
	it will have a copy button on the right.
	The user can click to copy the example
	so they can paste it into their code editor.
	```
	{: codeblock}

3. Etape 3. Dans cette étape, nous avons un exemple de commande sur une seule ligne. Lorsqu'elle est affichée par l'infrastructure de documentation, un symbole $ apparaît en début de ligne, ainsi qu'un bouton de copie sur la droite. Ce dernier copiera la commande mais pas le symbole $.

	```
	my command -and -options
	```
	{: pre}

4. Etape 4
	```
	This is a bunch of output from
		a command or program I ran
			and it can run lots of lines
			and the doc framework will show it as
			output with no copy button.
	```
	{: screen}

## Etapes suivantes

