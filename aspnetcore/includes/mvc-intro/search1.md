---
ms.openlocfilehash: 24726fba7f431f701b264a988a8de1b67d41d8a2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048716"
---
# <a name="add-search-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="5d032-101">Ajouter une fonction de recherche à une application ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="5d032-101">Add search to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="5d032-102">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5d032-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5d032-103">Dans cette section, vous ajoutez une fonctionnalité de recherche à la méthode d’action `Index` qui vous permet de rechercher des films par *genre* ou par *nom*.</span><span class="sxs-lookup"><span data-stu-id="5d032-103">In this section you add search capability to the `Index` action method that lets you search movies by *genre* or *name*.</span></span>

<span data-ttu-id="5d032-104">Mettez à jour la méthode `Index` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="5d032-104">Update the `Index` method with the following code:</span></span>
<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

<span data-ttu-id="5d032-105">La première ligne de la méthode d’action `Index` crée une requête [LINQ](/dotnet/standard/using-linq) pour sélectionner les films :</span><span class="sxs-lookup"><span data-stu-id="5d032-105">The first line of the `Index` action method creates a [LINQ](/dotnet/standard/using-linq) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="5d032-106">La requête est *seulement* définie à ce stade, elle n’a **pas** été exécutée sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="5d032-106">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="5d032-107">Si le paramètre `searchString` contient une chaîne, la requête de films est modifiée de façon à filtrer sur la valeur de la chaîne de recherche :</span><span class="sxs-lookup"><span data-stu-id="5d032-107">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

<span data-ttu-id="5d032-108">Le code `s => s.Title.Contains()` ci-dessus est une [expression lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="5d032-108">The `s => s.Title.Contains()` code above is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="5d032-109">Les expressions lambda sont utilisées dans les requêtes [LINQ](/dotnet/standard/using-linq) basées sur une méthode en tant qu’arguments pour les méthodes d’opérateur de requête standard, comme la méthode [Where](/dotnet/api/system.linq.enumerable.where) ou `Contains` (utilisée dans le code ci-dessus).</span><span class="sxs-lookup"><span data-stu-id="5d032-109">Lambdas are used in method-based [LINQ](/dotnet/standard/using-linq) queries as arguments to standard query operator methods such as the [Where](/dotnet/api/system.linq.enumerable.where) method or `Contains` (used in the code above).</span></span> <span data-ttu-id="5d032-110">Les requêtes LINQ ne sont pas exécutées quand elles sont définies ou quand elles sont modifiées en appelant une méthode comme `Where`, `Contains` ou `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="5d032-110">LINQ queries are not executed when they're defined or when they're modified by calling a method such as `Where`, `Contains`  or `OrderBy`.</span></span> <span data-ttu-id="5d032-111">Au lieu de cela, l’exécution de la requête est différée.</span><span class="sxs-lookup"><span data-stu-id="5d032-111">Rather, query execution is deferred.</span></span>  <span data-ttu-id="5d032-112">Cela signifie que l’évaluation d’une expression est retardée jusqu’à ce que sa valeur réalisée fasse l’objet d’une itération réelle ou que la méthode `ToListAsync` soit appelée.</span><span class="sxs-lookup"><span data-stu-id="5d032-112">That means that the evaluation of an expression is delayed until its realized value is actually iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="5d032-113">Pour plus d’informations sur l’exécution différée des requêtes, consultez [Exécution de requête](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="5d032-113">For more information about deferred query execution, see [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span></span>

<span data-ttu-id="5d032-114">Remarque : La méthode [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) est exécutée sur la base de données, et non pas dans le code C# ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="5d032-114">Note: The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the c# code shown above.</span></span> <span data-ttu-id="5d032-115">Le respect de la casse pour la requête dépend de la base de données et du classement.</span><span class="sxs-lookup"><span data-stu-id="5d032-115">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="5d032-116">Sur SQL Server, [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) est mappé à [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), qui ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="5d032-116">On SQL Server, [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="5d032-117">Dans SQLite, avec le classement par défaut, elle respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="5d032-117">In SQLlite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="5d032-118">Accédez à `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="5d032-118">Navigate to `/Movies/Index`.</span></span> <span data-ttu-id="5d032-119">Ajoutez une chaîne de requête comme `?searchString=Ghost` à l’URL.</span><span class="sxs-lookup"><span data-stu-id="5d032-119">Append a query string such as `?searchString=Ghost` to the URL.</span></span> <span data-ttu-id="5d032-120">Les films filtrés sont affichés.</span><span class="sxs-lookup"><span data-stu-id="5d032-120">The filtered movies are displayed.</span></span>

![Vue Index](~/tutorials/first-mvc-app/search/_static/ghost.png)

<span data-ttu-id="5d032-122">Si vous changez la signature de la méthode `Index` pour y inclure un paramètre nommé `id`, le paramètre`id` correspondra à l’espace réservé facultatif `{id}` pour les routes par défaut définies dans *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="5d032-122">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the optional `{id}` placeholder for the default routes set in *Startup.cs*.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]
