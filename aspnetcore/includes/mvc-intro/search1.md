---
ms.openlocfilehash: 24726fba7f431f701b264a988a8de1b67d41d8a2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048716"
---
# <a name="add-search-to-an-aspnet-core-mvc-app"></a>Ajouter une fonction de recherche à une application ASP.NET Core MVC

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Dans cette section, vous ajoutez une fonctionnalité de recherche à la méthode d’action `Index` qui vous permet de rechercher des films par *genre* ou par *nom*.

Mettez à jour la méthode `Index` avec le code suivant :
<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

La première ligne de la méthode d’action `Index` crée une requête [LINQ](/dotnet/standard/using-linq) pour sélectionner les films :

```csharp
var movies = from m in _context.Movie
             select m;
```

La requête est *seulement* définie à ce stade, elle n’a **pas** été exécutée sur la base de données.

Si le paramètre `searchString` contient une chaîne, la requête de films est modifiée de façon à filtrer sur la valeur de la chaîne de recherche :

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

Le code `s => s.Title.Contains()` ci-dessus est une [expression lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Les expressions lambda sont utilisées dans les requêtes [LINQ](/dotnet/standard/using-linq) basées sur une méthode en tant qu’arguments pour les méthodes d’opérateur de requête standard, comme la méthode [Where](/dotnet/api/system.linq.enumerable.where) ou `Contains` (utilisée dans le code ci-dessus). Les requêtes LINQ ne sont pas exécutées quand elles sont définies ou quand elles sont modifiées en appelant une méthode comme `Where`, `Contains` ou `OrderBy`. Au lieu de cela, l’exécution de la requête est différée.  Cela signifie que l’évaluation d’une expression est retardée jusqu’à ce que sa valeur réalisée fasse l’objet d’une itération réelle ou que la méthode `ToListAsync` soit appelée. Pour plus d’informations sur l’exécution différée des requêtes, consultez [Exécution de requête](/dotnet/framework/data/adonet/ef/language-reference/query-execution).

Remarque : La méthode [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) est exécutée sur la base de données, et non pas dans le code C# ci-dessus. Le respect de la casse pour la requête dépend de la base de données et du classement. Sur SQL Server, [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) est mappé à [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), qui ne respecte pas la casse. Dans SQLite, avec le classement par défaut, elle respecte la casse.

Accédez à `/Movies/Index`. Ajoutez une chaîne de requête comme `?searchString=Ghost` à l’URL. Les films filtrés sont affichés.

![Vue Index](~/tutorials/first-mvc-app/search/_static/ghost.png)

Si vous changez la signature de la méthode `Index` pour y inclure un paramètre nommé `id`, le paramètre`id` correspondra à l’espace réservé facultatif `{id}` pour les routes par défaut définies dans *Startup.cs*.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]
