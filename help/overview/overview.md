---
title: Présentation des applications Adobe LLM
description: Découvrez ce qu’est l’application Adobe LLM, son fonctionnement et ce dont vous avez besoin pour commencer.
source-git-commit: 1d677c4e21963d1b126abb6287fccedfc1933c1a
workflow-type: tm+mt
source-wordcount: '938'
ht-degree: 2%

---


# Applications Adobe LLM - Aperçu {#adobe-llm-apps-an-overview}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

## Qu’est-ce qu’[!DNL Adobe LLM Apps] ?

[!DNL Adobe LLM Apps] permet à votre marque de proposer des actions utiles, telles que la découverte de produits, les contrôles de disponibilité ou les réservations de services, dans les assistants d’IA tels que [!DNL ChatGPT].

[!DNL LLM Apps] est disponible à l’adresse [experience.adobe.com](https://experience.adobe.com/#/@llmapps/llm-apps/).

## Ce que vous pouvez faire avec [!DNL LLM Apps]

- **Créer des actions LLM de marque** — Définissez les flux métier spécifiques que vous souhaitez activer dans les assistants d’IA (par exemple, *Planifier un essai routier*, *Comparer des produits*, *Réserver un service*).
- **Créer des widgets LLM interactifs** — Créez des composants visuels d’interface utilisateur (cartes produit, formulaires de réservation, localisateurs de magasin) gérés en tant que composants AEM dans votre référentiel de [!DNL GitHub].
- **Maintenir une gouvernance de marque centralisée** — Les auteurs et les développeurs conservent un contrôle total sur l’ensemble du contenu, des copies et des visuels exposés dans la plateforme LLM, les approbations étant gérées via AEM.
- **Déploiement vers l’environnement d’évaluation et de production** — Un pipeline de déploiement contrôlé vous permet de tester l’expérience dans un environnement d’évaluation avant de passer à la production.
- **Contrôler la visibilité au niveau des actions** — Après le déploiement, les actions individuelles peuvent être activées ou désactivées sans redéployer l&#39;application entière.
- **Mesurer ce qui motive les décisions** — Analyses intégrées (optimisées par Adobe Customer Journey Analytics) le nombre de déclencheurs d’action de surface, les taux de succès, les taux d’abandon, les principales invites d’utilisateur et les scores de visibilité.

## Pourquoi [!DNL LLM Apps] important

Les interactions LLM sont fondamentalement différentes de la recherche traditionnelle. La durée moyenne d’une session LLM est quatre fois plus longue qu’une session de recherche traditionnelle. Plus de 40 % des consommateurs utilisent des outils d’IA pour prendre des décisions d’achat complexes. Sans [!DNL LLM Apps], vous pourriez gagner la mention mais perdre le client. [!DNL LLM Apps] garantit que votre marque est non seulement visible, mais aussi exploitable au moment précis où un utilisateur est prêt à prendre une décision.

## Concepts clés {#key-concepts}

### Application LLM

Votre assistant de marque avec lequel les utilisateurs interagissent au sein de [!DNL ChatGPT] ou d’autres plateformes LLM. Il regroupe toutes vos actions et déploie en une seule unité.

### Action {#actions}

Une fonctionnalité proposée par votre application, telle que *Trouver un distributeur* ou *Parcourir les produits*. La plateforme LLM appelle une action lorsqu’une requête correspond à sa description. Les métadonnées d’action sont gérées dans [!DNL LLM Apps], tandis que leur gestionnaire est le code de votre référentiel [!DNL GitHub].

### Gestionnaire d’actions

Fonction côté serveur qui s’exécute lorsqu’une action est appelée. Il peut valider les entrées, appeler vos API et renvoyer du texte ainsi que des données structurées.

### Widget {#widgets-eds}

Réponse visuelle affichée avec la réponse du LLM, telle qu’une carte, un carrousel ou un tableau. Les widgets générés sont des blocs dans un référentiel [!DNL Edge Delivery Services] (EDS) que vous détenez.

### Serveur MCP

Point d’entrée exposé après le déploiement. Une plateforme LLM prise en charge se connecte à ce point d’entrée pour découvrir et appeler vos actions.

## Fonctionnement

Le diagramme ci-dessous montre comment les différents éléments s’imbriquent, de la définition d’une application dans l’interface utilisateur à l’affichage des résultats en direct dans la plateforme LLM.

```
┌─────────────────────────────────────────────────────────────┐
│                      LLM Apps UI                            │
│  ┌──────────┐   ┌──────────┐   ┌───────────────────────┐    │
│  │   App    │──▶│ Actions  │──▶│ Metadata + Widget cfg │    │
│  └──────────┘   └──────────┘   └───────────┬───────────┘    │
└─────────────────────────────────────────── │ ────────────-──┘
                                             │ deploy
                                             ▼
┌─────────────────────────────────────────────────────────────┐
│                  Adobe I/O Runtime                          │
│               MCP Server (auto-generated)                   │
│  ┌───────────────┐ ┌──────────────────┐ ┌───────────────┐   │
│  │ search-       │ │ get-product-     │ │ find-where-   │   │
│  │ products      │ │ details          │ │ to-buy        │   │
│  └───────────────┘ └──────────────────┘ └───────────────┘   │
└──────────────────────────────┬──────────────────────────────┘
                               │ MCP protocol
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                        ChatGPT                              │
│  Conversation                                               │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  EDS Widget                                           │  │
│  │  Product carousel, store locator, detail card ...     │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

## Exigences {#requirements}

Remplissez toutes les conditions requises suivantes avant de créer une application.

### Console de développeur Adobe

Votre organisation Adobe IMS doit avoir accès à [[!DNL App Builder]](https://developer.adobe.com/app-builder/docs/intro_and_overview/). Vous avez besoin du rôle **Développeur** ou **Administrateur système**.

Pour vérifier votre accès, ouvrez [&#128279;](https://developer.adobe.com/console). L’écran de démarrage rapide confirme que vous disposez de l’accès requis.

![Adobe Developer Console — Écran de démarrage rapide confirmant l’accès développeur](/help/assets/overview/dev-console-access-granted.png)

Si vous voyez **Accès limité**, contactez l’administrateur de votre organisation IMS et demandez le rôle de développeur.

![Adobe Developer Console — Message à accès restreint](/help/assets/overview/dev-console-access-denied.png)

### [!DNL GitHub]

Vous avez besoin d’un compte [!DNL GitHub] qui **peut** comme suit. Il s’agit d’une vérification des autorisations — n’installez rien pour le moment :

- Créez deux référentiels dans le compte ou l’organisation propriétaire de l’application.
- Installez les applications [!DNL GitHub] ultérieurement au cours du processus de configuration ou demandez à un administrateur de l’organisation de les approuver.

Pour vérifier l’accès à la création du référentiel, ouvrez [&#128279;](https://github.com/new) et vérifiez que le compte ou l’organisation prévu s’affiche sous **Propriétaire**.

![GitHub — Sélectionnez un propriétaire de référentiel](/help/assets/overview/github-repo-owner-dropdown.png)

Pour les référentiels appartenant à l’organisation, un administrateur de l’organisation peut avoir besoin d’approuver les applications [!DNL GitHub].

>[!NOTE]
>
>Il s’agit d’une vérification des autorisations, pas d’une étape de configuration. N’installez pas encore d’applications [!DNL GitHub] : [Créer automatiquement votre première application](/help/guides/create-app.md) vous guide tout au long de l’installation de chacune d’elles, en fonction des référentiels exacts que vous créez, au point où cela est nécessaire.

### Site Web

Vous avez besoin d’un site web HTTPS public qui représente les produits, services ou tâches que l’application doit prendre en charge. La plateforme analyse ce site web pour proposer des actions et créer des exemples de données représentatives.

N’utilisez pas un site web qui expose des informations confidentielles ou dont l’accès est contrôlé.

### [!DNL ChatGPT] ou [!DNL Claude] pour les tests

Pour suivre le tutoriel de prise en main, utilisez un plan de [!DNL ChatGPT] pris en charge avec le mode Développeur activé ou un plan de [!DNL Claude] pris en charge avec les connecteurs personnalisés activés. Les administrateurs de Workspace ou d’une organisation peuvent restreindre l’accès. Voir [Test dans le ChatGPT](/help/guides/test-in-chatgpt.md#plan-requirements) ou [Test dans Claude](/help/guides/test-in-claude.md#plan-requirements).

## Choisissez votre parcours {#choose-your-journey}

### &#x200B;1. Créer et lancer votre première application

Commencez par [Générer et lancer votre première application](/help/guides/create-app.md). Ce parcours commence avec deux référentiels vides et se termine par une application prête pour la production testée en tant que plug-in dans une plateforme LLM prise en charge, telle que [!DNL ChatGPT].

### &#x200B;2. Personnaliser l’application générée

Choisissez ce parcours lorsque la plateforme a créé l’application automatiquement et que vous souhaitez remplacer l’exemple de comportement :

1. [Personnalisez les gestionnaires générés](/help/guides/customize-handler.md) pour connecter vos API et définir les données renvoyées par chaque action.
2. [Personnalisez les widgets générés](/help/guides/widgets.md) pour utiliser ces données et appliquer vos interactions et votre conception.

### &#x200B;3. Ajouter une nouvelle action à partir de zéro

Choisissez [Ajouter une nouvelle action à partir de zéro](/help/guides/create-action.md) pour définir de nouvelles métadonnées, écrire le gestionnaire, connecter un widget, tester et déployer l’action.

### &#x200B;4. Connecter un projet EDS existant

Choisissez [Connecter un projet EDS existant](/help/guides/bring-your-own-eds.md) lorsque vous disposez déjà d’un site EDS ou que vous n’avez pas créé l’application automatiquement.

Chaque parcours utilise l’étape partagée [déploiement](/help/guides/deploy-your-app.md), puis [test du plug-in ChatGPT](/help/guides/test-in-chatgpt.md) ou [test du connecteur Claude](/help/guides/test-in-claude.md).

