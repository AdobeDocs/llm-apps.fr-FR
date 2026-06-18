---
title: Intégration de Beta pour les applications Adobe LLM
description: Commencez avec le participant au programme Adobe LLM Apps as a Beta.
source-git-commit: 1a99e2e80e50a3bcf9ce6fb910365202bf06e113
workflow-type: tm+mt
source-wordcount: '1557'
ht-degree: 0%

---


# Intégration de Beta {#beta-onboarding}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] est actuellement dans Beta.
>
>Les fonctionnalités, les workflows et l’interface utilisateur affichés ici ne représentent pas nécessairement l’état final du produit. Pour rejoindre le Beta, envoyez un e-mail à llm-apps-beta@adobe.com.

>[!NOTE]
>
>Avant de commencer, assurez-vous que toutes les [conditions préalables](/help/beta-onboarding/prerequisites.md) sont remplies.

En tant que participant au programme Beta pour [!DNL Adobe LLM Apps], vous recevrez un e-mail contenant deux archives zip et une référence de configuration d’application. Suivez les étapes ci-dessous pour mettre votre application en ligne.

## Avant de commencer

Avant de passer aux étapes, familiarisez-vous avec les concepts clés utilisés dans ce guide. Cela vous fera gagner du temps et vous aidera à tout mettre en place.

**Application LLM** : assistant de marque avec lequel les utilisateurs interagissent au sein de [!DNL ChatGPT] ou d’autres plateformes LLM.

**Action** — une fonctionnalité que votre application offre. Par exemple, « Trouver un distributeur » ou « Parcourir les produits ». Chaque action est appelée par le LLM lorsque l’utilisateur pose une question pertinente.

**Gestionnaire d&#39;action** — Code qui s&#39;exécute lorsqu&#39;une action est appelée. Il peut appeler vos API, récupérer des données actives ou renvoyer des données statiques. Les exemples de gestionnaires fournis par Adobe renvoient des données codées en dur afin que vous puissiez vérifier la configuration de bout en bout avant de connecter votre serveur principal réel.

**Widget** — la réponse visuelle présentée à l’utilisateur — une carte, un carrousel, un tableau ou toute interface utilisateur personnalisée rendue avec la réponse textuelle du LLM.

**Référence de configuration de l’application** — Fichier fourni par Adobe qui vous indique exactement ce que vous devez entrer pour chaque action lors de la configuration de votre application.


## Étape 1 : transférer les archives fournies vers [!DNL GitHub]

Adobe fournit deux archives zip par e-mail :

- **Code de l’application** (`<project-name>.zip`) : gestionnaires d’actions qui s’exécutent sur [!DNL Adobe I/O Runtime] et alimentent la logique de votre application. Vous allez déployer ces applications en l’état pour que l’application fonctionne de bout en bout, puis les mettre à jour ultérieurement pour connecter votre véritable serveur principal.
- **Projet EDS** (`<project-name>-eds.zip`) : code frontal de vos widgets. Adobe les a préconfigurés pour vous ; il s’agit de votre base de code pour posséder, personnaliser et mettre en forme pour correspondre à votre marque.

Créez **deux référentiels vides** sur [!DNL GitHub] (un par archive), puis décompressez chaque archive et envoyez-la. Nous vous recommandons de nommer chaque référentiel d’après le fichier zip correspondant, `<project-name>` pour le code de l’application et `<project-name>-eds` pour le projet EDS.

`<your-github-org>` fait référence à votre nom d’utilisateur [!DNL GitHub] personnel ou à une organisation [!DNL GitHub], quel que soit le compte propriétaire des référentiels.

**Référentiel de code d’application** — Décompressez l’archive, initialisez un référentiel Git local et poussez-le vers [!DNL GitHub] :

```bash
# Unzip and enter the folder
unzip <project-name>.zip
cd <project-name>

# Initialize and push
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:<your-github-org>/<your-repo>.git
git push -u origin main
```

**Référentiel EDS** — répétez les mêmes étapes pour l&#39;archive EDS, en pointant vers le deuxième référentiel :

```bash
unzip <project-name>-eds.zip
cd <project-name>-eds

git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:<your-github-org>/<your-eds-repo>.git
git push -u origin main
```

## Étape 2 : créer une application LLM

