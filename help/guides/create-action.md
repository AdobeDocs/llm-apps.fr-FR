---
title: Création d’une action à partir de zéro
description: Définissez des métadonnées d’action, implémentez son gestionnaire, connectez un widget EDS, testez-le et déployez-le avec les applications Adobe LLM.
source-git-commit: 0f9dd0a5b1008e183eb8ab5a4c2eae34a78923bb
workflow-type: tm+mt
source-wordcount: '1453'
ht-degree: 0%
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

Ajoutez une fonctionnalité que la plateforme n’a pas créée. Définissez l’action dans [!DNL LLM Apps], écrivez son gestionnaire dans le référentiel lié et ajoutez un widget si nécessaire.

**Dans ce guide :** découvrez comment définir une action, implémenter son gestionnaire et connecter un widget facultatif. Testez l’action localement, déployez l’application et essayez l’action sur une plateforme LLM prise en charge.

Pour votre première application, commencez par [Configuration de l’infrastructure](/help/guides/create-app.md), puis [Création d’actions](/help/guides/creating-actions.md). Le guide Créer des actions explique la génération automatique à partir d’un site web et la création manuelle.

## Avant de commencer

Vous avez besoin des éléments suivants :

- Une application LLM existante.
- Référentiel de gestionnaire lié.
- Un clone local du référentiel avec ses dépendances installées.
- Un projet EDS si l’action affiche un widget.
- Une API ou une source de données claire pour les résultats de production.

## Planifier l’action

Concevez chaque action autour d’une tâche utilisateur claire. Avant d’ouvrir l’interface utilisateur, définissez les éléments suivants :

- **Intention** — Ce que l&#39;utilisateur tente d&#39;accomplir.
- **Description** — quand sélectionner l’action.
- **Entrées** — informations minimales requises de l&#39;utilisateur.
- **Résultat** : le texte et les données structurées renvoyés par le gestionnaire.
- **Comportement** : indique si l’action lit des données, modifie des données ou appelle des systèmes externes.
- **Widget** — si le résultat nécessite une interface visuelle.

Par exemple, utilisez ce plan pour une action **Rechercher des produits** :

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

Gardez les tâches liées mais différentes séparées. Utilisez des actions distinctes pour la recherche de produits et l’achat de produits, car les tâches comportent des entrées, des risques et des exigences de confirmation différents.

## Création des métadonnées d’action

Ouvrez l’application et sélectionnez **[!UICONTROL Actions]**, puis **[!UICONTROL Créer une action]**.

La boîte de dialogue de création contient les onglets **[!UICONTROL Action]** et **[!UICONTROL Métadonnées de widget]**. Développez chaque section pour configurer ses champs.

### Saisir des informations de base

![Créer une action — informations de base](/help/assets/guide-create-action/action-basic-info.png)

Enter :

- **[!UICONTROL Nom de l’action]** — Nom court de la tâche, tel que *Rechercher des produits*.
- **[!UICONTROL Description]** — expliquez quand utiliser l&#39;action et ce qu&#39;elle renvoie.

La boîte de dialogue génère un **[!UICONTROL identifiant de code]** à partir du nom, tel que `search-products`. L’identifiant est fixe après création et doit être unique dans l’application. Utilisez l’identifiant exact du dossier de gestionnaire.

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

Pour les actions avec des widgets, ajoutez **[!UICONTROL Description du widget]**. La description du widget diffère de la description de l’action :

- La **Description de l’action** aide le modèle à décider quand appeler l’action.
- **Description du widget** mappe à `_meta["openai/widgetDescription"]` et résume ce que le composant rendu affiche, réduisant ainsi la narration répétée.

[!DNL LLM Apps] applique la description du widget en tant que métadonnées de composant. Ne renvoyez pas la description à partir du gestionnaire .

### Configuration de la visibilité

- **[!UICONTROL Exposer au modèle d’IA]** permet au modèle de sélectionner l’action.
- **[!UICONTROL Afficher en tant que widget dans la surface de l’application]** affiche le widget configuré.

Pour une action en texte seul, sélectionnez également **[!UICONTROL Aucun widget]** sous **[!UICONTROL Métadonnées de widget]** > **[!UICONTROL Type]**. Les contrôles de visibilité ne choisissent pas le type de widget de l’action.

### Ajouter des paramètres d’entrée

Ajoutez un paramètre pour chaque valeur acceptée par le gestionnaire. Chaque paramètre nécessite :

- **Nom** — Clé reçue par le gestionnaire.
- **Type** — Chaîne, Nombre, Entier ou Booléen.
- **Description** — Comment extraire la valeur.
- **Obligatoire** — si l&#39;action requiert la valeur.

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

La collecte d’intention utilisateur est activée par défaut. Consultez **[!UICONTROL Description de l’intention de l’utilisateur]**, qui contient les instructions pour demander le résumé. Limitez les instructions à une intention non sensible et anonymisée et décrivez votre collecte de données dans votre politique de confidentialité. La description accepte jusqu’à 2 000 caractères.

![Créer une action — Analyse des intentions de l’utilisateur](/help/assets/guide-create-action/action-analytics-user-intent.png)

Pour obtenir des définitions de champ complètes, voir [Champs d’action et de widget](/help/reference/reference-docs.md).

## Configuration du widget

Pour une action en mode texte uniquement, ouvrez **[!UICONTROL Métadonnées de widget]** et définissez **[!UICONTROL Type]** sur **[!UICONTROL Aucun widget]**. Ignorez ensuite les paramètres de widget restants.

Ouvrez **[!UICONTROL Métadonnées de widget]**.

![Créer une action — métadonnées de widget](/help/assets/guide-create-action/widget-metadata.png)

Configurer :

