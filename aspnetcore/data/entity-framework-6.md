---
title: Bien démarrer avec ASP.NET Core et Entity Framework 6
author: rick-anderson
description: Cet article montre comment utiliser Entity Framework 6 dans une application ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/entity-framework-6
ms.openlocfilehash: b7679afbe4c364386fe8f16d22d7e9797a3e0c27
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57059156"
---
# <a name="get-started-with-aspnet-core-and-entity-framework-6"></a>Bien démarrer avec ASP.NET Core et Entity Framework 6

Par [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex) et [Tom Dykstra](https://github.com/tdykstra)

Cet article montre comment utiliser Entity Framework 6 dans une application ASP.NET Core.

## <a name="overview"></a>Vue d'ensemble

Pour utiliser Entity Framework 6, votre projet doit être compilé pour .NET Framework, car Entity Framework 6 ne prend pas en charge .NET Core. Si vous avez besoin de fonctionnalités multiplateformes, vous devez effectuer une mise à niveau vers [Entity Framework Core](/ef/).

La méthode recommandée pour utiliser Entity Framework 6 dans une application ASP.NET Core consiste à placer les classes de contexte et de modèle Entity Framework 6 (EF6) dans un projet de bibliothèque de classes qui cible le framework complet. Ajoutez une référence à la bibliothèque de classes depuis le projet ASP.NET Core. Consultez l’exemple [Solution Visual Studio avec des projets Entity Framework 6 et ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).

Vous ne pouvez pas mettre un contexte EF6 dans un projet ASP.NET Core, car les projets .NET Core ne prennent pas en charge toutes les fonctionnalités nécessaires pour les commandes EF6, comme *Enable-Migrations*.

Quel que soit le type de projet où vous placez votre contexte EF6, seuls les outils en ligne de commande EF6 fonctionnent avec un contexte EF6. Par exemple, `Scaffold-DbContext` est disponible seulement dans Entity Framework Core. Si vous devez effectuer une ingénierie à rebours pour une base de données dans un modèle EF6, consultez [Code First pour une base de données existante](https://msdn.microsoft.com/jj200620).

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a>Référencer le framework complet et EF6 dans le projet ASP.NET Core

Votre projet ASP.NET Core doit référencer .NET Framework et EF6. Par exemple, le fichier *.csproj* de votre projet ASP.NET Core doit être similaire à l’exemple suivant (seules les parties concernées du fichier sont montrées).

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

Quand vous créez un projet, utilisez le modèle **Application web ASP.NET Core (.NET Framework)**.

## <a name="handle-connection-strings"></a>Gérer les chaînes de connexion

Les outils en ligne de commande EF6 que vous allez utiliser dans le projet de bibliothèque de classes EF6 nécessitent un constructeur par défaut pour pouvoir instancier le contexte. Si vous voulez spécifier la chaîne de connexion à utiliser dans le projet ASP.NET Core, votre constructeur de contexte doit avoir un paramètre qui vous permet de passer la chaîne de connexion. Voici un exemple.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

Comme votre contexte EF6 n’a pas de constructeur sans paramètre, votre projet EF6 doit fournir une implémentation de [IDbContextFactory](https://msdn.microsoft.com/library/hh506876). Les outils en ligne de commande EF6 trouvent et utilisent cette implémentation : ils peuvent donc instancier le contexte. Voici un exemple.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

Dans cet exemple de code, l’implémentation de `IDbContextFactory` passe une chaîne de connexion codée en dur. Il s’agit de la chaîne de connexion utilisée par les outils en ligne de commande. Vous pouvez implémenter une stratégie pour garantir que la bibliothèque de classes utilise la même chaîne de connexion que celle de l’application appelante. Par exemple, vous pouvez obtenir la valeur d’une variable d’environnement dans les deux projets.

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a>Configurer l’injection de dépendances dans le projet ASP.NET Core

Dans le fichier *Startup.cs* du projet Core, configurez le contexte EF6 pour l’injection de dépendances dans `ConfigureServices`. Les objets de contexte EF doit avoir une durée de vie limitée à une demande.

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

Vous pouvez ensuite obtenir une instance du contexte dans vos contrôleurs en utilisant l’injection de dépendances. Le code est similaire à ce que vous pourriez écrire pour un contexte EF Core :

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a>Exemple d’application

Pour un exemple d’application opérationnelle, consultez [l’exemple de solution Visual Studio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) qui accompagne cet article.

Vous pouvez créer cet exemple à partir de zéro dans Visual Studio en effectuant les étapes suivantes :

* Créez une solution.

* **Ajouter** > **Nouveau projet** > **Web** > **Application web ASP.NET Core**
  * Dans la boîte de dialogue de sélection du modèle de projet, sélectionnez API et .NET Framework dans la liste déroulante

* **Ajouter** > **Nouveau projet** > **Windows Desktop** > **Bibliothèque de classes (.NET Framework)**

* Dans la **Console du Gestionnaire de package** pour les deux projets, exécutez la commande `Install-Package Entityframework`.

* Dans le projet de bibliothèque de classes, créez des classes de modèle de données et une classe de contexte, ainsi qu’une implémentation de `IDbContextFactory`.

* Dans la console du Gestionnaire de package, pour le projet de bibliothèque de classes, exécutez les commandes `Enable-Migrations` et `Add-Migration Initial`. Si vous avez défini le projet ASP.NET Core comme projet de démarrage, ajoutez `-StartupProjectName EF6` à ces commandes.

* Dans le projet Core, ajoutez une référence de projet au projet de bibliothèque de classes.

* Dans le projet Core, dans *Startup.cs*, inscrivez le contexte pour l’injection de dépendances.

* Dans le projet Core, dans *appsettings.json*, ajoutez la chaîne de connexion.

* Dans le projet Core, ajoutez un contrôleur et une ou plusieurs vues pour vérifier que vous pouvez lire et écrire des données. (Notez que la génération de modèles automatique d’ASP.NET Core MVC ne fonctionne pas avec le contexte EF6 référencé depuis la bibliothèque de classes.)

## <a name="summary"></a>Récapitulatif

Cet article a fourni des instructions de base pour utiliser Entity Framework 6 dans une application ASP.NET Core.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Entity Framework - Configuration basée sur le code](https://msdn.microsoft.com/data/jj680699.aspx)
