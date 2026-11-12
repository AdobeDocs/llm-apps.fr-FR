---
title: Configuration de l’infrastructure
description: Créez une application Adobe LLM à l’aide de l’infrastructure gérée par Adobe ou fournissez deux référentiels GitHub vides que la plateforme doit initialiser.
source-git-commit: 079eb2403f15a97842c5c336c03ec5bfe10de622
workflow-type: tm+mt
source-wordcount: '1595'
ht-degree: 0%
---

# Configuration de l’infrastructure {#create-first-app}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

L’infrastructure est la base technique de votre application : les référentiels qui stockent son code et les services qui exécutent ses gestionnaires d’action et diffusent ses widgets aux utilisateurs.

Comme expliqué dans la [présentation](/help/overview/overview.md#key-concepts), un **gestionnaire d’action** effectue une tâche, telle que la recherche de produits, tandis qu’un **widget** présente le résultat sous la forme d’une expérience interactive dans la conversation.

[!DNL LLM Apps] stocke le code source du gestionnaire et du widget dans deux référentiels [!DNL GitHub] distincts :

- **Référentiel d&#39;actions** — contient les gestionnaires côté serveur et leurs tests.
- **Référentiel de widgets** — contient le code et les styles pour les widgets interactifs.

Par exemple, un gestionnaire de recherche de produits récupère les produits correspondants et son widget les affiche sous forme de cartes de produits. Les référentiels stockent ce code ; Adobe I/O Runtime exécute les gestionnaires déployés et Edge Delivery Services diffuse les widgets.

L’assistant de configuration prépare ces référentiels et connecte les services dont votre application a besoin. Nous vous recommandons de commencer par **l’infrastructure gérée par Adobe**, la manière la plus rapide de commencer. Adobe crée et héberge les référentiels pour vos gestionnaires d’action et widgets. Vous n’avez pas besoin de configurer vous-même les référentiels.

Vous pouvez également fournir deux référentiels [!DNL GitHub] vides dans votre propre compte ou organisation. La plateforme les initialise avec les fichiers de projet requis ; il n’est pas nécessaire de préparer au préalable l’action ou le code du widget.

**Dans ce guide :** Découvrez comment choisir une option d&#39;infrastructure et créer votre application à l&#39;aide de l&#39;assistant de configuration. Après la configuration, passez à [Créer des actions](/help/guides/creating-actions.md).

La création de l’application ne lance **pas** la génération d’actions. Démarrez la génération de l’action séparément de la page de l’application.

## Ouvrir l’assistant de création d’application

Ouvrez les [applications Adobe LLM](https://experience.adobe.com/llm-apps). Sur la page d’accueil, sélectionnez **[!UICONTROL Commencer]**. Si une application est déjà ouverte, utilisez le sélecteur d’applications et sélectionnez **[!UICONTROL Créer une application]**.

L&#39;assistant **[!UICONTROL Créer votre application LLM]** commence par **[!UICONTROL Commencer]**. Choisissez comment configurer l’environnement technique.

![Créer votre application LLM : choisissez entre une infrastructure gérée par Adobe et la connexion de vos propres référentiels.](/help/assets/guide-infrastructure/setup-choice.png)

| Option d’infrastructure | Sélectionner dans l’assistant | Ce que cela signifie |
|---|---|---|
| **Infrastructure gérée par Adobe** | **[!UICONTROL Laissons-nous nous en occuper]** | Adobe crée et gère les référentiels et les services connectés pour votre application. Un accès au code est disponible sur demande. |
| **Vos propres référentiels** | **[!UICONTROL Je vais le configurer moi-même]** | Vous fournissez deux référentiels GitHub vides et accordez l’accès via l’assistant. La plateforme initialise l’un pour les gestionnaires d’action et l’autre pour les widgets. |

Les deux options prennent en charge la génération automatique d’actions ultérieurement. Aucune des deux options ne nécessite que vous écriviez des actions manuellement.

## Utilisation de l’infrastructure gérée par Adobe {#adobe-managed-infrastructure}

Utilisez cette option pour permettre à Adobe de configurer et de gérer les référentiels et les services connectés pour votre application. Vous n’avez pas besoin de créer de référentiels ni d’installer vous-même les applications [!DNL GitHub].

1. Sélectionnez **[!UICONTROL Laissons-nous gérer]**.
2. Sélectionnez **[!UICONTROL Étape suivante]**.
3. Complétez les [détails de l&#39;application](#app-details).
4. Sélectionnez **[!UICONTROL Créer Une Application]**.

La page de l’application affiche **[!UICONTROL Configuration de l’application]** pendant qu’Adobe prépare les référentiels et connecte le site du widget. L’interface utilisateur indique que la configuration prend généralement entre 1 et 2 minutes ; les actions, les tests et le déploiement ne sont pas disponibles tant que la configuration n’est pas prête.

![Configuration de l&#39;application — Adobe prépare l&#39;infrastructure de l&#39;application](/help/assets/guide-infrastructure/provisioning.png)

Si la configuration échoue, lisez l’erreur et sélectionnez **[!UICONTROL Réessayer]**. Si la page ne peut pas vérifier la progression, l’installation se poursuit en arrière-plan ; sélectionnez **[!UICONTROL Vérifier à nouveau]** pour vous reconnecter.

Une fois la configuration terminée, continuez avec [Création d’actions](/help/guides/creating-actions.md).

## Connecter vos propres référentiels {#your-own-repositories}

Sélectionnez **[!UICONTROL Je vais le configurer moi-même]**, puis **[!UICONTROL Étape suivante]**. Ce chemin d’accès comporte quatre étapes d’assistant :

**Prise en main → Configuration de l&#39;infrastructure Action → Configuration de l&#39;infrastructure Widget → Détails de l&#39;application.**

Vous devez accéder aux référentiels sélectionnés et disposer des autorisations nécessaires pour installer ou configurer les applications [!DNL GitHub] requises. Pour les référentiels appartenant à l’organisation, un administrateur peut avoir besoin d’approuver l’accès.

Créez deux référentiels vides : un pour les gestionnaires d’action et un pour les widgets. Ne pas initialiser avec un fichier README, une licence ou `.gitignore`. La plateforme ajoute la structure de projet requise lorsque vous validez chaque référentiel dans l’assistant.

Si vous disposez déjà de projets d’action et EDS compatibles, vous pouvez les connecter à la place, mais le code existant n’est pas requis.

>[!NOTE]
>
>Pour un référentiel vide, la validation écrit les fichiers de projet initiaux. Sélectionnez les référentiels destinés à cette application avant de sélectionner **[!UICONTROL Valider le référentiel]**.

### Configurer l’infrastructure d’action

Le référentiel d’actions contient les tests et gestionnaires côté serveur qui s’exécutent lorsqu’une plateforme LLM appelle une action.

1. Sous **[!UICONTROL Source des actions]**, sélectionnez **[!UICONTROL GitHub]**. Les autres sources de code ne sont actuellement pas disponibles.
2. Réponse **[!UICONTROL Avez-vous déjà créé un référentiel GitHub ?]** :
   - Sélectionnez **[!UICONTROL Oui, j&#39;utiliserai un référentiel déjà créé]** pour connecter un projet existant compatible ou un référentiel vide.
   - Sélectionnez **[!UICONTROL Je n’ai pas encore de référentiel]**, puis **[!UICONTROL Créez des référentiels sur GitHub]** pour créer un référentiel d’action vide. Revenez ensuite à l’assistant.
3. Connectez le référentiel à l’application Adobe LLM Apps [!DNL GitHub] :
   - Si aucune organisation n’est connectée, sélectionnez **[!UICONTROL Installer sur GitHub]**.
   - Si une organisation est déjà connectée, sélectionnez **[!UICONTROL Connecter le référentiel]** et ajoutez le référentiel à cette installation.
4. Sur [!DNL GitHub], choisissez **[!UICONTROL Sélectionner uniquement les référentiels]** et accorder l’accès uniquement aux référentiels utilisés par cette application.
5. Revenez à l’assistant. Sous **[!UICONTROL Sélectionner le référentiel]**, choisissez **[!UICONTROL Organisation]** et **[!UICONTROL Référentiel]**.
6. Sélectionnez **[!UICONTROL Valider le référentiel]**. La validation initialise un référentiel vide avec le modèle standard Applications LLM Adobe . Un référentiel existant doit contenir un projet d’actions compatible.
7. Une fois la validation réussie, sélectionnez **[!UICONTROL Étape suivante]**.

![Configuration de l’infrastructure d’action — choisissez GitHub et connectez un référentiel d’action](/help/assets/guide-infrastructure/action-repository.png)

Si une organisation ou un référentiel est manquant, utilisez **[!UICONTROL Connecter une autre organisation]** ou **[!UICONTROL Connecter un autre référentiel]** dans son sélecteur. Après avoir modifié l’accès sur [!DNL GitHub], sélectionnez **[!UICONTROL Actualiser les organisations et les référentiels]**.

### Configurer l’infrastructure du widget

Les widgets sont des réponses visuelles facultatives jointes à des actions, telles que des cartes de produits ou des formulaires de réservation. L’assistant connecte un référentiel [!DNL Edge Delivery Services] (EDS) pour les héberger.

Utilisez le même compte Adobe pour [!DNL LLM Apps], l’accès au site AEM et l’accès en écriture aux données numériques.

#### Connecter le référentiel de widgets {#connect-widget-repository}

1. Sous **[!UICONTROL Source du widget]**, sélectionnez **[!UICONTROL Sélectionner manuellement le référentiel du widget]**.
2. Sous **[!UICONTROL Source de code]**, sélectionnez **[!UICONTROL GitHub]**.
3. Choisissez si vous disposez déjà d’un référentiel. Si ce n’est pas le cas, sélectionnez **[!UICONTROL Créer des référentiels sur GitHub]**, créez un référentiel de widget vide, puis revenez à l’assistant.
4. Installez ou configurez l’application Adobe LLM Apps [!DNL GitHub] pour ce référentiel, comme vous l’avez fait pour le référentiel d’action.
5. Sélectionnez son **[!UICONTROL organisation]** et son **[!UICONTROL référentiel]**. Ne sélectionnez pas le référentiel d’actions.
6. Sélectionnez **[!UICONTROL Valider le référentiel]**. La validation initialise un référentiel vide avec le modèle AEM. Un référentiel existant doit être un projet EDS compatible.

#### &#x200B;7. Configurer l’accès au site AEM {#aem-site-access}

**Nouvelle installation :** Sélectionnez **[!UICONTROL Installer la synchronisation du code AEM]** et octroyez-lui l’accès au référentiel de widgets. Terminez l’assistant de configuration sur la page de confirmation de l’installation pour vous ajouter en tant qu’administrateur du site.

**Site EDS existant :** Conservez l’installation de la synchronisation du code AEM et vérifiez qu’elle couvre le référentiel de widgets sélectionné. Demandez à un administrateur de site existant d’ouvrir AEM User Admin à l’adresse `https://tools.aem.live/tools/user-admin/index.html/?org=<org>&site=<site>` et d’ajouter votre compte Adobe avec le rôle `admin`. Vous n’avez pas besoin de réinstaller la synchronisation du code.

Remplacez `<org>` et `<site>` par les noms de votre organisation et de votre site AEM. L’administrateur de votre site peut confirmer ces valeurs.

#### &#x200B;8. Accorder un accès en écriture aux données numériques {#da-write-access}

Pour les sites nouveaux et existants, demandez à un administrateur d’organisation DA de :

1. Ouvrez la configuration DA à l’adresse `https://da.live/config#/<org>/`, en remplaçant `<org>` par l’organisation DA qui stocke le contenu de votre site.
2. Ouvrez la feuille **autorisations**. S’il n’existe pas, sélectionnez **[!UICONTROL Ajouter une feuille]**, nommez-la `permissions` et ajoutez les en-têtes de colonne **chemin**, **groupes** et **actions**. Vous pouvez également ajouter une colonne **commentaires** pour décrire chaque règle.
3. Ajoutez une ligne accordant à votre compte Adobe l’accès en écriture à tout le contenu du site : utilisez le chemin de contenu du site dans **chemin**, l’adresse e-mail de votre compte Adobe dans **groupes** et `write` dans **actions**. Enregistrez la configuration.

   Feuille d’autorisations ![DA montrant des exemples de comptes dans la colonne groupes et un accès en écriture dans la colonne actions](/help/assets/guide-infrastructure/da-write-permissions.png)

   La capture d’écran utilise des comptes fictifs. Remplacez `widget-author@example.com` par l’e-mail de votre compte Adobe et utilisez le chemin d’accès au contenu de votre site au lieu de l’exemple `/ + **` à l’échelle de l’organisation. La ligne `CONFIG` est destinée aux administrateurs de configuration ; vous n’avez pas besoin de la modifier pour accorder l’accès au contenu.

Les rôles de site AEM n’accordent pas l’accès en écriture DA. Vous n’avez pas besoin d’un accès en écriture à l’échelle de l’organisation.

Pour plus d&#39;informations sur la configuration des autorisations DA, voir [Autorisations et sécurité](https://www.aem.live/docs/ew/administering/permissions).

#### &#x200B;9. Validation de la configuration d’AEM {#validate-aem-setup}

1. Revenez à [!DNL LLM Apps] et sélectionnez **[!UICONTROL Valider la configuration AEM]**.
2. Lorsque **[!UICONTROL Configuration d’AEM vérifiée]** apparaît, sélectionnez **[!UICONTROL Étape suivante]**.

Si la validation échoue, suivez le message pour vérifier [l’accès au site AEM](#aem-site-access) ou l’accès en écriture [DA](#da-write-access), puis validez à nouveau.

>[!NOTE]
>
>L’assistant marque **[!UICONTROL Utiliser Cloud Manager Edge Delivery Services]** comme **[!UICONTROL Bientôt disponible]** et n’autorise pas la sélection. Utilisez l’option manuelle widget-repository .

## Saisir les détails de l’application {#app-details}

Les deux options d’infrastructure se terminent par **[!UICONTROL Détails de l’application]**.

![Détails de l&#39;application — saisissez un nom d&#39;application, sélectionnez un programme AEM et choisissez la région de données analytics](/help/assets/guide-infrastructure/app-details.png)

1. Saisissez le **[!UICONTROL nom de l&#39;application LLM]**. Utilisez un nom qui identifie la marque ou l&#39;expérience, tel que *Café Frescopa*.
2. Sélectionnez le **[!UICONTROL programme AEM]** auquel l’application appartient. Si vous n’avez accès qu’à un seul programme, l’assistant le sélectionne automatiquement.
3. Sous **[!UICONTROL Région de données Analytics]**, choisissez **[!UICONTROL États-Unis (US)]** ou **[!UICONTROL Europe (UE)]**.
4. Sélectionnez **[!UICONTROL Créer Une Application]**.

>[!IMPORTANT]
>
>La région Programme AEM et données d’analyse ne peut pas être modifiée une fois l’application créée. La région Analytics détermine l’emplacement de stockage des données d’analyse de l’application.

Si aucun programme AEM n’est disponible, demandez l’accès à votre administrateur. Si le chargement des programmes échoue, sélectionnez **[!UICONTROL Réessayer de charger les programmes]**.

L’assistant ouvre la page de l’application après sa création. Avec l’infrastructure gérée par Adobe, attendez la fin de la configuration de l’infrastructure avant de continuer.

## Ensuite : créez les actions

Votre application dispose désormais de son infrastructure, mais aucune action générée pour le moment. Continuez avec [Création d’actions](/help/guides/creating-actions.md) pour analyser un site web ou choisissez la création manuelle d’actions.

Pour connaître la relation entre les métadonnées d&#39;action, les gestionnaires, les widgets et le serveur MCP déployé, consultez [Comment une application est connectée ensemble](/help/guides/app-architecture.md).