- **[!UICONTROL Type]** — sélectionnez **[!UICONTROL Bloc Edge Delivery Services]** pour un widget interactif.
- **[!UICONTROL Domaine du widget]** — origine du sandbox du widget. L’interface utilisateur l’identifie comme requise pour l’envoi de l’application et comme unique par application ; elle n’est pas identique à l’URL de la page EDS.
- **Préfère la bordure** — demande un conteneur bordé dans l&#39;hôte.

Développez **[!UICONTROL URL du modèle]** et saisissez :

- **[!UICONTROL URL du script]** — point d&#39;entrée du widget EDS.
- **[!UICONTROL URL du widget]** : page EDS publiée pour cette action.

![Créer une action : URL de script et de widget pour un widget EDS](/help/assets/guide-create-action/widget-template-urls.png)

Les URL standard sont les suivantes :

```text
Script URL:
https://main--<repo>--<owner>.aem.live/scripts/aem-embed.js

Widget URL:
https://main--<repo>--<owner>.aem.live/<widget-page>
```

Octroyez uniquement les autorisations de navigateur et les domaines CSP requis.

Développez **[!UICONTROL Autorisations (spécifiques au MCP)]** uniquement si le widget a besoin de fonctionnalités de navigateur telles que la géolocalisation. Laissez les autorisations inutilisées désactivées.

![Créer une action — Autorisations de navigateur facultatives](/help/assets/guide-create-action/widget-permissions.png)

Sous **[!UICONTROL Configuration CSP]**, ajoutez les origines requises pour les ressources et les requêtes réseau. Un widget qui charge des ressources externes peut ne pas s’afficher si sa CSP est incomplète.

![Créer une action — Listes de domaines de la politique de sécurité du contenu](/help/assets/guide-create-action/widget-csp.png)

Si le projet ou la page de widget EDS n’existe pas encore, complétez [Apportez votre propre projet EDS](/help/guides/bring-your-own-eds.md), puis revenez à l’action.

## Enregistrer l’action

Sélectionnez **[!UICONTROL Créer une action]**. L’action s’affiche sur la page Actions avec un badge **Non déployé**.

L’enregistrement crée les métadonnées, mais l’action a toujours besoin d’un gestionnaire.

Lorsque vous rouvrez l’action enregistrée, l’éditeur dispose des onglets **[!UICONTROL Détails du widget LLM]**, **[!UICONTROL Détails de l’action]** et **[!UICONTROL Analytics]**. Pour enregistrer les modifications ultérieures, sélectionnez **[!UICONTROL Enregistrer les modifications]**. La boîte de dialogue de création et l’éditeur d’action enregistrée ont des dispositions différentes.

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

La plateforme LLM lit `content` comme texte de remplacement :

```javascript
content: [
  { type: 'text', text: 'Found 3 matching products.' }
]
```

Renvoyez toujours des `content` utiles, même si l’action comporte un widget.

### `structuredContent` de retour

Le widget utilise `structuredContent` comme objet simple :

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

Conserver les appels API dans le gestionnaire côté serveur. Cet exemple appelle une API de produit publique. Configurez l’origine HTTPS de l’API dans une [variable d’application](/help/guides/app-variables.md) nommée `PRODUCT_API_ORIGIN`. L’exemple accepte les paramètres `query` et `category` définis ci-dessus, nécessite au moins un paramètre et ne nécessite pas d’informations d’identification.

```javascript
const { getVariable } = require('@adobe/llm-apps-runtime');

module.exports = async ({ query = '', category = '' } = {}, extra) => {
  const normalizedQuery = typeof query === 'string' ? query.trim() : '';
  const normalizedCategory = typeof category === 'string' ? category.trim() : '';
  if (typeof query !== 'string' || typeof category !== 'string'
      || (!normalizedQuery && !normalizedCategory)
      || normalizedQuery.length > 200 || normalizedCategory.length > 100) {
    return {
      isError: true,
      content: [{ type: 'text', text: 'Enter a valid product search.' }],
      structuredContent: { products: [], total: 0 }
    };
  }

  const apiOrigin = getVariable(extra, 'PRODUCT_API_ORIGIN');
  if (!apiOrigin) {
    throw new Error('Product API configuration is unavailable.');
  }

  const origin = new URL(apiOrigin);
  if (origin.protocol !== 'https:') {
    throw new Error('Product API configuration must use HTTPS.');
  }

  const url = new URL('/v1/products', origin);
  if (normalizedQuery) {
    url.searchParams.set('query', normalizedQuery);
  }
  if (normalizedCategory) {
    url.searchParams.set('category', normalizedCategory);
  }

  const response = await fetch(url, {
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

Ne placez pas les informations d’identification d’API dans le code source, les métadonnées d’action, le JavaScript de widget, les journaux ou les erreurs rencontrées par l’utilisateur ou l’utilisatrice. L’interface utilisateur **[!UICONTROL Variables et secrets]** ne prend actuellement en charge que les variables non sensibles, et non les secrets. Ne stockez pas de jeton API dans une variable. Si votre API en amont requiert des informations d’identification, organisez un mécanisme d’informations d’identification côté serveur approuvé avant de le connecter.

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
3. Testez l’application dans [ChatGPT](/help/guides/test-in-chatgpt.md) ou [Claude](/help/guides/test-in-claude.md).
4. Vérifiez la sélection de l’action avec les invites correspondant à l’action et les invites non liées qui ne l’appellent pas.
5. Une fois l’étape réussie, déployez en production.

Si des métadonnées existent sans gestionnaire correspondant, le déploiement enregistre l’action avec un stub par défaut. Ajoutez le gestionnaire avant de mettre l’action à la disposition des utilisateurs.
