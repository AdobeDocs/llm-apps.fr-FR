---
title: Configurer Adobe Analytics et Customer Journey Analytics
description: Transférez les événements des applications Adobe LLM à votre propre suite de rapports Adobe Analytics et à votre propre vue de données Customer Journey Analytics via un flux de données Adobe Experience Platform.
source-git-commit: 557173e8d95325b980efac4d72c06bb3d90dcd72
workflow-type: tm+mt
source-wordcount: '2962'
ht-degree: 3%
---

# Configurer Adobe Analytics et Customer Journey Analytics {#analytics-setup}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

Envoyez les événements de votre application à [!DNL Adobe Analytics], [!DNL Customer Journey Analytics] ou les deux. [!DNL LLM Apps] transfère les événements à [!DNL Adobe Experience Platform] Edge Network par le biais d’un flux de données que vous détenez. Le flux de données achemine les événements vers chaque service que vous lui ajoutez.

- **[!DNL Adobe Analytics]** indique les actions que la plateforme LLM appelle, les liens de widget que les personnes sélectionnent et l’intention derrière chaque appel.
- **[!DNL Customer Journey Analytics]** analyse les mêmes événements par personne, dans les vues de données que vous créez à partir de données [!DNL Experience Platform].

Cette configuration est distincte des analyses intégrées dans [!DNL LLM Apps].

**Dans ce guide :** découvrez les paramètres requis par [!DNL LLM Apps] dans [!DNL Experience Platform], [!DNL Adobe Analytics] et [!DNL Customer Journey Analytics], ainsi que des liens vers la procédure Experience League pour chaque écran de produit. Ensuite, activez Analytics dans votre application, déployez-la dans l’environnement intermédiaire et vérifiez les données avant de la déployer dans l’environnement de production.

## Liste de contrôle de configuration {#setup-checklist}

Effectuez chaque étape marquée **Obligatoire** pour les services que vous utilisez. Une étape ignorée affiche rarement une erreur. Au lieu de cela, les données n’arrivent pas.

