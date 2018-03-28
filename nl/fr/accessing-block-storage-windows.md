---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-15"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Connexion à des numéros d'unité logique (LUN) MPIO iSCSI sous Microsoft Windows

Avant de commencer, assurez-vous que l'hôte qui accède au volume {{site.data.keyword.blockstoragefull}} a été autorisé via le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} :

1. Dans la page de liste {{site.data.keyword.blockstorageshort}}, cliquez sur le bouton **Actions** associé au volume récemment mis à disposition, puis cliquez sur **Hôte autorisé**.
2. Sélectionnez le ou les hôtes autorisés dans la liste et cliquez sur **Soumettre** pour autoriser les hôtes à accéder au volume.

## Montage de volumes {{site.data.keyword.blockstorageshort}}

Vous trouverez ci-dessous la procédure requise pour connecter une instance de calcul {{site.data.keyword.BluSoftlayer_full}} basée sur Windows à un numéro d'unité logique (LUN) d'E-S multi-accès (MPIO) d'interface SCSI (iSCSI). L'exemple se fonde sur Windows Server 2012. Les étapes peuvent être ajustées pour les autres versions de Windows en fonction de la documentation du fournisseur du système d'exploitation. 

### Configuration de la fonction MPIO

1. Lancez le Gestionnaire de serveur et accédez à **Gérer**, **Ajouter des rôles et fonctionnalités**.
2. Cliquez sur **Suivant** pour accéder au menu des fonctionnalités.
3. Faites défiler l'écran vers le bas et cochez la case **MPIO (Multipath I/O)**.
4. Cliquez sur **Installer** pour installer MPIO sur le serveur hôte.
![Ajout de rôles et fonctionnalités](/images/Roles_Features.png)

### Ajout de la prise en charge iSCSI pour MPIO

1. Ouvrez les propriétés MPIO. Pour ce faire, cliquez sur **Démarrer**, pointez sur Outils d'administration, puis cliquez sur **MPIO**.
2. Cliquez sur l'onglet **Discover Multi-Paths**.
3. Cochez la case **Add support for iSCSI devices**, puis cliquez sur **Add**. Lorsque vous êtes invité à redémarrer l'ordinateur, cliquez sur **Oui**.

**Remarque** : Dans le cas de Windows Server 2008, l'ajout de la prise en charge iSCSI permet à Microsoft Device Specific Module (MSDSM) de demander tous les périphériques iSCSI pour MPIO, ce qui nécessite d'abord une connexion à une cible iSCSI.

### Configuration de l'initiateur iSCSI

