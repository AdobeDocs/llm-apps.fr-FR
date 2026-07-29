---
source-git-commit: bb3d8a02f22a91ceeeba5999453aeb4221060f80
workflow-type: tm+mt
source-wordcount: '696'
ht-degree: 0%

---
# Procédure de capture d’écran de production

Utilisez cette procédure pour capturer de vraies captures d’écran de documentation publique à partir de :

`https://experience.adobe.com/#/@llmapps/llm-apps/`

Le workflow préféré est la capture humaine suivie de la prise assistée par un agent. L’utilisateur ou l’utilisatrice décide des états de production pertinents ; la compétence les organise, les assainit et les intègre dans la documentation.

## Boîte de réception de capture

Placez chaque exécution de capture dans :

```text
docs-captures/<YYYY-MM-DD>/
```

Le répertoire est ignoré par Git. Les captures d’écran brutes doivent rester locales et ne doivent jamais être validées.

L’utilisateur peut utiliser n’importe quel nom de fichier, mais les noms triés facilitent la révision :

```text
01-create-app.png
02-connect-github.png
03-onboarding-enabled.png
04-generating-actions.png
05-review-actions.png
```

Une `capture-notes.md` facultative peut décrire des états manquants, un comportement inhabituel ou l’ordre prévu.

## Limites de sécurité

- L’utilisateur saisit les informations d’identification Adobe, GitHub et LLM-platform directement dans le navigateur.
- Arrêtez pour l’AMF, les clés de sécurité, les captchas, la sélection d’organisation et le consentement privilégié.
- Ne lisez, n’imprimez, n’enregistrez ou ne validez jamais de jetons, de cookies, d’espace de stockage du navigateur ou d’informations d’identification.
- Utilisez un site web public non sensible et de nouveaux référentiels de documentation uniquement.
- Accordez aux applications GitHub l’accès uniquement aux deux référentiels utilisés par la structure.
- Demandez avant de créer, déployer, supprimer, archiver ou modifier l’accès au référentiel.
- Ne capturez pas d’informations personnelles, d’ID d’organisation, d’ID d’installation de référentiel, de jetons ou d’URL d’exécution complète.

## Dénomination de l’appareil

Utilisez des noms qui identifient clairement les ressources de documentation jetables :

```text
App: LLM Apps Docs <YYYY-MM-DD>
Handler repo: llm-apps-docs-<YYYYMMDD>
EDS repo: llm-apps-docs-<YYYYMMDD>-eds
```

Avant de créer quoi que ce soit, confirmez l’organisation Adobe cible, le propriétaire GitHub, le site web public et les noms des ablocages avec l’utilisateur.

Créez des référentiels vides et privés. Ne les initialisez pas avec un fichier README, une licence ou une `.gitignore`.

## Paramètres de capture

- Utilisez une fenêtre d’affichage de bureau suffisamment grande pour afficher les boîtes de dialogue complètes sans Chrome du navigateur.
- Maintenez le zoom à 100 %.
- Utilisez le thème de produit par défaut, sauf si l’article enseigne spécifiquement des thèmes.
- Capturez la plus petite région complète contenant la tâche et le contexte nécessaire.
- Évitez les curseurs, les menus ouverts sans rapport avec l’étape, les toasts des actions précédentes et les éléments transitoires, sauf si l’élément rotatif est l’état documenté.
- Utilisez PNG.
- Conservez la stabilité des noms de fichier ; remplacez le contenu de l’image au lieu de renommer les fichiers lors des actualisations.

## Séquence de capture recommandée

L’utilisateur doit capturer les états pertinents du manifeste, notamment :

1. Créez une application avant la connexion de GitHub.
2. Sélection de l’accès au référentiel de l’application GitHub.
3. **Créer automatiquement mon application** activé avec les deux référentiels sélectionnés.
4. Création d’application ou démarrage automatique de la génération d’application.
5. Actions en cours de génération.
6. Actions générées prêtes pour la révision.
7. Métadonnées, gestionnaire et widget d’une action représentative.
8. Révision par action et état de toutes les actions examinées.
9. Déploiement intermédiaire réussi.
10. L’enregistrement de l’application et un représentant génèrent la plateforme LLM.

Capturez d’autres écrans lorsqu’ils expliquent une décision, une erreur ou une condition préalable réelle. Ne capturez pas chaque clic.

## Workflow d’entrée de compétences

Lorsque l’utilisateur demande à mettre à jour la documentation d’un dossier de capture :

1. Confirmez le répertoire de capture exact.
2. Répertoriez tous les fichiers PNG, JPEG et WebP et examinez visuellement chaque image.
3. Créez un mappage des fichiers sources aux entrées dans `screenshot-manifest.md`.
4. Comparez les libellés et la séquence d’interface utilisateur visibles avec le tutoriel existant.
5. Rapport :
   - les états requis manquants ;
   - les images en double ou redondantes ;
   - un ordre ambigu ;
   - les captures d’écran obsolètes ;
   - les informations sensibles ;
   - Comportement de production incompatible avec la documentation.
6. Ne modifiez pas les captures source.
7. Pour chaque image acceptée, créez une copie assainie avec le nom de fichier de manifeste stable sous `help/assets/guide-onboarding-agent/`.
8. Recadrer uniquement lorsque l’interface utilisateur qui l’entoure n’ajoute aucun contexte utile.
9. Masquez les valeurs sensibles. Si le port du masque n&#39;est pas possible, demandez une récupération.
10. Mettez à jour l’article et le texte secondaire pour qu’ils correspondent au workflow capturé.
11. Exécutez la validation du lien et de la ressource.
12. Laissez le dossier de capture en place jusqu’à ce que l’utilisateur demande explicitement de le supprimer.

## Capture guidée par l’agent en option

Si l’utilisateur demande à l’agent de piloter le navigateur, utilisez le même manifeste et les mêmes limites de sécurité. Mettre en pause pour l’authentification, le consentement privilégié, les modifications du référentiel, la création, le déploiement et le nettoyage de l’application. N’exécutez jamais ce flux de production par mutation sans assistance.

## Révision d’image

Pour chaque image :

- Associez-le à une entrée de manifeste.
- Vérifiez la copie de l’interface utilisateur par rapport à l’article.
- Supprimez la navigation de compte lorsque cela n’est pas nécessaire.
- Masquez les noms personnels, les avatars, les identifiants d’organisation, les identifiants d’installation du référentiel, les espaces de noms d’exécution et les applications non liées.
- Vérifiez qu’aucun détail de remplissage automatique, d’e-mail, de jeton d’accès ou de référentiel privé du navigateur n’est visible.
- Écrire un texte secondaire qui identifie à la fois l’écran et le statut.

## Quand arrêter

Arrêter et signaler un bloqueur lorsque :

- La production ne correspond pas au workflow en cours de documentation.
- Le flux de révision diffère considérablement de la documentation publiée.
- La validation du référentiel rejette le flux de référentiel vide prévu.
- Le pipeline d’intégration échoue.
- Une action privilégiée requiert un utilisateur ou une utilisatrice, ou un administrateur ou une administratrice.
- Une capture d’écran ne peut pas être sécurisée sans masquer les informations essentielles à l’étape.
