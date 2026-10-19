---
source-git-commit: 14d94a68dabddd2f2ef2cef65c51d31645d4a958
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 0%
---
# Modèle de contenu et terminologie

## Types de contenu

### Tutoriel

Enseigne à un nouvel utilisateur via un parcours complet et réussi.

- Indiquez le résultat et les conditions préalables.
- Utilisez un exemple d’application et une séquence.
- Expliquez uniquement les concepts nécessaires à chaque étape.
- Terminez par un résultat fonctionnel et effacez les étapes suivantes.

Parcours de première exécution : `help/guides/create-app.md` (configuration de l’infrastructure), puis `help/guides/creating-actions.md` (génération et révision de l’action).

### Concept

Explique comment les articles sont liés sans devenir une procédure ou un catalogue de champs.

- Concentrez-vous sur les modèles mentaux et les limites de propriété.
- Expliquez le rôle de chaque partie, comment l’utilisateur ou l’utilisatrice le perçoit et où apporter des modifications.
- Conservez les outils de création, les chemins d’accès aux fichiers, les appels de protocole et les contrats de code dans les guides techniques ou les références liés, et non dans la présentation générale.
- Utilisez un petit diagramme lorsque cela améliore la compréhension.
- Conserver les diagrammes existants utiles lors de la simplification d’une page ; simplifier les libellés plutôt que de supprimer l’explication visuelle.
- Lien vers des tutoriels, des guides pratiques et des références.

### Guide pratique

Permet à un utilisateur averti d’effectuer une tâche.

- Commencez par le résultat souhaité.
- N’inclure que les prérequis spécifiques à la tâche.
- Privilégiez un chemin d’accès recommandé.
- Lien vers une référence pour les champs exhaustifs.

Exemples : créer une action à partir de zéro, personnaliser un widget, apporter un projet EDS, déployer et tester.

### Référence

Fournit des informations factuelles que les utilisateurs consultent pendant leur travail.

- Organisez par produit ou surface de code.
- Définissez précisément chaque champ, contrat, commande, limite et statut.
- Évitez la narration du tutoriel et les exemples répétés.

### Résolution des problèmes

Commence par un symptôme observable.

- Décrivez les causes probables.
- Effectuez des étapes de diagnostic sûres.
- Évitez de demander aux utilisateurs de révéler des informations d’identification ou des journaux sensibles.

## Terminologie canonique

- **Applications Adobe LLM** — nom complet du produit sur la première mention.
- **Application LLM** : une application gérée par le produit.
- **Infrastructure gérée par Adobe** — infrastructure créée et gérée par Adobe ; sélectionnez **Laissons-nous la gérer** dans l’assistant de création d’application. Ne décrivez pas cette option comme étant à l’essai uniquement ou ne faites pas d’affirmations non confirmées sur l’éligibilité de la production.
- **Je le configurerai moi-même** — option d’assistant pour connecter vos propres référentiels, pas d’exigence d’écriture manuelle d’actions. Le chemin d’accès par défaut commence par deux référentiels vides initialisés par la plateforme. Les projets existants compatibles sont facultatifs.
- **Créer automatiquement des actions** : choix recommandé de création d’action sur la page de l’application après la configuration de l’infrastructure.
- **Créer mes actions** — bouton qui lance la génération d’actions sur le site web.
- **Action** — capacité exposée à la plateforme LLM.
- **Métadonnées d’action** : nom, description, schéma, annotations, visibilité et configuration de widget stockée par les applications LLM.
- **Visibilité** — L&#39;action de la page de l&#39;application active/désactive. Il contrôle si le prochain déploiement inclut l’action . L’éditeur d’actions ne dispose pas de commutateur Activé distinct ; ses paramètres d’exposition du modèle et du widget ont un objectif différent.
- **Gestionnaire d&#39;action** : fonction côté serveur dans le référentiel du gestionnaire.
- **Identifiant du code** — Identifiant d&#39;action non modifiable utilisé dans les appels MCP, distinct du nom d&#39;affichage. Les noms de gestionnaire et de dossier de blocs générés utilisent un formulaire avec des traits d’union ; le runtime résout également les traits de soulignement dans l’identifiant en traits d’union dans les chemins de gestionnaire.
- **Référentiel de gestionnaires** — Référentiel contenant des gestionnaires et des tests, sélectionnés lors de la **Configuration de l’infrastructure d’action** lors de la connexion de vos propres référentiels.
- **Référentiel EDS** — Référentiel contenant des blocs de widgets et du contenu.
- **Widget** : réponse visuelle générée dans la plateforme LLM.
- **`structuredContent`** — données structurées action-résultat utilisées par le widget. Ne le décrivez pas comme privé, invisible ou sans jeton ; il fait partie du résultat du MCP diffusé à l’hôte.
- **URL du serveur MCP** — Point d&#39;entrée déployé enregistré sur une plateforme LLM.
- **Plug-in ChatGPT** : l’intégration ChatGPT créée à partir d’une URL de serveur MCP.
- **Évaluation** et **Production** — environnements de déploiement.

Évitez de basculer entre « outil » et « action » dans une prose destinée à l’utilisateur, sauf en expliquant les détails d’un protocole MCP.

Le produit est indépendant de la plateforme : son serveur MCP fonctionne avec n&#39;importe quelle plateforme LLM prise en charge, pas seulement ChatGPT. Utilisez « une plateforme LLM prise en charge, telle que ChatGPT » (ou une plateforme similaire) pour des affirmations générales ou illustratives. Nommez ChatGPT uniquement dans le contenu qui est véritablement spécifique à ChatGPT aujourd’hui : le guide Test dans ChatGPT, ses liens directs et le contenu de référence ou de dépannage spécifique à ChatGPT.

## Séquence de lecteur recommandée

1. Vue d’ensemble.
2. Créez une application à l’aide de l’infrastructure gérée par Adobe ou de vos propres référentiels.
3. Démarrez la génération d’actions sur le site web à partir de la page d’application et passez en revue les actions générées.
4. Déployer dans l’environnement d’évaluation et tester dans une plateforme LLM prise en charge.
5. Personnalisez les gestionnaires et widgets générés.
6. Déployez l’application personnalisée en production.

La création d’une action à partir de zéro et l’introduction d’un projet EDS sont des branches avancées, et non le parcours de première exécution par défaut.

Le guide BYO EDS est actuellement masqué dans la table des matières avec `hidefromtoc: true`.
Conservez ce statut sauf si l’utilisateur demande à restaurer l’entrée de navigation.
