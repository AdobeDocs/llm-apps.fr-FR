---
source-git-commit: a48b4022979d9a13b4fc3aa870c4ee6375e8bc13
workflow-type: tm+mt
source-wordcount: '1109'
ht-degree: 0%
---
# Procédure de capture d’écran pilotée par l’agent

Capturez vous-même l’interface utilisateur en cours d’exécution avec l’automatisation du navigateur. L’utilisateur fournit
l’authentification et approuve les modifications de la ressource ; l’agent navigue, capture,
assainit et intègre les images. Ne pas exiger un assemblage manuel
pack de captures d’écran avant de commencer.

## Choisir l’environnement

Utilisez l’URL fournie par l’utilisateur. Sinon, utilisez Production :

`https://experience.adobe.com/llm-apps`

Enregistrer la date de capture, l’environnement et l’état observé dans
[screenshot-manifest.md](screenshot-manifest.md). Une capture locale ou d’évaluation ne le fait pas
Vérifiez le comportement en production. Comparez l’interface utilisateur à l’implémentation correspondante, et
signalez les conflits plutôt que de présenter un flux non vérifié comme publié.

## Ouvrir et authentifier le navigateur

1. Vérifiez que l’interface utilisateur demandée est en cours d’exécution. Ne démarrez pas un serveur en double.
2. Utilisez l’automatisation de navigateur disponible. Réutiliser une session de navigateur uniquement lorsque son
la propriété et la portée de l’accès sont claires.
3. Si l’automatisation ne peut pas se joindre, ouvrez un navigateur distinct sous contrôle de l’agent.
Pour Chrome, utilisez un profil de session dédié en dehors du référentiel et liez
débogage à distance à `127.0.0.1` sur un port local inutilisé. Ne copiez jamais le de l’utilisateur
profil de navigateur personnel ou exposer le port de débogage au réseau.
4. Vérifiez que le navigateur est en cours d’exécution et que son point d’entrée d’automatisation répond.
5. Ouvrez l’application et mettez-la en pause pour que l’utilisateur s’authentifie. L’utilisateur gère Adobe,
GitHub et informations d’identification de plateforme LLM, MFA, clés de sécurité, captchas, organisation
et le consentement privilégié.
6. Reprenez après que l’utilisateur a confirmé l’authentification. Avec Playwright, attachez via
   `chromium.connect_over_cdp()` lors de l’utilisation d’une session de débogage Chrome contrôlée.
7. Localisez le cadre de l’application selon son origine ou son contenu visible. Unified Shell peut placer
l’application à l’intérieur d’un iframe ; ne supposons pas un index d’image fixe.

Ne jamais lire, imprimer, exporter ou valider des informations d’identification, des jetons, des cookies ou des navigateurs
stockage. N’appelez pas `storage_state()` et n’inspectez pas les en-têtes d’authentification. Laisser le
le navigateur gère la connexion sans extraire son état.

## Inspecter et naviguer en toute sécurité

- Privilégiez les localisateurs sémantiques tels que `get_by_role()` et `get_by_label()`. Inspecter
l’arborescence d’accessibilité et le texte visible avant de choisir un contrôle.
- Vérifiez les libellés exacts de l’interface utilisateur par rapport à l’écran affiché. La boîte de dialogue de création et
l’éditeur d’action enregistrée comporte différents onglets ; ne les déduisez pas les uns des autres.
- Réutilisez une application approuvée uniquement pour la documentation lorsque cela est possible. Utiliser un public,
site web non sensible et contenu d’installation reconnaissable.
- Remplissez les formulaires non enregistrés avec des valeurs d’exemple uniquement après avoir confirmé que le formulaire
n’enregistre pas automatiquement. Annuler ou ignorer ces modifications après la capture, puis vérifier
que les paramètres enregistrés restent inchangés.
- Demandez avant de créer des applications ou des référentiels, de commencer la génération d’actions,
la validation d’un référentiel pouvant recevoir des fichiers standard, l’enregistrement des paramètres,
ajout ou suppression de variables, fusion de demandes d’extraction, déploiement, suppression ou
archivage des ressources ou modification de l’accès au référentiel.
- Traiter **Marquer comme révisé** comme une mutation de référentiel, car il peut fusionner l’extraction
demandes. Ouvrir l’écran de révision n’autorise pas l’approbation.
- Limitez l’accès de l’application GitHub aux référentiels d’appareils. Demander
approbation de l’administrateur ou de l’administratrice si nécessaire.
- N’approuvez pas les actions ou ne déployez pas une application uniquement pour obtenir une capture d’écran facultative.
Capturez les états accessibles et signalez l’état manquant.

Lorsque de nouvelles ressources sont nécessaires, confirmez l’organisation cible, le site web,
option d&#39;infrastructure, programme AEM et nom de l&#39;appareil avant de les créer. Pour
Infrastructure gérée par Adobe, laissez la plateforme créer les référentiels. Pour le
Chemin d’accès à votre propre référentiel, utilisez deux référentiels vides et obtenez une approbation avant
la validation les initialise.

## Capture des états stables de l’interface utilisateur

