---
title: Comment une application est connectée
description: Découvrez les trois parties d’une application LLM, son fonctionnement dans une conversation et où apporter des modifications.
source-git-commit: af83670aebde1e6f73585c89a891bc7f5e92fb3a
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 0%
---

# Comment une application est connectée {#app-architecture}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Une application **LLM** permet d’intégrer les services de votre marque dans une conversation IA. Par exemple, un client demande des recommandations de café, votre application trouve les produits correspondants et l’assistant affiche les résultats.

**Dans ce guide :** découvrez la fonction de chaque partie de votre application, où apporter des modifications et comment ces parties fonctionnent ensemble lorsqu’une personne utilise l’application.

## Les trois parties d’une application

Une application peut regrouper plusieurs actions associées, telles que *Parcourir les produits*, *Comparer les produits* et *Trouver un magasin*. Chaque action possède ses propres paramètres, gestionnaire et widget facultatif. L’ajout d’une action ne nécessite pas une autre paire de référentiels.

Une application avec des widgets interactifs se compose de trois parties : paramètres d’action, code de gestionnaire et code de widget. Vous gérez les paramètres dans [!DNL LLM Apps]. Le code du gestionnaire et du widget résident dans deux référentiels [!DNL GitHub] distincts.

```text
┌───────────────────────┐   ┌───────────────────────┐   ┌───────────────────────┐
│ 1. LLM Apps UI        │   │ 2. Handler repository │   │ 3. Widget repository  │
│                       │   │                       │   │                       │
│ Action descriptions   │   │ Connects to your APIs │   │ Widget layout         │
│ Required information  │   │ Runs the task         │   │ Branding and styles   │
│ When to use an action │   │ Returns results       │   │ Interactive controls  │
└───────────────────────┘   └───────────────────────┘   └───────────────────────┘
            │                           │                           │
            └───────────────────────────┼───────────────────────────┘
                                        ▼
                    ┌───────────────────────────────────────┐
                    │ Your app in an AI conversation        │
                    │ The action runs and the widget        │
                    │ shows the result.                     │
                    └───────────────────────────────────────┘
```

- **Les paramètres d’action décrivent la tâche.** Il indique à l’assistant quand utiliser une action et de quelles informations il a besoin. Pour la recherche de produits, cette information peut être une catégorie ou un terme de recherche.
- **Le gestionnaire effectue le travail.** Il récupère les données de vos systèmes ou effectue une tâche. Un gestionnaire de recherche de produits, par exemple, récupère les produits correspondants de votre catalogue.
- **Le widget présente le résultat.** Il contrôle ce que le client voit et avec quoi il interagit, par exemple des cartes de produits avec des noms, des prix et des boutons.

Il est recommandé d’ajouter un widget à chaque action. Un widget vous permet de contrôler l’aspect des résultats et la manière dont les clients interagissent avec eux, plutôt que de laisser la présentation à l’assistant d’IA. Les réponses en texte seul sont également prises en charge.

## Où apporter des modifications

| Ce que vous souhaitez modifier | Où modifier |
|---|---|
| Lorsque l&#39;assistant utilise une action ou les informations demandées par l&#39;action | [Paramètres d’action dans les applications LLM](/help/reference/reference-docs.md) |
| D’où proviennent les résultats ou ce qui se passe lorsqu’une action est exécutée | [Code du gestionnaire dans le référentiel de gestionnaires](/help/guides/customize-handler.md) |
| l’aspect des résultats, y compris la disposition, la marque et les interactions ; | [Code du widget dans le référentiel de widgets](/help/guides/widgets.md) |

Gardez les trois parties cohérentes. Par exemple, si vous modifiez les informations renvoyées par un gestionnaire, mettez à jour le widget pour afficher le nouveau résultat.

## Ce qui se passe dans une conversation

Après le déploiement, votre application dispose d’une URL de connexion appelée **URL du serveur MCP**. Ajoutez cette URL à une plateforme d’IA prise en charge afin que l’assistant puisse découvrir et utiliser les actions de votre application.

Pour une application avec une action et un widget de recherche de produit :

1. Un client demande à l’assistant *Montrez-moi les capsules de café.*
2. L’assistant utilise la description de l’action pour choisir l’action de recherche de produit.
3. Le gestionnaire récupère les produits correspondants du catalogue.
4. Le widget affiche les produits sous forme de cartes dans la conversation.
5. Le client explore les résultats ou pose une question de suivi.

Le client reste dans la conversation pendant que votre application fournit les informations ou le service. La prise en charge des widgets dépend de la plateforme LLM.

## Appliquer et tester vos modifications

Publiez les modifications du widget et déployez les modifications de l’action avant de tester l’expérience mise à jour. Suivez [Déployer votre application](/help/guides/deploy-your-app.md) pour effectuer un déploiement dans l’environnement intermédiaire et tester l’application sur une plateforme LLM prise en charge.

Les actions générées automatiquement commencent par des exemples de données. Connectez les gestionnaires à vos systèmes réels et testez l’application avant de la rendre disponible pour les clients.
