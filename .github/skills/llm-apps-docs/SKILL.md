---
name: llm-apps-docs
description: Créez, examinez et validez la documentation sur les applications Adobe LLM, appliquez ses normes éditoriales et capturez des captures d’écran actuelles de l’interface utilisateur grâce à l’automatisation du navigateur. Utilisez pour les articles llm-apps.en, la navigation, les captures d’écran et les lots de révision hors ligne.
source-git-commit: 14d94a68dabddd2f2ef2cef65c51d31645d4a958
workflow-type: tm+mt
source-wordcount: '1380'
ht-degree: 0%
---

# Documentation sur les applications LLM

Créez une documentation publique vérifiable et orientée tâche pour les applications Adobe LLM.

Cette compétence Copilote référentiel-local réside à `.github/skills/llm-apps-docs/`.

## ordre de Source-de-vérité

Utiliser l’interface utilisateur et l’environnement d’exécution spécifiés par l’utilisateur. Sinon, la valeur par défaut est
Production à `https://experience.adobe.com/llm-apps`.
Vérifiez les revendications de produit dans cet ordre pour cet environnement :

1. Interface utilisateur actuelle, observée via l’automatisation du navigateur.
2. Implémentation de l’interface utilisateur et de l’API correspondante dans l’espace de travail.
3. Le SDK public actuel, le runtime et le standard.
4. Documentation publique existante

Enregistrez la date de capture et l’environnement source dans le manifeste de capture d’écran. Ne pas
présenter les observations locales ou intermédiaires comme un comportement de production vérifié ; Si l’interface utilisateur :
mise en œuvre et directives publiées en désaccord, signalez l’inadéquation et décrivez
seul le comportement que les preuves disponibles confirment.

## Démarrer chaque tâche

1. Lisez `help/main-toc/TOC.md`.
2. Lisez l’article cible et les articles directement liés.
3. Classez le contenu à l’aide de [content-model.md](content-model.md).
4. Identifiez les libellés d’interface utilisateur, les URL, les commandes et les contrats qui doivent être vérifiés.
5. Conservez la cohérence de l’exemple et de la terminologie de bout en bout sur plusieurs pages.
6. Lorsque la tâche nécessite une vérification de l’interface utilisateur ou des captures d’écran, inspectez l’interface utilisateur en cours d’exécution et capturez les images vous-même. Demandez à l’utilisateur d’authentifier ou d’approuver les modifications de ressource, et non d’assembler un pack de captures d’écran.

## Règles de création

- Diriger les nouveaux utilisateurs et utilisatrices via **configuration de l’infrastructure**, puis **création d’actions**. L’assistant de création d’application configure l’infrastructure uniquement ; **[!UICONTROL Créer automatiquement des actions]** et **[!UICONTROL Créer mes actions]** lancez ensuite la génération d’actions sur le site web, à partir de la page de l’application. Utilisez **l’infrastructure gérée par** pour l’option intitulée **[!UICONTROL Laissez-nous la gérer]** dans l’interface utilisateur.
- Décrivez de manière neutre l’infrastructure gérée par Adobe telle qu’Adobe la crée et la gère. Ne l’étiquetez pas comme une option d’évaluation, de configuration temporaire ou de non-production uniquement et ne faites pas de déclarations non confirmées sur l’éligibilité de la production.
- Organisez la navigation autour des parcours utilisateur et des résultats, et non des rubriques d’implémentation.
- Au début de chaque guide, utilisez **Dans ce guide :** suivi d’une ou de deux phrases en langage clair expliquant la tâche et l’étape suivante. Préférer « Découvrez comment... » sur des promesses aux tensions futures. N&#39;utilisez pas de libellés **Parcours:** ou de listes d&#39;étapes séparées par des flèches.
- N’utilisez pas de noms de code internes (par exemple, « Agent d’intégration ») dans les documents destinés aux clients. Cette fonctionnalité n’est jamais exposée dans l’interface utilisateur du produit. Décrivez-la de manière générique (par exemple, « la plateforme ») et utilisez la copie exacte actuelle de l’interface utilisateur, telle que **[!UICONTROL Créer mes actions]** pour les contrôles.
- [!DNL Adobe LLM Apps] ne dépend pas de la plate-forme : son serveur MCP fonctionne avec n’importe quelle plate-forme LLM prise en charge, et pas seulement [!DNL ChatGPT]. Ne pas formuler d’affirmations générales ou indicatives comme si [!DNL ChatGPT] étiez la seule cible (par exemple, préférer « une plateforme LLM prise en charge telle que [!DNL ChatGPT] » à « ChatGPT » uniquement). Seul le nom [!DNL ChatGPT] explicitement dans le contenu qui est véritablement et actuellement spécifique au [!DNL ChatGPT] : le guide [Test dans le ChatGPT](/help/guides/test-in-chatgpt.md) dédié, ses liens directs/étapes de procédure et le contenu de référence ou de dépannage spécifique au [!DNL ChatGPT].
- Expliquez un concept technique lorsque l’utilisateur ou l’utilisatrice le rencontre pour la première fois ; liez-le à un concept plus approfondi ou à des documents de référence.
- Maintenez les tutoriels linéaires, les guides pratiques axés sur les tâches et les pages de référence factuelles.
- N’incluez que les informations dont le lecteur a besoin pour la tâche en cours ; préférez des phrases courtes et directes.
- Utilisez une application représentative sur l’ensemble du parcours.
- Distinguer le modèle automatique généré de l’intégration prête pour la production.
- Évitez les noms de programmes de travail internes, les champs de base de données, les tickets d’implémentation et les détails de pipelines instables.
- Ne dupliquez pas les tableaux de champs dans les guides ; liez-les à des références.
- Préservez le matériel et les directives d’Experience League : `[!DNL]`, ``, `[!IMPORTANT]`, `[!NOTE]` et `[!TIP]`.
- Utilisez des liens internes relatifs à la racine : `/help/...`.
- Utilisez la casse de phrase pour les titres et les en-têtes sauf si une étiquette de produit en exige autrement.
- Utilisez un texte secondaire d’image descriptive qui explique l’écran et le statut.

