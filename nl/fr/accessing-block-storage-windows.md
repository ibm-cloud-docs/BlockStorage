---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-26"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Connexion à des numéros d'unité logique (LUN) MPIO iSCSI sous Microsoft Windows

Avant de commencer, assurez-vous que les droits d'accès nécessaires ont été affectés via le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} à l'hôte qui accède au volume {{site.data.keyword.blockstoragefull}}.

1. Sur la page de liste {{site.data.keyword.blockstorageshort}}, repérez le nouveau volume et cliquez sur **Actions**. Cliquez sur **Hôte autorisé**.
2. Dans la liste, sélectionnez l'hôte ou les hôtes qui doivent avoir accès au volume et cliquez sur **Soumettre**.

## Montage de volumes {{site.data.keyword.blockstorageshort}}

Vous trouverez ci-dessous la procédure requise pour connecter une instance de calcul {{site.data.keyword.BluSoftlayer_full}} basée sur Windows à un numéro d'unité logique (LUN) d'E-S multi-accès (MPIO) d'interface SCSI (iSCSI). L'exemple se fonde sur Windows Server 2012. Les étapes peuvent être ajustées pour les autres versions de Windows en fonction de la documentation du fournisseur du système d'exploitation.

### Configuration de la fonction MPIO

1. Démarrez le gestionnaire de serveur et accédez à **Gérer**, **Ajouter des rôles et fonctionnalités**.
2. Cliquez sur **Suivant** pour accéder au menu des fonctionnalités.
3. Faites défiler l'écran vers le bas et cochez la case **MPIO (Multipath I/O)**.
4. Cliquez sur **Installer** pour installer MPIO sur le serveur hôte.
![Ajout de rôles et fonctionnalités dans le Gestionnaire de serveur](/images/Roles_Features.png)

### Ajout de la prise en charge iSCSI pour MPIO

1. Ouvrez les propriétés MPIO en cliquant sur **Démarrer**, en pointant sur **Outils d'administration**, puis en cliquant sur **MPIO**.
2. Cliquez sur **Découvrir plusieurs chemins**.
3. Cochez la case **Ajouter la prise en charge des périphériques iSCSI**, puis cliquez sur **Ajouter**. Lorsque vous êtes invité à redémarrer l'ordinateur, cliquez sur **Oui**.

**Remarque** : dans le cas de Windows Server 2008, l'ajout de la prise en charge iSCSI permet à Microsoft Device Specific Module (MSDSM) de demander tous les périphériques iSCSI pour MPIO, ce qui nécessite d'abord une connexion à une cible iSCSI.

### Configuration de l'initiateur iSCSI

1. Lancez l'initiateur iSCSI à partir du gestionnaire de serveur et sélectionnez **Outils**, **Initiateur iSCSI**.
2. Cliquez sur l'onglet **Configuration**.
    - La zone Nom de l'initiateur est peut-être déjà renseignée avec une entrée similaire à `iqn.1991-05.com.microsoft:`.
    - Cliquez sur **Modifier** pour remplacer les valeurs existantes par votre nom qualifié iSCSI. Ce dernier peut être obtenu à partir de l'écran Détails {{site.data.keyword.blockstorageshort}} du [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
