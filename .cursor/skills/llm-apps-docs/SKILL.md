---
name: llm-apps-docs
description: Créez, mettez à jour, examinez et validez la documentation publique et les captures d’écran des applications Adobe LLM. Utilisez lors de la modification des articles llm-apps.en , sa table des matières Experience League, ses conseils sur l’intégration-agent, ses documents sur les widgets EDS, ses conseils de préparation à la production ou ses captures d’écran de documentation.
source-git-commit: bb3d8a02f22a91ceeeba5999453aeb4221060f80
workflow-type: tm+mt
source-wordcount: '813'
ht-degree: 0%

---


# Documentation sur les applications LLM

Créez une documentation publique vérifiable et orientée tâche pour les applications Adobe LLM.

## ordre de Source-de-vérité

Vérifiez les revendications de produit dans cet ordre :

1. Interface utilisateur de production actuelle à `https://experience.adobe.com/#/@llmapps/llm-apps/`
2. Implémentation actuelle de l’interface utilisateur et de l’API disponible dans l’espace de travail
3. SDK publique actuelle et comportement standard
4. Documentation publique existante

Si la production entre en conflit avec la source ou les plans, indiquez la production et signalez l&#39;incohérence. Ne publiez pas de workflow à venir tel qu’il est actuellement disponible.

## Démarrer chaque tâche

1. Lisez `help/main-toc/TOC.md`.
2. Lisez l’article cible et les articles directement liés.
3. Classez le contenu à l’aide de [content-model.md](content-model.md).
4. Identifiez les libellés d’interface utilisateur, les URL, les commandes et les contrats qui doivent être vérifiés.
5. Conservez la cohérence de l’exemple et de la terminologie de bout en bout sur plusieurs pages.

## Règles de création

- Accompagner les nouveaux utilisateurs dans la création automatique d’applications (le flux **[!UICONTROL Créer automatiquement mon application]**).
- Organisez la navigation autour des parcours utilisateur et des résultats, et non des rubriques d’implémentation.
- Indiquez la séquence de parcours au début de chaque guide et indiquez l’étape partagée suivante.
- N’utilisez pas de noms de code internes (par exemple, « Agent d’intégration ») dans les documents destinés aux clients. Cette fonctionnalité n’est jamais exposée dans l’interface utilisateur du produit. Décrivez-le de manière générique (par exemple, « la plateforme ») et utilisez une copie exacte de l’interface utilisateur, telle que **[!UICONTROL Créer mon application automatiquement]** pour les contrôles.
- [!DNL Adobe LLM Apps] ne dépend pas de la plate-forme : son serveur MCP fonctionne avec n’importe quelle plate-forme LLM prise en charge, et pas seulement [!DNL ChatGPT]. Ne pas formuler d’affirmations générales ou indicatives comme si [!DNL ChatGPT] étiez la seule cible (par exemple, préférer « une plateforme LLM prise en charge telle que [!DNL ChatGPT] » à « ChatGPT » uniquement). Seul le nom [!DNL ChatGPT] explicitement dans le contenu qui est véritablement et actuellement spécifique au [!DNL ChatGPT] : le guide [Test dans le ChatGPT](/help/guides/test-in-chatgpt.md) dédié, ses liens directs/étapes de procédure et le contenu de référence ou de dépannage spécifique au [!DNL ChatGPT].
- Expliquez un concept technique lorsque l’utilisateur ou l’utilisatrice le rencontre pour la première fois ; liez-le à un concept plus approfondi ou à des documents de référence.
- Maintenez les tutoriels linéaires, les guides pratiques axés sur les tâches et les pages de référence factuelles.
- N’incluez que les informations dont le lecteur a besoin pour la tâche en cours ; préférez des phrases courtes et directes.
- Utilisez une application représentative sur l’ensemble du parcours.
- Distinguer le modèle automatique généré de l’intégration prête pour la production.
- Évitez les noms de programmes de travail internes, les champs de base de données, les tickets d’implémentation et les détails de pipelines instables.
- Ne dupliquez pas les tableaux de champs dans les guides ; liez-les à des références.
- Préservez le matériel et les directives d’Experience League : `[!DNL]`, &grave;&grave;, `[!IMPORTANT]`, `[!NOTE]` et `[!TIP]`.
- Utilisez des liens internes relatifs à la racine : `/help/...`.
- Utilisez la casse de phrase pour les titres et les en-têtes sauf si une étiquette de produit en exige autrement.
- Utilisez un texte secondaire d’image descriptive qui explique l’écran et le statut.

