---
source-git-commit: fa80b7b68e1b905eb22d4789a74b2f0de886d70c
workflow-type: tm+mt
source-wordcount: '1963'
ht-degree: 1%
---
# Manifeste de capture d’écran

Capturez ces états grâce à l’automatisation du navigateur à l’aide de
[screenshots.md](screenshots.md). Sélectionnez les points de contrôle pertinents pour l’article ;
ce manifeste n&#39;autorise pas la création, la génération, l&#39;approbation ou
déploiement.

Les captures brutes restent dans `docs-captures/<run-id>/`. Publication uniquement révisée visuellement,
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

Les captures existantes affichent **Action** et **Métadonnées de widget** dans la création
, et **Détails du widget LLM**, **Détails de l’action** et **Analytics** dans le
éditeur enregistré. L’implémentation actuelle comporte également **Extensions ChatGPT** dans les deux
flux ; vérifiez que l’onglet dans l’interface utilisateur en cours d’exécution lors de l’actualisation des images. Capturer le
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

## Extensions de ChatGPT

Article : `/help/guides/chatgpt-extensions.md`

Répertoire de sortie : `help/assets/guide-chatgpt-extensions/`

Captures d’écran des points d’entrée globaux et de thread, paramètres d’affichage et mention du compositeur
des captures d’écran de configuration ont été fournies le 2026-11-10 et sont publiées comme suit :
enregistré ci-dessous. D’autres points de contrôle restent planifiés, mais pas les ressources existantes. Ne pas
ajoutez des liens d’image d’article jusqu’à ce que les captures réelles aient été assainies et visuellement
examiné.

| Nom de fichier | État et contexte utile | Texte de remplacement suggéré |
|---|---|---|
| `extensions-tab.png` | Action enregistrée, soutenue par un widget avec Extensions GPT de conversation sélectionnées | Extensions GPT de conversation - points d’entrée, mode d’affichage et mentions du compositeur |
| `global-thread-entrypoints.png` | Global et Thread sélectionnés ensemble pour le widget Accueil, avec Fichier décoché | Extensions GPT de conversation - Global et thread sélectionné pour l’action d’accueil |
| `global-entrypoint-menu.png` | Point d’entrée principal dans le menu de navigation du client | Menu de navigation GPT de conversation affichant le point d’entrée global d’accueil |
| `global-entrypoint-open.png` | Fenêtre client complète avec Accueil ouvert en plein écran et rail de navigation visible | Tableau de bord de l’accueil ouvert à partir du point d’entrée global |
| `thread-entrypoint-menu.png` | Menu Conversation avec extension Plug-ins | Menu Conversation - Choisissez le point d’entrée du thread d’accueil. |
| `thread-entrypoint-open.png` | Tableau de bord de l’accueil en regard d’une conversation | Tableau de bord de l’accueil ouvert en regard d’une conversation GPT de conversation |
| `file-entrypoint.png` | Point d’entrée de fichier non enregistré avec .csv | Point d’entrée de fichier : proposez une visionneuse personnalisée pour les fichiers CSV |
| `entrypoint-icon.png` | URL d’icône facultative non enregistrée partagée par les points d’entrée sélectionnés de l’action | Icône Point d’entrée : définissez une icône d’action partagée facultative |
| `display-settings.png` | Préférence de lancement du modèle, modes de ressources pris en charge et affichage initial aux valeurs par défaut de l’hôte | Mode d’affichage : préférences de lancement des ressources et des modèles par défaut |
| `composer-mentions.png` | Action de recherche de mentions en texte seul avec le bouton (bascule) de mention activé | Rechercher des mentions - mentions du compositeur activées |
| `composer-mentions-query.png` | Détails de l’action Mentions de recherche avec une entrée de requête de chaîne facultative | Mentions de recherche - entrée de chaîne facultative nommée requête |
| `composer-mentions-conflict.png` | Désactivation du bouton (bascule) de mention et avertissement de dénomination du fournisseur existant | Les mentions du compositeur ne sont pas disponibles, car le fournisseur est déjà Search Mentions |

Utilisez un élément de catalogue approuvé et non sensible. Confirmer que les formulaires ne sont pas enregistrés automatiquement
avant de saisir des exemples de valeurs ; ignorez les modifications non enregistrées par la suite. Ne pas enregistrer,
approuvez les actions générées ou déployez uniquement pour compléter cette liste de contrôle.
Les résultats tiers nécessitent un accès authentifié à un client compatible. Fichier
Les points d’entrée nécessitent un bureau ; le navigateur de travail n’est pas un ChatGPT classique.

### Mise à jour de la documentation Entrypoint, 2026-11-05

- Source : composants `llm-apps-ui` locaux actuels et validation d’entrée, pas de
Présentation de l’interface utilisateur de production.
- Les commandes d’action rapide ont été supprimées. Utilisation globale, de threads et de fichiers
cases à cocher qui prennent en charge plusieurs sélections pour un widget. L’URL de l’icône est une
paramètre facultatif au niveau de l’action partagé par les points d’entrée.
- La sélection du fichier ajoute une entrée d’objet facultative nommée `file` en cas d’absence.
La combinaison d’un fichier avec un thread ou un fichier global nécessite que cette entrée reste facultative.
- L&#39;inspection du navigateur a été bloquée car les deux profils de navigateur d&#39;automatisation étaient
déjà utilisé. Aucune nouvelle capture d’écran n’a été effectuée.

### Provenance du brouillon de texte, 2026-11-03

