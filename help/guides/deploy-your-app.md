---
title: Déploiement de l’application
description: Découvrez comment déployer votre application LLM Adobe vers les environnements d’évaluation et de production à l’aide de l’interface utilisateur des applications LLM.
source-git-commit: 557173e8d95325b980efac4d72c06bb3d90dcd72
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 0%
---

# Déploiement de l’application {#deploy-your-app}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Une fois que vous avez révisé les actions générées ou envoyé votre propre code de gestionnaire au référentiel lié, déployez l’application à partir de [!DNL LLM Apps]. Pour les actions générées, examinez et fusionnez le travail généré avant le déploiement.

**Dans ce guide :** découvrez comment déployer votre application dans l’environnement intermédiaire, vérifier le résultat du déploiement et copier l’URL de son serveur MCP pour le test. Déploiement en production une fois le test terminé.

## Démarrer le déploiement

Ouvrez la page Détails de l’application et sélectionnez **[!UICONTROL Déployer]**.

Sélectionnez l’environnement cible, puis sélectionnez **[!UICONTROL Déployer]**.

Si vos gestionnaires utilisent des [variables d’application](/help/guides/app-variables.md), configurez-les pour l’environnement cible avant de procéder au déploiement. Les variables ajoutées, mises à jour ou supprimées prennent effet dans ce déploiement. Les variables d’évaluation et de production ont des valeurs indépendantes.

![Déployer — sélectionner l&#39;environnement cible](/help/assets/guide-onboarding-agent/deploy-stage.png)

Le déploiement s’exécute en quatre étapes :

1. **Préparation** — récupère la configuration requise pour déployer l&#39;application.
2. **Démarrer le déploiement** — lance le processus de déploiement en arrière-plan.
3. **Générer une application** — installe les dépendances et génère le code de référentiel le plus récent.
4. **Publier** — publie l&#39;application sur [!DNL Adobe I/O Runtime].

>[!NOTE]
>
>Si une action contient des métadonnées dans l’interface utilisateur mais aucun fichier de gestionnaire correspondant dans le référentiel, le déploiement enregistre toujours l’action. Les appels utilisent un gestionnaire de stub par défaut jusqu’à ce que vous ajoutiez le code réel.

## Après un déploiement réussi

Une fois toutes les étapes terminées, la boîte de dialogue affiche **Déploiement réussi**.

Pour fermer la boîte de dialogue, sélectionnez **Fermer**. Sur la page Détails de l’application, faites défiler l’écran pour **[!UICONTROL Tester l’application]** :

![Détails de l&#39;application — Copiez l&#39;URL du serveur MCP](/help/assets/guide-onboarding-agent/app-mcp-url.png)

Chaque environnement déployé affiche une URL de serveur MCP. Sélectionnez **[!UICONTROL Copier l’URL]** et utilisez-la pour créer un plug-in ou un connecteur dans la plateforme LLM cible.

La section **Historique de déploiement** affiche les 10 derniers déploiements :

![Historique de déploiement](/help/assets/guide-deploy/deployment-history.png)

Chaque ligne affiche la date cible **Environnement** (évaluation ou production), **Statut** (Succès ou Échec) et **Déployé à**. Utilisez le tableau pour suivre les dates de déploiement et vérifier que le dernier déploiement a réussi.

## Déployer les modifications de configuration

Les modifications apportées aux métadonnées d’action, aux commutateurs **[!UICONTROL Visibilité]**, à l’authentification, aux variables d’application et aux paramètres Analytics de la page d’application prennent effet lors du prochain déploiement dans cet environnement. L’enregistrement d’un paramètre ne met pas à jour le serveur MCP déployé en lui-même.

Vérifiez l’environnement cible à chaque fois. Le déploiement dans l’environnement intermédiaire ne déploie pas les mêmes modifications dans la production.

Si votre application ne répond pas aux exigences d’exécution d’Analytics ou de SDK, suivez la section [Mettre à niveau une application existante](/help/guides/upgrade-existing-app.md) avant de procéder à un redéploiement.

## Étape suivante

- [Testez l’application déployée en tant que plug-in ChatGPT](/help/guides/test-in-chatgpt.md).
- [Testez l’application déployée en tant que connecteur Claude](/help/guides/test-in-claude.md).
- [Envoyez les événements de votre application à Adobe Analytics ou Customer Journey Analytics](/help/guides/analytics-setup.md).
