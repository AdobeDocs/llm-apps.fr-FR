---
source-git-commit: 41bd4b6239171c7a3af7dc6349eaa3cbb880449c
workflow-type: tm+mt
source-wordcount: '1279'
ht-degree: 0%
---
# Manifeste de capture d’écran

Boîte de réception de capture : `docs-captures/<YYYY-MM-DD>/`

Capturez uniquement les points de contrôle qui aident matériellement l’utilisateur à prendre une décision ou à vérifier l’état.

Les noms de fichier Source ne doivent pas nécessairement correspondre aux noms de fichier finaux. Les compétences font des captures d’écran par état visible de l’interface utilisateur, préservent les fichiers bruts et créent des copies assainies à l’aide des noms ci-dessous.

Chaque guide ci-dessous déclare son propre répertoire de sortie. Utilisez celui de la section à laquelle appartient la capture.

&#x200B;# Guide d’intégration

Répertoire de sortie : `help/assets/guide-onboarding-agent/`

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
- Texte de remplacement : `Actions — generating recommendations`

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
- Inclure : **Ajouter <plugin> vers ChatGPT &#x200B;** et **&#x200B; Connect &#x200B;**.
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

&#x200B;# Guide d’authentification

Répertoire de sortie : `help/assets/guide-authentication/`

Référencé par [authentication.md](../../../help/guides/authentication.md).

L’étape **[!UICONTROL Copier l’identifiant de la ressource]** réutilise l’du guide d’intégration
`app-mcp-url.png`. Ne le capturez plus.

Chaque capture de cette section affiche la configuration de sécurité. Masque avant enregistrement :

- L’URL **[!UICONTROL Émetteur]** et tout nom d’hôte qui identifie le fournisseur d’identité ou son fournisseur.
- L’URL complète du serveur MCP, où elle apparaît.
- Identifiants client, client et organisation.
- Nom du compte, avatar et adresse électronique.