## Normes éditoriales

- Adressez-vous directement au lecteur et utilisez les instructions impératives suivantes : « Sélectionnez Enregistrer » et non « Vous pouvez sélectionner Enregistrer ».
- Placez l’objectif avant l’action lorsque l’objectif nécessite une explication : « Pour ajouter un paramètre, sélectionnez Ajouter ». Évitez les clauses d’objet redondantes.
- Préférez une voix active et le présent. Nommez l’acteur quand c’est important : « La plateforme génère le gestionnaire ».
- Remplacez les références ambiguës telles que « this », « this » ou « it » par un nom spécifique lorsque la référence n’est pas claire.
- Gardez une idée principale par phrase. Raccourcissez les phrases longues et fractionnez les clauses incorporées lorsque le détail supplémentaire obscurcit la tâche. Conservez les conditions et avertissements nécessaires.
- Réduire les chaînes prépositionnelles et les groupes nominaux denses. Préférez « configurer les variables d’application pour l’étape » à « effectuer la configuration des variables de l’application pour l’étape ».
- Utilisez un langage simple au lieu d’expressions idiomatiques, figuratives ou superflues. Expliquez les termes techniques inconnus lorsque le lecteur ou la lectrice les rencontre pour la première fois.
- Vérifiez l&#39;ordre des mots, la structure des phrases, les mots manquants ou dupliqués, la ponctuation et la majuscule au début de la phrase. Garder les éléments de liste grammaticalement parallèles.
- Conservez les majuscules et les noms officiels des produits : Adobe LLM Apps, Adobe I/O Runtime, Edge Delivery Services, GitHub, ChatGPT, Claude et Model Context Protocol (MCP). Conservez les étiquettes d’interface utilisateur et l’orthographe exactes du code même lorsqu’elles diffèrent des conventions de prose.
- Utilisez un langage inclusif et respectueux. Évitez les stéréotypes, les préjugés sexistes, les métaphores stigmatisantes et les formulations qui présument que tous les lecteurs ont les mêmes capacités ou la même expérience. Utilisez des termes tels que « » et « » lorsque cela est exact ; ne renommez pas des API littérales, des branches, des commandes ou des identifiants de code pour une préférence éditoriale.
- Conservez les verbes modaux lorsqu’ils expriment des fonctionnalités, des autorisations, des conditions, des incertitudes ou des variations d’hôte réels. Ne transformez pas une estimation en garantie ou une recommandation en exigence.
- Conservez les libellés exacts de l’interface utilisateur, les commandes, les exemples de code, les identifiants, les contrats techniques et la copie approuvée par le fabricant pendant une passe éditoriale.
- Utiliser les résultats automatisés comme signaux de révision, et non comme substitutions obligatoires. Recherchez la phrase marquée, corrigez le problème réel ou conservez un libellé significatif avec un motif. Une catégorie de grammaire générique ou un score agrégé ne suffit pas à identifier une erreur spécifique. Ne demandez pas un score amélioré sans exécuter le même évaluateur.

## Descriptif protégé approuvé par le PM

En `help/overview/overview.md`, ces sections sont approuvées par le responsable principal :

