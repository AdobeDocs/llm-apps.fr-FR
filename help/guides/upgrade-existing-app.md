---
title: Mettre à niveau une application existante
description: Mettez à jour un ancien runtime de l’application Adobe LLM et EDS SDK avant d’activer Adobe Analytics ou Customer Journey Analytics.
source-git-commit: 9b2ec369baba5d533088d2dd4adefcb80f5c9727
workflow-type: tm+mt
source-wordcount: '803'
ht-degree: 0%
---

# Mettre à niveau une application existante {#upgrade-existing-app}

>[!IMPORTANT]
>
>Les applications LLM sont disponibles pour tous les clients AEM Sites Cloud Service afin qu’ils puissent développer et gérer leurs propres applications. Une fois le comptage commencé, chaque appel d’application est comptabilisé dans les demandes de contenu AEM Sites du client. Adobe annoncera le taux de contrôle et la date de début avant que le contrôle ne prenne effet.

Utilisez ce guide uniquement si votre application utilise `@adobe/llm-apps-runtime` une version antérieure à la version 1.2.0 ou si ses widgets EDS utilisent le SDK des applications LLM antérieure à la version 0.1.13. Les applications qui répondent déjà aux deux exigences peuvent passer directement à [Configurer Adobe Analytics et Customer Journey Analytics](/help/guides/analytics-setup.md).

**Dans ce guide :** Découvrez comment mettre à jour et vérifier l’exécution d’une ancienne application et le SDK EDS. Revenez ensuite au guide de configuration d’Analytics pour activer Analytics et vérifier la collecte.

## Avant de commencer {#before-you-begin}

Vous devez accéder au référentiel du gestionnaire et, pour les applications comportant des widgets EDS, au référentiel EDS lié à votre application. Si votre application utilise l&#39;infrastructure gérée [!DNL LLM Apps], demandez l&#39;accès au code avant d&#39;effectuer des mises à jour. Voir [Utiliser [!DNL LLM Apps] l’infrastructure gérée](/help/guides/create-app.md#adobe-managed-infrastructure).

