---
title: Présentation des applications Adobe LLM
description: Découvrez ce qu’est une application Adobe LLM, son fonctionnement et comment commencer à la créer.
source-git-commit: a48b4022979d9a13b4fc3aa870c4ee6375e8bc13
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 1%
---

# Applications Adobe LLM - Aperçu {#adobe-llm-apps-an-overview}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

## Qu’est-ce qu’[!DNL Adobe LLM Apps] ?

[!DNL Adobe LLM Apps] permet à votre marque de proposer des actions utiles, telles que la découverte de produits, les contrôles de disponibilité ou les réservations de services, dans les assistants d’IA pris en charge, tels que [!DNL ChatGPT] et [!DNL Claude].

[!DNL LLM Apps] est disponible à l’adresse [experience.adobe.com/llm-apps](https://experience.adobe.com/llm-apps).

## Ce que vous pouvez faire avec [!DNL LLM Apps]

- **Créer des actions LLM de marque** — Définissez les flux métier spécifiques que vous souhaitez activer dans les assistants d’IA (par exemple, *Planifier un essai routier*, *Comparer des produits*, *Réserver un service*).
- **Créer des widgets LLM interactifs** — Créez des composants visuels d’interface utilisateur (cartes produit, formulaires de réservation, localisateurs de magasin) gérés en tant que composants AEM dans votre référentiel de [!DNL GitHub].
- **Maintenir une gouvernance de marque centralisée** — Les auteurs et les développeurs conservent un contrôle total sur l’ensemble du contenu, des copies et des visuels exposés dans la plateforme LLM, les approbations étant gérées via AEM.
- **Déploiement vers l’environnement d’évaluation et de production** — Un pipeline de déploiement contrôlé vous permet de tester l’expérience dans un environnement d’évaluation avant de passer à la production.
- **Contrôler la visibilité au niveau des actions** — Activez ou désactivez les actions individuelles, puis déployez l&#39;application pour appliquer la modification.
- **Mesurer ce qui motive les décisions** — Analyses intégrées (optimisées par Adobe Customer Journey Analytics) le nombre de déclencheurs d’action de surface, les taux de succès, les taux d’abandon, les principales invites d’utilisateur et les scores de visibilité.

## Pourquoi [!DNL LLM Apps] important

Les interactions LLM sont fondamentalement différentes de la recherche traditionnelle. La durée moyenne d’une session LLM est quatre fois plus longue qu’une session de recherche traditionnelle. Plus de 40 % des consommateurs utilisent des outils d’IA pour prendre des décisions d’achat complexes. Sans [!DNL LLM Apps], vous pourriez gagner la mention mais perdre le client. [!DNL LLM Apps] garantit que votre marque est non seulement visible, mais aussi exploitable au moment précis où un utilisateur est prêt à prendre une décision.

## Concepts clés {#key-concepts}

### Application LLM

Votre assistant de marque avec lequel les utilisateurs interagissent au sein de [!DNL ChatGPT] ou d’autres plateformes LLM. Il regroupe toutes vos actions et déploie en une seule unité.

### Action {#actions}

Une fonctionnalité proposée par votre application, telle que *Trouver un distributeur* ou *Parcourir les produits*. La plateforme LLM appelle une action lorsqu’une requête correspond à sa description. [!DNL LLM Apps] gère les métadonnées d’action et le référentiel [!DNL GitHub] stocke le code du gestionnaire.

### Gestionnaire d’actions

Fonction côté serveur qui s’exécute lorsque la plateforme LLM appelle une action. Utilisez le gestionnaire pour valider les entrées, appeler vos API et renvoyer du texte ainsi que des données structurées.

### Widget {#widgets-eds}

Réponse visuelle affichée avec la réponse du LLM, telle qu’une carte, un carrousel ou un tableau. Les widgets générés sont des blocs dans un référentiel [!DNL Edge Delivery Services] (EDS). Avec l’infrastructure gérée par Adobe, Adobe prépare et héberge le référentiel pour une version d’essai. Vous pouvez également connecter votre propre référentiel.

### Serveur MCP

Point d’entrée exposé après le déploiement. Une plateforme LLM prise en charge se connecte à ce point d’entrée pour découvrir et appeler vos actions.

## Fonctionnement

Vous définissez les fonctionnalités de votre marque dans [!DNL LLM Apps]. La plateforme rend ces fonctionnalités disponibles sous forme d’actions, et un assistant d’IA utilise ces actions pour répondre aux demandes des clients dans la conversation.

```
┌────────────────────┐          ┌────────────────────┐          ┌────────────────────┐
│     Your brand     │          │      LLM Apps      │          │    AI assistant    │
│                    │          │                    │          │                    │
│   What you offer   │   ───▶   │  Turns that into   │   ───▶   │ Answers with your  │
│  and how you help  │          │    something AI    │          │    brand, live     │
│  customers today   │          │     can act on     │          │  inside the chat   │
└────────────────────┘          └────────────────────┘          └────────────────────┘
```

Pour plus d’informations sur les éléments à créer et le fonctionnement des composants, voir [Comment une application est connectée](/help/guides/app-architecture.md).

## Commencer {#choose-your-journey}

La création d’une application et la création de ses actions sont des étapes distinctes :

1. [Configurer l’infrastructure](/help/guides/create-app.md). Utilisez l’infrastructure gérée par Adobe pour une version d’essai ou fournissez deux référentiels [!DNL GitHub] vides via l’assistant de création d’application. La plateforme initialise l’un pour les gestionnaires d’action et l’autre pour les widgets.
2. [Créer vos actions](/help/guides/creating-actions.md). Demandez à la plateforme d’analyser votre site web et de générer des actions, ou créez-les vous-même.
3. [Déployez et testez l’application](/help/guides/deploy-your-app.md). Examinez le travail généré, déployez-le dans l’environnement d’évaluation et testez-le dans une plateforme LLM prise en charge.

La génération automatique vous donne un point de départ avec un exemple de comportement. Avant de mettre l’application à la disposition des clients, [connectez vos gestionnaires à vos systèmes](/help/guides/customize-handler.md) et [personnalisez les widgets](/help/guides/widgets.md).
