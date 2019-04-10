---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Atelier pratique : Sites web Azure faciles à gérer : Gestion des modifications et mise à l’échelle | Microsoft Docs'
author: rick-anderson
description: Dans ce laboratoire, découvrez comment Microsoft Azure permet de facilement générer et déployer des sites Web en production.
ms.author: riande
ms.date: 07/16/2014
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: ec0058472f8bc1d8d58e7c78deeb8b6097532510
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59409731"
---
# <a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Atelier pratique : Sites web Azure faciles à gérer : gestion des modifications et de la mise à l’échelle

par [Web Camps Team](https://twitter.com/webcamps)

[Télécharger le Kit de formation de Web Camps](https://aka.ms/webcamps-training-kit)

> Microsoft Azure rend plus facile créer et déployer des sites Web en production. Mais vous n’avez pas terminé lorsque votre application est en ligne, vous n’êtes pas familiarisé ! Vous devrez gérer variation exigences, mises à jour de la base de données, mise à l’échelle et bien plus encore. Heureusement, Azure App Service a prévu, avec de nombreuses fonctionnalités pour vous aider à conserver vos sites en cours d’exécution sans heurts.
>
> Azure offre développement sécurisé et flexible, de déploiement et de mise à l’échelle des options pour n’importe quelle application web de taille. Tirer parti de vos outils existants pour créer et déployer des applications sans vous préoccuper des contraintes de gestion de l’infrastructure.
>
> Approvisionner une application web de production vous-même en quelques minutes par déployer facilement du contenu créé à l’aide de votre outil de développement favori. Vous pouvez déployer un site existant directement à partir de contrôle de code source avec prise en charge de **Git**, **GitHub**, **Bitbucket**, **TFS**et même  **DropBox**. Déployer directement à partir de votre IDE favori ou à partir de scripts à l’aide de **PowerShell** dans Windows ou **CLI** outils s’exécutant sur n’importe quel système d’exploitation. Une fois déployé, maintenir vos sites constamment à jour avec la prise en charge pour le déploiement continu.
>
> Azure fournit des solutions de récupération, de sauvegarde et de stockage cloud évolutives et durables pour toutes les données, volumineuses ou non. Lorsque vous déployez des applications pour un environnement de production, les services de stockage tels que les Tables, les objets BLOB et les bases de données SQL, vous aider à faire évoluer votre application dans le cloud.
>
> Les bases de données SQL, il est important de tenir votre base de données productive lors du déploiement de nouvelles versions de votre application. Merci à **Migrations Entity Framework Code First**, le développement et le déploiement de votre modèle de données a été simplifié pour mettre à jour de vos environnements en quelques minutes. Ce laboratoire pratique vous montrera les différentes rubriques que vous pouvez rencontrer lors du déploiement de votre application web pour les environnements de production dans Microsoft Azure.
>
> Tous les exemples de code et extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [ https://aka.ms/webcamps-training-kit ](https://aka.ms/webcamps-training-kit).
>
> Pour plus de couverture approfondie de cette rubrique, consultez le [développement d’applications Cloud réalistes avec Azure e-book](building-real-world-cloud-apps-with-windows-azure/introduction.md).


<a id="Overview"></a>
## <a name="overview"></a>Vue d'ensemble

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans cet atelier, vous allez apprendre comment :

- Activer les Migrations Entity Framework avec un modèle existant
- Mettre à jour le modèle objet et la base de données en conséquence à l’aide des Migrations Entity Framework
- Déployer sur Azure App Service à l’aide de Git
- Restauration à un déploiement précédent à l’aide du portail de gestion Azure
- Utiliser le stockage Azure à l’échelle une application web
- Configurer l’échelle automatique pour une application web à l’aide du portail de gestion Azure
- Créer et configurer un projet de test de charge dans Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prérequis

Les éléments suivants sont nécessaire pour terminer ce laboratoire pratique :

- [Visual Studio Express 2013 pour le Web](https://www.microsoft.com/visualstudio/) ou supérieur
- [Azure SDK pour .NET 2.2](https://www.microsoft.com/windowsazure/sdk/)
- [Système de contrôle de Version GIT](http://git-scm.com/download)
- Un abonnement Microsoft Azure

    - Inscrivez-vous pour un [version d’évaluation gratuite](https://aka.ms/watk-freetrial)
    - Si vous êtes abonné à MSDN ou plateformes MSDN un Visual Studio Professional, Test Professional, Premium ou Ultimate, activer votre [avantage MSDN](https://aka.ms/watk-msdn) maintenant pour commencer à développer et tester sur Azure
    - [BizSpark](https://aka.ms/watk-bizspark) les membres reçoivent automatiquement Azure avantage via leur Visual Studio Ultimate avec les abonnements MSDN
    - Membres de la [Microsoft Partner Network](https://aka.ms/watk-mpn) programme Cloud Essentials recevoir des crédits mensuels Azure sans frais

<a id="Setup"></a>
### <a name="setup"></a>Installation

Afin d’exécuter les exercices dans cet atelier, vous devez configurer votre environnement tout d’abord.

1. Ouvrez l’Explorateur Windows et accédez à l’atelier **Source** dossier.
2. Avec le bouton droit sur **Setup.cmd** et sélectionnez **exécuter en tant qu’administrateur** pour lancer le processus d’installation qui configure votre environnement et installer les extraits de code Visual Studio pour ce laboratoire.
3. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, confirmez l’action pour continuer.

> [!NOTE]
> Assurez-vous que vous avez activé toutes les dépendances pour ce laboratoire avant d’exécuter le programme d’installation.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>À l’aide d’extraits de Code

Dans le document de laboratoire, vous serez invité à insérer des blocs de code. Pour votre commodité, la majeure partie de ce code est fourni en tant que Visual Studio extraits de Code, vous pouvez y accéder à partir de Visual Studio 2013 pour éviter d’avoir à ajouter manuellement.

> [!NOTE]
> Chaque exercice est accompagnée d’une solution de départ située dans le **commencer** dossier de l’exercice qui vous permet de suivre chaque exercice indépendamment des autres. N’oubliez pas que les extraits de code sont ajoutés au cours d’un exercice sont manquants à partir de ces solutions de démarrage et peut ne pas fonctionnent jusqu'à ce que vous avez terminé l’exercice. Dans le code source pour un exercice, vous y trouverez également un **fin** dossier qui contient une solution Visual Studio avec le code qui résulte d’effectuer les étapes dans l’exercice correspondant. Si vous avez besoin d’aide au cours de cet atelier, vous pouvez utiliser ces solutions en tant que guide.


---

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique inclut les exercices suivants :

1. [À l’aide des Migrations Entity Framework](#Exercise1)
2. [Déploiement d’une application Web dans un environnement intermédiaire](#Exercise2)
3. [Exécution de l’annulation du déploiement en Production](#Exercise3)
4. [Mise à l’échelle à l’aide du stockage Azure](#Exercise4)
5. [À l’aide de la mise à l’échelle pour les applications Web](#Exercise5) (facultatif pour l’édition Visual Studio 2013 Ultimate)

Durée estimée pour effectuer ce laboratoire : **75 minutes**

> [!NOTE]
> Lorsque vous démarrez Visual Studio, vous devez sélectionner une des collections de paramètres prédéfinis. Chaque collection prédéfinie est conçue pour correspondre à un style de développement particulier et détermine les dispositions de fenêtres, le comportement de l’éditeur, extraits de code IntelliSense et les options de boîte de dialogue. Les procédures décrites dans ce laboratoire décrivent les actions nécessaires pour accomplir une tâche donnée dans Visual Studio lorsque vous utilisez le **paramètres de développement généraux** collection. Si vous choisissez une collection de paramètres différents pour votre environnement de développement, il peut y avoir des différences dans les étapes que vous devez prendre en compte.


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Exercice 1 : À l’aide des Migrations Entity Framework

Lorsque vous développez une application, votre modèle de données peut changer au fil du temps. Ces modifications peuvent affecter le modèle existant dans votre base de données (si vous créez une nouvelle version) et il est important de maintenir votre base de données à jour pour empêcher des erreurs.

Pour simplifier le suivi de ces modifications dans votre modèle, **Migrations Entity Framework Code First** automatiquement détecter les modifications de comparaison de votre modèle avec le schéma de base de données et génère du code spécifique pour mettre à jour votre base de données Création de nouveaux *versions* de votre base de données.

Cet exercice vous montre comment activer **Migrations** pour votre application et comment vous pouvez facilement détecter et générer des modifications pour mettre à jour de vos bases de données.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Tâche 1 : activation des Migrations

Dans cette tâche, vous allez passer par les étapes d’activation **Migrations Entity Framework Code First** à la **Geek questionnaire** base de données, la modification du modèle et de comprendre comment ces modifications sont répercutées dans le base de données.

1. Ouvrez Visual Studio et ouvrez le **GeekQuiz.sln** fichier de solution à partir de **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.
2. Générez la solution afin de télécharger et installer le **NuGet** dépendances de package. Pour ce faire, cliquez sur la solution et cliquez sur **générer la Solution** ou appuyez sur **Ctrl + Maj + B**.
3. À partir de la **outils** menu dans Visual Studio, sélectionnez **Gestionnaire de Package NuGet**, puis cliquez sur **Console du Gestionnaire de Package**.
4. Dans le **Console du Gestionnaire de Package**, entrez la commande suivante et appuyez sur **entrée**. Une migration initiale, selon le modèle existant sera créée.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![L’activation de Migrations](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "autorisant la migration")

    *Activation des migrations*

    > [!NOTE]
    > Cette commande ajoute un **Migrations** dossier au projet Geek questionnaire qui contient un fichier appelé **Configuration.cs**. Le **Configuration** classe vous permet de configurer le comportement de Migrations pour votre contexte.
5. Avec Migrations est activées, vous devez mettre à jour le **Configuration** classe pour remplir la base de données avec les données initiales qui **Geek questionnaire** requiert. Sous **Migrations**, remplacez le **Configuration.cs** fichier avec celui situé dans le **Source\Assets** dossier de ce laboratoire.

    > [!NOTE]
    > Dans la mesure où **Migrations** appellera le **Seed** méthode avec chaque mise à jour de la base de données, vous devez être sûr que les enregistrements ne sont pas dupliquées dans la base de données. Le **AddOrUpdate** méthode vous aidera à empêcher les données en double.
6. Pour ajouter une migration initiale, entrez la commande suivante et appuyez sur **entrée**.

    > [!NOTE]
    > Assurez-vous qu’il n’existe aucune base de données nommée &quot;GeekQuizProd&quot; dans votre instance de base de données locale.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Ajout de la migration du schéma de base](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "migration de schéma de base d’ajout")

    *Ajout de la migration du schéma de base*

    > [!NOTE]
    > **Add-Migration** sera structurer la prochaine migration en fonction des modifications apportées à votre modèle depuis la dernière migration a été créée. Dans ce cas, comme il s’agit de la première migration du projet, il ajoutera les scripts pour créer toutes les tables définies dans le **TriviaContext** classe.
7. Exécution de la migration pour mettre à jour de la base de données en exécutant la commande suivante. Pour cette commande, vous spécifierez le **Verbose** indicateur pour afficher les instructions SQL qui est appliquées à la base de données cible.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Création de base de données initiale](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "création de base de données initiale")

    *Création de base de données initiale*

    > [!NOTE]
    > **Mise à jour la base de données** tout en attente de migrations appliquera à la base de données. Dans ce cas, il crée la base de données à l’aide de la chaîne de connexion définie dans votre fichier web.config.
8. Accédez à **vue** menu ou ouvrir **Explorateur d’objets SQL Server**.

    ![Ouvrir dans l’Explorateur d’objets SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "ouvert dans l’Explorateur d’objets SQL Server")

    *Ouvrir dans l’Explorateur d’objets SQL Server*
9. Dans le **Explorateur d’objets SQL Server** fenêtre, connectez-vous à votre instance de base de données locale en double-cliquant sur le **SQL Server** nœud et en sélectionnant **ajouter SQL Server...**  option.

    ![Ajout d’une Instance SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Ajout d’une Instance SQL Server")

    *Ajout d’une instance de SQL Server à l’Explorateur d’objets SQL Server*
10. Définir le **nom du serveur** à *(localdb) \v11.0* et laissez **l’authentification Windows** comme votre mode d’authentification. Cliquez sur **Connect** pour continuer.

    ![Connexion à la base de données locale](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "connexion à la base de données locale")

    *Connexion à la base de données locale*
11. Ouvrez le **GeekQuizProd** de base de données et développez le **Tables** nœud. Comme vous pouvez le voir, le **Update-Database** commande générée toutes les tables définies dans le **TriviaContext** classe. Recherchez le **dbo. TriviaQuestions** table et ouvrez le nœud colonnes. Dans la tâche suivante, vous ajoutez une nouvelle colonne à cette table et mettre à jour la base de données à l’aide **Migrations**.

    ![Trivia Questions colonnes](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia Questions des colonnes")

    *Trivia Questions des colonnes*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Tâche 2 : schéma de base de données mise à jour à l’aide de Migrations

Dans cette tâche, vous allez utiliser **Migrations Entity Framework Code First** pour détecter une modification dans votre modèle et de générer le code nécessaire pour mettre à jour de la base de données. Vous mettrez à jour la **TriviaQuestions** entité en lui ajoutant une nouvelle propriété. Puis vous allez exécuter des commandes pour créer une nouvelle Migration pour inclure la nouvelle colonne dans la table.

1. Dans **l’Explorateur de solutions**, double-cliquez sur le **TriviaQuestion.cs** fichier situé à l’intérieur de la **modèles** dossier.
2. Ajouter une nouvelle propriété nommée **indicateur**, comme illustré dans l’extrait de code suivant.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. Dans le **Console du Gestionnaire de Package**, entrez la commande suivante et appuyez sur **entrée**. Une nouvelle migration sera créée qui reflète la modification dans notre modèle.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")

    *Add-Migration*

    > [!NOTE]
    > Un fichier de Migration se compose de deux méthodes, **des** et **vers le bas**.
    >
    > - Le **des** méthode permet de spécifier ce qui modifie la version actuelle de nos besoins de l’application à appliquer à la base de données.
    > - Le **vers le bas** est utilisé pour annuler les modifications que nous avons ajouté à la **des** (méthode).
    >
    > Lors de la Migration de base de données met à jour la base de données, il s’exécutera toutes les migrations dans l’ordre d’horodatage et ceux qui n’ont pas été utilisés depuis la dernière mise à jour (la \_MigrationHistory table effectue le suivi des migrations qui ont été appliquées). Le **des** méthode de toutes les migrations sera appelé et va effectuer les modifications que nous avons spécifié à la base de données. Si nous décidons de revenir à une migration précédente, le **vers le bas** méthode sera appelée pour rétablir les modifications dans l’ordre inverse.
4. Dans le **Console du Gestionnaire de Package**, entrez la commande suivante et appuyez sur **entrée**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. La sortie de la **Update-Database** commande générée une **Alter Table** instruction SQL pour ajouter une nouvelle colonne à la **TriviaQuestions** table, comme illustré dans l’image ci-dessous.

    ![Ajoutez l’instruction SQL de colonne générée](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "ajouter l’instruction SQL de colonne générée")

    *Ajoutez l’instruction SQL de colonne générée*
6. Dans **Explorateur d’objets SQL Server**, actualiser le **dbo. TriviaQuestions** table et vérifiez que la nouvelle **indicateur** colonne s’affiche.

    ![Affichage de la nouvelle colonne d’indicateur](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "montrant la nouvelle colonne d’indicateur")

    *Affichage de la nouvelle colonne d’indicateur*
7. Dans le **TriviaQuestion.cs** éditeur, ajoutez un **StringLength** contrainte à la *indicateur* propriété, comme indiqué dans l’extrait de code suivant.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. Dans le **Console du Gestionnaire de Package**, entrez la commande suivante et appuyez sur **entrée**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. Dans le **Console du Gestionnaire de Package**, entrez la commande suivante et appuyez sur **entrée**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. La sortie de la **Update-Database** commande générée une **Alter Table** instruction SQL pour mettre à jour le *indicateur* type de colonne de la **TriviaQuestions** table, comme illustré dans l’image ci-dessous.

    ![Modifier l’instruction SQL de colonne générée](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "modifier l’instruction SQL de colonne générée")

    *Modifier l’instruction SQL de colonne générée*
11. Dans **Explorateur d’objets SQL Server**, actualiser le **dbo. TriviaQuestions** table et vérifiez que le **indicateur** est de type de colonne **nvarchar(150)**.

    ![Affichage de la nouvelle contrainte](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "montrant la nouvelle contrainte")

    *Affichage de la nouvelle contrainte*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Exercice 2 : Déploiement d’une application Web dans un environnement intermédiaire

**Applications Web dans Azure App Service** vous permet d’effectuer la publication intermédiaire. La publication intermédiaire crée un emplacement de site intermédiaire pour chaque site de production par défaut et vous permet d’échanger ces emplacements sans temps d’arrêt. Il s’agit vraiment utile pour valider les modifications avant de libérer au public, intégrer le contenu du site incrémentielle et restauration si des modifications ne fonctionnent pas comme prévu.

Dans cet exercice, vous allez déployer le **Geek questionnaire** application dans l’environnement intermédiaire de votre application web à l’aide du contrôle de code source Git. Pour ce faire, vous créez l’application web et configurer les composants requis sur le portail de gestion, configurer un **Git** référentiel et push de l’application le code source à partir de votre ordinateur local à l’emplacement intermédiaire. Vous met également à jour votre base de données de production avec le **Migrations Code First** vous avez créé dans l’exercice précédent. Vous allez ensuite exécuter l’application dans cet environnement de test pour vérifier son fonctionnement. Une fois que vous jugez qu’il fonctionne en fonction de vos attentes, vous permet de déclarer l’application en production.

> [!NOTE]
> Pour activer la publication intermédiaire, l’application web doit être dans **mode Standard**. Notez que des frais supplémentaires seront facturés si vous modifiez votre application web en mode Standard. Pour plus d’informations sur la tarification, consultez [tarification d’App Service](https://azure.microsoft.com/pricing/details/app-service/).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Tâche 1 : création d’une application Web dans Azure App Service

Dans cette tâche, vous allez créer une application web dans **Azure App Service** à partir du portail de gestion. Vous allez également configurer un **base de données SQL** pour conserver les données d’application et configurer un référentiel Git local pour le contrôle de code source.

1. Accédez à la [portail de gestion Azure](https://manage.windowsazure.com) et connectez-vous en utilisant le compte Microsoft associé à votre abonnement.

    ![Connectez-vous au portail de gestion Azure](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Connectez-vous au portail de gestion Azure*
2. Cliquez sur **New** dans la barre de commandes au bas de la page.

    ![Création d’une application web](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "création d’une application web")

    *Création d’une application web*
3. Cliquez sur **calcul**, **site Web** , puis **création personnalisée**.

    ![Création d’une application web à l’aide de la création personnalisée](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "création d’une application web à l’aide de la création personnalisée")

    *Création d’une application web à l’aide de la création personnalisée*
4. Dans le **nouveau site Web - création personnalisée** boîte de dialogue zone, fournissez une disponible **URL** (par exemple, *geek-questionnaire*), sélectionnez un emplacement dans le **région** liste déroulante, puis sélectionnez **créer une base de données SQL** dans le **base de données** liste déroulante. Enfin, sélectionnez le **publier à partir du contrôle de code source** case à cocher et cliquez sur **suivant**.

    ![Personnalisation de la nouvelle application web](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Personnalisation de la nouvelle application web*
5. Spécifiez les informations suivantes pour les paramètres de base de données :

   - Dans le **nom** texte, entrez un nom de base de données (par exemple, *geekquiz\_db*)
   - Dans le serveur **déroulante** liste, sélectionnez **serveur de base de données SQL nouveau**. Vous pouvez également sélectionner un serveur existant.
   - Dans le **nom d’utilisateur de base de données** et **mot de passe de base de données** zones, entrez le nom d’utilisateur administrateur et le mot de passe pour le serveur de base de données SQL. Si vous sélectionnez un serveur, vous avez déjà créé, vous serez invité pour le mot de passe.

     ![En spécifiant les paramètres de base de données](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     *En spécifiant les paramètres de base de données*
6. Cliquez sur **Suivant** pour continuer.
7. Sélectionnez **référentiel Git Local** pour le contrôle de code source à utiliser et cliquez sur **suivant**.

    > [!NOTE]
    > Vous pouvez être invité pour les informations d’identification de déploiement (un nom d’utilisateur et mot de passe).

    ![Création du dépôt Git](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Création du dépôt Git*
8. Attendez que la nouvelle application web est créée.

    > [!NOTE]
    > Par défaut, Azure fournit les domaines au *azurewebsites.net* , mais vous donne également la possibilité de définir des domaines personnalisés à l’aide du portail de gestion Azure. Toutefois, vous pouvez uniquement gérer des domaines personnalisés si vous utilisez certains modes d’Azure App Service.
    >
    > Azure App Service est disponible dans les éditions gratuit, partagé, Basic, Standard et Premium. En mode gratuit et partagé, toutes les applications web s’exécutent dans un environnement mutualisé et possèdent des quotas pour l’utilisation du processeur, mémoire et réseau. Le nombre maximal d’applications gratuites peut-être varier avec votre plan. En mode Standard, vous choisissez les applications qui s’exécutent sur des machines virtuelles dédiées qui correspondent à Azure standard des ressources de calcul. Vous pouvez trouver la configuration de mode d’application web dans le **mise à l’échelle** menu de votre application web.
    >
    > ![Modes d’Azure App Service](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Modes Azure App Service")
    >
    > Si vous utilisez **partagé** ou **Standard** mode, vous serez en mesure de gérer les domaines personnalisés pour votre application web en accédant à votre application **configurer** menu et en cliquant sur **Gérer les domaines** sous *des noms de domaine*.
    >
    > ![Gérer les domaines](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "gérer les domaines")
    >
    > ![Gérer les domaines personnalisés](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "gérer les domaines personnalisés")
9. Une fois que l’application web est créée, cliquez sur le lien situé sous le **URL** colonne pour vérifier que la nouvelle application web est en cours d’exécution.

    ![Navigation vers la nouvelle application web](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Navigation vers la nouvelle application web*

    ![application Web en cours d’exécution](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *application Web en cours d’exécution*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Tâche 2 : création de la base de données SQL de Production

Dans cette tâche, vous allez utiliser le **Migrations Entity Framework Code First** pour créer la base de données ciblant les **base de données SQL Azure** instance que vous avez créé dans la tâche précédente.

1. Dans le portail de gestion, accédez à l’application web que vous avez créé dans la tâche précédente et à ses **tableau de bord**.
2. Dans le **tableau de bord** , cliquez sur **afficher les chaînes de connexion** situé sous le **aperçu rapide** section.

    ![Afficher les chaînes de connexion](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "afficher les chaînes de connexion")

    *Afficher les chaînes de connexion*
3. Copie le **chaîne de connexion** valeur et fermer la boîte de dialogue.

    ![Chaîne de connexion dans le portail de gestion Azure](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "chaîne de connexion dans le portail de gestion Azure")

    *Chaîne de connexion dans le portail de gestion Azure*
4. Cliquez sur **bases de données SQL** pour afficher la liste des bases de données SQL Azure

    ![Menu de la base de données SQL](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "menu de la base de données SQL")

    *Menu de la base de données SQL*
5. Recherchez la base de données que vous avez créé dans la tâche précédente, puis cliquez sur le serveur.

    ![Serveur de base de données SQL](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "serveur de base de données SQL")

    *Serveur de base de données SQL*
6. Dans le **démarrage rapide** page du serveur, cliquez sur **configurer**.

    ![Menu configuration](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "menu configurer")

    *Configurer le menu*
7. Dans le **adresses IP autorisées** section, cliquez sur **ajouter aux adresses IP autorisées** lien pour activer votre adresse IP pour se connecter au serveur de base de données SQL.

    ![Adresses IP autorisées](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "adresses IP autorisées")

    *Adresses IP autorisées*
8. Cliquez sur **enregistrer** en bas de la page pour procéder à l’étape.
9. Revenez à Visual Studio.
10. Dans le **Console du Gestionnaire de Package**, exécutez la commande suivante en remplaçant *[YOUR-CONNECTION-STRING]* espace réservé par la chaîne de connexion que vous avez copiée à partir d’Azure

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Ciblage de Windows Azure SQL Database de la base de données mise à jour](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "ciblant Windows Azure SQL Database de la base de données mise à jour")

    *Mettre à jour de la base de données ciblant la base de données SQL Azure*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Tâche 3 : déploiement Geek questionnaire dans l’environnement intermédiaire à l’aide de Git

Dans cette tâche, vous allez activer la publication intermédiaire dans votre application web. Ensuite, vous allez utiliser Git pour publier l’application Geek questionnaire directement à partir de votre ordinateur local dans l’environnement intermédiaire de votre application web.

1. Revenez au portail et cliquez sur le nom de l’application web sous le **nom** colonne pour afficher les pages de gestion.

    ![Ouvrir les pages de gestion d’application web](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Ouvrir les pages de gestion d’application web*
2. Accédez à la **mise à l’échelle** page. Sous le **général** section, sélectionnez **Standard** pour la configuration et cliquez sur **enregistrer** dans la barre de commandes.

    > [!NOTE]
    > Pour exécuter toutes les applications web dans la région actuelle et l’abonnement dans **Standard** mode, laissez le **sélectionner tout** case cochée dans la **choisir les Sites** configuration. Sinon, désactivez le **sélectionner tout** case à cocher.

    ![La mise à niveau de l’application web vers le mode Standard](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "la mise à niveau de l’application web vers le mode Standard")

    *La mise à niveau de l’application Web vers le mode Standard*
3. Cliquez sur **Oui** pour confirmer les modifications.

    ![Confirmer la modification sur le mode Standard](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "poursuivre la modification du mode application web")

    *Confirmer la modification sur le mode Standard*
4. Accédez à la **tableau de bord** page et cliquez sur **Enable-publication intermédiaire** sous le **aperçu rapide** section.

    ![L’activation de la publication intermédiaire](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "activation-publication intermédiaire")

    *L’activation de la publication intermédiaire*
5. Cliquez sur **Oui** pour activer la publication intermédiaire.

    ![Confirmation-publication intermédiaire](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "en cliquant sur Oui pour activer la publication intermédiaire")

    *Confirmation de la publication intermédiaire*
6. Dans la liste des applications web, développez la marque à gauche du nom de votre application web pour afficher l’emplacement de site intermédiaire. Il porte le nom de votre application web suivie ***(intermédiaire)***. Cliquez sur le site intermédiaire pour accéder à la page de gestion.

    ![Navigation vers l’application web intermédiaire](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "accédant à l’application web intermédiaire")

    *Navigation vers l’application intermédiaire*
7. Notez que cette page de gestion he ressemble à la page de gestion de n’importe quel autre application web. Accédez à la **déploiements** page puis copiez le **URL Git** valeur. Vous l’utiliserez plus loin dans cet exercice.

    ![Copie de la valeur de l’URL Git](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Copie de la valeur de l’URL Git*
8. Ouvrez une nouvelle **Git Bash** console et exécutez les commandes suivantes. Mise à jour le *[votre-APPLICATION-PATH]* espace réservé par le chemin d’accès à la **GeekQuiz** solution, située dans le **Source\Ex1-DeployingWebSiteToStaging\Begin** dossier de Ce laboratoire.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![L’initialisation de GIT et de la première validation](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *L’initialisation de GIT et de la première validation*
9. Exécutez la commande suivante pour envoyer votre application web à l’instance distante **Git** référentiel. Remplacez l’espace réservé par l’URL que vous avez obtenue à partir du portail de gestion. Vous demandera votre mot de passe de déploiement.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Placez dans Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *En exécutant un push vers Azure*

    > [!NOTE]
    > Lorsque vous déployez du contenu sur l’hôte FTP ou le référentiel GIT d’une application web, vous devez vous authentifier à l’aide de la **informations d’identification de déploiement** que vous avez créé à partir de l’application web **démarrage rapide** ou **tableau de bord**  pages de gestion. Si vous ne connaissez pas vos informations d’identification de déploiement vous pouvez facilement les réinitialiser à l’aide du portail de gestion. Ouvrez l’application web **tableau de bord** page et cliquez sur le **réinitialiser vos informations d’identification de déploiement** lien. Fournissez un nouveau mot de passe et cliquez sur **OK**. Informations d’identification de déploiement sont valides pour une utilisation avec toutes les applications web associées à votre abonnement.
10. Afin de vérifier que l’application web a été envoyée avec succès sur Azure, revenez au portail de gestion, puis cliquez sur **sites Web**.
11. Localisez votre application web et développez l’entrée pour afficher l’emplacement de site intermédiaire. Cliquez sur son **nom** pour accéder à la page de gestion.
12. Cliquez sur **déploiements** pour voir les **l’historique de déploiement**. Vérifiez qu’il existe un **déploiement actif** avec votre  *&quot;validation initiale&quot;*.

    ![Déploiement actif](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Déploiement actif*
13. Enfin, cliquez sur **Parcourir** dans la barre de commandes pour accéder à l’application web.

    ![Parcourir l’application web](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Parcourir l’application web*
14. Si l’application est déployée avec succès, vous verrez la page de connexion Geek questionnaire.

    > [!NOTE]
    > L’adresse URL de l’application déployée contient le nom de votre application web suivie *-intermédiaire*.

    ![Application qui s’exécute dans l’environnement intermédiaire](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *Application qui s’exécute dans l’environnement intermédiaire*
15. Si vous souhaitez Explorer l’application, cliquez sur **inscrire** pour inscrire un nouvel utilisateur. Terminez les détails du compte en entrant un nom d’utilisateur, adresse de messagerie et un mot de passe. Ensuite, l’application affiche la première question du questionnaire. Répondre à quelques questions pour vous assurer qu’il fonctionne comme prévu.

    ![Application prête à être utilisée](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Application prête à être utilisée*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Tâche 4 – la promotion de l’application Web en Production

Maintenant que vous avez vérifié que l’application web fonctionne correctement dans l’environnement intermédiaire, vous êtes prêt à passer en production. Dans cette tâche, vous vous remplacerez l’emplacement de site intermédiaire avec l’emplacement de site de production.

1. Revenez au portail de gestion et sélectionnez l’emplacement de site intermédiaire. Cliquez sur **échange** dans la barre de commandes.

    ![Passez à la production](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *Passez à la production*
2. Cliquez sur **Oui** dans la boîte de dialogue de confirmation pour poursuivre l’opération d’échange. Azure échangera immédiatement le contenu du site de production avec le contenu du site intermédiaire.

    > [!NOTE]
    > Certains paramètres de la version intermédiaire sont automatiquement copiées vers la version de production (par exemple, chaîne de connexion remplacements, des mappages de gestionnaires, etc.), mais d’autres paramètres ne changera pas (par exemple, les points de terminaison DNS, les liaisons SSL, etc.).

    ![Confirmer l’opération d’échange](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Confirmer l’opération d’échange*
3. Une fois que l’échange est terminé, sélectionnez l’emplacement de production et cliquez sur **Parcourir** dans la barre de commandes pour ouvrir le site de production. Notez l’URL dans la barre d’adresses.

    > [!NOTE]
    > Vous devrez peut-être actualiser votre navigateur pour effacer le cache. Dans Internet Explorer, vous pouvez accomplir cela en appuyant sur **CTRL + R**.

    ![Application Web s’exécutant dans l’environnement de production](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. Dans le **GitBash** de la console, mettez à jour de l’URL distante pour le référentiel Git local pour cibler l’emplacement de production. Pour ce faire, exécutez la commande suivante en remplaçant les espaces réservés avec votre nom d’utilisateur de déploiement et le nom de votre application web.

    > [!NOTE]
    > Dans les exercices suivants, vous transmet les modifications au site de production au lieu de transit uniquement pour la simplicité du laboratoire. Dans un scénario réel, il est recommandé de vérifier les modifications dans l’environnement intermédiaire avant la promotion en production.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Exercice 3 : Exécution de l’annulation du déploiement en Production

Il existe des scénarios où vous n’avez pas un emplacement intermédiaire pour effectuer l’échange à chaud entre intermédiaire et de production, par exemple, si vous travaillez avec **gratuit** ou **partagé** mode. Dans ces scénarios, vous devez tester votre application dans un environnement de test, localement ou dans un site distant, avant de déployer en production. Toutefois, il est possible qu’un problème n’a ne pas détecté pendant la phase de test peut-être survenir dans le site de production. Dans ce cas, il est important de disposer d’un mécanisme permettant de facilement basculer vers une version précédente et plus stable de l’application aussi rapidement que possible.

Dans **Azure App Service**, déploiement continu à partir du contrôle de code source rend cette possible grâce à la **redéployer** action disponible dans le portail de gestion. Azure effectue le suivi des déploiements associés à la validation de la poussée vers le référentiel et fournit une option permettant de redéployer votre application à l’aide de vos déploiements précédents, à tout moment.

Dans cet exercice, vous allez effectuer une modification du code dans le **Geek questionnaire** application intentionnellement injecte un *bogue*. Vous allez déployer l’application en production pour voir l’erreur, et vous serez tirer parti de la fonctionnalité de redéploiement pour revenir à l’état précédent.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Tâche 1 – la mise à jour de l’Application de questionnaire Geek

Dans cette tâche, vous devez refactoriser un petit morceau de code de la **TriviaController** classe extraire une partie de la logique qui Récupère l’option questionnaire sélectionné à partir de la base de données dans une nouvelle méthode.

1. Basculez vers l’instance de Visual Studio avec le **GeekQuiz** solution à partir de l’exercice précédent.
2. Dans **l’Explorateur de solutions**, ouvrez le **TriviaController.cs** de fichiers à l’intérieur de la **contrôleurs** dossier.
3. Recherchez le **StoreAsync** (méthode) et sélectionnez le code mis en surbrillance dans la figure suivante.

    ![En sélectionnant le code](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *En sélectionnant le code*
4. Cliquez sur le code sélectionné, développez le **refactoriser** menu et sélectionnez **extraire la méthode...** .

    ![Extraire le code en tant que nouvelle méthode](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *En sélectionnant la méthode d’extraction*
5. Dans le **extraire la méthode** boîte de dialogue, nom de la nouvelle méthode *MatchesOption* et cliquez sur **OK**.

    ![En spécifiant le nom de méthode](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *En spécifiant le nom de la méthode extraite*
6. Le code sélectionné est ensuite extrait dans le **MatchesOption** (méthode). Le code obtenu est illustré dans l’extrait de code suivant.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Appuyez sur **CTRL + S** pour enregistrer les modifications.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Tâche 2 : redéploiement de l’Application de questionnaire Geek

Vous allez maintenant pousser les modifications apportées dans la tâche précédente dans le référentiel, ce qui déclenchera un nouveau déploiement sur l’environnement de production. Ensuite, de dépannage un problème à l’aide de la **les outils de développement F12** fournie par Internet Explorer et effectuer une restauration sur le déploiement précédent à partir du portail de gestion Azure.

1. Ouvrez une nouvelle **Git Bash** console pour déployer l’application mis à jour dans Azure App Service.
2. Exécutez les commandes suivantes pour envoyer les modifications vers Azure. Mise à jour le *[votre-APPLICATION-PATH]* espace réservé par le chemin d’accès à la **GeekQuiz** solution. Vous demandera votre mot de passe de déploiement.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Placez le code refactorisé dans Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *Placez le code refactorisé dans Azure*
3. Ouvrez Internet Explorer et accédez à votre application web (par exemple, `http://<your-web-site>.azurewebsites.net`). Connectez-vous en utilisant les informations d’identification créées précédemment.
4. Appuyez sur **F12** pour lancer les outils de développement, sélectionnez le **réseau** onglet et cliquez sur le **lire** bouton pour commencer l’enregistrement.

    ![Démarrage de l’enregistrement réseau](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "à partir de l’enregistrement du réseau")

    *Démarrage de l’enregistrement du réseau*
5. Sélectionnez une option du questionnaire. Vous verrez que rien ne se produit.
6. Dans le **F12** fenêtre, l’entrée correspondant à la demande HTTP POST montre HTTP **500** résultat.

    ![HTTP 500 Erreur](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *HTTP 500 Erreur*
7. Sélectionnez le **Console** onglet. Une erreur est enregistrée avec les détails de la cause.

    ![Erreur connecté](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Erreur connecté*
8. Recherchez l’article détails de l’erreur. Clairement, cette erreur est provoquée par le code de refactorisation vous validées dans les étapes précédentes.

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.
9. Ne fermez pas le navigateur.
10. Dans une nouvelle instance du navigateur, accédez à la [portail de gestion Azure](https://manage.windowsazure.com) et connectez-vous en utilisant le compte Microsoft associé à votre abonnement.
11. Sélectionnez **sites Web** et cliquez sur l’application web que vous avez créé dans l’exercice 2.
12. Accédez à la **déploiements** page. Notez que toutes les validations effectuées sont répertoriées dans l’historique de déploiement.

    ![Liste des déploiements existants](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Liste des déploiements existants*
13. Sélectionnez la validation précédente et cliquez sur **redéployer** sur la barre de commandes.

    ![Redéploiement de la validation précédente](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Redéploiement de la validation précédente*
14. Lorsque vous êtes invité à confirmer, cliquez sur **Oui**.

    ![Confirmer le redéploiement](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Lorsque le déploiement terminé, basculez vers l’instance de navigateur avec votre application web et la presse **CTRL + F5**.
16. Cliquez sur les options. L’animation retournement sera désormais avoir lieu et le résultat (*correctes/incorrectes*) s’affiche.
17. (Facultatif) Basculez vers le **Git Bash** console et exécutez les commandes suivantes pour rétablir la validation précédente.

    > [!NOTE]
    > Ces commandes créent une nouvelle validation qui annule toutes les modifications dans le référentiel Git qui ont été apportées à la validation incorrecte. Azure puis redéployer l’application à l’aide de la nouvelle validation.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Exercice 4 : Mise à l’échelle à l’aide du stockage Azure

**Objets BLOB** constituent la manière la plus simple de stocker de grandes quantités de texte non structuré ou des données binaires telles que la vidéo, audio et images. Déplacez le contenu statique de votre application vers le stockage, permet à l’échelle de votre application en fournissant des images ou des documents directement dans le navigateur.

Dans cet exercice, vous allez déplacer le contenu statique de votre application à un conteneur d’objets Blob. Puis vous allez configurer votre application afin d’ajouter un **règle de réécriture d’URL ASP.NET** dans le **Web.config** pour rediriger votre contenu vers le conteneur d’objets Blob.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Tâche 1 : création d’un compte de stockage Azure

Dans cette tâche, vous allez apprendre à créer un compte de stockage à l’aide du portail de gestion.

1. Accédez à la [portail de gestion Azure](https://manage.windowsazure.com) et connectez-vous en utilisant le compte Microsoft associé à votre abonnement.
2. Sélectionnez **New | Data Services | Stockage | Création rapide** pour commencer à créer un compte de stockage. Entrez un nom unique pour le compte, puis sélectionnez un **région** dans la liste. Cliquez sur **créer un compte de stockage** pour continuer.

    ![Création d’un compte de stockage](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "création d’un compte de stockage")

    *Création d’un compte de stockage*
3. Dans le **stockage** section, attendez que l’état du nouveau compte de stockage devient *Online* pour pouvoir continuer avec l’étape suivante.

    ![Compte de stockage créé](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "compte de stockage créé")

    *Compte de stockage créé*
4. Cliquez sur le nom de compte de stockage, puis sur le **tableau de bord** lien en haut de la page. Le **tableau de bord** page vous fournit des informations sur l’état du compte et les points de terminaison de service qui peuvent être utilisées dans vos applications.

    ![Afficher le tableau de bord de compte de stockage](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "afficher le tableau de bord de compte de stockage")

    *Afficher le tableau de bord de compte de stockage*
5. Cliquez sur le **gérer les clés d’accès** dans la barre de navigation.

    ![Bouton de gérer les clés d’accès](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "bouton de gérer les clés d’accès")

    *Gérer les clés d’accès*
6. Dans le **gérer les clés d’accès** boîte de dialogue, copiez la **nom de compte de stockage** et **clé d’accès primaire** comme vous en aurez besoin dans l’exercice suivant. Ensuite, fermez la boîte de dialogue.

    ![Boîte de dialogue de clé d’accès gérer](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "boîte de dialogue Gérer la clé de l’accès")

    *Gérer la boîte de dialogue clé d’accès*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Tâche 2 : téléchargement d’un élément multimédia dans le stockage Blob Azure

Dans cette tâche, vous allez utiliser la fenêtre Explorateur de serveurs à partir de Visual Studio pour vous connecter à votre compte de stockage. Puis vous créez un conteneur d’objets blob et charger un fichier avec le logo Geek questionnaire dans le conteneur.

1. Basculez vers l’instance de Visual Studio avec le **GeekQuiz** solution à partir de l’exercice précédent.
2. Dans la barre de menus, sélectionnez **vue** puis cliquez sur **Explorateur de serveurs**.
3. Dans **Explorateur de serveurs**, cliquez sur le **Azure** nœud et sélectionnez **se connecter à Azure...** . Connectez-vous à l’aide de compte Microsoft associé à votre abonnement.

    ![Se connecter à Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Se connecter à Azure*
4. Développez le **Azure** nœud, avec le bouton droit **stockage** et sélectionnez **attacher un stockage externe...** .
5. Dans le **ajouter un nouveau compte de stockage** boîte de dialogue, entrez le **nom du compte** et **clé de compte** vous avez obtenu dans la tâche précédente et cliquez sur **OK**.

    ![Ajouter la boîte de dialogue Nouveau compte de stockage](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Ajouter la boîte de dialogue Nouveau compte de stockage*
6. Votre compte de stockage doit apparaître sous le **stockage** nœud. Développez votre compte de stockage, cliquez sur **Blobs** et sélectionnez **créer un conteneur d’objets Blob...** .

    ![Créer le conteneur d’objets Blob](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "créer le conteneur d’objets Blob")

    *Créer le conteneur d’objets Blob*
7. Dans le **créer un conteneur d’objets Blob** boîte de dialogue zone, entrez un nom pour le conteneur d’objets blob, puis cliquez sur **OK**.

    ![Boîte de dialogue Créer conteneur d’objets Blob](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "boîte de dialogue Créer un conteneur Blob")

    *Créer la boîte de dialogue de conteneur d’objets Blob*
8. Le conteneur d’objets blob doit être ajouté à la **Blobs** nœud. Modifier les autorisations d’accès dans le conteneur pour rendre le conteneur public. Pour ce faire, cliquez sur le **images** conteneur, puis sélectionnez **propriétés**.

    ![Propriétés du conteneur de l’image](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "propriétés du conteneur de l’image")

    *Propriétés de conteneur d’images*
9. Dans le **propriétés** fenêtre, définissez la **accès en lecture Public** à **conteneur**.

    ![La modification de propriété de l’accès en lecture public](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "la modification de propriété de l’accès en lecture public")

    *La modification de propriété de l’accès en lecture public*
10. Lorsque vous y êtes invité si vous êtes sûr que vous souhaitez modifier la propriété d’accès public, cliquez sur **Oui**.

    ![Avertissement de Microsoft Visual Studio](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "avertissement de Microsoft Visual Studio")

    *Avertissement de Microsoft Visual Studio*
11. Dans **Explorateur de serveurs**, avec le bouton droit dans le **images** conteneur d’objets blob et sélectionnez **conteneur d’objets Blob vue**.

    ![Afficher le conteneur d’objets Blob](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "afficher le conteneur d’objets Blob")

    *Conteneur d’objets Blob de vue*
12. Le conteneur d’images doit s’ouvrir dans une nouvelle fenêtre et une légende sans entrées doit être indiquée. Cliquez sur le **télécharger** icône pour charger un fichier dans le conteneur d’objets blob.

    ![Le conteneur d’images sans entrées](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "le conteneur d’Images sans entrées")

    *Conteneur d’images sans entrées*
13. Dans le **charger l’objet Blob** boîte de dialogue, accédez à la **actifs** dossier du laboratoire. Sélectionnez le **logo-big.png** de fichier et cliquez sur **Open**.
14. Attendez que le fichier est chargé. Lorsque le téléchargement terminé, le fichier doit être répertorié dans le conteneur d’images. Cliquez sur l’entrée du fichier et sélectionnez **copier l’URL**.

    ![Copier l’URL de blob](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "copier l’URL du fichier blob")

    *Copier l’URL de blob*
15. Ouvrez Internet Explorer et collez l’URL. L’image suivante doit figurer dans le navigateur.

    ![image de logo-big.png à partir de stockage d’objets Blob Windows](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "image de logo-big.png à partir du stockage")

    *image de logo-big.png depuis le stockage Blob Azure*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Tâche 3 : mise à jour de la Solution pour consommer du contenu statique à partir du stockage d’objets Blob Azure

Dans cette tâche, vous allez configurer le **GeekQuiz** solution pour utiliser l’image téléchargée vers le stockage Blob Azure (au lieu de l’image est situé dans l’application web) en ajoutant une règle de réécriture d’URL ASP.NET dans le **web.config**fichier.

1. Dans Visual Studio, ouvrez le **Web.config** de fichiers à l’intérieur de la **GeekQuiz** de projet et recherchez le **&lt;system.webServer&gt;** élément.
2. Ajoutez le code suivant pour ajouter une réécriture d’URL règle, la mise à jour de l’espace réservé avec le nom de votre compte de stockage.

    (Code Snippet - *WebSitesInProduction - Ex4 - UrlRewriteRule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > Réécriture d’URL consiste à intercepter une requête Web entrante et la redirection de la demande vers une autre ressource. Le règles de réécriture d’URL indique au moteur de réécriture quand une demande doit être redirigé, et vers lequel doivent leur être redirigés. Une règle de réécriture se compose de deux chaînes : le modèle à rechercher dans l’URL demandée (en règle générale, l’utilisation d’expressions régulières), et la chaîne de remplacement du modèle, si trouvée. Pour plus d’informations, consultez [réécriture d’URL dans ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).
3. Appuyez sur **CTRL + S** pour enregistrer les modifications.
4. Ouvrez une nouvelle **Git Bash** console pour déployer l’application mis à jour dans Azure App Service.
5. Exécutez les commandes suivantes pour envoyer les modifications vers Azure. Mise à jour le *[votre-APPLICATION-PATH]* espace réservé par le chemin d’accès à la **GeekQuiz** solution. Vous demandera votre mot de passe de déploiement.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Déploiement de mise à jour vers Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Déploiement de mise à jour vers Azure*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Tâche 4 : vérification

Dans cette tâche, vous allez utiliser **Internet Explorer** pour parcourir le **Geek questionnaire** application et vérifiez que l’URL de réécrire la règle pour le fonctionnement des images et que vous êtes redirigés vers l’image hébergée sur **Blob Azure Stockage**.

1. Ouvrez Internet Explorer et accédez à votre application web (par exemple, `http://<your-web-site>.azurewebsites.net`). Connectez-vous en utilisant les informations d’identification créées précédemment.

    ![Montrant l’application web Geek questionnaire avec l’image](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "montrant l’application web Geek questionnaire avec l’image")

    *Montrant l’application web Geek questionnaire avec l’image*
2. Appuyez sur **F12** pour lancer les outils de développement, sélectionnez le **réseau** onglet et démarrer l’enregistrement.

    ![Démarrage de l’enregistrement réseau](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "à partir de l’enregistrement du réseau")

    *Démarrage de l’enregistrement du réseau*
3. Appuyez sur **CTRL + F5** pour actualiser la page web.
4. Une fois le chargement de la page a terminé, vous devez voir une requête HTTP pour le **/img/logo-big.png** URL avec un HTTP **301** résultat (redirection) et une autre demande de `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL avec un HTTP **200** résultat.

    ![Vérification de la redirection d’URL](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "montrant la redirection dans les outils de développement")

    *Vérification de la redirection d’URL*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Exercice 5 : À l’aide de la mise à l’échelle pour les applications Web

> [!NOTE]
> Cet exercice est facultatif, car elle nécessite la prise en charge pour la charge Web &amp; qui est uniquement disponible pour les tests de performances **Visual Studio 2013 Ultimate Edition**. Pour plus d’informations sur les fonctionnalités spécifiques de Visual Studio 2013, comparer les versions [ici](https://www.microsoft.com/visualstudio/eng/products/compare).


**Azure App Service Web Apps** fournit la fonctionnalité de mise à l’échelle pour les applications web exécutées **Mode Standard**. Mise à l’échelle permet Azure automatiquement à l’échelle le nombre d’instances de votre application web en fonction de la charge. Lorsque l’échelle automatique est activée, Azure vérifie l’UC de votre application web une fois toutes les cinq minutes et ajoute les instances en fonction des besoins à ce stade dans le temps. Si l’utilisation du processeur est faible, Azure supprime des instances toutes les deux heures pour vous assurer que les performances de votre application web ne sont pas dégradées.

Dans cet exercice, vous allez passer par les étapes requises pour configurer le **mise à l’échelle** des fonctionnalités pour le **Geek questionnaire** application web. Vous allez vérifier cette fonctionnalité en exécutant un test de charge de Visual Studio pour générer suffisamment de charge du processeur sur l’application pour déclencher une mise à niveau de l’instance.

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Tâche 1 : configuration de l’échelle automatique basée sur la mesure du processeur

Dans cette tâche vous allez utiliser le portail de gestion Azure pour activer la fonctionnalité de mise à l’échelle de l’application web que vous avez créé dans l’exercice 2.

1. Dans le [portail de gestion Azure](https://manage.windowsazure.com/), sélectionnez **sites Web** et cliquez sur l’application web que vous avez créé dans l’exercice 2.
2. Accédez à la **mise à l’échelle** page. Sous le **capacité** section, sélectionnez **processeur** pour le **mise à l’échelle par métrique** configuration.

    > [!NOTE]
    > Lors de la mise à l’échelle par UC, Azure ajuste dynamiquement le nombre d’instances que l’application utilise si l’utilisation du processeur change.

    ![En sélectionnant à l’échelle par processeur](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "en sélectionnant la métrique du processeur pour la mise à l’échelle automatique")

    *En sélectionnant à l’échelle par processeur*
3. Modifier le **unité centrale cible** configuration **20**-**40** pour cent.

    > [!NOTE]
    > Cette plage représente l’utilisation moyenne du processeur de votre application web. Azure ajoutera ou retirera des instances pour maintenir votre application web dans cette plage. Le nombre minimal et maximal d’instances utilisées pour la mise à l’échelle est spécifié dans le **nombre d’instances** configuration. Azure n’ira jamais au-dessus ou au-delà de cette limite.
    >
    > La valeur par défaut **unité centrale cible** les valeurs sont modifiées uniquement dans le cadre de ce laboratoire. En configurant la plage des UC avec petites valeurs, vous augmentez les chances de déclencheur de mise à l’échelle lorsqu’une charge modérée est placé sur l’application.

    ![Modification de la cible du processeur pour être comprise entre 20 et 40 pour cent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "modification de la cible du processeur pour être comprise entre 20 et 40 pour cent")

    *Modification de l’unité centrale cible pour être comprise entre 20 et 40 pour cent*
4. Cliquez sur **enregistrer** dans la barre de commandes pour enregistrer les modifications.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Tâche 2 : test de charge avec Visual Studio

Maintenant que **mise à l’échelle** a été configuré, vous allez créer un **de performances Web et projet de Test de charge** dans Visual Studio pour générer une charge d’UC sur votre application web.

1. Ouvrez **Visual Studio Ultimate 2013** et sélectionnez **fichier | Nouveau | Projet...**  pour démarrer une nouvelle solution.

    ![Création d’un projet](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "création d’un projet")

    *Création d’un projet*
2. Dans le **nouveau projet** boîte de dialogue, sélectionnez **projet de Test de charge et de performances de site Web** sous le **Visual C# | Test** onglet. Assurez-vous que **.NET Framework 4.5** est sélectionnée, nommez le projet *WebAndLoadTestProject*, choisissez un **emplacement** et cliquez sur **OK**.

    ![Création d’un projet Web et de Test de charge](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "création d’un projet Web et de Test de charge")

    *Création d’un projet Web et de Test de charge*
3. Dans le **WebTest1.webtest** avec le bouton droit le **WebTest1** nœud et cliquez sur **ajouter une requête de**.

    ![Ajout d’une demande à WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Ajout d’une demande à WebTest1")

    *Ajout d’une demande à WebTest1*
4. Dans le **propriétés** fenêtre du nouveau nœud de la demande, mettre à jour le **Url** propriété pour pointer vers l’URL de votre application web (par exemple, *[ http://geek-quiz.azurewebsites.net/ ](http://geek-quiz.azurewebsites.net/)*).

    ![Modification de la propriété Url](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "modification de la propriété Url")

    *Modification de la propriété Url*
5. Dans le **WebTest1.webtest** fenêtre, avec le bouton droit **WebTest1** et cliquez sur **ajouter une boucle...** .

    ![Ajout d’une boucle à WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Ajout d’une boucle à WebTest1")

    *Ajout d’une boucle à WebTest1*
6. Dans le **ajouter une règle conditionnelle et des éléments à la boucle** boîte de dialogue, sélectionnez le **de boucles for** règle et modifier les propriétés suivantes.

   1. **Valeur de fin :** 1000
   2. **Nom du paramètre de contexte :** Itérateur
   3. **Valeur d’incrément :** 1

      ![En sélectionnant la règle de boucles for et la mise à jour les propriétés](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "en sélectionnant la règle de boucles for et la mise à jour les propriétés")

      *En sélectionnant la règle de boucles for et la mise à jour les propriétés*
7. Sous le **éléments de la boucle** , sélectionnez la requête que vous avez créé précédemment pour être le premier et dernier éléments de la boucle. Cliquez sur **OK** pour continuer.

    ![En sélectionnant les premier et dernier éléments de la boucle](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "en sélectionnant les premier et dernier éléments de la boucle")

    *En sélectionnant les premier et dernier éléments de la boucle*
8. Dans **l’Explorateur de solutions**, avec le bouton droit le **WebAndLoadTestProject** de projet, développez le **ajouter** menu et sélectionnez **Test de charge...** .

    ![Ajout d’un Test de charge au projet WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Ajout d’un Test de charge au projet WebAndLoadTestProject")

    *Ajout d’un Test de charge au projet WebAndLoadTestProject*
9. Dans le **Assistant Nouveau Test de charge** boîte de dialogue, cliquez sur **suivant**.

    ![Assistant Nouveau Test de charge](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Assistant Nouveau Test de charge")

    *Assistant Nouveau Test de charge*
10. Dans le **scénario** page, sélectionnez **n’utilisez pas le temps de réflexion** et cliquez sur **suivant**.

    ![Sélection de l’option ne doit ne pas utiliser des temps de réflexion](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "sélection ne doit ne pas utiliser des temps de réflexion")

    *Choisissant de ne pas pour utiliser des temps de réflexion*
11. Dans le **modèle de charge** page, assurez-vous que le **charge constante** option est sélectionnée. Modifier le **nombre d’utilisateurs** à **250** utilisateurs et cliquez sur **suivant**.

    ![Modifier le nombre d’utilisateurs à 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "modifiant le nombre d’utilisateurs à 250")

    *Modifier le nombre d’utilisateurs à 250*
12. Dans le **le modèle de combinaison de tests** page, sélectionnez **basé sur l’ordre séquentiel des tests** et cliquez sur **suivant**.

    ![En sélectionnant le modèle de combinaison de tests](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "en sélectionnant le modèle de combinaison de tests")

    *En sélectionnant le modèle de combinaison de tests*
13. Dans le **modèle de combinaison de tests** , cliquez sur **ajouter...**  pour ajouter un test à la combinaison.

    ![Ajout d’un test à la combinaison de tests](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Ajout d’un test à la combinaison de tests")

    *Ajout d’un test à la combinaison de tests*
14. Dans le **ajouter des Tests** boîte de dialogue, double-cliquez sur **WebTest1** pour ajouter le test à la **tests sélectionnés** liste. Cliquez sur **OK** pour continuer.

    ![Ajout du test WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Ajout du test WebTest1")

    *Ajout du test WebTest1*
15. Dans le **combinaison de tests** , cliquez sur **suivant**.

    ![Fin de la page de combinaison de tests](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "la fin de la page de combinaison de tests")

    *Fin de la page de combinaison de tests*
16. Dans le **la combinaison de réseaux** , cliquez sur **suivant**.

    ![Lorsque vous cliquez sur suivant dans la page de la combinaison de réseaux](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "lorsque vous cliquez sur suivant dans la page de la combinaison de réseaux")

    *En cliquant sur suivant dans la page de la combinaison de réseaux*
17. Dans le **la combinaison de navigateurs** page, sélectionnez **Internet Explorer 10.0** comme type de navigateur, puis cliquez sur **suivant**.

    ![Sélection du type de navigateur](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "en sélectionnant le type de navigateur")

    *Sélection du type de navigateur*
18. Dans le **ensembles de compteurs** , cliquez sur **suivant**.

    ![Cliquer sur suivant dans la page ensembles de compteurs](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "en cliquant sur suivant dans la page ensembles de compteurs")

    *Cliquer sur suivant dans la page ensembles de compteurs*
19. Dans le **paramètres d’exécution** , définissez le **durée du test de charge** à **5 minutes** et cliquez sur **Terminer**.

    ![Définition de la durée de test de charge à 5 minutes](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "définition de la durée de test de charge à 5 minutes")

    *Définition de la durée de test de charge à 5 minutes*
20. Dans **l’Explorateur de solutions**, double-cliquez sur le **Local.settings** fichier pour Explorer les paramètres de test. Par défaut, Visual Studio utilise votre ordinateur local pour exécuter les tests.

    > [!NOTE]
    > Vous pouvez également configurer votre projet de test pour exécuter les tests de charge dans le cloud à l’aide **Plans de Test Azure**. Plans de Test Azure fournit une charge de cloud service qui simule une charge plus réaliste de test, en évitant les limites de l’environnement local, comme la capacité de l’UC, la mémoire disponible et la bande passante réseau. Pour plus d’informations sur l’utilisation de Plans de Test Azure pour exécuter des tests de charge, consultez [les scénarios de tests de charge](/azure/devops/test/load-test/overview?view=vsts).

    ![Paramètres de test](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Tâche 3 : vérification de la mise à l’échelle

Maintenant vous exécutez le test de charge que vous avez créé dans la tâche précédente et voir comment votre application web se comporte sous charge.

1. Dans **l’Explorateur de solutions**, double-cliquez sur **LoadTest1.loadtest** pour ouvrir le test de charge.

    ![Ouverture LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "ouverture LoadTest1.loadtest")

    *Ouverture LoadTest1.loadtest*
2. Dans le **LoadTest1.loadtest** fenêtre, cliquez sur le premier bouton dans la boîte à outils pour exécuter le test de charge.

    ![Le test de charge en cours d’exécution](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "en cours d’exécution du test de charge")

    *Le test de charge en cours d’exécution*
3. Attendez que le test de charge se termine.

    > [!NOTE]
    > Le test de charge simule plusieurs utilisateurs qui envoient simultanément des requêtes à l’application web. Lors de l’exécution du test, vous pouvez surveiller les compteurs disponibles pour détecter des erreurs, avertissements ou autres informations relatives à votre série de tests de charge.

    ![En cours d’exécution de test de charge](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "en attente jusqu'à ce que le test de charge terminé")

    *Test de charge en cours d’exécution*
4. Une fois le test terminé, revenez au portail de gestion et accédez à la **mise à l’échelle** page de votre application web. Sous le **capacité** section, vous devriez voir dans le graphique qu’une nouvelle instance a été automatiquement déployée.

    ![Nouvelle instance automatiquement déployé](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Nouvelle instance automatiquement déployé*

    > [!NOTE]
    > Il peut prendre plusieurs minutes pour que les modifications apparaissent dans le graphique (appuyez sur **CTRL + F5** régulièrement pour actualiser la page). Si vous ne voyez pas toutes les modifications, vous pouvez essayer ce qui suit :
    >
    > - Augmentez la durée du test de charge (par exemple, pour **10 minutes**)
    > - Réduire les valeurs minimale et maximales de la **unité centrale cible** plage dans la configuration de mise à l’échelle de votre application web
    > - Exécuter le test de charge dans le cloud avec **Plans de Test Azure**. Plus d’informations [ici](/azure/devops/test/load-test/index?view=vsts)

---

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

Dans cet atelier, vous avez appris à configurer et déployer votre application pour les applications web de production dans Azure. Vous avez commencé en détectant et vos bases de données à l’aide de la mise à jour **Migrations Entity Framework Code First**, poursuivies en déployant de nouvelles versions de votre site en utilisant **Git** et effectuer des restaurations à la dernière version stable de votre site. En outre, vous avez appris à l’échelle votre application à l’aide de stockage pour déplacer votre contenu statique vers un conteneur d’objets Blob.
