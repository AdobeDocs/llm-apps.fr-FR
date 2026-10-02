---
title: Authentifier les utilisateurs finaux avec votre propre fournisseur d’identité
description: Activez l’authentification des utilisateurs finaux pour votre application Adobe LLM afin qu’une plateforme LLM prise en charge connecte l’utilisateur à votre fournisseur d’identité avant d’appeler des actions protégées.
source-git-commit: fd41dbcabc4db0cae766de19cb042d7c85d8b7aa
workflow-type: tm+mt
source-wordcount: '2149'
ht-degree: 0%
---

# Authentification des utilisateurs finaux avec votre propre fournisseur d’identité {#authentication}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Par défaut, chaque action de votre application est publique : toute plateforme LLM disposant de votre URL de serveur MCP peut l’appeler et votre gestionnaire ne peut pas identifier l’utilisateur final.

Activez l’authentification lorsqu’une action doit savoir quel utilisateur final demande, par exemple, de renvoyer ses commandes, ses droits ou les détails de son compte. La plateforme LLM connecte l’utilisateur avec **votre** fournisseur d’identité (IdP), envoie le jeton d’accès résultant avec chaque appel et votre gestionnaire reçoit l’identité vérifiée.

**Parcours :** copiez l’identifiant de ressource → configurez votre fournisseur d’identité → activez l’authentification → définissez un mode d’authentification pour chaque action → déployer → lire l’identité dans votre gestionnaire → tester l’application protégée.

Il s’agit d’une branche avancée, qui ne fait pas partie du parcours de première exécution. Terminez [Créer automatiquement votre première application](/help/guides/create-app.md) et [Déployer votre application](/help/guides/deploy-your-app.md) d’abord.

## Fonctionnement

Vous apportez votre propre fournisseur d’identité. L’application déployée n’est qu’un **serveur de ressources** OAuth 2.1, qui vérifie les jetons émis par le serveur d’autorisation. Il n’émet jamais de jetons et [!DNL Adobe] ne stocke jamais votre identifiant ou secret client.

```
┌── Your identity provider ───────────────────────────────────────────────┐
│  Authorization server — you own it                                      │
│  Issues access tokens, holds the user directory, defines the scopes     │
└─────────────────────────────────────────────────────────────────────────┘
        ▲  2  user signs in, platform gets an access token
        │                                    │
        │  1  platform discovers your        │  3  every tools/call carries
        │     authorization server from      │     Authorization: Bearer <token>
        │     your app's metadata            ▼
┌── LLM platform (ChatGPT, Claude, …) ────────────────────────────────────┐
└─────────────────────────────────────────────────────────────────────────┘
                                             │
                                             ▼
┌── Your LLM App on Adobe I/O Runtime ────────────────────────────────────┐
│  Resource server — verifies the token's signature, issuer, audience,    │
│  and expiry, then enforces the auth mode you set for each action        │
│                                                                         │
│  Your handler reads the verified identity from its second argument      │
└─────────────────────────────────────────────────────────────────────────┘
```

L’authentification est configurée **par environnement**. **[!UICONTROL Évaluation]** et **[!UICONTROL Production]** contiennent des paramètres indépendants afin que vous puissiez vérifier la configuration par rapport à un client IdP de développement avant de l’activer sur **[!UICONTROL Production]**.

## Avant de commencer

