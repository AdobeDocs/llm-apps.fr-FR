---
title: Référence d’authentification
description: Définitions de champ, exigences de jeton, points d’entrée de découverte, API de gestionnaire et comportement de plateforme LLM pour l’authentification des utilisateurs finaux dans les applications Adobe LLM.
source-git-commit: 0f9dd0a5b1008e183eb8ab5a4c2eae34a78923bb
workflow-type: tm+mt
source-wordcount: '1428'
ht-degree: 2%
---

# Référence d’authentification {#authentication-reference}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Utilisez cette page pour rechercher des champs et des contrats d’authentification. Pour obtenir des instructions de configuration, voir [Authentifier les utilisateurs finaux avec votre propre fournisseur d’identité](/help/guides/authentication.md).

## Paramètres d’authentification {#authentication-settings}

Ouvrez **[!UICONTROL Paramètres]** > **[!UICONTROL Authentification]**. Chaque environnement stocke ses propres paramètres. Utilisez le sélecteur **** pour sélectionner l’environnement à modifier ; l’enregistrement des paramètres n’affecte jamais l’autre environnement.

| Champ | Requis | Description |
|-------|----------|-------------|
| **[!UICONTROL Workspace.]** | — | Environnement pour les paramètres d’authentification : **[!UICONTROL Évaluation]** ou **[!UICONTROL Production]** |
| **[!UICONTROL Activer l’authentification]** | — | Commutateur de Principal. Lorsqu’elle est désactivée, chaque action est publique, quel que soit son mode d’authentification |
| **[!UICONTROL Émetteur]** | Oui | URL de l’émetteur de votre fournisseur d’identité et réclamation `iss` attendue. Doit être HTTPS. Également publié comme serveur d’autorisation de cette application. Un fournisseur d’identité par application |
| **[!UICONTROL Portées prises en charge]** | Non | Ensemble complet d’étendues dont les actions de cette application peuvent avoir besoin. Publié sur les plateformes LLM en tant que portées prises en charge par l’application |
| **[!UICONTROL URI JWKS]** | Non | Avancé. URL HTTPS de votre jeu de clés de signature. Nécessaire uniquement lorsqu’il diffère de ce que les métadonnées de votre serveur d’autorisation annoncent |

### Les règles de validation

