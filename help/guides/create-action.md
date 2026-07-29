---
title: Création d’une action à partir de zéro
description: Définissez des métadonnées d’action, implémentez son gestionnaire, connectez un widget EDS, testez-le et déployez-le avec les applications Adobe LLM.
source-git-commit: bb3d8a02f22a91ceeeba5999453aeb4221060f80
workflow-type: tm+mt
source-wordcount: '1137'
ht-degree: 1%

---


# Création d’une action à partir de zéro {#create-action-from-scratch}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

>[!NOTE]
>
>Ce guide suppose des connaissances de base d’Adobe Edge Delivery Services (EDS). Si vous êtes nouveau dans EDS, lisez d’abord le tutoriel de développement [EDS](https://www.aem.live/developer/tutorial) et [Exploration des blocs](https://www.aem.live/docs/exploring-blocks) pour en savoir plus sur l’essentiel (les blocs, la fonction `decorate` et la structure du projet EDS) avant de connecter un widget.

Utilisez ce guide pour ajouter une fonctionnalité que la plateforme n’a pas créée. Vous devez définir l’action dans [!DNL LLM Apps], écrire son gestionnaire dans le référentiel lié, ajouter un widget si nécessaire, le tester et le déployer.

**Parcours :** planifiez l’action → créer ses métadonnées → écrire le gestionnaire → connecter le widget → tester localement → déployer et tester le plug-in.

Pour votre première application, commencez par [Créer votre première application automatiquement](/help/guides/create-app.md).

## Avant de commencer

Vous avez besoin des éléments suivants :

- Une application LLM existante.
- Référentiel de gestionnaire lié.
- Le référentiel a été cloné localement avec ses dépendances installées.
- Un projet EDS si l’action affiche un widget.
- Une API ou une source de données claire pour les résultats de production.

## Planifier l’action

Une action doit exécuter une tâche utilisateur claire. Avant d’ouvrir l’interface utilisateur, définissez les éléments suivants :

- **Intention** — Ce que l&#39;utilisateur tente d&#39;accomplir.
- **Description** — quand la plateforme LLM doit sélectionner cette action.
- **Entrées** — informations minimales requises de l&#39;utilisateur.
- **Résultat** : le texte et les données structurées renvoyés par le gestionnaire.
- **Comportement** : indique si l’action lit des données, modifie des données ou appelle des systèmes externes.
- **Widget** — si le résultat nécessite une interface visuelle.

Par exemple, une action **Rechercher des produits** peut utiliser :

```text
Intent: Find products matching a category or search phrase
Inputs:
  category: optional string
  query: optional string
Result:
  content: text summary
  structuredContent: products and total count
Behavior: read-only, idempotent, open-world
Widget: product cards
```

Gardez les tâches liées mais différentes séparées. La recherche de produits et l’achat de produits ne doivent pas être une seule action, car ils comportent des entrées, des risques et des exigences de confirmation différents.

## Création des métadonnées d’action

Ouvrez l’application et sélectionnez **[!UICONTROL Actions]**, puis **[!UICONTROL Créer une action]**.

L’éditeur contient les onglets **[!UICONTROL Action]** et **[!UICONTROL Métadonnées de widget]**.

### Saisir des informations de base

![Créer une action — informations de base](/help/assets/guide-create-action/action-basic-info.png)

Enter :

- **[!UICONTROL Nom de l’action]** — Nom court de la tâche, tel que *Rechercher des produits*.
- **[!UICONTROL Description]** — expliquez quand utiliser l&#39;action et ce qu&#39;elle renvoie.

Une description utile est spécifique :

```text
Search the product catalog by category or keyword. Returns matching
products with their names, prices, categories, and image URLs.
```

Évitez les descriptions vagues telles que *Obtient des informations sur le produit*. La plateforme LLM utilise la description pour choisir entre les actions.

### Sélectionner des annotations

Les annotations décrivent le comportement de l’action :

- **Indice destructif** — l&#39;action peut supprimer ou modifier définitivement des données.
- **Idempotent (mêmes arguments = aucun effet supplémentaire)** — la répétition de la même requête a le même effet.
- **Indice du monde ouvert** — l&#39;action communique avec des systèmes externes.
- **Conseil en lecture seule** — l’action ne modifie pas les données.

Sélectionnez uniquement les annotations vraies. Par exemple, la recherche de produits est normalement en lecture seule, idempotent et open-world.

### Ajout de métadonnées OpenAI

Saisissez des messages courts affichés pendant l’exécution de l’action et une fois celle-ci terminée :

```text
Invoking: Searching products...
Invoked: Products found
```

Pour les actions avec des widgets, ajoutez **[!UICONTROL Description du widget]**. Elle est différente de la description de l’action :

- La **Description de l’action** aide le modèle à décider quand appeler l’action.
- **Description du widget** mappe à `_meta["openai/widgetDescription"]` et résume ce que le composant rendu affiche, réduisant ainsi la narration répétée.

[!DNL LLM Apps] l’applique en tant que métadonnées de composant. Ne le renvoyez pas à partir du gestionnaire .

### Configuration de la visibilité

- **[!UICONTROL Exposer au modèle d’IA]** permet au modèle de sélectionner l’action.
- **[!UICONTROL Afficher en tant que widget dans la surface de l’application]** affiche le widget configuré.

Désactivez la visibilité du widget lorsque l’action renvoie uniquement du texte.

### Ajouter des paramètres d’entrée

Ajoutez un paramètre pour chaque valeur acceptée par le gestionnaire. Chaque paramètre nécessite :

- **Name** — la clé reçue par le gestionnaire.
- **Type** — Chaîne, Nombre, Entier ou Booléen.
- **Description** — Comment le modèle doit extraire la valeur.
- **Obligatoire** — si l&#39;action peut s&#39;exécuter sans elle.

Pour **Rechercher des produits** :

```text
category
  Type: String
  Required: No
  Description: Product category used to narrow the catalog.

query
  Type: String
  Required: No
  Description: Product name or search phrase.
```

Utilisez des noms de paramètres stables. La modification d’un nom nécessite également la modification du gestionnaire et de ses tests.

### Configuration d’Analytics

Activez l’option **[!UICONTROL Collecter l’intention de l’utilisateur]** lorsque vous souhaitez que les analyses incluent un résumé de la conversation qui a conduit à l’action.

![Créer une action — Analyse des intentions de l’utilisateur](/help/assets/guide-create-action/action-analytics-user-intent.png)

Pour obtenir des définitions de champ complètes, voir [Champs d’action et de widget](/help/reference/reference-docs.md).

## Configuration du widget

Ignorez cette section pour une action en mode texte uniquement.

Ouvrez **[!UICONTROL Métadonnées de widget]**.

![Créer une action — métadonnées de widget](/help/assets/guide-create-action/widget-metadata.png)

Configurer :

- **Type** — sélectionnez EDS.
- **Domaine du widget** — origine EDS hébergeant le widget.
- **Préfère la bordure** — demande un conteneur bordé dans l&#39;hôte.
- **URL du script** — point d&#39;entrée du widget EDS.
- **URL du widget** : page EDS publiée pour cette action.

Les URL standard sont les suivantes :

```text
Script URL:
https://main--<repo>--<owner>.aem.live/scripts/aem-embed.js

Widget URL:
https://main--<repo>--<owner>.aem.live/<widget-page>
```

Octroyez uniquement les autorisations de navigateur et les domaines CSP requis.

![Créer une action : autorisations et CSP](/help/assets/guide-create-action/widget-permissions-csp.png)

Si le projet ou la page de widget EDS n’existe pas encore, complétez [Apportez votre propre projet EDS](/help/guides/bring-your-own-eds.md), puis revenez à l’action.

## Enregistrer l’action

Sélectionnez **[!UICONTROL Créer une action]**. L’action s’affiche sur la page Actions avec un badge **Non déployé**.

À ce stade, les métadonnées existent, mais l’action a toujours besoin d’un gestionnaire.

## Mettre en œuvre le gestionnaire

Clonez le référentiel de gestionnaire lié et installez ses dépendances :

```bash
npm install
```

Créer :

```text
actions/
└── search-products/
    └── index.js
```

Le nom du dossier doit correspondre à l’identifiant de code de l’action affiché dans l’éditeur d’actions.

Pour le contrat avec résultat complet et la relation gestionnaire-widget, voir [Personnaliser un gestionnaire généré](/help/guides/customize-handler.md).

### Contrat de gestionnaire

Exportez une fonction asynchrone :

```javascript
module.exports = async (args) => {
  return {
    content: [
      { type: 'text', text: 'Response for the LLM platform.' }
    ],
    structuredContent: {
      // Data for the widget.
    }
  };
};
```

Le gestionnaire reçoit les paramètres définis dans l’interface utilisateur.

### `content` de retour

`content` est le texte de remplacement lu par la plateforme LLM :

```javascript
content: [
  { type: 'text', text: 'Found 3 matching products.' }
]
```

Renvoyez toujours des `content` utiles, même si l’action comporte un widget.

### `structuredContent` de retour

`structuredContent` est un objet simple consommé par le widget :

```javascript
structuredContent: {
  products: [
    { id: 'P-100', name: 'Product A', price: '$20' }
  ],
  total: 1
}
```

La forme doit correspondre à ce que le bloc EDS lit de `bridge.toolResult`.

### Connexion à une API

Conservez l’accès protégé à l’API dans le gestionnaire côté serveur. Chargez la configuration à partir de l’environnement d’exécution et utilisez une origine HTTPS fixe.

```javascript
const API_ORIGIN = process.env.PRODUCT_API_ORIGIN;
const API_TOKEN = process.env.PRODUCT_API_TOKEN;

module.exports = async ({ query = '' } = {}) => {
  const normalizedQuery = String(query).trim();
  if (!normalizedQuery || normalizedQuery.length > 200) {
    return {
      content: [{ type: 'text', text: 'Enter a valid product search.' }],
      structuredContent: { products: [], total: 0 }
    };
  }

  if (!API_ORIGIN || !API_TOKEN) {
    throw new Error('Product API configuration is unavailable.');
  }

  const origin = new URL(API_ORIGIN);
  if (origin.protocol !== 'https:') {
    throw new Error('Product API configuration must use HTTPS.');
  }

  const url = new URL('/v1/products', origin);
  url.searchParams.set('query', normalizedQuery);

  const response = await fetch(url, {
    headers: { Authorization: `Bearer ${API_TOKEN}` },
    signal: AbortSignal.timeout(8000)
  });

  if (!response.ok) {
    throw new Error('Product service request failed.');
  }

  const payload = await response.json();
  if (!payload || !Array.isArray(payload.products)
      || !payload.products.every((product) =>
        product
        && typeof product.id === 'string'
        && typeof product.name === 'string'
        && typeof product.price === 'string')) {
    throw new Error('Product service returned an unexpected response.');
  }

  const products = payload.products.map((product) => ({
    id: product.id,
    name: product.name,
    price: product.price
  }));

  return {
    content: [
      { type: 'text', text: `Found ${products.length} matching products.` }
    ],
    structuredContent: {
      products,
      total: products.length
    }
  };
};
```

Ne placez pas les informations d’identification d’API dans le code source, les métadonnées d’action, le JavaScript de widget, les journaux ou les erreurs rencontrées par l’utilisateur ou l’utilisatrice.

Pour le code de production, validez la réponse amont complète avant de mapper les champs approuvés dans `structuredContent`.

## Ajouter des tests de gestionnaire

Créez le test correspondant :

```text
test/
└── actions/
    └── search-products.test.js
```

Testez au moins :

- Entrée valide.
- Entrée manquante ou non valide.
- Résultats vides.
- Temporisation ou échec de l’API.
- Données API incorrectes.
- Forme `structuredContent` attendue par le widget.

Exécutez :

```bash
npm test
```

Pour la disposition du projet et les tests MCP locaux, voir [Développement et test des gestionnaires locaux](/help/reference/development.md).

## Tester l’action localement

Exécutez :

```bash
npm run dev:local
```

Sans `actions.json` local, le serveur détecte le gestionnaire avec un minimum de métadonnées et aucune validation de schéma d’entrée.

Utilisez MCP Inspector ou `curl` pour :

1. Répertoriez les actions enregistrées.
2. Appelez la nouvelle action avec des arguments représentatifs.
3. Vérifiez les `content` et les `structuredContent`.
4. Testez les requêtes non valides et vides.

## Connexion et test du widget

Si l’action comporte un widget :

1. Faites en sorte que le widget lise le `structuredContent` du gestionnaire.
2. Effectuez le rendu de valeurs externes avec des API DOM sécurisées telles que `textContent`.
3. Ajoutez les états de chargement, vide et d’erreur.
4. Prévisualisez la page EDS localement.
5. Vérifiez les URL CSP, CORS et de widget.

Voir [Apporter votre propre projet EDS](/help/guides/bring-your-own-eds.md).

## Déployer et tester

1. Validez et envoyez les modifications du gestionnaire et du widget.
2. [Déployez l’application](/help/guides/deploy-your-app.md) dans l’environnement intermédiaire.
3. [Testez le plug-in ChatGPT](/help/guides/test-in-chatgpt.md).
4. Vérifiez les invites qui doivent ou non appeler l&#39;action.
5. Une fois l’étape réussie, déployez en production.

Si des métadonnées existent sans gestionnaire correspondant, le déploiement enregistre l’action avec un stub par défaut. Ajoutez le gestionnaire avant de mettre l’action à la disposition des utilisateurs.
- [Guide : configuration du widget (EDS)](/help/guides/widgets.md)
