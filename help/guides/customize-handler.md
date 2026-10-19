---
title: Personnaliser un gestionnaire d’actions généré
description: Comprenez le contrat du gestionnaire d’applications Adobe LLM, remplacez les données d’exemple générées et assurez-vous que la sortie du gestionnaire est alignée sur son widget.
source-git-commit: 0f9dd0a5b1008e183eb8ab5a4c2eae34a78923bb
workflow-type: tm+mt
source-wordcount: '880'
ht-degree: 0%
---

# Personnaliser un gestionnaire généré {#customize-generated-handler}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

La plateforme crée un gestionnaire fonctionnel pour chaque action générée. Le gestionnaire renvoie initialement des données d’exemple pour tester l’expérience complète.

**Dans ce guide :** découvrez comment trouver le gestionnaire généré, remplacer ses exemples de données par des appels à vos systèmes et conserver sa réponse compatible avec le widget. Ensuite, testez et déployez vos modifications.

## Rechercher le gestionnaire généré

Ouvrez le référentiel du gestionnaire connecté lors de la [configuration de l’infrastructure](/help/guides/create-app.md). Si vous utilisez l’infrastructure gérée par Adobe, demandez l’accès au code avant de continuer.

```text
actions/
└── <action-slug>/
    ├── index.js
    └── data.json
```

Les tests correspondants se trouvent dans un répertoire distinct :

```text
test/
└── actions/
    └── <action-slug>.test.js
```

Utilisez la requête de tirage générée pour localiser les fichiers exacts. Le slug du dossier utilise des tirets : la plateforme génère une action avec l’identifiant de code `search_products` sous `actions/search-products/`. Le runtime reconnaît le formulaire avec trait d’union ainsi qu’un dossier correspondant exactement à l’identifiant.

Modifiez le `index.js` généré et les données d’exemple associées. Ne modifiez pas les fichiers d’exécution tels que `entry.js`.

## Contrat de gestionnaire

Chaque gestionnaire exporte une fonction asynchrone :

```javascript
module.exports = async (args, extra) => {
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

Pour une action avec un schéma d’entrée, la fonction reçoit un objet `args` et un contexte d’exécution facultatif, `extra`, et renvoie un objet de résultat. Utilisez des `extra` pour les assistants d’exécution tels que `getVariable(extra, 'PRODUCT_API_ORIGIN')` ; les variables d’application ne sont pas automatiquement exposées par le biais de `process.env`. Voir [Configurer les variables et secrets d’application](/help/guides/app-variables.md).

### Entrée : `args`

`args` contient les paramètres définis pour l’action dans [!DNL LLM Apps].

Pour une action avec des paramètres `category` et `query` :

```javascript
module.exports = async ({ category = '', query = '' } = {}, extra) => {
  // Validate business rules before using the arguments.
};
```

Avec les métadonnées d’action, l’exécution convertit le schéma d’entrée déclaré en validation pour les règles prises en charge, y compris les champs obligatoires, les types primitifs, les énumérations et les éléments de tableau. Le runtime n’effectue pas de validation complète du schéma JSON. Vérifiez les contraintes telles que les longueurs de chaîne, les plages numériques et les propriétés d’objet imbriqué dans le gestionnaire.

La découverte locale sans métadonnées de schéma n’est pas équivalente au déploiement : le runtime ne transmet pas d’arguments d’action dans le formulaire `(args, extra)` normal. Utilisez des tests de gestionnaire direct ou des métadonnées de `actions.json` locales représentatives pour tester les arguments. Voir [Développement et test des gestionnaires locaux](/help/reference/development.md).

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

Veillez à ce que la réponse du texte soit concise. N’incluez pas d’informations d’identification, d’erreurs internes ou de données que l’utilisateur n’est pas autorisé à voir.

### Sortie : `structuredContent`

Renvoie `structuredContent` lorsque l’action comporte un widget. Il doit s’agir d’un objet simple, et non d’un tableau nu.

```javascript
structuredContent: {
  arguments: { query: 'coffee' },
  records: [
    {
      id: 'P-100',
      name: 'Frescopa House Blend',
      price: '$14.99'
    }
  ]
}
```

`structuredContent` fait partie du résultat de l’action MCP diffusé à l’hôte. Ne supposez pas qu’elle est masquée pour le modèle ou l’utilisateur ou l’utilisatrice. Renvoyez uniquement les données autorisées requises par l’expérience , jamais les informations d’identification ou la configuration du serveur privé.

Pour une action en mode texte uniquement, le renvoi de `structuredContent` est facultatif.

## Contrat de gestionnaire-widget

Le gestionnaire et le widget partagent le même contrat : la forme de `structuredContent`.

| Sortie du gestionnaire | Objectif |
|---|---|
| `content` | Réponse lisible par le modèle et solution de secours textuelle uniquement |
| `structuredContent` | Données diffusées à l’hôte et lues par le widget via `bridge.toolResult` |

Le widget lit le résultat du gestionnaire à partir du pont SDK des applications LLM :

```javascript
export default async function decorate(block, bridge) {
  if (!bridge?.isConnected || bridge.hostContext?.preview === true) {
    // Render the block's local preview data instead.
    return;
  }
  const result = await bridge.toolResult;
  const records = result?.structuredContent?.records ?? [];

  // Render records.
}
```

Les gestionnaires générés actuels utilisent un objet contenant `arguments` et `records`. Les applications existantes peuvent utiliser une forme différente. Vérifiez donc les fichiers générés avant de les modifier. La modification d’un nom ou d’un type de champ peut rompre le widget. Mettez à jour le gestionnaire, le widget et les tests ensemble.

## Remplacer les exemples de données

Les gestionnaires générés chargent des enregistrements représentatifs à partir de `data.json`. Remplacez la recherche de données d’exemple par un appel côté serveur à votre système. L’exemple suivant utilise une API de catalogue HTTPS publique et lit son origine non sensible à partir d’une variable d’application. Adaptez la validation de la réponse à votre API.

```javascript
const { getVariable } = require('@adobe/llm-apps-runtime');

