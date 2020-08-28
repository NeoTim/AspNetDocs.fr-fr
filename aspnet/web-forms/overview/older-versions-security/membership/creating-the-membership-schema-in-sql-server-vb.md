---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
title: Création du schéma d’appartenance dans SQL Server (VB) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel commence par examiner les techniques permettant d’ajouter le schéma nécessaire à la base de données afin d’utiliser le SqlMembershipProvider. Après cela, nous travaillons...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 112a674d-716f-41a6-99b8-4074d65a54c0
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 3596e85f6f1e36e046011212479128229be24201
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89045076"
---
# <a name="creating-the-membership-schema-in-sql-server-vb"></a>Création du schéma d’appartenance dans SQL Server (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_VB.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_vb.pdf)

> Ce didacticiel commence par examiner les techniques permettant d’ajouter le schéma nécessaire à la base de données afin d’utiliser le SqlMembershipProvider. À la suite de cela, nous examinerons les tables clés dans le schéma et nous étudierons leur objectif et leur importance. Ce didacticiel se termine par une présentation de la façon d’indiquer à une application ASP.NET le fournisseur que l’infrastructure d’appartenance doit utiliser.

## <a name="introduction"></a>Introduction

Les deux didacticiels précédents ont examiné l’utilisation de l’authentification par formulaire pour identifier les visiteurs du site Web. L’infrastructure d’authentification par formulaire permet aux développeurs de connecter facilement un utilisateur à un site Web et de le mémoriser sur les visites de page via l’utilisation de tickets d’authentification. La `FormsAuthentication` classe comprend des méthodes permettant de générer le ticket et de l’ajouter aux cookies du visiteur. Le `FormsAuthenticationModule` examine toutes les demandes entrantes et, pour celles avec un ticket d’authentification valide, crée et associe un `GenericPrincipal` et un `FormsIdentity` objet à la requête actuelle. L’authentification par formulaire est simplement un mécanisme d’octroi d’un ticket d’authentification à un visiteur lors de la connexion et, lors des demandes suivantes, de l’analyse de ce ticket pour déterminer l’identité de l’utilisateur. Pour qu’une application Web prenne en charge les comptes d’utilisateur, nous devons toujours implémenter un magasin d’utilisateurs et ajouter des fonctionnalités pour valider les informations d’identification, inscrire de nouveaux utilisateurs et la myriade d’autres tâches liées au compte d’utilisateur.

Avant ASP.NET 2,0, les développeurs étaient sur le crochet pour implémenter toutes ces tâches liées aux comptes d’utilisateur. Heureusement, l’équipe ASP.NET a reconnu cette lacune et a introduit l’infrastructure d’appartenance avec ASP.NET 2,0. L’infrastructure d’appartenance est un ensemble de classes dans le .NET Framework qui fournissent une interface de programmation pour accomplir les tâches principales relatives au compte d’utilisateur. Cette infrastructure est générée au-dessus du [modèle de fournisseur](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), qui permet aux développeurs de brancher une implémentation personnalisée dans une API standardisée.

Comme indiqué dans le didacticiel sur la <a id="Tutorial1"></a> [*sécurité et la prise en charge ASP.net*](../introduction/security-basics-and-asp-net-support-vb.md) , le .NET Framework est fourni avec deux fournisseurs d’appartenances intégrés : [`ActiveDirectoryMembershipProvider`](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) et [`SqlMembershipProvider`](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) . Comme son nom l’indique, le `SqlMembershipProvider` utilise une base de données Microsoft SQL Server comme magasin de l’utilisateur. Pour pouvoir utiliser ce fournisseur dans une application, nous devons indiquer au fournisseur la base de données à utiliser comme magasin. Comme vous pouvez l’imaginer, le `SqlMembershipProvider` attend que la base de données du magasin de l’utilisateur dispose de certaines tables de base de données, vues et procédures stockées. Nous devons ajouter ce schéma attendu à la base de données sélectionnée.

Ce didacticiel commence par examiner les techniques permettant d’ajouter le schéma nécessaire à la base de données afin d’utiliser le `SqlMembershipProvider` . À la suite de cela, nous examinerons les tables clés dans le schéma et nous étudierons leur objectif et leur importance. Ce didacticiel se termine par une présentation de la façon d’indiquer à une application ASP.NET le fournisseur que l’infrastructure d’appartenance doit utiliser.

Commençons.

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Étape 1 : choix de l’emplacement du magasin d’utilisateurs

Les données d’une application ASP.NET sont généralement stockées dans plusieurs tables d’une base de données. Lors de l’implémentation du `SqlMembershipProvider` schéma de base de données, nous devons décider si le schéma d’appartenance doit être placé dans la même base de données que les données d’application ou dans une autre base de données.

Je vous recommande de localiser le schéma d’appartenance dans la même base de données que les données d’application pour les raisons suivantes :