| Étape | [!DNL Adobe Analytics] | [!DNL Customer Journey Analytics] |
|---|---|---|
| [1. Créez l’espace de noms d’identité](#identity-namespace) | Requis | Requis |
| [2. Créez le schéma &#x200B;](#schema) | Requis | Requis |
| [3. Créez le jeu de données](#dataset) | Pas nécessaire | Requis |
| [4. Préparez la suite de rapports](#report-suite) | Requis | Pas nécessaire |
| [5. Créez le flux de données](#datastream) | Requis | Requis |
| [6. Activez Analytics et déployez vers l’environnement intermédiaire](#enable-analytics) | Requis | Requis |
| [7. Créez la connexion et la vue de données](#cja) | Pas nécessaire | Requis |
| [8. Vérifiez les données et déployez en production](#verify) | Requis | Requis |

Créez l’espace de noms, le schéma, le jeu de données et le flux de données dans le même sandbox [!DNL Experience Platform]. Utilisez le sandbox et le flux de données à partir desquels vous prévoyez de créer des rapports en production. Après avoir enregistré le flux de données dans votre application, contactez l’équipe applications LLM pour le modifier.

## Avant de commencer {#before-you-begin}

Vous avez besoin des éléments suivants :

- **Un runtime et SDK pris en charge.** Votre référentiel de gestionnaire doit utiliser `@adobe/llm-apps-runtime` version 1.2.0 ou ultérieure. Pour les widgets EDS, utilisez LLM Apps SDK 0.1.13 ou une version ultérieure. Pour mettre à jour une application plus ancienne vers le dernier runtime et SDK avant d’activer Analytics, suivez [&#x200B; Mise à niveau d’une application existante](/help/guides/upgrade-existing-app.md). Les applications qui répondent à ces exigences n’ont pas besoin d’être mises à niveau.
- **Les groupes de champs agentic.** Votre organisation doit être activée pour les groupes de champs Informations sur l’agence et Événement de conversation . Si ces groupes de champs n’apparaissent pas lorsque vous ajoutez des groupes de champs à un schéma, contactez votre équipe de compte Adobe. Voir [&#x200B; Implémentation d’Insights de conversation &#x200B;](https://experienceleague.adobe.com/en/docs/analytics-platform/using/conversation-insights/implement#schema-and-datasets). Les appels à l’action sont enregistrés dans le champ `agenticExperience.tools` des informations sur l’agence, qui se trouve dans Beta en [!DNL Experience Platform].
- **[!DNL Experience Platform]les autorisations** pour créer des espaces de noms d’identité, des schémas, des jeux de données et des flux de données dans votre sandbox.
- **Pour [!DNL Adobe Analytics] :** l’autorisation de gérer les suites de rapports et les règles de traitement. Les administrateurs de produit disposent des deux par défaut.
- **Par [!DNL Customer Journey Analytics] :** accès administrateur de produit pour créer des connexions et des vues de données.

## Ce que votre application envoie {#what-your-app-sends}

Après avoir activé Analytics et déployé, votre application envoie deux types d’événements :

| Événement | `eventType` | Envoyé lorsque | Inclut |
|---|---|---|---|
| Appel d&#39;action | `mcp.tool_call` | La plateforme LLM appelle l’une des actions de votre application, y compris les appels qui échouent ou renvoient une invite de connexion. | Identifiant du code de l’action et si l’appel a échoué. Intention de l’utilisateur ou de l’utilisatrice, lorsque l’action la collecte et que la plateforme en fournit une. Lorsque la plateforme les fournit : le nom de la plateforme, l’ID de conversation, l’ID utilisateur de la plateforme, les paramètres régionaux et l’organisation, ainsi que l’emplacement approximatif de l’utilisateur. |
| Interaction du widget | `web.webinteraction.linkClicks` | Quelqu’un sélectionne un lien ou un bouton dans un widget Edge Delivery Services. | Libellé du lien ou du bouton (100 caractères maximum). Lorsqu’il est fourni par l’appel d’action, son identifiant de conversation et son identifiant de tournant. Adobe Web SDK ajoute également l’URL du cadre du widget, la taille de la fenêtre de l’écran et du navigateur, l’heure et le fuseau horaire locaux, ainsi que sa propre version. |

En [!DNL Adobe Analytics], chaque appel d’action est comptabilisé comme une page vue d’une page nommée `mcp:<code identifier>` et chaque interaction de widget est comptabilisée comme un clic sur le lien. Voir [Types d’événements Edge Network dans Adobe Analytics](https://experienceleague.adobe.com/en/docs/analytics/implementation/aep-edge/hit-types).

Chaque appel d’action et chaque interaction de widget est un accès [!DNL Adobe Analytics] et un événement dans votre jeu de données [!DNL Experience Platform]. Sur une application à trafic élevé, vérifiez que votre contrat couvre ce volume avant d’activer Analytics.

Gardez ces détails à l’esprit lorsque vous lisez les rapports :

- **Les appels ayant échoué incluent les invites de connexion.** Si certaines actions nécessitent une connexion, attendez-vous à un taux d’échec plus élevé.
- **Certains appels rejetés ne sont pas enregistrés.** Un appel est rejeté avant l’exécution de l’action et n’envoie aucun événement lorsqu’une plateforme autre que [!DNL ChatGPT] appelle une action qui nécessite une connexion sans connexion valide ou sans la portée requise. Sur les applications où chaque action nécessite une connexion, tout appel sans connexion est rejeté de la même manière.
- **L’intention de l’utilisateur est souvent absente.** La plateforme décide de la fournir ou non. De ce fait, de nombreux appels n’en comportent aucun et apparaissent comme **Non spécifié**.
- **Les détails de la plateforme varient.** Actuellement, [!DNL ChatGPT] les fournit toutes. Par [!DNL Claude], seul le nom de la plateforme est enregistré.
- **Les libellés de widget proviennent de la page.** Pour définir un libellé stable, ajoutez un attribut `data-analytics` au lien ou au bouton. Dans le cas contraire, son texte visible est utilisé.

Pour choisir les actions qui collectent l’intention de l’utilisateur, voir [Configurer les analyses](/help/guides/create-action.md#configure-analytics).

>[!NOTE]
>
>Les événements peuvent inclure un identifiant utilisateur de plateforme et un emplacement approximatif. Consultez votre politique de confidentialité et les paramètres de gouvernance des données avant d’activer Analytics.

### Comment les événements identifient les personnes {#identities}

| Événement | Identité du Principal | Espace de noms |
|---|---|---|
| Appel d&#39;action | ID utilisateur de la plateforme LLM, lorsque la plateforme en fournit un | `MCPHOSTUSER` |
| Interaction du widget | Identifiant Experience Cloud (ECID) émis par Edge Network lors de l’appel d’action. L’ID de `MCPHOSTUSER` est inclus comme identité secondaire lorsqu’il est disponible. | `ECID` |

Certaines plateformes, telles que [!DNL Claude], ne fournissent pas d’identifiant utilisateur. Les appels d’action de ces plateformes ne comportent aucune identité, donc [!DNL Customer Journey Analytics] les ignore.

>[!NOTE]
>
>Chaque appel d’action obtient un nouvel ECID, et une interaction de widget utilise l’ECID de l’appel d’action qui a affiché le widget. Par conséquent :
>
>- [!DNL Customer Journey Analytics] compte les appels à l’action d’un utilisateur comme une seule personne, mais compte ses interactions de widget comme une personne différente pour chaque appel à l’action.
>- [!DNL Adobe Analytics] pouvez comptabiliser chaque appel d’action comme un nouveau visiteur.

## Étape 1 : créer l’espace de noms d’identité {#identity-namespace}

**Obligatoire pour :** [!DNL Adobe Analytics] et [!DNL Customer Journey Analytics].

Les appels d’action identifient des personnes dans l’espace de noms `MCPHOSTUSER`. L’espace de noms doit donc exister dans votre sandbox. Créez un espace de noms personnalisé avec les paramètres suivants :

| Configuration | Valeur |
|---|---|
| Nom d’affichage | `MCP host user` ou un autre nom de votre choix |
| Symbole d’identité | `MCPHOSTUSER`, exactement comme indiqué |
| Type d’identité | **[!UICONTROL Identifiant individuel sur plusieurs appareils]** |

Pour la procédure, voir [Création d’espaces de noms personnalisés](https://experienceleague.adobe.com/en/docs/experience-platform/identity/features/namespaces#create-namespaces).

## Étape 2 : créer le schéma {#schema}

**Obligatoire pour :** [!DNL Adobe Analytics] et [!DNL Customer Journey Analytics].

Le flux de données et le jeu de données utilisent tous deux ce schéma. Créez un schéma avec les paramètres suivants :

| Configuration | Valeur |
|---|---|
| Type de schéma | **[!UICONTROL Standard]**, créé avec l’option **[!UICONTROL Manuel]** |
| Classe de base | **[!UICONTROL Événement d’expérience]** |
| Groupes de champs | Informations sur l’agence, événement de conversation, détails web, détails de l’environnement et détails d’implémentation |
| Nom d’affichage | Tout nom, par exemple `AEM LLM Apps` |

>[!IMPORTANT]
>
>Ajoutez les cinq groupes de champs . [!DNL LLM Apps] nécessite des informations sur l’agence et un événement de conversation pour les appels à l’action. Les trois autres contiennent les champs de page, de lien, d’appareil, d’environnement et d’implémentation que les événements incluent également. Si un groupe de champs est manquant, [!DNL Experience Platform] pouvez supprimer ces champs ou rejeter l’événement.

Vous n’avez pas besoin du groupe de champs Adobe Analytics ExperienceEvent. L’intention de l’utilisateur atteint [!DNL Adobe Analytics] par le biais d’une règle de traitement. Voir [Étape 4](#report-suite).

Pour la procédure, voir [Création d’un schéma](https://experienceleague.adobe.com/en/docs/experience-platform/xdm/ui/resources/schemas#create) et [Ajout de groupes de champs](https://experienceleague.adobe.com/en/docs/experience-platform/xdm/ui/resources/schemas#add-field-groups).

## Étape 3 : créer le jeu de données {#dataset}

**Obligatoire pour :** [!DNL Customer Journey Analytics].

Le jeu de données stocke les événements de votre application dans [!DNL Experience Platform]. Créez un jeu de données avec les paramètres suivants :

| Configuration | Valeur |
|---|---|
| Méthode de création | **[!UICONTROL Créer un jeu de données à partir d’un schéma]** |
| Schéma | Le schéma de [étape 2](#schema) |
| Nom | Tout nom, par exemple `AEM LLM Apps` |

Pour connaître la procédure à suivre, voir [Créer un jeu de données](https://experienceleague.adobe.com/en/docs/experience-platform/catalog/datasets/user-guide#create).

## Étape 4 : préparation de la suite de rapports {#report-suite}

**Obligatoire pour :** [!DNL Adobe Analytics].

[!DNL Adobe Analytics] mappe automatiquement le nom de la page et les champs de lien afin que les pages `mcp:<code identifier>` et les clics sur les liens du widget n’aient pas besoin d’une configuration supplémentaire. L’intention de l’utilisateur a besoin d’une eVar et d’une règle de traitement.

### Créer ou choisir une suite de rapports {#create-report-suite}

Créez une suite de rapports ou choisissez-en une existante. Notez l’identifiant complet de la suite de rapports, y compris le préfixe ajouté par [!DNL Adobe Analytics]. Vous avez besoin de l’identifiant à [étape 5](#datastream).

Pour connaître la procédure à suivre, voir [Créer une suite de rapports](https://experienceleague.adobe.com/fr/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite).

### Activation de l’eVar des intentions de l’utilisateur {#user-intent-evar}

Dans les variables de conversion de la suite de rapports, activez une eVar avec les paramètres suivants :

| Configuration | Valeur |
|---|---|
| eVar | eVar3 |
| Nom | `User Intent` |
| Type | **[!UICONTROL Chaîne de texte]** |
| Allocation | **[!UICONTROL Le plus récent (dernier)]** |
| Expire après | **[!UICONTROL Visite]** |
| Statut | **[!UICONTROL Activé]** |

Si la suite de rapports utilise déjà eVar3, choisissez une autre eVar inutilisée et utilisez cette eVar dans la règle de traitement.

Pour connaître la procédure à suivre, voir [Ajouter ou modifier des variables de conversion](https://experienceleague.adobe.com/en/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin#add-or-edit-conversion-variables).

### Création de la règle de traitement {#processing-rule}

[!DNL Adobe Analytics] reçoit l’intention de l’utilisateur sous la forme d’une variable de données contextuelles, et non d’eVar. Pour en savoir plus, voir [Mappage implicite](https://experienceleague.adobe.com/en/docs/analytics/implementation/aep-edge/xdm-var-mapping#implicit-mapping). Créez une règle de traitement qui copie l’intention de l’utilisateur dans eVar :

- **Condition :** la variable de données contextuelles est `a.x.conversation.prompt.raw.0.purpose` égale à `intents`.
- **Action :** remplacez la valeur de **User Intent (eVar3)** par la variable de données contextuelles `a.x.conversation.prompt.raw.0.text`.

Sans cette règle, la dimension **Intention de l’utilisateur** reste vide. La règle s’applique uniquement aux données collectées après leur enregistrement. Les interactions de widget ne comportent aucune intention de l’utilisateur, la règle ne les modifie donc pas.

D’autres champs, tels que l’échec de l’appel ou le nom de la plateforme, arrivent également en tant que variables de données contextuelles. Pour générer des rapports à leur sujet dans [!DNL Adobe Analytics], créez d’autres règles de traitement de la même manière.

Pour connaître la procédure à suivre, reportez-vous aux sections [Présentation des règles de traitement](https://experienceleague.adobe.com/en/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/report-suite-general/processing-rules/pr-overview) et [Cas d’utilisation des règles de traitement](https://experienceleague.adobe.com/en/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/report-suite-general/processing-rules/pr-use-cases).

## Étape 5 : créer le flux de données {#datastream}

**Obligatoire pour :** [!DNL Adobe Analytics] et [!DNL Customer Journey Analytics].

Le flux de données reçoit les événements de votre application et les transfère à chaque service que vous ajoutez.

### Créer le flux de données {#create-datastream}

Créez un flux de données avec les paramètres suivants :

| Configuration | Valeur |
|---|---|
| Nom | Tout nom, par exemple `AEM LLM Apps` |
| Schéma de mapping | Le schéma de [étape 2](#schema) |

Créez le flux de données dans le même sandbox que le schéma. Vous ne pouvez pas modifier le sandbox d’un flux de données après l’avoir créé.

Pour connaître la procédure à suivre, voir [Créer un flux de données](https://experienceleague.adobe.com/en/docs/experience-platform/datastreams/configure#create).

### Ajouter les services {#add-services}

Ajoutez un service pour chaque destination :

| Service | Paramètres | Obligatoire pour |
|---|---|---|
| **&#x200B;**&#x200B;| **[!UICONTROL Activé]**. Dans **[!UICONTROL Identifiant de suite de rapports]**, saisissez l’identifiant de l’[étape 4](#report-suite). | [!DNL Adobe Analytics] |
| **&#x200B;**&#x200B;| **[!UICONTROL Activé]**. Dans **[!UICONTROL Jeu de données d’événement]**, sélectionnez le jeu de données à l’[étape 3](#dataset). Laissez les autres options désactivées. | [!DNL Customer Journey Analytics] |

![Ajouter un service - Adobe Analytics activé avec un exemple d’identifiant de suite de rapports](/help/assets/guide-analytics/datastream-analytics-service.png)

![service Adobe Experience Platform : jeu de données d’événement sélectionné et autres options effacées](/help/assets/guide-analytics/datastream-platform-service.png)

Pour la procédure, voir [Ajouter des services à un flux de données](https://experienceleague.adobe.com/en/docs/experience-platform/datastreams/configure#add-services), [paramètres d’Adobe Analytics](https://experienceleague.adobe.com/en/docs/experience-platform/datastreams/configure#analytics) et [paramètres de Adobe Experience Platform](https://experienceleague.adobe.com/en/docs/experience-platform/datastreams/configure#aep).

Copiez ensuite l’**[!UICONTROL identifiant du flux de données]** à partir des détails du flux de données. Vous en avez besoin à [étape 6](#enable-analytics).

![Détails du flux de données - Services Adobe Analytics et Adobe Experience Platform activés, avec l’exemple d’identifiant de flux de données et le schéma de mappage dans le panneau latéral](/help/assets/guide-analytics/datastream-services.png)

## Étape 6 : activer Analytics et déployer dans l’environnement intermédiaire {#enable-analytics}

**Obligatoire pour :** [!DNL Adobe Analytics] et [!DNL Customer Journey Analytics].

>[!IMPORTANT]
>
>Après l’enregistrement, le flux de données est en lecture seule dans les paramètres de l’application. Contactez l’équipe en charge des applications LLM pour la modifier ou désactiver Analytics. Vérifiez l’identifiant du flux de données avant d’enregistrer. Vous pouvez toujours modifier la suite de rapports et le jeu de données dans le flux de données lui-même.

1. Ouvrez votre application et sélectionnez **[!UICONTROL Paramètres]** dans le volet de navigation de gauche.
2. Sélectionnez l’onglet **[!UICONTROL Analytics]**.
3. Activez **[!UICONTROL Activer Analytics]**.
4. Dans **[!UICONTROL Identifiant du flux de données]**, collez l’identifiant à partir de [Étape 5](#datastream).
5. Sous **[!UICONTROL Conditions requises]**, sélectionnez **[!UICONTROL Afficher les détails]** et vérifiez que votre application répond à chaque condition.

   ![Paramètres d’application, onglet Analytics - Activation d’Analytics, champ d’identifiant de flux de données vide et liste des exigences développée](/help/assets/guide-analytics/analytics-settings.png)

6. Sélectionnez **[!UICONTROL Enregistrer les modifications]**.
7. [Déployer l’application](/help/guides/deploy-your-app.md) dans l’environnement d’évaluation. Le paramètre prend effet lors du déploiement suivant dans chaque environnement.

>[!NOTE]
>
>Le flux de données s’applique à l’ensemble de l’application. Les environnements d’évaluation et de production envoient des événements au même flux de données et les événements n’enregistrent pas l’environnement qui les a envoyés. Vos appels de test dans l’environnement intermédiaire apparaissent dans les mêmes rapports que le trafic de production.

## Étape 7 : Créer la connexion et la vue de données {#cja}

**Obligatoire pour :** [!DNL Customer Journey Analytics].

### Créer la connexion {#connection}

Créez une connexion et ajoutez le jeu de données de l’[étape 3](#dataset) avec les paramètres suivants :

| Configuration | Valeur |
|---|---|
| Sandbox | Sandbox contenant votre jeu de données |
| Nombre moyen d’événements quotidiens | Plage correspondant au volume attendu |
| Jeu de données | Le jeu de données de [Étape 3](#dataset) |
| ID de personne | **[!UICONTROL Carte des identités]** |
| Utiliser l’espace de noms d’identité principal | Sélectionné |
| Date et heure | **[!UICONTROL Horodatage]** |
| Type de source de données | **[!UICONTROL Autres]** |
| Description de la source de données | Toute description, par exemple `LLM Apps events` |
| Importer de nouvelles données | **[!UICONTROL Importer toutes les nouvelles données]** activé |
| Renvoi du jeu de données | **[!UICONTROL Renvoyer toutes les données existantes]** activé si le jeu de données contient déjà des événements à analyser |

>[!IMPORTANT]
>
>Activez **[!UICONTROL Importer toutes les nouvelles données]**. Ce paramètre crée une connexion continue, de sorte que de nouveaux événements sont automatiquement disponibles dans Workspace.

Avec cet ID de personne, [!DNL Customer Journey Analytics] utilise l’identité principale de chaque événement comme personne et ignore les événements qui n’ont pas d’identité principale. Voir [&#x200B; Comment les événements identifient les personnes &#x200B;](#identities).

Pour connaître la procédure à suivre, reportez-vous aux sections [Créer une connexion](https://experienceleague.adobe.com/en/docs/analytics-platform/using/cja-connections/create-connection#create-connection), [Paramètres du jeu de données d’événement](https://experienceleague.adobe.com/en/docs/analytics-platform/using/cja-connections/create-connection#event-dataset-settings) et [Utiliser le mappage d’identité comme ID de personne](https://experienceleague.adobe.com/en/docs/analytics-platform/using/cja-connections/create-connection#id-map).

### Création de la vue de données {#data-view}

Créez une vue de données sur la connexion avec les paramètres suivants :

| Configuration | Valeur |
|---|---|
| Connexion | La connexion que vous venez de créer |
| Nom | Tout nom, par exemple `LLM Apps` |
| Composants | Dimensions et mesures pour lesquelles vous souhaitez créer des rapports. Voir le tableau ci-dessous. |

Ajoutez ces champs de schéma en tant que composants :

| Rapport sur | Champ de schéma | Ajouter en tant que |
|---|---|---|
| Action | `agenticExperience.tools.name` | Dimension |
| Appels d’action ayant échoué | `agenticExperience.tools.failed` | Dimension |
| Raison de l’échec | `agenticExperience.tools.errorReason` | Dimension |
| Plateforme | `agenticExperience.agents.name` | Dimension |
| Intention de l’utilisateur | `conversation.prompt.raw.text` | Dimension |
| Conversation | `conversation.conversationID` | Dimension |
| Libellé du widget | `web.webInteraction.name` | Dimension |

Ajoutez des champs de texte, tels que l’intention de l’utilisateur, sous forme de dimensions. Un champ de texte ajouté en tant que mesure ne peut être comptabilisé que, et la ventilation en fonction de ses valeurs indique **Non valide**.

Pour connaître la procédure à suivre, voir [Configurer une vue de données](https://experienceleague.adobe.com/en/docs/analytics-platform/using/cja-dataviews/create-dataview#configure) et [Composants](https://experienceleague.adobe.com/en/docs/analytics-platform/using/cja-dataviews/create-dataview#components).

## Étape 8 : vérifier les données et déployer en production {#verify}

**Obligatoire pour :** [!DNL Adobe Analytics] et [!DNL Customer Journey Analytics].

Appelez une action dans votre application d’évaluation à partir d’une plateforme LLM prise en charge, puis sélectionnez un lien dans son widget. Voir [Test dans le ChatGPT](/help/guides/test-in-chatgpt.md) ou [Test dans Claude](/help/guides/test-in-claude.md). Vérifiez ensuite chaque service que vous avez configuré :

| Service | Où vérifier | À quoi s’attendre ? |
|---|---|---|
| [!DNL Experience Platform] | **[!UICONTROL Activité du jeu de données]** pour le jeu de données à partir de [Étape 3](#dataset) | Nouveaux lots ingérés et aucun lot en échec |
| [!DNL Adobe Analytics] | Un projet Workspace qui utilise la suite de rapports de l’[étape 4](#report-suite), avec un tableau à structure libre | **[!UICONTROL Page]** lignes nommées `mcp:<code identifier>`, **[!UICONTROL Lien personnalisé]** lignes pour les interactions de widget et texte d’intention dans la dimension **Intention de l’utilisateur** |
| [!DNL Customer Journey Analytics] | Un projet Workspace qui utilise la vue de données de [étape 7](#cja) | **[!UICONTROL Événements]** pour vos appels d’action et interactions de widgets |

Chaque service traite les données avec un certain retard, de sorte que les nouveaux événements n’apparaissent pas immédiatement.

Pour créer les rapports, voir [Création de projets dans Adobe Analytics](https://experienceleague.adobe.com/en/docs/analytics/analyze/analysis-workspace/build-workspace-project/create-projects), [Présentation des tableaux à structure libre](https://experienceleague.adobe.com/en/docs/analytics/analyze/analysis-workspace/visualizations/freeform-table/freeform-table) et [Création de projets dans Customer Journey Analytics](https://experienceleague.adobe.com/en/docs/analytics-platform/using/cja-workspace/build-workspace-project/create-projects).

Lorsque les données s’affichent correctement, [déployez votre application](/help/guides/deploy-your-app.md) en production.

## Restrictions {#limitations}

- **Les interactions de widget sont divisées en plusieurs personnes.** Chaque appel d’action obtient un nouvel ECID. [!DNL Customer Journey Analytics] comptabilise donc les interactions de widgets de différents appels d’action comme différentes personnes. Voir [&#x200B; Comment les événements identifient les personnes &#x200B;](#identities).
- **Aucun lien vers votre site web.** Lorsqu’une personne suit un lien de widget vers votre site web, la visite du site web n’est pas liée à la conversation.
- **Rapports de partage d’évaluation et de production.** Les deux environnements envoient des données au même flux de données et les événements n’enregistrent pas l’environnement qui les a envoyés.
- **Certaines plateformes n’envoient aucune identité.** Les appels d’action des plateformes qui ne fournissent pas d’ID utilisateur, comme [!DNL Claude], sont ignorés dans [!DNL Customer Journey Analytics].
- **Un tour par appel d’action.** Chaque appel d’action est enregistré comme son propre tour de conversation. Ainsi, une invite qui déclenche plusieurs actions compte comme plusieurs tours.

## Résolution des problèmes {#troubleshooting}

| Ce que vous voyez | Éléments à vérifier |
|---|---|
| Le jeu de données ne reçoit aucun lot | Vous avez déployé l’application après avoir enregistré les paramètres Analytics dans l’environnement que vous testez. L’identifiant du flux de données est correct. Le flux de données contient le service **&#x200B;**&#x200B;avec votre jeu de données. |
| Échec des lots | Le schéma comprend les cinq groupes de champs de l’[étape 2](#schema). |
| [!DNL Adobe Analytics] n’a pas de données | Le service **&#x200B;**&#x200B;est activé pour le flux de données avec l’identifiant correct de la suite de rapports. |
| **Page** affiche les `mcp:<code identifier>`, mais **Intention de l’utilisateur** et les champs d’action sont vides | Votre référentiel de gestionnaire utilise `@adobe/llm-apps-runtime` version 1.2.0 ou ultérieure et vous l’avez déployé après la mise à niveau. Voir [Avant de commencer](#before-you-begin). |
| **Intention de l’utilisateur** est vide pour certaines actions. | La règle de traitement de l’[étape 4](#processing-rule) utilise les variables de données contextuelles exactement comme indiqué, en minuscules, et dans l’eVar appropriée. L’action **[!UICONTROL Collecter l’intention de l’utilisateur]** est activée. La règle s’applique uniquement aux données collectées après leur enregistrement. |
| [!DNL Customer Journey Analytics] affiche moins d’événements que le jeu de données | L&#39;option **[!UICONTROL Importer toutes les nouvelles données]** est activée. Les événements sans identité principale sont ignorés, par exemple les appels d’action des plateformes qui ne fournissent pas d’identifiant utilisateur. |
| Vous ne pouvez pas modifier l’identifiant du flux de données ni désactiver Analytics | Ces paramètres sont en lecture seule dans les paramètres de l’application après activation. Contactez l’équipe LLM Apps pour obtenir de l’aide. Pour acheminer les données ailleurs, modifiez la suite de rapports ou le jeu de données dans le flux de données lui-même. |

## Prochaines étapes {#whats-next}

- [Configuration des analyses pour une action](/help/guides/create-action.md#configure-analytics)
- [Déploiement de l’application](/help/guides/deploy-your-app.md)
