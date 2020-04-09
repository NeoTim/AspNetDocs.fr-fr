---
uid: whitepapers/aspnet-data-access-content-map
title: ASP.NET accès aux données - Ressources recommandées Microsoft Docs
author: rick-anderson
description: Ce sujet fournit des liens vers des ressources de documentation sur la façon d’accéder aux données dans ASP.NET applications Web, principalement en utilisant le Cadre d’entité et SQL Se...
ms.author: riande
ms.date: 07/25/2013
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 357851f195bf233c7c34a32bd156e4408d3e1b24
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676166"
---
# <a name="aspnet-data-access---recommended-resources"></a>Accès aux données ASP.NET - Ressources recommandées

> Ce sujet fournit des liens vers des ressources de documentation sur la façon d’accéder aux données dans ASP.NET applications Web, principalement en utilisant le cadre d’entité et le serveur SQL.
> 
> Si vous connaissez un grand blog, [fil stackoverflow,](http://stackoverflow.com) ou tout autre lien qui serait utile, [envoyez-nous un e-mail](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) avec le lien.
> 
> Dernière mise à jour 3/04/2014

La rubrique contient les sections suivantes :

- [Démarrer avec l’accès aux données en ASP.NET](#gettingstarted)
- [Utilisation du cadre de l’entité](#ef)

    - [Utilisation du code-cadre de l’entité d’abord](#cf)
    - [Utilisation du Code-cadre de l’entité Premières migrations](#efcfmigrations)
    - [Utilisation de la base de données cadre d’entité d’abord ou du modèle d’abord (le concepteur EF)](#efdbf)
    - [Chargement des données connexes dans le cadre de l’entité (chargement paresseux, chargement impatie et chargement explicite)](#efrelateddata)
    - [Optimiser la performance du cadre de l’entité](#optimizingef)
    - [Gestion de la concordance dans une demande-cadre d’entité](#efconcurrency)
    - [Livres sur le cadre de l’entité](#efbooks)
    - [Ressources du Cadre d’entités supplémentaires](#otherefresources)
- [Liaison de données dans ASP.NET applications de formulaires Web](#wfdatabinding)

    - [Utilisation de la liaison du modèle web](#wfmodelbinding)
    - [Utilisation des formulaires Web Contrôle des sources de données](#wfdsc)
    - [Utilisation de formulaires Web Des contrôles liés aux données et des expressions liant des données](#wfdbc)
- [Travailler avec SQL Server Databases](#sqlserver)

    - [Travailler avec SQL Server Express LocalDB Databases](#sslocaldb)
    - [Travailler avec SQL Server Express Databases](#sse)
    - [Travailler avec Windows Azure SQL Database](#ssdb)
    - [Choisir entre SQL Server et Windows Azure SQL Database](#ssdbchoosing)
- [Travailler avec NoSQL Database Management Systems](#nosql)
- [Utilisation de requêtes LINQ dans ASP.NET applications](#linq)
- [Utilisation d’échafaudages de données dynamiques](#dd)
- [Sécurisation de l’accès aux données](#securing)
- [Optimiser les performances d’accès aux données](#optimizingdataaccess)
- [Déploiement d’une base de données](#deploying)
- [Accès aux données par l’intermédiaire d’un service Web](#webservice)
- [Ressources supplémentaires](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Démarrer avec l’accès aux données en ASP.NET

- [Options de stockage de données (Construire des applications Cloud Real-World avec Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Chapitre d’un e-book sur le développement pour le nuage. Présente les bases de données NoSQL comme une alternative que de nombreux développeurs familiers avec les bases de données relationnelles ont tendance à négliger. Présente des lignes directrices sur ce qu’il faut penser lors du choix relationnel ou NoSQL, ou le choix d’une plate-forme particulière.
- [ASP.NET options d’accès aux données](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Introduction aux options d’accès aux données pour les bases de données relationnelles pour ASP.NET et des conseils sur la façon de choisir les plates-formes et les méthodes d’accès qui conviennent à votre scénario.
- [Base de données relationnelle](http://en.wikipedia.org/wiki/Relational_database). Wikipédia). Si vous n’avez pas travaillé avec des bases de données relationnelles, consultez cette page pour une introduction à la terminologie et aux concepts de base de données relationnelles. Pour une introduction à SQL Server en particulier voir [Travailler avec les bases de données SQL Server](#sqlserver) plus tard dans ce sujet.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Utilisation du cadre de l’entité

- [Approches de développement du cadre de l’entité](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Conseils sur la façon de choisir une approche de développement du cadre d’entité Database First, Model First, ou Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Utilisation du code-cadre de l’entité d’abord

Les tutoriels suivants offrent des exemples téléchargeables :

- [Démarrer avec EF 6 en utilisant MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Couvre un large éventail de scénarios de code-cadre d’entités d’abord, y compris les migrations et les fonctionnalités EF 6 telles que la résilience de connexion, l’interception de commande, et async. Il s’agit d’une version mise à jour de la [série EF 5 / MVC 4](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). La série précédente comprend un tutoriel sur le référentiel et les modèles de l’unité de travail qui n’est pas inclus dans la nouvelle série.
- [Introduction à ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Couvre une gamme plus étroite de scénarios de code-cadre d’entités d’abord, mais fait un travail plus complet d’introduction des fonctionnalités MVC.
- [Modèle de liaison et formulaires Web](https://go.microsoft.com/fwlink/?LinkId=286117). Utilise Code First dans une application Web Forms.
- [Démarrer avec ASP.NET 4.5 formulaires Web](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Une introduction aux formulaires Web avec une certaine couverture du Code d’abord. Utilise la liaison de modèle.
- [Magasin de musique MVC](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Utilise Code First dans une application E-commerce MVC 3 qui met également en œuvre l’adhésion et l’autorisation. La version MVC et le système d’adhésion ASP.NET (authentification et autorisation) utilisés ici sont dépassés; pour plus d’informations à jour sur ASP.NET [https://asp.net/identity](https://asp.net/identity)membres, voir .

Autres ressources :

- [Cadre d’entité - Code d’abord à une base de données existante](https://msdn.microsoft.com/data/jj200620). Msdn. Vidéo et procédure pas à pas qui montre comment utiliser Code First avec une base de données existante.
- [Data Developer Center - Entity Framework](https://msdn.microsoft.com/data/ef). Msdn. Pour un guide de la documentation du Cadre d’entité qui a été créé et entretenu par l’équipe du Cadre d’entité, consultez le lien [Get Started.](https://msdn.microsoft.com/data/ee712907)

Voir aussi [des livres sur le cadre d’entité](#efbooks) et les [ressources-cadres supplémentaires de l’entité](#otherefresources) plus tard dans ce sujet.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Utilisation du Code-cadre de l’entité Premières migrations

La plupart des tutoriels Code First énumérés ci-dessus couvrent les migrations. Voir aussi les ressources suivantes.

- [Déploiement web ASP.NET en utilisant Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Série de tutoriels en 2 parties qui montre comment utiliser Code First Migrations pour déployer une base de données.
- [Déployer une application Secure ASP.NET MVC 5 avec Adhésion, OAuth et SQL Database sur un site Web windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Comment utiliser les migrations pour déployer des données d’adhésion et d’application à Azure.
- [Aperçu du déploiement Web pour Visual Studio et ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Consultez la section **Configureing Database Deployment in Visual Studio** pour expliquer comment Code First Migrations est intégré dans les fonctionnalités de déploiement Web de Visual Studio.
- [Data Developer Center - Code First Migrations](https://msdn.microsoft.com/data/jj591621) (MSDN). Documentation migrations de l’équipe du Cadre de l’entité.
- [Migrations Série Screencast](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). BLOG EF). Trois vidéos sur des sujets avancés dans Code First Migrations.
- [Code Premières migrations avec ASP.NET sites De pages Web](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Blog Mikesdotnetting). Montre comment utiliser code d’abord les migrations avec un site ASP.NET Pages Web en mettant le contexte des données dans un projet de bibliothèque de classe Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Utilisation de la base de données cadre d’entité d’abord ou du modèle d’abord (le concepteur EF)

- [Démarrage avec entity Framework 6 Base de données d’abord en utilisant MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Exécutez un script dans Server Explorer pour créer une base de données, puis utilisez le concepteur du cadre d’entité pour créer le modèle de données. Montre comment créer des pages Web CRUD simples, et pour d’autres fonctions de traitement des données, vous pouvez suivre l’un des tutoriels Code First puisque tous les flux de travail EF utilisent la même API DbContext.

Les ressources suivantes sont plus anciennes. Ils sont utiles si vous souhaitez utiliser la version 4.0 du Cadre d’entité, et que vous souhaitez utiliser un contrôle de source de données pour la liaison de données dans une application Web Forms.

- [Démarrer avec le cadre de l’entité 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Montre comment utiliser le contrôle **EntityDataSource.**
- [Continuer avec le cadre de l’entité](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(Montre comment utiliser le contrôle **ObjectDataSource.** Comprend un tutoriel sur la manipulation de la concurrence, un tutoriel sur les performances EF, et un tutoriel sur ce qui est nouveau dans EF 4.0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Manipulation des données connexes dans le cadre de l’entité (chargement paresseux, chargement impatie et chargement explicite)

- [Lire les données connexes avec le cadre de l’entité dans une application ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code Tout d’abord, l’exemple d’application MVC. Les méthodes montrées s’appliquent également à la liaison de modèle de formulaires Web et au flux de travail de base de données d’abord.
- [Data Developer Center - Charges entités connexes](https://msdn.microsoft.com/data/jj574232) (MSDN). Documentation de l’équipe du Cadre de l’entité sur le chargement des données connexes.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Optimiser les performances du Cadre d’entité

- [Scénarios-cadres d’entités avancées pour une application ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Montre comment exécuter vos propres instructions SQL ou appeler vos propres procédures stockées, comment désactiver la détection de changement, et comment désactiver la validation lors de l’enregistrement des modifications.
- [Considérations de rendement pour le cadre d’entité 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Considérations de rendement (Cadre d’entité)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Maximiser les performances avec le cadre d’entité dans une application Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). S’applique au cadre de l’entité 4.0.
- Voir aussi [Optimiser ASP.NET l’accès aux données](#optimizingdataaccess) plus tard dans ce sujet.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Gestion de la concordance dans une demande-cadre d’entité

- [Gestion de la concordance avec le Cadre d’entité dans une application ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code d’abord, API DbContext, à l’aide d’une application d’échantillon MVC.
- [Data Developer Center - Optimistic Concurrency Patterns](https://msdn.microsoft.com/data/jj592904) (MSDN). Documentation de l’équipe du Cadre de l’Entité.
- [Gestion de la concordance avec le cadre de l’entité dans une application Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). S’applique au cadre de l’entité 4.0. Base de données d’abord, ObjectContext API, à l’aide d’une application d’échantillon Web Forms.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Livres sur le cadre de l’entité

- [Cadre d’entité de programmation : DbContext](http://shop.oreilly.com/product/0636920022237.do) de Julie Lerman et Rowan Miller.
- [Cadre de l’entité de programmation : Code First](http://shop.oreilly.com/product/0636920022220.do) de Julie Lerman et Rowan Miller.

Ces deux livres sont à jour avec les techniques recommandées à jour. Ils offrent une introduction plus complète mais facile à suivre au Cadre d’entité que tout ce qui est disponible sur Internet. Un autre livre, [Programming Entity Framework](http://shop.oreilly.com/product/9780596807252.do) de Julie Lerman, est plus vaste et plus complet, mais il est plus ancien et bon nombre des techniques qu’il couvre ne sont plus la façon recommandée d’utiliser le Cadre d’entités. Voir aussi la liste des livres recommandés par l’équipe du Cadre d’entité au [Data Developer Center - Livres](https://msdn.microsoft.com/data/aa937716) sur le site MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Autres ressources-cadres de l’entité

- [Entity Framework (ADO.NET) blog de l’équipe](https://blogs.msdn.com/b/adonet/). L’une des meilleures ressources pour l’information la plus actuelle et les annonces de nouvelles améliorations. Pour d’autres blogs liés à l’EF, voir le Blogroll à [Get Started with Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [MsDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). Consultez la colonne **Points de données,** qui est fréquemment sur les sujets liés au Cadre d’entité.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Liaison de données dans ASP.NET applications de formulaires Web

- [ASP.NET Web forme des options d’accès](https://msdn.microsoft.com/library/jj822927.aspx) <a id="wfmodelbinding"></a>aux données (MSDN).

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Utilisation de la liaison du modèle web

- [Modèle de liaison et formulaires Web](https://go.microsoft.com/fwlink/?LinkId=286117). Série de tutoriels utilisant EF Code First.
- [Web Forms Model Binding Part 1: Selecting Data](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (Scott Guthrie’s blog). Dans ces anciens billets de blog, la propriété qui est actuellement nommé ItemType a été nommé ModelType, mais sinon les informations qu’ils contiennent sont valides.
- [Web Forms Model Binding Part 2: Filtering Data](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (Scott Guthrie’s blog).
- [Web Forms Model Binding Part 3: Mise à jour et validation](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (blog de Scott Guthrie).
- [ASP.NET 4.5 Web Forms Model Binding](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (vidéo).
- [Modèle Liaison Partie 1 - Sélection des données](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (vidéo).
- [Modèle Liaison Partie 2 - Filtrage](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (vidéo).
- [Démarrer avec ASP.NET 4.5 formulaires Web - Afficher les éléments et les détails des données](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Utilisation des formulaires Web Contrôle des sources de données

- [Contrôles des serveurs Web de source de données](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Annonce de la sortie du fournisseur de données dynamiques et du contrôle d’EntityDataSource pour Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (blog Microsoft Web Development).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Utilisation de formulaires Web Des contrôles liés aux données et des expressions liant des données

- [Modèle de liaison et formulaires Web](https://go.microsoft.com/fwlink/?LinkId=286117). Série de tutoriels qui utilise EF Code First.
- [Démarrer avec ASP.NET 4.5 formulaires Web - Afficher les éléments et les détails des données](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Contrôles de données fortement typés](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (blog de Scott Guthrie).
- [Contrôles de données fortement typés](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vidéo).
- [ASP.NET 4.5 Web Forme forts contrôles de données de type](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vidéo).
- [Contrôles des serveurs Web liés aux données](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Aperçu des expressions liant des données](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Cette page ne couvre que **Eval** et **Bind**; il n’a pas été mis à jour pour inclure **Item** et **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Travailler avec SQL Server Databases

- [Caractéristiques de base de données de serveurs SQL](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Pour une introduction générale à une grande variété de sujets SQL Server, voir les entrées sous celui-ci dans le TOC.
- [SQL Server Editions](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Un résumé des éditions SQL Server disponibles, avec des liens vers plus d’informations sur chacun d’eux.)
- [Cordes de connexion de serveur SQL pour ASP.NET applications Web](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Utilisation de SQL Server Compact pour ASP.NET applications Web](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Serveur SQL Microsoft: Échantillons de produits de base de données](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Exemples de bases de données AdventureWorks.
- [Installation de bases de données d’échantillons](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). En plus des méthodes présentées ici, vous pouvez également télécharger l’un des fichiers .mdf échantillon sur le dossier App\_Data d’un projet web, convertir la base de données en LocalDB, et créer une chaîne de connexion LocalDB. Pour plus d’informations sur la façon de le faire, voir [Comment: Mise à niveau vers LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Voir aussi les sections suivantes sur le travail avec SQL Server Express et LocalDB, et le choix entre SQL Server et SQL Database.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Travailler avec SQL Server Express LocalDB Databases

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). L’introduction officielle de MSDN à LocalDB.
- [Cordes de connexion de serveur SQL pour ASP.NET applications Web](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Comment : Mise à niveau vers LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Comment migrer un fichier .mdf d’une version antérieure de SQL Server Express à LocalDB. Vous devez également passer par ce processus si vous téléchargez l’une des bases de [données d’échantillons SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Présentation de LocalDB, un sqL Express amélioré](https://go.microsoft.com/fwlink/?LinkId=234375) (blog SQL Server Express). A plus d’antécédents sur les raisons pour lesquelles LocalDB a été créé que ce qui est inclus dans MSDN.
- [LocalDB: Où est ma base de données?](https://go.microsoft.com/fwlink/?LinkId=234376) (Blog SQL Server Express). Informations sur l’endroit où les fichiers de base de données LocalDB sont créés.
- [Utilisation de LocalDB avec Full IIS, Partie 1: Profil utilisateur](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (blog SQL Server Express). LocalDB n’est pas conçu pour travailler avec l’IIS. Cette série de billets de blog explique les questions et quelques solution de contournement.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Travailler avec SQL Server Express Databases

- [Cordes de connexion de serveur SQL pour ASP.NET applications Web](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Si vous utilisez le paramètre de connexion AttachDBFileName avec SQL Server Express, consultez en particulier la section Instance utilisateur de cette page.
- [Comment s’approprier votre SQL Server Express 2008 local](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (blog SQL Server Express). Un problème courant est de ne pas pouvoir travailler avec les bases de données SQL Server Express parce que vous n’êtes pas administrateur sur l’instance SQL Server Express. Par défaut, seule la personne qui a installé SQL Server Express est un administrateur. Ce blog explique comment vous faire un administrateur SQL Server Express si vous êtes un administrateur sur l’ordinateur.
- [Mon ASP.NET application web peut-elle utiliser une base de données SQL Server Express en production ?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Travailler avec Windows Azure SQL Database

- [Déployer une application MVC secure ASP.NET avec Adhésion, OAuth et SQL Database sur un site Web Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (site Microsoft Azure).
- [Bases de données SQL](https://docs.microsoft.com/azure/sql-database/) (site Microsoft Azure). Démarrage de tutoriels et de guides pratiques.
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). Le nœud de haut niveau de la table de contenu de SQL Database dans MSDN.
- [Windows Azure SQL Database TechNet Wiki Articles Index](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (site Microsoft TechNet).
- [Bloc d’application de traitement des défauts transitoires](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Un cadre qui vous permet de gérer les défauts de réseau transitoires et les erreurs de connexion résultant de la limitation. Disponible dans un forfait NuGet : [Bibliothèque d’entreprise 5.0 - Bloc d’application de manutention de défauts transitoires](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Démarrer avec SQL Database and Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Windows Azure Training Kit](https://www.microsoft.com/download/details.aspx?id=8396) (Microsoft Download Center). Comprend des laboratoires pratiques pour SQL Database.
- [Windows Azure SQL Database Community Forum](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Déménagement vers Windows Azure SQL Database](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Un chapitre d’un scénario complet de bout en bout par l’équipe de Microsoft Patterns and Practices. Couvre pourquoi vous pourriez vouloir migrer et comment migrer de SQL Server à SQL Database.
- [Migrer les bases de données SQL Server vers Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [SQL Base de données Migration Wizard](http://sqlazuremw.codeplex.com/). Un outil open source pour les bases de données de migration à et en provenance de SQL Database.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Choisir entre SQL Server et Windows Azure SQL Database

- [Comparez SQL Server avec Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (site Microsoft TechNet).
- [Migration de données vers Windows Azure SQL Database: Tools and Techniques](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Comprend les sections qui comparent SQL Server à SQL Database et fournissent des conseils sur le moment de migrer de SQL Server à SQL Database.
- [Windows Azure SQL Database Delivery Guide](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (site Microsoft TechNet).
- [Limitations de fonctionnalités de serveur SQL (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Windows Azure Table Storage et Windows Azure SQL Database - Compared and Contrasted](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Pour une application que vous déployez sur Windows Azure, le stockage Windows Azure Table peut être une alternative à la base de données SqL de Windows Azure. Ce sujet vous aide à choisir entre ces alternatives.
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Instructions et limitations (Microsoft Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Travailler avec NoSQL Database Management Systems

- [Windows Azure Data Services](https://www.windowsazure.com/develop/net/data/) (site Microsoft Azure). Consultez le [guide de fonctionnalités du service de table](https://docs.microsoft.com/azure/) et la section Big **Data** de la page.
- [ASP.NET application multi-tier à l’aide de tables de stockage, de files d’attente et de blobs](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (site Microsoft Azure). Tutoriel de bout en bout avec application d’échantillons téléchargeables qui utilise windows Azure stockage NoSQL tables.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Utilisation des requêtes LINQ dans ASP.NET applications

- [ASP.NET options d’accès aux données](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Inclut une introduction à LINQ.
- [LINQ Training Videos](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (blog de Joe Stagner).
- [ASP.NET fil forum avec des liens vers des ressources dynamiques LINQ](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Utilisation d’échafaudages de données dynamiques

- [Modèles dynamiques de projet de données](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Conseils sur le moment d’utiliser les projets de données dynamiques.
- [ASP.NET Données dynamiques](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Sécurisation de l’accès aux données

- [Sécurisation de l’accès aux données dans ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Considérations de sécurité (Cadre de l’entité)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Comment : Sécuriser les chaînes de connexion lors de l’utilisation des contrôles de source de données](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Optimiser les performances d’accès aux données

- [ASP.NET Aperçu du rendement](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [ASP.NET Caching](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Améliorer la performance ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). Il y a un avertissement de « contenu à la retraite » en haut de cette page, mais la plupart des informations sont toujours pertinentes et il n’existe pas de ressource mise à jour comparable.
- [Améliorer les performances du serveur SQL](https://msdn.microsoft.com/library/ff647793) (MSDN). Même commentaire que le lien précédent.

Voir aussi [optimiser les performances du Cadre d’entités](#optimizingef) plus tôt dans ce sujet.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Déploiement d’une base de données

- [ASP.NET déploiement Web - Ressources recommandées](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Accès aux données par l’intermédiaire d’un service Web

- [Accès aux données par l’intermédiaire d’un service Web](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Conseils sur le moment d’utiliser l’API Web par rapport à WCF.
- [Démarrer avec ASP.NET’API Web](../web-api/index.md).
- [WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Ressources supplémentaires

- [ASP.NET’accès aux données FAQ](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [ASP.NET Web Forms Tutorials - Données](../web-forms/overview/data-access/index.md). La plupart de ces tutoriels sont relativement vieux; assurez-vous de lire [ASP.NET options d’accès aux données](https://msdn.microsoft.com/library/ms178359.aspx) et d’options de stockage de données [(Construire des applications Cloud Real-World avec Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) d’abord afin que vous n’obtenez pas trop loin dans une méthode d’accès aux données qui n’est pas bon pour votre scénario.
- [ASP.NET carte de contenu MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [ASP.NET Web Pages Tutorials - Données](../web-pages/overview/data/index.md).
- [Accès aux données en studio visuel](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Fournit une liste de liens similaires à cette carte de contenu, mais avec un accent sur Visual Studio plutôt que ASP.NET.
