---
uid: mvc/overview/getting-started/introduction/adding-search
title: Rechercher | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/17/2019
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: be4e4d13e574b0fcb77d2d0fb8c6f58041b1ece2
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89044856"
---
# <a name="search"></a><span data-ttu-id="190e2-102">Recherche</span><span class="sxs-lookup"><span data-stu-id="190e2-102">Search</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="190e2-103">Ajout d’une méthode de recherche et d’un affichage de recherche</span><span class="sxs-lookup"><span data-stu-id="190e2-103">Adding a Search Method and Search View</span></span>

<span data-ttu-id="190e2-104">Dans cette section, vous allez ajouter la fonctionnalité de recherche à la `Index` méthode d’action qui vous permet de rechercher des films par genre ou par nom.</span><span class="sxs-lookup"><span data-stu-id="190e2-104">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="190e2-105">Prérequis</span><span class="sxs-lookup"><span data-stu-id="190e2-105">Prerequisites</span></span>

<span data-ttu-id="190e2-106">Pour faire correspondre les captures d’écran de cette section, vous devez exécuter l’application (F5) et ajouter les films suivants à la base de données.</span><span class="sxs-lookup"><span data-stu-id="190e2-106">To match this section's screenshots, you need to run the application (F5) and add the following movies to the database.</span></span>

| <span data-ttu-id="190e2-107">Intitulé</span><span class="sxs-lookup"><span data-stu-id="190e2-107">Title</span></span> | <span data-ttu-id="190e2-108">Date de version</span><span class="sxs-lookup"><span data-stu-id="190e2-108">Release Date</span></span> | <span data-ttu-id="190e2-109">Genre</span><span class="sxs-lookup"><span data-stu-id="190e2-109">Genre</span></span> | <span data-ttu-id="190e2-110">Prix</span><span class="sxs-lookup"><span data-stu-id="190e2-110">Price</span></span> |
| ----- | ------------ | ----- | ----- |
| <span data-ttu-id="190e2-111">Ghostbusters</span><span class="sxs-lookup"><span data-stu-id="190e2-111">Ghostbusters</span></span> | <span data-ttu-id="190e2-112">6/8/1984</span><span class="sxs-lookup"><span data-stu-id="190e2-112">6/8/1984</span></span> | <span data-ttu-id="190e2-113">Comédie</span><span class="sxs-lookup"><span data-stu-id="190e2-113">Comedy</span></span> | <span data-ttu-id="190e2-114">6,99</span><span class="sxs-lookup"><span data-stu-id="190e2-114">6.99</span></span> |
| <span data-ttu-id="190e2-115">Ghostbusters II</span><span class="sxs-lookup"><span data-stu-id="190e2-115">Ghostbusters II</span></span> | <span data-ttu-id="190e2-116">6/16/1989</span><span class="sxs-lookup"><span data-stu-id="190e2-116">6/16/1989</span></span> | <span data-ttu-id="190e2-117">Comédie</span><span class="sxs-lookup"><span data-stu-id="190e2-117">Comedy</span></span> | <span data-ttu-id="190e2-118">6,99</span><span class="sxs-lookup"><span data-stu-id="190e2-118">6.99</span></span> |
| <span data-ttu-id="190e2-119">Planète du singes</span><span class="sxs-lookup"><span data-stu-id="190e2-119">Planet of the Apes</span></span> | <span data-ttu-id="190e2-120">3/27/1986</span><span class="sxs-lookup"><span data-stu-id="190e2-120">3/27/1986</span></span> | <span data-ttu-id="190e2-121">Action</span><span class="sxs-lookup"><span data-stu-id="190e2-121">Action</span></span> | <span data-ttu-id="190e2-122">5,99</span><span class="sxs-lookup"><span data-stu-id="190e2-122">5.99</span></span> |

## <a name="updating-the-index-form"></a><span data-ttu-id="190e2-123">Mise à jour du formulaire d’index</span><span class="sxs-lookup"><span data-stu-id="190e2-123">Updating the Index Form</span></span>

