---
title: Création d’actions
description: Générez des actions de l’application LLM Adobe à partir d’un site web public, suivez la progression de la génération, passez en revue le travail généré ou choisissez une création d’action manuelle.
source-git-commit: 0f9dd0a5b1008e183eb8ab5a4c2eae34a78923bb
workflow-type: tm+mt
source-wordcount: '938'
ht-degree: 0%
---

# Création d’actions {#creating-actions}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Les actions sont les fonctionnalités que votre application offre dans une conversation LLM, telles que la recherche de produits, la vérification de la disponibilité ou la réservation d&#39;un service. Chaque gestionnaire d’action effectue la tâche, et un widget facultatif présente le résultat.

Après la [configuration de l’infrastructure](/help/guides/create-app.md), choisissez comment créer des actions à partir de la page de l’application. La génération automatique est disponible avec l’infrastructure gérée par Adobe et vos propres référentiels.

**Dans ce guide :** découvrez comment générer des actions à partir de votre site web ou les créer vous-même. Pour la génération automatique, lancez le processus et passez en revue les résultats avant de déployer et de tester l’application.

## Choisir comment créer des actions

Ouvrez votre application. Si Adobe prépare toujours son infrastructure, attendez la fin de la configuration.

Dans la section **[!UICONTROL Actions]**, **[!UICONTROL Comment souhaitez-vous que les actions soient créées ?]** offre deux choix :

| Choix | Utilisez-le lorsque |
|---|---|
| **[!UICONTROL Créer automatiquement des actions]** | Vous souhaitez que la plateforme analyse votre site web et génère les actions suggérées, le code de gestionnaire, les tests et les widgets. Il s’agit du point de départ recommandé. |
| **[!UICONTROL Je créerai des actions moi-même]** | Vous devez définir chaque action et fournir vous-même sa mise en œuvre. |

Choisir l’infrastructure dans l’assistant de création d’application ne sélectionne aucune de ces options pour vous.

## Générer des actions à partir d’un site web {#generate-from-website}

1. Sélectionnez **[!UICONTROL Créer automatiquement des actions]**.
2. Dans **[!UICONTROL Votre site web]**, saisissez un site web HTTPS public qui représente les produits, services ou tâches client de votre marque. Cet exemple utilise `https://frescopa.coffee/`.
3. Sélectionnez **[!UICONTROL Créer mes actions]**.

![Actions — génération automatique sélectionnée avec le site Web public saisi avant de commencer](/help/assets/guide-creating-actions/build-choice.png)

Utilisez un site web que vous êtes autorisé à analyser. Ne fournissez pas de site exposant des informations confidentielles ou dont l’accès est contrôlé.

Sélectionner **[!UICONTROL Créer mes actions]** lance la génération ; saisir l’URL seule ne le fait pas. Vous n’avez pas besoin de recréer l’application ni de rouvrir l’assistant d’infrastructure.

### Suivre la progression de la génération

La page de l’application affiche **[!UICONTROL Des actions sont en cours de génération]**. Sélectionnez **[!UICONTROL Accéder aux actions]** ou ouvrez **[!UICONTROL Actions]** dans la navigation pour suivre la progression.

L’interface utilisateur estime jusqu’à 30 minutes pour la génération. Quittez la page et revenez plus tard si nécessaire.

![Actions — La génération est en cours, avec un lien pour suivre la progression sur la page Actions](/help/assets/guide-creating-actions/generating.png)

Pendant la génération, la plateforme :

1. Analyse le site web et identifie les tâches client pour l’application.
2. Crée des métadonnées d’action suggérées, y compris des descriptions et des paramètres d’entrée.
3. Génère des gestionnaires côté serveur et des tests dans le référentiel d’actions.
4. Génère des widgets dans le référentiel EDS.
5. Prépare les actions générées pour la révision.

Tandis que la plateforme analyse le site web, la page Actions affiche **[!UICONTROL Découverte d’actions pour votre expérience de conversation]**. Les actions suggérées apparaissent lorsque la plateforme les détecte ; leurs aperçus de code et de widget peuvent encore être générés. Attendez la fin de la génération avant de passer en revue chaque action.

>[!IMPORTANT]
>
>Les gestionnaires générés utilisent initialement des données d’exemple représentatives. L’analyse de site web ne connecte pas automatiquement votre application aux API de production, aux inventaires dynamiques, aux services de paiement ou aux systèmes de réservation.