1. Sélectionnez les points de contrôle appropriés dans le manifeste. Capturer les décisions et
résultats vérifiables, pas tous les clics.
2. Utilisez une fenêtre d’affichage de bureau qui s’adapte au panneau approprié, au zoom 100 % et au
thème de produit par défaut.
3. Attendez que le contenu, les polices, les menus et la disposition de boîte de dialogue attendus soient réglés.
Unified Shell redimensionne son iframe autour des boîtes de dialogue, de sorte qu’un en-tête visible seul
ne prouve pas que la zone de capture est stable.
4. Vérifiez que la zone cible reste au même emplacement et à la même taille. Attendre pour
le chargement des recouvrements, les animations de sortie, les toasts et les transitions désactivé-activé
à terminer sauf si l&#39;un de ces états fait l&#39;objet de l&#39;image.
5. Éloignez la sélection des entrées et fermez les menus indépendants dans le cas contraire
modifiez l’état documenté.
6. Capturez la plus petite zone géographique complète comprenant la tâche et les éléments utiles
contexte. Préférez une capture d’écran de panneau ou de boîte de dialogue à une image de bureau complète.
7. Avec Playwright, utilisez des captures d&#39;écran et des `animations='disabled'` de localisation, plus
masques de temps de capture pour les valeurs sensibles. Inspectez le fichier PNG obtenu ; le
La réussite de l’API de capture d’écran ne prouve pas que la région est correcte.

Ne modifiez jamais le DOM, ne redessinez jamais les étiquettes et ne modifiez jamais l&#39;apparence d&#39;un contrôle pour en faire un
la capture d’écran semble prête ou réussie.

## Stocker et nettoyer les images

Enregistrez chaque exécution dans le répertoire Git-ignore :

```text
docs-captures/<run-id>/
```

Utilisez des noms de fichier descriptifs tels que `setup-choice.png`, `action-basic-info.png`,
et `generated-action-review.png`. Conserver les captures source inchangées. Conserver brut
capture et parcourt les profils des validations et des archives de révision.

Créez les copies finales sous les répertoires de sortie dans le manifeste :

- Supprimez le Chrome du navigateur, la navigation de compte, les applications non liées et la zone de travail vide.
- Excluez entièrement les secrets de la capture, y compris les captures brutes.
- Masquer les noms personnels, avatars, identifiants d&#39;organisation et d&#39;installation, privé
détails du référentiel, espaces de noms d’exécution et URL complètes du serveur MCP. Préférer un solide
masques neutres à brouiller, ce qui peut laisser des valeurs récupérables.
- Utilisez des valeurs sûres telles que `https://auth.example.com`, `catalog:read`,
  `GREETING_PREFIX` et `Good day` dans des exemples de formulaires non enregistrés.
- Exclure les coordonnées générées et d’autres données d’exemple d’aspect personnel lorsque
ce contenu n’est pas nécessaire pour l’étape .
- Conserver l’état réel de l’interface utilisateur. Recadrer ou supprimer du contenu sensible, et non des erreurs
avertissements ou fonctionnalités manquantes que l’article doit expliquer.
- Réutiliser des noms de fichiers publics stables lors de l’actualisation d’un point de contrôle existant

Ne supprimez pas les captures source, les profils de navigateur ou les ressources d’appareil sans
approbation. Ne supprimez pas un profil de navigateur actif lorsque son navigateur est en cours d’exécution.

## Révision et intégration

Pour chaque image finale :

1. Ouvrez l’image et examinez-la visuellement.
2. Vérifiez que le panneau prévu, les libellés, l’état et les commandes pertinentes sont visibles.
3. Recherche de texte tronqué, de zones vides ou incorrectes, de superpositions de chargement, de transitoires
menus et détails du compte.
4. Vérifiez que l’article décrit le workflow observé et que le texte secondaire
identifie l’écran et l’état.
5. Mettez à jour le manifeste avec l’environnement de capture, la date et les limites sur .
vérification.
6. Exécutez le programme de validation de la documentation et créez le site de révision à l’aide des commandes
en `SKILL.md`.
7. Inspectez l’article rendu et vérifiez que chaque image se charge à un emplacement lisible.
taille.

Pour les plateformes tierces, utilisez un navigateur authentifié uniquement dans le fichier de l’utilisateur
étendue approuvée. Si l’accès n’est pas disponible, vérifiez les instructions par rapport au
documentation officielle du fournisseur, omettre les captures d’écran de configuration non vérifiées et indiquer
que ces écrans n&#39;ont pas été capturés. Ne pas présenter d’ancienne configuration tierce
image comme actuelle.

Si l’utilisateur fournit des images, inspectez et vérifiez-les avant de les réutiliser. Ils
sont des matières premières supplémentaires, et ne sont pas une condition préalable au processus de capture.

## Bloqueurs de rapports

Mettre en pause pour l’authentification, le consentement ou une modification de ressource non approuvée. Rapport
échecs d’automatisation, états non disponibles, captures non sécurisées, validation du référentiel
erreurs, échecs de génération et incohérences entre l’interface utilisateur et la source. Continuer avec
travaux vérifiés, non mutants dans la mesure du possible ; ne jamais inventer un état réussi ou
remplacez silencieusement les preuves manquantes par une image obsolète.
