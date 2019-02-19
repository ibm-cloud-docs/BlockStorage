---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-05"

---

{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Windows 2012 R2 - unités iSCSI multiples
{: #troubleshootingWin12}

Si vous utilisez plus de deux périphériques iSCSI, cette procédure peut vous être utile, notamment si les 4 allocations iSCSI proviennent du même périphérique de stockage. Si le gestionnaire de disque n'affiche que deux périphériques, vous devez vous connecter manuellement chaque périphérique dans l'initiateur iSCSI sur chaque noeud de serveur.

1. Ouvrez l'initiateur iSCSI Windows.
2. Cliquez sur l'onglet **Cibles**, puis sur **Périphériques**.

   ![Propriétés de l'initiateur iSCSI](/images/win12-ts1.png)
3. Confirmez le nombre de périphériques affichés. Si vous voyez 2 périphériques au lieu des 4 autorisés, passez à l'étape suivante.
4. Cliquez sur **Cibles**, puis sur **Connecter**.
5. Sélectionnez **%%% Multipath**, puis **%%% Advanced**.
6. Sélectionnez initiateur iSCSI Microsoft comme adaptateur Local. L'IP de l'initiateur appartient à votre server.
7. Sélectionnez la première des adresses IP affichées dans la liste des adresses IP du portail cible.

   ![Paramètres avancés, adresses IP](/images/win12-ts3.png)

   Vous devez répéter cette étape pour chacune des adresses IP répertoriées.
   {:tip}

8. Cochez la case **Activer CHAP** et entrez l'ID et le mot de passe de protocole CHAP du serveur.

   ![Paramètres avancés, protocole CHAP](/images/win12-ts4.png)
9. Cliquez sur **OK**.
10. Répétez les étapes 5 à 9 pour chaque adresse IP entrée dans l'initiateur iSCSI. Lorsque vous avez terminé, cliquez sur l'onglet **Périphériques** et examinez les résultats. Vous devez voir chaque numéro d'unité logique que vous avez configuré répertorié deux fois.

    ![Onglet Périphériques](/images/win12-ts5.png)
11. Cliquez sur **OK**.
12. Ouvrez le gestionnaire de disque et vérifiez que vos périphériques sont désormais affichés.

    ![Gestionnaire d'unités](/images/win12-ts6.png)
