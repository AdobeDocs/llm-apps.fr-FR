---
title: Créer automatiquement votre première application LLM
description: Créez une application Adobe LLM à partir de votre site web, passez en revue les actions générées, déployez-la et testez-la sur une plateforme LLM prise en charge, telle que ChatGPT.
source-git-commit: f91bb73a39cc5aacf44979ee55dd0ab5f69d4c81
workflow-type: tm+mt
source-wordcount: '1272'
ht-degree: 0%

---


# Création Automatique De Votre Première Application {#create-first-app}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

La plateforme transforme votre site web en une application entièrement fonctionnelle. Il propose des actions, écrit du code de gestionnaire et des tests, crée des widgets EDS et envoie les fichiers générés aux deux référentiels [!DNL GitHub] que vous détenez.

Compter environ 15 minutes pour la génération. À la fin de ce tutoriel, vous disposez d’une application déployée que vous pouvez tester sur une plateforme LLM prise en charge, telle que [!DNL ChatGPT].

**Parcours :** confirmer les exigences → créer deux référentiels → créer l’application → passer en revue les actions générées → déployer dans l’environnement d’évaluation → tester le plug-in → connecter les systèmes de production.

## Avant de commencer

Renseignez toutes les [exigences relatives aux applications LLM](/help/overview/overview.md#requirements) avant de commencer ce tutoriel.

Ce tutoriel crée une application LLM pour [ Frescopa Coffee ](https://frescopa.coffee/).

## Créer deux référentiels vides

La plateforme a besoin de deux référentiels vides. Créez les deux sous le même compte ou la même organisation [!DNL GitHub] :

- **Référentiel de gestionnaires** — stocke les gestionnaires d&#39;actions et les tests. Par exemple, `my-brand-llm-app`.
- **Référentiel EDS** — stocke les blocs de widgets et les styles générés. Par exemple, `my-brand-llm-app-eds`.

Accédez à [](https://github.com/new) pour chaque référentiel.

N’initialisez aucun référentiel avec une licence README, `.gitignore` ou . La plateforme prépare la structure de projet requise.

>[!TIP]
>
>Utilisez des noms de référentiel qui identifient l’application et la fonction de chaque référentiel. Cela les rend plus faciles à reconnaître dans la boîte de dialogue de création d’application.

## Démarrer l’application

1. Ouvrez [Applications Adobe LLM](https://experience.adobe.com/#/@llmapps/llm-apps/) puis sélectionnez **[!UICONTROL Créer une application]**.
2. Saisissez le **[!UICONTROL Nom de l’application LLM]** et une description facultative.
3. Sélectionnez la **[!UICONTROL région Analytics]**.

   >[!IMPORTANT]
   >
   >La région Analytics ne peut pas être modifiée une fois l’application créée.

4. Dans **[!UICONTROL Créer mon application]**, sélectionnez **[!UICONTROL Créer mon application automatiquement]**.
5. Dans **[!UICONTROL votre site web]**, saisissez l’URL de votre site web, y compris le protocole `https://`. La plateforme analyse ce site afin de déterminer des actions utiles et des exemples de résultats représentatifs.

![Créer une application LLM — détails de l&#39;application et activation de Créer mon application](/help/assets/guide-onboarding-agent/app-details-onboarding.png)

## Accorder à [!DNL LLM Apps] l’accès aux référentiels

L’application de [!DNL GitHub] des applications Adobe LLM [!DNL LLM Apps] donne accès aux référentiels que vous sélectionnez.

>[!NOTE]
>
>La connexion à une organisation [!DNL GitHub] est une configuration ponctuelle. Si l’organisation apparaît déjà dans la boîte de dialogue, utilisez **[!UICONTROL Gérer les référentiels sur GitHub]** au lieu de la connecter à nouveau.

### Organisation connectée

Si l’application Adobe LLM Apps [!DNL GitHub] a déjà été installée avant la création des référentiels :

1. Sélectionnez l’organisation connectée.
2. Sélectionnez **[!UICONTROL Gérer les référentiels sur GitHub]**.
3. Ajoutez les deux référentiels à l’installation de l’application [!DNL GitHub] existante.
4. Revenez à [!DNL LLM Apps] et actualisez les listes de référentiel.

### Première connexion uniquement

Si l’organisation n’apparaît pas dans la boîte de dialogue :

1. Sélectionnez **[!UICONTROL Connecter une organisation GitHub]**.
2. Installez l’application [!DNL GitHub] Adobe LLM Apps.
3. Choisissez **[!UICONTROL Sélectionner uniquement les référentiels]** puis sélectionnez les deux référentiels.
4. Revenez à la boîte de dialogue Créer une application LLM .

Si vous ne pouvez pas installer ni mettre à jour l’application [!DNL GitHub], contactez l’administration de l’organisation.

## Sélectionner les référentiels

1. Sous **[!UICONTROL Référentiel standard]**, sélectionnez l’organisation et le référentiel de gestionnaire vide.
2. Sous **[!UICONTROL Référentiel EDS]**, sélectionnez l’organisation et le référentiel EDS vide.

   ![Créer mon application — sélectionnez l’organisation GitHub, le référentiel standard et le référentiel EDS](/help/assets/guide-onboarding-agent/repos-selected.png)

3. Sous **[!UICONTROL Conditions générales]**, cochez la case **[!UICONTROL J’accepte les conditions générales d’Adobe Developer]**.
4. Sélectionnez **[!UICONTROL Créer une application]**.

## Terminer la configuration d’EDS

Lorsque le référentiel EDS sélectionné est vide, [!DNL LLM Apps] l’initialise avec le standard AEM. La boîte de dialogue vous demande ensuite d’installer la synchronisation du code AEM avant de tenter de créer à nouveau l’application.

1. Dans le message situé sous le référentiel EDS, sélectionnez **[!UICONTROL Installer la synchronisation du code AEM]**.
2. Sur [!DNL GitHub], installez la synchronisation du code AEM et accordez-lui l’accès au référentiel EDS.

   Sur la page de confirmation **Synchronisation du code AEM enregistrée**, sous **[!UICONTROL Utilisateurs du site]**, sélectionnez **[!UICONTROL + Ajouter un utilisateur]** et ajoutez l’adresse e-mail que vous utilisez pour vous connecter à [!DNL LLM Apps] avec le rôle **[!UICONTROL admin]**. Sélectionnez ensuite **[!UICONTROL Terminer la configuration]** au bas de la page.

   ![Synchronisation du code AEM enregistrée : ajoutez-vous en tant qu’utilisateur du site avec le rôle d’administrateur](/help/assets/guide-onboarding-agent/aem-code-sync-site-users-admin.png)

3. Revenez à la boîte de dialogue Créer une application LLM .

![Créer une application LLM : référentiel EDS vide initialisé et synchronisation du code AEM requise](/help/assets/guide-onboarding-agent/install-aem-code-sync.png)

Vous devez être administrateur du site EDS. Si la boîte de dialogue indique que vous n’êtes pas administrateur :

![Créer une application LLM — Accès administrateur EDS requis](/help/assets/guide-onboarding-agent/eds-admin-required.png)

1. Sélectionnez **[!UICONTROL Ouvrir l’administration AEM Live]**.
2. Ajoutez-vous en tant qu’administrateur du site EDS en cliquant sur le bouton **[!UICONTROL + Ajouter un ou plusieurs utilisateurs]** .

   ![Créer une application LLM — Vous ajouter en tant qu&#39;administrateur EDS](/help/assets/guide-onboarding-agent/add-eds-admin.png)

3. Revenez à [!DNL LLM Apps], actualisez le référentiel EDS, puis sélectionnez à nouveau **[!UICONTROL Créer une application]**.

Une fois le référentiel et l’administrateur vérifiés, [!DNL LLM Apps] crée l’application et commence à générer des actions.

## Attente de la génération des actions

Accédez à la page **[!UICONTROL Actions]** à partir de la gauche. La page Actions affiche **Découverte d’actions pour votre expérience de conversation** pendant que l’agent analyse le site web et génère l’application. La génération prend généralement environ 15 minutes. Vous pouvez quitter cette page et revenir ultérieurement.

![Actions — génération de recommandations](/help/assets/guide-onboarding-agent/actions-generating.png)

Pendant la génération, [!DNL LLM Apps] :

1. Analyse le site web et identifie les intentions utiles du client.
2. Crée des métadonnées d’action, y compris des descriptions et des paramètres d’entrée.
3. Génère un gestionnaire et teste pour chaque action dans le référentiel de gestionnaires.
4. Génère un widget EDS pour chaque action dans le référentiel EDS.
5. Prépare les actions à réviser.

Les gestionnaires générés utilisent initialement des données d’exemple provenant du site web. Ils présentent une expérience complète, mais ne se connectent pas à vos systèmes de production.

## Vérifier les actions générées

Une fois la génération terminée, la page Actions affiche les actions générées et les aperçus de widget. Chaque action comporte une action **[!UICONTROL générée par l’IA, nécessite une révision]** un badge.

![Actions — actions générées prêtes à être révisées](/help/assets/guide-onboarding-agent/actions-ready-for-review.png)

Pour chaque action :

1. Sélectionnez **[!UICONTROL Vérifier]**.
2. Examinez le nom, la description, les paramètres, les annotations, le gestionnaire généré et le widget.
3. Sélectionnez **[!UICONTROL Marquer comme révisé]**. Cette opération fusionne les demandes d’extraction générées.
4. Revenez à la page Actions et répétez l’opération pour les actions restantes.

![Action générée : prête à être marquée comme révisée](/help/assets/guide-onboarding-agent/generated-action-review.png)

Lorsque toutes les actions sont passées en revue, sélectionnez **[!UICONTROL Aller à la page de l’application]**.

![Actions — toutes les actions générées examinées](/help/assets/guide-onboarding-agent/actions-reviewed.png)

>[!NOTE]
>
>Le code généré est un point de départ que vous possédez. Vous pouvez modifier les métadonnées d’action, les gestionnaires, les tests, le JavaScript de widget et les styles de widget après révision.

## Déploiement de l’application

1. Revenez à la page Détails de l’application.
2. Sélectionnez **[!UICONTROL Déployer]**.
3. Sélectionnez **[!UICONTROL Phase]** comme environnement cible.
4. Sélectionnez **[!UICONTROL Déployer]**.

![Déployer — sélectionner l&#39;environnement intermédiaire](/help/assets/guide-onboarding-agent/deploy-stage.png)

Patientez pendant que [!DNL LLM Apps] prépare, crée et publie l’application.

![Déployer — Pipeline de déploiement en cours d’exécution](/help/assets/guide-onboarding-agent/deploy-running.png)

![Déploiement — déploiement intermédiaire réussi](/help/assets/guide-onboarding-agent/deploy-successful.png)

Après le déploiement, la section **[!UICONTROL Tester l’application]** affiche l’URL du serveur MCP d’évaluation. Sélectionnez **[!UICONTROL Copier l’URL]**.

![Détails de l&#39;application — Copiez l&#39;URL du serveur MCP intermédiaire](/help/assets/guide-onboarding-agent/app-mcp-url.png)

## Test en [!DNL ChatGPT]

Suivez la procédure [Test dans ChatGPT](/help/guides/test-in-chatgpt.md) pour créer un plug-in à l’aide de l’URL du serveur MCP intermédiaire.

Posez une question correspondant à l’une des actions générées. Vérifiez que :

- [!DNL ChatGPT] sélectionne l’action attendue.
- Le widget effectue le rendu et contient les exemples de données attendus.
- Les contrôles de widget produisent le comportement de suivi attendu.
- La réponse textuelle résume précisément le résultat.

![ChatGPT — réponse du plug-in de l&#39;application LLM générée](/help/assets/guide-onboarding-agent/chatgpt-generated-app.png)

Vous disposez désormais d’une application de bout en bout entièrement fonctionnelle et fonctionnelle.

## Préparation de l’application pour la production

L’application générée utilise des exemples de données. Avant de l’utiliser avec les clients :

1. **Connecter vos systèmes** — [personnaliser chaque gestionnaire généré](/help/guides/customize-handler.md) pour remplacer les données d’exemple par des appels à vos API ou sources de données.
2. **Protéger les informations d’identification** — Stockez les URL d’API et les informations d’identification dans la configuration d’exécution gérée, jamais dans le code source ou le widget JavaScript.
3. **Valider les données** : validez les arguments d’action et les réponses de l’API, ajoutez des délais d’expiration de requête et renvoyez des messages d’erreur sécurisés.
4. **Mettre à jour les widgets** — Conservez chaque widget aligné sur les `structuredContent` de son gestionnaire, puis appliquez vos exigences en matière d’identité graphique et d’accessibilité. Voir [Personnaliser un widget généré](/help/guides/widgets.md).
5. **Tester les gestionnaires** : couvrent les entrées valides, non valides, les résultats vides, les échecs d’API et la forme de données attendue par le widget.
6. **Vérifier dans l’environnement intermédiaire** — redéployez et testez chaque action via le plug-in [!DNL ChatGPT].
7. **Déployer en production** — Une fois les tests d’évaluation réussis, déployez en production et créez ou mettez à jour le plug-in avec l’URL du serveur MCP de production.

Pour ajouter une fonctionnalité que la plateforme n’a pas créée, voir [Créer une action à partir de zéro](/help/guides/create-action.md).

