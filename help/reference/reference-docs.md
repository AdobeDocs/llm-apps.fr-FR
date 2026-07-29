---
title: Champs d’action et de widget
description: Définitions de champ pour les métadonnées d’action, les paramètres, les widgets, les CSP et les autorisations dans les applications Adobe LLM.
source-git-commit: eec74b87457bc852d7a8dd0e46c2a4385a93ae0a
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 5%

---


# Champs d’action et de widget {#action-widget-configuration}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Utilisez cette page pour rechercher des champs dans l’éditeur d’actions. Pour obtenir le parcours de création complet, voir [Créer une action à partir de zéro](/help/guides/create-action.md).

## Paramètres de l&#39;action

Les paramètres d’entrée sont les valeurs que la plateforme LLM envoie à votre gestionnaire d’actions. Le modèle les extrait du message de l’utilisateur et les mappe à ces champs.

| Propriété | Description |
|----------|-------------|
| **Nom** | Identifiant du paramètre (par exemple, `category`, `query`) |
| **Type** | `String`, `Number`, `Integer` ou `Boolean` |
| **Description** | Une explication lisible par l’utilisateur : la plateforme LLM l’utilise pour extraire la valeur appropriée |
| **Requis** | Si cette case est cochée, le modèle doit fournir ce paramètre avant d’appeler l’action |

### Paramètres de fichier

Les paramètres de fichier sont des noms de champ d’entrée configurés dans l’éditeur d’actions. Lorsqu’un utilisateur charge un fichier, l’hôte fournit un objet de fichier pour ces arguments, généralement `download_url` et `file_id`.

## Champs de métadonnées

### Informations de base

| Field (Champ) | Requis | Description |
|-------|----------|-------------|
| **Nom de l’action** | Oui | Nom d’affichage de l’action (par exemple, *Rechercher des produits*) |
| **Description** | Oui | Explication de la fonction de l’action : la plateforme LLM l’utilise pour décider quand l’appeler |

Après la création, l’éditeur affiche également un **identifiant de code** non modifiable. Il mappe l’action à `actions/<code-identifier>/index.js` dans le référentiel du gestionnaire.

### Annotations

Conseils facultatifs qui décrivent le comportement de l’action :

| Annotation | Description |
|------------|-------------|
| **Indice destructif** | L’action modifie ou supprime des données |
| **Idempotent** | Appeler plusieurs fois l’action avec les mêmes arguments aboutit au même résultat |
| **Indice d’ouverture** | L’action interagit avec des systèmes externes. |
| **Conseil en lecture seule** | L’action lit uniquement des données, mais n’écrit jamais |

### Métadonnées OpenAI

| Field (Champ) | Longueur maximale | Description |
|-------|------------|-------------|
| **Appeler le texte du statut** | 64 caractères | Message affiché dans la plateforme LLM pendant l’exécution de l’action (par exemple, *Chargement des produits ...* ) |
| **Texte du statut appelé** | 64 caractères | Message affiché une fois l’action terminée (par exemple, *Produits chargés ...* ) |
| **Description du widget** | 512 caractères | Associe à `_meta["openai/widgetDescription"]` ; résume le composant rendu pour le modèle et réduit la narration répétée. |

La description de l’action détermine à quel moment le modèle sélectionne l’action. La description du widget explique ce que le composant affiche après son rendu.

### Visibilité

| Activer/désactiver | Description |
|--------|-------------|
| **Exposition au modèle d’IA** | L’action peut être appelée par le modèle d’IA lors des conversations |
| **Afficher en tant que widget dans la surface de l’application** | L’action génère un widget visuel dans l’application |

### Analytics

| Champ | Description |
|-------|-------------|
| **Collecter les intentions des utilisateurs** | Collecte un résumé de la conversation qui a conduit à l’action pour Analytics |

## Champs de widget

### Informations sur le widget

| Champ | Description |
|-------|-------------|
| **Type** | Technologie des widgets — actuellement **[!UICONTROL EDS]** |
| **Domaine du widget (origine du sandbox)** | Origine de l’hébergement du widget ; doit être unique par application |
| **Bordure préférée** | Si cette case est cochée, le widget s’affiche dans une carte avec bordure sur la plateforme LLM |

### URL du modèle

| Champ | Description |
|-------|-------------|
| **[!UICONTROL URL du script]** | URL HTTPS du point d’entrée EDS `scripts/aem-embed.js`. Partagé entre les actions d’un même projet EDS |
| **URL du widget** | URL HTTPS de la page EDS générée par cette action. Les actions générées le configurent automatiquement |

## Configuration de CSP

La politique de sécurité du contenu contrôle les domaines externes avec lesquels l’iframe du widget peut entrer en contact. Chaque domaine externe doit être explicitement sélectionné.

| Directive | Description |
|-----------|-------------|
| **Domaines de ressources** | Domaines pour les ressources statiques (images, polices, scripts, styles) |
| **Connecter des domaines** | Domaines avec lesquels le widget peut communiquer via `fetch`, `XHR` ou `WebSocket` |
| **Domaines de trame** | Origines autorisées pour les iFrames imbriquées ; déclenche une révision plus stricte des applications |
| **Domaines de redirection** | Cibles approuvées pour les liens de redirection `openExternal` (spécifiques à [!DNL ChatGPT]) |
| **Domaines URI de base** | La directive CSP `base-uri` (MCP Apps SDK uniquement, pas [!DNL ChatGPT]) |

## Autorisations

API matérielles et de navigateur auxquelles le widget peut accéder. Ils correspondent à la politique d’autorisation de l’iframe.

| Autorisation | Description |
|------------|-------------|
| **Appareil photo** | Accéder à la caméra de l’appareil |
| **Microphone** | Accéder au microphone de l’appareil |
| **Géolocalisation** | Accéder à l’emplacement de l’utilisateur |
| **Presse-papiers** | Lecture ou écriture dans le presse-papiers |

