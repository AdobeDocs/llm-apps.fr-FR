---
title: Personnaliser un gestionnaire d’actions généré
description: Comprenez le contrat du gestionnaire d’applications Adobe LLM, remplacez les données d’exemple générées et assurez-vous que la sortie du gestionnaire est alignée sur son widget.
source-git-commit: eec74b87457bc852d7a8dd0e46c2a4385a93ae0a
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 0%

---


# Personnaliser un gestionnaire généré {#customize-generated-handler}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

L’agent d’intégration crée un gestionnaire fonctionnel pour chaque action générée. Le gestionnaire renvoie initialement des données d’exemple afin que vous puissiez tester l’expérience complète.

Utilisez ce guide pour comprendre le contrat du gestionnaire et remplacer les exemples de données par vos API ou vos sources de données.

**Parcours :** recherchez le gestionnaire généré → comprendre ses entrées et les résultats → connectez votre système → que le contrat du widget soit aligné → tester et déployer.

## Rechercher le gestionnaire généré

Ouvrez le référentiel de gestionnaire sélectionné lors de l’intégration :

```text
actions/
└── <action-name>/
    └── index.js
```

Les tests correspondants sont stockés séparément :

```text
test/
└── actions/
    └── <action-name>.test.js
```

Modifiez le `index.js` généré. Ne modifiez pas les fichiers d’exécution tels que `entry.js`.

## Contrat de gestionnaire

Chaque gestionnaire exporte une fonction asynchrone :

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

La fonction reçoit un objet `args` et renvoie un objet de résultat.

### Entrée : `args`

`args` contient les paramètres définis pour l’action dans [!DNL LLM Apps].

Pour une action avec des paramètres `category` et `query` :

```javascript
module.exports = async ({ category = '', query = '' } = {}) => {
  // Use the validated action arguments.
};
```

Le runtime valide le schéma d’entrée lorsque les métadonnées d’action incluent des `inputSchema`, comme il le fait après le déploiement. La découverte du gestionnaire local sans `actions.json` n’applique pas la validation du schéma. Le gestionnaire doit toujours appliquer les règles métier, telles que les valeurs prises en charge, les longueurs maximales et les combinaisons autorisées.

### Sortie : `content`

Toujours renvoyer `content`. Il s’agit d’un tableau de parties de contenu lues par la plateforme LLM et par les hôtes qui n’affichent pas de widgets.

```javascript
content: [
  {
    type: 'text',
    text: 'Found 3 products matching your search.'
  }
]
```

Faites en sorte que cette réponse soit concise. N’incluez pas d’informations d’identification, d’erreurs internes ou de données que l’utilisateur n’est pas autorisé à voir.

### Sortie : `structuredContent`

Renvoie `structuredContent` lorsque l’action comporte un widget. Il doit s’agir d’un objet simple, et non d’un tableau nu.

```javascript
structuredContent: {
  products: [
    {
      id: 'P-100',
      name: 'Frescopa House Blend',
      price: '$14.99'
    }
  ],
  total: 1
}
```

`structuredContent` est envoyé au widget, et non au LLM. Renvoyer uniquement les champs requis par l’interface.

Pour une action en mode texte uniquement, `structuredContent` peut être omis.

## Contrat de gestionnaire-widget

Le gestionnaire et le widget partagent le même contrat : la forme de `structuredContent`.

```text
Action arguments
      ↓
Handler
      ├── content → LLM text response
      └── structuredContent → Widget
                                  ↓
                           bridge.toolResult
```

Le widget lit le résultat du gestionnaire à partir du pont SDK des applications LLM :

```javascript
export default async function decorate(block, bridge) {
  const result = await bridge.toolResult;
  const products = result?.structuredContent?.products ?? [];

  // Render products.
}
```

Si le gestionnaire renvoie :

```javascript
structuredContent: {
  products: [...],
  total: 3
}
```

le widget doit lire `structuredContent.products` et `structuredContent.total`.

La modification d’un nom ou d’un type de champ peut rompre le widget. Mettez à jour le gestionnaire, le widget et les tests ensemble.

## Remplacer les exemples de données

Les gestionnaires générés contiennent généralement un tableau d’échantillons en mémoire. Remplacez cette recherche de données par un appel côté serveur à votre système.

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

  const products = payload.products.map(({ id, name, price }) => ({
    id,
    name,
    price
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

Conserver l’accès réseau protégé dans le gestionnaire. Ne placez jamais les informations d’identification d’API dans le JavaScript de widget ou le contrôle de code source.

## Gérer les états attendus

Conserver une forme de sortie prévisible pour chaque résultat.

### Résultats trouvés

```javascript
{
  content: [{ type: 'text', text: 'Found 3 products.' }],
  structuredContent: { products: [...], total: 3 }
}
```

### Aucun résultat

```javascript
{
  content: [{ type: 'text', text: 'No matching products were found.' }],
  structuredContent: { products: [], total: 0 }
}
```

Le widget peut désormais rendre un état vide sans deviner s’`products` existe.

En cas d’échec de service, renvoyez ou renvoyez une erreur sécurisée sans exposer les traces de la pile, les jetons, les hôtes internes ou les corps de réponse en amont.

## Tester le contrat

Mettez à jour les tests générés chaque fois que le gestionnaire change. Couverture :

- Arguments valides et non valides.
- Résultats et états sans résultats.
- Échecs d’API et délais d’expiration.
- Réponses API incorrectes.
- `content` est toujours présent.
- `structuredContent` est un objet simple.
- Forme attendue par le widget.

Exécutez :

```bash
npm test
```

Pour les tests MCP locaux, voir [Développement et test des gestionnaires locaux](/help/reference/development.md).

## Déployer la modification

1. Validez et envoyez les modifications du gestionnaire.
2. Si la forme de données a changé, mettez à jour et poussez le widget.
3. [Déployez l’application](/help/guides/deploy-your-app.md) dans l’environnement intermédiaire.
4. [Testez le plug-in ChatGPT](/help/guides/test-in-chatgpt.md).
5. Une fois l’étape réussie, déployez en production.

Voir ensuite [Personnaliser un widget généré](/help/guides/widgets.md).
