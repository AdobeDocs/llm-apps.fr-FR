---
title: Développement et test des gestionnaires locaux
description: Structure de projet de gestionnaire, commandes de serveur local, tests MCP et tests unitaires pour les applications Adobe LLM.
source-git-commit: 0f9dd0a5b1008e183eb8ab5a4c2eae34a78923bb
workflow-type: tm+mt
source-wordcount: '664'
ht-degree: 0%
---

# Développement et test des gestionnaires locaux {#development}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Utilisez cette référence lors du développement local de gestionnaires. Pour le contrat de résultat du gestionnaire, voir [Personnaliser un gestionnaire généré](/help/guides/customize-handler.md).

## Conditions requises

- Node.js 24 ou version ultérieure.
- npm.
- Un clone local du référentiel du gestionnaire lié.

Pour l’infrastructure gérée par Adobe, demandez l’accès au référentiel avant de suivre cette référence. L’exécution du gestionnaire local ne nécessite pas d’informations d’identification Adobe, mais les services en amont protégés nécessitent toujours leur propre accès autorisé.

## Structure du projet

Le modèle de gestionnaire actuel suit cette disposition :

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

- **`entry.js`** est le point d’entrée du webpack. Au moment de la création, il détecte chaque fichier `actions/*/index.js` et les regroupe dans un seul `dist/index.js`. Ne modifiez pas le point d’entrée.
- **`actions.json`** est ignoré. Le pipeline de déploiement l’écrit automatiquement à partir des métadonnées d’action dans [!DNL LLM Apps].
- **Tests** vivent sous `test/actions/`, **pas** à l’intérieur du `actions/`. Le build détecte les points d’entrée de gestionnaire et regroupe leurs importations ; conservez les tests en dehors de cette arborescence de détection.
- **Chemins d’action** utilisez l’identifiant de code ou sa forme avec traits d’union. Par exemple, `search_products` est résolu sur `actions/search_products/index.js` ou, pour le code généré, sur `actions/search-products/index.js`. Le nom d’affichage n’est pas la clé de recherche.
- **Les données de prise en charge** telles qu’un `data.json` généré, sont regroupées lorsque le gestionnaire les importe.

## Développement local

Pour développer et tester les gestionnaires localement sans les informations d’identification Adobe, exécutez :

```bash
npm install
npm run dev:local
```

La commande de développement crée le projet avec webpack et démarre un serveur HTTP Node.js simple sur `http://localhost:9080`. Arrêtez et réexécutez `npm run dev:local` après modification des métadonnées locales ou du gestionnaire ; la commande ne regarde pas les fichiers.

Pour utiliser un autre port :

```bash
PORT=9081 npm run dev:local
```

### Comportement des métadonnées locales

L’interface utilisateur actuelle ne fournit pas de téléchargement `actions.json`, même si les bannières standard README et de démarrage d’exécution en mentionnent un. Le serveur local démarre également sans fichier renseigné : la version crée des `{"actions":[]}` lorsque le fichier est manquant et l’exécution retourne à la découverte du gestionnaire.

La découverte est utile pour les gestionnaires de liste, mais il ne s’agit pas d’un mode de test d’arguments complet. Sans schéma d’entrée, le SDK MCP appelle le gestionnaire avec le contexte d’exécution comme premier argument au lieu d’utiliser la signature `(args, extra)` normale. Le SDK ne transfère pas les arguments d’action fournis.

Pour les appels MCP avec arguments , créez un `actions.json` **local uniquement** avec des métadonnées représentatives correspondant à l’éditeur d’action. Pour le gestionnaire de `echo` standard :

```json
{
  "actions": [
    {
      "name": "echo",
      "description": "Echo the supplied message.",
      "inputSchema": {
        "type": "object",
        "properties": {
          "message": { "type": "string" }
        },
        "required": ["message"]
      }
    }
  ]
}
```

Lorsque le tableau `actions` n’est pas vide, le runtime enregistre uniquement les actions nommées dans le tableau. Utilisez l’identifiant de code de chaque action comme `name`. Même une action sans paramètre a besoin d’un schéma d’objet avec `"properties": {}` pour conserver la signature normale du gestionnaire.

Les métadonnées locales ne mettent pas à jour la configuration de l’interface utilisateur ou du déploiement. Le runtime valide les types pris en charge, les champs obligatoires, les énumérations et les éléments de tableau, mais pas toutes les contraintes de schéma JSON. Il valide les longueurs, les plages, les objets imbriqués et les règles métier dans le gestionnaire . Les tests d’intégration standard utilisent `test/fixtures/actions.json` comme métadonnées représentatives.

### Variables d’application locale

Les variables d’application atteignent le gestionnaire par `extra.variables`, et non par `process.env`. Lisez-les avec les `getVariable(extra, name)` de `@adobe/llm-apps-runtime`. Pour simuler un paramètre non sensible après la création :

```bash
npm run build
node server/local.js \
  --param 'LLMA_VARIABLE_NAMES=["GREETING_PREFIX"]' \
  --param GREETING_PREFIX=Howdy
```

Fournissez le manifeste JSON et la valeur nommée . Incluez le gestionnaire dans le `actions.json` local avec un schéma d’entrée lors du test de son contrat de `(args, extra)` normal. Les indicateurs de ligne de commande ne chargent pas les paramètres de l’application à partir de la plateforme. Voir [Configurer les variables et secrets d’application](/help/guides/app-variables.md) pour les paramètres de déploiement spécifiques à un environnement.

### Test avec curl

Après avoir ajouté les métadonnées de `echo` locales ci-dessus, créez et démarrez le serveur, puis exécutez :

```bash
# List all registered tools
curl -sX POST "http://localhost:9080" \
  -H 'content-type: application/json' \
  -H 'accept: application/json;q=1.0, text/event-stream;q=0.5' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}'

# Call the boilerplate echo action (returns "Echo: hello")
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

Les tests unitaires du gestionnaire sont actifs sous `test/actions/` et appellent directement la fonction exportée, sans validation de schéma MCP :

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
npm test                                 # configured Jest suite
npm test -- --runInBand test/actions/echo.test.js  # one boilerplate action
```

Certains tests générés utilisent l’exécuteur de test intégré de Node au lieu de Jest. Vérifiez les imports de test et utilisez l’exécuteur correspondant, par exemple `node --test test/actions/search-products.test.js` pour un `node:test` d’import de fichier. Vérifiez les imports relatifs générés après avoir placé le fichier sous `test/actions/`.

Les tests unitaires seuls ne vérifient pas les métadonnées, la validation MCP, le chargement des widgets ou le comportement de l’hôte. Testez le point d’entrée local avec des métadonnées représentatives, puis testez l’application déployée sur une plateforme LLM prise en charge.

Une fois les tests locaux réussis, envoyez les modifications et suivez [Déployer les modifications](/help/guides/deploy-your-app.md).
