---
source-git-commit: eec74b87457bc852d7a8dd0e46c2a4385a93ae0a
workflow-type: tm+mt
source-wordcount: '387'
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

Tutoriel Principal : `help/guides/create-app.md`.

### Concept

Explique comment les articles sont liés sans devenir une procédure ou un catalogue de champs.

- Concentrez-vous sur les modèles mentaux et les limites de propriété.
- Utilisez un petit diagramme lorsque cela améliore la compréhension.
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
- **Agent d’intégration** — Fonction qui crée le modèle automatique initial.
- **Générer mon application** — Section de l’interface utilisateur dans la boîte de dialogue de création d’application.
- **Créer mon application automatiquement** — libellé exact de la case à cocher.
- **Action** — capacité exposée à la plateforme LLM.
- **Métadonnées d’action** : nom, description, schéma, annotations, visibilité et configuration de widget stockée par les applications LLM.
- **Gestionnaire d&#39;action** : fonction côté serveur dans le référentiel du gestionnaire.
- **Référentiel de gestionnaires** — Référentiel contenant des gestionnaires et des tests. Utilisez le libellé de l’interface utilisateur **référentiel standard** uniquement pour décrire ce contrôle.
- **Référentiel EDS** — Référentiel contenant des blocs de widgets et du contenu.
- **Widget** : réponse visuelle générée dans la plateforme LLM.
- **URL du serveur MCP** — Point d&#39;entrée déployé enregistré sur une plateforme LLM.
- **Plug-in ChatGPT** : l’intégration ChatGPT créée à partir d’une URL de serveur MCP.
- **Évaluation** et **Production** — environnements de déploiement.

Évitez de basculer entre « outil » et « action » dans une prose destinée à l’utilisateur, sauf en expliquant les détails d’un protocole MCP.

## Parcours de lecteur recommandé

1. Présentation et conditions préalables.
2. Créez une application avec l’agent d’intégration.
3. Examinez les actions générées.
4. Déployez sur l’environnement d’évaluation et testez le plug-in ChatGPT.
5. Personnalisez les gestionnaires et widgets générés.
6. Déployez l’application personnalisée en production.

La création d’une action à partir de zéro et l’introduction d’un projet EDS sont des branches avancées, et non le parcours de première exécution par défaut.