- **Maintenabilité**  une application dont les données sont encapsulées dans une base de données est plus facile à comprendre, à gérer et à déployer qu’une application qui a deux bases de données distinctes.
- **Intégrité relationnelle**  en localisant les tables associées à l’appartenance dans la même base de données que les tables d’application, il est possible d’établir des [contraintes de clé étrangère](http://en.wikipedia.org/wiki/Foreign_key) entre les clés primaires dans les tables liées aux appartenances et les tables d’application associées.

Le découplage du magasin d’utilisateurs et des données d’application dans des bases de données distinctes n’a de sens que si vous avez plusieurs applications qui utilisent chacune des bases de données distinctes, mais qui doivent partager un magasin d’utilisateurs commun.

### <a name="creating-a-database"></a>Création d’une base de données

L’application que nous avons créé depuis le deuxième didacticiel n’a pas encore besoin d’une base de données. Toutefois, nous avons besoin d’une fois pour le magasin de l’utilisateur. Nous allons en créer un, puis y ajouter le schéma requis par le `SqlMembershipProvider` fournisseur (Voir l’étape 2).

> [!NOTE]
> Tout au long de cette série de didacticiels, nous allons utiliser une base de données [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) pour stocker nos tables d’application et le `SqlMembershipProvider` schéma. Cette décision a été prise pour deux raisons : tout d’abord, en raison de son coût-gratuit, l’édition Express est la version la plus accessible en lecture de SQL Server 2005 ; Deuxièmement, SQL Server 2005 Express Edition bases de données peuvent être placées directement dans le dossier de l’application Web `App_Data` , ce qui en fait un ensemble pour créer un package de la base de données et de l’application Web dans un fichier zip et la redéployer sans aucune instruction de configuration ou option de configuration spéciale. Si vous préférez suivre l’utilisation d’une version de non-Express Edition de SQL Server, n’hésitez pas. Les étapes sont virtuellement identiques. Le `SqlMembershipProvider` schéma fonctionne avec n’importe quelle version de Microsoft SQL Server 2000 et versions.

Dans le Explorateur de solutions, cliquez avec le bouton droit sur le `App_Data` dossier et choisissez Ajouter un nouvel élément. (Si vous ne voyez pas `App_Data` de dossier dans votre projet, cliquez avec le bouton droit sur le projet dans Explorateur de solutions, sélectionnez Ajouter un dossier ASP.net, puis choisissez `App_Data` .) Dans la boîte de dialogue Ajouter un nouvel élément, choisissez d’ajouter un nouveau SQL Database nommé `SecurityTutorials.mdf` . Dans ce didacticiel, nous allons ajouter le `SqlMembershipProvider` schéma à cette base de données. dans les didacticiels suivants, nous allons créer des tables supplémentaires pour capturer les données de l’application.

[![Ajoutez un nouveau SQL Database nommé base de données SecurityTutorials. mdf au dossier App_Data](creating-the-membership-schema-in-sql-server-vb/_static/image2.png)](creating-the-membership-schema-in-sql-server-vb/_static/image1.png)

**Figure 1**: ajouter une nouvelle SQL Database nommée `SecurityTutorials.mdf` base de données au `App_Data` dossier ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image3.png))

L’ajout d’une base de données au `App_Data` dossier l’intègre automatiquement dans la vue Explorateur de base de données. (Dans la version non Express Edition de Visual Studio, le explorateur de base de données est appelé le Explorateur de serveurs.) Accédez à la explorateur de base de données et développez la base de données qui vient d’être ajoutée `SecurityTutorials` . Si vous ne voyez pas l’explorateur de base de données à l’écran, accédez au menu Affichage et choisissez explorateur de base de données ou appuyez sur Ctrl + Alt + S. Comme le montre la figure 2, la `SecurityTutorials` base de données est vide, elle ne contient aucune table, aucune vue et aucune procédure stockée.

[![La base de données SecurityTutorials est actuellement vide](creating-the-membership-schema-in-sql-server-vb/_static/image5.png)](creating-the-membership-schema-in-sql-server-vb/_static/image4.png)

**Figure 2**: la `SecurityTutorials` base de données est actuellement vide ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image6.png))

## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Étape 2 : ajout du `SqlMembershipProvider` schéma à la base de données

Le `SqlMembershipProvider` requiert un ensemble particulier de tables, de vues et de procédures stockées à installer dans la base de données du magasin de l’utilisateur. Ces objets de base de données nécessaires peuvent être ajoutés à l’aide de l' [ `aspnet_regsql.exe` outil](https://msdn.microsoft.com/library/ms229862.aspx). Ce fichier se trouve dans le `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\` dossier.

> [!NOTE]
> L' `aspnet_regsql.exe` outil offre à la fois des fonctionnalités de ligne de commande et une interface utilisateur graphique. L’interface graphique est plus conviviale et c’est ce que nous allons examiner dans ce didacticiel. L’interface de ligne de commande est utile lorsque l’ajout du `SqlMembershipProvider` schéma doit être automatisé, par exemple dans les scripts de génération ou les scénarios de test automatisé.

L' `aspnet_regsql.exe` outil est utilisé pour ajouter ou supprimer des *services d’application ASP.net* dans une base de données SQL Server spécifiée. Les services d’application ASP.NET englobent les schémas pour `SqlMembershipProvider` et `SqlRoleProvider` , ainsi que les schémas pour les fournisseurs basés sur SQL pour d’autres frameworks ASP.NET 2,0. Nous devons fournir deux bits d’informations à l' `aspnet_regsql.exe` outil :

- Si vous souhaitez ajouter ou supprimer des services d’application, et
- Base de données à partir de laquelle ajouter ou supprimer le schéma des services d’application

Dans l’invite de la base de données à utiliser, l' `aspnet_regsql.exe` outil nous invite à fournir le nom du serveur sur lequel réside la base de données, les informations d’identification de sécurité pour la connexion à la base de données et le nom de la base de données. Si vous utilisez l’édition non Express de SQL Server, vous devez déjà connaître ces informations, car il s’agit des mêmes informations que vous devez fournir via une chaîne de connexion lors de l’utilisation de la base de données via une page Web ASP.NET. Toutefois, le fait de déterminer le nom du serveur et de la base de données lors de l’utilisation d’une base de données SQL Server 2005 Express Edition dans le `App_Data` dossier est un peu plus complexe.

La section suivante examine un moyen simple de spécifier le nom du serveur et de la base de données pour une base de données SQL Server 2005 Express Edition dans le `App_Data` dossier. Si vous n’utilisez pas SQL Server 2005 Express Edition n’hésitez pas à passer directement à la section installation de la Services d’application.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theapp_datafolder"></a>Détermination du nom du serveur et de la base de données d’une base de données SQL Server 2005 Express Edition dans le `App_Data` dossier

Pour utiliser l’outil, `aspnet_regsql.exe` vous devez connaître le nom du serveur et celui de la base de données. Le nom du serveur est `localhost\InstanceName` . La plupart du possible, *InstanceName* est `SQLExpress` . Toutefois, si vous avez installé SQL Server 2005 Express Edition manuellement (c’est-à-dire que vous ne l’avez pas installé automatiquement lors de l’installation de Visual Studio), il est possible que vous ayez sélectionné un autre nom d’instance.

Le nom de la base de données est un peu plus difficile à déterminer. Les bases de données dans le `App_Data` dossier ont généralement un nom de base de données qui comprend un [identificateur global unique](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) , ainsi que le chemin d’accès au fichier de base de données. Nous devons déterminer ce nom de base de données afin d’ajouter le schéma des services d’application via `aspnet_regsql.exe` .

