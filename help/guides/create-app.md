---
title: Configuration de l’infrastructure
description: Créez une application Adobe LLM à l’aide de l’infrastructure gérée par Adobe ou fournissez deux référentiels GitHub vides que la plateforme doit initialiser.
source-git-commit: 14d94a68dabddd2f2ef2cef65c51d31645d4a958
workflow-type: tm+mt
source-wordcount: '1337'
ht-degree: 0%
---

# Configuration de l’infrastructure {#create-first-app}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

L’infrastructure est la base technique de votre application : les référentiels qui stockent son code et les services qui exécutent ses gestionnaires d’action et diffusent ses widgets aux utilisateurs.

Comme expliqué dans la [présentation](/help/overview/overview.md#key-concepts), un **gestionnaire d’actions** effectue une tâche, telle que la recherche de produits, tandis qu’un **widget** présente le résultat sous la forme d’une expérience interactive dans la conversation.

[!DNL LLM Apps] stocke le code source du gestionnaire et du widget dans deux référentiels [!DNL GitHub] distincts :

- **Référentiel d’action** : contient les gestionnaires côté serveur et leurs tests.
- **Référentiel de widgets** — contient le code et les styles pour les widgets interactifs.

Par exemple, un gestionnaire de recherche de produits récupère les produits correspondants et son widget les affiche sous forme de cartes de produits. Les référentiels stockent ce code ; Adobe I/O Runtime exécute les gestionnaires déployés et Edge Delivery Services diffuse les widgets.

L’assistant de configuration prépare ces référentiels et connecte les services dont votre application a besoin. Nous vous recommandons de commencer par **l’infrastructure gérée par**, la manière la plus rapide de commencer. Adobe crée et héberge les référentiels pour vos gestionnaires d’action et widgets. Vous n’avez pas besoin de configurer vous-même les référentiels.

Vous pouvez également fournir deux référentiels [!DNL GitHub] vides dans votre propre compte ou organisation. La plateforme les initialise avec les fichiers de projet requis ; il n’est pas nécessaire de préparer au préalable l’action ou le code du widget.

**Dans ce guide :** découvrez comment choisir une option d’infrastructure et créer votre application à l’aide de l’assistant de configuration. Après la configuration, passez à [Création d’actions](/help/guides/creating-actions.md).

La création de l’application ne lance **la génération d’actions.** Démarrez la génération de l’action séparément de la page de l’application.

## Ouvrir l’assistant de création d’application

Ouvrez [Applications Adobe LLM](https://experience.adobe.com/llm-apps). Sur la page d’accueil, sélectionnez **[!UICONTROL Commencer]**. Si une application est déjà ouverte, utilisez le sélecteur d’applications et sélectionnez **[!UICONTROL Créer une application]**.

L’assistant **[!UICONTROL Créer votre application LLM]** commence par **[!UICONTROL Commencer]**. Choisissez comment configurer l’environnement technique.

![Création de votre application LLM : choisissez entre une infrastructure gérée par Adobe et la connexion de vos propres référentiels](/help/assets/guide-infrastructure/setup-choice.png)

| Option d’infrastructure | Sélectionner dans l’assistant | Ce que cela signifie |
|---|---|---|
| **Infrastructure gérée par** | **[!UICONTROL Laissez-nous nous en occuper]** | Adobe crée et gère les référentiels et les services connectés pour votre application. Un accès au code est disponible sur demande. |
| **Vos propres référentiels** | **[!UICONTROL Je vais le configurer moi-même]** | Vous fournissez deux référentiels GitHub vides et accordez l’accès via l’assistant. La plateforme initialise l’un pour les gestionnaires d’action et l’autre pour les widgets. |

Les deux options prennent en charge la génération automatique d’actions ultérieurement. Aucune des deux options ne nécessite que vous écriviez des actions manuellement.

## Utilisation de l’infrastructure gérée par Adobe {#adobe-managed-infrastructure}

Utilisez cette option pour permettre à Adobe de configurer et de gérer les référentiels et les services connectés pour votre application. Vous n’avez pas besoin de créer de référentiels ni d’installer vous-même des applications [!DNL GitHub].

1. Sélectionnez **[!UICONTROL Laissons-nous nous en occuper]**.
2. Sélectionnez **[!UICONTROL Étape suivante]**.
3. Complétez [Détails de l’application](#app-details).
4. Sélectionnez **[!UICONTROL Créer une application]**.

La page de l’application affiche **[!UICONTROL Configuration de l’application]** pendant qu’Adobe prépare les référentiels et connecte le site du widget. L’interface utilisateur indique que la configuration prend généralement entre 1 et 2 minutes ; les actions, les tests et le déploiement ne sont pas disponibles tant que la configuration n’est pas prête.

![Configuration de l’application — Adobe prépare l’infrastructure de l’application](/help/assets/guide-infrastructure/provisioning.png)

Si la configuration échoue, lisez l’erreur et sélectionnez **[!UICONTROL Réessayer]**. Si la page ne peut pas vérifier la progression, l’installation se poursuit en arrière-plan ; sélectionnez **[!UICONTROL Vérifier à nouveau]** pour vous reconnecter.

Une fois la configuration terminée, passez à [Création d’actions](/help/guides/creating-actions.md).

## Connecter vos propres référentiels {#your-own-repositories}

Sélectionnez **[!UICONTROL Je vais le configurer moi-même]**, puis **[!UICONTROL Étape suivante]**. Ce chemin d’accès comporte quatre étapes d’assistant :

**Prise en main → Configuration de l’infrastructure Action → Configuration de l’infrastructure Widget → Détails de l’application.**

Vous devez accéder aux référentiels sélectionnés et disposer des autorisations nécessaires pour installer ou configurer les applications [!DNL GitHub] requises. Pour les référentiels appartenant à l’organisation, un administrateur peut avoir besoin d’approuver l’accès.

Créez deux référentiels vides : un pour les gestionnaires d’action et un pour les widgets. N’initialisez pas avec un fichier README, une licence ou une `.gitignore`. La plateforme ajoute la structure de projet requise lorsque vous validez chaque référentiel dans l’assistant.

Si vous disposez déjà de projets d’action et EDS compatibles, vous pouvez les connecter à la place, mais le code existant n’est pas requis.

>[!NOTE]
>
>Pour un référentiel vide, la validation écrit les fichiers de projet initiaux. Sélectionnez les référentiels destinés à cette application avant de sélectionner **[!UICONTROL Valider le référentiel]**.

### Configurer l’infrastructure d’action

Le référentiel d’actions contient les tests et gestionnaires côté serveur qui s’exécutent lorsqu’une plateforme LLM appelle une action.

1. Sous **[!UICONTROL Source des actions]**, sélectionnez **[!UICONTROL GitHub]**. Les autres sources de code ne sont actuellement pas disponibles.
2. Réponse **[!UICONTROL Avez-vous déjà créé un référentiel GitHub ?]** :
   - Sélectionnez **[!UICONTROL Oui, j’utiliserai un référentiel déjà créé]** pour connecter un projet existant compatible ou un référentiel vide.
   - Sélectionnez **[!UICONTROL Je n’ai pas encore de référentiel]** puis **[!UICONTROL Créer des référentiels sur GitHub]** pour créer un référentiel d’actions vide. Revenez ensuite à l’assistant.
3. Connectez le référentiel à l’application [!DNL GitHub] Adobe LLM Apps :
   - Si aucune organisation n’est connectée, sélectionnez **[!UICONTROL Installer sur GitHub]**.
   - Si une organisation est déjà connectée, sélectionnez **[!UICONTROL Connecter le référentiel]** et ajoutez le référentiel à cette installation.
4. Sur [!DNL GitHub], choisissez **[!UICONTROL Ne sélectionner que les référentiels]** et accordez l’accès uniquement aux référentiels utilisés par cette application.
5. Revenez à l’assistant. Sous **[!UICONTROL Sélectionner le référentiel]**, choisissez les options **[!UICONTROL Organisation]** et **[!UICONTROL Référentiel]**.
6. Sélectionnez **[!UICONTROL Valider le référentiel]**. La validation initialise un référentiel vide avec le modèle standard Applications LLM Adobe . Un référentiel existant doit contenir un projet d’actions compatible.
7. Une fois la validation réussie, sélectionnez **[!UICONTROL Étape suivante]**.

![Configuration de l’infrastructure d’action — Sélectionnez GitHub et connectez un référentiel d’action](/help/assets/guide-infrastructure/action-repository.png)

Si une organisation ou un référentiel est manquant, utilisez **[!UICONTROL Connecter une autre organisation]** ou **[!UICONTROL Connecter un autre référentiel]** dans son sélecteur. Après avoir modifié l’accès sur [!DNL GitHub], sélectionnez **[!UICONTROL Actualiser les organisations et les référentiels]**.

### Configurer l’infrastructure du widget

Les widgets sont des réponses visuelles facultatives jointes à des actions, telles que des cartes de produits ou des formulaires de réservation. L’assistant connecte un référentiel [!DNL Edge Delivery Services] (EDS) pour les héberger.

1. Sous **[!UICONTROL Source du widget]**, sélectionnez **[!UICONTROL Sélectionner manuellement le référentiel de widgets]**.
2. Sous **[!UICONTROL Source du code]**, sélectionnez **[!UICONTROL GitHub]**.
3. Choisissez si vous disposez déjà d’un référentiel. Dans le cas contraire, sélectionnez **[!UICONTROL Créer des référentiels sur GitHub]**, créez un référentiel de widget vide et revenez à l’assistant.
4. Installez ou configurez l’application [!DNL GitHub] Adobe LLM Apps pour ce référentiel, comme vous l’avez fait pour le référentiel d’action.
5. Sélectionnez ses **[!UICONTROL Organisation]** et **[!UICONTROL Référentiel]**. Ne sélectionnez pas le référentiel d’actions.
6. Sélectionnez **[!UICONTROL Valider le référentiel]**. La validation initialise un référentiel vide avec le modèle AEM. Un référentiel existant doit être un projet EDS compatible.
7. Sélectionnez **[!UICONTROL Installer la synchronisation du code AEM]** et accordez à la synchronisation du code AEM l’accès au référentiel de widgets.
8. Sur la page de confirmation de l’installation, utilisez l’assistant d’installation pour vous ajouter en tant qu’administrateur du site EDS.
9. Revenez à [!DNL LLM Apps] et sélectionnez **[!UICONTROL Valider l’installation de la synchronisation du code AEM]**.
10. Lorsque **[!UICONTROL La synchronisation du code AEM est installée]** apparaît, sélectionnez **[!UICONTROL Étape suivante]**.

>[!NOTE]
>
>L’assistant marque **[!UICONTROL Utiliser Cloud Manager Edge Delivery Services]** comme **[!UICONTROL Bientôt disponible]** et n’autorise pas la sélection. Utilisez l’option manuelle widget-repository .

Si la validation ne détecte pas la synchronisation du code AEM, vérifiez que la synchronisation du code AEM a accès au référentiel de widgets sélectionné et que vous vous êtes ajouté en tant qu’administrateur ou administratrice du site. Validez à nouveau.

## Saisir les détails de l’application {#app-details}

Les deux options d’infrastructure se terminent par **[!UICONTROL Détails de l’application]**.

![Détails de l’application : saisissez un nom d’application, sélectionnez un programme AEM, puis choisissez la région données d’analyse](/help/assets/guide-infrastructure/app-details.png)

1. Saisissez le **[!UICONTROL Nom de l’application LLM]**. Utilisez un nom qui identifie la marque ou l’expérience, tel que *café Frescopa*.
2. Sélectionnez le programme **** auquel l’application appartient. Si vous n’avez accès qu’à un seul programme, l’assistant le sélectionne automatiquement.
3. Sous **[!UICONTROL Région de données Analytics]**, choisissez **[!UICONTROL États-Unis (US)]** ou **[!UICONTROL Europe (UE)]**.
4. Sélectionnez **[!UICONTROL Créer une application]**.

>[!IMPORTANT]
>
>La région Programme AEM et données d’analyse ne peut pas être modifiée une fois l’application créée. La région Analytics détermine l’emplacement de stockage des données d’analyse de l’application.

Si aucun programme AEM n’est disponible, demandez l’accès à votre administrateur. Si le chargement des programmes échoue, sélectionnez **[!UICONTROL Réessayer de charger les programmes]**.

L’assistant ouvre la page de l’application après sa création. Avec l’infrastructure gérée par Adobe, attendez la fin de la configuration de l’infrastructure avant de continuer.

## Ensuite : créez les actions

Votre application dispose désormais de son infrastructure, mais aucune action générée pour le moment. Continuez avec [Créer des actions](/help/guides/creating-actions.md) pour analyser un site web ou choisissez la création d’actions manuelles.

Pour connaître la relation entre les métadonnées d’action, les gestionnaires, les widgets et le serveur MCP déployé, consultez la section [Comment une application est connectée ensemble](/help/guides/app-architecture.md).
