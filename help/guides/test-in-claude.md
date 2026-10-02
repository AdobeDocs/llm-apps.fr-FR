---
title: Tester votre application LLM en tant que connecteur Claude
description: Créez un connecteur Claude à partir de l’URL de votre serveur MCP Applications LLM Adobe et testez-le dans une conversation.
source-git-commit: fd41dbcabc4db0cae766de19cb042d7c85d8b7aa
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 1%
---

# Tester votre application LLM en tant que connecteur [!DNL Claude] {#test-in-claude}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Après le déploiement, votre application LLM expose une URL de serveur MCP. Ajoutez cette URL à [!DNL Claude] en tant que connecteur personnalisé, puis testez les actions et widgets générés.

Il s’agit de l’étape de vérification finale après la création, la personnalisation ou l’extension d’une application.

Ce guide suppose que les actions de l’application sont publiques. Si l’authentification de l’utilisateur final est activée pour l’application, [!DNL Claude] vous demande de vous connecter avec le fournisseur d’identité de l’application avant de pouvoir utiliser le connecteur. Aucun outil n’est répertorié tant que vous ne l’avez pas fait. Voir [&#x200B; Authentifier les utilisateurs finaux avec votre propre fournisseur d’identité](/help/guides/authentication.md).

## Planifier les exigences

Les connecteurs personnalisés utilisant MCP à distance sont disponibles sur [!DNL Claude], [!DNL Claude] Desktop et Cowork pour les plans Free, Pro, Max, Team et Enterprise. Les comptes de plan gratuits sont limités à un connecteur personnalisé. Pour les organisations d’équipe et d’entreprise, un propriétaire ou un propriétaire de Principal doit activer les connecteurs avant que d’autres membres puissent les utiliser.

## Copier l’URL du serveur MCP

En [!DNL LLM Apps] :

1. Ouvrez la page Détails de l’application .
2. Recherchez **[!UICONTROL Tester l’application]**.
3. Sous **[!UICONTROL Environnement d’évaluation]**, sélectionnez **[!UICONTROL Copier l’URL]**.

## Ajouter le connecteur personnalisé

1. Ouvrez [&#128279;](https://claude.ai/new?modal=add-custom-connector#settings/customize-connectors). Cela ouvre directement la boîte de dialogue **[!UICONTROL Ajouter un connecteur personnalisé]**.
2. Enter :
   - **[!UICONTROL Nom]** : nom du connecteur.
   - **[!UICONTROL URL du serveur MCP distant]** : URL du serveur MCP que vous avez copiée.
3. Sélectionnez **[!UICONTROL Ajouter]**.

   ![Claude — Boîte de dialogue Ajouter un connecteur personnalisé](/help/assets/guide-test-claude/claude-add-custom-connector.png)

>[!NOTE]
>
>N’utilisez que les connecteurs de développeurs de confiance. Anthropic ne contrôle pas les outils que les développeurs rendent disponibles et ne peut pas vérifier qu’ils fonctionneront comme prévu ou qu’ils ne changeront pas.

## Autoriser les outils générés

Chaque action générée est répertoriée sous **[!UICONTROL Autorisations des outils]** sur la page du connecteur. Par défaut, les nouveaux outils sont définis sur **[!UICONTROL Approbation requise]**, ce qui vous invite à approuver chaque appel pendant le test.

Définissez chaque outil, ou l’ensemble du groupe **[!UICONTROL Outils interactifs]**, sur **[!UICONTROL Toujours autoriser]** afin que le test ne soit pas interrompu par des invites d’approbation.

![Claude — définir les autorisations d&#39;outil sur Toujours autoriser](/help/assets/guide-test-claude/claude-tool-permissions.png)

## Tester le connecteur

1. Commencez une nouvelle conversation.
2. Sélectionnez **+** dans la zone de message (ou saisissez `/`), passez la souris sur **[!UICONTROL Connecteurs]**, puis activez le connecteur que vous avez ajouté pour cette conversation.

   ![Claude — activez le connecteur pour la conversation](/help/assets/guide-test-claude/claude-enable-connector-chat.png)

3. Posez une question correspondant à l’une des actions générées. Par exemple : *Montrez-moi du café.*

Vérifiez que :

- [!DNL Claude] appelle l’action attendue.
- Le widget affiche les exemples de données attendus.
- La réponse textuelle correspond au widget.
- Les contrôles de widget fonctionnent comme prévu.

## Prochaines étapes

- [Personnaliser les widgets générés](/help/guides/widgets.md).
- [Créer une action à partir de zéro](/help/guides/create-action.md).
