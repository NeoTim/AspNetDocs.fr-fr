---
title: Ajouter un nouveau champ à une application ASP.NET Core MVC
author: rick-anderson
description: Découvrez comment utiliser la fonctionnalité Migrations Code First d’Entity Framework pour ajouter un nouveau champ à un modèle et migrer ce changement vers une base de données.
ms.author: riande
ms.custom: mvc
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 7993b36bf9115225e082d2929bb253aba5b18310
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57039316"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="9b7f8-103">Ajouter un nouveau champ à une application ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="9b7f8-103">Add a new field to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="9b7f8-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9b7f8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9b7f8-105">Dans cette section, Migrations [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First est utilisé pour :</span><span class="sxs-lookup"><span data-stu-id="9b7f8-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="9b7f8-106">Ajouter un nouveau champ au modèle.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-106">Add a new field to the model.</span></span>
* <span data-ttu-id="9b7f8-107">Migrer le nouveau champ vers la base de données.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-107">Migrate the new field to the database.</span></span>

<span data-ttu-id="9b7f8-108">Quand EF Code First est utilisé pour créer automatiquement une base de données, Code First :</span><span class="sxs-lookup"><span data-stu-id="9b7f8-108">When EF Code First is used to automatically create a database, Code First:</span></span>

* <span data-ttu-id="9b7f8-109">Ajoute une table à la base de données pour en suivre le schéma.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-109">Adds a table to the database to  track the schema of the database.</span></span>
* <span data-ttu-id="9b7f8-110">Vérifie que la base de données est synchronisée avec les classes de modèle à partir desquelles elle a été générée.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-110">Verify the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="9b7f8-111">S’ils ne sont pas synchronisés, EF lève une exception.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-111">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="9b7f8-112">Cela facilite la recherche de problèmes d’incohérence de code/de bases de données.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-112">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="add-a-rating-property-to-the-movie-model"></a><span data-ttu-id="9b7f8-113">Ajouter une propriété Rating au modèle Movie</span><span class="sxs-lookup"><span data-stu-id="9b7f8-113">Add a Rating Property to the Movie Model</span></span>

<span data-ttu-id="9b7f8-114">Ajouter une propriété `Rating` à *Models/Movie.cs* :</span><span class="sxs-lookup"><span data-stu-id="9b7f8-114">Add a `Rating` property to *Models/Movie.cs*:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="9b7f8-115">Générez l’application (Ctrl+Maj+B).</span><span class="sxs-lookup"><span data-stu-id="9b7f8-115">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="9b7f8-116">Comme vous avez ajouté un nouveau champ à la classe `Movie`, vous devez mettre à jour la liste verte des liaisons pour y inclure cette nouvelle propriété.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-116">Because you've added a new field to the `Movie` class, you need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="9b7f8-117">Dans *MoviesController.cs*, mettez à jour l’attribut `[Bind]` des méthodes d’action `Create` et `Edit` pour y inclure la propriété `Rating` :</span><span class="sxs-lookup"><span data-stu-id="9b7f8-117">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="9b7f8-118">Mettez à jour les modèles de vue pour afficher, créer et modifier la nouvelle propriété `Rating` dans la vue du navigateur.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-118">Update the view templates in order to display, create, and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="9b7f8-119">Ouvrez le fichier */Views/Movies/Index.cshtml* et ajoutez un champ `Rating` :</span><span class="sxs-lookup"><span data-stu-id="9b7f8-119">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGenreRating.cshtml?highlight=16,38&range=24-64)]

<span data-ttu-id="9b7f8-120">Mettez à jour le fichier */Views/Movies/Create.cshtml* avec un champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-120">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span>

<!-- VS -------------------------->
# <a name="visual-studio--visual-studio-for-mactabvisual-studiovisual-studio-mac"></a>[<span data-ttu-id="9b7f8-121">Visual Studio / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="9b7f8-121">Visual Studio / Visual Studio for Mac</span></span>](#tab/visual-studio+visual-studio-mac)

<span data-ttu-id="9b7f8-122">Vous pouvez copier/coller le « groupe de formulaire » précédent et laisser IntelliSense vous aider à mettre à jour les champs.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-122">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="9b7f8-123">IntelliSense fonctionne avec des [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="9b7f8-123">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Le développeur a tapé la lettre R comme valeur d’attribut asp-for dans le deuxième élément étiquette de la vue.](new-field/_static/cr.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9b7f8-127">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9b7f8-127">Visual Studio Code</span></span>](#tab/visual-studio-code)
<!-- This tab intentionally left blank. -->
---  
<!-- End of VS tabs -->