![Propriétés de l'initiateur iSCSI](/images/iSCSI.png)
    - Cliquez sur l'onglet **Découverte**, puis sur **Découvrir un portail**.
    - Saisissez l'adresse IP de votre cible iSCSI et laissez la valeur par défaut 3260 dans la zone Port. 
    - Cliquez sur **Avancé** pour ouvrir la fenêtre Paramètres avancés.
    - Sélectionnez **Activer l'ouverture de session CHAP** pour activer l'authentification CHAP.
    ![Activer l'ouverture de session CHAP](/images/Advanced_0.png)
    **Remarque :** Les zones Nom et Secret de la cible sont sensibles à la casse.
         - Dans la zone **Nom**, supprimez les entrées existantes et saisissez le nom d'utilisateur à partir du [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
         - Dans la zone **Secret de la cible**, saisissez le mot de passe à partir du [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
    - Cliquez sur **OK** dans les fenêtres **Paramètres avancés** et **Détecter un portail cible** pour revenir à l'écran principal des propriétés de l'initiateur SCSI. Si des erreurs d'authentification s'affichent, vérifiez le nom d'utilisateur et le mot de passe.
    ![Cible inactive](/images/Inactive_0.png)
    **Remarque** : le nom de votre cible apparaît dans la section Cibles découvertes avec un statut Inactif. 

    
### Activation de la cible

1. Cliquez sur **Connexion** pour vous connecter à la cible.
2. Cochez la case **Activer la prise en charge de plusieurs chemins d'accès** pour activer la fonction MPIO sur la cible.
![Activer la prise en charge de plusieurs chemins d'accès](/images/Connect_0.png)
3. Cliquez sur **Avancé** et sélectionnez **Activer l'ouverture de session CHAP**.
![Activer l'ouverture de session CHAP](/images/chap_0.png)
4. Saisissez le nom d'utilisateur dans la zone Nom et le mot de passe dans la zone Secret de la cible.<br/>
**Remarque :** Les valeurs figurant dans les zones Nom et Secret de la cible peuvent être obtenues à partir de l'écran Détails {{site.data.keyword.blockstorageshort}}.
5. Cliquez sur **OK** jusqu'à ce que la fenêtre **Propriétés de l'initiateur iSCSI** s'affiche. Le statut de la cible dans la section **Cibles découvertes** passe d'**Inactif** à **Connecté**.
![Statut Connecté](/images/Connected.png) 


### Configuration de la fonction MPIO dans l'initiateur iSCSI

1. Lancez l'initiateur iSCSI et sur l'onglet Cibles, cliquez sur **Propriétés**.
2. Cliquez sur **Ajouter une session** dans la fenêtre Propriétés pour ouvrir la fenêtre Se connecter à la cible.
3. Cochez la case **Activer la prise en charge de plusieurs chemins d’accès** et cliquez sur **Avancé...**.
  ![Cible](/images/Target.png) 
  
4. Dans la fenêtre Paramètres avancés
   - Laissez la valeur Par défaut pour les zones Adaptateur local et IP de l'initiateur. Pour les serveurs hôte avec plusieurs interfaces vers iSCSI, vous devez choisir la valeur appropriée pour la zone IP de l'initiateur.
   - Sélectionnez l'adresse IP de votre stockage iSCSI dans la liste déroulante **IP du portail cible**.
   - Cochez la case **Activer l'ouverture de session CHAP**.
   - Saisissez les valeurs des zones Nom et Secret de la cible obtenues à partir du portail et cliquez sur **OK**.
   - Cliquez sur **OK** dans la fenêtre Se connecter à la cible pour revenir à la fenêtre Propriétés. Cette fenêtre affiche maintenant plusieurs sessions dans la sous-fenêtre Identificateur. Vous disposez désormais de plusieurs sessions dans le stockage iSCSI.
   ![Paramètres](/images/Settings.png) 
   
5. Dans la fenêtre Propriétés, cliquez sur **Périphériques** pour ouvrir la fenêtre correspondante. Le nom de l'interface de périphérique débute par `mpio`. <br/>
  ![Périphériques](/images/Devices.png) 
  
6. Cliquez sur **MPIO** pour ouvrir la fenêtre **Détails du périphérique**. Elle vous permet de choisir les règles d'équilibrage de charge pour MPIO et vous indique les chemins d'accès à l'interface iSCSI. Dans cet exemple, deux chemins sont disponibles pour MPIO avec une règle d'équilibrage de charge Round Robin avec sous-ensemble.
  ![Boîte de dialogue Détails du périphérique affichant deux chemins d'accès disponibles pour MPIO avec une règle d'équilibrage de charge Round Robin avec sous-ensemble](/images/DeviceDetails.png) 
  
7. Cliquez plusieurs fois sur **OK** pour quitter l'initiateur iSCSI.



## Vérification que MPIO est correctement configuré dans les systèmes d'exploitation Windows

Pour vérifier si Windows MPIO est configuré, vous devez d'abord vous assurer que le module complémentaire MPIO est activé et redémarrer le serveur.

![Roles_Features_0](/images/Roles_Features_0.png)

Une fois le réamorçage terminé et le périphérique de stockage ajouté, vous pouvez vérifier si MPIO est configuré et fonctionne. Pour ce faire, consultez les **Détails du périphérique cible** et cliquez sur **MPIO** :
![DeviceDetails_0](/images/DeviceDetails_0.png)

Si MPIO n'a pas été configuré correctement, votre périphérique de stockage est déconnecté et devient indisponible en cas de panne réseau ou de maintenance par les équipes {{site.data.keyword.BluSoftlayer_full}}. MPIO garantit un niveau supplémentaire de connectivité au cours de ces événements et conserve une session établie avec des lectures/écritures actives à destination du numéro d'unité logique (LUN).

## Démontage de volumes {{site.data.keyword.blockstorageshort}}

Vous trouverez ci-dessous la procédure à suivre pour déconnecter une instance de calcul {{site.data.keyword.Bluemix_short}} basée sur Windows d'un numéro d'unité logique MPIO iSCSI. L'exemple se fonde sur Windows Server 2012. Les étapes peuvent être ajustées pour les autres versions de Windows en fonction de la documentation du fournisseur du système d'exploitation.

### Démarrage de l'initiateur iSCSI

1. Cliquez sur l'onglet **Cibles**.
2. Sélectionnez les cibles à retirer et cliquez sur **Déconnexion**.

### Retrait de cibles
Facultatif, si vous n'avez plus besoin d'accéder aux cibles iSCSI.

1. Cliquez sur **Découverte** dans l'initiateur iSCSI.
2. Mettez en évidence le portail cible qui est associé à votre volume de stockage et cliquez sur **Supprimer**.