- Un fournisseur d’identité OAuth 2.1 ou OpenID Connect qui émet des jetons d’accès **JWT** signés avec un algorithme asymétrique. Les jetons opaques et les jetons signés HMAC ne sont pas pris en charge. Voir [ Exigences en matière de jetons ](/help/reference/authentication-reference.md#token-requirements).
- Un accès administrateur à ce fournisseur d’identité, afin que vous puissiez enregistrer une API et un client.
- Votre application a été déployée au moins une fois dans l’environnement que vous êtes en train de configurer. L’URL du serveur MCP déployé est la valeur sur laquelle vos jetons doivent être inclus.

## Copier l’identifiant de la ressource

L’**identifiant de ressource** de votre application est son URL de serveur MCP. Chaque jeton d’accès émis par votre fournisseur d’identité pour cette application doit nommer cette URL exacte comme audience. Cette liaison empêche la relecture d’un jeton émis pour un autre service sur votre application.

1. Ouvrez la page Détails de l’application .
2. Faites défiler l’écran pour **[!UICONTROL Tester l’application]**.
3. Sous l’environnement en cours de configuration, sélectionnez **[!UICONTROL Copier l’URL]**.

![Détails de l&#39;application — Copiez l&#39;URL du serveur MCP intermédiaire](/help/assets/guide-onboarding-agent/app-mcp-url.png)

Conservez cette valeur : vous en aurez besoin dans votre fournisseur d’identité à l’étape suivante. Collez l’URL copiée plutôt que de la saisir à nouveau. La vérification de l’audience correspond exactement aux chaînes, y compris à tout composant de chemin d’accès. Par conséquent, une seule différence de caractère entraîne l’échec de la validation de chaque jeton.

>[!NOTE]
>
>Chaque environnement possède sa propre URL de serveur MCP, et donc sa propre audience. Configurez **[!UICONTROL Évaluation]** et **[!UICONTROL Production]** séparément.

## Configuration de votre fournisseur d’identité

Les étapes exactes diffèrent selon le fournisseur, mais chaque fournisseur a besoin des mêmes quatre choses.

1. **Enregistrez votre application en tant qu’API (ressource).** Définissez son identifiant (la valeur que le fournisseur place dans la revendication `aud` du jeton) sur l’URL du serveur MCP que vous avez copiée. Les fournisseurs étiquettent ce champ différemment, généralement *Identifiant* ou *Audience*. N’utilisez pas de valeur générique telle que `api`. L’identifiant doit être propre à cette application ou un jeton émis pour un autre service peut être relu par rapport à celle-ci.
2. **Définissez les portées** vous souhaitez bloquer les actions avec, par exemple `orders:read` ou `profile:read`. Utilisez une portée par autorisation significative afin qu’une action ne demande que ce dont elle a besoin.
3. **Support PKCE.** Les plateformes LLM envoient un `code_challenge` avec `code_challenge_method=S256` à chaque demande d’autorisation. Votre serveur d’autorisation doit donc prendre en charge S256 PKCE et `"code_challenge_methods_supported": ["S256"]` annoncer dans ses métadonnées.
4. **Autoriser la plateforme LLM à s’enregistrer en tant que client.** Les plateformes LLM prises en charge créent leur propre client OAuth par rapport à votre serveur d’autorisation. Par conséquent, activez l’enregistrement client dynamique si votre fournisseur vous le propose. Sinon, créez manuellement un client public et fournissez son identifiant client (et son secret, uniquement si votre fournisseur requiert une authentification client confidentielle) lors de la configuration du connecteur sur la plateforme. Enregistrez l’URI de redirection dans les documents Platform ; pour les surfaces hébergées de [!DNL Claude] qui sont `https://claude.ai/api/mcp/auth_callback`. Certaines plateformes émettent un URI de redirection distinct pour chaque connecteur créé par l’utilisateur, [!DNL ChatGPT] entre autres. Lisez donc la valeur de l’écran de configuration du connecteur et enregistrez-le avant la première connexion. Un URI de redirection non enregistré entraîne le rejet pur et simple de la demande d’autorisation par votre serveur d’autorisations.

>[!IMPORTANT]
>
>Les points d’entrée d’émetteur, JWKS, d’autorisation et de jeton de votre fournisseur d’identité doivent tous être accessibles via HTTPS public. La plateforme LLM et votre application déployée récupèrent les métadonnées directement auprès de votre fournisseur. Par conséquent, un fournisseur d’identité derrière un VPN ou un place sur la liste autorisée IP ne peut pas se connecter. Un pare-feu ou un pare-feu d’application web situé en face de votre fournisseur est une cause courante et il peut interrompre le flux même lorsque votre application elle-même est accessible.

## Activer l’authentification

1. Dans le volet de navigation de gauche, sélectionnez **[!UICONTROL Paramètres]**, puis ouvrez l’onglet **[!UICONTROL Authentification]**.
2. Dans ****, choisissez **[!UICONTROL Évaluation]** ou **[!UICONTROL Production]**.
3. Activez **[!UICONTROL Activer l’authentification]**.
4. Sous **[!UICONTROL Paramètres principaux]**, saisissez :
   - **[!UICONTROL Émetteur]** : URL de l’émetteur de votre fournisseur d’identité, qui correspond également à la valeur qu’il place dans la réclamation `iss` de chaque jeton. Ce code est obligatoire, doit être au format HTTPS et est également publié en tant que serveur d’autorisation de votre application afin que les plateformes LLM puissent découvrir où envoyer les utilisateurs. Un seul fournisseur d’identité est pris en charge par application.
   - **[!UICONTROL Portées prises en charge]** — chaque portée que les actions de cette application sont autorisées à exiger. Reflète les portées que vous avez définies dans votre fournisseur d’identité.
5. **[!UICONTROL Paramètres avancés]** est facultatif. Définissez **[!UICONTROL URI JWKS]** uniquement lorsque vos clés de signature ne se trouvent pas à l’endroit où les métadonnées de votre serveur d’autorisation les annoncent. Dans le cas contraire, l’application les découvre automatiquement.
6. Sélectionnez **[!UICONTROL Enregistrer]**.

![Authentification : activez l&#39;authentification et renseignez les paramètres principaux](/help/assets/guide-authentication/auth-core-settings.png)

Pour connaître les éléments acceptés par chaque champ, voir [Paramètres d’authentification](/help/reference/authentication-reference.md#authentication-settings).

## Choisir un mode d’authentification pour chaque action

Lorsque vous activez **[!UICONTROL Activer l’authentification]**, chaque action actuellement définie sur **[!UICONTROL Aucune]** devient **[!UICONTROL Obligatoire]**. Sous **[!UICONTROL Configuration par action]**, vérifiez cette affectation et définissez le mode dont chaque action a besoin :

| Mode | Comportement |
|------|----------|
| **[!UICONTROL Aucune]** | Publique. L’action peut être appelée sans jeton. |
| **[!UICONTROL Requis]** | Fermé. L’action peut uniquement être appelée avec un jeton valide qui transfère chaque portée que vous répertoriez pour elle. Les appelants non authentifiés doivent se connecter. |
| **[!UICONTROL facultatif]** | Appelable anonymement, mais l’action indique également qu’elle prend en charge la connexion. Votre gestionnaire décide par appel de signifier ou non un résultat générique ou de demander à l’utilisateur de se connecter pour un résultat personnalisé. |

![Authentification — Définissez un mode d&#39;authentification et des portées pour chaque action](/help/assets/guide-authentication/auth-per-action.png)

Les actions déjà définies sur **[!UICONTROL Obligatoire]** ou **[!UICONTROL Facultatif]** conservent leur mode existant.

Pour une action **[!UICONTROL Obligatoire]** ou **[!UICONTROL Facultatif]**, ajoutez les **[!UICONTROL Portées]** dont elle a besoin. Chaque portée doit déjà apparaître dans les **[!UICONTROL Portées prises en charge]** ci-dessus. Sinon, l’application nécessiterait une autorisation qu’elle ne communique pas aux plateformes LLM. L’enregistrement est bloqué jusqu’à ce que la correspondance soit résolue.

**[!UICONTROL Portées prises en charge]** est l’autorité pour cette liste. Si vous supprimez une portée, celle-ci est supprimée de chaque action qui l’exigeait dès que vous apportez la modification. Ajoutez donc d’abord une portée, puis affectez-la à une action.

**[!UICONTROL Exiger une authentification pour toutes les actions]** définit chaque action sur **[!UICONTROL Obligatoire]**. L’effacement renvoie chaque action à **[!UICONTROL Aucune]**.

Sélectionnez **[!UICONTROL Enregistrer]** lorsque vous avez terminé. Les modifications du mode d’authentification et de l’étendue sont enregistrées avec les paramètres au niveau de l’application.

>[!IMPORTANT]
>
>La désactivation de l’option **[!UICONTROL Activer l’authentification]** annule cette configuration par action pour l’environnement sélectionné ; chaque mode et portée d’action sont effacés et ne sont plus mémorisés. L’activer à nouveau recommence à partir de l’état **[!UICONTROL requis]**.

>[!NOTE]
>
>Définir chaque action sur **[!UICONTROL Aucune]** ne désactive pas l’authentification. Aucun appel n’est refusé dans cet état, mais l’application publie toujours votre serveur d’autorisation sur les plateformes LLM, de sorte qu’un client peut proposer à l’utilisateur une connexion qui n’accorde aucun accès supplémentaire. Pour rendre l’application entièrement publique, désactivez **[!UICONTROL Activer l’authentification]** et déployez.

Les modes mixtes dans une application (certaines actions sont publiques, d&#39;autres sont bloquées) sont pris en charge et [!DNL ChatGPT] applique individuellement le mode de chaque action : seules les actions bloquées invitent l&#39;utilisateur à se connecter.

>[!IMPORTANT]
>
>[!DNL Claude] est l’exception. Il applique l’authentification par connecteur plutôt que par action. Ainsi, si une action de l’application est définie sur **[!UICONTROL Obligatoire]** ou **[!UICONTROL Facultatif]**, [!DNL Claude] demande à l’utilisateur de se connecter avant d’utiliser le connecteur, y compris les actions définies sur **[!UICONTROL Aucune]**. Pour qu’une action reste publique pour [!DNL Claude] utilisateurs et utilisatrices, hébergez-la dans une application distincte.

## Déployer la modification

Les modifications d’authentification prennent effet lors du prochain déploiement de cette application. **Déployez à nouveau l’application** dans l’environnement que vous avez configuré. Voir [Déployer votre application](/help/guides/deploy-your-app.md).

L’URL de votre serveur MCP ne change pas. Par conséquent, tout plug-in ou connecteur que vous avez déjà créé continue à fonctionner. Elle est désormais fermée, de sorte que ses utilisateurs sont invités à se connecter la prochaine fois qu’ils l’utiliseront.

## Lire l’identité dans votre gestionnaire

Une identité vérifiée atteint votre gestionnaire en tant que deuxième argument. Elle est présente chaque fois que l’appelant a envoyé un jeton valide, quel que soit le mode d’authentification de l’action. Une action **[!UICONTROL facultative]** peut donc personnaliser son résultat lorsqu’un jeton est présent et renvoyer un résultat lorsqu’il ne l’est pas.

Utilisez `getAuthenticatedUser` pour lire l’utilisateur connecté :

```javascript
const { getAuthenticatedUser } = require('@adobe/llm-apps-runtime');

module.exports = async ({ orderId }, extra) => {
  const userId = getAuthenticatedUser(extra);

  if (!userId) {
    return { content: [{ type: 'text', text: 'Sign in to see your orders.' }] };
  }

  const order = await fetchOrderForUser(userId, orderId);

  return {
    content: [{ type: 'text', text: `Order ${order.id} is ${order.status}.` }],
    structuredContent: order
  };
};
```

Vous n’avez pas besoin de vérifier le jeton vous-même. Pour une action **[!UICONTROL Obligatoire]**, l’exécution bloque chaque appel auquel il manque un jeton valide portant les portées que vous avez répertoriées. De ce fait, le gestionnaire ne s’exécute que pour un appelant autorisé. Utilisez `hasScope` lorsque vous souhaitez créer une branche sur une autorisation plutôt que de dépendre du point de contrôle, par exemple, dans une action **[!UICONTROL facultative]**.

Une action **[!UICONTROL facultative]** peut demander à l’utilisateur de se connecter en cours de conversation en renvoyant la `extra.challengeAuth()`. Cette option est disponible uniquement sur les actions **[!UICONTROL facultatives]** :

```javascript
module.exports = async ({ signIn }, extra) => {
  if (signIn && !extra.authInfo) {
    return extra.challengeAuth({
      error: 'invalid_token',
      errorDescription: 'Sign in to see member pricing.'
    });
  }

  return {
    content: [{
      type: 'text',
      text: extra.authInfo ? await memberDeals() : await publicDeals()
    }]
  };
};
```

Décidez s’il faut réaffecter à partir d’un paramètre d’entrée explicite, comme `signIn` le fait ici, plutôt que d’examiner le libellé de l’utilisateur ou de l’utilisatrice.

Définissez `error` pour correspondre à la condition que vous signalez. Utilisez `invalid_token` lorsque l’appelant ne dispose d’aucune session valide et doit se connecter, comme dans l’exemple ci-dessus, et `insufficient_scope` lorsque l’appelant est déjà connecté mais que l’action n’a pas de portée nécessaire. La plateforme LLM choisit le libellé de l’invite que l’utilisateur voit et la mesure dans laquelle ce libellé varie en fonction de cette valeur. Envoyez donc le code qui décrit la condition avec précision.

Ne défiez que lorsque l’identité dont vous avez besoin est réellement manquante, comme le fait ici le contrôle de `!extra.authInfo`. Un gestionnaire qui défie inconditionnellement ne peut pas être satisfait en se connectant. L’utilisateur est donc invité à s’authentifier à nouveau à chaque appel.

>[!NOTE]
>
>Le [!DNL ChatGPT], une connexion levée de cette manière demande à l’utilisateur de reconnecter le connecteur plutôt que d’accorder une autorisation supplémentaire. Une [!DNL Claude], l’utilisateur se connecte avant l’exécution de toute action. Par conséquent, une action n’a jamais besoin d’en déclencher une.

Conserver l’identité côté serveur. Transmettez uniquement ce dont le widget a besoin dans `structuredContent`, et n’y placez jamais le jeton d’accès. Voir [ Personnaliser un gestionnaire généré](/help/guides/customize-handler.md).

Pour le contrat complet, voir [API d’authentification de gestionnaire](/help/reference/authentication-reference.md#handler-auth-api).

## Tester l’application protégée

Votre plug-in ou connecteur existant récupère la modification après le déploiement. Pour en configurer un à partir de zéro :

### [!DNL ChatGPT]

Dans la boîte de dialogue **[!UICONTROL Nouveau plug-in]**, définissez **[!UICONTROL Authentification]** pour correspondre à la manière dont vous avez configuré les actions de l’application :

| Actions de votre application | Sélectionner |
|--------------------|--------|
| Tout est défini sur **[!UICONTROL Aucun]** | **[!UICONTROL Aucune authentification]** |
| Tout est défini sur **[!UICONTROL Obligatoire]** | **[!UICONTROL Oauth]** |
| Toute autre combinaison | **[!UICONTROL Mixte]** |

![ChatGPT — sélectionnez le mode d&#39;authentification pour le plug-in](/help/assets/guide-authentication/chatgpt-authentication-mode.png)

Une action **[!UICONTROL facultative]** accepte toujours les appels anonymes, de sorte qu’une application qui en contient un n’est jamais complètement fermée ; choisissez **[!UICONTROL Mixte]** même si chaque action est définie sur **[!UICONTROL Facultative]**. Seul **[!UICONTROL Obligatoire]** refuse les appelants non authentifiés.

Voir [Tester le plug-in ChatGPT](/help/guides/test-in-chatgpt.md) pour la suite de la boîte de dialogue.

### [!DNL Claude]

Ajoutez le connecteur personnalisé, puis sélectionnez **[!UICONTROL Se connecter]** et effectuez la connexion présentée par votre fournisseur d’identité. Aucun choix d’authentification à effectuer : [!DNL Claude] ouvre l’ensemble du connecteur chaque fois qu’une action est fermée. Voir [Test du connecteur Claude](/help/guides/test-in-claude.md).

### Vérifier

- La plateforme vous redirige vers la page de connexion de votre propre fournisseur d’identité.
- Une action protégée renvoie des données spécifiques à l’utilisateur après la connexion.
- Une action protégée vous invite à vous connecter lorsque vous êtes déconnecté.
- Sur [!DNL ChatGPT], une action définie sur **[!UICONTROL Aucune]** répond toujours sans se connecter. Sur [!DNL Claude], l’ensemble du connecteur est fermé.

Si la connexion ne démarre pas ou si un jeton est rejeté, voir [Dépannage](/help/reference/troubleshooting.md#authentication).

## Conseils de sécurité

- Accordez la portée la plus étroite dont chaque action a besoin. Ne réutilisez pas une portée large pour chaque action.
- Conservez le secret client dans votre fournisseur d’identité et dans la configuration du connecteur de la plateforme LLM. Ne le mettez jamais en action avec des métadonnées, du code de gestionnaire, un JavaScript de widget ou un contrôle de code source.
- Traiter les revendications de jeton comme une entrée provenant d’un système externe. Validez tout ce que vous lisez sur `authInfo.extra` avant de l’utiliser dans une requête.
- Autoriser et authentifier. Un jeton valide prouve qui est l’utilisateur, non qu’il puisse voir un enregistrement particulier. Vérifiez la propriété dans votre gestionnaire avant de renvoyer les données.
- Ne consignez pas de jetons, de jeux de réclamations complets ou d’identifiants d’utilisateur.
- Renvoyer les erreurs sécurisées. Ne faites pas apparaître les réponses du fournisseur d’identité en amont ni les traces de pile à l’utilisateur.
- Configurez et vérifiez **[!UICONTROL Stage]** par rapport à un client de fournisseur d’identité hors production avant d’activer l’authentification sur **[!UICONTROL Production]**.

## Prochaines étapes

- [Référence d’authentification](/help/reference/authentication-reference.md) : champs, exigences en matière de jeton et comportement de la plateforme.
- [Personnaliser un gestionnaire généré](/help/guides/customize-handler.md) — appelez une API en amont protégée depuis un gestionnaire.
- [Déployez votre application](/help/guides/deploy-your-app.md).
