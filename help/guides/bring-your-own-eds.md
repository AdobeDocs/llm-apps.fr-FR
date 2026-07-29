---
title: Apporter votre propre projet Edge Delivery Services
description: Connecter un projet Adobe Edge Delivery Services existant à une action Adobe LLM Apps.
source-git-commit: eec74b87457bc852d7a8dd0e46c2a4385a93ae0a
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 3%

---


# Apportez votre propre projet EDS {#bring-your-own-eds}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Utilisez ce guide lorsque vous disposez déjà d’un projet Edge Delivery Services (EDS) ou lorsque vous avez créé une application sans l’agent d’intégration.

Si l’agent d’intégration a créé votre widget, suivez [Personnaliser un widget généré](/help/guides/widgets.md) à la place. Le projet généré inclut déjà la configuration des fichiers, blocs, contenus et actions SDK décrite ici.

**Parcours :** préparez le projet EDS → installez la version de → SDK et publiez le bloc → configurez l’action → déployer et tester.

## Avant de commencer

Vous avez besoin des éléments suivants :

- Un référentiel EDS avec [AEM Code Sync](https://github.com/apps/aem-code-sync) installé.
- Autorisation d’ajouter des dépendances et de créer des blocs dans ce référentiel.
- Autorisation de configurer les en-têtes de réponse pour le site EDS.
- Action en [!DNL LLM Apps] avec un gestionnaire qui renvoie des `structuredContent`.

## Installation de LLM Apps SDK

À partir de la racine du projet EDS :

```bash
npm install @adobe/llmapps-sdk
```

Le package copie le point d’entrée du widget et l’implémentation du pont dans le projet :

```text
scripts/
├── aem-embed.js
└── llmapps-sdk.js
```

L’URL du script utilisée par l’action pointe vers `scripts/aem-embed.js`.

## Création du bloc de widget

Créez un bloc pour l’action :

```text
blocks/
└── search-products/
    ├── search-products.js
    └── search-products.css
```

Exportez la fonction `decorate` EDS standard avec le pont connecté comme deuxième argument :

```javascript
export default async function decorate(block, bridge) {
  if (bridge) {
    bridge.applyHostStyles();
  }

  const result = bridge ? await bridge.toolResult : null;
  const products = result?.structuredContent?.products ?? [];

  const list = document.createElement('ul');
  products.forEach((product) => {
    const item = document.createElement('li');
    item.textContent = String(product.name ?? 'Product');
    list.append(item);
  });

  block.replaceChildren(list);

  if (bridge) {
    bridge.autoResize(block);
  }
}
```

Utilisez des API DOM qui codent des valeurs de texte. Ne concaténez pas de données externes dans HTML.

## Créer et publier la page du widget

Créez une page EDS pour le widget et ajoutez le bloc à cette page. Publiez la page.

L’URL de la page active devient l’URL du widget de l’action :

```text
https://main--<repo>--<owner>.aem.live/<widget-page>
```

Le chemin d’accès à la page ne doit pas nécessairement correspondre au nom de l’action, mais une convention cohérente facilite la gestion du projet.

## Configurer CORS

Le widget charge la page EDS ainsi que les scripts, les styles, les blocs et les médias dans toutes les origines. Configurez l’en-tête du site EDS :

```json
{
  "/**": [
    {
      "key": "access-control-allow-origin",
      "value": "<allowed-host-origin>"
    }
  ]
}
```

Utilisez l’origine d’hôte spécifique requise par votre plateforme LLM prise en charge. Utilisez `*` uniquement lorsque le widget est intentionnellement public, n’utilise pas de requêtes cross-origin authentifiées et que vos exigences de sécurité le permettent.

Pour plus d’informations sur la configuration EDS, voir [Service de configuration](https://aem.live/docs/config-service-setup).

## Configuration de l’action

Dans [!DNL LLM Apps], ouvrez l’action et sélectionnez **[!UICONTROL Métadonnées de widget]**.

Enter :

- **[!UICONTROL URL du script]**

  ```text
  https://main--<repo>--<owner>.aem.live/scripts/aem-embed.js
  ```

- **[!UICONTROL URL du widget]**

  ```text
  https://main--<repo>--<owner>.aem.live/<widget-page>
  ```

Configurez les domaines CSP et les autorisations de navigateur avec le privilège minimum. Ajoutez uniquement les origines et les fonctionnalités requises par le widget.

Pour les définitions de champ, voir [Champs d’action et de widget](/help/reference/reference-docs.md).

## Test de l’intégration

1. Prévisualisez directement la page EDS et vérifiez ses données d’exemple de secours.
2. Testez localement le gestionnaire et comparez sa `structuredContent` à la forme attendue par le bloc.
3. Déployez l’application vers l’environnement d’évaluation.
4. Appelez l’action depuis [!DNL ChatGPT].
5. Vérifiez les états de chargement, de succès, de vide et d’erreur.

Si la page fonctionne directement, mais pas dans la plateforme LLM, vérifiez CORS, CSP, URL HTTPS et la forme `structuredContent`. Voir [Dépannage](/help/reference/troubleshooting.md).