Il n’est pas nécessaire de créer une autre application. Mettez à jour uniquement les composants qui se trouvent sous les versions prises en charge. Vous pouvez réutiliser une configuration Adobe existante qui répond aux [exigences de configuration d’Analytics](/help/guides/analytics-setup.md#setup-checklist).

Pour les applications disposant de widgets EDS, publiez et vérifiez d’abord la mise à jour de SDK, puis fusionnez la mise à jour d’exécution et redéployez l’application. Les deux mises à jour du référentiel peuvent être préparées indépendamment, mais les deux doivent être disponibles avant de tester le déploiement compatible avec les analyses.

## Vérifier et mettre à jour le référentiel d’actions des applications LLM {#upgrade-runtime}

1. Ouvrez le référentiel de gestionnaire lié à votre application et installez ses dépendances :

   ```bash
   npm install
   ```

1. Vérifiez le runtime installé :

   ```bash
   npm ls @adobe/llm-apps-runtime
   ```

1. Si le runtime est antérieur à la version 1.2.0, installez la version actuelle :

   ```bash
   npm install @adobe/llm-apps-runtime@latest
   npm ls @adobe/llm-apps-runtime
   ```

   Vérifiez que la version installée est la version 1.2.0 ou ultérieure. Conservez une version ultérieure prise en charge si votre application en utilise déjà une.

1. Vérifiez que la dépendance d’exécution et sa version résolue répondent à l’exigence minimale. La version de l’application est distincte de la dépendance d’exécution.
1. Exécutez les vérifications du référentiel. Pour la norme LLM Apps :

   ```bash
   npm test -- --runInBand
   npm run build
   npm run lint
   ```

   Vérifiez les échecs avant de procéder au déploiement. Si une vérification échoue déjà sur la version d’origine, enregistrez cette ligne de base séparément des échecs introduits par la mise à niveau. Une version locale réussie ne vérifie pas la collecte d’analyses.

1. Validez la mise à jour des dépendances et son fichier verrouillé, puis effectuez une notification push et fusionnez la mise à jour dans la branche par défaut du référentiel de gestionnaires, généralement `main`. Le déploiement clone cette branche. Une mise à jour qui n’existe que localement ou sur une branche de fonctionnalité non fusionnée n’est pas incluse dans le déploiement.

   Lorsqu&#39;un fichier verrouillé existe, le déploiement installe les dépendances avec `npm ci`, qui utilise les versions enregistrées. Le redéploiement ne met pas automatiquement à niveau l’exécution. Avant le redéploiement, confirmez la dépendance mise à niveau et la version résolue sur la branche par défaut du référentiel.

## Vérifier et mettre à jour le référentiel de widgets d’applications LLM {#upgrade-sdk}

Pour les applications disposant de widgets EDS, vérifiez le SDK dans le référentiel EDS ainsi que l’exécution du serveur. La mise à jour d’un référentiel ne met pas à jour l’autre.

Certains référentiels EDS incluent des scripts SDK copiés sans dépendance npm. Le navigateur charge directement ces scripts diffusés. Ainsi, l’absence d’entrée de package ne signifie pas que le SDK est manquant. Actualisez la distribution SDK copiée à l’aide du processus de mise à jour SDK de votre projet. L’ajout d’une dépendance npm persistante n’est pas nécessaire pour un projet qui conserve le SDK en tant que scripts copiés.

Pour les projets qui gèrent le SDK via npm :

1. À partir de la racine du référentiel EDS, installez le SDK actuel et confirmez sa version :

   ```bash
   npm install --save-dev @adobe/llmapps-sdk@latest
   npm ls @adobe/llmapps-sdk
   ```

   Utilisez la version 0.1.13 ou ultérieure. Conservez une version ultérieure prise en charge si votre projet en utilise déjà une.

1. Vérifiez que l’installation a actualisé la distribution SDK diffusée. Le hook d’installation remplace les scripts SDK copiés. Passez donc en revue toutes les personnalisations locales avant de valider. La mise à jour de la seule déclaration de dépendance ne met pas à jour les scripts diffusés par EDS. Voir [Installer le SDK des applications LLM](/help/guides/bring-your-own-eds.md#install-the-llm-apps-sdk).
1. Exécutez les vérifications du référentiel EDS, telles que `npm run lint`, et vérifiez qu’un widget existant effectue toujours le rendu et répond aux interactions.
1. Validez la distribution SDK actualisée ainsi que les mises à jour des dépendances et des fichiers verrouillés. EDS diffuse directement les scripts copiés, et non le package npm installé.

Après l’une ou l’autre des méthodes de mise à jour, effectuez une notification push et fusionnez la mise à jour dans la branche EDS utilisée par l’**[!UICONTROL URL de script]** de l’action. Vérifiez que l’URL diffuse le SDK mis à jour et que ses bibliothèques de prise en charge se chargent correctement. Si l’URL utilise votre site en ligne EDS, suivez les étapes de publication pour ce site.

## Étape suivante {#next-step}

Revenez à [Configurer Adobe Analytics et Customer Journey Analytics](/help/guides/analytics-setup.md). Toutes les applications doivent activer Analytics et être déployées pour collecter les événements ; la mise à niveau du runtime ou de SDK seule n’active pas la collecte.

Après avoir enregistré les paramètres Analytics, déployez sur l’environnement intermédiaire et [vérifiez les nouveaux événements d’appel d’action et d’interaction de widget](/help/guides/analytics-setup.md#verify) avant de les déployer en production. Vérifiez que l’action et le widget fonctionnent toujours et qu’une seule sélection de lien produit un événement d’interaction de widget.

Analytics commence à collecter les nouvelles interactions après le déploiement activé pour Analytics. La mise à niveau ne reconstruit pas les interactions qui se sont produites avant l’activation de la collecte.
