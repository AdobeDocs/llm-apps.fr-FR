---
source-git-commit: a48b4022979d9a13b4fc3aa870c4ee6375e8bc13
workflow-type: tm+mt
source-wordcount: '1052'
ht-degree: 1%
---
# Manifeste de capture d’écran

Capturez ces états grâce à l’automatisation du navigateur à l’aide de
[screenshots.md](screenshots.md). Sélectionnez les points de contrôle pertinents pour l’article ;
ce manifeste n&#39;autorise pas la création, la génération, l&#39;approbation ou
déploiement.

Les captures brutes restent en `docs-captures/<run-id>/`. Publication uniquement révisée visuellement,
PNG assainis sous `help/assets/`. Gardez les noms de fichier ci-dessous stables lorsque
actualisation des images.

## Configuration de l’infrastructure

Article : `/help/guides/create-app.md`

Répertoire de sortie : `help/assets/guide-infrastructure/`

| Nom de fichier | État et contexte utile | Texte de remplacement suggéré |
|---|---|---|
| `setup-choice.png` | Prise en main des deux options d’infrastructure visibles | Création de votre application LLM : choisissez l’infrastructure gérée par Adobe ou vos propres référentiels. |
| `app-details.png` | Nom de l’application, programme AEM, zone géographique Analytics et avertissement de paramètre non modifiable | Détails de l’application : saisissez un nom, sélectionnez un programme AEM, puis choisissez une région Analytics |
| `action-repository.png` | Chemin d’accès au référentiel avec GitHub sélectionné et instructions de connexion au référentiel | Configuration de l’infrastructure d’action : connectez un référentiel d’action vide ou compatible. |
| `provisioning.png` | Configuration gérée d’Adobe approuvée en cours | Configuration de l’application - Adobe prépare l’infrastructure |

Exclure les détails du compte, les ID d’organisation, les noms de programme réels, le référentiel
identifiants et ID d’application. Utilisez des champs vides ou un nom d&#39;appareil neutre où
possible. La validation du référentiel peut initialiser le code, donc obtenez l’approbation avant
capture du résultat de validation.

## Création d’actions et révision du travail généré

Article : `/help/guides/creating-actions.md`

Répertoire de sortie : `help/assets/guide-creating-actions/`

| Nom de fichier | État et contexte utile | Texte de remplacement suggéré |
|---|---|---|
| `build-choice.png` | Les actions de build sont automatiquement sélectionnées, un site web public est entré et une commande de démarrage stable est incluse, le cas échéant | Actions : choisissez la génération automatique et saisissez un site web public. |
| `generating.png` | Génération en cours, estimation du temps et Accéder aux actions | Actions - La génération est en cours |
| `actions-ready-for-review.png` | Noms des actions de correction, badges de révision et contrôles de révision | Actions : les actions générées sont prêtes pour la révision et ne sont pas déployées. |
| `generated-action-review.png` | Détails du widget LLM, détails de l’action, Analytics, avis de demande d’extraction générée et Marquer comme révisé | Action générée : inspectez le widget et le gestionnaire avant l’approbation |

Masquez le référentiel privé et extrayez les liens de requête. Supprimer les données personnelles ou
coordonnées lorsque l’article n’a besoin que du statut de révision. Ne pas sélectionner
**Marquer comme révisé** sans approbation.

Capturez l’état toutes les actions examinées uniquement lorsque l’approbation est dans la portée et le
l&#39;État existe vraiment. Ne pas approuver le travail généré pour répondre à une capture d’écran
liste de contrôle.

## Création d’une action à partir de zéro

Article : `/help/guides/create-action.md`

Répertoire de sortie : `help/assets/guide-create-action/`

| Nom de fichier | État et contexte utile | Texte de remplacement suggéré |
|---|---|---|
| `action-basic-info.png` | Rechercher des produits, son identifiant dérivé, une description utile et des annotations en lecture seule | Créer une action : informations de base et annotations |
| `action-analytics-user-intent.png` | Collecter l’intention de l’utilisateur et les instructions de résumé anonymisé par défaut | Créer une action - vérifier la collection d’intentions de l’utilisateur |
| `widget-metadata.png` | Type de bloc Edge Delivery Services et exemple d’origine sandbox sécurisée | Création d’une action - Type de widget et domaine de sandbox |
| `widget-template-urls.png` | Exemples d’URL de script et de widget | Créer une action - URL de script et de widget |
| `widget-permissions.png` | Autorisations de navigateur facultatives, avec autorisations inutilisées désactivées | Création d’une action - Autorisations du navigateur |
| `widget-csp.png` | Contrôles de domaine URI de ressource, de connexion, de trame, de redirection et de base | Créer une action - domaines de la politique de sécurité du contenu |

La boîte de dialogue de création comporte des onglets **Action** et **Métadonnées de widget**. Éditeur enregistré
comporte les onglets **Détails du widget LLM**, **Détails de l’action** et **Analytics**. Capturer le
disposition que la procédure utilise réellement. Remplissez les champs d’exemple sans enregistrer, puis
ignorez les modifications.

## Authentification

Article : `/help/guides/authentication.md`

Répertoire de sortie : `help/assets/guide-authentication/`

| Nom de fichier | État et contexte utile | Texte de remplacement suggéré |
|---|---|---|
| `auth-core-settings.png` | Évaluation, authentification activée, `https://auth.example.com`, `catalog:read`, `profile:read` et paramètres avancés | Authentification : activez l’authentification et renseignez les paramètres principaux. |
| `auth-per-action.png` | Actions de correction affichant Aucune, Obligatoire, Facultatif et une portée affectée | Authentification : sélectionnez un mode d’authentification et des portées pour chaque action. |

