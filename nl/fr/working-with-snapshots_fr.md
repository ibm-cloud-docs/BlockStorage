---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Gestion des instantanés

## Comment puis-je créer un planning d'instantané ? 

Les plannings d'instantané vous permettent de choisir la fréquence et le moment de création d'une référence ponctuelle de votre volume de stockage. Vous pouvez avoir 50 instantanés par volume de stockage au maximum. Les plannings sont gérés via l'onglet **Stockage**, **{{site.data.keyword.blockstorageshort}}** du portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}. 

Avant de configurer votre planning initial, vous devez d'abord acheter de l'espace d'instantané si vous ne l'avez pas fait lors de la mise à disposition initiale du volume de stockage. 

### Comment puis-je ajouter un planning d'instantané ? 

Les plannings d'instantané peuvent être configurés pour des intervalles horaires, quotidiens et hebdomadaires, chacun disposant d'un cycle de conservation distinct. Il existe un nombre maximal de 50 instantanés planifiés, qui peuvent être un mélange de plannings horaires, quotidiens et hebdomadaires, et instantanés manuels par volume de stockage. 

1. Cliquez sur votre volume de stockage, cliquez sur la zone déroulante **Actions**, puis sur **Planifier un échantillonnage**. 
2. La boîte de dialogue du nouveau planning d'instantané contient trois fréquences d'instantané différentes, parmi lesquelles vous devez effectuer un choix. Utilisez n'importe quelle combinaison des trois pour établir un planning d'instantané complet. 
   - Horaire
      - Indiquez la minute de chaque heure à laquelle un instantané doit être pris ; la valeur par défaut est la minute en cours. 
      - Spécifiez le nombre d'instantanés horaires à conserver avant d'effacer le plus ancien. 
   - Quotidien
      - Indiquez l'heure et la minute auxquelles un instantané doit être pris ; la valeur par défaut est l'heure et la minute en cours. 
      - Sélectionnez le nombre d'instantanés horaires à conserver avant d'effacer le plus ancien. 
   - Hebdomadaire
      - Indiquez le jour de la semaine, l'heure et la minute auxquels un instantané doit être pris ; la valeur par défaut est le jour, l'heure et la minute en cours. 
      - Sélectionnez le nombre d'instantanés hebdomadaires à conserver avant d'effacer le plus ancien. 
3. Cliquez sur **Sauvegarder** et créez un autre planning avec une fréquence différente. Notez que vous recevrez un message d'avertissement et ne pourrez pas effectuer de sauvegarde si le nombre total d'instantanés planifiés dépasse 50. 

La liste des instantanés s'affiche lors de leur prise dans la section Instantanés de la page Détails. 

## Comment puis-je prendre un instantané manuel ? 

Vous pouvez prendre des instantanés manuels à différents moments lors de la mise à niveau ou de la maintenance d'une application. Vous pouvez également prendre des instantanés sur plusieurs machines qui ont été temporairement désactivées au niveau de l'application. 

Il existe un nombre maximal de 50 instantanés manuels par volume de stockage. 

1. Cliquez sur votre volume de stockage. 
2. Cliquez sur la zone déroulante Actions. 
3. Cliquez sur **Prendre un instantané manuel**.
L'instantané sera pris et s'affichera dans la section Instantanés de la page Détails. Son planning sera Manuel. 

## Comment puis-je voir la liste des instantanés avec l'espace consommé et les fonctions de gestion ? 

Vous pouvez afficher la liste des instantanés conservés ainsi que l'espace consommé sur la page **Détails** (Stockage, {{site.data.keyword.blockstorageshort}}). Les fonctions de gestion (édition des plannings et ajout d'espace supplémentaire) sont exécutées sur la page Détails à l'aide du menu déroulant **Actions** ou des liens figurant dans les diverses sections de la page. 

## Comment puis-je afficher la liste des instantanés conservés ? 

Les instantanés conservés se fondent sur le nombre que vous avez saisi dans la zone Conserver les derniers lors de la configuration de vos plannings. Vous pouvez afficher les instantanés qui ont été pris sous la section Instantané. Les instantanés sont répertoriés par planning. 