Utilisez des valeurs d’espace réservé neutres où un champ doit rester lisible (par exemple, un émetteur de
`https://auth.example.com`. Les noms de portée doivent être lus comme des exemples génériques, tels que `orders:read`.

## Captures requises

### `auth-core-settings.png`

- État : **[!UICONTROL Paramètres]** > **[!UICONTROL Authentification]** avec **[!UICONTROL Activer l’authentification]** activé et **[!UICONTROL Paramètres principaux]** renseigné.
- Inclure : le sélecteur **&#x200B;**&#x200B;affichant **[!UICONTROL Phase]**, **[!UICONTROL Activer l’authentification]** dans son propre état, **[!UICONTROL Émetteur]** et **[!UICONTROL Portées prises en charge]** contenant au moins deux portées.
- Insérez le contrôle réduit **[!UICONTROL Paramètres avancés]** afin que le lecteur puisse voir que **[!UICONTROL URI JWKS]** est facultatif et qu’il se trouve à cet emplacement.
- Masque : nom d’hôte de l’émetteur.
- Texte de remplacement : `Authentication — enable authentication and complete the core settings`

Capturé le 25 août 2026. Recadré pour déposer la zone de travail vide ; aucun masquage n’est nécessaire, car
**[!UICONTROL Émetteur]** a été défini sur `https://auth.example.com` dans le produit avant le
capture. Préférez-le à la modification de l’image par la suite. **[!UICONTROL Portées prises en charge]** contient
une portée (`read:all`) ; deux illustreraient mieux le champ, mais cela ne vaut pas la peine
se refaire une place par lui-même.

### `auth-per-action.png`

- État : **[!UICONTROL configuration par action]** après l’activation de l’authentification, avec les modes délibérément mixtes.
- Inclure : au moins trois actions, une par mode — **[!UICONTROL Aucune]**, **[!UICONTROL Obligatoire]** et **[!UICONTROL Facultatif]** — et la colonne **[!UICONTROL Portées]** renseignée sur les points de contrôle.
- Inclure : **[!UICONTROL exiger une authentification sur toutes les actions]**, idéalement dans son état indéterminé, ce qui est ce qu’une configuration mixte produit.
- Utilisez uniquement des noms d&#39;actions d&#39;installation.
- Texte de remplacement : `Authentication — set an auth mode and scopes for each action`

Capturé le 25 août 2026. Recadré uniquement, rien à masquer. Affiche les trois modes, une valeur renseignée
**[!UICONTROL Portées]** cellule et **[!UICONTROL Exiger une authentification sur toutes les actions]** dans son
état indéterminé, avec `Test Action 1/2/3` comme noms d&#39;élément.

Recadrez **à l’intérieur** la bordure du conteneur du panneau des paramètres : une règle de 1px pleine hauteur se trouve à chaque niveau
côté de la capture, et laisser l&#39;un ou l&#39;autre dans frame se lit comme une ligne perdue le long du bord de la capture
image.

L’avertissement du produit concernant l’application d’[!DNL Claude] authentification par connecteur était le suivant :
**non observé sur cet onglet lors de deux rondes de capture**, il n’est donc pas obligatoire ici. Le
Le guide indique plutôt ce comportement en prose. Si l’avertissement existe dans une version ultérieure,
capturez-le en tant que `auth-claude-warning.png` et ajoutez une entrée .

### `chatgpt-authentication-mode.png`

- État : la boîte de dialogue **[!UICONTROL Nouveau module externe]** avec le menu déroulant **[!UICONTROL Authentification]** s’ouvre.
- Inclure : les trois valeurs — **[!UICONTROL Aucune authentification]**, **[!UICONTROL Mixte]** et **[!UICONTROL OAuth]** — de sorte que le tableau de mappage du guide puisse être vérifié par rapport au contrôle réel.
- Masque : l’URL du serveur MCP et tout identifiant de connecteur figurant dans l’URL du navigateur.
- Texte de remplacement : `ChatGPT — select the authentication mode for the plugin`

Encadrez-le de la même manière que le `chatgpt-new-plugin.png` du guide d’intégration : la carte de dialogue avec .
une marge de la page toujours visible autour, environ 40px à gauche et en haut. Ne pas recadrer le vidage vers
la carte.

Capturé le 25 août 2026, en mode clair, pour correspondre à toutes les autres captures dans la documentation. Le
La liste déroulante occulte le champ **[!UICONTROL URL du serveur]**, de sorte que l’URL MCP n’est pas lisible, mais
son matériau translucide laisse passer une image floue du contenu de ce champ à côté du
options. Les trois lignes non mises en surbrillance ont été recouvertes avec le remplissage du panneau et leurs étiquettes
rendu à nouveau, ce qui le supprime. Vérifier par prélèvement, et non par œil : le saignement est suffisamment faible pour
manquant et il s’agit de l’URL du serveur MCP.

Notez que le contrôle en direct offre **quatre** valeurs — **[!UICONTROL OAuth]**, **Access
jeton/clé API&rbrack;**, &#x200B;** [!UICONTROL Aucune authentification] **&#x200B; et &#x200B;** [!UICONTROL Mixte]**. Mappage du guide
le tableau couvre uniquement les trois vers lesquels les modes d’authentification d’une application peuvent mapper, ce qui est correct, mais pas
décrivez la liste déroulante comme ayant trois options.

## Captures facultatives

Ajouter seulement si la prose s&#39;avère insuffisante :

- `auth-scope-blocked.png` — **[!UICONTROL Enregistrement]** bloqué car une action nécessite une portée manquante dans **[!UICONTROL Portées prises en charge]**. Utile pour l’entrée de dépannage.
- L’invite de connexion en milieu de conversation déclenche une action **[!UICONTROL Facultatif]**. Interface utilisateur de Platform qui change souvent et qui est déjà décrite en prose.

Ne capturez pas la propre page de connexion du fournisseur d’identité. Il identifie le fournisseur, que cette documentation ne nomme pas.

&#x200B;# Guide des variables d’application

Répertoire de sortie : `help/assets/guide-app-variables/`

Référencé par [app-variables.md](../../../help/guides/app-variables.md).

Utilisez la variable d&#39;`GREETING_PREFIX` avec la valeur `Good day`, dans l&#39;espace de travail **[!UICONTROL Stage]**. Les valeurs de variable sont visibles dans le tableau, ne capturez donc jamais un paramètre réel.

## Captures requises

### `variables-empty.png`

- État : **[!UICONTROL Paramètres]** > **[!UICONTROL Variables et secrets]** sans variable dans **[!UICONTROL Étape]**.
- Inclure : la navigation des paramètres, le sélecteur **&#x200B;**&#x200B;et **[!UICONTROL Ajouter]**.
- Texte de remplacement : `Variables & Secrets — empty Stage workspace with the Add button`

Capturé 2026-10-05. Recadré pour déposer la zone de travail vide ; rien à masquer.

### `add-variable-dialog.png`

- État : boîte de dialogue **[!UICONTROL Ajouter une variable ou un secret]** renseignée, avant d’enregistrer.
- Inclure : les *Secrets ne sont pas encore pris en charge* remarque, **[!UICONTROL Nom]** `GREETING_PREFIX`, **[!UICONTROL Type]** **[!UICONTROL Variable]** et **[!UICONTROL Valeur]** `Good day`.
- Texte de remplacement : `Add Variable or Secret — GREETING_PREFIX set to Good day`

Capturé 2026-10-05. Recadré sous la boîte de dialogue ; rien à masquer.

### `variable-added.png`

- État : le tableau des variables après l’enregistrement, avec une ligne `GREETING_PREFIX`.
- Inclure : **[!UICONTROL Nom]**, **[!UICONTROL Type]**, **[!UICONTROL Valeur]**, **[!UICONTROL Dernière mise à jour]** et les contrôles de copie, de modification et de suppression.
- Texte de remplacement : `Variables & Secrets — GREETING_PREFIX saved in the Stage workspace`

Capturé 2026-10-05. Recadré pour déposer la zone de travail vide ; rien à masquer.

### `update-variable-dialog.png`

- État : boîte de dialogue **[!UICONTROL Mettre à jour GREETING_PREFIX]** avec les `Howdy` **[!UICONTROL Valeur actuelle]** `Good day` et **[!UICONTROL Nouvelle valeur]**.
- Texte de remplacement : `Update GREETING_PREFIX — change the value from Good day to Howdy`

Capturé 2026-10-05. Recadrez le titre de la page tronquée et la superposition vide sous la boîte de dialogue ; le signe d’insertion de texte après `Howdy` a été peint. Rien à masquer.

### `delete-variable-dialog.png`

- État : **[!UICONTROL Supprimer le préfixe_SALUTATIONS ?]** boîte de dialogue de confirmation.
- Texte de remplacement : `Delete GREETING_PREFIX — confirm the permanent deletion`

Capturé 2026-10-05. Recadrez le recouvrement vide sous la boîte de dialogue ; rien à masquer.