Le moyen le plus simple de déterminer le nom de la base de données consiste à l’examiner dans SQL Server Management Studio. SQL Server Management Studio fournit une interface graphique pour la gestion des bases de données SQL Server 2005, mais n’est pas fournie avec l’édition Express de SQL Server 2005. La bonne nouvelle, c’est que [vous pouvez télécharger](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) l’édition Express gratuite de SQL Server Management Studio.

> [!NOTE]
> Si vous disposez également d’une version de non-Express Edition de SQL Server 2005 installée sur votre bureau, la version complète de Management Studio est probablement installée. Vous pouvez utiliser la version complète pour déterminer le nom de la base de données, en suivant les mêmes étapes que celles décrites ci-dessous pour l’édition Express.

Commencez par fermer Visual Studio pour vous assurer que tous les verrous imposés par Visual Studio sur le fichier de base de données sont fermés. Ensuite, lancez SQL Server Management Studio et connectez-vous à la `localhost\InstanceName` base de données pour SQL Server 2005 Express Edition. Comme indiqué précédemment, il est probable que le nom de l’instance soit `SQLExpress` . Pour l’option d’authentification, sélectionnez authentification Windows.

[![Se connecter à l’instance de SQL Server 2005 Express Edition](creating-the-membership-schema-in-sql-server-vb/_static/image8.png)](creating-the-membership-schema-in-sql-server-vb/_static/image7.png)

**Figure 3**: connexion à l’instance SQL Server 2005 Express Edition ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image9.png))

Après vous être connecté à l’instance SQL Server 2005 Express Edition, Management Studio affiche les dossiers des bases de données, les paramètres de sécurité, les objets serveur, etc. Si vous développez l’onglet bases de données, vous verrez que la `SecurityTutorials.mdf` base de données n’est *pas* inscrite dans l’instance de base de données. nous devons d’abord attacher la base de données.

Cliquez avec le bouton droit sur le dossier bases de données, puis choisissez attacher dans le menu contextuel. La boîte de dialogue attacher des bases de données s’affiche. À partir de là, cliquez sur le bouton Ajouter, accédez à la `SecurityTutorials.mdf` base de données, puis cliquez sur OK. La figure 4 illustre la boîte de dialogue attacher des bases de données une fois que la `SecurityTutorials.mdf` base de données a été sélectionnée. La figure 5 montre l’Explorateur d’objets de Management Studio une fois que la base de données a été attachée avec succès.

[![Attacher la base de données SecurityTutorials. mdf](creating-the-membership-schema-in-sql-server-vb/_static/image11.png)](creating-the-membership-schema-in-sql-server-vb/_static/image10.png)

**Figure 4**: attacher la `SecurityTutorials.mdf` base de données ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image12.png))

[![La base de données SecurityTutorials. mdf s’affiche dans le dossier bases de données.](creating-the-membership-schema-in-sql-server-vb/_static/image14.png)](creating-the-membership-schema-in-sql-server-vb/_static/image13.png)

**Figure 5**: la `SecurityTutorials.mdf` base de données s’affiche dans le dossier bases de données ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image15.png))

Comme le montre la figure 5, le `SecurityTutorials.mdf` nom de la base de données est plutôt abstruse. Nous allons le remplacer par un nom plus mémorable (et plus facile à taper). Cliquez avec le bouton droit sur la base de données, choisissez Renommer dans le menu contextuel, puis renommez-le `SecurityTutorialsDatabase` . Cela ne modifie pas le nom de fichier, mais uniquement le nom que la base de données utilise pour s’identifier auprès de SQL Server.

[![Renommez la base de données en SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-vb/_static/image17.png)](creating-the-membership-schema-in-sql-server-vb/_static/image16.png)

**Figure 6**: renommer la base de données en `SecurityTutorialsDatabase` ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image18.png))

À ce stade, nous savons les noms du serveur et de la base de données pour le `SecurityTutorials.mdf` fichier de base de données : `localhost\InstanceName` et `SecurityTutorialsDatabase` , respectivement. Nous sommes maintenant prêts à installer les services d’application via l' `aspnet_regsql.exe` outil.

### <a name="installing-the-application-services"></a>Installation du Services d’application

Pour lancer l' `aspnet_regsql.exe` outil, accédez au menu Démarrer, puis choisissez Exécuter. Entrez `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` dans la zone de texte, puis cliquez sur OK. Vous pouvez également utiliser l’Explorateur Windows pour explorer le dossier approprié et double-cliquer sur le `aspnet_regsql.exe` fichier. Les deux approches ont les mêmes résultats.

L’exécution de l' `aspnet_regsql.exe` outil sans arguments de ligne de commande lance l’interface utilisateur graphique de l’Assistant Installation de ASP.NET SQL Server. L’Assistant facilite l’ajout ou la suppression des services d’application ASP.NET sur une base de données spécifiée. Le premier écran de l’Assistant, illustré à la figure 7, décrit l’objectif de l’outil.

[![Utilisez l’Assistant Installation de ASP.NET SQL Server permet d’ajouter le schéma d’appartenance](creating-the-membership-schema-in-sql-server-vb/_static/image20.png)](creating-the-membership-schema-in-sql-server-vb/_static/image19.png)

**Figure 7**: utilisation de l’Assistant installation de ASP.net SQL Server permet d’ajouter le schéma d’appartenance ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image21.png))

La deuxième étape de l’Assistant nous demande si vous souhaitez ajouter les services d’application ou les supprimer. Étant donné que nous souhaitons ajouter les tables, les vues et les procédures stockées nécessaires à `SqlMembershipProvider` , choisissez l’option configurer les SQL Server pour les services d’application. Par la suite, si vous souhaitez supprimer ce schéma de votre base de données, réexécutez cet Assistant, mais choisissez plutôt l’option Supprimer les informations des services d’application d’une base de données existante.

[![Choisir l’option configurer le SQL Server pour Services d’application](creating-the-membership-schema-in-sql-server-vb/_static/image23.png)](creating-the-membership-schema-in-sql-server-vb/_static/image22.png)

**Figure 8**: choisissez l’option configurer le SQL Server pour services d’application ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image24.png))