- **Que pouvez-vous faire avec les applications LLM**
- **Pourquoi les applications LLM sont importantes**

Conservez leurs en-têtes, puces, libellés, ordres et assertions verbatim.
Ne les raccourcissez pas, ne les réécrivez pas, ne les réorganisez pas et ne les supprimez pas dans la documentation générale
mises à jour. Modifiez l’une ou l’autre section uniquement lorsque l’utilisateur la demande explicitement et
confirme que la nouvelle copie est approuvée par le PM.

## Exigences de sécurité

- N’incluez jamais d’informations d’identification, de jetons, d’URL privées, de données personnelles, de noms d’hôte internes ou d’identifiants client.
- Afficher les secrets chargés depuis la configuration gérée, jamais codés en dur.
- Exiger HTTPS pour les services externes.
- Validez les entrées externes et les réponses en amont.
- Effectuez le rendu de valeurs externes avec des API DOM sécurisées ; il est déconseillé de les interpoler dans des `innerHTML`.
- Recommandez les autorisations de navigateur, l’API, la CSP, CORS et l’application GitHub avec les privilèges les moins élevés.
- Utilisez des erreurs sécurisées visibles par l’utilisateur et évitez de consigner des données sensibles.

## Workflow de capture d’écran

Prenez des captures d’écran grâce à l’automatisation du navigateur disponible. S’abonner
[screenshots.md](screenshots.md) pour la configuration du navigateur, l’authentification, la navigation sécurisée,
les contrôles de stabilité, la désinfection et la révision des images. Utiliser
[screenshot-manifest.md](screenshot-manifest.md) pour les points de contrôle et noms de fichier actuels.

1. Ouvrez l’interface utilisateur d’exécution demandée et mettez-la en pause pour que l’utilisateur s’authentifie.
2. Réutilisez une structure de documentation approuvée et examinez les états requis de l’interface utilisateur.
3. Capturez des captures d’écran stables dans le répertoire `docs-captures/<run-id>/` ignoré. N’attendez pas qu’un pack de capture créé par l’utilisateur soit disponible.
4. Recadrez ou masquez les copies sécurisées pour `help/assets/`, puis examinez visuellement chaque image finale.
5. Faites correspondre l’article, les libellés exacts de l’interface utilisateur et le texte de remplacement à l’état observé.
6. Validez les liens et les ressources, puis inspectez le site de révision rendu.

Demandez avant de créer des applications ou des référentiels, de commencer la génération, de valider un
référentiel pouvant recevoir des fichiers standard, enregistrement des paramètres, fusion générée
extraire des requêtes, déployer, supprimer ou modifier l’accès. Conserver l’authentification et
consentement privilégié avec l’utilisateur. N’inspectez ou n’exportez jamais les informations d’identification, les jetons,
les cookies ou l’espace de stockage du navigateur et n’exécutez jamais un flux de capture mutant sans approbation.

Si l’automatisation, l’accès ou un état requis n’est pas disponible, signalez les
bloqueur. Ne créez pas de capture d’écran, ne réutilisez pas silencieusement une image obsolète ou ne présentez pas
vérification de la source uniquement en tant que nouvelle capture de l’interface utilisateur. Conserver les captures sources existantes.

## Création d’une archive de révision

Générez un site HTML et une archive ZIP partageables hors ligne :

```bash
node .github/skills/llm-apps-docs/scripts/build_review_bundle.mjs
```

La version est écrite en regard du référentiel, et non à l’intérieur. Il comprend uniquement
articles publiés et ressources assainies, convertit les directives Experience League
pour une révision hors ligne et note que son style n’est pas l’expérience finale.
Rendu de League.

## Valider

Exécutez :

```bash
python3 .github/skills/llm-apps-docs/scripts/validate_docs.py
```

Corrigez tous les articles internes manquants, les ressources manquantes, les chemins d’accès relatifs à la racine non valides et les champs frontend manquants avant de les transmettre.

Vérifiez également :

- Les libellés et captures d’écran de l’interface utilisateur correspondent à l’environnement source enregistré.
- Les exemples d’URL de script et de widget s’accordent dans les guides et les références.
- Les commandes correspondent au standard actuel.
- Les nouvelles pages sont liées à partir de la table des matières, sauf si elles sont intentionnellement marquées `hidefromtoc: true`.
- Le workflow de validation d’article Adobe réussit lorsqu’il est disponible.

## Références annexes

- [Modèle de contenu et terminologie](content-model.md)
- [Procédure de capture d’écran pilotée par l’agent](screenshots.md)
- [Manifeste de capture d’écran](screenshot-manifest.md)
