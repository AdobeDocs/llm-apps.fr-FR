---
title: Dépannage des applications Adobe LLM
description: Résolvez les problèmes courants d’infrastructure, de génération d’actions, de gestionnaire, de widget, de déploiement, d’authentification et de connexion à la plateforme LLM.
source-git-commit: a48b4022979d9a13b4fc3aa870c4ee6375e8bc13
workflow-type: tm+mt
source-wordcount: '1350'
ht-degree: 0%
---

# Résolution des problèmes {#troubleshooting}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Commencez par le symptôme observable. Ne partagez pas les informations d’identification, les jetons, les URL MCP privées ou les résultats de gestionnaires sensibles lors de la résolution des problèmes.

## Création d’applications et génération d’actions

| Symptôme | Quoi essayer |
|---------|-------------|
| Échec de la configuration de l’infrastructure gérée par Adobe | Lisez l’erreur sous **Échec de l’installation**, puis sélectionnez **Réessayer**. Si la page ne peut pas vérifier la progression, sélectionnez **Vérifier à nouveau** ; l’installation se poursuit en arrière-plan |
| Aucun programme AEM n’est disponible. | Demandez à votre administrateur l’accès au programme. Si le chargement a échoué, sélectionnez **Réessayer de charger les programmes** |
| Les nouveaux référentiels n’apparaissent pas | Utilisez **Connecter un autre référentiel** pour accorder l’accès à l’application GitHub des applications LLM Adobe, revenez à l’assistant et sélectionnez **Actualiser les organisations et les référentiels** |
| Échec de la validation du référentiel d’action ou de widget | Utilisez un projet existant compatible ou un référentiel vide. Utilisez des référentiels distincts pour les actions et les widgets, puis sélectionnez à nouveau **Valider le référentiel**. |
| La synchronisation du code AEM n’est pas détectée | Installez la synchronisation du code AEM pour le référentiel de widgets, ajoutez-vous en tant qu’administrateur de site sur sa page de confirmation, revenez à l’assistant, puis sélectionnez **Valider l’installation de la synchronisation du code AEM** |
| La création de l’application n’a pas généré d’actions | La création d’applications ne configure l’infrastructure que . Sur la page de l’application, sélectionnez **Créer automatiquement les actions**, saisissez **Votre site web**, puis sélectionnez **Créer automatiquement mes actions**. Voir [&#x200B; Création d’actions &#x200B;](/help/guides/creating-actions.md) |
| Actions en cours de génération | L’interface utilisateur estime jusqu’à 30 minutes pour la génération. Suivez la progression sur la page Actions ou quittez la page et revenez plus tard |
| Échec de la génération des rapports d’action | Vérifiez que le site web est public via HTTPS et que, pour vos propres référentiels, cet accès au référentiel est toujours valide. Si le problème persiste, contactez l’équipe Beta en indiquant le message d’erreur visible |

## Actions et gestionnaires

| Symptôme | Quoi essayer |
|---------|-------------|
| Action non appelée | Activez le plug-in ou le connecteur de l’application dans la conversation. Confirmez **Exposer au modèle d’IA** dans les paramètres d’action et le commutateur **Visibilité** de l’action sur la page de l’application est activé. Amélioration de la description et redéploiement des modifications apportées aux métadonnées |
| Réponse vide ou d’erreur | Exécutez `npm test`, puis appelez le gestionnaire avec MCP Inspector ou `curl`. Voir [Développement et test des gestionnaires locaux](/help/reference/development.md) |
| Le gestionnaire fonctionne localement, mais pas après le déploiement | Vérifiez la dernière validation push, confirmez la configuration de l’exécution et vérifiez que l’identifiant du code d’action correspond `actions/<code-identifier>/index.js` |
| L’action générée ne peut pas être marquée comme révisée. | La génération du gestionnaire et du widget de confirmation a réussi. Vérifiez les demandes d’extraction générées à la recherche de conflits de fusion, rechargez l’action et sélectionnez à nouveau **Marquer comme révisé** |

## Widgets

| Symptôme | Quoi essayer |
|---------|-------------|
| Le widget n’est pas rendu | Vérifiez l’URL du script, l’URL du widget, le HTTPS, la publication EDS, les domaines CSP et les en-têtes CORS |
| Le widget s’affiche mais n’affiche aucune donnée. | Appelez le gestionnaire avec MCP Inspector et comparez sa forme `structuredContent` avec les champs lus depuis `bridge.toolResult` |
| Le widget fonctionne en prévisualisation directe, mais pas dans le ChatGPT | L’aperçu direct peut utiliser des données d’exemple. Testez le résultat du gestionnaire déployé et vérifiez que l’origine EDS est autorisée par CORS et CSP. |
| Requête du navigateur bloquée | Ajoutez uniquement l’origine requise au champ CSP correct et redéployez |
| L’éditeur d’en-têtes HTTP ne peut pas enregistrer la configuration | Utilisez le [service de configuration &#x200B;](https://aem.live/docs/config-service-setup) ou demandez à l’administrateur EDS d’initialiser la configuration des en-têtes de site |

Ne consignez pas les valeurs de `bridge.toolResult` complètes lorsqu’elles peuvent contenir des données personnelles ou sensibles.

## Déploiement

| Symptôme | Quoi essayer |
|---------|-------------|
| Le déploiement échoue lors de la **préparation** | Vérifiez que le référentiel du gestionnaire est lié et que votre accès Adobe Developer Console est toujours valide. |
| Le déploiement échoue pendant **création de l’application** | Exécutez `npm install`, `npm test` et `npm run build` localement. Correction des échecs de dépendance, de syntaxe ou de test et transmission des modifications |
| Le déploiement a réussi mais les modifications sont manquantes. | Vérifiez que la validation attendue a été envoyée et redéployée dans le même environnement. |
| L’action reste **non déployée** | Effectuez un nouveau déploiement après avoir examiné l’action ou modifié ses métadonnées |

## Authentification {#authentication}

S’applique lorsque l’option **[!UICONTROL Activer l’authentification]** est activée. Voir [&#x200B; Authentifier les utilisateurs finaux avec votre propre fournisseur d’identité](/help/guides/authentication.md).

| Symptôme | Quoi essayer |
|---------|-------------|
| Les actions sont toujours publiques après l’enregistrement des paramètres | Déployez à nouveau l’application dans cet environnement. Les modifications d’authentification prennent effet lors du prochain déploiement |
| Les paramètres ne s’affichent pas correctement après le changement d’environnement | Vérifiez que le sélecteur **&#x200B;**&#x200B;affiche l’environnement souhaité. **[!UICONTROL Évaluation]** et **[!UICONTROL Production]** sont configurés indépendamment |
| La connexion ne démarre pas | Vérifiez que vous avez déployé l’application après l’activation de l’authentification et que l’action utilise **[!UICONTROL Obligatoire]**. Une action **[!UICONTROL facultative]** s’affiche uniquement lorsque son gestionnaire demande la connexion |
| La plateforme envoie l’utilisateur vers une page de connexion incorrecte | Vérifiez que **[!UICONTROL Émetteur]** correspond exactement à l’URL de l’émetteur de votre fournisseur d’identité et qu’il est accessible via HTTPS public |
| Connexion réussie mais chaque appel est toujours rejeté | Confirmez que votre fournisseur d’identité émet des jetons dont l’audience est l’URL du serveur MCP de l’application pour cet environnement et que le jeton est un jeton JWT signé avec un algorithme asymétrique. Voir [Exigences en matière de jetons](/help/reference/authentication-reference.md#token-requirements) |
| Le fournisseur d’identité ne reçoit jamais de trafic | Les points d’entrée de découverte, d’autorisation et de jeton de votre fournisseur doivent être accessibles via HTTPS public. Recherchez un pare-feu, un WAF ou une adresse IP devant celui-ci — votre application peut être accessible alors que votre fournisseur ne l&#39;est pas |
| Le fournisseur d’identité refuse d’émettre un jeton pour l’audience demandée | Certains fournisseurs n’émettent des jetons que pour une ressource qui est enregistrée auprès d’eux. Vérifiez que l’URL du serveur MCP de l’application est enregistrée comme identifiant de ressource dans votre fournisseur d’identité. |
| Une nouvelle étendue ajoutée n’est pas demandée lors de la connexion | Les plateformes LLM mettent en cache les métadonnées publiées de l’application pendant quelques minutes. Patientez, puis réessayez de vous connecter. |
| L’utilisateur est invité à se reconnecter pour une étendue | L’action nécessite une portée que le jeton ne transporte pas. Ajoutez la portée à l’octroi du client dans votre fournisseur d’identité ou supprimez-la de l’action |
| L’utilisateur reçoit plusieurs invites de connexion | Un gestionnaire qui renvoie des `extra.challengeAuth()` à chaque appel sans vérifier l’identité de l’appelant demande à nouveau de se connecter après l’authentification. Demander l’authentification uniquement lorsque l’identité dont l’action a besoin est manquante |
| La connexion est refusée avant que l&#39;utilisateur n&#39;atteigne votre fournisseur d&#39;identité | L’URI de redirection envoyé par la plateforme LLM n’est pas enregistré auprès de votre serveur d’autorisation. Certaines plateformes émettent une valeur différente pour chaque connecteur. Enregistrez donc la valeur affichée sur l’écran de configuration du connecteur |
| L’enregistrement est bloqué avec un message d’étendue non prise en charge | Ajoutez la portée à **[!UICONTROL Portées prises en charge]** ou supprimez-la de l’action qui la requiert |
| Une action publique invite toujours à se connecter dans [!DNL Claude] | Vérifiez que le connecteur utilise **[!UICONTROL Se connecter lorsque cela s’avère nécessaire]** et non **[!UICONTROL Se connecter maintenant]**. Pour une application déployée plus ancienne, mettez à jour l’exécution et redéployez. Une application mixte sur l’exécution actuelle rend les actions publiques appelables anonymement |
| Une action **[!UICONTROL Facultative]** renvoie un texte de connexion, mais aucune invite de connexion Claude | `extra.challengeAuth()` est un défi de résultat d&#39;outil ChatGPT. Pour un point de connexion portable, utilisez une action **[!UICONTROL Obligatoire]** afin que l’exécution renvoie le défi HTTP attendu par Claude |
| L’application demande la connexion même si chaque action est **[!UICONTROL Aucune]** | Désactivez **[!UICONTROL activer l’authentification]** puis déployez. Lorsqu’elle est activée, l’application publie un serveur d’autorisation même lorsqu’aucune action n’est déclenchée |

Ne collez pas de jetons d’accès, de jeux de réclamations ou de secrets clients de fournisseurs d’identité dans une demande d’assistance.

## Plug-ins ChatGPT

| Symptôme | Quoi essayer |
|---------|-------------|
| Le plug-in n’apparaît pas | Ouvrez [&#128279;](https://chatgpt.com/plugins), vérifiez que votre compte ou votre espace de travail autorise les serveurs MCP personnalisés, puis installez ou connectez le plug-in obtenu |
| Échec de la création du plug-in | Copiez à nouveau l’URL complète du serveur HTTPS MCP depuis **Tester l’application** et choisissez la méthode d’authentification correspondant à l’application. Utilisez **Aucune authentification** uniquement pour une application publique, et non comme solution de contournement pour OAuth ayant échoué |
| Le plug-in se connecte, mais ne peut pas appeler d’actions. | Vérifiez que le plug-in est associé au chat, que les actions sont exposées au modèle et que la dernière version est déployée |
| Le plug-in utilise un environnement incorrect. | Modifiez ou recréez le module externe avec l’URL du serveur MCP d’évaluation ou de production prévue |
| Le plug-in affiche toujours les anciennes métadonnées d’action. | Déployez les modifications, sélectionnez **Actualiser** sur la connexion ChatGPT, puis démarrez une nouvelle conversation |

Si le problème persiste, enregistrez le nom de l’application, l’environnement, l’étape d’échec, l’heure et le message d’erreur visible avant de contacter l’équipe Beta. N’incluez pas de secrets ou de données client sensibles.
