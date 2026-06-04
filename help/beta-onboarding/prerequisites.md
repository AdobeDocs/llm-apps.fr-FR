---
title: Conditions préalables
description: Ce que vous devez configurer avant votre session d’intégration Adobe LLM Apps Beta.
source-git-commit: 1ff383dff82068f68746d665d079216375ba523a
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 2%

---


Avant de commencer votre session d’intégration avec Adobe, vérifiez que les éléments suivants sont en place. Dans la mesure du possible, exécutez les étapes de vérification ci-dessous — les résultats vous indiquent qui doit être dans la salle, et non si vous pouvez continuer.

## Console de développeur Adobe

Vous devez accéder au [&#128279;](https://developer.adobe.com/console) avec le rôle **Développeur** (ou **Administrateur système**) dans votre organisation Adobe IMS. Vérifiez que votre organisation a accès à [[!DNL App Builder]](https://developer.adobe.com/app-builder/docs/intro_and_overview/).

Pour vérifier, accédez à [&#128279;](https://developer.adobe.com/console). Si l’écran de démarrage rapide s’affiche, vos autorisations sont correctement configurées.

![Adobe Developer Console — Écran de démarrage rapide confirmant l’accès développeur](/help/assets/overview/dev-console-access-granted.png)

Si un message **Accès limité** s’affiche à la place de cette réponse, cela signifie que vous ne disposez pas du rôle Développeur. Invitez votre administrateur d’organisation IMS à la session d’intégration.

![Adobe Developer Console — Message à accès restreint](/help/assets/overview/dev-console-access-denied.png)

## [!DNL GitHub]

Vous avez besoin d’un compte [!DNL GitHub] avec les autorisations suivantes dans votre organisation :

- **Créer des référentiels** — vous devez créer deux référentiels dans votre organisation : un pour le code de l’application et un pour le projet EDS. Pour vérifier, accédez à [&#128279;](https://github.com/new) — si vous pouvez sélectionner votre organisation dans la liste déroulante **Propriétaire**, vous disposez de l’autorisation.

  ![GitHub, nouveau menu déroulant Propriétaire du référentiel qui affiche la sélection de l’organisation](/help/assets/overview/github-repo-owner-dropdown.png)

- **Installer des applications [!DNL GitHub]** — vous avez besoin des autorisations appropriées pour installer une application [!DNL GitHub] sur votre organisation. Voir [Conditions requises pour installer une application GitHub](https://docs.github.com/en/apps/using-github-apps/installing-a-github-app-from-a-third-party#requirements-to-install-a-github-app).

**Vérification des autorisations avant la session d’intégration**

Effectuez cette vérification rapide avant de rencontrer Adobe. Le résultat vous indique qui doit être dans la salle, et non si vous pouvez continuer.

1. Accédez à [&#128279;](https://github.com/new) sélectionnez votre organisation en tant que propriétaire, puis créez un référentiel nommé `llm-apps-test`.
2. Accédez à la page d’installation du Vérificateur d’autorisations des applications LLM [&#128279;](https://github.com/apps/adobe-llm-apps-permission-checker/installations/new) d’Adobe et installez l’application pour le référentiel `llm-apps-test` uniquement.

| Résultat | Ce que cela signifie | Action |
|---|---|---|
| Les deux étapes réussissent | Vous disposez des autorisations requises | Vous êtes prêt pour la session d’intégration |
| L’étape 2 indique **Requête** au lieu de **Installation** | Vous n’êtes pas autorisé à installer les applications [!DNL GitHub] | Invitez l’administrateur de votre organisation [!DNL GitHub] à la réunion d’intégration |

Une fois que vous avez terminé, supprimez le référentiel `llm-apps-test` et désinstallez l’application de vérification des autorisations à partir des paramètres de votre organisation.

## AEM Sites avec [!DNL Edge Delivery Services]

Les widgets d’action sont hébergés sur **Adobe Experience Manager [!DNL Edge Delivery Services] (EDS)**. Votre entreprise a besoin d’une licence AEM Sites qui inclut [!DNL Edge Delivery Services]. Vous devez disposer du rôle **Admin** dans votre organisation EDS.

Pour vérifier, accédez à l’outil [EDS User Admin](https://tools.aem.live/tools/user-admin/index.html), saisissez le nom de votre organisation, laissez le champ **Site** vide, puis cliquez sur **Récupérer des utilisateurs**. Recherchez votre compte dans la liste et vérifiez qu’il affiche le badge **admin**.

![&#x200B; Outil d’administration des utilisateurs EDS présentant un utilisateur avec le rôle d’administrateur](/help/assets/overview/eds-user-admin.png)

Si vous n’avez pas encore d’organisation EDS, aucune action n’est nécessaire ; une organisation sera créée pour vous pendant le processus d’intégration.

## Plateforme LLM (pour les tests)

Pour tester votre application déployée, vous avez besoin d’un niveau d’abonnement pris en charge qui autorise les applications MCP personnalisées et l’activation du **mode Développeur**. Par exemple, [!DNL ChatGPT] nécessite un abonnement **Pro**, **Business** ou **Enterprise/Edu**.
