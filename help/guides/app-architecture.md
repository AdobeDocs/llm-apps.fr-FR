---
title: Comment une application est connectée
description: Regardez de plus près comment les éléments que vous possédez (métadonnées d’action, code de gestionnaire et widgets) se regroupent dans une application LLM en cours d’exécution, au moment de la création et de l’exécution.
source-git-commit: 2f3480b3667a6ab7c4ed65b999eed4638c383edb
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 0%

---


# Comment une application est connectée {#app-architecture}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

## En une seule phrase

Une **application LLM** est un ensemble d’**actions** (chacune étant un outil exposé sur le **protocole de contexte modèle** ou **MCP**) que vous publiez sur un seul point d’entrée. Un hôte de chat comme [!DNL ChatGPT] découvre ces outils, les appelle en cours de conversation et génère un widget **interactif** avec le résultat, directement dans le chat.

## Tout le câblage, la construction → l&#39;exécution

**Diagramme 1 - Heure de création.** Vous possédez trois surfaces distinctes ; la plateforme les fusionne en une seule application déployable.

```
┌─────────────────────┐   ┌─────────────────────┐   ┌─────────────────────┐
│ 1  LLM Apps UI      │   │ 2  Action Handler   │   │ 3  Widget repo      │
│                     │   │    repo             │   │                     │
│ Create, edit, and   │   │                     │   │ Each widget is an   │
│ manage your action  │   │ Business logic —    │   │ EDS block,          │
│ definitions here    │   │ built from our      │   │ published to a      │
│ (metadata)          │   │ boilerplate         │   │ public URL on       │
│                     │   │                     │   │ *.aem.page          │
│                     │   │ Returns content     │   │                     │
│                     │   │ (for the LLM) +     │   │                     │
│                     │   │ structuredContent   │   │                     │
│                     │   │ (for the widget)    │   │                     │
└─────────────────────┘   └─────────────────────┘   └─────────────────────┘
           │                         │                         │
           └─────────────────────────┼─────────────────────────┘
                                     ▼
                       ┌─────────────────────────────┐
                       │ LLM Apps deploy pipeline    │
                       │ Combines the 3 surfaces     │
                       │ into one running app        │
                       └─────────────────────────────┘
                                     │
                                     ▼
                 ┌─────────────────────────────────────────┐
                 │ ONE MCP server on Adobe I/O Runtime     │
                 │ https://<ns>.adobeioruntime.net/.../mcp │
                 └─────────────────────────────────────────┘
```

- **Interface utilisateur des applications LLM** — où vous créez, modifiez et gérez la définition de chaque action : son **identifiant de code** (un rappel fixe que vous définissez une fois ici, par exemple `my_action`, qui lie cette même action à travers l&#39;interface utilisateur, le gestionnaire et le widget), la description, le schéma d&#39;entrée, le choix du widget et les indicateurs CSP/visibilité. Pas de code.
- **Référentiel du gestionnaire d’action** : référentiel côté serveur (basé sur notre modèle standard) où vous écrivez la logique commerciale. Chaque fonction de gestionnaire renvoie deux éléments : `content` (texte brut lu par le *LLM*) et `structuredContent` (objet de données lu par le *widget*).
- **Référentiel de widgets** — Référentiel EDS dans lequel chaque widget vit en tant que bloc et est publié sur une URL de `*.aem.page` publique. Chaque bloc utilise [`@adobe/llmapps-sdk`](https://www.npmjs.com/package/@adobe/llmapps-sdk), le pont entre le widget et l’hôte/le serveur. Il met en œuvre la spécification **MCP Apps**, le protocole sous-jacent, derrière une API simple et extrait l’hôte LLM lui-même, de sorte que le même widget fonctionne sans modification dans [!DNL ChatGPT], [!DNL Claude], Gemini ou tout autre hôte MCP.

**Diagramme 2 — runtime.** Ce qui se passe pour chaque message envoyé par l’utilisateur, une fois que ce serveur est actif. Affiché avec [!DNL ChatGPT] comme exemple d’hôte : la même séquence s’affiche pour tout hôte MCP, tel que [!DNL Claude].

```
┌── ChatGPT  (the MCP host) ──────────────────────────────────────────────┐
│  1  tools/list  >  sees `my_action` + its description + input schema    │
│  2  user asks   >  "I need help with …"                                 │
│  3  model picks >  the description matches -> calls this tool           │
│  4  tools/call  >  { name: "my_action", arguments: {situation, ...} }   │
└─────────────────────────────────────────────────────────────────────────┘
                                     │
                 routes by CODE IDENTIFIER  ->  my_action
                                     ▼
┌── Adobe I/O Runtime ────────────────────────────────────────────────────┐
│  actions/my_action/index.js  --  your handler runs                      │
│  returns  { content -> text for the model , structuredContent -> data } │
└─────────────────────────────────────────────────────────────────────────┘
                                     │
                                     ▼
┌── Rendered inside the conversation ─────────────────────────────────────┐
│  5  render      >  ChatGPT renders the Widget repo's EDS block          │
│  The EDS block reads the structuredContent the Action Handler repo      │
│  returned, and draws the interactive card — live, inside the chat.      │
└─────────────────────────────────────────────────────────────────────────┘
```
