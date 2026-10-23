---
title: Apporter votre propre projet Edge Delivery Services
description: Connecter un projet Adobe Edge Delivery Services existant à une action Adobe LLM Apps.
hidefromtoc: true
source-git-commit: 557173e8d95325b980efac4d72c06bb3d90dcd72
workflow-type: tm+mt
source-wordcount: '866'
ht-degree: 1%
---

# Apportez votre propre projet EDS {#bring-your-own-eds}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Utilisez ce guide pour connecter un projet Edge Delivery Services (EDS) compatible ou pour ajouter un widget à une action que vous avez vous-même créée. Un projet existant est facultatif. Si vous configurez votre propre infrastructure, le chemin d’accès par défaut commence par deux référentiels vides que la plateforme initialise lors de la [ configuration de l’infrastructure ](/help/guides/create-app.md).

Si la plateforme a créé votre widget automatiquement, suivez [Personnaliser un widget généré](/help/guides/widgets.md) à la place. Le projet généré inclut déjà la configuration des fichiers, blocs, contenus et actions SDK décrite ici.

**Dans ce guide :** découvrez comment préparer votre projet EDS, installer le SDK, et créer et publier un widget. Connectez ensuite le widget à une action et testez-le dans votre application déployée.

## Avant de commencer

Vous avez besoin des éléments suivants :

