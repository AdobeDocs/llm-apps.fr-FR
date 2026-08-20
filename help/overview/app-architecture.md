---
title: Comment une application est connectée
description: Regardez de plus près comment les éléments que vous possédez (métadonnées d’action, code de gestionnaire et widgets) se regroupent dans une application LLM en cours d’exécution, au moment de la création et de l’exécution.
source-git-commit: e066f66b37914e2f747176e865e26dcc074bff20
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

Une application **LLM** est un ensemble d’**actions** (chacune étant un outil exposé sur le **modèle
Protocole contextuel **&#x200B; ou &#x200B;** MCP**) que vous publiez sur un seul point d’entrée. Un hôte de chat
like [!DNL ChatGPT] détecte ces outils, les appelle mid-conversation et effectue le rendu
un **widget interactif** avec le résultat, directement dans le chat.

## Tout le câblage, la construction → l&#39;exécution

**Diagramme 1 - Heure de création.** Vous possédez trois surfaces distinctes ; la plateforme fusionne
dans une seule application déployable.

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

- **Interface utilisateur des applications LLM** — où vous créez, modifiez et gérez la définition de chaque action :
son **identifiant de code** (un slug fixe que vous définissez une fois ici, par exemple `my_action`, qui
associe cette même action dans l’interface utilisateur, le gestionnaire et le widget),
description, schéma d’entrée, choix du widget et indicateurs de CSP/visibilité. Pas de code.
- **Référentiel du gestionnaire d’action** — Référentiel côté serveur (basé sur un modèle standard)
où vous écrivez la logique commerciale. Chaque fonction de gestionnaire renvoie deux éléments :
  `content` (texte brut lu par le *LLM*) et `structuredContent` (objet de données)
  le *widget* lit).
- **Référentiel de widgets** — Référentiel EDS où chaque widget vit en tant que bloc et reçoit
publié sur une URL de `*.aem.page` publique. Chaque bloc utilise
  [`@adobe/llmapps-sdk`](https://www.npmjs.com/package/@adobe/llmapps-sdk), le
pont entre le widget et l’hôte/le serveur. Il met en œuvre les applications **MCP
la spécification** — le protocole sous-jacent — derrière une API simple, et
extrait l’hôte LLM lui-même, de sorte que le même widget fonctionne sans modification dans .
  [!DNL ChatGPT], [!DNL Claude], Gemini ou tout autre hôte MCP.

**Diagramme 2 — runtime.** Ce qui se passe pour chaque message envoyé par l’utilisateur, une fois
qu’un serveur est actif. Affiché avec [!DNL ChatGPT] comme exemple d’hôte : le
La même séquence s’applique à tout hôte MCP, tel que [!DNL Claude].

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