La troisième étape vous invite à entrer les informations de la base de données : le nom du serveur, les informations d’authentification et le nom de la base de données. Si vous avez déjà utilisé ce didacticiel et que vous avez ajouté la `SecurityTutorials.mdf` base de données à, vous l’avez `App_Data` attachée à, puis `localhost\InstanceName` l’avez renommée en `SecurityTutorialsDatabase` , puis utilisez les valeurs suivantes :

- Serveur : `localhost\InstanceName`
- Authentification Windows
- Database `SecurityTutorialsDatabase`

[![Entrer les informations de la base de données](creating-the-membership-schema-in-sql-server-vb/_static/image26.png)](creating-the-membership-schema-in-sql-server-vb/_static/image25.png)

**Figure 9**: entrer les informations de la base de données ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image27.png))

Après avoir entré les informations de la base de données, cliquez sur suivant. La dernière étape résume les étapes à effectuer. Cliquez sur suivant pour installer les services d’application, puis sur Terminer pour terminer l’Assistant.

> [!NOTE]
> Si vous avez utilisé Management Studio pour attacher la base de données et renommer le fichier de base de données, veillez à détacher la base de données et à fermer Management Studio avant de rouvrir Visual Studio. Pour détacher la `SecurityTutorialsDatabase` base de données, cliquez avec le bouton droit sur le nom de la base de données et, dans le menu tâches, choisissez détacher.

Une fois l’Assistant terminé, revenez à Visual Studio et accédez au explorateur de base de données. Développez le dossier Tables. Vous devez voir une série de tables dont les noms commencent par le préfixe `aspnet_` . De même, un grand nombre de vues et de procédures stockées se trouvent sous les dossiers vues et procédures stockées. Ces objets de base de données composent le schéma des services d’application. Nous allons examiner les objets de base de données spécifiques aux appartenances et aux rôles à l’étape 3.

[![Plusieurs tables, vues et procédures stockées ont été ajoutées à la base de données.](creating-the-membership-schema-in-sql-server-vb/_static/image29.png)](creating-the-membership-schema-in-sql-server-vb/_static/image28.png)

**Figure 10**: plusieurs tables, vues et procédures stockées ont été ajoutées à la base de données ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image30.png))

> [!NOTE]
> L' `aspnet_regsql.exe` interface utilisateur graphique de l’outil installe l’ensemble du schéma des services d’application. Toutefois, lors `aspnet_regsql.exe` de l’exécution à partir de la ligne de commande, vous pouvez spécifier les composants de services d’application à installer (ou supprimer). Par conséquent, si vous souhaitez ajouter uniquement les tables, les vues et les procédures stockées nécessaires pour `SqlMembershipProvider` les `SqlRoleProvider` fournisseurs et, exécutez `aspnet_regsql.exe` à partir de la ligne de commande. Vous pouvez également exécuter manuellement le sous-ensemble approprié de scripts T-SQL CREATE utilisés par `aspnet_regsql.exe` . Ces scripts se trouvent dans le `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` dossier avec des noms tels que `InstallCommon.sql` , `InstallMembership.sql` , `InstallRoles.sql` ,, `InstallProfile.sql` `InstallSqlState.sql` , et ainsi de suite.

À ce stade, nous avons créé les objets de base de données requis par le `SqlMembershipProvider` . Toutefois, nous devons toujours indiquer à l’infrastructure d’appartenance qu’il doit utiliser `SqlMembershipProvider` (par opposition à `ActiveDirectoryMembershipProvider` ) et que le `SqlMembershipProvider` doit utiliser la `SecurityTutorials` base de données. Nous allons voir comment spécifier le fournisseur à utiliser et comment personnaliser les paramètres du fournisseur sélectionné à l’étape 4. Mais tout d’abord, examinons plus en détail les objets de base de données qui viennent d’être créés.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Étape 3 : examiner les tables principales du schéma

Lorsque vous travaillez avec les infrastructures d’appartenance et de rôles dans une application ASP.NET, les détails de l’implémentation sont encapsulés par le fournisseur. Dans les prochains didacticiels, nous allons interagir avec ces frameworks via les `Membership` classes et de .NET Framework `Roles` . Lors de l’utilisation de ces API de haut niveau, nous n’avons pas à nous soucier des détails de bas niveau, tels que les requêtes exécutées ou les tables modifiées par `SqlMembershipProvider` et `SqlRoleProvider` .

À partir de cela, nous pourrions utiliser en toute confiance les infrastructures d’appartenance et de rôles sans avoir exploré le schéma de base de données créé à l’étape 2. Toutefois, lors de la création des tables pour stocker des données d’application, nous pouvons être amenés à créer des entités liées aux utilisateurs ou aux rôles. Elle vous permet de vous familiariser avec les `SqlMembershipProvider` `SqlRoleProvider` schémas et lors de l’établissement de contraintes de clé étrangère entre les tables de données d’application et les tables créées à l’étape 2. En outre, dans certains cas rares, nous pouvons être amenés à interagir avec l’utilisateur et les magasins de rôles directement au niveau de la base de données (et non par le biais des `Membership` `Roles` classes ou).

### <a name="partitioning-the-user-store-into-applications"></a>Partitionnement du magasin de l’utilisateur en applications

Les infrastructures d’appartenance et de rôles sont conçues de façon à ce qu’un seul utilisateur et magasin de rôles puissent être partagés entre plusieurs applications différentes. Une application ASP.NET qui utilise les infrastructures d’appartenance ou de rôles doit spécifier la partition d’application à utiliser. En bref, plusieurs applications Web peuvent utiliser le même utilisateur et les mêmes magasins de rôles. La figure 11 décrit les magasins d’utilisateurs et de rôles qui sont partitionnés en trois applications : HRSite, CustomerSite et SalesSite. Ces trois applications Web ont chacune leurs propres rôles et utilisateurs uniques, mais elles stockent toutes physiquement leurs informations de compte d’utilisateur et de rôle dans les mêmes tables de base de données.

[![Les comptes d’utilisateurs peuvent être partitionnés entre plusieurs applications](creating-the-membership-schema-in-sql-server-vb/_static/image32.png)](creating-the-membership-schema-in-sql-server-vb/_static/image31.png)

**Figure 11**: les comptes d’utilisateurs peuvent être partitionnés entre plusieurs applications ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image33.png))