- Référentiel EDS AEM compatible standard avec [la synchronisation de code AEM](https://github.com/apps/aem-code-sync) installé et une source de contenu configurée.
- Autorisation d’ajouter des dépendances et de créer des blocs dans ce référentiel.
- Autorisation de configurer les en-têtes de réponse pour le site EDS.
- Action en [!DNL LLM Apps] avec un gestionnaire qui renvoie des `structuredContent`.

Lorsque vous liez un référentiel existant, la plateforme vérifie les `scripts/aem.js`, les `blocks/`, les `styles/styles.css` et les `head.html`. L’incorporation importe également des `scripts/scripts.js` et appelle sa fonction `decorateMain` exportée pour préparer les blocs. Gardez intact le contrat de chargement des blocs EDS. Les fichiers requis seuls ne confirment pas qu’un projet fortement personnalisé s’affiche correctement.

## Installation de LLM Apps SDK {#install-the-llm-apps-sdk}

À partir de la racine du projet EDS :

```bash
npm install @adobe/llmapps-sdk
```

Le hook postinstall du package copie le point d’entrée du widget, l’implémentation du pont et la prise en charge des bibliothèques Adobe dans le projet :

```text
scripts/
├── aem-embed.js
├── llmapps-sdk.js
├── alloy-base.min.js
└── alloy-standalone.min.js
```

L’URL du script de l’action pointe vers `scripts/aem-embed.js`, et non `scripts/llmapps-sdk.js`. Vérifiez que l’installation a copié les fichiers SDK, puis validez et poussez-les avec le projet. Le hook remplace les fichiers copiés lors de l’installation ou de la mise à jour du SDK ; ne personnalisez pas les fichiers SDK copiés.

## Création du bloc de widget

Créez un bloc pour l’action :

```text
blocks/
└── search-products/
    ├── search-products.js
    └── search-products.css
```

Exportez la fonction `decorate` EDS standard avec le pont connecté comme deuxième argument :

```javascript
const PREVIEW_RESULT = {
  records: [{ id: 'P-100', name: 'Frescopa House Blend' }]
};

export default async function decorate(block, bridge) {
  if (bridge?.isConnected) {
    bridge.applyHostStyles();
  }

  const live = bridge?.isConnected && bridge.hostContext?.preview !== true;
  const result = live ? await bridge.toolResult : null;
  const data = live ? result?.structuredContent : PREVIEW_RESULT;
  const records = Array.isArray(data?.records) ? data.records : [];

  if (result?.isError) {
    const message = document.createElement('p');
    message.textContent = 'Product search is unavailable. Try again later.';
    block.replaceChildren(message);
    bridge.autoResize(block);
    return;
  }

  const list = document.createElement('ul');
  records.forEach((product) => {
    const item = document.createElement('li');
    item.textContent = String(product?.name ?? 'Product');
    list.append(item);
  });

  if (records.length === 0) {
    const item = document.createElement('li');
    item.textContent = 'No matching products were found.';
    list.append(item);
  }
  block.replaceChildren(list);

  if (bridge?.isConnected) {
    bridge.autoResize(block);
  }
}
```

Utilisez des API DOM qui codent des valeurs de texte. Ne concaténez pas de données externes dans HTML.

L’exemple exige `structuredContent.records`. Faites correspondre les noms de propriété et les champs d’enregistrement renvoyés par votre gestionnaire. Vérifiez `bridge.isConnected`, pas seulement si un pont existe : `toolResult` ne se résout jamais en mode autonome. Dans l’aperçu des applications LLM, utilisez les exemples de données du bloc lorsque `bridge.hostContext.preview` indique le mode d’aperçu.

## Créer et publier la page du widget

Créez une page EDS pour le widget et ajoutez le bloc à cette page. Publiez la page.

L’URL de la page devient l’URL du widget de l’action. Par exemple, la configuration générée par défaut utilise l’origine d’aperçu EDS :

```text
https://main--<repo>--<owner>.aem.page/eds-widgets/search-products-demo
```

Une origine dynamique EDS telle que `https://main--<repo>--<owner>.aem.live` fonctionne également lorsque la page et les ressources y sont publiées. Utilisez l’origine réelle du site. Les sites EDS configurés peuvent avoir des noms de site et d’organisation différents du référentiel et du propriétaire GitHub.

Vérifiez que l’ajout de `.plain.html` à l’URL du widget renvoie le fragment de page et que la page contient le bloc de `search-products`. Le chemin d’accès à la page ne doit pas nécessairement correspondre à l’identifiant d’action, mais une convention cohérente facilite la gestion du projet.

## Configurer CORS

Le widget charge la page EDS, les scripts, les styles, les blocs et les médias dans toutes les origines. Configurez l’en-tête du site EDS :

```json
{
  "/**": [
    {
      "key": "access-control-allow-origin",
      "value": "<allowed-host-origin>"
    }
  ]
}
```

Utilisez l’origine réelle du sandbox de widget requise par la plateforme LLM prise en charge, et pas seulement l’origine du site web de conversation. Vérifiez la norme CORS sur la réponse `.plain.html` et sur les JavaScript, styles, polices et médias importés.

La configuration automatique d’EDS de la plateforme configure `access-control-allow-origin: *` pour la diffusion de widgets publics et peut remplacer une valeur existante plus étroite. Utilisez cette politique uniquement pour les ressources publiques délibérément sans demandes d’origine croisée authentifiées et lorsque vos exigences de sécurité le permettent. Si vous gérez un site existant limité, passez en revue sa politique avant de le connecter ; n’exposez pas le contenu d’un site privé pour permettre à un widget de fonctionner.

Pour plus d’informations sur la configuration EDS, voir [Service de configuration](https://aem.live/docs/config-service-setup).

## Configuration de l’action

Dans [!DNL LLM Apps], ouvrez l’action et sélectionnez **[!UICONTROL Détails du widget LLM]**. Sous **[!UICONTROL Informations sur le widget]**, définissez **[!UICONTROL Type de widget]** sur **[!UICONTROL EDS]**, puis développez **[!UICONTROL URL de modèle]**.

Enter :

- **[!UICONTROL URL du script]**

  ```text
  https://main--<repo>--<owner>.aem.page/scripts/aem-embed.js
  ```

- **[!UICONTROL URL du widget]**

  ```text
  https://main--<repo>--<owner>.aem.page/eds-widgets/search-products-demo
  ```

Remplacez les exemples par l’origine et la page du site de travail que vous avez vérifiées ci-dessus. Configurez les domaines CSP et les autorisations de navigateur avec le privilège minimum. L’origine EDS doit être autorisée pour les deux connexions, telles que la récupération page-fragment, et les ressources, telles que les scripts et les styles de bloc. Ajoutez d’autres origines uniquement lorsque le widget l’exige.

Sélectionnez **[!UICONTROL Enregistrer les modifications]**, puis redéployez l’environnement que vous testez. La configuration de widget enregistrée ne met pas à jour le serveur MCP en cours d’exécution avant le déploiement.

Pour les définitions de champ, voir [Champs d’action et de widget](/help/reference/reference-docs.md).

## Test de l’intégration

1. Prévisualisez directement la page EDS et vérifiez ses données d’exemple de secours.
2. Testez localement le gestionnaire et comparez sa `structuredContent` à la forme attendue par le bloc.
3. Déployez le gestionnaire et les métadonnées d’action dans l’environnement intermédiaire. La publication du code EDS et du contenu est une étape distincte.
4. Appelez l’action à partir d’une plateforme LLM prise en charge. Voir [Test dans le ChatGPT](/help/guides/test-in-chatgpt.md) ou [Test dans Claude](/help/guides/test-in-claude.md).
5. Vérifiez les états de chargement, de succès, de vide et d’erreur.

Si la page fonctionne directement, mais pas dans la plateforme LLM, vérifiez CORS, CSP, URL HTTPS et la forme `structuredContent`. Voir [Dépannage](/help/reference/troubleshooting.md).
