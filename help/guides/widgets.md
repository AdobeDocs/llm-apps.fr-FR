---
title: Personnaliser un widget EDS généré
description: Découvrez et personnalisez le widget Edge Delivery Services créé automatiquement par les applications Adobe LLM.
source-git-commit: 0f9dd0a5b1008e183eb8ab5a4c2eae34a78923bb
workflow-type: tm+mt
source-wordcount: '1065'
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
>Ce guide suppose des connaissances de base d’Adobe Edge Delivery Services (EDS). Si vous découvrez EDS, lisez le tutoriel de développement [EDS](https://www.aem.live/developer/tutorial) et [Exploration des blocs](https://www.aem.live/docs/exploring-blocks) avant de personnaliser un widget. Ces ressources couvrent les blocs, la fonction `decorate` et la structure de projet EDS.

Une fois la génération réussie, la plateforme fournit un widget EDS lié à l’action en [!DNL LLM Apps]. Le widget effectue le rendu des données représentatives pour la prévisualisation et lit le résultat du gestionnaire lorsqu’il est connecté à une plateforme LLM prise en charge. Examinez la conception et le comportement générés avant de personnaliser le widget.

Commencez par tester le widget généré. Personnalisez ensuite son contrat de données, son interaction et sa conception visuelle.

**Dans ce guide :** découvrez comment trouver le widget généré et l’adapter aux données de votre action et à la conception de votre marque. Ensuite, prévisualisez vos modifications localement et testez-les dans l’application déployée.

## Recherche du widget généré

Ouvrez le référentiel EDS connecté lors de la [&#x200B; configuration de l’infrastructure &#x200B;](/help/guides/create-app.md). Si vous utilisez l’infrastructure gérée par Adobe, demandez l’accès au code avant de continuer. Chaque widget généré est un bloc EDS :

```text
blocks/
└── <action-slug>/
    ├── <action-slug>.js
    └── <action-slug>.css
```

- Le fichier JavaScript lit le résultat de l’action et crée l’interface.
- Le fichier CSS contrôle la disposition, le comportement réactif et la conception visuelle.
- La requête de tirage générée affiche les fichiers exacts créés pour l’action.

Le rappel utilise des tirets : une action avec un identifiant de code `search_products` utilise un bloc tel que `blocks/search-products/`. La page du widget généré est généralement `/eds-widgets/search-products-demo` ; utilisez l’**[!UICONTROL URL du widget]** réel dans les métadonnées de l’action.

La plateforme configure également les URL des widgets et les fichiers SDK pris en charge. Vous n’avez pas besoin de créer un second projet EDS ni de saisir à nouveau les URL des widgets pour personnaliser un widget généré.

## Comment le SDK des applications LLM connecte le widget

Le package `@adobe/llmapps-sdk` connecte le widget EDS à l’hôte LLM. Ses points d’entrée dans le référentiel EDS généré sont les suivants :

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

L’intégration récupère le contenu `.plain.html` de la page du widget et importe ses blocs. Vous n’importez pas le SDK dans le bloc . L’intégration fournit automatiquement des `bridge` ; le chargement normal de page EDS ne les fournit pas. Le pont permet au widget :

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
  content: [{ type: 'text', text: `Found ${records.length} products.` }],
  structuredContent: { arguments: { query: 'coffee' }, records }
};
```

```javascript
// EDS block
const PREVIEW_RESULT = {
  records: [{ id: 'P-100', name: 'Frescopa House Blend' }]
};

export default async function decorate(block, bridge) {
  const live = bridge?.isConnected && bridge.hostContext?.preview !== true;
  const result = live ? await bridge.toolResult : null;
  const data = live ? result?.structuredContent : PREVIEW_RESULT;
  const records = Array.isArray(data?.records) ? data.records : [];
  // Render records or an error state when result?.isError is true.
}
```

Dans cet exemple, `PREVIEW_RESULT` est l’objet de données d’exemple local du bloc. Les widgets générés actuels utilisent `structuredContent.arguments` et `structuredContent.records`. Vérifiez que les fichiers générés ont la forme exacte. Ne remplacez jamais un résultat dynamique vide par des enregistrements de prévisualisation.

`bridge.toolResult` est résolu avec le premier résultat uniquement. Utilisez `bridge.onToolResult(callback)` si un widget reste monté et doit gérer les résultats ultérieurs.

Lorsque vous modifiez `structuredContent`, mettez à jour le gestionnaire et le widget ensemble. Ne partez pas du principe que les données structurées sont masquées dans le modèle ; elles font partie du résultat de l’action de l’hôte. Voir [Personnaliser un gestionnaire généré](/help/guides/customize-handler.md) pour l’ensemble du contrat de retour.

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
  button.disabled = !bridge?.isConnected;
  const status = document.createElement('p');
  status.setAttribute('role', 'status');
  button.addEventListener('click', async () => {
    if (bridge?.isConnected && typeof product.id === 'string') {
      try {
        await bridge.sendMessage(`Show me details for product ${product.id}`);
      } catch {
        status.textContent = 'Unable to send the request. Try again.';
      }
    }
  });

  card.append(title, button, status);
  return card;
}
```

Validez les URL avant de les affecter à des `href` ou des `src` et n’autorisez que les protocoles requis par l’expérience.

## Utiliser le pont hôte

L&#39;intégration passe un pont à `decorate(block, bridge)`. Protégez les appels interactifs avec `bridge?.isConnected` afin que le bloc s’affiche également lors de l’aperçu EDS direct. Un objet de pont peut exister en mode autonome sans être connecté ; l’attente de `toolResult` en mode autonome ne se résout jamais. L’aperçu des applications LLM définit également les `bridge.hostContext.preview` pour permettre au bloc de choisir des données d’exemple.

### Application de styles d’hôte

```javascript
if (bridge?.isConnected) {
  bridge.applyHostStyles();
}
```

La méthode applique les variables CSS hôtes et les styles de police disponibles. L’application de styles hôtes est facultative : les widgets générés peuvent utiliser leurs propres jetons de conception à la place. L’incorporation conserve les classes `.light` et `.dark` sur son corps fantôme. Utilisez `bridge.hostContext.theme` et `bridge.onContextChange(callback)` lorsque votre bloc doit également réagir aux modifications du thème.

### Envoyer un message de relance

```javascript
if (bridge?.isConnected) {
  await bridge.sendMessage('Show me similar products.');
}
```

Utilisez `sendMessage` pour continuer la conversation après une interaction.

### Appeler directement une action

```javascript
if (bridge?.isConnected) {
  const result = await bridge.callTool('search_products', { query: 'coffee' });
  // Re-render this widget from result.structuredContent, or show result.isError.
}
```

Utilisez `callTool` pour une interaction explicite qui nécessite un résultat d’action. Transmettez l’identifiant de code de l’action cible réelle, et non son nom de dossier ou d’affichage de bloc ; `search_products` est un exemple. L’hôte doit prendre en charge les appels d’action et l’action cible doit permettre la visibilité de l’application. L’appel d’une action ne rend pas automatiquement le widget actif. Transmettez uniquement des valeurs validées et gérez les appels rejetés sans exposer les détails internes.

Si la visibilité de l’action cible doit changer, modifiez uniquement cette action sous **[!UICONTROL Détails de l’action]**, enregistrez la modification et redéployez-la dans l’environnement que vous testez.

### Conserver la taille du widget synchronisée

```javascript
if (bridge?.isConnected) {
  bridge.autoResize(block);
}
```

Pour permettre à l’hôte de répondre aux modifications du contenu, appelez `autoResize` après le rendu initial. La méthode renvoie une fonction de nettoyage qui arrête l’observation lorsque le widget est supprimé.

## Prévisualiser vos modifications

Les blocs générés incluent des exemples de données pour un aperçu direct lorsque le pont est manquant, déconnecté ou marqué comme aperçu. Conservez la version de secours des données d’exemple séparément des états actif vide et erreur.

Pour prévisualiser le projet EDS localement :

```bash
npm install -g @adobe/aem-cli
aem up
```

Ouvrez le chemin d’accès à partir de l’URL du widget de l’action sur le serveur local, par exemple `http://localhost:3000/eds-widgets/search-products-demo`, plutôt que uniquement la racine du site. Vérifier :

- États vide, chargement, succès et erreur.
- Texte long et champs facultatifs manquants.
- Navigation au clavier et sélection visible.
- Thèmes clairs et sombres.
- Mises en page étroites et larges.

L’aperçu EDS direct ne teste pas les données de l’établissement de liaison de l’hôte ou des actions en direct. Publiez les modifications EDS et testez-les en direct `structuredContent` la plateforme LLM prise en charge.

## Publier la personnalisation

1. Validez et envoyez les modifications EDS.
2. Vérifiez que la page du widget et les scripts et styles modifiés sont disponibles dans les URL EDS configurées. Publiez les modifications de contenu par le biais de votre workflow de publication EDS.
3. Si vous avez modifié les métadonnées du gestionnaire ou de l’action, poussez ces modifications et déployez l’application dans l’environnement d’évaluation.
4. Testez l’action et le widget dans une plateforme LLM prise en charge. Voir [Test dans le ChatGPT](/help/guides/test-in-chatgpt.md) ou [Test dans Claude](/help/guides/test-in-claude.md).
5. Une fois les tests réussis, déployez les modifications du gestionnaire et des métadonnées dans l’environnement de production, si nécessaire.

La publication EDS est distincte du déploiement d’applications. Si les environnements d’évaluation et de production utilisent les mêmes URL EDS, les modifications de widget apportées à ces URL affectent les deux environnements ; le déploiement dans l’environnement d’évaluation ne crée pas de copie de widget isolée.

## Autres configurations EDS

Si vous avez créé des actions vous-même ou si vous souhaitez intégrer un site EDS compatible, reportez-vous à la section [Apporter votre propre projet EDS](/help/guides/bring-your-own-eds.md).
