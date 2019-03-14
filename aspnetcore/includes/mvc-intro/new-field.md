---
ms.openlocfilehash: e098ad497b13b4add583de017885cdbed952a421
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57036756"
---
<!-- This include not used by windows version -->
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="773fa-101">Ajouter un nouveau champ à une application ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="773fa-101">Add a new field to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="773fa-102">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="773fa-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="773fa-103">Ce didacticiel ajoute un nouveau champ à la table `Movies`.</span><span class="sxs-lookup"><span data-stu-id="773fa-103">This tutorial will add a new field to the `Movies` table.</span></span> <span data-ttu-id="773fa-104">Nous supprimons la base de données et nous en créons une nouvelle quand nous changeons le schéma (nous ajoutons un nouveau champ).</span><span class="sxs-lookup"><span data-stu-id="773fa-104">We'll drop the database and create a new one when we change the schema (add a new field).</span></span> <span data-ttu-id="773fa-105">Ce workflow fonctionne bien au début du développement, quand nous n’avons pas de données de production à conserver.</span><span class="sxs-lookup"><span data-stu-id="773fa-105">This workflow works well early in development when we don't have any production data to perserve.</span></span>

<span data-ttu-id="773fa-106">Une fois que votre application est déployée et que vous avez des données à conserver, vous ne pouvez pas supprimer votre base de données quand vous devez changer le schéma.</span><span class="sxs-lookup"><span data-stu-id="773fa-106">Once your app is deployed and you have data that you need to perserve, you can't drop your DB when you need to change the schema.</span></span> <span data-ttu-id="773fa-107">Entity Framework [Migrations Code First](/ef/core/get-started/aspnetcore/new-db) vous permet de mettre à jour votre schéma et de migrer la base de données sans perte de données.</span><span class="sxs-lookup"><span data-stu-id="773fa-107">Entity Framework [Code First Migrations](/ef/core/get-started/aspnetcore/new-db) allows you to update your schema and migrate the database without losing data.</span></span> <span data-ttu-id="773fa-108">Les migrations sont une fonctionnalité répandue dans l’utilisation de SQL Server, mais SQLite ne prend pas en charge de nombreuses de schéma de migration : seules des migrations très simples sont donc possibles.</span><span class="sxs-lookup"><span data-stu-id="773fa-108">Migrations is a popular feature when using SQL Server, but SQLlite doesn't support many migration schema operations, so only very simply migrations are possible.</span></span> <span data-ttu-id="773fa-109">Pour plus d’informations, consultez [Limitations de SQLite](/ef/core/providers/sqlite/limitations).</span><span class="sxs-lookup"><span data-stu-id="773fa-109">See [SQLite Limitations](/ef/core/providers/sqlite/limitations) for more information.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="773fa-110">Ajout d’une propriété Rating au modèle Movie</span><span class="sxs-lookup"><span data-stu-id="773fa-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="773fa-111">Ouvrez le fichier *Models/Movie.cs* et ajoutez une propriété `Rating` :</span><span class="sxs-lookup"><span data-stu-id="773fa-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=12&name=snippet)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

<span data-ttu-id="773fa-112">Comme vous avez ajouté un nouveau champ à la classe `Movie`, vous devez également mettre à jour la liste verte des liaisons pour y inclure cette nouvelle propriété.</span><span class="sxs-lookup"><span data-stu-id="773fa-112">Because you've added a new field to the `Movie` class, you also need to update the binding whitelist so this new property will be included.</span></span> <span data-ttu-id="773fa-113">Dans *MoviesController.cs*, mettez à jour l’attribut `[Bind]` des méthodes d’action `Create` et `Edit` pour y inclure la propriété `Rating` :</span><span class="sxs-lookup"><span data-stu-id="773fa-113">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="773fa-114">Vous devez aussi mettre à jour les modèles de vue pour afficher, créer et modifier la nouvelle propriété `Rating` dans la vue du navigateur.</span><span class="sxs-lookup"><span data-stu-id="773fa-114">You also need to update the view templates in order to display, create, and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="773fa-115">Ouvrez le fichier */Views/Movies/Index.cshtml* et ajoutez un champ `Rating` :</span><span class="sxs-lookup"><span data-stu-id="773fa-115">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="773fa-116">Mettez à jour le fichier */Views/Movies/Create.cshtml* avec un champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="773fa-116">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span>

