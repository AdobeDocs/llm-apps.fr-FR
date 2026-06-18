---
title: Développement d’applications Adobe LLM
description: Structure de projet, workflow de développement local et configuration des tests pour le code du gestionnaire d’applications Adobe LLM.
source-git-commit: 1a99e2e80e50a3bcf9ce6fb910365202bf06e113
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 4%

---


# Développement {#development}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Cette section couvre la structure de projet du gestionnaire, le workflow de développement local et la configuration des tests pour [!DNL Adobe LLM Apps]. Pour le contrat du gestionnaire et l’exemple de code, voir [Écrire le gestionnaire d’actions](/help/guides/write-action-handler.md).

## Structure du projet

Votre référentiel lié suit cette disposition :

```
your-llm-app/
├── entry.js                   # Webpack entry — do not modify
├── actions/                   # One folder per action
│   ├── search-products/
│   │   └── index.js           # Handler (async function)
│   ├── get-product-details/
│   │   └── index.js
│   └── echo/
│       └── index.js
├── test/
│   ├── actions/
│   │   └── search-products.test.js
│   ├── fixtures/
│   │   └── actions.json
│   ├── html-transform.js
│   ├── jest.setup.js
│   └── server.test.js
├── server/
│   └── local.js               # Local dev server (port 9080)
├── actions.json               # Gitignored — local copy of UI metadata
├── app.config.yaml            # Adobe I/O Runtime config
├── webpack.config.js
└── package.json
```

Points clés :

- **`entry.js`** est le point d’entrée du webpack. Au moment de la création, il détecte chaque fichier `actions/*/index.js` et les regroupe dans un seul `dist/index.js`. Ne pas modifier.
- **`actions.json`** est ignoré. Téléchargez-le à partir de la page Actions de l’interface utilisateur pour le développement local. Pour les déploiements , le pipeline l’écrit automatiquement à partir de l’API.
- **Tests** vivent sous `test/actions/`, **pas** à l’intérieur du `actions/`. Webpack regroupe tout ce qui se trouve sous `actions/` dans l’artefact déployé ; les tests de colocalisation les expédient vers [!DNL Adobe I/O Runtime].

## Développement local

Vous pouvez développer et tester des gestionnaires localement sans les informations d’identification Adobe :

```bash
npm install
npm run dev:local
```

Cette opération crée le projet avec webpack et démarre un serveur HTTP Node.js simple sur `http://localhost:9080`. Le serveur détecte automatiquement vos fichiers de gestionnaire sous `actions/` et les enregistre en tant qu’outils MCP.

### Télécharger `actions.json`

Pour que le serveur local soit informé des métadonnées de votre action (nom, description, schéma d’entrée), téléchargez les `actions.json` à partir de la page Actions de l’interface utilisateur de [!DNL LLM Apps] et placez-les à la racine du référentiel. Sans cela, le serveur découvre vos gestionnaires mais les enregistre avec un minimum de métadonnées.

Vous pouvez également copier des `actions.example.json` dans `actions.json` comme point de départ.

### Test avec curl

```bash
# List all registered tools
curl -sX POST "http://localhost:9080" \
  -H 'content-type: application/json' \
  -H 'accept: application/json;q=1.0, text/event-stream;q=0.5' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}'

# Call the search-products action
curl -sX POST "http://localhost:9080" \
  -H 'content-type: application/json' \
  -H 'accept: application/json;q=1.0, text/event-stream;q=0.5' \
  -d '{"jsonrpc":"2.0","id":2,"method":"tools/call","params":{"name":"search-products","arguments":{"category":"bagged-coffee"}}}'
```

### Tester avec MCP Inspector

```bash
npx @modelcontextprotocol/inspector
```

Définissez **Type de transport** sur `streamable-http` et **URL** sur `http://localhost:9080`.

## Tests

Les tests unitaires du gestionnaire sont actifs sous `test/actions/` et reflètent la disposition `actions/` :

```javascript
// test/actions/search-products.test.js
const handler = require('../../actions/search-products/index.js')

test('returns all products when no filter is given', async () => {
  const result = await handler({})
  expect(result.content[0].text).toContain('product')
  expect(result.structuredContent.products.length).toBeGreaterThan(0)
})

test('filters by category', async () => {
  const result = await handler({ category: 'bagged-coffee' })
  expect(result.structuredContent.products.every(
    (p) => p.category === 'bagged-coffee'
  )).toBe(true)
})

test('filters by query', async () => {
  const result = await handler({ query: 'dark-roast' })
  expect(result.structuredContent.products.length).toBeGreaterThan(0)
})

test('returns empty result for unknown category', async () => {
  const result = await handler({ category: 'nonexistent' })
  expect(result.structuredContent.products).toHaveLength(0)
})
```

Exécutez des tests avec :

```bash
npm test                                      # all tests
npx jest test/actions/search-products        # one action only
```

## Déploiement

Vous ne créez ni ne déployez manuellement de contenu. Pour une présentation complète du pipeline de déploiement, voir [&#x200B; Déployer votre application &#x200B;](/help/guides/deploy-your-app.md).

Votre workflow quotidien est :

| Étape | Action |
|------|--------|
| &#x200B;1. Gestionnaire d’écriture ou de modification | `actions/<name>/index.js` |
| &#x200B;2. Téléchargement de métadonnées | Page Actions → **Télécharger actions.json** |
| &#x200B;3. Tester localement | `npm run dev:local` |
| &#x200B;4. Code push | `git push` |
| &#x200B;5. Déployer | Page Détails de l’application → **[!UICONTROL Déployer]** |

