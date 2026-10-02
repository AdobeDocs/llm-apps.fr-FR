---
title: Référence d’authentification
description: Définitions de champ, exigences de jeton, points d’entrée de découverte, API de gestionnaire et comportement de plateforme LLM pour l’authentification des utilisateurs finaux dans les applications Adobe LLM.
source-git-commit: fd41dbcabc4db0cae766de19cb042d7c85d8b7aa
workflow-type: tm+mt
source-wordcount: '1260'
ht-degree: 3%
---

# Référence d’authentification {#authentication-reference}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Utilisez cette page pour rechercher des champs et des contrats d’authentification. Pour le parcours de configuration, voir [Authentifier les utilisateurs finaux avec votre propre fournisseur d’identité](/help/guides/authentication.md).

## Paramètres d’authentification {#authentication-settings}

Sous **[!UICONTROL Paramètres]** > **[!UICONTROL Authentification]**. Chaque champ est stocké par environnement : le sélecteur **** sélectionne celui que vous modifiez, et l’enregistrement n’affecte jamais l’autre.

| Champ | Requis | Description |
|-------|----------|-------------|
| **[!UICONTROL Workspace.]** | — | Environnement auquel ces paramètres s’appliquent : **[!UICONTROL Évaluation]** ou **[!UICONTROL Production]** |
| **[!UICONTROL Activer l’authentification]** | — | Commutateur de Principal. Lorsqu’elle est désactivée, chaque action est publique, quel que soit son mode d’authentification |
| **[!UICONTROL Émetteur]** | Oui | L’URL de l’émetteur de votre fournisseur d’identité et la réclamation `iss` attendue. Doit être HTTPS. Également publié comme serveur d’autorisation de cette application. Un fournisseur d’identité par application |
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

Les barres obliques de fin sur **[!UICONTROL Émetteur]** sont normalisées et la comparaison `iss` tolère la différence ; un fournisseur qui émet toujours une barre oblique de fin valide toujours.

## Modes d’authentification {#auth-modes}

Définissez par action sous **[!UICONTROL Configuration par action]**.

| Mode | Jeton obligatoire | Le gestionnaire reçoit l’identité | Annoncé sur la plateforme en tant que |
|------|----------------|---------------------------|-------------------------------|
| **[!UICONTROL Aucune]** | Non | Uniquement lorsque l’appelant fournit un jeton valide | `noauth` |
| **[!UICONTROL Requis]** | Oui, avec chaque portée répertoriée | Toujours | `oauth2` |
| **[!UICONTROL facultatif]** | Non | Lorsqu’un jeton valide est présent | `noauth` et `oauth2` |

Un gestionnaire d’action **[!UICONTROL Obligatoire]** ne s’exécute jamais sans jeton valide et de portée correcte. Un gestionnaire d’action **[!UICONTROL facultatif]** s’exécute toujours et peut demander à se connecter lui-même avec `extra.challengeAuth()`.

**[!UICONTROL Obligatoire]** est donc le seul mode qui refuse les appelants non authentifiés. Une application n’est entièrement fermée que lorsque chacune de ses actions est **[!UICONTROL obligatoire]** ; une seule action **[!UICONTROL aucune]** ou **[!UICONTROL facultative]** rend l’application mixte, car les appels anonymes réussissent toujours pour au moins une action.

Les modes d’authentification ne sont en vigueur que lorsque l’option **[!UICONTROL Activer l’authentification]** est activée. Les modifications prennent effet lors du prochain déploiement de l’application.

Basculer ce commutateur pour réécrire les modes par action :

| Changer de modification | Effet sur les modes par action |
|---------------|----------------------------|
| Désactivé à activé | Chaque action **[!UICONTROL Aucune]** devient **[!UICONTROL Obligatoire]**. Les actions déjà **[!UICONTROL Obligatoires]** ou **[!UICONTROL Facultatives]** conservent leur mode |
| Activé/désactivé | Le mode et les portées de chaque action sont effacés pour cet environnement. La configuration n’est pas restaurée si vous rallumez le commutateur |

La commutation de **** ne réécrit jamais les modes — elle charge la configuration enregistrée de l&#39;autre environnement en l&#39;état.

**[!UICONTROL Activer l’authentification]** activé avec chaque action définie sur **[!UICONTROL Aucune]** est une combinaison valide mais inerte : aucun appel n’est jamais refusé, mais l’application publie toujours son serveur d’autorisation pour la découverte. Désactivez cette option pour rendre l’application entièrement publique.

