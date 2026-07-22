---
title: Création d’une action
description: Découvrez comment définir une action dans l’interface utilisateur des applications LLM, y compris les métadonnées, les paramètres d’entrée et la configuration de widget.
source-git-commit: ae2748319b5401555c3a616971f5697c17e74ac3
workflow-type: tm+mt
source-wordcount: '900'
ht-degree: 1%

---


# Création d’une action

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Ce guide vous guide tout au long de la définition d’une action dans l’interface utilisateur [!DNL LLM Apps]. Pour en savoir plus sur les actions et leur fonctionnement, voir [&#x200B; Concepts de base &#x200B;](/help/overview/overview.md#actions).

## Ouvrez la page Actions .

Accédez à **[!UICONTROL Actions]** dans la barre latérale gauche, ou cliquez sur **Accéder aux actions** dans la page Détails de l’application. Si aucune action n’existe à ce jour, la page affiche un état vide.

![Page Actions — Aucune action pour le moment](/help/assets/guide-create-action/actions-empty.png)

Cliquez sur **+ Créer une action** pour ouvrir la boîte de dialogue plein écran.

## Cartes d’action

Chaque action s’affiche sous la forme d’une carte qui indique :

- Action **nom** et **description**
- Une **image d’aperçu de widget** — générée automatiquement à partir du widget, montrant à quoi ressemble la sortie d’action dans la plateforme LLM
- **Badges** : type de widget (**[!UICONTROL EDS]**), statut du déploiement (**Non déployé**, **Déployé dans l’évaluation**, **Déployé en production**), **Modifications non déployées** lorsque l’action a été modifiée depuis le dernier déploiement et nombre de paramètres
- Un bouton **Visibilité** permet d’activer ou de désactiver l’action sur le point d’entrée actif sans redéploiement
- Un lien **Réviser** dans le coin supérieur droit pour ouvrir l’éditeur d’actions.

![Page Actions — Cartes d’action](/help/assets/guide-create-action/action-card.png)

Lorsqu’une ou plusieurs actions ont été modifiées depuis le dernier déploiement, une bannière **Déploiement nécessaire** s’affiche en haut de la page Actions. Redéployez l’application pour appliquer les modifications.

## Onglet Action

La boîte de dialogue comporte deux onglets : **Action** et **[!UICONTROL Métadonnées de widget]**.

### Informations de base

![Créer une action — informations de base](/help/assets/guide-create-action/action-basic-info.png)

- **Nom de l’action** (obligatoire) : identifiant de l’action (par exemple, *Rechercher des produits*).
- **Description** (obligatoire) : explication claire de l’action. La plateforme LLM l’utilise pour décider quand appeler votre action. Par exemple : *Rechercher le catalogue de produits par mot-clé. Renvoie les produits correspondants avec le nom, la catégorie, l’image et le prix.*
- **Annotations** — conseils facultatifs qui décrivent le comportement de l’action :

  | Annotation | Description |
  |-----------|-------------|
  | **Indice destructif** | L’action modifie ou supprime des données |
  | **Idempotent** | Appeler plusieurs fois l’action avec les mêmes arguments produit le même résultat |
  | **Indice d’ouverture** | L’action interagit avec des systèmes externes. |
  | **Conseil en lecture seule** | L’action lit uniquement des données, mais n’écrit jamais |

  Voir [Référence : champs de métadonnées](/help/reference/reference-docs.md) pour plus d’informations.

### Métadonnées OpenAI

- **Invoquer le texte du statut** — Message affiché sur la plateforme LLM pendant l’exécution de l’action (64 caractères maximum). Exemple : *Chargement des produits...*
- **Texte du statut appelé** — Message affiché une fois l’action terminée (64 caractères maximum). Exemple : *Products loaded.*

### Visibilité et paramètres d’entrée

**Visibilité** contrôle l’emplacement de l’action :

- **Exposer au modèle d’IA** — l’action peut être appelée par le modèle d’IA.
- **Afficher en tant que widget dans la surface de l’application** — l’action effectue le rendu d’un widget visuel.

Les **paramètres d’entrée** sont les valeurs que la plateforme LLM envoie à votre gestionnaire. Le modèle les extrait automatiquement du message de l’utilisateur. Pour *Rechercher des produits* nous définissons :

- **category** (chaîne, facultatif) : filtre de catégorie permettant de limiter les résultats (type de produit ou service, par exemple).
- **query** (chaîne, facultatif) — terme de recherche en texte libre.

Chaque paramètre possède une case à cocher **Nom**, **Type** (chaîne, nombre, entier, booléen), **Description** et **Obligatoire**. Cliquez sur **+ Ajouter** pour ajouter d’autres paramètres.

Pour plus d’informations, voir [Référence : paramètres d’action](/help/reference/reference-docs.md).

### Analytics

![Créer une action : intention de l’utilisateur d’Analytics](/help/assets/guide-create-action/action-analytics-user-intent.png)

- **Intention de l’utilisateur** — lorsqu’il est activé, [!DNL ChatGPT] est invité à résumer la conversation qui a conduit à l’appel de cette action. Ce résumé est collecté et affiché dans les analyses, vous donnant ainsi un aperçu de ce que les utilisateurs tentaient d’accomplir au moment du déclenchement de l’action.

## Onglet Métadonnées du widget

Cet onglet configure la manière dont la réponse visuelle de l’action est rendue dans la plateforme LLM. Pour une explication complète du fonctionnement des widgets, voir [Guide : Configuration du widget (EDS)](/help/guides/widgets.md).

![Créer une action — métadonnées de widget](/help/assets/guide-create-action/widget-metadata.png)

### Informations sur le widget

- **Type** — la technologie de widget (actuellement **[!UICONTROL EDS]**).
- **Domaine du widget (origine du sandbox)** — origine où votre widget est hébergé. Obligatoire pour l’envoi de l’application à OpenAI ; doit être unique par application.
- **Bordure préférée** : effectue le rendu du widget dans une carte avec bordure.

### URL du modèle

- **[!UICONTROL URL du script]** : point d’entrée qui amorce le widget, partagé dans toutes les actions :
  `https://main--<repo>--<owner>.aem.live/scripts/aem-embed.js`
- **URL incorporée du widget** — Page EDS pour cette action spécifique :
  `https://main--<repo>--<owner>.aem.live/eds-widgets/<action-name>`

### Autorisations

API matérielles et de navigateur auxquelles le widget peut accéder :

| Autorisation | Description |
|-----------|-------------|
| **Appareil photo** | Accéder à la caméra de l’appareil |
| **Microphone** | Accéder au microphone de l’appareil |
| **Géolocalisation** | Accéder à l’emplacement de l’utilisateur |
| **Presse-papiers** | Lecture ou écriture dans le presse-papiers |

### Configuration de CSP

![Créer une action : autorisations et CSP](/help/assets/guide-create-action/widget-permissions-csp.png)

Contrôle les domaines externes que l’iframe du widget peut contacter. Chaque domaine externe doit être explicitement sélectionné.

| Directive | Description |
|-----------|-------------|
| **Domaines de ressources** | Domaines pour les ressources statiques (images, polices, scripts, styles) |
| **Connecter des domaines** | Domaines avec lesquels le widget peut communiquer via `fetch`, `XHR` ou `WebSocket` |
| **Domaines de trame** | Origines autorisées pour les iFrames imbriquées ; l’ajout d’entrées déclenche une révision plus stricte de l’application depuis OpenAI |
| **Domaines de redirection** | Cibles approuvées pour les liens de redirection `openExternal` (spécifiques à [!DNL ChatGPT]) |
| **Domaines URI de base** | La directive CSP `base-uri` (applications MCP SDK uniquement, non prises en charge par [!DNL ChatGPT]) |

Cliquez sur **Créer une action** pour enregistrer.

## Après avoir créé une action

Votre action s’affiche sous forme de vignette sur la page Actions :

![Page Actions — Action créée](/help/assets/guide-create-action/actions-with-action.png)

Chaque carte affiche le nom de l’action, sa description, le badge de type (**[!UICONTROL EDS]**), le statut de déploiement (**Non déployé**) et le nombre de paramètres. Vous pouvez cliquer sur **...** pour modifier ou supprimer, ou cliquer sur **Vérifier** pour examiner la configuration.

![Détails de l’application — non déployés](/help/assets/guide-create-action/app-detail-not-deployed.png)

Les métadonnées de l’action sont enregistrées, mais aucun code n’a encore été déployé. Pour rendre l’action fonctionnelle, vous devez :

1. **Configurer le widget EDS** — Voir [Guide : Configurer le widget (EDS)](/help/guides/widgets.md).
2. **Écrire le gestionnaire** — voir [Guide : Écrire le gestionnaire d&#39;actions](/help/guides/write-action-handler.md).
3. **[!UICONTROL Déployer]** — Voir [Guide : Déployer votre application](/help/guides/deploy-your-app.md).

## Étapes suivantes

- [Guide : configuration du widget (EDS)](/help/guides/widgets.md)
