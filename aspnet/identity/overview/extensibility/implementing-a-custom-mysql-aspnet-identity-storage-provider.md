---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Implémentation d’un fournisseur de stockage ASP.NET Identity MySQL personnalisé - ASP.NET 4.x
author: raquelsa
description: ASP.NET Identity est un système extensible qui vous permet de créer votre propre fournisseur de stockage et le connecter à votre application sans utiliser de nouveau l’appli...
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: 224fa56a455affcbbdf76eceee5422850415037e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59420768"
---
# <a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Implémentation d’un fournisseur de stockage ASP.NET Identity MySQL personnalisé

par [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity est un système extensible qui vous permet de créer votre propre fournisseur de stockage et le connecter à votre application sans utiliser de nouveau l’application. Cette rubrique décrit comment créer un fournisseur de stockage MySQL pour ASP.NET Identity. Pour une vue d’ensemble de la création de fournisseurs de stockage personnalisés, consultez [vue d’ensemble de stockage fournisseurs personnalisés pour ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Pour suivre ce didacticiel, vous devez disposer de Visual Studio 2013 avec Update 2.
> 
> Ce didacticiel va :
> 
> - Montrer comment créer une instance de base de données MySQL sur Azure.
> - Montrer comment utiliser un outil client de MySQL (MySQL Workbench) pour créer des tables et gérer votre base de données à distance sur Azure.
> - Montrer comment remplacer l’implémentation de stockage d’identité ASP.NET par défaut par notre implémentation personnalisée sur un projet d’application MVC.
> 
> Ce didacticiel a été initialement écrit par Raquel Soares De Almeida et Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ). L’exemple de projet a été mis à jour pour l’identité 2.0 par Suhas Joshi. La rubrique a été mis à jour pour l’identité 2.0 par Tom FitzMacken.


## <a name="download-completed-project"></a>Projet de téléchargement terminé

À la fin de ce didacticiel, vous disposerez un projet d’application MVC avec ASP.NET Identity fonctionne avec une base de données MySQL hébergée sur Azure.

Vous pouvez télécharger le fournisseur de stockage MySQL terminé à [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).

## <a name="the-steps-you-will-perform"></a>Les étapes que vous allez effectuer

Dans ce didacticiel, vous allez :

1. Créer une base de données MySQL sur Azure
2. Créer des tables de l’identité ASP.NET dans MySQL
3. Créer une application MVC et configurez-le pour utiliser le fournisseur MySQL
4. Exécuter l'application

Cette rubrique ne couvre pas l’architecture d’identité ASP.NET et les décisions à prendre lors de l’implémentation d’un fournisseur de stockage client. Pour plus d’informations, consultez [vue d’ensemble de fournisseurs de stockage personnalisés pour ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Passez en revue les classes de fournisseur de stockage MySQL

Avant d’aborder les étapes pour créer le fournisseur de stockage MySQL, passons en revue les classes qui composent le fournisseur de stockage. Vous devez les classes qui gèrent les opérations de base de données et des classes qui sont appelées à partir de l’application pour gérer les utilisateurs et rôles.

### <a name="storage-classes"></a>Classes de stockage

- [IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -contient les propriétés de l’utilisateur.
- [UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) -contient des opérations d’ajout, de la mise à jour ou de récupération des utilisateurs.
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -contient les propriétés des rôles.
- [RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) -contient des opérations pour l’ajout, la suppression, la mise à jour et récupération des rôles.

### <a name="data-access-layer-classes"></a>Classes de couche d’accès aux données

Pour cet exemple, les classes de couche d’accès aux données contiennent des instructions SQL pour travailler avec les tables ; Toutefois, dans votre code, vous souhaiterez utiliser mappage objet-relationnel (ORM) tel que Entity Framework ou NHibernate. En particulier, votre application peut rencontrer des performances médiocres sans un ORM qui inclut le chargement différé et la mise en cache de l’objet. Pour plus d’informations, consultez [ASP.NET Identity 2.0 sans Entity Framework ?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -contient la connexion de base de données MySQL et des méthodes pour effectuer des opérations de base de données. UserStore et RoleStore sont instanciés avec une instance de cette classe.
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) -contient des opérations de base de données pour la table qui stocke les rôles.
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -contient des opérations de base de données pour la table qui stocke des revendications d’utilisateur.
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -contient des opérations de base de données pour la table qui stocke les informations de connexion utilisateur.
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) -contient des opérations de base de données pour la table qui stocke les utilisateurs sont affectés à des rôles.
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) -contient des opérations de base de données pour la table qui stocke les utilisateurs.

## <a name="create-a-mysql-database-instance-on-azure"></a>Créer une instance de base de données MySQL sur Azure

1. Connectez-vous au [portail Azure](https://manage.windowsazure.com/).
2. Cliquez sur **+ nouveau** en bas de la page, puis sélectionnez **STORE**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. Dans le **choisir et du module complémentaire** Assistant, sélectionnez **base de données MySQL ClearDB** , puis cliquez sur la flèche suivant en bas à droite de la boîte de dialogue.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Conservez la valeur par défaut **gratuit** planifier et de modifier le **nom** à **IdentityMySQLDatabase**. Sélectionnez la région plus proche de vous et puis cliquez sur la flèche suivant.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Cliquez sur la coche pour terminer la création de la base de données.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Une fois que votre base de données a été créé, vous pouvez le gérer à partir de la **modules complémentaires** onglet dans le portail de gestion.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Vous pouvez obtenir les informations de connexion de base de données en cliquant sur **informations de connexion** en bas de la page.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Copier la chaîne de connexion en cliquant sur le bouton de copie et l’enregistrer afin de pouvoir utiliser ultérieurement dans votre application MVC.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>Créer des tables de l’identité ASP.NET dans une base de données MySQL

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Installer l’outil MySQL Workbench pour vous connecter et gérer la base de données MySQL

1. Installer le **MySQL Workbench** outil à partir de la [page des téléchargements de MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Lancez l’application et ajouter un clic sur le **MySQLConnections +** pour ajouter une nouvelle connexion. Utiliser les données de chaîne de connexion que vous avez copié à partir de la base de données Azure MySQL que vous avez créé précédemment dans ce didacticiel.
3. Après avoir établi la connexion, ouvrez une nouvelle **requête** onglet ; collez les commandes à partir de [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) dans la requête et exécutez-le pour créer les tables de base de données.
4. Vous avez maintenant toutes les tables nécessaires ASP.NET Identity créés sur une base de données MySQL hébergée sur Azure, comme indiqué ci-dessous.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Créer un projet d’application MVC de modèle et configurez-le pour utiliser le fournisseur MySQL

Si nécessaire, installez [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) avec Update 2.

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>Télécharger le projet ASP.NET.Identity.MySQL à partir de CodePlex

1. Accédez à l’URL de référentiel [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).
2. Télécharger le code source.
3. Extrayez le fichier .zip dans un dossier local.
4. Ouvrez la solution AspNet.Identity.MySQL et générez-le.

### <a name="create-a-new-mvc-application-project-from-template"></a>Créer un nouveau projet d’application MVC de modèle

1. Bouton droit sur le **AspNet.Identity.MySQL** solution et **ajouter**, **nouveau projet**
2. Dans le **ajouter un nouveau projet** select de la boîte de dialogue **Visual C#** sur la gauche, puis **Web** , puis sélectionnez **Application Web ASP.NET**. Nommez votre projet **IdentityMySQLDemo**; puis cliquez sur OK.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le modèle MVC avec les options par défaut (qui inclut **comptes d’utilisateur individuels** comme méthode d’authentification) et cliquez sur **OK** .![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. Dans l’Explorateur de solutions, cliquez sur votre projet IdentityMySQLDemo et sélectionnez **gérer les Packages NuGet**. Dans la boîte de dialogue zone de texte de recherche, tapez **Identity.EntityFramework**. Sélectionnez ce package dans la liste des résultats, cliquez sur **désinstallation**. Vous devrez désinstaller le package de dépendance EntityFramework. Cliquez sur Oui comme nous ne seront plus ce package sur cette application.
5. Cliquez avec le bouton droit sur le projet IdentityMySQLDemo, sélectionnez **ajouter**, **référence, Solution, les projets ;** sélectionnez le projet AspNet.Identity.MySQL et cliquez sur **OK**.
6. Dans le projet IdentityMySQLDemo, remplacez toutes les références à  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   par  
     `using AspNet.Identity.MySQL;`
7. Dans IdentityModels.cs, définissez **ApplicationDbContext** dériver **MySqlDatabase** et inclure un constructeur qui accepte un seul paramètre avec le nom de connexion.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Ouvrez le fichier IdentityConfig.cs. Dans le **ApplicationUserManager.Create** (méthode), remplacez l’instanciation UserManager par le code suivant :  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Ouvrez le fichier web.config et remplacez la chaîne de connexion par défaut par cette entrée en remplaçant les valeurs en surbrillance avec la chaîne de connexion de la base de données MySQL que vous avez créé sur les étapes précédentes :  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Exécutez l’application et se connecter à la base de données MySQL

1. Bouton droit sur le **IdentityMySQLDemo** de projet et sélectionnez **définir comme projet de démarrage**
2. Appuyez sur **Ctrl + F5** pour générer et exécuter l’application.
3. Cliquez sur **inscrire** onglet en haut de la page.
4. Entrez un nouveau nom d’utilisateur et le mot de passe, puis cliquez sur **inscrire**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. Le nouvel utilisateur est maintenant inscrit et connecté.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Revenez à l’outil MySQL Workbench et inspecter le **IdentityMySQLDatabase** contenu de la table. Inspecter le tableau des utilisateurs pour les entrées que vous inscrivez des nouveaux utilisateurs.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’activation d’autres méthodes d’authentification sur cette application, consultez [créer une application ASP.NET MVC 5 avec Facebook et Google OAuth2 et OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Pour apprendre à intégrer votre base de données avec OAuth et de définir des rôles pour limiter l’accès des utilisateurs à votre application, consultez [déployer une application sécurisée ASP.NET MVC 5 avec appartenance, OAuth et base de données SQL dans Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
