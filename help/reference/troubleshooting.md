---
title: Dépannage des applications Adobe LLM
description: Résolvez les problèmes courants liés au référentiel, à l’intégration, au gestionnaire, au widget, au déploiement et au plug-in ChatGPT.
source-git-commit: eec74b87457bc852d7a8dd0e46c2a4385a93ae0a
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 1%

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

## Plug-ins ChatGPT

| Symptôme | Quoi essayer |
|---------|-------------|
| Le plug-in n’apparaît pas | Activez le mode Développeur, ouvrez [&#128279;](https://chatgpt.com/plugins) vérifiez que le plug-in existe et sélectionnez **Connect** |
| Échec de la création du plug-in | Vérifiez que le mode Développeur est activé, copiez à nouveau l’URL du serveur MCP depuis **Tester l’application**, puis utilisez **URL du serveur** avec **Aucune authentification**. |
| Le plug-in se connecte, mais ne peut pas appeler d’actions. | Vérifiez que le plug-in est associé au chat, que les actions sont exposées au modèle et que la dernière version est déployée |
| Le plug-in utilise un environnement incorrect. | Modifiez ou recréez le module externe avec l’URL du serveur MCP d’évaluation ou de production prévue |

Si le problème persiste, enregistrez le nom de l’application, l’environnement, l’étape d’échec, l’heure et le message d’erreur visible avant de contacter l’équipe Beta. N’incluez pas de secrets ou de données client sensibles.
