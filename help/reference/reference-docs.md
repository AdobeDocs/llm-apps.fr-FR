---
title: Champs d’action et de widget
description: Définitions de champ pour les métadonnées d’action, les paramètres, les widgets, les CSP et les autorisations dans les applications Adobe LLM.
source-git-commit: a48b4022979d9a13b4fc3aa870c4ee6375e8bc13
workflow-type: tm+mt
source-wordcount: '890'
ht-degree: 4%
---

# Champs d’action et de widget {#action-widget-configuration}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Utilisez cette page pour rechercher des champs dans l’éditeur d’actions. Pour connaître les étapes de création, voir [Créer entièrement une action](/help/guides/create-action.md).

## Rechercher les paramètres

La boîte de dialogue **[!UICONTROL Créer une action]** comporte des onglets **[!UICONTROL Action]** et **[!UICONTROL Métadonnées de widget]**.

Après la création, l’éditeur d’action enregistrée utilise trois onglets :

| Tabulation | Paramètres |
|---|---|
| **[!UICONTROL Détails du widget LLM]** | Type de widget, domaine de sandbox, description du widget, URL du modèle et CSP |
| **[!UICONTROL Détails de l’action]** | Identifiant du code, description, paramètres d’entrée et **[!UICONTROL paramètres LLM]** y compris les annotations, le texte du statut d’appel et la visibilité |
| **[!UICONTROL Analytics]** | Collecte des intentions des utilisateurs et instructions |

Pour enregistrer vos modifications, sélectionnez **[!UICONTROL Enregistrer les modifications]**. Les actions générées en attente de révision affichent plutôt **[!UICONTROL Marquer comme révisé]**, qui gère également leurs demandes d’extraction générées.

## Paramètres de l&#39;action

Les paramètres d’entrée sont les valeurs que la plateforme LLM envoie à votre gestionnaire d’actions. Le modèle extrait les valeurs du message de l’utilisateur et les mappe aux paramètres configurés.

| Propriété | Description |
|----------|-------------|
| **Nom** | Identifiant du paramètre (par exemple, `category`, `query`) |
| **Type** | `String`, `Number`, `Integer` ou `Boolean` |
| **Description** | Une explication lisible par l’utilisateur : la plateforme LLM utilise l’explication pour extraire la valeur appropriée |
| **Requis** | Si cette case est cochée, le modèle doit fournir ce paramètre avant d’appeler l’action |

### Paramètres de fichier

Les paramètres de fichier sont des noms de champ d’entrée configurés dans l’éditeur d’actions. Lorsqu’un utilisateur charge un fichier, l’hôte fournit un objet de fichier pour ces arguments, généralement `download_url` et `file_id`.

## Champs de métadonnées

### Informations de base

| Champ | Requis | Description |
|-------|----------|-------------|
| **Nom de l’action** | Oui | Nom d’affichage de l’action (par exemple, *Rechercher des produits*) |
| **Description** | Oui | Explication de la fonction de l’action : la plateforme LLM utilise l’explication pour décider quand appeler l’action |

La boîte de dialogue de création prévisualise un **identifiant de code** non modifiable dérivé du nom de l’action. Il est unique dans l’application et contient jusqu’à 63 caractères, en commençant par une lettre minuscule et en utilisant des lettres minuscules, des chiffres, des tirets ou des traits de soulignement. Il mappe l’action à `actions/<code-identifier>/index.js` dans le référentiel du gestionnaire.

### Annotations

Conseils facultatifs qui décrivent le comportement de l’action :

| Annotation | Description |
|------------|-------------|
| **Indice destructif** | L’action peut supprimer ou remplacer des données |
| **Idempotent (mêmes arguments = aucun effet supplémentaire)** | La répétition d’une même requête n’a aucun effet supplémentaire sur l’environnement ; les requêtes répétées ne renvoient pas nécessairement des données identiques |
| **Indice d’ouverture** | L’action interagit avec des systèmes externes. |
| **Conseil en lecture seule** | L’action lit uniquement des données, mais n’écrit jamais |