- Environnement Source : implémentation locale de l’interface utilisateur/l’API/le runtime/SDK, non terminée.
Présentation de l’éditeur d’action de production.
- API d’extension confirmées par la vérification du package publié dans runtime 1.3.0 et
SDK 0.2.1. Il s’agit des versions de base documentées, et non d’une revendication selon laquelle chaque
Cette fonctionnalité a été introduite dans ces versions exactes.
- La spécification d’extension publique d’OpenAI a été vérifiée pour les restrictions de plateforme.
- La connexion de production a réussi dans un navigateur séparé contrôlé par un agent. Aucune application
les paramètres ont été enregistrés, aucune correction n’a été sélectionnée et aucun déploiement n’a démarré.
- L’utilisateur a différé les captures d’écran. Aucune nouvelle image n’a été capturée ou remplacée.
Les libellés de contrôle de production exacts et le comportement de bout en bout de l’hôte nécessitent toujours un
présentation en direct avant publication.

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

Pour `/help/guides/test-in-chatgpt.md`, la valeur conservée
`help/assets/guide-onboarding-agent/chatgpt-generated-app.png` illustre un produit
réponse. Il ne vérifie pas l’interface utilisateur de configuration actuelle. Nouvelle configuration ChatGPT ou Claude
les captures d’écran nécessitent un accès authentifié actuel ; sinon, utilisez un fournisseur officiel
instructions et omettez les images non vérifiées.

## Configuration d’Analytics

Article : `/help/guides/analytics-setup.md`

Répertoire de sortie : `help/assets/guide-analytics/`

| Nom de fichier | État et contexte utile | Texte de remplacement suggéré |
|---|---|---|
| `analytics-settings.png` | Paramètres de l’application, onglet Analytics, Activation d’Analytics, ID de flux de données vide, Exigences développées, Enregistrement des modifications désactivé | Paramètres de l’application, onglet Analytics - Activation d’Analytics, champ d’identifiant de flux de données vide et liste des exigences développée |
| `datastream-analytics-service.png` | Collecte de données, ajout de service, Adobe Analytics activé avec un identifiant de suite de rapports factice | Ajouter un service : Adobe Analytics activé avec un exemple d’identifiant de suite de rapports |
| `datastream-platform-service.png` | Collecte de données, service Adobe Experience Platform avec uniquement le jeu de données d’événement sélectionné | service Adobe Experience Platform : jeu de données d’événement sélectionné et autres options effacées |
| `datastream-services.png` | Détails du flux de données avec les deux services activés, un identifiant de flux de données factice et le schéma de mappage | Détails du flux de données - les deux services sont activés, avec l’exemple d’identifiant de flux de données et le schéma de mappage |

Ne capturez jamais un ID de flux de données réel. Ne sélectionnez pas **Enregistrer les modifications** : après un enregistrement,
seul un administrateur peut modifier le flux de données. Afficher Experience Platform, Adobe Analytics,
et Customer Journey Analytics uniquement lorsqu’ils affichent un paramètre spécifique à
Applications LLM ; lien vers Experience League pour tout le reste. Remplacer l’organisation
noms, identifiants de suite de rapports, identifiants de flux de données, identifiants de jeu de données, adresses électroniques et noms personnels
avec ces valeurs factices : `example-org`, `demo.<suffix>` suites de rapports,
`00000000-0000-0000-0000-000000000000`, `0000…0001`, `user@example.com` et
`Example User`.

## Provenance de capture existante

L’exécution de capture 2026-10-19 a utilisé l’interface utilisateur locale demandée par l’utilisateur, avec l’interface utilisateur/l’API correspondante
vérifications de sources. Elle n’établit pas la parité de production.

- Captures brutes : `docs-captures/2026-10-19/` et son sous-répertoire `full-review/`.
- Les images publiques antérieures ou remplacées restent dans les sous-répertoires locaux `retired/`.
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

Actualisation de `guide-creating-actions/generated-action-review.png` à partir de l’exécution
Interface utilisateur locale après la suppression du commutateur Activé de l’éditeur. Visibilité de la page d’application
le bouton reste la commande d’activation/de désactivation ; paramètres d’exposition du modèle et du widget
restez dans l’éditeur. Le lien du référentiel est masqué et la navigation du compte est .
exclus. Aucune action n’a été enregistrée, approuvée, basculée ou déployée lors de cette actualisation.
Capture brute : `docs-captures/2026-10-19/remove-enabled/`.

### Paramètres d’Analytics, 21/10/2026

Actualisation de `guide-analytics/analytics-settings.png` à partir du fourni par l&#39;utilisateur
`Screenshot 2026-10-21 at 11.48.36.png`, exécution non pilotée par un agent. Source
environnement : non enregistré et non visible sur l’image ; la parité de production ne l’est pas
vérifié. Vérification visuelle confirmée L’activation d’Analytics dans le champ Identifiant du flux de données
est vide, le champ Exigences est développé et l’option Enregistrer les modifications est désactivée. L’image était
copiés sans modifications ; aucun identifiant ou détail de compte n’est visible.

### Captures d’écran de flux de données, 2026-10-19

`guide-analytics/datastream-analytics-service.png` ajouté,
`datastream-platform-service.png` et `datastream-services.png` à partir d’un existant
Captures de la collecte de données interne (2026-07-29 et 2026-10-19). Identifiant de suite de rapports
et l’identifiant du flux de données ont été remplacés par des superpositions de texte factices (`demo.aem.llm` et
`00000000-0000-0000-0000-000000000000`) ; la police de recouvrement est San Francisco, donc elle
diffère légèrement de la police du produit. L’image du service de plateforme avait besoin de
modifications. L’image du service Analytics affiche toujours le pointeur de la souris à partir de l’
capture originale. Un passage OCR sur les images finales n&#39;a trouvé aucun identifiant original.
Remplacez-les par de nouvelles captures qui utilisent des valeurs factices saisies dans l’interface utilisateur lorsque
l’article est ensuite actualisé.