## Descriptif protégé approuvé par le PM

En `help/overview/overview.md`, ces sections sont approuvées par le responsable principal :

- **Que pouvez-vous faire avec les applications LLM**
- **Pourquoi les applications LLM sont importantes**

Conservez leurs en-têtes, puces, libellés, ordres et assertions verbatim.
Ne les raccourcissez pas, ne les réécrivez pas, ne les réorganisez pas et ne les supprimez pas dans la documentation générale
Mises à jour. Modifiez l’une des sections uniquement lorsque l’utilisateur la demande explicitement et
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

Pour des images nouvelles ou actualisées, suivez [screenshots.md](screenshots.md) et [screenshot-manifest.md](screenshot-manifest.md).

Le workflow par défaut utilise un pack de capture de production créé par l’utilisateur :

1. Recherchez des captures d’écran sous `docs-captures/<run-id>/` ou utilisez le dossier fourni par l’utilisateur.
2. Faites l’inventaire et inspectez visuellement chaque fichier PNG, JPEG et WebP ; ne vous fiez pas uniquement à son nom de fichier.
3. Faire correspondre les captures d’écran aux états de manifeste à l’aide du contenu visible de l’interface utilisateur.
4. Signalez les captures manquantes, en double, ambiguës, obsolètes ou risquées avant de modifier la documentation.
5. Conserver les captures source inchangées.
6. Créez des copies finales assainies, en utilisant les noms de fichier de manifeste stables sous `help/assets/`.
7. Mettez à jour le tutoriel et les guides associés pour qu’ils correspondent au workflow de production capturé.
8. Ajoutez un texte secondaire précis et exécutez la validation de la documentation.

La capture du navigateur guidée par l’agent reste une solution de secours facultative. Ne stockez pas l’état ou les informations d’identification du navigateur et n’exécutez pas de capture d’écran de mutation de production dans CI.

Lorsque vous êtes invité à « mettre à jour les documents à partir de captures d’écran » :

- Traiter le dossier de capture le plus récent explicitement sélectionné comme source.
- Demandez uniquement lorsque le flux d’application ou le mappage de capture d’écran est véritablement ambigu.
- Ne validez jamais les dossiers de capture bruts.
- Ne supprimez ou ne modifiez jamais les captures source sans approbation explicite.
- Si des informations sensibles ne peuvent pas être supprimées sans masquer la tâche, demandez une récupération en toute sécurité.

## Création d’une archive de révision

Générez un site HTML et une archive ZIP partageables hors ligne :

```bash
node .cursor/skills/llm-apps-docs/scripts/build_review_bundle.mjs
```

La version est écrite en regard du référentiel, et non à l’intérieur. Il comprend uniquement
articles publiés et ressources assainies, convertit les directives Experience League
pour une révision hors ligne et note que son style n’est pas l’expérience finale.
Rendu de League.

## Valider

Exécutez :

```bash
python3 .cursor/skills/llm-apps-docs/scripts/validate_docs.py
```

Corrigez tous les articles internes manquants, les ressources manquantes, les chemins d’accès relatifs à la racine non valides et les champs frontend manquants avant de les transmettre.

Vérifiez également :

- Les libellés et captures d’écran de l’interface utilisateur correspondent à Production.
- Les exemples d’URL de script et de widget s’accordent dans les guides et les références.
- Les commandes correspondent au standard actuel.
- Les nouvelles pages sont liées à partir de la table des matières.
- Le workflow de validation d’article Adobe réussit lorsqu’il est disponible.

## Références annexes

- [Modèle de contenu et terminologie](content-model.md)
- [Procédure de capture d’écran de production](screenshots.md)
- [Manifeste de capture d’écran](screenshot-manifest.md)
