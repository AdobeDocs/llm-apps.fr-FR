---
title: Présentation des applications Adobe LLM
description: Découvrez ce qu’est l’application Adobe LLM, son fonctionnement et ce dont vous avez besoin pour commencer.
source-git-commit: 344c5457eb79a19b1dae823732a1cd9866dcd9dc
workflow-type: tm+mt
source-wordcount: '831'
ht-degree: 2%

---


# Applications Adobe LLM - Aperçu {#adobe-llm-apps-an-overview}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

## Qu’est-ce qu’[!DNL Adobe LLM Apps] ?

[!DNL Adobe LLM Apps] permet à votre marque d’exposer ses actions clés, telles que la découverte de produits, les contrôles de disponibilité ou les réservations de services, directement dans les assistants d’IA tels que [!DNL ChatGPT] ou Claude. Au lieu d&#39;être mentionnée passivement dans les réponses générées par l&#39;IA, votre marque peut guider les clients à travers les flux réels de l&#39;entreprise sans qu&#39;ils ne quittent jamais la conversation.

[!DNL LLM Apps] est disponible à l’adresse [experience.adobe.com/llm-apps](https://experience.adobe.com/llm-apps).

## Ce que vous pouvez faire avec [!DNL LLM Apps]

- **Créer des actions LLM de marque** — Définissez les flux métier spécifiques que vous souhaitez activer dans les assistants d’IA (par exemple, *Planifier un essai routier*, *Comparer des produits*, *Réserver un service*).
- **Créer des widgets LLM interactifs** — Créez des composants visuels d’interface utilisateur (cartes produit, formulaires de réservation, localisateurs de magasin) gérés en tant que composants AEM dans votre référentiel de [!DNL GitHub].
- **Maintenir une gouvernance de marque centralisée** — Les auteurs et les développeurs conservent un contrôle total sur l’ensemble du contenu, des copies et des visuels exposés dans la plateforme LLM, les approbations étant gérées via AEM.
- **Déploiement vers l’environnement d’évaluation et de production** — Un pipeline de déploiement contrôlé vous permet de tester l’expérience dans un environnement d’évaluation avant de passer à la production.
- **Contrôler la visibilité au niveau des actions** — Après le déploiement, les actions individuelles peuvent être activées ou désactivées sans redéployer l&#39;application entière.
- **Mesurer ce qui motive les décisions** — Analyses intégrées (optimisées par Adobe Customer Journey Analytics) le nombre de déclencheurs d’action de surface, les taux de succès, les taux d’abandon, les principales invites d’utilisateur et les scores de visibilité.

## Pourquoi [!DNL LLM Apps] important

Les interactions LLM sont fondamentalement différentes de la recherche traditionnelle. La durée moyenne d’une session [!DNL ChatGPT] est quatre fois plus longue qu’une session de recherche traditionnelle. Plus de 40 % des consommateurs utilisent des outils d’IA pour prendre des décisions d’achat complexes. Sans [!DNL LLM Apps], vous pourriez gagner la mention mais perdre le client. [!DNL LLM Apps] garantit que votre marque est non seulement visible, mais aussi exploitable au moment précis où un utilisateur est prêt à prendre une décision.

## Concepts clés

**Application LLM** : assistant de marque avec lequel les utilisateurs interagissent au sein de [!DNL ChatGPT] ou d’autres plateformes LLM. Il regroupe toutes vos actions et déploie en une seule unité.

**Action** — une fonctionnalité que votre application offre. Par exemple, « Trouver un distributeur » ou « Parcourir les produits ». Chaque action est appelée par le LLM lorsque l’utilisateur pose une question pertinente. Chaque action comporte deux parties : les métadonnées (nom, description, paramètres) gérées dans l’interface utilisateur de [!DNL LLM Apps], et un gestionnaire (votre code) dans [!DNL GitHub].

**Gestionnaire d&#39;action** — Code qui s&#39;exécute lorsqu&#39;une action est appelée. Il peut appeler vos API, récupérer des données actives ou renvoyer des données statiques. Les gestionnaires résident dans votre référentiel [!DNL GitHub] à l’adresse `actions/<name>/index.js`.

**Widget** — la réponse visuelle présentée à l’utilisateur — une carte, un carrousel, un tableau ou toute interface utilisateur personnalisée rendue avec la réponse textuelle du LLM. Les widgets sont des pages HTML hébergées sur un site [!DNL Edge Delivery Services] (EDS).

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

## Conditions préalables

### Console de développeur Adobe

Vous devez accéder au [](https://developer.adobe.com/console) avec le rôle **Développeur** (ou **Administrateur système**) dans votre organisation Adobe IMS. Vérifiez que votre organisation a accès à [[!DNL App Builder]](https://developer.adobe.com/app-builder/docs/intro_and_overview/).

Pour vérifier, accédez à [](https://developer.adobe.com/console). Si l’écran de démarrage rapide s’affiche, vos autorisations sont correctement configurées.

![Adobe Developer Console — Écran de démarrage rapide confirmant l’accès développeur](/help/assets/overview/dev-console-access-granted.png)

Si un message **Accès limité** s’affiche à la place de cette réponse, cela signifie que vous ne disposez pas du rôle Développeur. Contactez votre administrateur d’organisation IMS pour demander l’accès.

![Adobe Developer Console — Message à accès restreint](/help/assets/overview/dev-console-access-denied.png)

### [!DNL GitHub]

Vous avez besoin d’un compte [!DNL GitHub] avec les autorisations suivantes dans votre organisation :

- **Créer des référentiels** — vous devez créer deux référentiels dans votre organisation : un pour le code de l’application et un pour le projet EDS. Pour vérifier, accédez à [](https://github.com/new) — si vous pouvez sélectionner votre organisation dans la liste déroulante **Propriétaire**, vous disposez de l’autorisation.

  ![GitHub, nouveau menu déroulant Propriétaire du référentiel qui affiche la sélection de l’organisation](/help/assets/overview/github-repo-owner-dropdown.png)

- **Installer des applications [!DNL GitHub]** — vous avez besoin des autorisations appropriées pour installer une application [!DNL GitHub] sur votre organisation. Voir [Conditions requises pour installer une application GitHub](https://docs.github.com/en/apps/using-github-apps/installing-a-github-app-from-a-third-party#requirements-to-install-a-github-app).

### AEM Sites avec [!DNL Edge Delivery Services]

Les widgets d’action sont hébergés sur **Adobe Experience Manager [!DNL Edge Delivery Services] (EDS)**. Votre entreprise a besoin d’une licence AEM Sites qui inclut [!DNL Edge Delivery Services]. Vous devez disposer du rôle **Admin** dans votre organisation EDS.

Pour vérifier, accédez à l’outil [EDS User Admin](https://tools.aem.live/tools/user-admin/index.html), saisissez le nom de votre organisation, laissez le champ **Site** vide, puis cliquez sur **Récupérer des utilisateurs**. Recherchez votre compte dans la liste et vérifiez qu’il affiche le badge **admin**.

![ Outil d’administration des utilisateurs EDS présentant un utilisateur avec le rôle d’administrateur](/help/assets/overview/eds-user-admin.png)

### Plateforme LLM (pour les tests)

Pour tester votre application déployée, vous avez besoin d’un niveau d’abonnement pris en charge qui autorise les applications MCP personnalisées et l’activation du **mode Développeur**. Par exemple, [!DNL ChatGPT] nécessite un abonnement **Pro**, **Business** ou **Enterprise/Edu**.

## Commencer

En gardant à l’esprit un cas d’utilisation, [créez une application](/help/guides/create-app.md) pour commencer à créer et déployer votre expérience [!DNL LLM Apps].