## Comment puis-je voir quelle quantité d'espace d'instantané a été utilisée ? 

Le graphique circulaire situé en haut de la page Détails affiche la quantité d'espace qui a été utilisée, ainsi que la quantité d'espace restant. Vous recevrez des notifications lorsque vous commencerez à atteindre des seuils d'espace : 75 %, 90 % et 95 %.

## Comment puis-je modifier la quantité d'espace d'instantané pour mon volume ? 

Vous aurez peut-être besoin d'ajouter de l'espace d'instantané à un volume qui n'en avait pas jusque-là ou qui peut nécessiter de l'espace d'instantané supplémentaire. Vous pouvez ajouter de 5 Go à 4 000 Go, en fonction de vos besoins.  

**Remarque** : L'espace d'instantané peut uniquement être augmenté ; il ne peut pas être réduit. Vous voudrez peut-être sélectionner une quantité d'espace moins importante tant que vous n'avez pas déterminé la quantité d'espace dont vous avez réellement besoin. N'oubliez pas que les instantanés automatisés et manuels partagent le même espace. 

L'espace d'instantané est modifié via les options **Stockage, {{site.data.keyword.blockstorageshort}}**. 

1. Cliquez sur vos volumes de stockage, cliquez sur la zone déroulante **Actions**, puis sur **Ajouter de l'espace instantané supplémentaire**. 
2. A l'invite, sélectionnez une taille dans la plage proposée. En général, elles s'échelonnent de 0 à la taille de votre volume. 
3. Cliquez sur **Continuer** pour mettre à disposition l'espace supplémentaire. 
4. Saisissez éventuellement un code promotionnel et cliquez sur **Recalculer**. Les zones Prix pour cette commande et Vérification de la commande contiennent les valeurs par défaut. 
5. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service...** et cliquez sur **Valider la commande**. Votre espace d'instantané supplémentaire sera mis à disposition dans quelques minutes. 

## Comment puis-je recevoir des notifications lorsque je suis proche de ma limite d'espace d'instantané et que des instantanés sont supprimés ? 

Des notifications sont envoyées via des tickets de demande de service à partir du support vers l'utilisateur maître sur votre compte lorsque vous atteignez trois seuils d'espace différents : 75 %, 90 % et 95 %.

- **Capacité de 75 % **: Un avertissement est envoyé pour indiquer que l'utilisation de l'espace d'instantané a dépassé 75 %. Si vous tenez compte de l'avertissement et que vous ajoutez manuellement de l'espace ou que vous supprimez les instantanés conservés inutiles, l'action est consignée et le ticket est fermé. Si vous n'effectuez aucune action, vous devez accuser réception manuellement du ticket avant qu'il puisse être fermé. 
- **Capacité de 90 % **: Un deuxième avertissement est envoyé lorsque l'utilisation de l'espace d'instantané a dépassé 90 %. Comme lorsque la limite de 75 % de la capacité est atteinte, si vous prenez les mesures adéquates pour réduire l'espace utilisé, l'action est consignée et le ticket est fermé. Si vous n'effectuez aucune action, vous devez accuser réception manuellement du ticket avant qu'il puisse être fermé. 
- **Capacité de 95 % **: Un dernier avertissement est envoyé. Si aucune action n'est effectuée pour ramener l'espace en dessous du seuil, une notification est générée et une suppression automatique a lieu afin que d'autres instantanés puissent être créés ultérieurement. Les instantanés planifiés sont supprimés, en commençant par le plus ancien, jusqu'à ce que l'utilisation se situe en dessous de 95 %, et continueront à être supprimés chaque fois que l'utilisation dépassera 95 % jusqu'à ce qu'elle se situe en dessous du seuil. Si l'espace est augmenté manuellement ou que des instantanés sont supprimés, l'avertissement sera réinitialisé et renvoyé si le seuil est à nouveau dépassé. Si aucune action n'est exécutée, il s'agira du seul avertissement reçu. 