La `aspnet_Applications` table définit ces partitions. Chaque application qui utilise la base de données pour stocker les informations de compte d’utilisateur est représentée par une ligne dans cette table. La `aspnet_Applications` table contient quatre colonnes : `ApplicationId` , `ApplicationName` , `LoweredApplicationName` et `Description` .`ApplicationId` est de type [`uniqueidentifier`](https://msdn.microsoft.com/library/ms187942.aspx) et est la clé primaire de la table ; `ApplicationName` fournit un nom convivial unique pour chaque application.

Les autres tables liées à l’appartenance et au rôle renvoient au `ApplicationId` champ dans `aspnet_Applications` . Par exemple, la `aspnet_Users` table, qui contient un enregistrement pour chaque compte d’utilisateur, possède un `ApplicationId` champ de clé étrangère ; Ditto pour la `aspnet_Roles` table. Le `ApplicationId` champ de ces tables spécifie la partition d’application à laquelle le compte d’utilisateur ou le rôle appartient.

### <a name="storing-user-account-information"></a>Stockage des informations de compte d’utilisateur

Les informations de compte d’utilisateur sont hébergées dans deux tables : `aspnet_Users` et `aspnet_Membership` . La `aspnet_Users` table contient des champs qui contiennent les informations de compte d’utilisateur essentielles. Les trois colonnes les plus pertinentes sont les suivantes :

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` est la clé primaire (et de type `uniqueidentifier` ). `UserName` est de type `nvarchar(256)` et, avec le mot de passe, constitue les informations d’identification de l’utilisateur. (Le mot de passe d’un utilisateur est stocké dans la `aspnet_Membership` table.) `ApplicationId` lie le compte d’utilisateur à une application particulière dans `aspnet_Applications` . Il existe une [ `UNIQUE` contrainte](https://msdn.microsoft.com/library/ms191166.aspx) composite sur `UserName` les `ApplicationId` colonnes et. Cela permet de s’assurer que, dans une application donnée, chaque nom d’utilisateur est unique, mais qu’il est possible de l' `UserName` utiliser dans des applications différentes.

La `aspnet_Membership` table contient des informations supplémentaires sur le compte d’utilisateur, telles que le mot de passe de l’utilisateur, l’adresse de messagerie, la date et l’heure de la dernière connexion, etc. Il existe une correspondance un-à-un entre les enregistrements dans `aspnet_Users` les `aspnet_Membership` tables et. Cette relation est assurée par le `UserId` champ dans `aspnet_Membership` , qui sert de clé primaire de la table. Comme la `aspnet_Users` table, `aspnet_Membership` comprend un `ApplicationId` champ qui associe ces informations à une partition d’application particulière.

### <a name="securing-passwords"></a>Sécurisation des mots de passe

Les informations de mot de passe sont stockées dans la `aspnet_Membership` table. Le `SqlMembershipProvider` permet de stocker les mots de passe dans la base de données à l’aide de l’une des trois techniques suivantes :

- **Clear** : le mot de passe est stocké dans la base de données en tant que texte brut. Je déconseille fortement d’utiliser cette option. Si la base de données est compromise, qu’il s’agisse d’un pirate qui trouve une porte dérobée ou un employé mécontent disposant d’un accès à la base de données, les informations d’identification de chaque utilisateur sont là pour le prélèvement.
- Les mots de passe **hachés** sont hachés à l’aide d’un algorithme de hachage unidirectionnel et d’une valeur Salt générée de façon aléatoire. Cette valeur hachée (avec le Salt) est stockée dans la base de données.
- **Chiffré** : une version chiffrée du mot de passe est stockée dans la base de données.

La technique de stockage du mot de passe utilisée dépend des `SqlMembershipProvider` paramètres spécifiés dans `Web.config` . Nous allons examiner la personnalisation des `SqlMembershipProvider` paramètres à l’étape 4. Le comportement par défaut consiste à stocker le hachage du mot de passe.

Les colonnes responsables du stockage du mot de passe sont `Password` , `PasswordFormat` et `PasswordSalt` . `PasswordFormat` champ de type dont la `int` valeur indique la technique utilisée pour stocker le mot de passe : 0 pour Clear ; 1 pour le hachage ; 2 pour le chiffrement. `PasswordSalt` reçoit une chaîne générée de façon aléatoire, quelle que soit la technique utilisée pour le stockage du mot de passe ; la valeur de `PasswordSalt` est utilisée uniquement lors du calcul du hachage du mot de passe. Enfin, la `Password` colonne contient les données de mot de passe réelles, qu’il s’agit du mot de passe en texte brut, du hachage du mot de passe ou du mot de passe chiffré.

Le tableau 1 illustre ce à quoi peuvent ressembler ces trois colonnes pour les différentes techniques de stockage lors du stockage du mot de passe MySecret ! .

| **Technique de stockage &lt; \_ O3A \_ p/&gt;** | **O3A de mot de passe &lt; \_ \_ p/&gt;** | **PasswordFormat &lt; \_ O3A \_ p/&gt;** | **PasswordSalt &lt; \_ O3A \_ p/&gt;** |
| --- | --- | --- | --- |
| Désactiver | MySecret! | 0 | tTnkPlesqissc2y2SMEygA = = |
| Hachée | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM = | 1 | wFgjUfhdUFOCKQiI61vtiQ = = |
| Chiffré | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/AA/oqAXGLHJNBw = = |

**Tableau 1**: exemples de valeurs pour les champs liés au mot de passe lors du stockage du mot de passe mysecret !

> [!NOTE]
> Le chiffrement ou l’algorithme de hachage particulier utilisé par le `SqlMembershipProvider` est déterminé par les paramètres de l' `<machineKey>` élément. Nous avons abordé cet élément de configuration à l’étape 3 du didacticiel configuration de l' <a id="Tutorial3"></a> [*authentification par formulaire et Rubriques avancées*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) .

### <a name="storing-roles-and-role-associations"></a>Stockage des rôles et des associations de rôles

L’infrastructure de rôles permet aux développeurs de définir un ensemble de rôles et de spécifier les utilisateurs qui appartiennent aux rôles. Ces informations sont capturées dans la base de données par le biais de deux tables : `aspnet_Roles` et `aspnet_UsersInRoles` . Chaque enregistrement de la `aspnet_Roles` table représente un rôle pour une application particulière. À l’instar de la `aspnet_Users` table, la `aspnet_Roles` table comporte trois colonnes pertinentes pour notre discussion :

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` est la clé primaire (et de type `uniqueidentifier` ). `RoleName` est de type `nvarchar(256)`. Et `ApplicationId` associe le compte d’utilisateur à une application particulière dans `aspnet_Applications` . Il existe une contrainte composite `UNIQUE` sur `RoleName` les `ApplicationId` colonnes et, garantissant que chaque nom de rôle est unique dans une application donnée.

La `aspnet_UsersInRoles` table sert de mappage entre les utilisateurs et les rôles. Il n’y a que deux colonnes, `UserId` et `RoleId` -et ensemble, elles constituent une clé primaire composite.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Étape 4 : spécification du fournisseur et personnalisation de ses paramètres

Tous les frameworks qui prennent en charge le modèle de fournisseur (tels que les infrastructures d’appartenance et de rôles) n’ont pas de détails d’implémentation eux-mêmes et délèguent plutôt cette responsabilité à une classe de fournisseur. Dans le cas de l’infrastructure d’appartenance, la `Membership` classe définit l’API pour la gestion des comptes d’utilisateur, mais elle n’interagit pas directement avec les magasins des utilisateurs. Au lieu de cela, les méthodes de la classe déplacent `Membership` la demande au fournisseur configuré. nous utiliserons le `SqlMembershipProvider` . Quand vous appelez l’une des méthodes de la `Membership` classe, comment l’infrastructure d’appartenance sait-elle déléguer l’appel à l’objet `SqlMembershipProvider` ?

La `Membership` classe a une [ `Providers` propriété](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) qui contient une référence à toutes les classes de fournisseur inscrites pouvant être utilisées par l’infrastructure d’appartenance. Chaque fournisseur inscrit a un nom et un type associés. Le nom offre une méthode conviviale pour référencer un fournisseur particulier dans la `Providers` collection, tandis que le type identifie la classe de fournisseur. En outre, chaque fournisseur inscrit peut inclure des paramètres de configuration. Les paramètres de configuration de l’infrastructure d’appartenance incluent `PasswordFormat` et `requiresUniqueEmail` , entre autres. Consultez le tableau 2 pour obtenir la liste complète des paramètres de configuration utilisés par le `SqlMembershipProvider` .

Le `Providers` contenu de la propriété est spécifié via les paramètres de configuration de l’application Web. Par défaut, toutes les applications Web ont un fournisseur nommé `AspNetSqlMembershipProvider` de type `SqlMembershipProvider` . Ce fournisseur d’appartenances par défaut est inscrit dans `machine.config` (à l’emplacement `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG` ) :

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample1.xml)]

