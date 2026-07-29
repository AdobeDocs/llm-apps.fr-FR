---
title: Déploiement de l’application
description: Découvrez comment déployer votre application LLM Adobe vers les environnements d’évaluation et de production à l’aide de l’interface utilisateur des applications LLM.
source-git-commit: eec74b87457bc852d7a8dd0e46c2a4385a93ae0a
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---


# Déploiement De L’Application {#deploy-your-app}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Une fois que vous avez écrit votre code de gestionnaire et que vous l’avez envoyé à votre référentiel lié, vous pouvez déployer l’application à partir de l’interface utilisateur de [!DNL LLM Apps].

Il s’agit d’une étape partagée pour chaque parcours. Après le déploiement, continuez à [tester le plug-in ChatGPT](/help/guides/test-in-chatgpt.md).

## Démarrer le déploiement

Ouvrez la page Détails de l’application et sélectionnez **[!UICONTROL Déployer]**.

Sélectionnez l’environnement cible, puis sélectionnez **[!UICONTROL Déployer]**.

![Déployer — sélectionner l&#39;environnement cible](/help/assets/guide-onboarding-agent/deploy-stage.png)

Le déploiement s’exécute en quatre étapes :

1. **Préparation** — récupère la configuration requise pour déployer l&#39;application.
2. **Démarrer le déploiement** — lance le processus de déploiement en arrière-plan.
3. **Générer une application** — installe les dépendances et génère le code de référentiel le plus récent.
4. **Publier** — publie l&#39;application sur [!DNL Adobe I/O Runtime].

![Déployer — Pipeline de déploiement en cours d’exécution](/help/assets/guide-onboarding-agent/deploy-running.png)

>[!NOTE]
>
>Si une action contient des métadonnées dans l’interface utilisateur mais aucun fichier de gestionnaire correspondant dans le référentiel, elle est toujours enregistrée. Les appels utilisent un gestionnaire de stub par défaut jusqu’à ce que vous ajoutiez le code réel.

## Après un déploiement réussi

Une fois toutes les étapes terminées, la boîte de dialogue affiche **Déploiement réussi**.

![Déploiement — déploiement réussi](/help/assets/guide-onboarding-agent/deploy-successful.png)

Cliquez sur **Fermer** pour fermer la boîte de dialogue. Faites défiler l’écran jusqu’à la section **[!UICONTROL Tester l’application]** de la page Détails de l’application :

![Détails de l&#39;application — Copiez l&#39;URL du serveur MCP](/help/assets/guide-onboarding-agent/app-mcp-url.png)

Chaque environnement déployé affiche une URL de serveur MCP. Sélectionnez **[!UICONTROL Copier l’URL]** et utilisez-la pour créer un module externe dans la plateforme LLM cible.

La section **Historique de déploiement** affiche les 10 derniers déploiements :

![Historique de déploiement](/help/assets/guide-deploy/deployment-history.png)

Chaque ligne affiche la date cible **Environnement** (d’évaluation ou de production), **Statut** (de réussite ou d’échec) et la date **Déployé à**. Vous pouvez utiliser ce tableau pour suivre le moment où les déploiements se sont produits et vérifier que les
dernier déploiement réussi.

## Étape suivante

[Testez l’application déployée en tant que plug-in ChatGPT](/help/guides/test-in-chatgpt.md).

