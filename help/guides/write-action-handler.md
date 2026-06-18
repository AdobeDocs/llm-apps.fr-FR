---
title: Écrire le gestionnaire d’actions
description: Découvrez comment créer un gestionnaire d’action pour votre application LLM Adobe, y compris le contrat du gestionnaire, le contenu structuré et un exemple de travail.
source-git-commit: 1a99e2e80e50a3bcf9ce6fb910365202bf06e113
workflow-type: tm+mt
source-wordcount: '719'
ht-degree: 0%

---


# Écrire le gestionnaire d’actions

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Après avoir créé une action dans l’interface utilisateur de [!DNL Adobe LLM Apps], les métadonnées sont stockées dans l’API [!DNL LLM Apps], mais il n’y a pas encore de code derrière. Ce guide vous guide tout au long de l’écriture de la fonction de gestionnaire qui s’exécute lorsqu’une plateforme LLM (telle que [!DNL ChatGPT] ou Claude) appelle votre action.

Pour plus d’informations sur la disposition du projet, le développement local et les tests, voir [Développement](/help/reference/development.md).

## Contrat de développement

Vous écrivez uniquement des gestionnaires. Tous les autres éléments (nom de l’action, description, schéma d’entrée, annotations, visibilité du widget, autorisations, CSP) résident dans l’interface utilisateur de [!DNL LLM Apps] et sont automatiquement transmis au runtime au moment du déploiement. Vous ne modifiez jamais manuellement les métadonnées dans votre référentiel et vous n’enregistrez jamais un outil dans le code.

| Préoccupation | Où il vit |
|---------|----------------|
| Métadonnées (nom, description, schéma, paramètres du widget) | Interface utilisateur de [!DNL LLM Apps] — enregistré dans l’API |
| Code de gestionnaire (fonction qui s’exécute) | Votre référentiel [!DNL GitHub] — `actions/<name>/index.js` |
| `actions.json` (instantané de métadonnées) | Écrit par le pipeline de déploiement ; téléchargé à partir de l’interface utilisateur pour le développement local. |

## Prise en main