Accédez à [](https://experience.adobe.com/llm-apps/) puis cliquez sur **[!UICONTROL Créer une application LLM]**.

![Page applications — aucune application créée pour le moment](/help/assets/guide-create-app/first-load.png)

Renseignez les **[!UICONTROL Détails de l’application]** à l’aide des valeurs de la section **[!UICONTROL Détails de l’application]** dans la référence de configuration de l’application :

- **[!UICONTROL Nom de l’application LLM]**
- **[!UICONTROL Description de l’application LLM]**
- **[!UICONTROL Votre site web]**

![ Boîte de dialogue Créer une application ](/help/assets/guide-create-app/app-details-1.png)

Sous **[!UICONTROL Région des données Analytics]**, sélectionnez la région où les données Analytics seront stockées. Ce **ne peut pas être modifié** après la création de l’application.

>[!IMPORTANT]
>
>La région de données Analytics ne peut pas être modifiée une fois l’application créée.

![Liste déroulante de la région de données Analytics](/help/assets/guide-create-app/app-details-analytics-dropdown.png)

Sous **Référentiel**, sélectionnez l’organisation [!DNL GitHub] et le **référentiel de code de l’application** que vous avez intégrés précédemment.

>[!NOTE]
>
>Si c’est la première fois que vous configurez une application, votre organisation [!DNL GitHub] n’apparaît pas encore dans la liste. Cliquez sur **[!UICONTROL Connecter une autre organisation GitHub]** pour lier votre organisation et accorder l’accès au référentiel.

![Boîte de dialogue Créer une application — Référentiel lié](/help/assets/guide-create-app/app-details-repo-linked.png)

Laissez la case **[!UICONTROL Suggérer automatiquement des actions en fonction de votre site web]** décochée ; vous allez configurer les actions manuellement.

Acceptez les **[!UICONTROL conditions d’]** puis cliquez sur **[!UICONTROL Créer une application]**.

![Création de l’application — écran de chargement](/help/assets/guide-create-app/app-loading.png)

![Page Détails de l’application](/help/assets/guide-create-app/app-detail-top.png)


## Étape 3 : activer vos widgets

Au cours de cette étape, vous configurez le projet EDS fourni par Adobe et vous le publiez via [!DNL DA.live] couche de création Adobe et de réseau CDN. Chaque document publié devient le widget affiché pour l’utilisateur ou l’utilisatrice lorsqu’une action est appelée.

### Étape 3.1 : connecter le référentiel EDS à [!DNL DA.live]

1. Accédez à [](https://github.com/apps/aem-code-sync). Si l’application n’est pas encore installée, cliquez sur **[!UICONTROL Installer]**. S’il est déjà installé, cliquez sur **[!UICONTROL Configurer]** et ajoutez-`<your-eds-repo>` à la liste des référentiels auxquels il peut accéder.
2. Après l’installation, vous accédez à une page de confirmation **[!DNL AEM Code Sync]enregistrée** Sous **Que faire ensuite → Créer votre contenu**, cliquez sur le lien [!DNL DA.live].
3. Sur l’écran **Contenu de démonstration**, sélectionnez **Aucun** et cliquez sur **Créer quelque chose de merveilleux**.
4. Vous accédez à la vue Auteur [!DNL DA.live] pour votre site.

### Étape 3.2 : créer un document [!DNL DA.live] pour chaque action

Dans [!DNL DA.live], vous devez créer **un document par action**. Une fois publié, chaque document devient le widget affiché pour l’utilisateur ou l’utilisatrice lorsque cette action est appelée.

Pour chaque action :

1. Dans [!DNL DA.live], créez un nouveau document à la racine de votre site et nommez-le comme spécifié dans la référence de configuration de votre application (voir la section **[!DNL DA.live]des documents**).
2. Dans le document, utilisez la barre latérale gauche et cliquez sur **[!UICONTROL Bloc]** pour insérer un nouveau bloc.
3. Définissez l’en-tête du bloc sur le nom de bloc spécifié dans votre référence de configuration d’application (voir **[!DNL DA.live]section documents**).
4. Publiez le document à l’aide du bouton **[!UICONTROL Publier]** (icône de plan de papier dans la barre d’outils supérieure).

Une fois publié, chaque document est accessible au `https://main--<your-eds-repo>--<your-github-org>.aem.live/<document-name>`. Cette URL est ce que vous allez saisir dans le champ **[!UICONTROL URL du widget]** lors de la configuration de chaque action à l’étape 4.


### Étape 3.3 : Configurer les en-têtes CORS pour le site EDS

Pour permettre aux plateformes LLM de charger vos widgets à plusieurs origines, vous devez ajouter un en-tête `Access-Control-Allow-Origin` à votre site EDS.

Accédez à l’**éditeur d’en-têtes HTTP** à l’adresse [tools.aem.live/tools/headers-edit/index.html](https://tools.aem.live/tools/headers-edit/index.html).

1. Saisissez votre **Organisation** (`<your-github-org>`) et **Site** (nom de votre référentiel EDS), puis cliquez sur **[!UICONTROL Récupérer]**. Vous serez invité à vous authentifier et à autoriser l’accès à votre site.
2. Sous path `/**`, cliquez sur **[!UICONTROL Add Header]**.
3. Définissez le nom de l’en-tête sur `Access-Control-Allow-Origin` et la valeur sur `*`.
4. Cliquez sur **[!UICONTROL Enregistrer]**.

Pour consulter la documentation complète sur les en-têtes HTTP personnalisés dans [!DNL AEM Edge Delivery Services], voir [aem.live/docs/custom-headers](https://www.aem.live/docs/custom-headers).

Après avoir enregistré les en-têtes, déclenchez une synchronisation du code pour propager les modifications dans tous les fichiers :

```bash
curl -X POST "https://admin.hlx.page/code/<your-github-org>/<your-eds-repo>/main/*"
```


## Étape 4 : Ajouter des actions

Dans l’[interface utilisateur des applications LLM](https://experience.adobe.com/llm-apps/), ouvrez votre application et accédez à **[!UICONTROL Actions]** dans la barre latérale gauche. Cliquez sur **+** pour créer une action. Répétez l’opération pour chaque action décrite dans la référence de configuration de votre application (voir les sections **Action 1**, **Action 2**, **Action 3**).

![Page Actions — Aucune action pour le moment](/help/assets/guide-create-action/actions-empty.png)

### Onglet Action

- **Nom de l’action** et **Description** : utilisés par les plateformes LLM pour décider quand appeler l’action. Utilisez les valeurs exactes de la section **Onglet Action** dans votre référence de configuration d’application.
- **Paramètres d’entrée** — nom, type et description de chaque paramètre. Utilisez les valeurs de la section **Onglet Action** dans votre référence de configuration d’application.

![Créer une action — informations de base](/help/assets/guide-create-action/action-basic-info.png)

### Onglet Métadonnées du widget

- **Type** — sélectionnez **[!UICONTROL EDS]**.

Développez **[!UICONTROL Configuration de CSP]** et renseignez les éléments suivants :

- **[!UICONTROL CSP — Connecter des domaines]** — utilisez les valeurs de la section **Onglet Métadonnées du widget** dans la référence de configuration de l’application.
- **[!UICONTROL CSP — Domaines de ressources]** — utilisez les valeurs de la section **Onglet Métadonnées du widget** dans la référence de configuration de l’application.

![Créer une action — métadonnées de widget](/help/assets/guide-create-action/widget-metadata.png)

![Créer une action : autorisations et CSP](/help/assets/guide-create-action/widget-permissions-csp.png)

### Onglet Widget Builder

Sous **[!UICONTROL Source du widget]**, sélectionnez **[!UICONTROL Utiliser un widget existant]**, puis renseignez les champs suivants :

- **[!UICONTROL URL de script]** : utilisez la valeur de la section **Onglet Métadonnées du widget** dans la référence de configuration de l&#39;application.
- **[!UICONTROL URL du widget]** : utilisez la valeur de la section **Onglet Métadonnées du widget** dans la référence de configuration de l&#39;application.

Cliquez sur **[!UICONTROL Créer une action]**. L’action s’affiche sous forme de vignette sur la page Actions avec un badge **[!UICONTROL EDS]** et un nombre de paramètres.

![Page Actions — Action créée](/help/assets/guide-create-action/actions-with-action.png)


## Étape 5 : Déployer

Une fois toutes les actions configurées, accédez à la page Détails de l’application et cliquez sur **[!UICONTROL Déployer]** dans le coin supérieur droit.

![Détails de l’application : prêt à être déployé](/help/assets/guide-deploy/app-detail-deploy-ready.png)

Sélectionnez l’environnement cible et cliquez sur **[!UICONTROL Déployer]**. Le pipeline s’exécute en quatre étapes : préparation des informations d’identification, démarrage du déploiement, création de votre application à partir du référentiel et publication sur [!DNL Adobe I/O Runtime].

![Exécution du pipeline de déploiement](/help/assets/guide-deploy/deploy-pipeline-deploying.png)

Une fois l’opération terminée, faites défiler l’écran jusqu’à la section **[!UICONTROL Tester l’application]** de la page Détails de l’application et copiez l’URL du serveur **[!UICONTROL MCP]** — vous en aurez besoin pour enregistrer votre application dans [!DNL ChatGPT].

![Déploiement réussi](/help/assets/guide-deploy/app-detail-deploy-finish.png)

![Tester l’application — URL déployées](/help/assets/guide-deploy/test-app-deployed.png)


## Étape 6 : ajouter l’application à [!DNL ChatGPT]

L’ajout d’applications personnalisées à [!DNL ChatGPT] nécessite un abonnement **Pro**, **Business** ou **Enterprise**. Les forfaits gratuits et Plus ne prennent pas en charge les applications MCP personnalisées.

1. Dans [!DNL ChatGPT], cliquez sur l’avatar de votre profil et accédez à **[!UICONTROL Paramètres]**.

   ![ChatGPT — Menu Paramètres](/help/assets/guide-test-chatgpt/chatgpt-settings-menu.png)

2. Sélectionnez **[!UICONTROL Applications]** dans la barre latérale, cliquez sur **[!UICONTROL Paramètres avancés]**, puis activez **[!UICONTROL Mode Développeur]**.

   ![ChatGPT — Mode Développeur activé](/help/assets/guide-test-chatgpt/chatgpt-developer-mode.png)

3. Accédez à **[!UICONTROL Paramètres] → [!UICONTROL Applications]** et cliquez sur **[!UICONTROL Créer une application]**.

   ![ChatGPT — Boîte de dialogue Créer une application](/help/assets/guide-test-chatgpt/chatgpt-create-app.png)

4. Collez l’**[!UICONTROL URL du serveur MCP]** copiée à partir de [!DNL LLM Apps], définissez **[!UICONTROL Authentification]** sur *Aucune authentification*, cochez la case d’accusé de réception, puis cliquez sur **Créer**.

Votre application apparaît sous **[!UICONTROL Applications activées]** avec un badge **[!UICONTROL DEV]**.

![ChatGPT — application activée](/help/assets/guide-test-chatgpt/chatgpt-app-enabled.png)

Commencez une nouvelle conversation, joignez votre application à l’aide du bouton **+** ou en saisissant **@** suivi du nom de votre application, puis posez une question correspondant à l’une de vos actions configurées.

![ChatGPT — sélectionnez l&#39;application dans le menu](/help/assets/guide-test-chatgpt/chatgpt-select-app.png)

![ChatGPT — résultat de l&#39;action](/help/assets/guide-test-chatgpt/chatgpt-response.png)

## Prochaines étapes

L’exemple d’application que vous avez déployé utilise des données codées en dur. Pour en faire une expérience prête pour la production :

- **Connecter vos API** — Mettez à jour les gestionnaires d&#39;actions dans le référentiel de code de votre application pour appeler vos API, bases de données ou services réels. Chaque gestionnaire vit à `actions/<action-name>/index.js`.
- **Examinez et affinez vos widgets** — Ouvrez votre projet EDS, ajustez les styles de bloc et la disposition pour qu’ils correspondent à votre marque, et vérifiez que le rendu du widget est correct avec les données actives.
- **Redéployer** — Une fois vos gestionnaires et widgets mis à jour, poussez vos modifications vers [!DNL GitHub] et cliquez sur **[!UICONTROL Déployer]** dans l’interface utilisateur [!DNL LLM Apps] pour publier la nouvelle version.
- **Envoyer pour publication** — Lorsque vous êtes satisfait(e) de l’expérience, envoyez votre application pour révision via le plug-in [!DNL ChatGPT] ou le processus de publication du connecteur. Adobe ne contrôle pas ce processus. Consultez la documentation de la plateforme LLM pour connaître les exigences en matière de soumission et le calendrier.