module.exports = async ({ query = '' } = {}, extra) => {
  const normalizedQuery = typeof query === 'string' ? query.trim() : '';
  if (!normalizedQuery || normalizedQuery.length > 200) {
    return {
      content: [{ type: 'text', text: 'Enter a valid product search.' }],
      structuredContent: { arguments: {}, records: [] },
      isError: true
    };
  }

  const knownArguments = { query };
  try {
    const apiOrigin = getVariable(extra, 'PRODUCT_API_ORIGIN');
    if (!apiOrigin) {
      throw new Error('Missing configuration');
    }
    const origin = new URL(apiOrigin);
    if (origin.protocol !== 'https:' || origin.username || origin.password) {
      throw new Error('Invalid API origin');
    }
    const url = new URL('/v1/products', origin);
    url.searchParams.set('query', normalizedQuery);

    const response = await fetch(url, {
      signal: AbortSignal.timeout(8000)
    });
    if (!response.ok) {
      throw new Error('Request failed');
    }
    const payload = await response.json();
    if (!payload || !Array.isArray(payload.products)
        || !payload.products.every((product) =>
          product
          && typeof product.id === 'string'
          && typeof product.name === 'string'
          && typeof product.price === 'string')) {
      throw new Error('Unexpected response');
    }

    const records = payload.products.map(({ id, name, price }) => ({
      id,
      name,
      price
    }));

    return {
      content: [
        { type: 'text', text: `Found ${records.length} matching products.` }
      ],
      structuredContent: { arguments: knownArguments, records }
    };
  } catch {
    return {
      content: [{ type: 'text', text: 'Product search is unavailable. Try again later.' }],
      structuredContent: { arguments: knownArguments, records: [] },
      isError: true
    };
  }
};
```

Conserver l’accès réseau protégé dans le gestionnaire. Ne placez jamais les informations d’identification d’API dans le JavaScript de widget ou le contrôle de code source. Pour les demandes effectuées pour le compte d&#39;un utilisateur connecté, voir [Authentifier les utilisateurs finaux](/help/guides/authentication.md).

Pour les paramètres non sensibles dont votre gestionnaire a besoin, tels qu’une URL de service, voir [Configurer les variables et secrets d’application](/help/guides/app-variables.md). Configurez les variables séparément pour chaque environnement. Les modifications apportées aux variables prennent effet lors du prochain déploiement. L’interface utilisateur actuelle prend uniquement en charge l’ajout de variables. N’utilisez pas de variables pour stocker les informations d’identification d’API.

## Gérer les états attendus

Conserver une forme de sortie prévisible pour chaque résultat.

### Résultats trouvés

```javascript
{
  content: [{ type: 'text', text: 'Found 3 products.' }],
  structuredContent: { arguments: { query: 'coffee' }, records: [...] }
}
```

### Aucun résultat

```javascript
{
  content: [{ type: 'text', text: 'No matching products were found.' }],
  structuredContent: { arguments: { query: 'coffee' }, records: [] }
}
```

Une forme de résultat prévisible permet au widget de rendre un état vide sans deviner s’`records` existe.

Pour les échecs de service, renvoyez un résultat d’erreur sécurisé avec `isError: true`, comme dans l’exemple. Vérifiez `result.isError` dans le widget afin qu’une requête ayant échoué ne soit pas présentée comme une recherche vide réussie. N’exposez pas les traces de pile, les informations d’identification, les hôtes internes ou les corps de réponse en amont.

## Tester le contrat

Mettez à jour les tests générés chaque fois que le gestionnaire change. Couverture :

- Arguments valides et non valides.
- Résultats et états sans résultats.
- Échecs d’API et délais d’expiration.
- Réponses API incorrectes.
- Présence de `content` dans chaque résultat.
- Un objet brut pour `structuredContent`.
- Forme attendue par le widget.

Exécutez la suite de tests configurée du référentiel :

```bash
npm test
```

Si un test généré importe du `node:test` plutôt que du Jest, exécutez-le avec `node --test test/actions/<action-slug>.test.js` et vérifiez que son chemin d’importation est résolu à partir de `test/actions/`. Utilisez l’exécuteur de test approprié au fichier ; la commande `npm test` du modèle standard exécute Jest.

Pour les tests MCP locaux, voir [Développement et test des gestionnaires locaux](/help/reference/development.md).

## Déployer la modification

1. Validez et envoyez les modifications du gestionnaire.
2. Si vous modifiez la forme de données, mettez à jour et poussez le widget.
3. [Déployez l’application](/help/guides/deploy-your-app.md) dans l’environnement intermédiaire.
4. Testez dans une plateforme LLM prise en charge. Voir [Test dans le ChatGPT](/help/guides/test-in-chatgpt.md) ou [Test dans Claude](/help/guides/test-in-claude.md).
5. Une fois les tests réussis dans l’environnement intermédiaire, effectuez le déploiement en production.

Voir ensuite [Personnaliser un widget généré](/help/guides/widgets.md).