Si la génération signale une erreur, lisez l’erreur visible et vérifiez les [conseils de dépannage](/help/reference/troubleshooting.md). N’approuvez pas une action simplement pour contourner une erreur de génération.

## Vérifier les actions générées {#review-generated-actions}

Examinez chaque action suggérée avant le déploiement. Vérifiez qu’elle représente une tâche utile et que le comportement généré correspond à ce que vous souhaitez que l’application fasse.

![Actions — les actions Frescopa générées sont prêtes à être examinées et n&#39;ont pas été déployées](/help/assets/guide-creating-actions/actions-ready-for-review.png)

1. Sur la page **[!UICONTROL Actions]**, sélectionnez **[!UICONTROL Réviser]** pour une action prête.
2. Consultez la description de l’action, les paramètres d’entrée, les annotations, la visibilité et le gestionnaire généré.
3. Vérifiez la configuration du widget et l’aperçu, si l’action comporte un widget.
4. Inspectez le code et les tests générés, y compris les requêtes d’extraction liées. Vérifiez ce qu’est un exemple de comportement et ce qui doit être remplacé par des intégrations réelles.
5. Lorsque l’action est prête, sélectionnez **[!UICONTROL Marquer comme révisé]**.
6. Revenez à la liste d’actions et répétez l’opération pour les actions restantes.

L’éditeur d’actions enregistrées sépare **[!UICONTROL détails du widget LLM]**, **[!UICONTROL détails de l’action]** et **[!UICONTROL Analytics]**. Examinez les demandes d’extraction de widget et de gestionnaire dans leurs onglets respectifs.

![Action générée : les onglets actuels de l’éditeur et Marquer comme révisé, avec le lien du référentiel masqué](/help/assets/guide-creating-actions/generated-action-review.png)

La sélection de **[!UICONTROL Marquer comme révisé]** fusionne les demandes d’extraction générées par l’action s’il ne reste aucun bloqueur de fusion. Si une demande d’extraction ouverte bloque la révision, résolvez le bloqueur et réessayez. N’approuvez pas une implémentation présentant des échecs de génération de code ou de fusion non résolus.

Une fois que vous avez examiné toutes les actions générées, la page Actions affiche **[!UICONTROL Toutes les actions sont examinées !]**. Pour revenir à l’application, sélectionnez **[!UICONTROL Aller à la page de l’application]**.

La révision n’est pas un déploiement. Les actions ne sont pas disponibles via le serveur MCP de votre application tant que vous ne l’avez pas déployée.

## Création d’actions vous-même {#manual-actions}

Si vous préférez ne pas générer d’actions à partir d’un site web :

1. Dans le choix de création d’actions de la page d’application, sélectionnez **[!UICONTROL Je créerai des actions moi-même]**.
2. Sélectionnez **[!UICONTROL Je vais créer mes actions]**.
3. Sur la page Actions , sélectionnez **[!UICONTROL Créer une action]**.
4. Suivez la procédure [Créer une action à partir de zéro](/help/guides/create-action.md) pour définir les métadonnées, implémenter un gestionnaire et connecter un widget si nécessaire.

L&#39;infrastructure reste la même. Choisir la création manuelle ne crée pas de code de gestionnaire et ne connecte pas vos systèmes d’entreprise pour vous.

Ajoutez des actions manuelles après avoir examiné les actions générées automatiquement si nécessaire.

## Déployer et tester

Après avoir examiné le travail généré :

1. [Déployez l’application](/help/guides/deploy-your-app.md) dans **[!UICONTROL Phase]**.
2. Testez-le dans [ChatGPT](/help/guides/test-in-chatgpt.md) ou [Claude](/help/guides/test-in-claude.md), à l’aide de l’URL du serveur MCP d’évaluation.
3. Invites de test correspondant à chaque action et invites sans rapport qui n’appellent pas cette action. Vérifiez à la fois la réponse textuelle et les interactions du widget.

Avant de mettre l’application à la disposition des clients, [personnalisez les gestionnaires](/help/guides/customize-handler.md) pour connecter vos API et [personnalisez les widgets](/help/guides/widgets.md). Testez l’application dans l’environnement d’évaluation pour confirmer qu’elle fonctionne comme prévu. Déployez en production uniquement après la réussite du test.
