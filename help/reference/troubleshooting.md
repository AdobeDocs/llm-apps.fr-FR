---
title: Dépannage des applications Adobe LLM
description: Résolvez les problèmes courants liés au référentiel, à l’intégration, au gestionnaire, au widget, au déploiement et au plug-in ChatGPT.
source-git-commit: fd41dbcabc4db0cae766de19cb042d7c85d8b7aa
workflow-type: tm+mt
source-wordcount: '1144'
ht-degree: 0%
---

# Résolution des problèmes {#troubleshooting}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Commencez par les symptômes que vous pouvez voir. Ne partagez pas les informations d’identification, les jetons, les URL MCP privées ou les résultats de gestionnaires sensibles lors de la résolution des problèmes.

## Création et intégration d’applications

| Symptôme | Quoi essayer |
|---------|-------------|
| Les nouveaux référentiels n’apparaissent pas | Sélectionnez **Gérer les référentiels sur GitHub**, accordez à l’application GitHub des applications LLM Adobe l’accès aux deux référentiels, revenez à la boîte de dialogue et actualisez les listes |
| Le référentiel EDS nécessite la synchronisation du code AEM. | Installez la synchronisation du code AEM pour le référentiel EDS, puis revenez à la boîte de dialogue Créer une application LLM . |
| La validation EDS indique que vous n’êtes pas un administrateur | Sélectionnez **Ouvrir l’administration AEM Live**, ajoutez-vous en tant qu’administrateur du site EDS, puis actualisez le référentiel |
| L’intégration est toujours en cours | Compter environ 15 minutes. Vous pouvez quitter la page et revenir ultérieurement |
| Échec des rapports d’intégration | Vérifiez que les deux référentiels sont accessibles et que le site web est public via HTTPS, puis contactez l’équipe Beta avec le message d’erreur visible |

## Actions et gestionnaires

| Symptôme | Quoi essayer |
|---------|-------------|
| Action non appelée | Joignez le plug-in ChatGPT, vérifiez **Exposer au modèle d’IA** est activé, améliorez la description de l’action et redéployez les modifications apportées aux métadonnées. |
| Réponse vide ou d’erreur | Exécutez `npm test`, puis appelez le gestionnaire avec MCP Inspector ou `curl`. Voir [Développement et test des gestionnaires locaux](/help/reference/development.md) |
| Le gestionnaire fonctionne localement, mais pas après le déploiement | Vérifiez que la dernière validation a été transmise, que la configuration d’exécution est présente et que l’identifiant du code d’action correspond à `actions/<code-identifier>/index.js` |
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
| La connexion ne démarre pas | Vérifiez que l’application a été déployée depuis que vous avez activé l’authentification et que l’action que vous appelez est définie sur **[!UICONTROL Obligatoire]**. Une action **[!UICONTROL facultative]** s’affiche uniquement lorsque son gestionnaire demande la connexion |
| La plateforme envoie l’utilisateur vers une page de connexion incorrecte | Vérifiez que **[!UICONTROL Émetteur]** correspond exactement à l’URL de l’émetteur de votre fournisseur d’identité et qu’il est accessible via HTTPS public |
| Connexion réussie mais chaque appel est toujours rejeté | Confirmez que votre fournisseur d’identité émet des jetons dont l’audience est l’URL du serveur MCP de l’application pour cet environnement et que le jeton est un jeton JWT signé avec un algorithme asymétrique. Voir [Exigences en matière de jetons](/help/reference/authentication-reference.md#token-requirements) |
| Le fournisseur d’identité ne reçoit jamais de trafic | Les points d’entrée de découverte, d’autorisation et de jeton de votre fournisseur doivent être accessibles via HTTPS public. Recherchez un pare-feu, un WAF ou une adresse IP devant celui-ci — votre application peut être accessible alors que votre fournisseur ne l&#39;est pas |
| Le fournisseur d’identité refuse d’émettre un jeton pour l’audience demandée | Certains fournisseurs n’émettent des jetons que pour une ressource qui est enregistrée auprès d’eux. Vérifiez que l’URL du serveur MCP de l’application est enregistrée comme identifiant de ressource dans votre fournisseur d’identité. |
| Une nouvelle étendue ajoutée n’est pas demandée lors de la connexion | Les plateformes LLM mettent en cache les métadonnées publiées de l’application pendant quelques minutes. Patientez, puis réessayez de vous connecter. |
| L’utilisateur est invité à se reconnecter pour une étendue | L’action nécessite une portée que le jeton ne transporte pas. Ajoutez la portée à l’octroi du client dans votre fournisseur d’identité ou supprimez-la de l’action |
| L’utilisateur est invité à se connecter à plusieurs reprises, en boucle | Une action représente un défi à chaque appel. Un gestionnaire qui renvoie des `extra.challengeAuth()` sans vérifier au préalable si l’appelant est déjà authentifié ne peut jamais être satisfait, car une nouvelle connexion produit le même problème. Défi uniquement lorsque l’identité dont l’action a besoin est manquante |
| La connexion est refusée avant que l&#39;utilisateur n&#39;atteigne votre fournisseur d&#39;identité | L’URI de redirection envoyé par la plateforme LLM n’est pas enregistré auprès de votre serveur d’autorisation. Certaines plateformes émettent une valeur différente pour chaque connecteur. Enregistrez donc la valeur affichée sur l’écran de configuration du connecteur |
| L’enregistrement est bloqué avec un message d’étendue non prise en charge | Ajoutez la portée à **[!UICONTROL Portées prises en charge]** ou supprimez-la de l’action qui la requiert |
| Une action définie sur **[!UICONTROL Aucune]** invite toujours à se connecter | Attendu sur [!DNL Claude], qui s’authentifie par connecteur plutôt que par action. |
| L’application demande la connexion même si chaque action est **[!UICONTROL Aucune]** | Désactivez **[!UICONTROL activer l’authentification]** puis déployez. Lorsqu’elle est activée, l’application publie un serveur d’autorisation même lorsqu’aucune action n’est déclenchée |

Ne collez pas de jetons d’accès, de jeux de réclamations ou de secrets clients de fournisseurs d’identité dans une demande d’assistance.

## Plug-ins ChatGPT

| Symptôme | Quoi essayer |
|---------|-------------|
| Le plug-in n’apparaît pas | Activez le mode Développeur, ouvrez [&#128279;](https://chatgpt.com/plugins) vérifiez que le plug-in existe et sélectionnez **Connect** |
| Échec de la création du plug-in | Vérifiez que le mode Développeur est activé, copiez à nouveau l’URL du serveur MCP depuis **Tester l’application**, puis utilisez **URL du serveur** avec **Aucune authentification**. |
| Le plug-in se connecte, mais ne peut pas appeler d’actions. | Vérifiez que le plug-in est associé au chat, que les actions sont exposées au modèle et que la dernière version est déployée |
| Le plug-in utilise un environnement incorrect. | Modifiez ou recréez le module externe avec l’URL du serveur MCP d’évaluation ou de production prévue |

Si le problème persiste, enregistrez le nom de l’application, l’environnement, l’étape d’échec, l’heure et le message d’erreur visible avant de contacter l’équipe Beta. N’incluez pas de secrets ou de données client sensibles.