Comme le montre le balisage ci-dessus, l' [ `<membership>` élément](https://msdn.microsoft.com/library/1b9hw62f.aspx) définit les paramètres de configuration de l’infrastructure d’appartenance, tandis que l' [ `<providers>` élément enfant](https://msdn.microsoft.com/library/6d4936ht.aspx) spécifie les fournisseurs inscrits. Des fournisseurs peuvent être ajoutés ou supprimés à l’aide des [`<add>`](https://msdn.microsoft.com/library/whae3t94.aspx) [`<remove>`](https://msdn.microsoft.com/library/aykw9a6d.aspx) éléments ou ; utilisez l' [`<clear>`](https://msdn.microsoft.com/library/t062y6yc.aspx) élément pour supprimer tous les fournisseurs actuellement inscrits. Comme le montre le balisage ci-dessus, `machine.config` ajoute un fournisseur nommé `AspNetSqlMembershipProvider` de type `SqlMembershipProvider` .

Outre les `name` `type` attributs et, l' `<add>` élément contient des attributs qui définissent les valeurs de divers paramètres de configuration. Le tableau 2 répertorie les `SqlMembershipProvider` paramètres de configuration spécifiques à disponibles, ainsi qu’une description de chacun d’eux.

> [!NOTE]
> Toutes les valeurs par défaut indiquées dans le tableau 2 font référence aux valeurs par défaut définies dans la `SqlMembershipProvider` classe. Notez que tous les paramètres de configuration de `AspNetSqlMembershipProvider` correspondent aux valeurs par défaut de la `SqlMembershipProvider` classe. Par exemple, s’il n’est pas spécifié dans un fournisseur d’appartenances, le paramètre a la valeur `requiresUniqueEmail` true par défaut. Toutefois, le `AspNetSqlMembershipProvider` substitue cette valeur par défaut en spécifiant explicitement une valeur de `false` .

| **Définition de &lt; \_ O3A \_ p/&gt;** | **Description &lt; \_ O3A \_ p/&gt;** |
| --- | --- |
| `ApplicationName` | Souvenez-vous que l’infrastructure d’appartenance permet de partitionner un seul magasin d’utilisateur entre plusieurs applications. Ce paramètre indique le nom de la partition d’application utilisée par le fournisseur d’appartenances. Si cette valeur n’est pas explicitement spécifiée, elle est définie, au moment de l’exécution, à la valeur du chemin d’accès de la racine virtuelle de l’application. |
| `commandTimeout` | Spécifie la valeur du délai d’expiration de la commande SQL (en secondes). La valeur par défaut est 30. |
| `connectionStringName` | Nom de la chaîne de connexion dans l' `<connectionStrings>` élément à utiliser pour se connecter à la base de données du magasin de l’utilisateur. Cette valeur est requise. |
| `description` | Fournit une description conviviale du fournisseur inscrit. |
| `enablePasswordRetrieval` | Spécifie si les utilisateurs peuvent récupérer leur mot de passe oublié. La valeur par défaut est `false`. |
| `enablePasswordReset` | Indique si les utilisateurs sont autorisés à réinitialiser leur mot de passe. La valeur par défaut est `true`. |
| `maxInvalidPasswordAttempts` | Nombre maximal de tentatives de connexion qui peuvent se produire pour un utilisateur donné pendant l’opération spécifiée `passwordAttemptWindow` avant que l’utilisateur ne soit verrouillé. La valeur par défaut est 5. |
| `minRequiredNonalphanumericCharacters` | Nombre minimal de caractères non alphanumériques qui doivent apparaître dans le mot de passe d’un utilisateur. Cette valeur doit être comprise entre 0 et 128 ; la valeur par défaut est 1. |
| `minRequiredPasswordLength` | Nombre minimal de caractères requis dans un mot de passe. Cette valeur doit être comprise entre 0 et 128 ; la valeur par défaut est 7. |
| `name` | Nom du fournisseur inscrit. Cette valeur est requise. |
| `passwordAttemptWindow` | Nombre de minutes pendant lesquelles les échecs de tentatives de connexion sont suivis. Si un utilisateur fournit des informations d’identification de connexion non valides `maxInvalidPasswordAttempts` dans cette fenêtre spécifiée, il est verrouillé. La valeur par défaut est 10. |
| `PasswordFormat` | Format de stockage du mot de passe : `Clear` , `Hashed` ou `Encrypted` . Par défaut, il s’agit de `Hashed`. |
| `passwordStrengthRegularExpression` | Si elle est fournie, cette expression régulière est utilisée pour évaluer la force du mot de passe sélectionné par l’utilisateur lors de la création d’un nouveau compte ou lors de la modification de son mot de passe. La valeur par défaut est une chaîne vide. |
| `requiresQuestionAndAnswer` | Spécifie si un utilisateur doit répondre à sa question de sécurité lors de la récupération ou de la réinitialisation de son mot de passe. La valeur par défaut est `true`. |
| `requiresUniqueEmail` | Indique si tous les comptes d’utilisateur d’une partition d’application donnée doivent avoir une adresse de messagerie unique. La valeur par défaut est `true`. |
| `type` | Spécifie le type du fournisseur. Cette valeur est requise. |

**Tableau 2**: paramètres d’appartenance et de `SqlMembershipProvider` Configuration

En plus de `AspNetSqlMembershipProvider` , d’autres fournisseurs d’appartenances peuvent être inscrits au niveau application par application en ajoutant un balisage similaire au `Web.config` fichier.

> [!NOTE]
> L’infrastructure Roles fonctionne à peu près de la même façon : il existe un fournisseur de rôle inscrit par défaut dans `machine.config` et les fournisseurs inscrits peuvent être personnalisés pour chaque application dans `Web.config` . Nous examinerons en détail l’infrastructure des rôles et son balisage de configuration dans un prochain didacticiel.

### <a name="customizing-thesqlmembershipprovidersettings"></a>Personnalisation des `SqlMembershipProvider` paramètres

L’attribut par défaut `SqlMembershipProvider` ( `AspNetSqlMembershipProvider` ) a la `connectionStringName` valeur `LocalSqlServer` . À l’instar du `AspNetSqlMembershipProvider` fournisseur, le nom de la chaîne `LocalSqlServer` de connexion est défini dans `machine.config` .

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample2.xml)]