## Comment puis-je supprimer un planning d'instantané ? 

Les plannings d'instantané peuvent être annulés via les options **Stockage, {{site.data.keyword.blockstorageshort}}**. 

1. Cliquez sur le planning à supprimer dans le cadre **Plannings d'échantillonnage** sur la page **Détails**. 
2. Cochez la case en regard du planning à supprimer et cliquez sur **Enregistrer**. <br />
**Attention** : Si vous utilisez la fonction de réplication, vérifiez que le planning que vous supprimez n'est pas celui utilisé par la réplication. Cliquez [ici](replication.html) pour plus d'informations sur la suppression d'un planning de réplication. 

## Comment puis-je supprimer un instantané ? 

Les instantanés inutiles peuvent être supprimés manuellement pour libérer de l'espace pour les instantanés ultérieurs. La suppression s'effectue via **Stockage, {{site.data.keyword.blockstorageshort}}**. 

1. Cliquez sur votre volume de stockage et faites défiler l'écran vers le bas jusqu'à la section Instantané pour afficher la liste des instantanés existants. 
2. Cliquez sur la liste déroulante **Actions** en regard d'un instantané spécifique, puis cliquez sur **Supprimer** pour supprimer l'instantané. Cette opération n'affectera pas les instantanés ultérieurs ou antérieurs figurant dans le même planning, car il n'existe pas de dépendance entre les instantanés. 

Les instantanés manuels qui ne sont pas supprimés selon la procédure décrite ci-dessus le sont automatiquement (le plus en ancien en premier) lorsque les limitations d'espace sont atteintes. 

## Comment puis-je restaurer mon volume de stockage à un point de cohérence spécifique à l'aide d'un instantané ? 

Vous aurez peut-être besoin de restaurer votre volume de stockage à un point de cohérence spécifique suite à une erreur de l'utilisateur ou à l'altération des données. 

1. Démontez et détachez votre volume de stockage de l'hôte. 
   - Cliquez [ici](accessing_block_storage_linux.html)  pour obtenir des instructions sur {{site.data.keyword.blockstorageshort}} sous Linux. 
   - Cliquez [ici](accessing-block-storage-windows.html)  pour obtenir des instructions sur {{site.data.keyword.blockstorageshort}} sous Microsoft Windows. 
2. Cliquez sur **Stockage**, **{{site.data.keyword.blockstorageshort}}** dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}. 
3. Faites défiler l'écran vers le bas et cliquez sur le volume à restaurer. La section **Instantanés** de la page **Détails** affichera la liste de tous les instantanés sauvegardés, ainsi que leur taille et leur date de création. 
4. Cliquez sur le bouton **Actions** en regard de l'instantané à utiliser, puis cliquez sur **Restaurer**. <br/>
  **Remarque** : L'exécution d'une restauration provoquera la perte des données créées ou modifiées depuis la prise de l'instantané utilisé. Une fois l'opération terminée, votre volume de stockage reviendra à l'état qui était le sien lors de la prise de l'instantané. Un message s'affichera pour vous en informer. 
5. Cliquez sur **Oui** pour lancer la restauration. Vous recevrez un message dans la partie supérieure de la page, indiquant que le volume a été restauré à l'aide de l'instantané sélectionné. En outre, une icône apparaîtra en regard de votre volume sur le service {{site.data.keyword.blockstorageshort}}, indiquant qu'une transaction est en cours. Survolez l'icône pour afficher une boîte de dialogue présentant la transaction. L'icône disparaîtra une fois la transaction terminée. 
6. Montez et reconnectez votre volume de stockage à l'hôte. 
   - Cliquez [ici](accessing_block_storage_linux.html)  pour obtenir des instructions sur {{site.data.keyword.blockstorageshort}} sous Linux. 
   - Cliquez [ici](accessing-block-storage-windows.html)  pour obtenir des instructions sur {{site.data.keyword.blockstorageshort}} sous Microsoft Windows. 
   
**Remarque** : La restauration d'un volume entraînera la suppression de tous les instantanés antérieurs à l'instantané restauré. 
