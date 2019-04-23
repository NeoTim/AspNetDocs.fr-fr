---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 'Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : Déploiement de SQL Server Compact bases de données - 2 de 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: cc8568847e050e868a3e7563b5fc1fc6fbf25d86
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59405480"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : Déploiement de SQL Server Compact bases de données - 2 de 12

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour une introduction à la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui montre les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions de SQL Server autre que SQL Server Compact et montre comment déployer dans Azure App Service Web Apps, consultez [déploiement Web ASP.NET à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Vue d'ensemble

Ce didacticiel montre comment définir les deux bases de données SQL Server Compact et le moteur de base de données pour le déploiement.

Pour un accès de base de données, l’application Contoso University nécessite les logiciels suivants doivent être déployés avec l’application, car il n’est pas inclus dans le .NET Framework :

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (moteur de base de données).
- [Fournisseurs universels ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (lequel activer le système d’appartenance ASP.NET utiliser SQL Server Compact)
- [Entity Framework 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First avec des Migrations).

La structure de base de données et certains (pas tous) des données dans les deux de l’application bases de données doivent également être déployés. En règle générale, lorsque vous développez une application, vous devez entrer les données de test dans une base de données que vous ne souhaitez pas déployer sur un site de production. Toutefois, vous pouvez également entrer des données de production que vous ne souhaitez pas déployer. Dans ce didacticiel, vous allez configurer le projet de Contoso University afin que les logiciels requis et les données correctes sont incluses lorsque vous déployez.

Rappel : Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez le didacticiel, veillez à consulter le [page Résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact et SQL Server Express

L’exemple d’application utilise SQL Server Compact 4.0. Ce moteur de base de données est une option relativement nouveau pour les sites Web ; les versions antérieures de SQL Server Compact ne fonctionnent pas dans un environnement d’hébergement web. SQL Server Compact offre plusieurs avantages par rapport au scénario plus courant de développement avec SQL Server Express et le déploiement vers SQL Server. Selon le fournisseur d’hébergement que vous choisissez, SQL Server Compact peut être plus économique à déployer, étant donné que certains fournisseurs facturent supplémentaire pour prendre en charge une base de données SQL Server complète. Il est sans frais supplémentaires pour SQL Server Compact, car vous pouvez déployer le moteur de base de données lui-même en tant que partie de votre application web.

Toutefois, vous devez également être conscient de ses limitations. SQL Server Compact ne prend pas en charge les procédures stockées, des déclencheurs, des vues ou réplication. (Pour obtenir une liste complète des fonctionnalités de SQL Server qui ne sont pas pris en charge par SQL Server Compact, consultez [différences entre SQL Server Compact et SQL Server](https://msdn.microsoft.com/library/bb896140.aspx).) En outre, certains des outils que vous pouvez utiliser pour manipuler des schémas et données dans SQL Server Express et les bases de données SQL Server ne fonctionnent pas avec SQL Server Compact. Par exemple, vous ne pouvez pas utiliser SQL Server Management Studio ou SQL Server Data Tools dans Visual Studio avec des bases de données SQL Server Compact. Vous disposez d’autres options pour travailler avec des bases de données SQL Server Compact :

- Vous pouvez utiliser l’Explorateur de serveurs dans Visual Studio, qui offre des fonctionnalités de manipulation de base de données limité pour SQL Server Compact.
- Vous pouvez utiliser la fonctionnalité de manipulation de base de données de [WebMatrix](https://www.microsoft.com/web/webmatrix/), ce qui a plus de fonctionnalités que l’Explorateur de serveurs.
- Vous pouvez utiliser relativement complète par des tiers ou outils open source, tel que le [boîte à outils de SQL Server Compact](https://github.com/ErikEJ/SqlCeToolbox) et [utilitaire de script de données SQL Compact et schéma](https://github.com/ErikEJ/SqlCeToolbox).
- Vous pouvez écrire et exécuter votre propre des scripts DDL (langage de définition de données) pour manipuler le schéma de base de données.

Vous pouvez commencer avec SQL Server Compact et mettre à niveau ultérieurement à mesure que vos besoins évoluent. Autres didacticiels de cette série vous montrent comment migrer à partir de SQL Server Compact pour SQL Server Express et SQL Server. Toutefois, si vous créez une application et que vous pensez pas avoir besoin de SQL Server dans un avenir proche, il est probablement préférable de commencer avec SQL Server ou SQL Server Express.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Configuration du moteur de base de données Compact de SQL Server pour le déploiement

Les logiciels requis pour accéder aux données dans l’application Contoso University a été ajouté en installant les packages NuGet suivants :

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (fournisseurs universels ASP.NET)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Les liens pointent vers les versions actuelles de ces packages, ce qui peuvent être plus récents que ce qui est installé dans le projet de démarrage que vous avez téléchargé pour ce didacticiel. Pour un déploiement vers un fournisseur d’hébergement, assurez-vous que vous utilisez Entity Framework 5.0 ou version ultérieure. Les versions antérieures de Migrations Code First nécessitent la confiance totale, et à nombreux fournisseurs d’hébergement votre application s’exécute en confiance moyenne. Pour plus d’informations sur Medium Trust, consultez le [déploiement vers IIS comme environnement de Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) didacticiel.

Installation du package NuGet généralement s’occupe de tout ce que vous avez besoin pour déployer ce logiciel avec l’application. Dans certains cas, cela implique des tâches telles que la modification du fichier Web.config et l’ajout de scripts PowerShell qui s’exécutent chaque fois que vous générez la solution. **Si vous souhaitez ajouter la prise en charge pour une de ces fonctionnalités (telles que SQL Server Compact et Entity Framework) sans l’aide de NuGet, assurez-vous que vous savez ce que fait installation du package NuGet afin que vous pouvez effectuer le même travail manuellement.**

Il existe une exception où NuGet ne prend soin de tout ce que vous avez à faire pour garantir la réussite du déploiement. Le package NuGet de SqlServerCompact ajoute un script de post-build à votre projet qui copie les assemblys natifs à *x86* et *amd64* sous-dossiers sous le projet *bin* dossier, mais le script n’inclut pas ces dossiers dans le projet. Par conséquent, Web Deploy ne copie pas les vers le site web de destination, sauf si vous les incluez manuellement dans le projet. (Ce comportement résultant de la configuration de déploiement par défaut ; vous n’utilisez pas dans ces didacticiels, une autre option consiste à modifier le paramètre qui contrôle ce comportement. Le paramètre que vous pouvez modifier est **uniquement les fichiers nécessaires pour exécuter l’application** sous **éléments à déployer** sur le **Package/Publication Web** onglet de la **projet Propriétés** fenêtre. Modification de ce paramètre n’est généralement pas recommandée, car cela peut entraîner le déploiement de nombreux fichiers plus à l’environnement de production qu’il nécessaires. Pour plus d’informations sur les alternatives, consultez le [configuration des propriétés de projet](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) didacticiel.)

Générez le projet, puis dans **l’Explorateur de solutions** cliquez sur **afficher tous les fichiers** si vous ne le n'avez pas déjà fait. Vous devrez peut-être également cliquez sur **Actualiser**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Développez le **bin** dossier pour voir les **amd64** et **x86** dossiers, puis sélectionnez ces dossiers, avec le bouton droit, puis sélectionnez **inclure dans le projet**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Les icônes de dossier changent et indique que le dossier a été inclus dans le projet.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Configuration des Migrations Code First pour le déploiement de base de données d’Application

Lorsque vous déployez une base de données d’application, en général, vous ne simplement déployer votre base de données de développement avec toutes les données qu’il contient en production, car une grande partie des données qu’il contient existe-t-il probablement uniquement à des fins de test. Par exemple, les noms d’étudiants dans une base de données de test sont fictifs. En revanche, vous souvent impossible de déployer uniquement la structure de base de données sans données qu’il contient tout. Certaines des données dans votre base de données de test peuvent être des données réelles et doivent être il lorsque les utilisateurs commencent à utiliser l’application. Par exemple, votre base de données peut avoir une table qui contient les valeurs de niveau valide ou les noms de service réel.

Pour simuler ce scénario courant, vous allez configurer une méthode de la valeur de départ de Migrations premier Code qui insère dans la base de données uniquement les données que vous souhaitez être il en production. Cette méthode de valeur initiale ne sont pas insérer des données de test, car il s’exécutera en production une fois que le Code First crée la base de données en production.

Dans les versions antérieures de Code First avant la parution de Migrations, il était courant pour les méthodes de valeur initiale insérer des données de test en outre, étant donné qu’à chaque modification de modèle au cours du développement la base de données a dû être complètement supprimé et recréé à partir de zéro. Avec les Migrations Code First, les données de test sont conservées après les modifications de la base de données, y compris les données de test dans la méthode Seed n’est pas nécessaire. Le projet que vous avez téléchargé utilise la méthode de Migrations préalable d’inclusion de toutes les données dans la méthode Seed d’une classe d’initialiseur. Dans ce didacticiel, vous allez désactiver l’initialiseur de classe et permettre des Migrations. Puis vous mettrons à jour la méthode Seed dans la classe de configuration de Migrations afin qu’il insère uniquement les données que vous souhaitez insérer dans la production.

Le diagramme suivant illustre le schéma de la base de données d’application :

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Pour ces didacticiels, vous allez supposer que le `Student` et `Enrollment` tables doivent être vides lorsque le site est tout d’abord déployé. Les autres tables contiennent des données qui doit être préchargée lors de l’application est publiée.

Dans la mesure où vous allez utiliser Migrations Code First, vous n’avez plus à utiliser le **DropCreateDatabaseIfModelChanges** initialiseurs Code First. Le code pour cet initialiseur est dans le fichier SchoolInitializer.cs dans le projet ContosoUniversity.DAL. Un paramètre dans le **appSettings** élément du fichier Web.config entraîne cet initialiseur exécuter chaque fois que l’application tente d’accéder à la base de données pour la première fois :

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Ouvrez le fichier Web.config de l’application et supprimez l’élément qui spécifie la classe d’initialiseur de Code First à partir de l’élément appSettings. L’élément appSettings ressemble maintenant à ceci :

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Une autre façon de spécifier une classe d'initialisation est de le faire en appelant `Database.SetInitializer` dans la méthode `Application_Start` du fichier *Global.asax* Si vous activez les Migrations dans un projet qui utilise cette méthode pour spécifier l’initialiseur, supprimez cette ligne de code.

Ensuite, activez les Migrations Code First.

La première étape consiste à vous assurer que le projet de ContosoUniversity est défini comme projet de démarrage. Dans **l’Explorateur de solutions**, cliquez sur le projet ContosoUniversity et sélectionnez **définir comme projet de démarrage**. Migrations Code First recherchera dans le projet de démarrage pour rechercher la chaîne de connexion de base de données.

À partir de la **outils** menu, cliquez sur **Gestionnaire de Package NuGet** , puis **Console du Gestionnaire de Package**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

En haut de la fenêtre **de la console du gestionnaire** de packages, sélectionnez ContosoUniversity.DAL comme projet par défaut, puis à l'invite `PM>`, entrez "enable-migrations".

![enable-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Cette commande crée un *Configuration.cs* fichier dans un nouveau *Migrations* dossier dans le projet ContosoUniversity.DAL.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Vous avez sélectionné le projet de la couche DAL, car la commande « enable-migrations » doit être exécutée dans le projet qui contient la classe de contexte de Code First. Lorsque cette classe est dans un projet de bibliothèque de classes, les Migrations Code First recherche la chaîne de connexion de base de données dans le projet de démarrage pour la solution. Dans la solution ContosoUniversity, le projet web a été défini comme projet de démarrage. (Si vous ne voulez pas que désigner le projet dont la chaîne de connexion en tant que projet de démarrage dans Visual Studio, vous pouvez spécifier le projet de démarrage dans la commande PowerShell. Pour afficher la syntaxe de commande pour la commande enable-migrations, vous pouvez entrer les commande « get-help enable-migrations ».)

Ouvrez le fichier Configuration.cs et remplacez les commentaires dans le `Seed` méthode avec le code suivant :

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

Les références à `List` ont des lignes ondulées rouges sous les, car vous n’avez pas un `using` instruction encore pour son espace de noms. Cliquez sur un des instances de `List` et cliquez sur **résoudre**, puis cliquez sur **using System.Collections.Generic**.

![Résoudre avec l’instruction using](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Cette sélection de menu ajoute le code suivant aux `using` instructions situées en haut du fil

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Dans SSMS `Seed`Explorateur d’objets, avec le bouton droit bases de données et cliquez sur attacher. Une alternative consiste à ajouter du code aux méthodes `Up` et `Down` de chaque classe de migration. Les méthodes `Up` et `Down` contiennent du code qui implémente les modifications de base de données. Vous en trouverez des exemples dans le didacticiel [Déploiement d'une base de données Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md).
> 
> Vous pouvez également écrire du code qui exécute des instructions SQL à l'aide de la méthode `Sql`. Par exemple, si vous ajoutiez une colonne de Budget pour la table Department et que vous voulez initialiser tous les budgets de département à 1 000,00 $ dans le cadre d’une migration, vous pouvez ajouter la ligne suivante de code pour le `Up` méthode pour que la migration :
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> Cet exemple indiqué pour ce didacticiel utilise le `AddOrUpdate` méthode dans le `Seed` méthode les Migrations Code First `Configuration` classe. Code First Migrations appelle la `Seed` méthode après chaque migration et cette méthode met à jour les lignes qui ont déjà été insérés, ou les insère si elles n’existent pas encore. La méthode `AddOrUpdate` peut ne pas être le meilleur choix pour votre scénario. Pour plus d'informations, voir [ Faites attention avec la méthode AddOrUpdate d'EF 4.3 ](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) sur le blog de Julie Lerman.


Appuyez sur CTRL-MAJ-B pour générer le projet.

L’étape suivante consiste à créer un `DbMigration` classe pour la migration initiale. Vous souhaitez que cette migration pour créer une nouvelle base de données, donc vous devez supprimer la base de données qui existe déjà. Bases de données SQL Server Compact sont contenues dans *.sdf* des fichiers dans le *application\_données* dossier. Dans **l’Explorateur de solutions**, développez *application\_données* dans le projet ContosoUniversity pour voir les deux bases de données SQL Server Compact, qui sont représenté par *.sdf*fichiers.

Avec le bouton droit le *School.sdf* de fichier et cliquez sur **supprimer**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

Dans le **Console du Gestionnaire de Package** fenêtre, entrez la commande « add-migration Initial » pour créer la migration initiale et nommez-le « Initiale ».

![add-migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Migrations Code First crée un autre fichier de classe dans le *Migrations* dossier et cette classe contient le code qui crée le schéma de base de données.

Dans le **Console du Gestionnaire de Package**, entrez la commande « update-base de données » pour créer la base de données et exécuter le **Seed** (méthode).

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Si vous obtenez une erreur qui indique une table existe déjà et ne peut pas être créée, il est probablement parce que vous avez exécuté l’application une fois que vous avez supprimé la base de données et avant l’exécution de `update-database`. Dans ce cas, supprimez le *School.sdf* à nouveau de fichier et recommencez la `update-database` commande.)

Exécutez l'application. Maintenant la page des étudiants est vide, mais la page des formateurs contient des formateurs. C’est ce que vous obtiendrez en production après avoir déployé l’application.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Le projet est maintenant prêt à déployer le *School* base de données.

## <a name="creating-a-membership-database-for-deployment"></a>Création d’une base de données d’appartenance pour le déploiement

L’application Contoso University utilise l’authentification de formulaires et de système de l’appartenance ASP.NET pour authentifier et autoriser les utilisateurs. Un de ses pages est accessible uniquement aux administrateurs. Pour afficher cette page, exécutez l’application et sélectionnez **crédits de la mise à jour** à partir du menu menu volant sous **cours**. L’application affiche le **Log In** page, étant donné que seuls les administrateurs sont autorisés à utiliser le **crédits de la mise à jour** page.

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Connectez-vous en tant que « administrateur » par le mot de passe « Pas$ w0rd » (Notez le nombre de zéro à la place de la lettre « o » dans « w0rd »). Après vous être connecté, le **mise à jour crédits** page s’affiche.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Lorsque vous déployez un site pour la première fois, il est courant d’exclure la plupart ou tous les comptes d’utilisateur que vous créez pour le test. Dans ce cas, vous allez déployer un compte d’administrateur et aucun compte d’utilisateur. Au lieu de supprimer manuellement les comptes de test, vous allez créer une nouvelle base de données d’appartenance qui a uniquement le compte d’utilisateur un administrateur dont vous avez besoin en production.

> [!NOTE]
> La base de données d’appartenances stocke un hachage des mots de passe de compte. Pour déployer des comptes à partir d’un ordinateur à un autre, il se peut que vous devez vous assurer que les routines de hachage ne pas générer de hachages différents sur le serveur de destination qu’ils l’ordinateur source. Ils génèrent les hachages mêmes lorsque vous utilisez les fournisseurs universels ASP.NET, tant que vous ne modifiez pas l’algorithme par défaut. L’algorithme par défaut est HMACSHA256 et qu’il est spécifié dans le **validation** attribut de la **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** élément dans le fichier Web.config.


La base de données d’appartenance n’est pas gérée par les Migrations Code First, et il n’est pas automatique d’initialiseur qui peuple la base de données avec des comptes de test (car il est pour la base de données School). Par conséquent, pour conserver les données de test disponibles vous apporterez une copie de la base de données de test avant de créer un nouveau.

Dans **l’Explorateur de solutions**, renommer le *aspnet.sdf* de fichiers dans le *application\_données* dossier *aspnet-Dev.sdf*. (Ne pas effectuer une copie, simplement la renommer, vous allez créer une nouvelle base de données dans un instant.)

Dans **l’Explorateur de solutions**, assurez-vous que le projet web (ContosoUniversity, pas ContosoUniversity.DAL) est sélectionné. Ensuite, dans le **projet** menu, sélectionnez **Configuration ASP.NET** pour exécuter le **outil d’Administration de Site Web**(WAT).

Sélectionnez le **sécurité** onglet.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Cliquez sur **créer ou gérer des rôles** et ajoutez un **administrateur** rôle.

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Revenez à la **sécurité** , cliquez sur **Create User**, et ajoutez l’utilisateur « admin » en tant qu’administrateur. Avant de cliquer sur le **Create User** bouton sur le **Create User** , assurez-vous que vous sélectionnez le **administrateur** case à cocher. Le mot de passe utilisé dans ce didacticiel est « Pas$ w0rd », et vous pouvez entrer n’importe quelle adresse de messagerie.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Fermez le navigateur. Dans **l’Explorateur de solutions**, cliquez sur le bouton Actualiser pour voir le nouveau *aspnet.sdf* fichier.

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Avec le bouton droit **aspnet.sdf** et sélectionnez **inclure dans le projet**.

## <a name="distinguishing-development-from-production-databases"></a>Distinction de développement de bases de données de Production

Dans cette section, vous allez renommer les bases de données afin que les versions de développement soient Dev.sdf à l’école et aspnet-Dev.sdf et les versions de production sont Prod.sdf à l’école et aspnet-Prod.sdf. Cela n’est pas nécessaire, mais cela par conséquent, vous serez toujours d’obtenir des versions de test et de production des bases de données confondus.

Dans **l’Explorateur de solutions**, cliquez sur **Actualiser** et développez l’application\_dossier de données pour voir la base de données School que vous avez créé précédemment ; faites un clic droit et sélectionnez **inclure dans le projet** .

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Renommer *aspnet.sdf* à *aspnet-Prod.sdf*.

Renommer *School.sdf* à *School-Dev.sdf*.

Lorsque vous exécutez l’application dans Visual Studio, vous ne souhaitez pas utiliser le *-Prod* versions des fichiers de base de données, que vous souhaitez utiliser *- Dev* versions. Par conséquent, vous devez modifier les chaînes de connexion dans le fichier Web.config afin qu’ils pointent vers le *- Dev* versions des bases de données. (Vous n’avez pas créé un fichier Prod.sdf à l’école, mais qui est OK, car le Code First crée cette base de données de production, la première exécution de votre application il).

Ouvrez le fichier Web.config de l’application et recherchez les chaînes de connexion :

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Change "aspnet.sdf" to "aspnet-Dev.sdf", and change "School.sdf" to "School-Dev.sdf":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

Le moteur de base de données SQL Server Compact et les bases de données sont désormais prêts à être déployé. Dans ce didacticiel vous configurez automatique *Web.config* transformations de fichiers pour les paramètres qui doivent être différents dans les environnements de développement, test et production. (Les paramètres qui doivent être modifiés sont les chaînes de connexion, mais vous allez configurer ces modifications ultérieurement lorsque vous créez un profil de publication).

## <a name="more-information"></a>Informations complémentaires

Pour plus d’informations sur NuGet, consultez [gérer les bibliothèques de projets avec NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) et [Documentation de NuGet](http://docs.nuget.org/docs/start-here/overview). Si vous ne souhaitez pas utiliser NuGet, vous devrez apprendre à analyser un package NuGet pour déterminer ce qu’il fait lorsqu’il est installé. (Par exemple, il peut configurer *Web.config* transformations, configurer des scripts PowerShell à exécuter au moment de la génération, etc..) Pour en savoir plus sur le fonctionne de NuGet, consultez surtout [créer et publier un Package](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) et [fichier de Configuration et des Transformations de Code Source](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [Suivant](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
