---
title: Tester dans ChatGPT
description: Découvrez comment ajouter votre application Adobe LLM déployée à ChatGPT et la tester dans une conversation réelle.
source-git-commit: 483a71f5f1de5caf1bd89b26f4d67d2d5a0aa15a
workflow-type: tm+mt
source-wordcount: '798'
ht-degree: 2%

---


# Test en [!DNL ChatGPT]

>[!IMPORTANT]
>
>**Clause de non-responsabilité :** il s’agit d’une version bêta de [!DNL LLM Apps]. Les fonctionnalités, les workflows et l’interface utilisateur présentés ici ne représentent pas nécessairement l’état final de l’application ou du produit.

>[!NOTE]
>
>Ce guide utilise [!DNL ChatGPT] comme exemple. Les étapes générales (enregistrement d’une URL de serveur MCP et test dans une conversation) s’appliquent également à d’autres plateformes LLM, bien que le flux de configuration et l’interface utilisateur varient.

Après un déploiement réussi, votre application s’exécute sur [!DNL Adobe I/O Runtime] et expose une URL de serveur MCP. Ce guide vous explique comment l’ajouter à [!DNL ChatGPT] et la tester dans une conversation réelle.

## Planifier les exigences

L’ajout d’applications de développement personnalisées à [!DNL ChatGPT] est régi par les niveaux d’abonnement d’OpenAI. Il ne s’agit pas d’une limitation de [!DNL LLM Apps], mais plutôt de la manière dont OpenAI gère actuellement l’accès aux applications MCP personnalisées.

| Plan [!DNL ChatGPT] | Applications MCP personnalisées |
|--------------|-----------------|
| Libre | Non disponible |
| Aller | Non disponible |
| Plus | Non disponible |
| Pro | Disponible |
| Entreprise | Disponible |
| Entreprise / Edu | Disponible |

>[!NOTE]
>
>Si vous bénéficiez d’un plan Free, Go ou Plus, vous **ne pourrez pas ajouter votre application déployée** à [!DNL ChatGPT]. Effectuez la mise à niveau vers **Pro** ou demandez à l’administrateur de votre organisation de l’activer dans un espace de travail **Entreprise** ou **Entreprise**.

## Activer le mode Développeur

Pour ajouter une application MCP personnalisée, le **mode développeur** doit être activé dans votre compte [!DNL ChatGPT]. S’abonner
pour vérifier et activer, procédez comme suit.

### Ouvrir les paramètres

Cliquez sur l’avatar de votre profil dans le coin inférieur gauche, puis sur **[!UICONTROL Paramètres]**.

![ChatGPT — Menu Paramètres](/help/assets/guide-test-chatgpt/chatgpt-settings-menu.png)

### Accéder aux applications

Dans la boîte de dialogue Paramètres, sélectionnez **[!UICONTROL Applications]** dans la barre latérale gauche. Cliquez sur **[!UICONTROL Paramètres avancés]** dans la partie inférieure.

![ChatGPT — Paramètres des applications](/help/assets/guide-test-chatgpt/chatgpt-apps-settings.png)

### Activer le mode Développeur

Assurez-vous que le bouton (bascule) **[!UICONTROL Mode Développeur]** est activé (bleu). Vous pouvez ainsi enregistrer des URL de serveur MCP personnalisées et non vérifiées.

>[!NOTE]
>
>Le mode Développeur est étiqueté *Risque élevé* car il autorise les applications qui n’ont pas été examinées par OpenAI. [!DNL ChatGPT] désactive automatiquement la mémoire pour les conversations qui utilisent les applications en mode développeur.

![ChatGPT — Mode Développeur activé](/help/assets/guide-test-chatgpt/chatgpt-developer-mode.png)

## Ajouter votre application à [!DNL ChatGPT]

### Copier l’URL du serveur MCP

Accédez à la page **Détails de l’application** dans [!DNL LLM Apps] et recherchez la section **[!UICONTROL Tester l’application]**. Copiez l’URL **Évaluation** ou **Production** : elle ressemble à ceci :

```
https://<namespace>.adobeioruntime.net/api/v1/web/llm-apps/mcp
```

### Ouvrir la page Applications

Dans [!DNL ChatGPT], accédez à **[!UICONTROL Paramètres] → [!UICONTROL Applications]**.