Comme vous pouvez le voir, cette chaîne de connexion définit une base de données SQL 2005 Express Edition située dans | DataDirectory | aspnetdb. mdf. La chaîne | DataDirectory | est traduite au moment de l’exécution pour pointer vers le `~/App_Data/` répertoire, donc le chemin d’accès de la base de données | DataDirectory | aspnetdb. mdf se traduit par `~/App_Data` / `aspnet.mdf` .

Si nous n’avons pas spécifié d’informations sur le fournisseur d’appartenances dans le fichier de l’application `Web.config` , l’application utilise le fournisseur d’appartenances inscrit par défaut, `AspNetSqlMembershipProvider` . Si la `~/App_Data/aspnet.mdf` base de données n’existe pas, le runtime ASP.net le crée automatiquement et ajoute le schéma des services d’application. Toutefois, nous ne souhaitons pas utiliser la `aspnet.mdf` base de données. au lieu de cela, nous voulons utiliser la `SecurityTutorials.mdf` base de données que nous avons créée à l’étape 2. Cette modification peut être effectuée de deux manières :

- <strong>Spécifiez une valeur pour l'</strong> <strong>`LocalSqlServer`</strong> <strong>nom de la chaîne de connexion dans</strong> <strong>`Web.config`</strong> <strong>.</strong> En remplaçant la `LocalSqlServer` valeur de nom de la chaîne de connexion dans `Web.config` , nous pouvons utiliser le fournisseur d’appartenances inscrit par défaut ( `AspNetSqlMembershipProvider` ) et le faire fonctionner correctement avec la `SecurityTutorials.mdf` base de données. Cette approche est correcte si vous êtes content avec les paramètres de configuration spécifiés par `AspNetSqlMembershipProvider` . Pour plus d’informations sur cette technique, consultez le billet de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/), [configuring ASP.NET 2,0 Services d’application to use SQL Server 2000 ou SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Ajouter un nouveau fournisseur inscrit de type</strong> <strong>`SqlMembershipProvider`</strong> <strong>et configurez son</strong> <strong>`connectionStringName`</strong> <strong>paramètre pour pointer sur le</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>base de données.</strong> Cette approche est utile dans les scénarios où vous souhaitez personnaliser d’autres propriétés de configuration en plus de la chaîne de connexion à la base de données. Dans mes propres projets, j’utilise toujours cette approche en raison de sa flexibilité et de sa lisibilité.

Avant de pouvoir ajouter un nouveau fournisseur inscrit qui référence la `SecurityTutorials.mdf` base de données, nous devons d’abord ajouter une valeur de chaîne de connexion appropriée dans la `<connectionStrings>` section de `Web.config` . Le balisage suivant ajoute une nouvelle chaîne de connexion nommée `SecurityTutorialsConnectionString` qui fait référence `SecurityTutorials.mdf` à la base de données SQL Server 2005 Express Edition dans le `App_Data` dossier.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample3.xml)]

> [!NOTE]
> Si vous utilisez un autre fichier de base de données, mettez à jour la chaîne de connexion en fonction des besoins. Pour plus d’informations sur la façon de former la chaîne de connexion correcte, consultez [connectionStrings.com](http://www.connectionstrings.com/).

Ensuite, ajoutez le balisage de configuration d’appartenance suivant au `Web.config` fichier. Ce balisage inscrit un nouveau fournisseur nommé `SecurityTutorialsSqlMembershipProvider` .

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample4.xml)]