### Métadonnées OpenAI

| Champ | Longueur maximale | Description |
|-------|------------|-------------|
| **Appeler le texte du statut** | 64 caractères | Message affiché dans la plateforme LLM pendant l’exécution de l’action (par exemple, *Chargement des produits ...*) |
| **Texte du statut appelé** | 64 caractères | Message affiché une fois l’action terminée (par exemple, *Produits chargés ...*) |
| **Description du widget** | 512 caractères | Associe à `_meta["openai/widgetDescription"]` ; résume le composant rendu pour le modèle et réduit la narration répétée. |

La description de l’action détermine à quel moment le modèle sélectionne l’action. La description du widget explique ce que le composant affiche après son rendu.

### Visibilité

Pour activer ou désactiver une action, utilisez son bouton **[!UICONTROL Visibilité]** sur la page de l’application. Le commutateur contrôle si le prochain déploiement inclut l’action . L’éditeur d’actions n’a pas de bouton marche/arrêt distinct.

Les paramètres de l’éditeur d’action suivants contrôlent l’exposition du modèle et du widget, et non l’état activé/désactivé au niveau de l’application :

| Activer/désactiver | Description |
|--------|-------------|
| **Exposition au modèle d’IA** | L’action peut être appelée par le modèle d’IA lors des conversations |
| **Afficher en tant que widget dans la surface de l’application** | L’action génère un widget visuel dans l’application |

Les paramètres d’exposition du modèle et du widget ne remplacent pas le choix d’un type de widget.

### Analytics

| Champ | Description |
|-------|-------------|
| **Collecter les intentions des utilisateurs** | Activé par défaut. Demande un résumé anonyme de l’objectif qui a conduit à l’action pour Analytics |
| **Description du mode d’utilisation** | Instructions utilisées pour demander le résumé, jusqu’à 2 000 caractères. La valeur par défaut exclut les informations personnelles et sensibles. Vérifiez toute personnalisation par rapport à votre politique de confidentialité. |

## Champs de widget

### Informations sur le widget

| Champ | Description |
|-------|-------------|
| **Type / Type de widget** | Dans la boîte de dialogue de création, sélectionnez **[!UICONTROL Bloc]** ou **[!UICONTROL Aucun widget]**. L’éditeur enregistré étiquette le choix texte uniquement **[!UICONTROL Aucune interface utilisateur interactive]** |
| **Domaine du widget** | Origine du sandbox utilisé par l’hôte. L’interface utilisateur l’identifie comme requis pour l’envoi de l’application et comme unique par application. Elle est distincte de l’**URL du widget** de la page EDS |
| **Bordure préférée** | Demande un conteneur bordé ; le rendu dépend de l’hôte. Disponible dans la boîte de dialogue de création |

### URL du modèle

| Champ | Description |
|-------|-------------|
| **[!UICONTROL URL du script]** | URL HTTPS du point d’entrée EDS `scripts/aem-embed.js`. Partagé entre les actions d’un même projet EDS |
| **URL du widget** | URL HTTPS de la page EDS générée par l’action. Les actions générées configurent automatiquement l’URL. |

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

Les autorisations de widget régissent l’accès aux API matérielles et de navigateur. Les autorisations sont mappées à la politique d’autorisation de l’iframe.

Les contrôles des autorisations apparaissent sous **[!UICONTROL Autorisations (spécifiques au MCP)]** dans la boîte de dialogue de création. N’accordez que les fonctionnalités requises par le widget ; la prise en charge de l’hôte et le consentement de l’utilisateur s’appliquent également.

| Autorisation | Description |
|------------|-------------|
| **Appareil photo** | Accéder à la caméra de l’appareil |
| **Microphone** | Accéder au microphone de l’appareil |
| **Géolocalisation** | Accéder à l’emplacement de l’utilisateur |
| **Presse-papiers** | Lecture ou écriture dans le presse-papiers |
