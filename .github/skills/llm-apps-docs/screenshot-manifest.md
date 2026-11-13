---
source-git-commit: 0276ae4e4627e49157911ad579a42c0e77045eee
workflow-type: tm+mt
source-wordcount: '2314'
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
| `setup-choice.png` | Commencez avec **Laissez-nous le gérer** et **Utilisez ma configuration Cloud Manager**, légende sur l’option de référentiel personnel | Création de votre application LLM - Prise en main Laissez-nous la gérer et utiliser ma configuration Cloud Manager |
| `action-repository.png` | Programme et référentiel Cloud Manager sélectionnés, **Référentiel validé** | Configuration de l’infrastructure d’action - Programme et référentiel Cloud Manager sélectionnés |
| `widget-source.png` | Source du widget avec **Sélectionner manuellement le référentiel de widgets** (Cloud Manager EDS marqué Bientôt) | Configuration de l’infrastructure des widgets - Source des widgets |
| `widget-repository.png` | Référentiel de widgets non GitHub validé, **site Edge Delivery Services** remarque | Configuration de l’infrastructure des widgets : référentiel de widgets validé et son site EDS lié |
| `app-details.png` | Nom de l’application, jeu de programmes AEM du référentiel, région de données Informations sur l’application | Détails de l’application : nom, programme AEM et région de données Informations sur l’application |
| `settings-repositories.png` | Paramètres > Référentiel avec les cartes de référentiel Cloud Manager et le site lié | Paramètres de l’application - Section Référentiel |
| `provisioning.png` | Configuration gérée des applications LLM approuvées en cours | Configuration de l’application - Adobe prépare l’infrastructure |

Dernière capture (toutes sauf `provisioning.png` et `da-write-permissions.png`) : 2026-11-13, étape
(`experience-stage.adobe.com/llm-apps`), Référentiels d’actions et de widgets GitLab avec Bring Your
Propre Git. Les valeurs de programme, de référentiel, de site et d’origine sont masquées ; les contours rouges sont des légendes.
Non capturé : chemin d’accès du widget GitHub (installation de la synchronisation du code AEM, validation de la configuration d’AEM). Pas encore
vérifié en production.

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
| `generated-action-review.png` | Détails d’action, détails du widget LLM, informations sur l’application, extensions ChatGPT, notification **Branch push** (propres référentiels) et marquer comme révisé. Dernière capture : 2026-11-13, Phase, application GitLab ; nom de l’application masqué | Révision de l’action générée - Branche envoyée et Marquer comme révisée |

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
| `action-basic-info.png` | Détails de l’action : rechercher des produits, son identifiant de code, sa description et les paramètres `category` et `query` | Créer une action : détails de l’action avec des informations sur l’action et des paramètres d’entrée |
| `action-llm-settings.png` | Paramètres LLM étendus : lecture seule, idempotent, annotations de monde ouvert, textes de statut, visibilité | Créer une action - Paramètres LLM |
| `action-analytics-user-intent.png` | Application Insights : collectez l’intention de l’utilisateur et les instructions par défaut | Créer une action - Informations sur l’application |
| `widget-metadata.png` | Détails du widget LLM : bloc Edge Delivery Services, exemple de domaine de widget, description du widget et URL du modèle | Créer une action - Détails du widget LLM |
| `widget-csp.png` | Listes des URL de modèles et des domaines de configuration CSP | Création d’une action - Configuration de CSP |

Dernière capture : 2026-11-13, Phase, après la refactorisation de la boîte de dialogue (mêmes onglets que l’éditeur d’actions :
Détails d’action, détails du widget LLM, informations sur l’application, extensions ChatGPT ; autorisations,
Préfère une bordure et la section Paramètres de fichier (supprimée). Exemples de valeurs uniquement ; la boîte de dialogue
a été annulé sans enregistrement. Les contours rouges sont des légendes.

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

## Préparer la soumission

Article : `/help/guides/prepare-submission.md`

Répertoire de sortie : `help/assets/guide-submission/`

| Nom de fichier | État et contexte utile | Texte de remplacement suggéré |
|---|---|---|
| `submission-card.png` | Page d’application après un déploiement en production, Préparer la carte d’envoi avec Configuration | Préparer pour l’envoi - Statut et configuration de la préparation en production |
| `submission-setup.png` | Boîte de dialogue Préparer pour l’envoi, Configurer et préparer, exemples de détails de l’éditeur et les deux icônes enregistrées | Configurer et préparer - Éditeur et informations juridiques, icônes approuvées et Enregistrer les modifications |
| `submission-prepare.png` | La même boîte de dialogue a défilé pour Préparer votre package, Préparation indique Prêt pour la préparation, case à cocher désactivée | Préparer le package - Prêt pour la préparation, la case à cocher de confirmation et Préparer le package |
| `submission-progress.png` | Même boîte de dialogue lorsqu’une exécution est en cours | Préparation en cours - barre de progression et heure de début |
| `submission-results.png` | Résultats après une exécution terminée avec des liens légaux d’espace réservé : la préparation requiert une attention particulière, le package de téléchargement est désactivé. | Résultats : résumé de la préparation, rapport de téléchargement, package de téléchargement et enregistrement de téléchargement. |
| `submission-test-cases.png` | Résultats, cas de test, description du premier cas | Cas de test : cas de révision avec sa demande, son comportement attendu et son statut Enregistré |
| `submission-before-submit.png` | Résultats, avant d’envoyer la liste et les contrôles de réussite/avertissement/échec ; exécutez l’ID sous le recadrage | Avant envoi : détails restants, mise à jour des détails et vérifications du package. |

Capturé à l’étape 2026-11-13 à partir d’une application déployée en production. Utiliser uniquement
exemples de valeurs d’éditeur (`Frescopa Coffee (example)`, `https://www.example.com/...`)
et les icônes d’espace réservé. Chaque exécution de préparation appelle l’application déployée et prend environ
20 minutes ; démarrez-en une uniquement lorsque les écrans Résultats doivent être actualisés. Masquer les URL MCP,
ID d’exécution et noms d’application dans les captures de résultats.

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