1. Lancez l'initiateur iSCSI à partir du Gestionnaire de serveur et sélectionnez **Outils**, **Initiateur iSCSI**.
2. Cliquez sur l'onglet **Configuration**.
    - Le champ Nom de l'initiateur est peut-être déjà renseigné avec une entrée similaire à iqn.1991-05.com.microsoft:.
    - Cliquez sur **Modifier** pour remplacer les valeurs existantes par votre nom qualifié iSCSI. Ce dernier peut être obtenu à partir de l'écran Détails {{site.data.keyword.blockstorageshort}} du portail.
    ![Propriétés de l'initiateur iSCSI](/images/iSCSI.png)
    - Cliquez sur l'onglet **Découverte**, puis sur **Découvrir un portail**.
    - Saisissez l'adresse IP de votre cible iSCSI et laissez la valeur par défaut 3260 dans la zone Port. 
    - Cliquez sur **Avancé** pour ouvrir la fenêtre Paramètres avancés.
    - Sélectionnez **Activer l'ouverture de session CHAP** pour activer l'authentification CHAP.
    ![Activer l'ouverture de session CHAP](/images/Advanced_0.png)
    **Remarque :** Les zones Nom et Secret de la cible sont sensibles à la casse.
         - Supprimez les entrées existantes dans la zone Nom et saisissez le nom d'utilisateur fourni par le portail.
         - Saisissez le mot de passe provenant du portail dans la zone Secret de la cible.<br/>
         **Remarque :** Les valeurs figurant dans les zones Nom et Secret de la cible peuvent être obtenues à partir de l'écran Détails {{site.data.keyword.blockstorageshort}} du portail en tant que Nom d'utilisateur et Mot de passe respectivement. 
    - Cliquez sur **OK** dans les fenêtres Paramètres avancés et Détecter un portail cible pour revenir à l'écran principal des propriétés de l'initiateur SCSI. Si des erreurs d'authentification s'affichent, vérifiez à nouveau le nom d'utilisateur et le mot de passe.
    ![Cible inactive](/images/Inactive_0.png)
    Notez que le nom de votre cible apparaît dans la section Cibles découvertes avec un statut Inactif. 
    
    Les étapes suivantes montrent comment activer la cible.
    
### Activation de la cible

1. Cliquez sur **Connexion** pour vous connecter à la cible.
2. Cochez la case **Enable multi-path** pour activer la fonction MPIO sur la cible.
![Enable Multi-path](/images/Connect_0.png)
3. Cliquez sur **Avancé** et sélectionnez **Activer l'ouverture de session CHAP**.
![Activer l'ouverture de session CHAP](/images/chap_0.png)
4. Saisissez le nom d'utilisateur dans la zone Nom et le mot de passe dans la zone Secret de la cible.<br/>
**Remarque :** Les valeurs figurant dans les zones Nom et Secret de la cible peuvent être obtenues à partir de l'écran Détails {{site.data.keyword.blockstorageshort}} du portail en tant que Nom d'utilisateur et Mot de passe respectivement. 
5. Cliquez sur **OK** jusqu'à ce que la fenêtre Propriétés de l'initiateur iSCSI s'affiche. Le statut de la cible dans la section Cibles découvertes passe d'Inactif à Connecté.
![Statut Connecté](/images/Connected.png) 


### Configuration de la fonction MPIO dans l'initiateur iSCSI

1. Lancez l'initiateur iSCSI. Sur l'onglet Cibles, cliquez sur **Propriétés**.
2. Cliquez sur **Add Session** dans la fenêtre Propriétés pour ouvrir la fenêtre Connect To Target.
3. Cochez la case **Enable multi-path** et cliquez sur **Avancé...**.
  ![Cible](/images/Target.png) 
  
4. Dans la fenêtre Paramètres avancés
   - Laissez la valeur Par défaut pour les zones Adaptateur local et IP de l'initiateur. Pour les serveurs hôte avec plusieurs interfaces vers iSCSI, vous devrez choisir la valeur appropriée pour la zone IP de l'initiateur. 
   - Sélectionnez l'adresse IP de votre stockage iSCSI dans la liste déroulante IP du portail cible.
   - Cochez la case **Activer l'ouverture de session CHAP**.
   - Saisissez les valeurs des zones Nom et Secret de la cible obtenues à partir du portail et cliquez sur **OK**.
   - Cliquez sur **OK** dans la fenêtre Connect To Target pour revenir à la fenêtre Propriétés. Cette fenêtre doit maintenant afficher plusieurs sessions dans la fenêtre Identificateur. Vous disposez désormais de plusieurs sessions dans le stockage iSCSI.
   ![Paramètres](/images/Settings.png) 
   
5. Dans la fenêtre Propriétés, cliquez sur **Périphériques** pour ouvrir la fenêtre correspondante. Le nom de l'interface de périphérique doit comporter mpio au début. <br/>
  ![Périphériques](/images/Devices.png) 
  
6. Cliquez sur **MPIO** pour ouvrir la fenêtre contenant les détails du périphérique. Elle vous permet de choisir les règles d'équilibrage de charge pour MPIO et vous indique également les chemins d'accès à l'interface iSCSI. Dans cet exemple, deux chemins sont disponibles pour MPIO avec une règle d'équilibrage de charge Round Robin avec sous-ensemble.
  ![DeviceDetails](/images/DeviceDetails.png) 
  
7. Cliquez plusieurs fois sur **OK** pour quitter l'initiateur iSCSI. 



## Comment vérifier si MPIO est correctement configuré dans les systèmes d'exploitation Windows

Pour vérifier si Windows MPIO est configuré, vous devez d'abord vous assurer que le module complémentaire MPIO est activé et que le réamorçage prérequis s'est effectué. 

![Roles_Features_0](/images/Roles_Features_0.png)

Une fois le réamorçage terminé et le périphérique de stockage des performances ajouté, nous pouvons vérifier si MPIO est configuré et fonctionne. Pour ce faire, consultez les **Détails du périphérique cible** et cliquez sur **MPIO** :
![DeviceDetails_0](/images/DeviceDetails_0.png)

Si MPIO n'a pas été configuré sur votre périphérique de stockage des performances et que l'équipe {{site.data.keyword.BluSoftlayer_full}} effectue une opération de maintenance ou en cas d'indisponibilité du réseau, votre périphérique de stockage se déconnectera et ne sera plus disponible. MPIO garantira un niveau supplémentaire de connectivité au cours de ces événements et conservera une session établie avec des lectures/écritures actives à destination du numéro d'unité logique (LUN). 

## Démontage de volumes {{site.data.keyword.blockstorageshort}}

Vous trouverez ci-dessous la procédure à suivre pour déconnecter une instance de calcul Bluemix basée sur Windows d'un numéro d'unité logique MPIO iSCSI. L'exemple se fonde sur Windows Server 2012. Les étapes peuvent être ajustées pour les autres versions de Windows en fonction de la documentation du fournisseur du système d'exploitation. 

### Lancez l'initiateur iSCSI.

1. Cliquez sur l'onglet **Cibles**.
2. Sélectionnez les cibles à supprimer et cliquez sur **Déconnexion**.

### Facultatif si vous n'avez plus besoin d'accéder aux cibles iSCSI à l'avenir :

1. Cliquez sur l'onglet **Découverte** dans l'initiateur iSCSI.
2. Mettez en évidence le portail cible associé à votre volume de stockage et cliquez sur **Supprimer**.
