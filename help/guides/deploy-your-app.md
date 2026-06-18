---
title: Déploiement De L’Application
description: Découvrez comment déployer votre application LLM Adobe vers les environnements d’évaluation et de production à l’aide de l’interface utilisateur des applications LLM.
source-git-commit: 1a99e2e80e50a3bcf9ce6fb910365202bf06e113
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 0%

---


# Déploiement De L’Application

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Une fois que vous avez écrit votre code de gestionnaire et que vous l’avez envoyé à votre référentiel lié, vous pouvez déployer l’application à partir de l’interface utilisateur de [!DNL LLM Apps].

## Démarrer le déploiement

Accédez à la page Détails de l’application. Cliquez sur le bouton **[!UICONTROL Déployer]** dans le coin supérieur droit :

![Détails de l’application : prêt à être déployé](/help/assets/guide-deploy/app-detail-deploy-ready.png)

La boîte de dialogue de déploiement s’ouvre. Sélectionnez l’environnement cible dans la liste déroulante :

![Boîte de dialogue Déployer — Sélectionner l’environnement cible](/help/assets/guide-deploy/deploy-pipeline-dropdown.png)

Cliquez sur **[!UICONTROL Déployer]** pour démarrer le pipeline. Les quatre étapes sont les suivantes :

1. **Collecter les informations d’identification** — lit les métadonnées de l’application, génère un jeton [!DNL GitHub] et récupère les informations d’identification d’exécution à partir de l’API de console.
2. **Déclencher le pipeline de création** — envoie tous les paramètres au pipeline de création.
3. **Cloner et créer** : le pipeline clone votre référentiel, génère des `actions.json` à partir des métadonnées de l’interface utilisateur, exécute `npm install` et webpack pour produire des `dist/index.js`.
4. **Déployer au moment de l’exécution** — déploie le bundle dans l’espace de noms [!DNL Adobe I/O Runtime] de votre application.

Une fois démarré, le pipeline s’exécute automatiquement et affiche la progression en temps réel :

![Exécution du pipeline de déploiement](/help/assets/guide-deploy/deploy-pipeline-deploying.png)

>[!NOTE]
>
>Si une action contient des métadonnées dans l’interface utilisateur mais aucun fichier de gestionnaire correspondant dans le référentiel, elle est toujours enregistrée. Les appels utilisent un gestionnaire de stub par défaut jusqu’à ce que vous ajoutiez le code réel.

## Après un déploiement réussi

Une fois toutes les étapes terminées, la boîte de dialogue affiche une confirmation **Déploiement réussi** avec l’URL déployée et les détails de l’artefact :

![Déploiement réussi](/help/assets/guide-deploy/app-detail-deploy-finish.png)

Cliquez sur **Fermer** pour fermer la boîte de dialogue. Faites défiler l’écran jusqu’à la section **[!UICONTROL Tester l’application]** de la page Détails de l’application :

![Tester l’application — URL déployées](/help/assets/guide-deploy/test-app-deployed.png)

Chaque environnement (**Évaluation** et **Production**) affiche l’URL du serveur MCP sur [!DNL Adobe I/O Runtime]. Il s’agit de l’URL que vous fournissez à la plateforme LLM lors de l’enregistrement de votre application. Cliquez sur **Copier l’URL** pour la copier dans le presse-papiers.

La section **Historique de déploiement** ci-dessous conserve un journal complet de chaque déploiement dans les environnements :

![Historique de déploiement](/help/assets/guide-deploy/deployment-history.png)

Chaque ligne affiche la date cible **Environnement** (d’évaluation ou de production), **Statut** (de réussite ou d’échec) et la date **Déployé à**. Vous pouvez utiliser ce tableau pour suivre le moment où les déploiements se sont produits et vérifier que les
dernier déploiement réussi.