![Page ChatGPT — Applications](/help/assets/guide-test-chatgpt/chatgpt-apps-page.png)

### Création d’une application

Cliquez sur **[!UICONTROL Créer une application]** dans la ligne Paramètres avancés.

![ChatGPT — Boîte de dialogue Créer une application](/help/assets/guide-test-chatgpt/chatgpt-create-app.png)

Renseignez les champs suivants :

| Champ | Valeur |
|-------|-------|
| **Icône** | Facultatif — Chargez un fichier PNG de 128 x 128 Ko (max. 10 Ko) |
| **Nom** | Un nom d’affichage pour votre application (par exemple, *My Brand App*) |
| **Description** | Brève description de la fonction de l’application |
| **URL du serveur MCP** | Collez l’URL depuis [!DNL LLM Apps] |
| **[!UICONTROL Authentication]** | Sélectionnez *Aucune authentification* |

Cochez la case **Je comprends et souhaite continuer** — cela signifie que le serveur MCP
n&#39;a pas été examiné par OpenAI — et cliquez sur **Créer**.

### Vérifiez que l’application est activée.

Une fois créée, votre application s’affiche sous **[!UICONTROL Applications activées]** avec un badge **[!UICONTROL DEV]**, confirmant qu’elle est active.

>[!NOTE]
>
>Votre application apparaît également sous **Brouillons** — il s’agit d’applications privées que vous avez créées en mode développeur et qui ne sont visibles que par votre compte.

Votre application est maintenant prête à être utilisée dans les conversations [!DNL ChatGPT].

![ChatGPT — application activée](/help/assets/guide-test-chatgpt/chatgpt-app-enabled.png)

## Tester dans une conversation

Une fois l’application activée, démarrez une nouvelle conversation dans [!DNL ChatGPT]. Avant de poser une question, joignez votre application à l’aide de l’une des deux méthodes suivantes.

### Option 1 — Sélectionner dans le menu

Cliquez sur le bouton **+** dans l’entrée de conversation, puis **Plus** pour développer la liste complète des outils disponibles. Sélectionnez votre application dans la liste pour la joindre à la conversation en cours.

![ChatGPT — sélectionnez l&#39;application dans le menu](/help/assets/guide-test-chatgpt/chatgpt-select-app.png)

### Option 2 — Utiliser @mention

Saisissez **@** dans l’entrée de conversation et sélectionnez votre application dans la liste déroulante. L’application est jointe en ligne et vous pouvez continuer à saisir votre question dans le même message.

>[!NOTE]
>
>Si vous utilisez **** une seconde fois sur la même application, vous la désélectionnez et la supprimez de la conversation.

![ChatGPT — @mention l&#39;application](/help/assets/guide-test-chatgpt/chatgpt-mention-app.png)

Une fois sélectionnée, l’application est jointe en ligne et vous pouvez saisir votre question dans le même message :

![ChatGPT — application jointe via @mention](/help/assets/guide-test-chatgpt/chatgpt-mention.png)

### Consulter le résultat

Une fois l’application jointe, saisissez une question alignée sur l’une de vos actions configurées, par exemple *« Afficher vos produits »*. [!DNL ChatGPT] la fait correspondre à l’action appropriée, extrait les paramètres d’entrée, appelle votre gestionnaire sur [!DNL Adobe I/O Runtime] et effectue le rendu du résultat :

![ChatGPT — résultat de l&#39;action](/help/assets/guide-test-chatgpt/chatgpt-response.png)

La réponse inclut :

- **Le widget EDS** — un composant d’IU riche avec des images, des évaluations et des boutons d’action.
- **La réponse texte** — sous le widget, [!DNL ChatGPT] utilise le `content` renvoyé par votre gestionnaire
pour formuler un résumé en langage naturel des résultats.
- **Indicateur de statut** — Le texte *Statut appelé* que vous avez configuré dans la boîte de dialogue Créer une action.

## Prochaines étapes

- **Ajouter d’autres actions** — Définissez des actions supplémentaires dans l’interface utilisateur, écrivez leurs gestionnaires et redéployez.
- **Déployer en production** — si vous avez effectué un test dans l’environnement intermédiaire, déployez en production pour l’expérience en direct.
- **Partager avec votre équipe** — Utilisez **Copier l’URL** sur la page Détails de l’application pour partager l’URL du serveur MCP avec vos coéquipiers.

