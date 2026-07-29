---
source-git-commit: eec74b87457bc852d7a8dd0e46c2a4385a93ae0a
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 0%

---
# Manifeste de capture d’écran d’intégration

Boîte de réception de capture : `docs-captures/<YYYY-MM-DD>/`

Répertoire de sortie : `help/assets/guide-onboarding-agent/`

Capturez uniquement les points de contrôle qui aident matériellement l’utilisateur à prendre une décision ou à vérifier l’état.

Les noms de fichier Source ne doivent pas nécessairement correspondre aux noms de fichier finaux. Les compétences font des captures d’écran par état visible de l’interface utilisateur, préservent les fichiers bruts et créent des copies assainies à l’aide des noms ci-dessous.

## Captures requises

### `app-details-onboarding.png`

- État : nom de l’application, région Analytics et **créer automatiquement mon application** sélectionnés.
- Inclure : détails de l’application, région Analytics et début de Créer mon application.
- Texte de remplacement : `Create LLM App — app details and Build My App enabled`

### `install-aem-code-sync.png`

- État : référentiel EDS vide initialisé avec le modèle standard AEM ; synchronisation du code AEM requise.
- Inclure : message de validation du référentiel EDS et lien d’installation.
- Texte de remplacement : `Create LLM App — empty EDS repository initialized and AEM Code Sync required`

### `eds-admin-required.png`

- État : synchronisation du code AEM installée, mais l’utilisateur actuel n’est pas un administrateur de site EDS.
- Inclure : le message de validation complet et **Ouvrir l’administration AEM Live**.
- Texte de remplacement : `Create LLM App — EDS administrator access required`

### `actions-generating.png`

- État : page Actions lorsque l’intégration est active.
- Inclure : message de progression et étapes de génération.
- Texte de remplacement : `Actions — Onboarding Agent generating recommendations`

### `actions-ready-for-review.png`

- État : liste d’actions générée une fois l’intégration terminée et avant l’approbation.
- Inclure : noms d’action, statut de génération/révision et contrôle de révision.
- Utilisez uniquement le contenu de l’appareil.
- Texte de remplacement : `Actions — generated actions ready for review`

### `generated-action-review.png`

- État : une action générée par un représentant.
- Inclure : navigation dans les métadonnées d’action et de widget, résultat de la génération du gestionnaire et **Marquer comme révisé**.
- Masque : propriétaire du référentiel, si nécessaire.
- Texte de remplacement : `Generated action — ready to mark as reviewed`

### `actions-reviewed.png`

- État : chaque action générée a été examinée.
- Inclure : **toutes les actions sont passées en revue**, les badges d’action et **Aller à la page de l’application**.
- Texte de remplacement : `Actions — all generated actions reviewed`

### `deploy-stage.png`

- État : boîte de dialogue de déploiement avant de commencer.
- Inclure : environnement cible d’évaluation et **Déployer**.
- Texte de remplacement : `Deploy — select the Stage environment`

### `deploy-running.png`

- État : pipeline de déploiement en cours d’exécution.
- Inclure : étapes de préparation, de démarrage, de création et de publication.
- Texte de remplacement : `Deploy — deployment pipeline running`

### `deploy-successful.png`

- Statut : déploiement intermédiaire réussi.
- Inclure : environnement et statut de réussite.
- Masque : espace de noms d’exécution, URL MCP complète, identifiants, horodatages si identification.
- Texte de remplacement : `Deploy — successful staging deployment`

### `app-mcp-url.png`

- État : test de la section d’application après le déploiement.
- Inclure : environnement d’évaluation, **Copier l’URL** et historique de déploiement réussi.
- Masque : URL du serveur MCP.
- Texte de remplacement : `App Detail — copy the staging MCP server URL`

### `chatgpt-plugins-page.png`

- État : page Plug-ins ChatGPT.
- Inclure : onglet Modules externes, bouton Rechercher et Créer .
- Texte de remplacement : `ChatGPT — Plugins page`

### `chatgpt-new-plugin.png`

- État : boîte de dialogue Nouveau plug-in.
- Inclure : nom, description, URL du serveur, authentification, accusé de réception et créer.
- Masque : URL du serveur MCP.
- Texte de remplacement : `ChatGPT — create a plugin with the MCP server URL`

### `chatgpt-plugin-connect.png`

- État : confirmation après la création du plug-in.
- Inclure : **Ajouter <plugin> vers ChatGPT **et** Connect **.
- Masque : URL du navigateur et identifiants de connecteur.
- Texte de remplacement : `ChatGPT — connect the new plugin`

### `chatgpt-generated-app.png`

- État : plug-in de correction appelé dans ChatGPT.
- Inclure : application jointe, widget généré et réponse textuelle.
- Exclure : historique des conversations, nom du compte et applications non liées.
- Texte de remplacement : `ChatGPT — generated LLM App plugin response`

## Captures facultatives

Ajoutez une capture uniquement lorsque la prose ne peut pas expliquer clairement la décision :

- Sélection de l’accès au référentiel de l’application GitHub.
- Échec de l’intégration pour le dépannage.
- Chargement de l’icône du plug-in.

N’ajoutez pas de captures d’écran pour les listes de champs statiques qui sont déjà effacées en prose.
