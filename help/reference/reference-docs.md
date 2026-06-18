---
title: Documentation de référence pour les applications Adobe LLM
description: Référence au niveau du champ pour la configuration d’action dans l’interface utilisateur des applications Adobe LLM.
source-git-commit: 1a99e2e80e50a3bcf9ce6fb910365202bf06e113
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 6%

---


# Matériau De Référence {#reference-material}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Cette section fournit des références au niveau du champ pour la configuration d’une action dans l’interface utilisateur de [!DNL Adobe LLM Apps].

## Paramètres de l&#39;action

Les paramètres d’entrée sont les valeurs que la plateforme LLM ([!DNL ChatGPT], Claude) envoie à votre gestionnaire d’action. Le modèle les extrait du message de l’utilisateur et les mappe automatiquement à ces champs.

| Propriété | Description |
|----------|-------------|
| **Nom** | Identifiant du paramètre (par exemple, `category`, `query`) |
| **Type** | `String`, `Number`, `Integer` ou `Boolean` |
| **Description** | Une explication lisible par l’utilisateur : la plateforme LLM l’utilise pour extraire la valeur appropriée |
| **Requis** | Si cette case est cochée, le modèle doit fournir ce paramètre avant d’appeler l’action |

### Paramètres de fichier

Les paramètres de fichier transportent des objets de fichier avec des propriétés `download_url` et `file_id`. Définissez des noms de champ d’entrée qui doivent recevoir des données de fichier lorsqu’un utilisateur charge un fichier dans la conversation.

## Champs de métadonnées

### Informations de base

| Champ | Requis | Description |
|-------|----------|-------------|
| **Nom de l’action** | Oui | Identifiant de l’action (par exemple, *Rechercher des produits*) |
| **Description** | Oui | Explication de la fonction de l’action : la plateforme LLM l’utilise pour décider quand l’appeler |

### Annotations

Conseils facultatifs qui décrivent le comportement de l’action :

| Annotation | Description |
|------------|-------------|
| **Indice destructif** | L’action modifie ou supprime des données |
| **Idempotent** | Appeler plusieurs fois l’action avec les mêmes arguments aboutit au même résultat |
| **Indice d’ouverture** | L’action interagit avec des systèmes externes. |
| **Conseil en lecture seule** | L’action lit uniquement des données, mais n’écrit jamais |

### Métadonnées OpenAI

| Champ | Longueur maximale | Description |
|-------|------------|-------------|
| **Appeler le texte du statut** | 64 caractères | Message affiché dans la plateforme LLM pendant l’exécution de l’action (par exemple, *Chargement des produits ...* ) |
| **Texte du statut appelé** | 64 caractères | Message affiché une fois l’action terminée (par exemple, *Produits chargés ...* ) |

### Visibilité

| Activer/désactiver | Description |
|--------|-------------|
| **Exposition au modèle d’IA** | L’action peut être appelée par le modèle d’IA lors des conversations |
| **Afficher en tant que widget dans la surface de l’application** | L’action génère un widget visuel dans l’application |

### Informations sur le widget

| Champ | Description |
|-------|-------------|
| **Type** | Technologie des widgets — actuellement **[!UICONTROL EDS]** |
| **Domaine du widget (origine du sandbox)** | Origine de l’hébergement du widget ; doit être unique par application |
| **Bordure préférée** | Si cette case est cochée, le widget s’affiche dans une carte avec bordure sur la plateforme LLM |

### URL du modèle

| Champ | Description |
|-------|-------------|
| **[!UICONTROL URL du script]** | Script du point d&#39;entrée — `https://main--<repo>--<owner>.aem.live/scripts/aem-embed.js`. Partagé entre toutes les actions |
| **URL intégrée du widget** | Page EDS pour cette action — `https://main--<repo>--<owner>.aem.live/eds-widgets/<action-name>`. Unique par action |

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

