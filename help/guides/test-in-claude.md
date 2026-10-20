---
title: Tester votre application LLM en tant que connecteur Claude
description: Créez un connecteur Claude à partir de l’URL de votre serveur MCP Applications LLM Adobe et testez-le dans une conversation.
source-git-commit: 0f9dd0a5b1008e183eb8ab5a4c2eae34a78923bb
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 0%
---

# Tester votre application LLM en tant que connecteur [!DNL Claude] {#test-in-claude}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Après le déploiement, votre application LLM expose une URL de serveur MCP. Ajoutez cette URL à [!DNL Claude] en tant que connecteur personnalisé, puis testez les actions et widgets générés.

**Dans ce guide :** découvrez comment ajouter votre application déployée en tant que connecteur [!DNL Claude], choisir son comportement de connexion et tester ses actions et widgets dans une conversation.

Pour une application avec des actions publiques et protégées, [!DNL Claude] prend en charge la connexion différée jusqu’à ce qu’elle appelle une action protégée. Une application dont les actions nécessitent toutes une authentification doit se connecter avant utilisation. Voir [&#x200B; Authentifier les utilisateurs finaux avec votre propre fournisseur d’identité](/help/guides/authentication.md).

## Planifier les exigences

Les connecteurs personnalisés utilisant MCP à distance sont disponibles pour les plans Free, Pro, Max, Team et Enterprise. Les comptes gratuits prennent en charge un connecteur personnalisé. Dans Équipe et Entreprise, un propriétaire autorisé ajoute le connecteur pour l’organisation avant que les membres ne s’y connectent. Les rôles personnalisés d’entreprise peuvent également accorder cette autorisation.

Consultez les instructions actuelles de Claude sur le connecteur personnalisé [Claude](https://claude.com/docs/connectors/custom/add-unlisted) pour les contrôles de plan et d&#39;organisation. Le point d&#39;entrée MCP distant doit être accessible à partir de l&#39;infrastructure de Claude, et pas seulement à partir de votre navigateur local.

## Copier l’URL du serveur MCP

En [!DNL LLM Apps] :

1. Ouvrez la page Détails de l’application .
2. Recherchez **[!UICONTROL Tester l’application]**.
3. Sous **[!UICONTROL Environnement d’évaluation]**, sélectionnez **[!UICONTROL Copier l’URL]**.

## Ajouter le connecteur personnalisé

1. Ouvrez [Personnaliser > Connecteurs](https://claude.ai/customize/connectors). Pour un connecteur à l’échelle de l’organisation, un propriétaire autorisé ouvre plutôt [Paramètres de l’organisation > Connecteurs](https://claude.ai/admin-settings/connectors).
2. Sélectionnez **[!UICONTROL Ajouter un connecteur personnalisé]** ou **[!UICONTROL Ajouter]** > **[!UICONTROL Personnalisé]** > **[!UICONTROL Web]** pour le flux d’organisation.
3. Saisissez le nom du connecteur, le cas échéant, et collez l’URL complète du serveur HTTPS MCP.
4. Si la boîte de dialogue comporte plusieurs étapes, sélectionnez **[!UICONTROL Continuer]** et vérifiez les paramètres d’authentification détectés :
   - **[!UICONTROL Pas de connexion]** pour une application publique dont l’authentification est désactivée.
   - **[!UICONTROL Connectez-vous si nécessaire]** pour une application avec des actions publiques et protégées.
   - **[!UICONTROL Connectez-vous maintenant]** pour demander une connexion avant l’utilisation du connecteur.
5. Pour OAuth, choisissez une méthode d’identité client prise en charge par votre fournisseur d’identité. Si vous utilisez un client préenregistré, saisissez uniquement ses détails dans les paramètres OAuth du connecteur, et non dans l’URL ou les métadonnées d’action.
6. Sélectionnez **[!UICONTROL Ajouter]**. Pour un connecteur d’organisation, chaque membre sélectionne ensuite **[!UICONTROL Connect]** dans sa propre liste de connecteurs.

Certains comptes affichent une boîte de dialogue à un seul écran avec **[!UICONTROL Paramètres avancés]**, tandis que d’autres affichent un flux à plusieurs étapes. Utilisez les [instructions actuelles de la plateforme](https://claude.com/docs/connectors/custom/add-unlisted#choose-authentication-settings) si la disposition diffère.

>[!NOTE]
>
>Utilisez uniquement les connecteurs des développeurs de confiance. Anthropic ne contrôle pas quels outils les développeurs rendent disponibles ou ne garantit pas que ces outils fonctionnent comme prévu ou restent inchangés.

## Autoriser les outils générés

[!DNL Claude] répertorie chaque action générée sous **[!UICONTROL Autorisations d’outil]** sur la page du connecteur. Les nouveaux outils sont définis par défaut sur **[!UICONTROL Approbation requise]**, ce qui vous invite à approuver chaque appel pendant le test.

Laissez les invites d&#39;approbation activées pour les actions qui modifient les données ou effectuent des opérations sensibles. Utilisez **[!UICONTROL Toujours autoriser]** uniquement pour des actions de confiance spécifiques, à faible risque, lorsque cela est approprié pour votre test.

## Tester le connecteur

1. Commencez une nouvelle conversation.
2. Sélectionnez **+** dans la zone de message (ou saisissez `/`), passez la souris sur **[!UICONTROL Connecteurs]**, puis activez le connecteur que vous avez ajouté pour cette conversation.
3. Posez une question correspondant à l’une des actions générées. Par exemple : *Montrez-moi du café.*

Vérifiez que :

- [!DNL Claude] appelle l’action attendue.
- Le widget affiche les exemples de données attendus.
- La réponse textuelle correspond au widget.
- Les contrôles de widget fonctionnent comme prévu.

Pour une authentification mixte, appelez d’abord une action publique sans vous connecter. Ensuite, appelez une action protégée, remplissez l’invite de connexion et confirmez que la même requête réussit. Testez également les autorisations manquantes et les entrées non valides.

Après avoir modifié l’application, redéployez et répétez les tests concernés. Si vous devez modifier les paramètres d’authentification enregistrés d’un connecteur, [!DNL Claude] devrez peut-être supprimer et ajouter à nouveau le connecteur.

## Prochaines étapes

- [Personnaliser les widgets générés](/help/guides/widgets.md).
- [Créer une action à partir de zéro](/help/guides/create-action.md).