Utilisez les paramètres non enregistrés et ignorez-les par la suite. Vérifier que l’authentification enregistrée
reste inchangé. L’étape d’identifiant de ressource réutilise
`help/assets/guide-onboarding-agent/app-mcp-url.png`.

Si l’interface utilisateur affiche un avertissement qui entre en conflit avec le runtime déployé ou le
comportement de la plateforme, documenter la différence. Un recadrage de table uniquement est approprié
lors de l’enseignement des modes, mais ne masquez pas un avertissement pertinent sans l’expliquer.

## Variables de l’application

Article : `/help/guides/app-variables.md`

Répertoire de sortie : `help/assets/guide-app-variables/`

Utilisez `GREETING_PREFIX` avec `Good day` comme valeur d’exemple et `Howdy` comme
valeur de remplacement. Ne capturez jamais une valeur de configuration réelle.

| Nom de fichier | État et contexte utile | Texte de remplacement suggéré |
|---|---|---|
| `variables-empty.png` | Espace de travail d’évaluation vide, sélecteur Workspace et Ajouter | Variables et secrets - espace de travail d’étape vide |
| `add-variable-dialog.png` | Variable GREETING_PREFIX non enregistrée et avis de prise en charge du secret actuel | Ajouter une variable ou un secret - saisissez GREETING_PREFIX et Good day |
| `variable-added.png` | Variable d&#39;installation approuvée avec contrôle de nom, de type, de valeur, d&#39;horodatage et de ligne | Variables et secrets - GREETING_PREFIX dans l’espace de travail d’évaluation |
| `update-variable-dialog.png` | Valeur actuelle Bonne journée et nouvelle valeur non enregistrée Howdy | Mettre à jour GREETING_PREFIX - remplacez Good day par Howdy |
| `delete-variable-dialog.png` | Boîte de dialogue de confirmation sans confirmation de suppression | Delete GREETING_PREFIX - Vérifier la confirmation de suppression |

L’ouverture d’une boîte de dialogue n’autorise pas d’enregistrement ou de suppression. Réutiliser un existant sécurisé
fixer une variable ou une tâche avant d&#39;en créer une. Conserver une ancienne image uniquement après
la vérification que ses champs et contrôles correspondent toujours à l’interface utilisateur actuelle.

## Déploiement et tests

Article : `/help/guides/deploy-your-app.md`

| Répertoire de sortie | Nom de fichier | État et contexte utile |
|---|---|---|
| `help/assets/guide-onboarding-agent/` | `deploy-stage.png` | Étape de l’environnement cible et contrôle de déploiement, avant de commencer |
| `help/assets/guide-onboarding-agent/` | `app-mcp-url.png` | Tester la carte de l’application, les libellés d’environnement et Copier l’URL ; masquer chaque URL d’exécution |
| `help/assets/guide-deploy/` | `deployment-history.png` | Colonnes de date d’environnement, de statut et de déploiement sans identifiants d’application |

Utilisez un déploiement existant en lecture seule pour la carte de test et l’historique lorsque cela est autorisé.
Ne démarrez pas un déploiement pour obtenir ces états, sauf si l’utilisateur ou l’utilisatrice le valide.
Capturez les boîtes de dialogue de déploiement en cours ou réussies uniquement lorsque cet état est à la fois
pertinentes et autorisées.

Par `/help/guides/test-in-chatgpt.md`, la valeur conservée
`help/assets/guide-onboarding-agent/chatgpt-generated-app.png` illustre un produit
réponse. Il ne vérifie pas l’interface utilisateur de configuration actuelle. Nouvelle configuration ChatGPT ou Claude
les captures d’écran nécessitent un accès authentifié actuel ; sinon, utilisez un fournisseur officiel
instructions et omettez les images non vérifiées.

## Provenance de capture existante

L’exécution de capture 2026-10-19 a utilisé l’interface utilisateur locale demandée par l’utilisateur, avec l’interface utilisateur/l’API correspondante
vérifications de sources. Elle n’établit pas la parité de production.

- Captures brutes : `docs-captures/2026-10-19/` et son sous-répertoire `full-review/`.
- Les images publiques antérieures ou remplacées restent dans les sous-répertoires `retired/` locaux.
- L’élément de documentation a atteint l’étape de révision de l’action générée. Aucune action
approbations, fusions des demandes d’extraction, déploiements ou authentification/variable enregistrée
les modifications faisaient partie de cette exécution de capture.
- Les images de ligne de variable, de mise à jour et de suppression d’une exécution précédente ont été conservées après
en vérifiant leurs contrôles par rapport à l’implémentation actuelle.
- Les écrans de configuration tiers n’ont pas été capturés à nouveau. Les captures d’écran de la configuration obsolète étaient
a supprimé au lieu d’être présenté comme actuel.

Pour chaque nouvelle actualisation, mettez à jour la date, l’environnement source, les états vérifiés,
l’assainissement et toutes les limites de vérification restantes. Ne pas copier l&#39;ancienne provenance
vers l&#39;avant comme preuve d&#39;une nouvelle capture.

### Suppression du commutateur de l’éditeur d’actions, 2026-10-19

Actualisation des `guide-creating-actions/generated-action-review.png` en cours d’exécution
Interface utilisateur locale après la suppression du commutateur Activé de l’éditeur. Visibilité de la page d’application
le bouton reste la commande d’activation/de désactivation ; paramètres d’exposition du modèle et du widget
restez dans l’éditeur. Le lien du référentiel est masqué et la navigation du compte est .
exclus. Aucune action n’a été enregistrée, approuvée, basculée ou déployée lors de cette actualisation.
Capture brute : `docs-captures/2026-10-19/remove-enabled/`.
