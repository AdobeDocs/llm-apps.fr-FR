---
title: Champs d’action et de widget
description: Définitions de champ pour les métadonnées d’action, les paramètres, les widgets, les CSP et les autorisations dans les applications Adobe LLM.
source-git-commit: 0276ae4e4627e49157911ad579a42c0e77045eee
workflow-type: tm+mt
source-wordcount: '1173'
ht-degree: 3%
---

# Champs d’action et de widget {#action-widget-configuration}

>[!IMPORTANT]
>
>Les applications LLM sont disponibles pour tous les clients AEM Sites Cloud Service afin qu’ils puissent développer et gérer leurs propres applications. Une fois le comptage commencé, chaque appel d’application est comptabilisé dans les demandes de contenu AEM Sites du client. Adobe annoncera le taux de contrôle et la date de début avant que le contrôle ne prenne effet.

Utilisez cette page pour rechercher des champs dans l’éditeur d’actions. Pour connaître les étapes de création, voir [Créer une action à partir de zéro](/help/guides/create-action.md).

## Rechercher les paramètres

La boîte de dialogue **[!UICONTROL Créer une action]** et l’éditeur d’actions enregistrées utilisent les quatre mêmes onglets :

| Tabulation | Paramètres |
|---|---|
| **[!UICONTROL Détails de l&#39;action]** | Nom de l’action, identifiant de code, description, paramètres d’entrée et **[!UICONTROL paramètres LLM]**, y compris les annotations, le texte du statut d’appel et la visibilité |
| **[!UICONTROL Détails du widget LLM]** | Type de widget, domaine de sandbox, description du widget, URL du modèle et CSP |
| **[!UICONTROL Informations sur l’application]** | Collecte des intentions des utilisateurs et instructions |
| **[!UICONTROL Extensions ChatGPT]** | Points d’entrée à ouverture directe, icônes d’action, préférences d’affichage et mentions du compositeur |

Pour enregistrer vos modifications, sélectionnez **[!UICONTROL Enregistrer les modifications]**. Les actions générées en attente de révision affichent plutôt **[!UICONTROL Marquer comme révisée]**, ce qui fusionne également leurs demandes ou branches d’extraction générées.

## Paramètres de l&#39;action

Les paramètres d’entrée sont les valeurs que la plateforme LLM envoie à votre gestionnaire d’actions. Le modèle extrait les valeurs du message de l’utilisateur et les mappe aux paramètres configurés.

| Propriété | Description |
|----------|-------------|
| **Nom** | Identifiant du paramètre (par exemple, `category`, `query`) |
| **Type** | `String`, `Number`, `Integer`, `Boolean`, `Object` ou `File` pour les paramètres de fichier chargé |
| **Description** | Une explication lisible par l’utilisateur : la plateforme LLM utilise l’explication pour extraire la valeur appropriée |
| **Requis** | Si cette case est cochée, le modèle doit fournir ce paramètre avant d’appeler l’action |

### Paramètres de fichier

Les paramètres de fichier sont des paramètres d’entrée de type `File`. Lorsqu’un utilisateur charge un fichier, l’hôte fournit un objet de fichier pour ces arguments, comprenant généralement `download_url` et `file_id`.

Les paramètres du fichier chargé sont différents des [points d&#39;entrée de fichier](/help/guides/chatgpt-extensions.md#file-entrypoint). Un point d’entrée de fichier utilise un paramètre d’objet nommé `file`, contenant les éléments `name` et `resourceUri` du fichier ouvert.

## Champs de métadonnées

### Informations de base

| Champ | Requis | Description |
|-------|----------|-------------|
| **Nom de l&#39;action** | Oui | Nom d’affichage de l’action (par exemple, *Rechercher des produits*) |
| **Description** | Oui | Explication de la fonction de l’action : la plateforme LLM utilise l’explication pour décider quand appeler l’action |

La boîte de dialogue de création prévisualise un **identifiant de code** non modifiable dérivé du nom de l’action. Il est unique dans l’application et contient jusqu’à 63 caractères, en commençant par une lettre minuscule et en utilisant des lettres minuscules, des chiffres, des tirets ou des traits de soulignement. Il mappe l’action sur `actions/<code-identifier>/index.js` dans le référentiel du gestionnaire.

### Annotations

Conseils facultatifs qui décrivent le comportement de l’action :

| Annotation | Description |
|------------|-------------|
| **Indice destructeur** | L’action peut supprimer ou remplacer des données |
| **Idempotent (mêmes arguments = aucun effet supplémentaire)** | La répétition d’une même requête n’a aucun effet supplémentaire sur l’environnement ; les requêtes répétées ne renvoient pas nécessairement des données identiques |
| **Ouvrir l&#39;indice du monde** | L’action interagit avec des systèmes externes. |
| **Conseil en lecture seule** | L’action lit uniquement des données, mais n’écrit jamais |

### Métadonnées OpenAI

| Champ | Longueur maximale | Description |
|-------|------------|-------------|
| **Appeler le texte du statut** | 64 caractères | Message affiché dans la plateforme LLM pendant l’exécution de l’action (par exemple, *Chargement des produits ...*) |
| **Texte du statut appelé** | 64 caractères | Message affiché une fois l’action terminée (par exemple, *Produits chargés ...*) |

### Visibilité

Pour activer ou désactiver une action, utilisez son bouton **[!UICONTROL Visibilité]** sur la page de l&#39;application. Le commutateur contrôle si le prochain déploiement inclut l’action . L’éditeur d’actions n’a pas de bouton marche/arrêt distinct.

Les paramètres de l’éditeur d’action suivants contrôlent l’exposition du modèle et du widget, et non l’état activé/désactivé au niveau de l’application :

| Activer/désactiver | Description |
|--------|-------------|
| **Exposer au modèle d’IA** | L’action peut être appelée par le modèle d’IA lors des conversations |
| **Afficher en tant que widget dans la surface d’application** | L’action génère un widget visuel dans l’application |

Les paramètres d’exposition du modèle et du widget ne remplacent pas le choix d’un type de widget.

### Analytics

| Champ | Description |
|-------|-------------|
| **Collecter l’intention de l’utilisateur** | Activé par défaut. Demande un résumé anonyme de l’objectif qui a conduit à l’action pour Analytics |
| **Description de l’intention de l’utilisateur** | Instructions utilisées pour demander le résumé, jusqu’à 2 000 caractères. La valeur par défaut exclut les informations personnelles et sensibles. Vérifiez toute personnalisation par rapport à votre politique de confidentialité. |

## Champs de widget

### Informations sur le widget

| Champ | Description |
|-------|-------------|
| **Type de widget** | **[!UICONTROL Bloc Edge Delivery Services]** pour un widget interactif ou **[!UICONTROL Pas d’IU interactive]** pour une action en texte seul |
| **Domaine du widget** | Origine du sandbox utilisé par l’hôte. L’interface utilisateur l’identifie comme requis pour l’envoi de l’application et comme unique par application. Ceci est distinct de l’URL du **widget** de la page EDS |
| **Description du widget** | Jusqu’à 512 caractères. Mappe sur `_meta["openai/widgetDescription"]` ; résume le composant rendu pour le modèle et réduit la narration répétée. La description de l’action détermine à quel moment le modèle sélectionne l’action ; la description du widget explique ce que le composant affiche après son rendu |

### URL du modèle

| Champ | Description |
|-------|-------------|
| **[!UICONTROL URL du script]** | URL HTTPS pour le point d’entrée EDS `scripts/aem-embed.js`. Partagé entre les actions d’un même projet EDS |
| **URL du widget** | URL HTTPS de la page EDS générée par l’action. Les actions générées configurent automatiquement l’URL. |

## Configuration de CSP

La politique de sécurité du contenu contrôle les domaines externes avec lesquels l’iframe du widget peut entrer en contact. Chaque domaine externe doit être explicitement sélectionné.

| Directive | Description |
|-----------|-------------|
| **Domaines de ressources** | Domaines pour les ressources statiques (images, polices, scripts, styles) |
| **Connecter des domaines** | Domaines que le widget peut contacter via `fetch`, `XHR` ou `WebSocket` |
| **Domaines de trame** | Origines autorisées pour les iFrames imbriquées ; déclenche une révision plus stricte des applications |
| **Domaines de redirection** | Cibles de confiance pour `openExternal` liens de redirection ([!DNL ChatGPT]-spécifiques) |
| **Domaines URI de base** | La directive CSP `base-uri` (MCP Apps SDK uniquement, et non [!DNL ChatGPT]) |

## Champs d’extension ChatGPT {#chatgpt-extension-fields}

Ces paramètres facultatifs sont uniquement disponibles pour le ChatGPT ; ils ne configurent pas Claude ni d&#39;autres plateformes LLM. La disponibilité varie en fonction du client ChatGPT. Pour connaître les exigences et les étapes, voir [Configurer les extensions de ChatGPT](/help/guides/chatgpt-extensions.md).

### Points d’entrée et icônes d’action

Les points d’entrée nécessitent un widget. Sélectionnez une combinaison de global, de thread et de fichier pour le même widget ou laissez-la décochée. Global et Thread appellent l’action sans entrées ; le fichier fournit uniquement l’objet `file`. D’autres entrées requises empêchent le point d’entrée correspondant de fonctionner. Si Fichier est combiné avec Global ou Thread, `file` doit être facultatif.

| Champ | Description |
|---|---|
| **Où les utilisateurs peuvent ouvrir cette application** | Cases à cocher pour Global (plein écran depuis la navigation), Thread (un onglet de contenu côté conversation) et Fichier (une visionneuse personnalisée pour les extensions de fichier correspondantes). Plusieurs types peuvent être sélectionnés |
| **Icône URL** | Icône d’action facultative partagée par ses points d’entrée. Accepte une URL HTTP(S) ou un URI de données ; préfère une URL d’image HTTPS publique. La modification ou l’effacement de l’URL remplace les variantes d’icône enregistrées |
| **Extensions de fichier** | Au moins une extension, telle que `.csv`, pour un point d’entrée de fichier. L’éditeur ajoute un point de début lorsqu’il est omis |

La sélection de Fichier ajoute une entrée d’objet facultative nommée `file` si elle est manquante. Les définitions existantes sont conservées et doivent accepter les `name` et `resourceUri` du fichier ouvert.

### Préférences d’affichage

Ces paramètres de widget sont distincts des points d’entrée. L’hôte choisit la présentation finale.

| Champ | Choix et comportement par défaut |
|---|---|
| **Afficher lors du lancement par le modèle** | `inline`, `fullscreen` ou **Aucune préférence** |
| **Modes d’affichage des ressources pris en charge** | **En ligne**, **Plein écran** ou les deux. Aucune sélection ne laisse les paramètres par défaut de l’hôte en vigueur |
| **Affichage initial des ressources** | **Hôte par défaut**, **Inline** ou **Plein écran**. Lorsque les modes pris en charge sont sélectionnés, le mode initial doit être inclus |

### Mentions du compositeur

**Activer les mentions du compositeur** désigne l&#39;action de recherche de ressources de l&#39;application. Une seule action par application peut fournir la recherche de mention. L’action doit être en lecture seule et visible par l’application, avec un paramètre de chaîne nommé `query` qui accepte du texte vide et aucune autre entrée requise. Un fournisseur peut être en mode texte uniquement ; l’activation de la visibilité de l’application ne nécessite pas la configuration d’un widget pour le fournisseur de recherche.
