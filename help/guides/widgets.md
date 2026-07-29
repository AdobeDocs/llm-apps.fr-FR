---
title: Personnaliser un widget EDS généré
description: Découvrez et personnalisez le widget Edge Delivery Services créé automatiquement par les applications Adobe LLM.
source-git-commit: bb3d8a02f22a91ceeeba5999453aeb4221060f80
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---


# Personnaliser un widget généré {#customize-generated-widget}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

>[!NOTE]
>
>Ce guide suppose des connaissances de base d’Adobe Edge Delivery Services (EDS). Si vous êtes nouveau dans EDS, lisez d’abord le tutoriel de développement [EDS](https://www.aem.live/developer/tutorial) et [Exploration des blocs](https://www.aem.live/docs/exploring-blocks) pour en savoir plus sur l’essentiel (les blocs, la fonction `decorate` et la structure du projet EDS) avant de personnaliser un widget.

La plateforme crée un widget EDS pour chaque action générée. Le widget reçoit déjà le résultat de l’action, effectue le rendu des exemples de données, applique le style de l’hôte et est lié à l’action dans [!DNL LLM Apps].

Commencez par tester le widget généré. Personnalisez ensuite son contrat de données, son interaction et sa conception visuelle.

**Parcours :** recherchez le bloc généré → alignez son contrat de données → personnalisez en toute sécurité l’aperçu → localement → déployez et testez.

## Recherche du widget généré

Ouvrez le référentiel EDS sélectionné lors de la création de l’application. Chaque widget généré est un bloc EDS :

```text
blocks/
└── <action-name>/
    ├── <action-name>.js
    └── <action-name>.css
```

- Le fichier JavaScript lit le résultat de l’action et crée l’interface.
- Le fichier CSS contrôle la disposition, le comportement réactif et la conception visuelle.
- La requête de tirage générée affiche les fichiers exacts créés pour l’action.

La plateforme configure également les URL des widgets et les fichiers SDK pris en charge. Vous n’avez pas besoin de créer un second projet EDS ni de saisir à nouveau ces valeurs pour personnaliser un widget généré.

## Comment le SDK des applications LLM connecte le widget

Le package `@adobe/llmapps-sdk` connecte le widget EDS à l’hôte LLM. Le référentiel EDS généré comprend les éléments suivants :

```text
scripts/
├── aem-embed.js
└── llmapps-sdk.js
```

`aem-embed.js` établit la connexion de l’hôte, charge la page EDS et appelle votre bloc :

```javascript
export default async function decorate(block, bridge) {
  // Customize the widget here.
}
```

Vous n’importez pas le SDK dans le bloc . Le `bridge` connecté est fourni automatiquement. Il permet au widget :

- Lisez le résultat du gestionnaire avec `bridge.toolResult`.
- Appliquez le style de l’hôte avec `bridge.applyHostStyles()`.
- Poursuivez la conversation avec `bridge.sendMessage()`.
- Appelez une autre action avec `bridge.callTool()`.
- Conserver sa taille synchronisée avec les `bridge.autoResize()`.

Ce guide couvre les méthodes de pont courantes. Voir le package [`@adobe/llmapps-sdk` pour &#x200B;](https://www.npmjs.com/package/@adobe/llmapps-sdk)’API complète.

## Comprendre le contrat de données

Le gestionnaire d’actions renvoie `structuredContent` et le bloc le lit à partir de `bridge.toolResult`.

```javascript
// Handler result
return {
  content: [{ type: 'text', text: `Found ${products.length} products.` }],
  structuredContent: { products, total: products.length }
};
```

```javascript
// EDS block
export default async function decorate(block, bridge) {
  const result = bridge ? await bridge.toolResult : null;
  const products = result?.structuredContent?.products ?? [];
  // Render products.
}
```

Lorsque vous modifiez `structuredContent`, mettez à jour le gestionnaire et le widget ensemble. Voir [Personnaliser un gestionnaire généré](/help/guides/customize-handler.md) pour l’ensemble du contrat de retour.

## Rendre des données externes en toute sécurité

Traiter la sortie du gestionnaire comme des données non approuvées. Privilégiez les API DOM telles que `textContent` au lieu d’insérer des valeurs de réponse dans `innerHTML`.

```javascript
function createProductCard(product, bridge) {
  const card = document.createElement('article');
  card.className = 'product-card';

  const title = document.createElement('h3');
  title.textContent = String(product.name ?? 'Product');

  const button = document.createElement('button');
  button.type = 'button';
  button.textContent = 'Tell me more';
  button.addEventListener('click', () => {
    if (bridge && product.id) {
      bridge.sendMessage(`Show me details for product ${String(product.id)}`);
    }
  });

  card.append(title, button);
  return card;
}
```

Validez les URL avant de les affecter à des `href` ou des `src` et n’autorisez que les protocoles requis par l’expérience.

## Utiliser le pont hôte

EDS passe un pont connecté à `decorate(block, bridge)`. Guard Bridge appelle pour que le bloc s’affiche également lors de l’aperçu EDS direct.

### Application de styles d’hôte

```javascript
if (bridge) {
  bridge.applyHostStyles();
}
```

Cela s’applique à la typographie de l’hôte et aux variables de thème. Votre widget CSS doit prendre en charge les thèmes hôtes clairs et sombres.

### Envoyer un message de relance

```javascript
await bridge.sendMessage('Show me similar products.');
```

Utilisez `sendMessage` lorsqu’une interaction doit continuer la conversation.

### Appeler une autre action

```javascript
const result = await bridge.callTool('get-product-details', {
  id: product.id
});
```

Utilisez `callTool` pour une interaction explicite qui nécessite un autre résultat d’action. Transmettez uniquement des valeurs validées et gérez les échecs sans exposer les détails internes.

### Conserver la taille du widget synchronisée

```javascript
if (bridge) {
  bridge.autoResize(block);
}
```

Appelez `autoResize` après le rendu initial afin que l’hôte puisse répondre aux modifications du contenu.

## Prévisualiser vos modifications

Les blocs générés doivent inclure des exemples de données pour la prévisualisation directe lorsque la `bridge` n’est pas disponible.

Pour prévisualiser le projet EDS localement :

```bash
npm install -g @adobe/aem-cli
aem up
```

Ouvrez la page du widget généré sur `http://localhost:3000`. Vérifier :

- États vide, chargement, succès et erreur.
- Texte long et champs facultatifs manquants.
- Navigation au clavier et sélection visible.
- Thèmes clairs et sombres.
- Mises en page étroites et larges.

Déployez ensuite l’application pour l’évaluation et le test avec des `structuredContent` en direct dans la plateforme LLM.

## Publier la personnalisation

1. Validez et envoyez les modifications EDS.
2. Si vous avez modifié la forme de données, validez et poussez les modifications de gestionnaire correspondantes.
3. Déployez l’application vers l’environnement d’évaluation.
4. Testez l’action et le widget dans [!DNL ChatGPT].
5. Promouvez la version vérifiée en production.

## Autres configurations EDS

Si vous n’avez pas créé l’application automatiquement ou si vous souhaitez intégrer un site EDS existant, reportez-vous à la section [&#x200B; Apporter votre propre projet EDS &#x200B;](/help/guides/bring-your-own-eds.md).
