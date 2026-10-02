---
title: Tester votre application LLM en tant que plug-in ChatGPT
description: Créez un plug-in ChatGPT à partir de l’URL de votre serveur MCP Applications LLM Adobe et testez-le dans une conversation.
source-git-commit: fd41dbcabc4db0cae766de19cb042d7c85d8b7aa
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 1%
---

# Tester votre application LLM en tant que plug-in [!DNL ChatGPT] {#test-in-chatgpt}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Après le déploiement, votre application LLM expose une URL de serveur MCP. Ajoutez cette URL à [!DNL ChatGPT] en tant que plug-in, puis testez les actions et widgets générés.

Il s’agit de l’étape de vérification finale après la création, la personnalisation ou l’extension d’une application.

## Planifier les exigences

Le mode Développeur est disponible sur le web pour les comptes Pro, Plus, Business, Enterprise et Education. Les administrateurs et administratrices de Workspace peuvent restreindre l’accès.

## Activer le mode Développeur

En [!DNL ChatGPT] :

1. Ouvrez **[!UICONTROL Paramètres] → [!UICONTROL Sécurité et connexion]**.
2. Activez le **[!UICONTROL mode Développeur]**.

Le bouton Plus de la page Modules externes crée des modules externes pris en charge par MCP uniquement après l’activation du mode Développeur. Voir [Mode Développeur GPT de conversation](https://developers.openai.com/api/docs/guides/developer-mode).

## Copier l’URL du serveur MCP

En [!DNL LLM Apps] :

1. Ouvrez la page Détails de l’application .
2. Recherchez **[!UICONTROL Tester l’application]**.
3. Sous **[!UICONTROL Environnement d’évaluation]**, sélectionnez **[!UICONTROL Copier l’URL]**.

## Création du plug-in

1. Ouvrez [](https://chatgpt.com/plugins).
2. Dans l’onglet **[!UICONTROL Plugins]**, sélectionnez **+** en regard du champ de recherche.

   ![Page ChatGPT — Modules externes](/help/assets/guide-onboarding-agent/chatgpt-plugins-page.png)

3. Dans **[!UICONTROL Nouveau plug-in]**, saisissez :
   - **[!UICONTROL Nom]** — nom du module externe.
   - **[!UICONTROL Description]** — facultatif.
   - **[!UICONTROL Connexion]** — sélectionnez **[!UICONTROL URL du serveur]** et collez l&#39;URL du serveur MCP.
   - **[!UICONTROL Authentification]** — sélectionnez **[!UICONTROL Aucune authentification]**.

   >[!NOTE]
   >
   >**[!UICONTROL Aucune authentification]** s’applique lorsque chaque action de l’application est publique. Si vous avez activé l’authentification de l’utilisateur final, sélectionnez **[!UICONTROL OAuth]** lorsque chaque action est définie sur **[!UICONTROL Obligatoire]** et **[!UICONTROL Mixte]** pour toute autre combinaison. Voir [Authentifier les utilisateurs finaux avec votre propre fournisseur d’identité](/help/guides/authentication.md).

4. Sélectionnez **[!UICONTROL Je comprends et je souhaite continuer]**.
5. Sélectionnez **[!UICONTROL Créer]**.

   ![ChatGPT — Créez un plug-in avec l&#39;URL du serveur MCP](/help/assets/guide-onboarding-agent/chatgpt-new-plugin.png)

6. Dans la boîte de dialogue de confirmation, sélectionnez **[!UICONTROL Connexion]**.

   ![ChatGPT — Connectez le nouveau plug-in](/help/assets/guide-onboarding-agent/chatgpt-plugin-connect.png)

## Tester le plug-in

1. Commencez une nouvelle conversation.
2. Dans le menu Plus , choisissez **[!UICONTROL mode Développeur]** et sélectionnez le module externe.
3. Posez une question correspondant à l’une des actions générées. Par exemple : *Montrez-moi du café.*

![ChatGPT — réponse du plug-in de l&#39;application LLM générée](/help/assets/guide-onboarding-agent/chatgpt-generated-app.png)

Vérifiez que :

- [!DNL ChatGPT] appelle l’action attendue.
- Le widget affiche les exemples de données attendus.
- La réponse textuelle correspond au widget.
- Les contrôles de widget fonctionnent comme prévu.

## Prochaines étapes

- [Personnaliser les widgets générés](/help/guides/widgets.md).
- [Créer une action à partir de zéro](/help/guides/create-action.md).
