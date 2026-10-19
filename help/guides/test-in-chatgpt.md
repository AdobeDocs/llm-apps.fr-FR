---
title: Tester votre application LLM en tant que plug-in ChatGPT
description: Créez un plug-in ChatGPT à partir de l’URL de votre serveur MCP Applications LLM Adobe et testez-le dans une conversation.
source-git-commit: 0f9dd0a5b1008e183eb8ab5a4c2eae34a78923bb
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 0%
---

# Tester votre application LLM en tant que plug-in [!DNL ChatGPT] {#test-in-chatgpt}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Après le déploiement, votre application LLM expose une URL de serveur MCP. Ajoutez cette URL à [!DNL ChatGPT] en tant que plug-in, puis testez les actions et widgets générés.

**Dans ce guide :** découvrez comment connecter votre application déployée à [!DNL ChatGPT] et tester ses actions et widgets dans une conversation. Actualisez la connexion après avoir modifié l’application.

## Planifier les exigences

Utilisez un compte et un espace de travail qui permettent d’ajouter des serveurs MCP personnalisés. Les contrôles de disponibilité et d’administration varient selon le compte et l’espace de travail. Voir la documentation actuelle d’OpenAI [Connect and test your plugin](https://developers.openai.com/plugins/deploy/connect-chatgpt).

## Vérifier l’accès aux serveurs MCP personnalisés

Ouvrez [Plugins ChatGPT](https://chatgpt.com/plugins) et vérifiez que votre compte permet d’ajouter un serveur MCP personnalisé. Si votre compte nécessite le mode Développeur, activez ce mode à l’aide des contrôles de votre compte. Si l’option de serveur MCP personnalisé est manquante ou bloquée, contactez votre administrateur d’espace de travail.

## Copier l’URL du serveur MCP

En [!DNL LLM Apps] :

1. Ouvrez la page Détails de l’application .
2. Recherchez **[!UICONTROL Tester l’application]**.
3. Sous **[!UICONTROL Environnement d’évaluation]**, sélectionnez **[!UICONTROL Copier l’URL]**.

## Création du plug-in

1. Ouvrez [](https://chatgpt.com/plugins).
2. Sélectionnez **+**, puis **[!UICONTROL Ajouter un serveur MCP personnalisé]**.
3. Configurez la connexion :
   - **[!UICONTROL Nom]** — nom du module externe.
   - **[!UICONTROL Description]** — facultatif.
   - Choisissez la méthode de connexion au serveur public et saisissez l’URL complète du serveur HTTPS MCP de votre application.
   - Sélectionnez la méthode d’authentification qui correspond à votre application. Pour une application publique dont l’authentification est désactivée, utilisez **[!UICONTROL Aucune authentification]** le cas échéant.

   >[!NOTE]
   >
   >Si vous avez activé l’authentification de l’utilisateur final, sélectionnez **[!UICONTROL OAuth]** lorsque chaque action est définie sur **[!UICONTROL Obligatoire]** ou **[!UICONTROL Mixte]** pour une combinaison d’actions publiques et protégées, où ces choix sont proposés. Voir [ Authentifier les utilisateurs finaux avec votre propre fournisseur d’identité](/help/guides/authentication.md).

4. Sélectionnez **[!UICONTROL Je comprends et je souhaite continuer]**.
5. Sélectionnez **[!UICONTROL Créer en tant que plug-in]**.
6. Passez en revue les outils et les métadonnées découverts, puis installez ou connectez le plug-in obtenu. Effectuez la connexion si votre application l’exige.

OpenAI possède l’interface utilisateur de configuration et sa disposition peut varier en fonction du compte. Suivez les instructions de connexion [en cours](https://developers.openai.com/plugins/deploy/connect-chatgpt) si les libellés affichés diffèrent.

## Tester le plug-in

1. Commencez une nouvelle conversation.
2. Saisissez `@` dans le champ d’invite, puis sélectionnez le plug-in installé.
3. Posez une question correspondant à l’une des actions générées. Par exemple : *Montrez-moi du café.*

![Réponse illustrative ChatGPT — une action Frescopa renvoie des cartes de produits dans la conversation](/help/assets/guide-onboarding-agent/chatgpt-generated-app.png)

Vérifiez que :

- [!DNL ChatGPT] appelle l’action attendue.
- Le widget affiche les exemples de données attendus.
- La réponse textuelle correspond au widget.
- Les contrôles de widget fonctionnent comme prévu.

Testez également une entrée non valide, des résultats vides, des invites non liées qui n&#39;appellent pas l&#39;application et des actions qui modifient les données. Examinez chaque invite de confirmation et d’authentification plutôt que d’approuver toutes les demandes à l’avance.

## Actualiser après les modifications de l’application

Après avoir modifié les descriptions des actions, les entrées, l’authentification ou la configuration de widget :

1. Déployez l’application mise à jour dans le même environnement.
2. Ouvrez sa connexion sur [Plug-ins ChatGPT](https://chatgpt.com/plugins) et sélectionnez **[!UICONTROL Actualiser]**.
3. Vérifiez que les actions annoncées ont changé.
4. Démarrez une nouvelle conversation et répétez les tests concernés.

## Prochaines étapes

- [Personnaliser les widgets générés](/help/guides/widgets.md).
- [Créer une action à partir de zéro](/help/guides/create-action.md).