<span data-ttu-id="773fa-117">L’application ne fonctionne pas tant que la mise à jour de la base de données pour inclure le nouveau champ n’est pas effectuée.</span><span class="sxs-lookup"><span data-stu-id="773fa-117">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="773fa-118">Si vous l’exécutez maintenant, vous recevez l’exception `SqliteException` suivante :</span><span class="sxs-lookup"><span data-stu-id="773fa-118">If you run it now, you'll get the following `SqliteException`:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

<span data-ttu-id="773fa-119">Vous voyez cette erreur car la classe du modèle Movie mise à jour est différente du schéma de la table Movie de la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="773fa-119">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="773fa-120">(Il n’existe pas de colonne `Rating` dans la table de base de données.)</span><span class="sxs-lookup"><span data-stu-id="773fa-120">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="773fa-121">Plusieurs approches sont possibles pour résoudre l’erreur :</span><span class="sxs-lookup"><span data-stu-id="773fa-121">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="773fa-122">Supprimez la base de données et laissez Entity Framework recréer automatiquement la base de données sur la base du nouveau schéma de la classe du modèle.</span><span class="sxs-lookup"><span data-stu-id="773fa-122">Drop the database and have the Entity Framework automatically re-create the database based on the new model class schema.</span></span> <span data-ttu-id="773fa-123">Avec cette approche, vous perdez les données existantes de la base de données : vous ne pouvez donc pas l’appliquer à une base de données en production.</span><span class="sxs-lookup"><span data-stu-id="773fa-123">With this approach, you lose existing data in the database — so you can't do this with a production database!</span></span> <span data-ttu-id="773fa-124">L’utilisation d’un initialiseur pour amorcer automatiquement une base de données avec des données de test est souvent un moyen efficace pour développer une application.</span><span class="sxs-lookup"><span data-stu-id="773fa-124">Using an initializer to automatically seed a database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="773fa-125">Modifiez manuellement le schéma de la base de données existante pour le faire correspondre aux classes du modèle.</span><span class="sxs-lookup"><span data-stu-id="773fa-125">Manually modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="773fa-126">L’avantage de cette approche est que vous conservez vos données.</span><span class="sxs-lookup"><span data-stu-id="773fa-126">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="773fa-127">Vous pouvez apporter cette modification manuellement ou en créant un script de modification de la base de données.</span><span class="sxs-lookup"><span data-stu-id="773fa-127">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="773fa-128">Utilisez les migrations Code First pour mettre à jour le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="773fa-128">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="773fa-129">Pour ce didacticiel, nous supprimons puis nous recréons la base de données quand le schéma change.</span><span class="sxs-lookup"><span data-stu-id="773fa-129">For this tutorial, we'll drop and re-create the database when the schema changes.</span></span> <span data-ttu-id="773fa-130">À partir d’un terminal, exécutez la commande suivante pour supprimer la base de données :</span><span class="sxs-lookup"><span data-stu-id="773fa-130">Run the following command from a terminal to drop the db:</span></span>

`dotnet ef database drop`

<span data-ttu-id="773fa-131">Mettez à jour la classe `SeedData` pour qu’elle fournisse une valeur pour la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="773fa-131">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="773fa-132">Vous pouvez voir un exemple de modification ci-dessous, mais elle doit être appliquée à chaque `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="773fa-132">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="773fa-133">Ajoutez le champ `Rating` aux vues `Edit`, `Details` et `Delete`.</span><span class="sxs-lookup"><span data-stu-id="773fa-133">Add the `Rating` field to the `Edit`, `Details`, and `Delete` view.</span></span>

<span data-ttu-id="773fa-134">Exécutez l’application et vérifiez que vous pouvez créer/modifier/afficher des films avec un champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="773fa-134">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="773fa-135">modèles.</span><span class="sxs-lookup"><span data-stu-id="773fa-135">templates.</span></span>
