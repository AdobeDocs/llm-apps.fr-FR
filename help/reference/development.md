---
title: Développement et test des gestionnaires locaux
description: Structure de projet de gestionnaire, commandes de serveur local, tests MCP et tests unitaires pour les applications Adobe LLM.
source-git-commit: eec74b87457bc852d7a8dd0e46c2a4385a93ae0a
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 2%

---


# Développement et test des gestionnaires locaux {#development}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Utilisez cette référence lors du développement local de gestionnaires. Pour le contrat de résultat du gestionnaire, voir [Personnaliser un gestionnaire généré](/help/guides/customize-handler.md).

## Exigences

- Node.js 24 ou version ultérieure.
- npm.
- Un clone local du référentiel du gestionnaire lié.

## Structure du projet

Votre référentiel lié suit cette disposition :

```
your-llm-app/
├── entry.js                   # Webpack entry — do not modify
├── actions/                   # One folder per action
│   └── echo/
│       └── index.js           # Example handler
├── test/
│   ├── actions/
│   │   └── echo.test.js
│   ├── fixtures/
│   │   └── actions.json
│   ├── html-transform.js
│   ├── jest.setup.js
│   └── server.test.js
├── server/
│   └── local.js               # Local dev server (port 9080)
├── actions.json               # Gitignored — optional local metadata
├── app.config.yaml            # Adobe I/O Runtime config
├── webpack.config.js
└── package.json
```

Points clés :

- **`entry.js`** est le point d’entrée du webpack. Au moment de la création, il détecte chaque fichier `actions/*/index.js` et les regroupe dans un seul `dist/index.js`. Ne pas modifier.
- **`actions.json`** est ignoré. Le pipeline de déploiement l’écrit automatiquement à partir des métadonnées d’action dans [!DNL LLM Apps].
- **Tests** vivent sous `test/actions/`, **pas** à l’intérieur du `actions/`. Webpack regroupe tout ce qui se trouve sous `actions/` dans l’artefact déployé ; les tests de colocalisation les expédient vers [!DNL Adobe I/O Runtime].

## Développement local

Vous pouvez développer et tester des gestionnaires localement sans les informations d’identification Adobe :

```bash
npm install
npm run dev:local
```

Cette opération crée le projet avec webpack et démarre un serveur HTTP Node.js simple sur `http://localhost:9080`. Le serveur détecte automatiquement vos fichiers de gestionnaire sous `actions/` et les enregistre en tant qu’outils MCP.

### Comportement des métadonnées locales

L’interface utilisateur actuelle ne fournit pas de téléchargement `actions.json`. Vous pouvez exécuter le serveur local sans ce fichier ; il détecte les gestionnaires sous `actions/` et les enregistre avec un minimum de métadonnées.

Sans `actions.json`, les arguments d’action locale ne sont pas validés par rapport au schéma d’entrée de l’interface utilisateur. Les tests unitaires et d’intégration utilisent des `test/fixtures/actions.json` pour les métadonnées représentatives.

### Test avec curl

```bash
# List all registered tools
curl -sX POST "http://localhost:9080" \
  -H 'content-type: application/json' \
  -H 'accept: application/json;q=1.0, text/event-stream;q=0.5' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}'

# Call the boilerplate echo action
curl -sX POST "http://localhost:9080" \
  -H 'content-type: application/json' \
  -H 'accept: application/json;q=1.0, text/event-stream;q=0.5' \
  -d '{"jsonrpc":"2.0","id":2,"method":"tools/call","params":{"name":"echo","arguments":{"message":"hello"}}}'
```

### Tester avec MCP Inspector

```bash
npx @modelcontextprotocol/inspector
```

Définissez **Type de transport** sur `streamable-http` et **URL** sur `http://localhost:9080`.

## Tests

Les tests unitaires du gestionnaire sont actifs sous `test/actions/` et reflètent la disposition `actions/` :

```javascript
// test/actions/echo.test.js
const handler = require('../../actions/echo/index.js')

test('echoes the message', async () => {
  const result = await handler({ message: 'hello' })
  expect(result.content[0].text).toBe('Echo: hello')
})

test('always returns content parts', async () => {
  const result = await handler({})
  expect(Array.isArray(result.content)).toBe(true)
})
```

Exécutez des tests avec :

```bash
npm test                                      # all tests
npx jest test/actions/echo                   # one action only
```

Une fois les tests locaux réussis, envoyez les modifications et suivez [Déployer les modifications](/help/guides/deploy-your-app.md).

