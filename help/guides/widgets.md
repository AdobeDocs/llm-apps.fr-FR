---
title: Configuration du widget (EDS)
description: Découvrez comment configurer un projet de widget Edge Delivery Services et implémenter le contrat de bloc pour le rendu des réponses visuelles dans les plateformes LLM.
source-git-commit: 483a71f5f1de5caf1bd89b26f4d67d2d5a0aa15a
workflow-type: tm+mt
source-wordcount: '1214'
ht-degree: 1%

---


# Configuration du widget (EDS)

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta. Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit.

Ce guide explique comment créer un widget EDS de bout en bout : de la configuration de votre action dans l’interface utilisateur de [!DNL LLM Apps] à la configuration de votre projet EDS, en passant par l’écriture du code de bloc qui effectue le rendu de vos données dans la plateforme LLM. Pour une présentation générale, voir [Concepts de base](/help/overview/overview.md#widgets-eds).

## Le [!DNL LLM Apps] SDK

Tout commence par le package npm [`@adobe/llmapps-sdk`](https://www.npmjs.com/package/@adobe/llmapps-sdk). Le SDK est la bibliothèque JavaScript qui alimente le canal de communication bidirectionnel entre le widget et l’hôte LLM.

Le SDK est également livré avec `aem-embed.js`, le point d’entrée spécifique à EDS qui connecte le SDK au pipeline de blocs EDS standard. Lorsque vous `npm install @adobe/llmapps-sdk`, un script de post-installation copie automatiquement deux fichiers dans votre projet :

```
scripts/
└── llm-apps/
    ├── aem-embed.js     ← EDS widget entry point, ships with the SDK
    └── llmapps-sdk.js   ← core SDK, loaded internally by aem-embed.js
```

Dans les projets EDS, **vous n’utilisez jamais le SDK directement dans votre code bloc.** `aem-embed.js` crée et gère la connexion SDK et transmet une instance `LLMApp` entièrement connectée à votre bloc en tant qu’argument `bridge` dans `decorate(block, bridge)`. L’API SDK complète est disponible sur `bridge` ; aucune importation n’est nécessaire.

Si vous créez un widget **sans EDS** (un bundler standard ou un projet TypeScript), vous pouvez utiliser directement SDK :

```javascript
import { LLMApp } from '@adobe/llmapps-sdk';

const app = new LLMApp({ appInfo: { name: 'MyWidget', version: '1.0.0' } });
await app.connect();

const { structuredContent } = await app.toolResult;
```

## Comment tout se tient

Lorsque l’IA appelle votre action et que le gestionnaire renvoie des `structuredContent`, la plateforme LLM effectue le rendu d’un widget interactif dans la conversation. Trois éléments permettent de faire fonctionner ce système :

**L’interface utilisateur [!DNL LLM Apps]** — lorsque vous créez une action, vous saisissez une **[!UICONTROL URL de script]** et une **[!UICONTROL URL de widget]** dans l’onglet Métadonnées de widget. L’URL du script pointe vers `aem-embed.js`, le fichier fourni avec le SDK et résidant dans votre référentiel EDS à l’adresse `scripts/llm-apps/aem-embed.js`. Cette option indique à la plateforme LLM le script à charger lorsque l’action est appelée.

**`aem-embed.js`** : la plateforme LLM charge ce script dans une surface de widget en sandbox. `aem-embed.js` est un élément HTML personnalisé (`<aem-embed>`) qui agit comme point d’entrée basé sur EDS pour votre widget. Il effectue la liaison avec l’hôte LLM à l’aide de SDK, supprime le pipeline de page EDS normal (pas d’en-tête/de pied de page), récupère le contenu de votre page EDS à partir de l’URL du widget, exécute le pipeline de bloc EDS et diffuse un objet Live `bridge` à la fonction `decorate()` de chaque bloc.

**Votre code de bloc** — vous écrivez un bloc EDS standard qui exporte une fonction `decorate(block, bridge)`. L’`bridge` est l’instance SDK connectée : elle vous donne le résultat structuré de l’action et vous permet de renvoyer des messages dans la conversation.

## Ajouter à un projet EDS existant

Si vous disposez déjà d’un projet EDS, vous ne pouvez commencer à écrire des blocs qu’en deux étapes.

1. Installez `@adobe/llmapps-sdk`. Le script de post-installation copie `aem-embed.js` et `llmapps-sdk.js` dans `scripts/llm-apps/` :

   ```bash
   npm install @adobe/llmapps-sdk
   ```

2. Configurez les en-têtes CORS pour que la plateforme LLM puisse charger vos pages de widgets et scripts cross-origin (voir [Configurer les en-têtes CORS](#configure-cors-headers) ci-dessous).

Créez ensuite votre bloc suivant le contrat de ](#the-decorateblock-bridge-contract) créez la page du widget et saisissez les URL dans la boîte de dialogue Créer une action .[`decorate(block, bridge)`

## Configuration d’un nouveau projet EDS

### Création du référentiel

1. Créez un référentiel [!DNL GitHub] basé sur le modèle [AEM standard](https://github.com/adobe/aem-boilerplate).
2. Ajoutez l’application GitHub de synchronisation du code AEM [](https://github.com/apps/aem-code-sync) au référentiel.
3. Installez l’interface de ligne de commande AEM pour le développement local : `npm install -g @adobe/aem-cli`.
4. Installez `@adobe/llmapps-sdk`. Le script de post-installation copie `aem-embed.js` et `llmapps-sdk.js` dans `scripts/llm-apps/` :

   ```bash
   npm install @adobe/llmapps-sdk
   ```

Pour obtenir un guide complet sur les projets EDS, consultez le [tutoriel de développement ](https://www.aem.live/developer/tutorial) et [anatomie du projet](https://www.aem.live/developer/anatomy-of-a-project).

Une fois configuré, votre site EDS est disponible à l’adresse suivante :

- **Prévisualisation :** `https://main--<repo>--<owner>.aem.page/`
- **En direct :** `https://main--<repo>--<owner>.aem.live/`

### Structure du référentiel

```
my-brand-eds/
├── scripts/
│   ├── llm-apps/
│   │   ├── aem-embed.js           # Widget entry point — copied by post-install
│   │   └── llmapps-sdk.js         # Core SDK — copied by post-install
│   ├── aem.js                     # AEM core library
│   └── scripts.js                 # Site-level decoration and loading
├── blocks/
│   └── search-products/           # One folder per widget block
│       ├── search-products.js
│       └── search-products.css
├── styles/
│   └── styles.css
├── head.html
└── package.json
```

### Configuration des en-têtes CORS

Vos pages de widget EDS sont chargées dans une surface de widget en sandbox par la plateforme LLM. Le site EDS doit renvoyer les en-têtes `access-control-allow-origin` corrects afin que l’hôte puisse récupérer votre contenu de widget à l’origine.

Les en-têtes sont configurés via le panneau d’administration d’AEM à l’adresse `admin.hlx.page` à l’aide du [Service de configuration](https://aem.live/docs/config-service-setup). Ajoutez des en-têtes de réponse personnalisés pour les chemins d’accès où vivent vos pages de widgets et vos scripts SDK :

```json
{
  "/<your-widget-pages-path>/**": [
    { "key": "access-control-allow-origin", "value": "*" }
  ],
  "/scripts/**": [
    { "key": "access-control-allow-origin", "value": "*" }
  ]
}
```

>[!NOTE]
>
>L’utilisation de `*` comme valeur d’origine est acceptable pour le contenu de widget public sur le domaine `.aem.live`. Si votre site contient du contenu protégé, limitez l’origine à des domaines spécifiques.

### Création de la page du widget

Créez une page dans votre outil de création EDS et ajoutez-y votre bloc. L’URL de la page devient l’**[!UICONTROL URL du widget]** que vous configurez dans l’action ; il s’agit de la seule connexion entre l’action et le bloc. Il n’est pas nécessaire d’attribuer un nom entre votre bloc et le nom de l’action.

![Création EDS — Bloc ajouté à la page du widget](/help/assets/guide-widget/aem-author.png)

### Saisissez les URL dans la boîte de dialogue Créer une action .

Après avoir configuré votre référentiel EDS, accédez à **URL des modèles de → de métadonnées de widget** lors de la création de votre action :

**[!UICONTROL URL du script]** — pointe vers `aem-embed.js` dans votre référentiel EDS. Il s’agit de la même valeur pour chaque action du même projet EDS :

```
https://main--<repo>--<owner>.aem.live/scripts/llm-apps/aem-embed.js
```

**[!UICONTROL URL du widget]** : URL de la page EDS que vous avez créée pour ce widget. Unique par action :

```
https://main--<repo>--<owner>.aem.live/<path-to-your-widget-page>
```

La plateforme LLM charge les `aem-embed.js` à partir de l’URL du script. `aem-embed.js` récupère ensuite le `.plain.html` à partir de l’URL du widget pour obtenir le contenu de votre bloc.

## Flux de données

Chemin complet de votre gestionnaire vers un widget rendu :

1. **Gestionnaire d&#39;action** renvoie `structuredContent` :

```javascript
// actions/search-products/index.js
return {
  structuredContent: {
    products: [
      { id: 'COF-001', name: 'Single Origin Ethiopian Coffee', price: '$18', rating: 4.7 },
      { id: 'COF-002', name: 'Colombia Huila Natural', price: '$22', rating: 4.5 },
    ],
    total: 2,
    category: 'coffee'
  }
};
```

1. **Plateforme LLM** ouvre une surface de widget et charge les `aem-embed.js` à partir de l’URL de script.

1. **`aem-embed.js`** se connecte à l’hôte via le SDK, récupère le `.plain.html` à partir de l’URL du widget, exécute le pipeline de bloc EDS et appelle `decorate(block, bridge)` sur votre bloc .

1. **Votre bloc** lit les données du `bridge.toolResult` et effectue le rendu de l’interface utilisateur.

1. **Interaction utilisateur** déclenche l’`bridge.sendMessage(...)` ou la `bridge.callTool(...)`, ce qui entraîne l’envoi d’une relance dans la conversation.

## Le contrat `decorate(block, bridge)`

Chaque bloc de widget EDS doit exporter une fonction de `decorate` par défaut. Il s’agit de la signature de bloc EDS standard, étendue avec un second argument : le `bridge` connecté, qui est une instance SDK [`LLMApp`](https://www.npmjs.com/package/@adobe/llmapps-sdk) avec l’API complète disponible :

```javascript
export default async function decorate(block, bridge) {
  // ...
}
```

`bridge` n’est présent que lors de l’exécution dans la surface du widget de plateforme LLM. Gardez toujours vos appels au pont afin que votre bloc soit également rendu lorsqu’il est prévisualisé directement dans un navigateur ou votre serveur de développement local.

### Effectuer le rendu des données à partir du résultat de l’action

`bridge.toolResult` est une promesse qui se résout avec le résultat complet renvoyé par votre gestionnaire, y compris `structuredContent`.

```javascript
const SAMPLE_PRODUCTS = [
  { id: 'COF-001', name: 'Single Origin Ethiopian Coffee', price: '$18', rating: 4.7 },
];

export default async function decorate(block, bridge) {
  let products = SAMPLE_PRODUCTS;

  if (bridge) {
    const result = await bridge.toolResult;
    products = result?.structuredContent?.products ?? [];
  }

  block.innerHTML = products.map(p => `
    <div class="product-card">
      <h3>${p.name}</h3>
      <p class="price">${p.price}</p>
      <button data-id="${p.id}">Tell me more</button>
    </div>
  `).join('');
}
```

### Application du thème de l’hôte

Appelez `bridge.applyHostStyles()` tôt dans la `decorate` pour injecter les variables et polices CSS de l’hôte (thème clair/sombre, typographie) dans le widget. Votre widget reste ainsi visuellement cohérent avec l’interface utilisateur de la plateforme LLM qui l’entoure.

```javascript
export default async function decorate(block, bridge) {
  if (bridge) {
    bridge.applyHostStyles();
  }
  // ...
}
```

Pour réagir aux modifications du thème au moment de l’exécution (par exemple, lorsque l’utilisateur ou l’utilisatrice passe du mode clair au mode sombre) :

```javascript
if (bridge) {
  bridge.onContextChange(ctx => {
    block.dataset.theme = ctx.theme; // 'light' | 'dark'
  });
}
```

### Envoyer un message de relance

`bridge.sendMessage(text)` injecte un message utilisateur dans la conversation. C’est la principale manière dont un widget déclenche d’autres interactions de l’IA, par exemple, lorsqu’un utilisateur clique sur une fiche produit pour demander des détails.

```javascript
block.querySelectorAll('button[data-id]').forEach(btn => {
  btn.addEventListener('click', () => {
    bridge.sendMessage(`Show me details for product ${btn.dataset.id}`);
  });
});
```

### Appeler directement une autre action

`bridge.callTool(name, args)` appelle une autre action à partir du widget sans passer par un message de l’utilisateur. Utile pour charger les données associées à la demande.

```javascript
btn.addEventListener('click', async () => {
  const result = await bridge.callTool('get-product-details', { id: product.id });
  renderDetails(result.structuredContent);
});
```

### Redimensionnement automatique du widget

La plateforme LLM dimensionne le widget en fonction de ce que vous signalez. Utilisez `bridge.autoResize(element)` pour que la hauteur du widget reste synchronisée à mesure que votre contenu change : il utilise un `ResizeObserver` en interne. Appelez-le après votre rendu initial :

```javascript
export default async function decorate(block, bridge) {
  // ... render content ...

  if (bridge) {
    bridge.autoResize(block);
  }
}
```

Ou signaler manuellement une taille fixe :

```javascript
bridge.reportSize(block.offsetWidth, block.offsetHeight);
```

### Mode Aperçu et développement local

Lors de la prévisualisation d’une page EDS directement dans un navigateur ou sur le serveur de développement local, `bridge` est `undefined`. Utilisez l’exemple de modèle de secours de données illustré ci-dessus afin que votre bloc s’affiche immédiatement sans gestionnaire dynamique.

Pour démarrer un serveur de développement local, procédez comme suit :

```bash
npm install -g @adobe/aem-cli
aem up
```

Cela s’ouvre `http://localhost:3000` où vous pouvez accéder à vos pages de widget et voir le rendu des blocs avec des exemples de données. Les modifications apportées au bloc JS et CSS sont répercutées immédiatement.

## Étapes suivantes

- [Guide : écriture du gestionnaire d’actions](/help/guides/write-action-handler.md)