| Règle | Effet |
|------|--------|
| **[!UICONTROL Émetteur]** est vide lorsque **[!UICONTROL Activer l&#39;authentification]** est activé | L’enregistrement est bloqué. |
| **[!UICONTROL Émetteur]** ou **[!UICONTROL URI JWKS]** n’est pas une URL HTTPS | L’enregistrement est bloqué. |
| Une action nécessite une portée manquante dans **[!UICONTROL Portées prises en charge]** | L’enregistrement est bloqué jusqu’à ce que vous ajoutiez la portée ou la supprimiez de l’action |
| Une portée est supprimée de **[!UICONTROL Portées prises en charge]** | Il est supprimé de chaque action qui le nécessitait, immédiatement, sans attendre un enregistrement |
| **[!UICONTROL Portées prises en charge]** est vide | Aucune portée ne peut être accordée. Toute portée déjà associée à une action est donc supprimée. Aucun avertissement ne s&#39;affiche dans ce cas |
| `offline_access` est répertorié dans **[!UICONTROL Portées prises en charge]** ou sur une action | Supprimé lorsque l’application est déployée, quelle que soit la casse ou l’espace blanc environnant, de sorte que la page de paramètres peut afficher une étendue que l’application déployée n’a pas. `offline_access` demande un jeton d’actualisation à votre serveur d’autorisation plutôt que d’accorder l’accès à cette application. Il ne s’agit donc pas d’une portée que cette application annonce. Vous n’avez pas besoin de le répertorier : la plateforme LLM le demande directement à votre serveur d’autorisation |

Les barres obliques de fin sur **[!UICONTROL Émetteur]** sont normalisées. La comparaison des `iss` tolère la différence, de sorte que les jetons d’un fournisseur qui inclut toujours une barre oblique restent valides.

## Modes d’authentification {#auth-modes}

Définissez par action sous **[!UICONTROL Configuration par action]**.

| Mode | Jeton obligatoire | Le gestionnaire reçoit l’identité | Annoncé sur la plateforme en tant que |
|------|----------------|---------------------------|-------------------------------|
| **[!UICONTROL Aucune]** | Non | Uniquement lorsque l’appelant fournit un jeton valide | `noauth` |
| **[!UICONTROL Requis]** | Oui, avec chaque portée répertoriée | Toujours | `oauth2` |
| **[!UICONTROL facultatif]** | Non | Lorsqu’un jeton valide est présent | `noauth` et `oauth2` |

Un gestionnaire d’action **[!UICONTROL Obligatoire]** ne s’exécute jamais sans jeton valide et de portée correcte. Un gestionnaire d’action **[!UICONTROL facultatif]** s’exécute toujours et peut demander à se connecter lui-même avec `extra.challengeAuth()`.

**[!UICONTROL Obligatoire]** est donc le seul mode qui rejette les appelants non authentifiés. Une application rejette tous les appels anonymes uniquement lorsque chaque action est **[!UICONTROL obligatoire]**. Si une action est **[!UICONTROL Aucune]** ou **[!UICONTROL Facultative]**, l’application utilise l’authentification mixte, car au moins une action accepte les appels anonymes.

Les modes d’authentification prennent effet uniquement lorsque l’option **[!UICONTROL Activer l’authentification]** est activée. Les modifications prennent effet lors du prochain déploiement de l’application.

La modification du commutateur d’authentification réécrit également les modes par action :

| Changer de modification | Effet sur les modes par action |
|---------------|----------------------------|
| Désactivé à activé | Chaque action **[!UICONTROL Aucune]** devient **[!UICONTROL Obligatoire]**. Les actions déjà **[!UICONTROL Obligatoires]** ou **[!UICONTROL Facultatives]** conservent leur mode |
| Activé/désactivé | Le mode et les portées de chaque action sont effacés pour cet environnement. La configuration n’est pas restaurée si vous rallumez le commutateur |

La commutation de **** ne réécrit jamais les modes. Le sélecteur charge la configuration enregistrée de l’autre environnement sans la modifier.

Avec l’option **[!UICONTROL Activer l’authentification]** activée et chaque action définie sur **[!UICONTROL Aucune]**, l’application ne nécessite pas d’authentification, mais publie toujours son serveur d’autorisation pour la découverte. Pour rendre l’application entièrement publique, désactivez l’interrupteur.

Une application peut utiliser différents modes d’authentification pour des actions individuelles. Voir [Comportement de plateforme LLM](/help/reference/authentication-reference.md#platform-behavior) pour savoir comment chaque plateforme les applique.

## Exigences de jeton {#token-requirements}

Votre fournisseur d’identité doit émettre des jetons d’accès qui répondent à toutes les exigences suivantes. L’exécution traite un jeton pour lequel aucun contrôle n’a échoué comme absent : l’appelant n’est pas authentifié et une action **[!UICONTROL Obligatoire]** demande à l’appelant de se connecter.

| Condition requise | Détails |
|-------------|--------|
| Format | JWT signé. Les jetons opaques ne sont pas pris en charge |
| Algorithme de signature | `RS256`, `RS384`, `RS512`, `ES256`, `ES384`, `ES512`, `PS256`, `PS384` ou `PS512`. Les algorithmes HMAC tels que `HS256` sont rejetés |
| `iss` | Doit correspondre **[!UICONTROL Émetteur]** |
| `aud` | Doit contenir l’identifiant de ressource de l’application, c’est-à-dire son URL de serveur MCP pour cet environnement. |
| `exp` | Doit être dans le futur |
| `scope` ou `scp` | Le runtime lit une chaîne de `scope` délimitée par des espaces ou retourne à un tableau de chaînes dans `scp`. Utilisez l&#39;une de ces formes pour les portées vérifiées par rapport aux exigences de chaque action |
| `sub` | Identifiant utilisateur que le gestionnaire lit `getAuthenticatedUser` |
| Transfert | En-tête de requête `Authorization: Bearer <token>` |

Toutes les réclamations forfaitaires supplémentaires incluses par votre fournisseur (par exemple, `tenant` ou `email`) sont transmises à votre gestionnaire. Les objets imbriqués sont supprimés et les valeurs de chaîne longues sont tronquées.

## Découverte de fournisseur d’identité {#discovery}

Votre application publie ses propres métadonnées de ressources protégées [RFC 9728](https://datatracker.ietf.org/doc/html/rfc9728) afin que les plateformes LLM puissent localiser votre serveur d’autorisation. Vous n’avez pas besoin de créer, d’héberger ou de configurer les métadonnées de la ressource protégée.

Votre fournisseur d’identité doit fournir les informations de découverte suivantes :

| Condition requise | Détails |
|-------------|--------|
| Métadonnées du serveur d’autorisation | Votre serveur d’autorisation doit publier des métadonnées de découverte pour les clients LLM. À moins que l’**[!UICONTROL URI JWKS]** ne soit défini, le runtime de cette application recherche spécifiquement les métadonnées du serveur d’autorisation [RFC 8414](https://datatracker.ietf.org/doc/html/rfc8414) OAuth pour localiser les `jwks_uri` ; il ne retourne pas au chemin de découverte OIDC |
| Émetteur avec un chemin | Le segment bien connu se situe avant le chemin d’accès, et non après. Un émetteur sur `https://auth.example.com/oauth2/default` diffuse ses métadonnées sur `https://auth.example.com/.well-known/oauth-authorization-server/oauth2/default` |
| URL de clé de signature directe | Définissez **[!UICONTROL URI JWKS]** lorsque les clés sont hébergées ailleurs ou que le fournisseur expose uniquement la découverte OIDC. Cela contourne la découverte d’exécution des clés, et non la découverte requise par les clients LLM |

## API d’authentification du gestionnaire {#handler-auth-api}

Le package `@adobe/llm-apps-runtime` exporte les assistants suivants. Chaque helper accepte `extra`, le deuxième argument que votre gestionnaire reçoit.

| Helper | Renvoie |
|--------|---------|
| `getAuthenticatedUser(extra)` | Demande de `sub` de l’utilisateur connecté ou `undefined` lorsque l’appel n’est pas authentifié |
| `hasScope(extra, scope)` | `true` lorsque le jeton de l’appelant porte des `scope` |

Les informations du jeton vérifié sont disponibles dans `extra.authInfo`, qui est `undefined` pour un appel non authentifié.

| Propriété | Description |
|----------|-------------|
| `authInfo.token` | Jeton du porteur brut. Ne le consignez pas et ne le renvoyez pas au client |
| `authInfo.clientId` | La revendication `client_id` ou `azp`, ou `unknown` |
| `authInfo.scopes` | Tableau des étendues accordées |
| `authInfo.expiresAt` | Expiration du jeton, comme l’affirme la `exp` |
| `authInfo.resource` | Identifiant de ressource de l’application en fonction duquel le jeton a été validé |
| `authInfo.extra` | `sub` plus toutes les autres réclamations forfaitaires incluses par votre fournisseur d’identité |

`extra.challengeAuth(options)` est disponible uniquement sur les actions **[!UICONTROL facultatives]**. Renvoyez son résultat à partir de votre gestionnaire pour demander à l’utilisateur de se connecter au lieu de renvoyer du contenu.

L’assistant renvoie un résultat de l’outil comme défi pour [!DNL ChatGPT]. [!DNL Claude] nécessite un défi HTTP au niveau du transport. Le runtime fournit ce défi pour les actions **[!UICONTROL Obligatoires]**, mais pas par le biais de l’assistant optional-handler.

| Option | Description |
|--------|-------------|
| `error` | Code d’erreur du porteur [RFC 6750](https://datatracker.ietf.org/doc/html/rfc6750) : `invalid_token`, `insufficient_scope` ou `invalid_request`. La valeur par défaut est `insufficient_scope`. |
| `errorDescription` | Message affiché à l’utilisateur. La valeur par défaut est une invite de connexion générique |
| `scope` | Portées délimitées par des espaces à demander. Omettez-le pour laisser la plateforme revenir aux portées prises en charge par l’application |

>[!IMPORTANT]
>
>Définissez toujours le `error` explicitement. Utilisez `invalid_token` pour un appelant sans session valide et `insufficient_scope` uniquement pour un appelant dont le jeton est valide mais n’a pas de portée requise. Le runtime transmet la valeur à la plateforme LLM, qui choisit le libellé de l’invite de connexion. Envoyez le code qui décrit précisément la condition plutôt que le code avec l’invite de votre choix.

## Comportement de la plateforme LLM {#platform-behavior}

Les deux plateformes prennent en charge les actions publiques et protégées dans la même application, mais leurs déclencheurs de connexion diffèrent.

| Comportement | [!DNL ChatGPT] | [!DNL Claude] |
|----------|----------------|---------------|
| Authentification mixte | Les actions publiques fonctionnent de manière anonyme ; les actions **[!UICONTROL Obligatoires]** demandent à se connecter | Avec **[!UICONTROL Se connecter si nécessaire]**, les actions publiques fonctionnent de manière anonyme et un appel protégé démarre la connexion |
| Application avec chaque action **[!UICONTROL obligatoire]** | Configurer l’authentification OAuth | Nécessite une authentification avant utilisation |
| Configuration du connecteur | Utilisez **[!UICONTROL Aucune authentification]** pour une application publique, **[!UICONTROL OAuth]** lorsque chaque action est **[!UICONTROL Requise]** et **[!UICONTROL Mixte]** dans les autres cas, où ces choix sont proposés | Vérifiez les paramètres détectés. Choisissez **[!UICONTROL Se connecter si nécessaire]** pour les applications mixtes ou **[!UICONTROL Se connecter maintenant]** pour la connexion initiale |
| Identité manquante ou étendue requise | L’exécution renvoie le défi d’authentification de la plateforme | L’exécution renvoie le HTTP 401 en cas d’identité manquante/non valide ou le HTTP 403 avec un défi de portée insuffisante pour un jeton valide sans portée obligatoire |
| `extra.challengeAuth()` du gestionnaire facultatif | Peut démarrer la réauthentification | Une réponse HTTP avec résultat de l&#39;outil réussie ne démarre pas la connexion. Utiliser une action **[!UICONTROL obligatoire]** pour un point de connexion portable |

Voir [Authentification différée Claude](https://claude.com/docs/connectors/building/lazy-authentication) et [Authentification ChatGPT](https://developers.openai.com/plugins/build/auth) pour connaître le comportement actuel du client. L’interface utilisateur et la politique du connecteur peuvent varier en fonction du compte et de l’organisation.


## En relation

- [Authentifier les utilisateurs finaux avec votre propre fournisseur d’identité](/help/guides/authentication.md)
- [Champs d’action et de widget](/help/reference/reference-docs.md)
- [Résolution des problèmes](/help/reference/troubleshooting.md#authentication)