<span data-ttu-id="190e2-124">Commencez par mettre à jour la `Index` méthode d’action à la `MoviesController` classe existante.</span><span class="sxs-lookup"><span data-stu-id="190e2-124">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="190e2-125">Voici le code :</span><span class="sxs-lookup"><span data-stu-id="190e2-125">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="190e2-126">La première ligne de la `Index` méthode crée la requête [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) suivante pour sélectionner les films :</span><span class="sxs-lookup"><span data-stu-id="190e2-126">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="190e2-127">La requête est définie à ce stade, mais elle n’a pas encore été exécutée sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="190e2-127">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="190e2-128">Si le `searchString` paramètre contient une chaîne, la requête de films est modifiée pour filtrer sur la valeur de la chaîne de recherche, à l’aide du code suivant :</span><span class="sxs-lookup"><span data-stu-id="190e2-128">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="190e2-129">Le code `s => s.Title` ci-dessus est une [expression lambda](https://msdn.microsoft.com/library/bb397687.aspx).</span><span class="sxs-lookup"><span data-stu-id="190e2-129">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/library/bb397687.aspx).</span></span> <span data-ttu-id="190e2-130">Les expressions lambda sont utilisées dans les requêtes [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) basées sur une méthode en tant qu’arguments pour les méthodes d’opérateur de requête standard, telles que la méthode [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) utilisée dans le code ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="190e2-130">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="190e2-131">Les requêtes LINQ ne sont pas exécutées quand elles sont définies ou quand elles sont modifiées en appelant une méthode telle que `Where` ou `OrderBy` .</span><span class="sxs-lookup"><span data-stu-id="190e2-131">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="190e2-132">Au lieu de cela, l’exécution de la requête est différée, ce qui signifie que l’évaluation d’une expression est retardée jusqu’à ce que sa valeur réalisée soit réellement itérée ou que la [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) méthode soit appelée.</span><span class="sxs-lookup"><span data-stu-id="190e2-132">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="190e2-133">Dans l' `Search` exemple, la requête est exécutée dans la vue *index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="190e2-133">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="190e2-134">Pour plus d’informations sur l’exécution différée des requêtes, consultez [Exécution de requête](https://msdn.microsoft.com/library/bb738633.aspx).</span><span class="sxs-lookup"><span data-stu-id="190e2-134">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="190e2-135">La méthode [Contains](https://msdn.microsoft.com/library/bb155125.aspx) est exécutée sur la base de données, et non pas dans le code c# ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="190e2-135">The [Contains](https://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="190e2-136">Sur la base de données, [contient](https://msdn.microsoft.com/library/bb155125.aspx) des mappages à [SQL comme](https://msdn.microsoft.com/library/ms179859.aspx), ce qui ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="190e2-136">On the database, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="190e2-137">À présent, vous pouvez mettre à jour la `Index` vue qui affichera le formulaire pour l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="190e2-137">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="190e2-138">Exécutez l’application et accédez à */movies/index*.</span><span class="sxs-lookup"><span data-stu-id="190e2-138">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="190e2-139">Ajoutez une chaîne de requête comme `?searchString=ghost` à l’URL.</span><span class="sxs-lookup"><span data-stu-id="190e2-139">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="190e2-140">Les films filtrés sont affichés.</span><span class="sxs-lookup"><span data-stu-id="190e2-140">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="190e2-142">Si vous modifiez la signature de la `Index` méthode pour qu’elle ait un paramètre nommé `id` , le `id` paramètre correspondra à l' `{id}` espace réservé pour les itinéraires par défaut définis dans le fichier *app \_ Start\RouteConfig.cs* .</span><span class="sxs-lookup"><span data-stu-id="190e2-142">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.txt)]

<span data-ttu-id="190e2-143">La méthode d’origine se `Index` présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="190e2-143">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="190e2-144">La méthode modifiée se `Index` présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="190e2-144">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="190e2-145">Vous pouvez maintenant passer le titre de la recherche en tant que données de routage (un segment de l’URL) et non pas en tant que valeur de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="190e2-145">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="190e2-146">Cependant, vous ne pouvez pas attendre des utilisateurs qu’ils modifient l’URL à chaque fois qu’ils veulent rechercher un film.</span><span class="sxs-lookup"><span data-stu-id="190e2-146">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="190e2-147">Vous allez donc maintenant ajouter une interface utilisateur pour les aider à filtrer les films.</span><span class="sxs-lookup"><span data-stu-id="190e2-147">So now you'll add UI to help them filter movies.</span></span> <span data-ttu-id="190e2-148">Si vous avez modifié la signature de la `Index` méthode pour tester la manière de passer le paramètre d’ID lié à l’itinéraire, modifiez-le de façon à ce que votre `Index` méthode prenne un paramètre de chaîne nommé `searchString` :</span><span class="sxs-lookup"><span data-stu-id="190e2-148">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="190e2-149">Ouvrez le fichier *Views\Movies\Index.cshtml* et juste après `@Html.ActionLink("Create New", "Create")` , ajoutez le balisage de formulaire en surbrillance ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="190e2-149">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="190e2-150">L' `Html.BeginForm` application auxiliaire crée une `<form>` balise d’ouverture.</span><span class="sxs-lookup"><span data-stu-id="190e2-150">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="190e2-151">L' `Html.BeginForm` application d’assistance fait en sorte que le formulaire publie sur lui-même lorsque l’utilisateur envoie le formulaire en cliquant sur le bouton de **filtre** .</span><span class="sxs-lookup"><span data-stu-id="190e2-151">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="190e2-152">Visual Studio 2013 a une amélioration intéressante lors de l’affichage et de la modification des fichiers de vue.</span><span class="sxs-lookup"><span data-stu-id="190e2-152">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="190e2-153">Quand vous exécutez l’application avec un fichier de vue ouvert, Visual Studio 2013 appelle la méthode d’action de contrôleur appropriée pour afficher la vue.</span><span class="sxs-lookup"><span data-stu-id="190e2-153">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="190e2-154">Avec la vue d’index ouverte dans Visual Studio (comme indiqué dans l’image ci-dessus), appuyez sur CTR F5 ou F5 pour exécuter l’application, puis essayez de rechercher un film.</span><span class="sxs-lookup"><span data-stu-id="190e2-154">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="190e2-155">Il n’y a aucune `HttpPost` surcharge de la `Index` méthode.</span><span class="sxs-lookup"><span data-stu-id="190e2-155">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="190e2-156">Vous n’en avez pas besoin, car la méthode ne modifie pas l’état de l’application, il suffit de filtrer les données.</span><span class="sxs-lookup"><span data-stu-id="190e2-156">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="190e2-157">Vous pourriez ajouter la méthode `HttpPost Index` suivante.</span><span class="sxs-lookup"><span data-stu-id="190e2-157">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="190e2-158">Dans ce cas, le demandeur d’action correspondrait à la `HttpPost Index` méthode et la `HttpPost Index` méthode s’exécuterait comme indiqué dans l’image ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="190e2-158">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="190e2-160">Cependant, même si vous ajoutez cette version `HttpPost` de la méthode `Index`, il existe une limitation dans la façon dont tout ceci a été implémenté.</span><span class="sxs-lookup"><span data-stu-id="190e2-160">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="190e2-161">Imaginez que vous voulez insérer un signet pour une recherche spécifique, ou que vous voulez envoyer un lien à vos amis sur lequel ils peuvent cliquer pour afficher la même liste filtrée de films.</span><span class="sxs-lookup"><span data-stu-id="190e2-161">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="190e2-162">Notez que l’URL de la requête HTTP POSTÉRIEURe est identique à l’URL de la demande d’extraction (localhost : xxxxx/movies/index)--il n’y a pas d’informations de recherche dans l’URL proprement dite.</span><span class="sxs-lookup"><span data-stu-id="190e2-162">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="190e2-163">Pour le moment, les informations de la chaîne de recherche sont envoyées au serveur en tant que valeur de champ de formulaire.</span><span class="sxs-lookup"><span data-stu-id="190e2-163">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="190e2-164">Cela signifie que vous ne pouvez pas capturer les informations de recherche dans un signet ou les envoyer à des amis dans une URL.</span><span class="sxs-lookup"><span data-stu-id="190e2-164">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="190e2-165">La solution consiste à utiliser une surcharge de `BeginForm` qui spécifie que la requête de publication doit ajouter les informations de recherche à l’URL et qu’elle doit être routée vers la `HttpGet` version de la `Index` méthode.</span><span class="sxs-lookup"><span data-stu-id="190e2-165">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="190e2-166">Remplacez la méthode sans paramètre existante `BeginForm` par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="190e2-166">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="190e2-168">Désormais, lorsque vous envoyez une recherche, l’URL contient une chaîne de requête de recherche.</span><span class="sxs-lookup"><span data-stu-id="190e2-168">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="190e2-169">La recherche accède également à la méthode d’action `HttpGet Index`, même si vous avez une méthode `HttpPost Index`.</span><span class="sxs-lookup"><span data-stu-id="190e2-169">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="190e2-171">Ajout de la recherche par genre</span><span class="sxs-lookup"><span data-stu-id="190e2-171">Adding Search by Genre</span></span>

<span data-ttu-id="190e2-172">Si vous avez ajouté la `HttpPost` version de la `Index` méthode, supprimez-la maintenant.</span><span class="sxs-lookup"><span data-stu-id="190e2-172">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="190e2-173">Ensuite, vous allez ajouter une fonctionnalité pour permettre aux utilisateurs de rechercher des films par genre.</span><span class="sxs-lookup"><span data-stu-id="190e2-173">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="190e2-174">Remplacez la méthode `Index` par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="190e2-174">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="190e2-175">Cette version de la `Index` méthode prend un paramètre supplémentaire, à savoir `movieGenre` .</span><span class="sxs-lookup"><span data-stu-id="190e2-175">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="190e2-176">Les premières lignes de code créent un `List` objet pour stocker les genres de films à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="190e2-176">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="190e2-177">Le code suivant est une requête LINQ qui récupère tous les genres dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="190e2-177">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="190e2-178">Le code utilise la `AddRange` méthode de la `List` collection générique pour ajouter tous les genres distincts à la liste.</span><span class="sxs-lookup"><span data-stu-id="190e2-178">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="190e2-179">(Sans le `Distinct` modificateur, les genres en double seraient ajoutés, par exemple, comédie serait ajouté deux fois dans notre exemple).</span><span class="sxs-lookup"><span data-stu-id="190e2-179">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="190e2-180">Le code stocke ensuite la liste des genres dans l' `ViewBag.MovieGenre` objet.</span><span class="sxs-lookup"><span data-stu-id="190e2-180">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="190e2-181">Le stockage des données de catégorie (telles que les genres de films) en tant qu’objet [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) dans un `ViewBag` , l’accès aux données de catégorie dans une zone de liste déroulante est une approche typique pour les applications MVC.</span><span class="sxs-lookup"><span data-stu-id="190e2-181">Storing category data (such a movie genres) as a [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="190e2-182">Le code suivant montre comment vérifier le `movieGenre` paramètre.</span><span class="sxs-lookup"><span data-stu-id="190e2-182">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="190e2-183">S’il n’est pas vide, le code limite davantage la requête de films pour limiter les films sélectionnés au genre spécifié.</span><span class="sxs-lookup"><span data-stu-id="190e2-183">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="190e2-184">Comme indiqué précédemment, la requête n’est pas exécutée sur la base de données tant que la liste de films n’a pas été itérée (ce qui se produit dans la vue, après le retour de la `Index` méthode d’action).</span><span class="sxs-lookup"><span data-stu-id="190e2-184">As stated previously, the query is not run on the database until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="190e2-185">Ajout de balises à la vue d’index pour prendre en charge la recherche par genre</span><span class="sxs-lookup"><span data-stu-id="190e2-185">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="190e2-186">Ajoutez une `Html.DropDownList` application d’assistance au fichier *Views\Movies\Index.cshtml* , juste avant l' `TextBox` application auxiliaire.</span><span class="sxs-lookup"><span data-stu-id="190e2-186">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="190e2-187">Le balisage terminé est illustré ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="190e2-187">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="190e2-188">Dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="190e2-188">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="190e2-189">Le paramètre « MovieGenre » fournit la clé pour que l' `DropDownList` application d’assistance trouve un `IEnumerable<SelectListItem>` dans le `ViewBag` .</span><span class="sxs-lookup"><span data-stu-id="190e2-189">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="190e2-190">Le `ViewBag` a été rempli dans la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="190e2-190">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="190e2-191">Le paramètre « All » fournit une étiquette d’option.</span><span class="sxs-lookup"><span data-stu-id="190e2-191">The parameter "All" provides an option label.</span></span> <span data-ttu-id="190e2-192">Si vous examinez ce choix dans votre navigateur, vous verrez que son attribut « value » est vide.</span><span class="sxs-lookup"><span data-stu-id="190e2-192">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="190e2-193">Étant donné que notre contrôleur filtre uniquement `if` la chaîne n’est pas `null` ou vide, l’envoi d’une valeur vide pour `movieGenre` affiche tous les genres.</span><span class="sxs-lookup"><span data-stu-id="190e2-193">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="190e2-194">Vous pouvez également définir une option à sélectionner par défaut.</span><span class="sxs-lookup"><span data-stu-id="190e2-194">You can also set an option to be selected by default.</span></span> <span data-ttu-id="190e2-195">Si vous souhaitez utiliser « comédie » comme option par défaut, vous devez modifier le code du contrôleur de la manière suivante :</span><span class="sxs-lookup"><span data-stu-id="190e2-195">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="190e2-196">Exécutez l’application et accédez à */movies/index*.</span><span class="sxs-lookup"><span data-stu-id="190e2-196">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="190e2-197">Essayez d’effectuer une recherche par genre, par nom de film et les deux critères.</span><span class="sxs-lookup"><span data-stu-id="190e2-197">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="190e2-198">Dans cette section, vous avez créé une méthode et une vue d’action de recherche qui permettent aux utilisateurs de rechercher par titre et par genre de films.</span><span class="sxs-lookup"><span data-stu-id="190e2-198">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="190e2-199">Dans la section suivante, vous allez examiner comment ajouter une propriété au `Movie` modèle et comment ajouter un initialiseur qui créera automatiquement une base de données de test.</span><span class="sxs-lookup"><span data-stu-id="190e2-199">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="190e2-200">[Précédent](examining-the-edit-methods-and-edit-view.md) 
>  [Suivant](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="190e2-200">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
