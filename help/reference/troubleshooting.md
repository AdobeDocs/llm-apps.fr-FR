---
title: Dépannage pour les applications Adobe LLM
description: Solutions aux problèmes courants de création, de déploiement et de test des applications Adobe LLM.
source-git-commit: 1a99e2e80e50a3bcf9ce6fb910365202bf06e113
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 0%

---


# Résolution des problèmes {#troubleshooting}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Cette section fournit des informations de dépannage lors de l’utilisation de [!DNL Adobe LLM Apps].

## Problèmes courants

| Symptôme | Cause possible | Quoi essayer |
|---------|----------------|-------------|
| L’application n’apparaît pas sur la plateforme LLM. | Votre abonnement à la plateforme LLM ne prend pas en charge les applications MCP personnalisées ou le mode Développeur n&#39;est pas activé | Vérifiez que votre plan prend en charge les applications MCP personnalisées. Activez le mode Développeur dans **Paramètres → Applications → Paramètres avancés** |
| Erreur « Échec de connexion » dans la plateforme LLM | L’URL du serveur MCP est incorrecte ou le déploiement a échoué. | Vérifiez deux fois l’URL dans la page Détails de l’application. Rechercher les échecs dans l’historique de déploiement |
| Action non appelée | La plateforme LLM n&#39;a pas pu faire correspondre la question de l&#39;utilisateur à votre action | Utilisez `@YourApp` pour l’appeler explicitement. Améliorez la description de l’action pour aider le modèle à correspondre à l’intention |
| Le widget n’est pas rendu | Les URL de widget EDS ou les domaines CSP sont mal configurés. | Vérifiez l’URL du script et l’URL incorporée du widget dans la boîte de dialogue Créer une action . Vérifiez que la ressource CSP et les domaines de connexion incluent votre origine EDS. |
| Réponse vide ou d’erreur | Le gestionnaire présente un bogue ou est manquant | Testez localement avec `npm start` en premier. Voir [&#x200B; Développement local &#x200B;](/help/reference/development.md#local-development) |
| Le widget se charge mais n’affiche aucune donnée. | La forme `structuredContent` ne correspond pas à ce que le bloc attend | Enregistrez `bridge.toolResult` dans la fonction `decorate` de votre bloc et comparez-la à la sortie du gestionnaire |
| Échec du déploiement à « Cloner et créer » | `npm install` ou erreur de build webpack dans votre référentiel | Exécutez `npm install && npm run build` localement pour reproduire l’erreur |
| Échec du déploiement à « Collecter les informations d’identification » | Référentiel non lié ou projet Developer Console mal configuré | Vérifiez que le référentiel est lié sur la page Paramètres des détails de l’application . |
| Erreur CORS lors du chargement du widget | En-têtes de `access-control-allow-origin` manquants sur le site EDS | Configuration des en-têtes CORS via `admin.hlx.page` |
| L’éditeur d’en-têtes HTTP renvoie `404 Error updating config: config not found` lors de l’enregistrement des en-têtes CORS | Il manque une section `headers` à la configuration du site | Consultez [&#x200B; Initialisation des en-têtes de configuration de site EDS &#x200B;](#initialize-the-eds-site-config-headers-section) ci-dessous |
| Le widget est rendu dans l’aperçu, mais pas dans la plateforme LLM. | Le bloc retourne aux données d’exemple en mode aperçu, mais échoue avec les données actives | Testez avec des `structuredContent` réelles en utilisant l&#39;Inspecteur MCP ou curl |

## Initialisez la section En-têtes de configuration de site EDS .

Si l’éditeur d’en-têtes HTTP renvoie `404 Error updating config: config not found`, il manque une section `headers` à la configuration du site. Corrigez-le manuellement :

1. Accédez à [tools.aem.live/tools/headers-edit/index.html](https://tools.aem.live/tools/headers-edit/index.html), saisissez votre organisation et votre site, puis cliquez sur **[!UICONTROL Récupérer]**.
2. Ouvrez le navigateur DevTools (onglet Network) et copiez la valeur de l’en-tête `x-auth-token` à partir de la requête Fetch.
3. Récupérez la configuration actuelle du site :

   ```bash
   curl -H "x-auth-token: $TOKEN" \
     https://admin.hlx.page/config/<your-github-org>/sites/<your-eds-repo>.json > config.json
   ```

4. Ouvrez `config.json` et ajoutez des `"headers": {}` à l’objet JSON.
5. PUBLIEZ la configuration mise à jour :

   ```bash
   curl -X POST \
     -H "x-auth-token: $TOKEN" \
     -H "Content-Type: application/json" \
     -d @config.json \
     "https://admin.hlx.page/config/<your-github-org>/sites/<your-eds-repo>.json"
   ```

6. Rechargez l’éditeur d’en-têtes et enregistrez l’en-tête `Access-Control-Allow-Origin` normalement.