Votre référentiel lié a besoin de la structure du projet avant de pouvoir écrire des gestionnaires. Clonez le modèle standard **[Applications Adobe LLM &#x200B;](https://github.com/Adobe-AIFoundations/llm-apps-boilerplate)** pour commencer avec un point de départ vide.

Envoyez le contenu au référentiel que vous avez lié lors de la création de l’application (par exemple, `your-org/your-repo`).

Une fois le code en place, exécutez :

```bash
npm install
```

Cela installe toutes les dépendances, y compris [`@adobe/llm-apps-runtime`](https://www.npmjs.com/package/@adobe/llm-apps-runtime), le runtime qui gère la communication du protocole MCP, la découverte des actions et le routage des requêtes. Vous n’interagissez pas directement avec le runtime ; il est utilisé par les `entry.js` au moment de la création.

>[!TIP]
>
>Si vous utilisez [Code Claude](https://claude.ai/code) ou [Curseur](https://cursor.com), le standard inclut une compétence Claude prête à l&#39;emploi à l&#39;`.claude/skills/llm-apps-action-author/`. Il peut créer un modèle automatique pour de nouvelles actions, générer des fichiers de test, valider des formes de gestionnaire et vous guider tout au long du contrat de gestionnaire, le tout à partir de votre éditeur. Pour l&#39;utiliser, demandez à Claude de *« ajouter une action appelée search-products »* et elle suivra automatiquement les conventions de projet correctes.

## Contrat de gestionnaire

Un gestionnaire est un fichier unique au `actions/<name>/index.js` qui exporte une fonction asynchrone :

```javascript
module.exports = async (args) => {
  return {
    content: [{ type: 'text', text: 'response for the LLM' }],
    structuredContent: { /* data for the widget */ }
  }
}
```

La fonction reçoit les arguments d’entrée de l’action sous la forme d’un objet simple ; il s’agit des paramètres que vous avez définis dans la boîte de dialogue Créer une action . Le serveur les valide par rapport au schéma d’entrée avant l’appel de votre gestionnaire.

### `content` (obligatoire)

Tableau de parties de contenu envoyées aux hôtes LLM et texte uniquement. C&#39;est ce que la plateforme LLM lit pour formuler sa réponse.

```javascript
content: [
  { type: 'text', text: 'Found 5 products matching category "bagged-coffee".' }
]
```

Renvoyez toujours `content` : il s’agit de la solution de secours universelle pour tout hôte.

### `structuredContent`

Un objet JavaScript brut envoyé au widget. Ces données n’ont **aucun coût de jeton** — elles sont utilisées par le bloc de widget EDS pour effectuer le rendu d’une interface utilisateur riche comme un carrousel de produit ou une carte.

```javascript
structuredContent: {
  products: [
    { name: 'Product A', category: 'bagged-coffee', imageUrl: '...' },
    { name: 'Product B', category: 'bagged-coffee', imageUrl: '...' }
  ],
  total: 2,
  category: 'bagged-coffee'
}
```

La structure dépend de vous : elle doit correspondre à ce que votre bloc de widget EDS attend via `bridge.toolResult`.

>[!IMPORTANT]
>
>`structuredContent` doit être un objet simple, et non un tableau nu.

### `_meta` (facultatif)

Métadonnées supplémentaires envoyées avec le résultat. La touche `openai/widgetDescription` indique à la plateforme LLM comment présenter le widget :

```javascript
_meta: {
  'openai/widgetDescription': 'The widget displays a scrollable product carousel. '
    + 'Do NOT repeat the product list. Instead, highlight one or two recommendations.'
}
```

## Exemple : gestionnaire de recherches de produits

Voici un exemple de gestionnaire de `search-products`. Elle accepte un filtre de `category` facultatif et un `query` de texte libre, effectue une recherche dans un catalogue de produits et renvoie à la fois un résumé textuel pour le LLM et des données structurées pour le carrousel de widgets.

>[!NOTE]
>
>Pour plus de simplicité, cet exemple utilise un tableau de produits codé en dur. Dans une application réelle, vous appelez généralement votre propre API de produit ou base de données pour récupérer les résultats de manière dynamique.

```javascript
// actions/search-products/index.js

const PRODUCTS = [
  {
    name: 'Product A',
    description: 'A short description of Product A.',
    category: 'bagged-coffee',
    sub_category: 'dark-roast',
    image_url: 'https://www.example.com/products/product-a/hero.jpg',
    url: 'https://www.example.com/products/product-a',
    productId: 'PROD-001',
    rating: 4.7,
    reviewCount: 58
  },
  // ... more products
];

const WIDGET_DESCRIPTION = 'The widget displays a scrollable product carousel '
  + 'with images, star ratings, and review counts. Do NOT repeat the product list.';

module.exports = async ({ category = '', query = '' } = {}) => {
  let results = PRODUCTS;

  if (category) {
    const categoryLower = category.toLowerCase();
    results = results.filter((p) =>
      p.category.toLowerCase().includes(categoryLower)
      || p.sub_category.toLowerCase().includes(categoryLower)
    );
  }

  if (query) {
    const queryLower = query.toLowerCase();
    results = results.filter((p) =>
      p.name.toLowerCase().includes(queryLower)
      || p.description.toLowerCase().includes(queryLower)
    );
  }

  const products = results.map((p) => ({
    productId: p.productId,
    name: p.name,
    shortDescription: p.description,
    category: p.category,
    rating: p.rating,
    reviewCount: p.reviewCount,
    imageUrl: p.image_url,
    productUrl: p.url,
  }));

  if (products.length === 0) {
    return {
      content: [{ type: 'text', text: `No products found for "${category}".` }],
      structuredContent: { products: [], total: 0, category: null },
      _meta: { 'openai/widgetDescription': WIDGET_DESCRIPTION }
    };
  }

  return {
    content: [
      { type: 'text', text: `Found ${products.length} product(s) in "${category}".` }
    ],
    structuredContent: { products, total: products.length, category },
    _meta: { 'openai/widgetDescription': WIDGET_DESCRIPTION }
  };
};
```

**Que se passe-t-il au moment de l’exécution :**

1. Un utilisateur demande à la plateforme LLM *« Montrez-moi vos produits à base de café »*.
2. La plateforme LLM correspond à l’intention de *Rechercher des produits* et extrait les `category`.
3. Le serveur MCP appelle votre gestionnaire avec `{ category: 'bagged-coffee' }`.
4. Votre gestionnaire filtre le catalogue et renvoie `content` (résumé textuel pour le LLM) + `structuredContent` (tableau de produits pour le widget).
5. La plateforme LLM affiche la réponse textuelle et transmet les données structurées au widget EDS, qui effectue le rendu d’un carrousel de produits.

## Que se passe-t-il si le gestionnaire est absent ?

Si vous avez défini une action dans l’interface utilisateur, mais que vous n’avez pas encore créé le fichier de gestionnaire, l’action est toujours enregistrée au moment du déploiement. Les appels utilisent un gestionnaire de stub par défaut qui renvoie du contenu vide jusqu’à ce que vous ajoutiez le code réel. Cela signifie que vous pouvez d’abord définir toutes vos actions dans l’interface utilisateur et les implémenter de manière incrémentielle.

