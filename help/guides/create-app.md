---
title: Configuration de l’infrastructure
description: Créez une application LLM Adobe à l’aide de l’infrastructure gérée des applications LLM ou de vos propres référentiels ajoutés à Cloud Manager.
source-git-commit: 9b2ec369baba5d533088d2dd4adefcb80f5c9727
workflow-type: tm+mt
source-wordcount: '1947'
ht-degree: 1%
---

# Configuration de l’infrastructure {#create-first-app}

>[!IMPORTANT]
>
>Les applications LLM sont disponibles pour tous les clients AEM Sites Cloud Service afin qu’ils puissent développer et gérer leurs propres applications. Une fois le comptage commencé, chaque appel d’application est comptabilisé dans les demandes de contenu AEM Sites du client. Adobe annoncera le taux de contrôle et la date de début avant que le contrôle ne prenne effet.

L’infrastructure est la base technique de votre application : les référentiels qui stockent son code et les services qui exécutent ses gestionnaires d’action et diffusent ses widgets aux utilisateurs.

Comme expliqué dans la [présentation](/help/overview/overview.md#key-concepts), un **gestionnaire d’action** effectue une tâche, telle que la recherche de produits, tandis qu’un **widget** présente le résultat sous la forme d’une expérience interactive dans la conversation.

[!DNL LLM Apps] stocke le code source du gestionnaire et du widget dans deux référentiels distincts :

- **Référentiel d&#39;actions** — contient les gestionnaires côté serveur et leurs tests.
- **Référentiel de widgets** — contient le code et les styles pour les widgets interactifs.

Par exemple, un gestionnaire de recherche de produits récupère les produits correspondants et son widget les affiche sous forme de cartes de produits. Les référentiels stockent ce code ; Adobe I/O Runtime exécute les gestionnaires déployés et Edge Delivery Services diffuse les widgets.

L’assistant de configuration prépare ces référentiels et connecte les services dont votre application a besoin. Choisissez l’une des deux options suivantes :

- L’infrastructure gérée **[!DNL LLM Apps]** est le moyen le plus rapide de commencer. Adobe crée et héberge les référentiels pour vos gestionnaires d’action et widgets, de sorte que vous n’avez pas besoin de configurer de référentiels vous-même.
- **Vos propres référentiels** conservez le code de votre application dans les référentiels que vous possédez, ajoutés à [!DNL Cloud Manager]. Ils peuvent être hébergés sur [!DNL GitHub], [!DNL GitHub Enterprise], [!DNL GitLab], [!DNL Bitbucket] ou [!DNL Azure DevOps]. La plateforme initialise les référentiels vides avec les fichiers de projet requis, de sorte que vous n’avez pas besoin de préparer au préalable l’action ou le code du widget.

**Dans ce guide :** Découvrez comment choisir une option d&#39;infrastructure et créer votre application à l&#39;aide de l&#39;assistant de configuration. Après la configuration, passez à [Créer des actions](/help/guides/creating-actions.md).

La création de l’application ne lance **pas** la génération d’actions. Démarrez la génération de l’action séparément de la page de l’application.

## Ouvrir l’assistant de création d’application

Ouvrez les [applications Adobe LLM](https://experience.adobe.com/llm-apps). Sur la page d’accueil, sélectionnez **[!UICONTROL Commencer]**. Si une application est déjà ouverte, utilisez le sélecteur d’applications et sélectionnez **[!UICONTROL Créer une application]**.

L&#39;assistant **[!UICONTROL Créer votre application LLM]** commence par **[!UICONTROL Commencer]**. Choisissez comment configurer l’environnement technique.

![Création de votre application LLM — Prise en main : laissez-nous la gérer et utiliser ma configuration Cloud Manager ; le contour rouge indique l’option de référentiel personnel](/help/assets/guide-infrastructure/setup-choice.png)

| Option d’infrastructure | Sélectionner dans l’assistant | Ce que cela signifie |
|---|---|---|
| **[!DNL LLM Apps]infrastructure gérée** | **[!UICONTROL Laissons-nous nous en occuper]** | Adobe crée et gère les référentiels et les services connectés pour votre application. Un accès au code est disponible sur demande. |
| **Vos propres référentiels** | **[!UICONTROL Utiliser ma configuration Cloud Manager]** | Vous sélectionnez deux référentiels ajoutés à [!DNL Cloud Manager] : un pour les gestionnaires d’action et un pour les widgets. La plateforme les initialise si elles sont vides. |

Les deux options prennent en charge la génération automatique d’actions ultérieurement. Aucune des deux options ne nécessite que vous écriviez des actions manuellement.

## Utiliser l’infrastructure gérée [!DNL LLM Apps] {#adobe-managed-infrastructure}

Utilisez cette option pour permettre à Adobe de configurer et de gérer les référentiels et les services connectés pour votre application. Vous n’avez pas besoin de créer de référentiels ni d’installer vous-même les applications [!DNL GitHub].

1. Sélectionnez **[!UICONTROL Laissons-nous gérer]**.
2. Sélectionnez **[!UICONTROL Étape suivante]**.
3. Complétez les [détails de l&#39;application](#app-details).
4. Sélectionnez **[!UICONTROL Créer Une Application]**.

La page de l’application affiche **[!UICONTROL Configuration de l’application]** pendant qu’Adobe prépare les référentiels et connecte le site du widget. L’interface utilisateur indique que la configuration prend généralement entre 1 et 2 minutes ; les actions, les tests et le déploiement ne sont pas disponibles tant que la configuration n’est pas prête.

![Configuration de l&#39;application — Adobe prépare l&#39;infrastructure de l&#39;application](/help/assets/guide-infrastructure/provisioning.png)

Si la configuration échoue, lisez l’erreur et sélectionnez **[!UICONTROL Réessayer]**. Si la page ne peut pas vérifier la progression, l’installation se poursuit en arrière-plan ; sélectionnez **[!UICONTROL Vérifier à nouveau]** pour vous reconnecter.

Une fois la configuration terminée, continuez avec [Création d’actions](/help/guides/creating-actions.md).

## Utiliser vos propres référentiels {#your-own-repositories}

Sélectionnez **[!UICONTROL Utiliser ma configuration Cloud Manager]**, puis **[!UICONTROL Étape suivante]**. Ce chemin d’accès comporte quatre étapes d’assistant :

**Prise en main → Configuration de l&#39;infrastructure Action → Configuration de l&#39;infrastructure Widget → Détails de l&#39;application.**

[!DNL LLM Apps] atteint vos référentiels via [!DNL Cloud Manager], à l’aide de votre compte Adobe. Vous n&#39;installez pas d&#39;application [!DNL GitHub].

### Avant de commencer {#own-repositories-prerequisites}

- Accès au programme [!DNL Cloud Manager] auquel l&#39;application appartiendra.
- Deux référentiels, l’un pour les gestionnaires d’action et l’autre pour les widgets. Les référentiels vides conviennent, y compris un référentiel contenant uniquement le fichier LISEZ-MOI et son hôte Git créés.
- Les deux référentiels [&#x200B; ont été ajoutés à Cloud Manager](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-manager/content/managing-code/external-repositories) en tant que référentiels privés, avec la propriété validée et le statut **Prêt**. [!DNL Cloud Manager] doit pouvoir y écrire ; un jeton d&#39;accès en lecture seule ne suffit pas.

Seuls les référentiels que vous hébergez sont répertoriés. Les référentiels hébergés par Adobe dans [!DNL Cloud Manager] ne sont pas pris en charge.

#### Votre référentiel de widgets est un site Edge Delivery Services {#widget-repository-is-an-eds-site}

Les widgets sont diffusés à partir d’un site [!DNL Edge Delivery Services] (EDS) et votre référentiel de widgets contient le code de ce site. Assurez-vous que le référentiel de widgets appartient ou appartiendra à un site EDS. Vous pouvez en configurer un de deux manières différentes :

- **N’importe quel fournisseur Git, avec votre propre Git.** Créez le site EDS dans [!DNL Cloud Manager] et liez-le à votre référentiel avec **Apportez votre propre Git**. [!DNL Cloud Manager] synchronise ensuite le code du référentiel avec le site. Effectuez cette opération avant de démarrer l’assistant. Voir [Apporter votre propre Git](https://www.aem.live/developer/byo-git).
- **Un référentiel [!DNL GitHub], avec synchronisation du code AEM.** Configurez vous-même le site EDS, comme indiqué dans le tutoriel [Edge Delivery Services](https://www.aem.live/developer/tutorial). L’assistant vous guide tout au long de l’installation de la synchronisation du code AEM et de la vérification de l’accès après avoir validé le référentiel.

L’étape du widget détecte la casse à appliquer. Voir [Configurer l’infrastructure du widget](#set-up-the-widget-infrastructure).

>[!TIP]
>
>Même si votre société dispose déjà d’un site EDS, créez un site EDS distinct pour vos widgets [!DNL LLM Apps].

>[!NOTE]
>
>Pour un référentiel vide, la validation écrit les fichiers de projet initiaux. Sélectionnez les référentiels destinés à cette application avant de sélectionner **[!UICONTROL Valider le référentiel]**.

### Configurer l’infrastructure d’action

Le référentiel d’actions contient les tests et gestionnaires côté serveur qui s’exécutent lorsqu’une plateforme LLM appelle une action.

1. Sous **[!UICONTROL Programme Cloud Manager]**, sélectionnez le programme contenant votre référentiel d’actions.
2. Sous **[!UICONTROL Référentiel Cloud Manager]**, sélectionnez le référentiel d’actions. S’il est manquant, ajoutez-le dans [!DNL Cloud Manager] avec **[!UICONTROL Accédez à Cloud Manager]**, puis actualisez la liste.
3. Sélectionnez **[!UICONTROL Valider le référentiel]**. La validation initialise un référentiel vide avec le modèle standard Applications LLM Adobe . Un référentiel existant doit contenir un projet d’actions compatible.
4. Lorsque **[!UICONTROL Référentiel validé]** apparaît, sélectionnez **[!UICONTROL Étape suivante]**.

![Configuration de l&#39;infrastructure d&#39;action — Programme Cloud Manager et référentiel sélectionnés, avec référentiel validé](/help/assets/guide-infrastructure/action-repository.png)

La plateforme transmet le code d’action généré à une branche de ce référentiel. Sélectionner **[!UICONTROL Marquer comme révisé]** fusionne la branche. Voir [Création d’actions](/help/guides/creating-actions.md).

### Configurer l’infrastructure du widget {#set-up-the-widget-infrastructure}

Les widgets sont des réponses visuelles facultatives jointes à des actions, telles que des cartes de produits ou des formulaires de réservation. Edge Delivery Services (EDS) les diffuse à partir d’un site qui utilise votre référentiel de widgets.

1. Sous **[!UICONTROL Source du widget]**, sélectionnez **[!UICONTROL Sélectionner manuellement le référentiel du widget]**.

   ![Configuration de l’infrastructure des widgets — Source des widgets avec Sélection manuelle du référentiel des widgets](/help/assets/guide-infrastructure/widget-source.png)

2. Sous **[!UICONTROL Programme Cloud Manager]**, sélectionnez le programme contenant le référentiel de widgets. Il peut différer du programme du référentiel d’actions.
3. Sous **[!UICONTROL Référentiel Edge Delivery Services]**, sélectionnez le référentiel de widgets. Ne sélectionnez pas le référentiel d’actions.
4. Sélectionnez **[!UICONTROL Valider le référentiel]**. La validation initialise un référentiel vide avec le modèle AEM. Un référentiel existant doit être un projet EDS compatible.

La suite dépend de l’emplacement d’hébergement du référentiel de widgets.

#### Référentiel de widgets sur un autre fournisseur (apporter votre propre Git) {#widget-repository-byog}

Pour un référentiel sur [!DNL GitLab], [!DNL Bitbucket], [!DNL Azure DevOps] ou [!DNL GitHub Enterprise], la validation trouve le site EDS lié par [!DNL Cloud Manager] au référentiel. L’assistant affiche le site sous **[!UICONTROL Site Edge Delivery Services]**.

![Configuration de l’infrastructure des widgets : référentiel de widgets validé et site Edge Delivery Services sur lequel Cloud Manager le synchronise](/help/assets/guide-infrastructure/widget-repository.png)

[!DNL Cloud Manager] synchronise le code du référentiel avec ce site. Vous n’avez donc pas besoin de synchroniser le code AEM. Sélectionnez **[!UICONTROL Étape suivante]**.

Si la validation signale qu’aucun site n’est lié, liez le référentiel à un site EDS dans [!DNL Cloud Manager] avec **Apportez votre propre Git**, puis validez à nouveau. Voir [Apporter votre propre Git](https://www.aem.live/developer/byo-git).

#### Référentiel de widgets sur GitHub {#widget-repository-github}

Un référentiel [!DNL GitHub] utilise la synchronisation de code AEM pour publier son code. Après la validation, l’assistant affiche **[!UICONTROL Installer la synchronisation du code AEM]**. Utilisez le même compte Adobe pour [!DNL LLM Apps], l’accès au site AEM et l’accès en écriture aux données numériques.

##### Configurer l’accès au site AEM {#aem-site-access}

**Nouvelle installation :** Sélectionnez **[!UICONTROL Installer la synchronisation du code AEM]** et octroyez-lui l’accès au référentiel de widgets. Terminez l’assistant de configuration sur la page de confirmation de l’installation pour vous ajouter en tant qu’administrateur du site.

**Site EDS existant :** Conservez l’installation de la synchronisation du code AEM et vérifiez qu’elle couvre le référentiel de widgets sélectionné. Demandez à un administrateur de site existant d’ouvrir AEM User Admin à l’adresse `https://tools.aem.live/tools/user-admin/index.html/?org=<org>&site=<site>` et d’ajouter votre compte Adobe avec le rôle `admin`. Vous n’avez pas besoin de réinstaller la synchronisation du code.

Remplacez `<org>` et `<site>` par les noms de votre organisation et de votre site AEM. L’administrateur de votre site peut confirmer ces valeurs.

##### Accorder un accès en écriture aux données numériques {#da-write-access}

Pour les sites nouveaux et existants, demandez à un administrateur d’organisation DA de :

1. Ouvrez la configuration DA à l’adresse `https://da.live/config#/<org>/`, en remplaçant `<org>` par l’organisation DA qui stocke le contenu de votre site.
2. Ouvrez la feuille **autorisations**. S’il n’existe pas, sélectionnez **[!UICONTROL Ajouter une feuille]**, nommez-la `permissions` et ajoutez les en-têtes de colonne **chemin**, **groupes** et **actions**. Vous pouvez également ajouter une colonne **commentaires** pour décrire chaque règle.
3. Ajoutez une ligne accordant à votre compte Adobe l’accès en écriture à tout le contenu du site : utilisez le chemin de contenu du site dans **chemin**, l’adresse e-mail de votre compte Adobe dans **groupes** et `write` dans **actions**. Enregistrez la configuration.

   Feuille d’autorisations ![DA montrant des exemples de comptes dans la colonne groupes et un accès en écriture dans la colonne actions](/help/assets/guide-infrastructure/da-write-permissions.png)

   La capture d’écran utilise des comptes fictifs. Remplacez `widget-author@example.com` par l’e-mail de votre compte Adobe et utilisez le chemin d’accès au contenu de votre site au lieu de l’exemple `/ + **` à l’échelle de l’organisation. La ligne `CONFIG` est destinée aux administrateurs de configuration ; vous n’avez pas besoin de la modifier pour accorder l’accès au contenu.

Les rôles de site AEM n’accordent pas l’accès en écriture DA. Vous n’avez pas besoin d’un accès en écriture à l’échelle de l’organisation.

Pour plus d&#39;informations sur la configuration des autorisations DA, voir [Autorisations et sécurité](https://www.aem.live/docs/ew/administering/permissions).

##### Validation de la configuration d’AEM {#validate-aem-setup}

1. Revenez à [!DNL LLM Apps] et sélectionnez **[!UICONTROL Valider la configuration AEM]**.
2. Lorsque **[!UICONTROL Configuration d’AEM vérifiée]** apparaît, sélectionnez **[!UICONTROL Étape suivante]**.

Si la validation échoue, suivez le message pour vérifier [l’accès au site AEM](#aem-site-access) ou l’accès en écriture [DA](#da-write-access), puis validez à nouveau.

>[!NOTE]
>
>L’assistant marque **[!UICONTROL Utiliser Cloud Manager Edge Delivery Services]** comme **[!UICONTROL Bientôt disponible]** et n’autorise pas la sélection. Utilisez l’option manuelle widget-repository .

## Saisir les détails de l’application {#app-details}

Les deux options d’infrastructure se terminent par **[!UICONTROL Détails de l’application]**.

![Détails de l’application : nom de l’application LLM saisi, programme AEM défini à partir du référentiel Cloud Manager et région de données Informations sur l’application](/help/assets/guide-infrastructure/app-details.png)

1. Saisissez le **[!UICONTROL nom de l&#39;application LLM]**. Utilisez un nom qui identifie la marque ou l&#39;expérience, tel que *Café Frescopa*.
2. Sélectionnez le **[!UICONTROL programme AEM]** auquel l’application appartient. Si vous n’avez accès qu’à un seul programme, l’assistant le sélectionne automatiquement. Avec vos propres référentiels, le programme est le programme du référentiel d’actions et ne peut pas être modifié ici.
3. Dans la région de données **[!UICONTROL Informations sur l’application]**, choisissez **[!UICONTROL États-Unis (US)]** ou **[!UICONTROL Europe (UE)]**.
4. Sélectionnez **[!UICONTROL Créer Une Application]**.

>[!IMPORTANT]
>
>Le programme AEM et la région de données Informations sur l’application ne peuvent pas être modifiés une fois l’application créée. La région détermine l’emplacement de stockage des données Application Insights de l’application.

Si aucun programme AEM n’est disponible, demandez l’accès à votre administrateur. Si le chargement des programmes échoue, sélectionnez **[!UICONTROL Réessayer de charger les programmes]**.

L’assistant ouvre la page de l’application après sa création. Avec l’infrastructure gérée [!DNL LLM Apps], attendez la fin de la configuration de l’infrastructure avant de continuer.

Pour vérifier les référentiels ultérieurement, ouvrez les **[!UICONTROL Paramètres]** de l’application. La section **[!UICONTROL Référentiel]** affiche le programme et le nom de chaque référentiel et, pour un référentiel de widget qui utilise Apporter votre propre Git, son site EDS et son origine. Les référentiels ne peuvent pas être modifiés une fois l’application créée.

![Paramètres d&#39;application : section Référentiel avec les référentiels standard et Edge Delivery Services, y compris le site lié et l&#39;origine](/help/assets/guide-infrastructure/settings-repositories.png)

Les applications créées précédemment avec l’application Adobe LLM Apps [!DNL GitHub] continuent à fonctionner avec leurs référentiels existants.

## Ensuite : créez les actions

Votre application dispose désormais de son infrastructure, mais aucune action générée pour le moment. Continuez avec [Création d’actions](/help/guides/creating-actions.md) pour analyser un site web ou choisissez la création manuelle d’actions.

Pour connaître la relation entre les métadonnées d&#39;action, les gestionnaires, les widgets et le serveur MCP déployé, consultez [Comment une application est connectée ensemble](/help/guides/app-architecture.md).