Les modes peuvent être mélangés librement dans une application. Voir [Comportement de plateforme LLM](/help/reference/authentication-reference.md#platform-behavior) pour savoir comment chaque plateforme les applique.

## Exigences de jeton {#token-requirements}

Votre fournisseur d’identité doit émettre des jetons d’accès qui répondent à toutes les conditions suivantes. Un jeton qui échoue à toute vérification est considéré comme absent — l’appelant n’est pas authentifié et une action **[!UICONTROL Obligatoire]** l’empêche de se connecter.

| Condition requise | Détails |
|-------------|--------|
| Format | JWT signé. Les jetons opaques ne sont pas pris en charge |
| Algorithme de signature | `RS256`, `RS384`, `RS512`, `ES256`, `ES384`, `ES512`, `PS256`, `PS384` ou `PS512`. Les algorithmes HMAC tels que `HS256` sont rejetés |
| `iss` | Doit correspondre **[!UICONTROL Émetteur]** |
| `aud` | Doit contenir l’identifiant de ressource de l’application, c’est-à-dire son URL de serveur MCP pour cet environnement. |
| `exp` | Doit être dans le futur |
| `scope` ou `scp` | Chaîne délimitée par des espaces ou tableau de chaînes. Fournit les portées vérifiées par rapport aux exigences de chaque action |
| `sub` | Identifiant utilisateur que le gestionnaire lit `getAuthenticatedUser` |
| Transfert | En-tête de requête `Authorization: Bearer <token>` |

Toutes les réclamations forfaitaires supplémentaires incluses par votre fournisseur (par exemple, `tenant` ou `email`) sont transmises à votre gestionnaire. Les objets imbriqués sont supprimés et les valeurs de chaîne longues sont tronquées.

## Découverte de fournisseur d’identité {#discovery}

Votre application publie ses propres métadonnées de ressources protégées [RFC 9728](https://datatracker.ietf.org/doc/html/rfc9728) afin que les plateformes LLM puissent localiser votre serveur d’autorisation. Vous ne créez, n’hébergez et ne configurez rien pour cela.

Vous devez fournir la découverte de votre propre côté :

| Condition requise | Détails |
|-------------|--------|
| Métadonnées du serveur d’autorisation | Votre émetteur doit diffuser ses propres métadonnées [RFC 8414](https://datatracker.ietf.org/doc/html/rfc8414), ou [!DNL OpenID Connect] Discovery, sur son chemin d&#39;accès `/.well-known/`. Votre application la lit pour localiser vos clés de signature |
| Émetteur avec un chemin | Le segment bien connu se situe avant le chemin d’accès, et non après. Un émetteur sur `https://auth.example.com/oauth2/default` diffuse ses métadonnées sur `https://auth.example.com/.well-known/oauth-authorization-server/oauth2/default` |
| Clés hébergées ailleurs | Définissez **[!UICONTROL URI JWKS]** lorsque vos clés de signature ne se trouvent pas à l’endroit où ces métadonnées les publient |

## API d’authentification du gestionnaire {#handler-auth-api}

Exporté à partir de `@adobe/llm-apps-runtime`. Chaque helper prend `extra`, le deuxième argument que votre gestionnaire reçoit.

| Helper | Renvoie |
|--------|---------|
| `getAuthenticatedUser(extra)` | Demande de `sub` de l’utilisateur connecté ou `undefined` lorsque l’appel n’est pas authentifié |
| `hasScope(extra, scope)` | `true` lorsque le jeton de l’appelant porte des `scope` |

Les informations de jeton brutes vérifiées sont en `extra.authInfo`, ce qui est `undefined` pour un appel non authentifié.

| Propriété | Description |
|----------|-------------|
| `authInfo.token` | Jeton du porteur brut. Ne le consignez pas et ne le renvoyez pas au client |
| `authInfo.clientId` | La revendication `client_id` ou `azp`, ou `unknown` |
| `authInfo.scopes` | Tableau des étendues accordées |
| `authInfo.expiresAt` | Expiration du jeton, comme l’affirme la `exp` |
| `authInfo.resource` | Identifiant de ressource de l’application en fonction duquel le jeton a été validé |
| `authInfo.extra` | `sub` plus toutes les autres réclamations forfaitaires incluses par votre fournisseur d’identité |

`extra.challengeAuth(options)` est disponible uniquement sur les actions **[!UICONTROL facultatives]**. Renvoyez son résultat à partir de votre gestionnaire pour demander à l’utilisateur de se connecter au lieu de renvoyer du contenu.

| Option | Description |
|--------|-------------|
| `error` | Code d’erreur du porteur [RFC 6750](https://datatracker.ietf.org/doc/html/rfc6750) : `invalid_token`, `insufficient_scope` ou `invalid_request`. La valeur par défaut est `insufficient_scope`. |
| `errorDescription` | Message affiché à l’utilisateur. La valeur par défaut est une invite de connexion générique |
| `scope` | Portées délimitées par des espaces à demander. Omettez-le pour laisser la plateforme revenir aux portées prises en charge par l’application |

>[!IMPORTANT]
>
>Définissez toujours le `error` explicitement. Utilisez `invalid_token` pour un appelant sans session valide et `insufficient_scope` uniquement pour un appelant dont le jeton est valide mais n’a pas de portée requise. La valeur est transmise à la plateforme LLM, qui décide elle-même du libellé de l’invite présentée à l’utilisateur. Envoyez le code qui décrit précisément la condition plutôt que celui dont vous préférez l’invite.

## Comportement de la plateforme LLM {#platform-behavior}

La prise en charge de l’authentification des actions individuelles varie selon la plateforme. Configurez de la même manière pour les deux ; la différence réside dans ce que l’utilisateur ou l’utilisatrice voit.

| Comportement | [!DNL ChatGPT] | [!DNL Claude] |
|----------|----------------|---------------|
| Granularité | Par action | Par connecteur |
| Authentification mixte, où l’application n’est pas entièrement fermée | Pris en charge. Seules les actions **[!UICONTROL Obligatoires]** invitent à se connecter | Pas de prise en charge. L’ensemble du connecteur demande la connexion, y compris les actions non activées |
| Configuration du connecteur | Définissez **[!UICONTROL Authentification]** sur **[!UICONTROL Aucune authentification]** lorsque chaque action est **[!UICONTROL Aucune]**, **[!UICONTROL OAuth]** lorsque chaque action est **[!UICONTROL Requise]** et **[!UICONTROL Mixed]** dans les autres cas | Aucun choix d’authentification à effectuer ; la connexion commence sur **[!UICONTROL Connect]** |
| Réauthentification | Invité dans la conversation lorsqu’une action avec point de contrôle est appelée | Invité pour le connecteur |


## En relation

- [Authentifier les utilisateurs finaux avec votre propre fournisseur d’identité](/help/guides/authentication.md)
- [Champs d’action et de widget](/help/reference/reference-docs.md)
- [Résolution des problèmes](/help/reference/troubleshooting.md#authentication)
