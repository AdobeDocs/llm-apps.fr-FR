---
title: Création d’une application
description: Découvrez comment créer votre première application LLM et la lier à votre référentiel GitHub.
source-git-commit: 344c5457eb79a19b1dae823732a1cd9866dcd9dc
workflow-type: tm+mt
source-wordcount: '720'
ht-degree: 1%

---


# Création d’une application

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

>[!NOTE]
>
>Avant de commencer, assurez-vous que toutes les [conditions préalables](/help/overview/overview.md#prerequisites) sont remplies.

Ce guide vous guide tout au long de la création de votre première [!DNL Adobe LLM Apps], depuis le statut vide jusqu’à un projet entièrement configuré et lié à votre référentiel [!DNL GitHub].

## Ouvrez [!DNL LLM Apps].

Accédez à [](https://experience.adobe.com/llm-apps). Si aucune application n’a encore été créée, la page du premier chargement s’affiche avec une invite vous demandant de créer votre première application.

![Page applications — aucune application créée pour le moment](/help/assets/guide-create-app/first-load.png)

La barre latérale gauche vous permet de naviguer entre **[!UICONTROL Applications]** et **[!UICONTROL Actions]**. Cliquez sur **[!UICONTROL Créer une application]** pour commencer.

## Renseigner les détails de l’application

La boîte de dialogue Créer une application s’ouvre en plein écran.

![ Boîte de dialogue Créer une application ](/help/assets/guide-create-app/app-details-1.png)

Entrez la commande suivante :

- **[!UICONTROL Nom de l’application LLM]** (obligatoire) : nom d’affichage de votre application. Seuls les chiffres, les lettres et les espaces sont autorisés.
- **[!UICONTROL Description de l’application LLM]** — Brève description de la fonction de votre application. Par exemple, *Aide les utilisateurs à découvrir des produits et à réserver des services via une plateforme LLM*.
- **[!UICONTROL Votre site web]** (obligatoire) : URL du site web de votre marque. [!DNL LLM Apps] l’utilise pour créer automatiquement des actions préconfigurées.

## Sélectionner une région de données Analytics

Choisissez la région où les données d’analyse de cette application seront stockées.

>[!IMPORTANT]
>
>La région de données Analytics ne peut pas être modifiée une fois l’application créée.

![Liste déroulante de la région de données Analytics](/help/assets/guide-create-app/app-details-analytics-dropdown.png)

La liste déroulante **région Analytics** est définie par défaut sur **États-Unis (États-Unis)**. Les options disponibles sont **États-Unis (US)** et **Europe (UE)**. Sélectionnez la région qui correspond le mieux à vos exigences en matière de résidence des données avant de continuer.

## Liaison d’un référentiel [!DNL GitHub]

Sous les détails de l’application, vous pouvez lier un référentiel [!DNL GitHub]. Ce référentiel est l’emplacement de votre code de gestionnaire d’action - JavaScript fonctionne sous un dossier `actions/` qui s’exécute sur [!DNL Adobe I/O Runtime] lorsque la plateforme LLM appelle votre application.

Si c’est la première fois, aucun référentiel n’apparaît dans la liste. Vous devez installer l’application **[!DNL Adobe LLM Apps Link]** [!DNL GitHub] sur votre organisation :

1. Cliquez sur **Gérer les référentiels sur Github** au bas de la boîte de dialogue.
2. La page Application [!DNL Adobe LLM Apps Link] [!DNL GitHub] s’ouvre alors dans un nouvel onglet.

   ![Lien vers les applications Adobe LLM — Page d’installation de l’application GitHub](/help/assets/guide-create-app/github-app-install.png)

3. Cliquez sur **[!UICONTROL Installer]** et sélectionnez votre organisation [!DNL GitHub].
4. Sous **[!UICONTROL Accès au référentiel]**, choisissez **Sélectionner uniquement les référentiels** et sélectionnez le référentiel qui hébergera le code de l’application.

   ![Lien vers les applications Adobe LLM — accès au référentiel](/help/assets/guide-create-app/github-repo-access.png)

5. Cliquez sur **[!UICONTROL Enregistrer]**. Revenez à la boîte de dialogue Créer une application : votre référentiel apparaît désormais dans le menu déroulant **Sélectionner le référentiel**.
6. Sélectionnez le référentiel que vous souhaitez utiliser.

![Boîte de dialogue Créer une application — Référentiel lié](/help/assets/guide-create-app/app-details-repo-linked.png)

>[!NOTE]
>
>Vous pouvez ignorer la liaison d’un référentiel lors de la création de l’application et le faire ultérieurement à partir des paramètres de l’application. Cependant, vous ne pouvez pas effectuer de déploiement tant qu’un référentiel n’est pas lié.

## Création de l’application

Cliquez sur **[!UICONTROL Créer une application]**. Un écran de chargement s’affiche lors de la création du projet dans Developer Console.

![Création de l’application — écran de chargement](/help/assets/guide-create-app/app-loading.png)

Une fois l’opération terminée, vous êtes redirigé vers la page **Détails de l’application**.

## Page Détails de l’application

La page Détails de l’application est le hub central pour gérer votre application.

![Page Détails de l’application — Sections supérieures](/help/assets/guide-create-app/app-detail-top.png)

### Bannière d’application

![Bannière d’application](/help/assets/guide-create-app/app-banner.png)

La bannière colorée en haut affiche l’application actuellement sélectionnée, y compris l’avatar, le nom, la description et une liste déroulante pour basculer entre les applications. La bannière reste fixe en haut lorsque vous faites défiler l’écran.

### Titre de la page et actions

![Bannière d’application](/help/assets/guide-create-app/page-title.png)

Sous la bannière, vous voyez le nom de l’application comme en-tête, avec les boutons d’action suivants :

- **...** (autres actions) — Créez une nouvelle application ou supprimez l&#39;application actuelle.
- **[!UICONTROL Paramètres]** — Configurez le référentiel lié et d’autres options.
- **[!UICONTROL Déployer]** — déployez votre application sur [!DNL Adobe I/O Runtime] (désactivé jusqu&#39;à ce qu&#39;un référentiel soit lié).

### Carte d’informations de l’application

![Carte d’informations de l’application](/help/assets/guide-create-app/app-info-card.png)

Cette carte résume les métadonnées clés de votre application : nom, description, badge d’état (**non déployé** ou **déployé**), identifiant de l’application et date de création. Elle affiche également les deux référentiels liés :

- **Référentiel de gestionnaire** — Emplacement du code du gestionnaire d’actions (les fonctions JavaScript sont sur [!DNL Adobe I/O Runtime]).
- **référentiel EDS** — Emplacement de l’interface utilisateur du widget (blocs et styles servis par [!DNL Edge Delivery Services]).

### Actions, test de l’application et historique de déploiement

![Page Détails de l’application — sections inférieures](/help/assets/guide-create-app/app-detail-bottom.png)

Sous la carte d&#39;informations, vous trouverez trois sections :

- **[!UICONTROL Actions]** — répertorie les gestionnaires d&#39;actions définis pour votre application. Cliquez sur **Accéder aux actions** pour accéder à la page Actions.
- **[!UICONTROL Tester l’application]** : après le déploiement, affiche les URL du serveur MCP pour les environnements d’évaluation et de production.
- **Historique de déploiement** — Effectue le suivi de chaque déploiement dans les environnements avec statut et date.

## Étapes suivantes

- [Guide : créer une action](/help/guides/create-action.md) — Définir une action avec des paramètres de métadonnées et de widget.