<span data-ttu-id="9b7f8-128">Mettez à jour la classe `SeedData` pour qu’elle fournisse une valeur pour la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-128">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="9b7f8-129">Vous pouvez voir un exemple de modification ci-dessous, mais elle doit être appliquée à chaque `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-129">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="9b7f8-130">L’application ne fonctionne pas tant que la base de données n’est pas mise à jour pour inclure le nouveau champ.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-130">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="9b7f8-131">Si elle est exécutée maintenant, l’erreur `SqlException` est levée :</span><span class="sxs-lookup"><span data-stu-id="9b7f8-131">If it's run now, the following `SqlException` is thrown:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="9b7f8-132">Cette erreur survient car la classe du modèle Movie mise à jour est différente du schéma de la table Movie de la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-132">This error occurs because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="9b7f8-133">(Il n’existe pas de colonne `Rating` dans la table de base de données.)</span><span class="sxs-lookup"><span data-stu-id="9b7f8-133">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="9b7f8-134">Plusieurs approches sont possibles pour résoudre l’erreur :</span><span class="sxs-lookup"><span data-stu-id="9b7f8-134">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="9b7f8-135">Laissez Entity Framework supprimer et recréer automatiquement la base de données sur la base du nouveau schéma de classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-135">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="9b7f8-136">Cette approche est très utile au début du cycle de développement quand vous effectuez un développement actif sur une base de données de test. Elle permet de faire évoluer rapidement le schéma de modèle et de base de données ensemble.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-136">This approach is very convenient early in the development cycle when you're doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="9b7f8-137">L’inconvénient, cependant, est que vous perdez les données existantes dans la base de données. Par conséquent, n’utilisez pas cette approche sur une base de données de production.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-137">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="9b7f8-138">L’utilisation d’un initialiseur pour amorcer automatiquement une base de données avec des données de test est souvent un moyen efficace pour développer une application.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-138">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="9b7f8-139">Il s’agit d’une bonne approche pour le développement initial et lors de l’utilisation de SQLite.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-139">This is a good approach for early development and when using SQLite.</span></span>

2. <span data-ttu-id="9b7f8-140">Modifier explicitement le schéma de la base de données existante pour le faire correspondre aux classes du modèle.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-140">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="9b7f8-141">L’avantage de cette approche est que vous conservez vos données.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-141">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="9b7f8-142">Vous pouvez apporter cette modification manuellement ou en créant un script de modification de la base de données.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-142">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="9b7f8-143">Utilisez les migrations Code First pour mettre à jour le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-143">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="9b7f8-144">Pour ce didacticiel, les migrations Code First sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-144">For this tutorial, Code First Migrations is used.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9b7f8-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b7f8-145">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9b7f8-146">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet > Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-146">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![Menu Console du Gestionnaire de package](adding-model/_static/pmc.png)

<span data-ttu-id="9b7f8-148">Dans la console du Gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="9b7f8-148">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="9b7f8-149">La commande `Add-Migration` indique au framework de migration d’examiner le modèle `Movie` actuel avec le schéma de la base de données `Movie` actuel et de créer le code nécessaire pour migrer la base de données vers le nouveau modèle.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-149">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="9b7f8-150">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="9b7f8-150">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="9b7f8-151">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="9b7f8-151">Run the following command:</span></span>

```cli
dotnet ef migrations add Rating
dotnet ef database update
```

---  
<!-- End of VS tabs -->

<span data-ttu-id="9b7f8-152">Le nom « Rating » est arbitraire et est utilisé pour nommer le fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-152">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="9b7f8-153">Il est utile d’utiliser un nom explicite pour le fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-153">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="9b7f8-154">Si tous les enregistrements de la base de données sont supprimés, la méthode d’initialisation amorce la base de données et inclut le champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-154">If all the records in the DB are deleted, the initialize method will seed the DB and include the `Rating` field.</span></span>

<span data-ttu-id="9b7f8-155">Exécutez l’application et vérifiez que vous pouvez créer/modifier/afficher des films avec un champ `Rating`.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-155">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="9b7f8-156">Vous devez ajouter le champ `Rating` aux modèles de vue `Edit`, `Details` et `Delete`.</span><span class="sxs-lookup"><span data-stu-id="9b7f8-156">You should add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9b7f8-157">[Précédent](search.md)
> [Suivant](validation.md)</span><span class="sxs-lookup"><span data-stu-id="9b7f8-157">[Previous](search.md)
[Next](validation.md)</span></span>  