Outre l’inscription du `SecurityTutorialsSqlMembershipProvider` fournisseur, le balisage ci-dessus définit le `SecurityTutorialsSqlMembershipProvider` comme fournisseur par défaut (via l' `defaultProvider` attribut dans l' `<membership>` élément). Rappelez-vous que l’infrastructure d’appartenance peut avoir plusieurs fournisseurs inscrits. Étant donné que `AspNetSqlMembershipProvider` est inscrit en tant que premier fournisseur dans `machine.config` , il sert de fournisseur par défaut, sauf indication contraire.

Actuellement, notre application a deux fournisseurs inscrits : `AspNetSqlMembershipProvider` et `SecurityTutorialsSqlMembershipProvider` . Toutefois, avant d’inscrire le `SecurityTutorialsSqlMembershipProvider` fournisseur, nous aurions pu effacer tous les fournisseurs précédemment inscrits en ajoutant un [ `<clear />` élément](https://msdn.microsoft.com/library/t062y6yc.aspx) juste avant notre `<add>` élément. Cela efface le `AspNetSqlMembershipProvider` de la liste des fournisseurs inscrits, ce qui signifie que est `SecurityTutorialsSqlMembershipProvider` le seul fournisseur d’appartenances inscrit. Si nous avons utilisé cette approche, nous n’aurions pas besoin de marquer `SecurityTutorialsSqlMembershipProvider` comme fournisseur par défaut, car il s’agit du seul fournisseur d’appartenances inscrit. Pour plus d’informations sur l’utilisation de `<clear />` , consultez Utilisation de lors de l' [Ajout de `<clear />` fournisseurs](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Notez que le `SecurityTutorialsSqlMembershipProvider` `connectionStringName` paramètre de fait référence au `SecurityTutorialsConnectionString` nom de la chaîne de connexion que vous venez d’ajouter et que son `applicationName` paramètre a la valeur SecurityTutorials. En outre, le `requiresUniqueEmail` paramètre a été défini sur `true` . Toutes les autres options de configuration sont identiques aux valeurs de `AspNetSqlMembershipProvider` . N’hésitez pas à apporter des modifications de configuration ici, si vous le souhaitez. Par exemple, vous pouvez renforcer la force du mot de passe en exigeant deux caractères non alphanumériques au lieu d’un, ou en accroissant la longueur du mot de passe à huit caractères au lieu de sept.

> [!NOTE]
> Souvenez-vous que l’infrastructure d’appartenance permet de partitionner un seul magasin d’utilisateur entre plusieurs applications. Le paramètre du fournisseur d’appartenances `applicationName` indique l’application utilisée par le fournisseur lors de l’utilisation du magasin de l’utilisateur. Il est important de définir explicitement une valeur pour le `applicationName` paramètre de configuration car si le `applicationName` n’est pas défini explicitement, il est assigné au chemin d’accès racine virtuel de l’application Web au moment de l’exécution. Cela fonctionne correctement tant que le chemin d’accès de la racine virtuelle de l’application ne change pas, mais si vous déplacez l’application vers un autre chemin d’accès, le `applicationName` paramètre change également. Dans ce cas, le fournisseur d’appartenances commencera à utiliser une partition d’application différente de celle précédemment utilisée. Les comptes d’utilisateur créés avant le déplacement se trouvent dans une partition d’application différente et ces utilisateurs ne pourront plus se connecter au site. Pour une discussion plus approfondie sur ce sujet, consultez [toujours définir la propriété lors de la configuration de l' `applicationName` appartenance à ASP.NET 2,0 et d’autres fournisseurs](https://weblogs.asp.net/scottgu/443634).

## <a name="summary"></a>Résumé

À ce stade, nous disposons d’une base de données avec les services d’application configurés ( `SecurityTutorials.mdf` ) et vous avez configuré notre application Web afin que l’infrastructure d’appartenance utilise le `SecurityTutorialsSqlMembershipProvider` fournisseur que nous venons d’inscrire. Ce fournisseur inscrit est de type `SqlMembershipProvider` et a son `connectionStringName` défini sur la chaîne de connexion appropriée ( `SecurityTutorialsConnectionString` ) et sa `applicationName` valeur définie explicitement.

Nous sommes maintenant prêts à utiliser l’infrastructure d’appartenance de notre application. Dans le didacticiel suivant, nous allons examiner comment créer des comptes d’utilisateur. Après cela, nous explorerons l’authentification des utilisateurs, l’exécution de l’autorisation basée sur l’utilisateur et le stockage d’informations supplémentaires relatives à l’utilisateur dans la base de données.

Bonne programmation !

### <a name="further-reading"></a>En savoir plus

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Toujours définir la `applicationName` propriété lors de la configuration de l’appartenance à ASP.NET 2,0 et d’autres fournisseurs](https://weblogs.asp.net/scottgu/443634)
- [Configuration de ASP.NET 2,0 Services d’application pour utiliser SQL Server 2000 ou SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Télécharger SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Examen de l’appartenance, des rôles et du profil ASP.NET 2.0 s](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [`<add>`Élément pour les fournisseurs pour l’appartenance](https://msdn.microsoft.com/library/whae3t94.aspx)
- [`<membership>`Élément](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [L' `<providers>` élément pour l’appartenance](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Utilisation `<clear />` lors de l’ajout de fournisseurs](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Travailler directement avec le `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Formation vidéo sur les rubriques contenues dans ce didacticiel

- [Présentation des appartenances d’ASP.NET](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Configuration de SQL pour l’utilisation de schémas d’appartenance](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Modification des paramètres d’appartenance dans le schéma d’appartenance par défaut](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs ouvrages ASP/ASP. NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est *[Sams vous apprend vous-même ASP.NET 2,0 en 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott peut être contacté à l’adresse [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/) .

### <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Alicja maziarz. Vous souhaitez revoir mes prochains articles MSDN ? Dans ce cas, déposez-moi une ligne à l’adresse [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) .

> [!div class="step-by-step"]
> [Précédent](storing-additional-user-information-cs.md) 
>  [Suivant](creating-user-accounts-vb.md)
