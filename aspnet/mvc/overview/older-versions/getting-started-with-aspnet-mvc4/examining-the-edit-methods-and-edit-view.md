---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: Examen des méthodes de modification et de la vue Edit | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : une version mise à jour de ce didacticiel est disponible ici et utilise ASP.NET MVC 5 et Visual Studio 2013. C’est plus sécurisé, bien plus simple à suivre et à faire une démonstration...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 85ad9a5758d70b5fe4ed792efb80217d7b3e2132
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "86162986"
---
# <a name="examining-the-edit-methods-and-edit-view"></a>Examen des méthodes de modification et de la vue de modification

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) et utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sûr, bien plus simple à suivre et qui illustre davantage de fonctionnalités.

Dans cette section, vous allez examiner les méthodes et les vues d’action générées pour le contrôleur de films. Vous allez ensuite ajouter une page de recherche personnalisée.

Exécutez l’application et accédez au `Movies` contrôleur en ajoutant */movies* à l’URL dans la barre d’adresses de votre navigateur. Maintenez le pointeur de la souris sur un lien **modifier** pour afficher l’URL à laquelle il est lié.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Le lien **Edit** a été généré par la `Html.ActionLink` méthode dans la vue *Views\Movies\Index.cshtml* :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![Html. ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

L' `Html` objet est une application d’assistance exposée à l’aide d’une propriété sur la classe de base [System. Web. Mvc. WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) . La `ActionLink` méthode de l’application auxiliaire facilite la génération dynamique des liens hypertexte HTML qui sont liés aux méthodes d’action sur les contrôleurs. Le premier argument de la `ActionLink` méthode est le texte de lien à afficher (par exemple, `<a>Edit Me</a>` ). Le deuxième argument est le nom de la méthode d’action à appeler. Le dernier argument est un [objet anonyme](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) qui génère les données d’itinéraire (dans le cas présent, l’ID 4).

Le lien généré affiché dans l’image précédente est `http://localhost:xxxxx/Movies/Edit/4` . L’itinéraire par défaut (défini dans l' *application \_ Start\RouteConfig.cs*) prend le modèle d’URL `{controller}/{action}/{id}` . Par conséquent, ASP.NET se traduit par `http://localhost:xxxxx/Movies/Edit/4` une requête à la `Edit` méthode d’action du `Movies` contrôleur avec le paramètre `ID` égal à 4. Examinez le code suivant dans le fichier *app \_ Start\RouteConfig.cs* .

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Vous pouvez également passer des paramètres de méthode d’action à l’aide d’une chaîne de requête. Par exemple, l’URL `http://localhost:xxxxx/Movies/Edit?ID=4` passe également le paramètre `ID` de 4 à la `Edit` méthode d’action du `Movies` contrôleur.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Ouvrez le `Movies` contrôleur. Les deux `Edit` méthodes d’action sont présentées ci-dessous.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

Notez que la deuxième méthode d’action `Edit` est précédée de l’attribut `HttpPost`. Cet attribut spécifie que la surcharge de la `Edit` méthode peut être appelée uniquement pour les requêtes de publication. Vous pouvez appliquer l' `HttpGet` attribut à la première méthode Edit, mais cela n’est pas nécessaire, car il s’agit de la valeur par défaut. (Nous reviendrons sur les méthodes d’action qui sont assignées de manière implicite à l' `HttpGet` attribut en tant que `HttpGet` méthodes.)

La `HttpGet` `Edit` méthode prend le paramètre Movie ID, recherche le film à l’aide de la `Find` méthode Entity Framework et retourne le film sélectionné à la vue Edit. Le paramètre ID spécifie une [valeur par défaut](https://msdn.microsoft.com/library/dd264739.aspx) de zéro si la `Edit` méthode est appelée sans paramètre. Si un film est introuvable, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) est retourné. Quand le système de génération de modèles automatique a créé la vue Edit, il a examiné la classe `Movie` et a créé le code pour restituer les éléments `<label>` et `<input>` de chaque propriété de la classe. L’exemple suivant montre la vue Edit qui a été générée :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

Notez que le modèle de vue contient une `@model MvcMovie.Models.Movie` instruction en haut du fichier, ce qui indique que la vue s’attend à ce que le modèle du modèle de vue soit de type `Movie` .

Le code de génération de modèles automatique utilise plusieurs *méthodes d’assistance* pour simplifier le balisage HTML. L' [`Html.LabelFor`](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) application auxiliaire affiche le nom du champ ( &quot; titre &quot; , &quot; version finale &quot; , &quot; genre &quot; ou &quot; prix &quot; ). L' [`Html.EditorFor`](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) application d’assistance restitue un `<input>` élément HTML. L' [`Html.ValidationMessageFor`](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) application auxiliaire affiche tous les messages de validation associés à cette propriété.

Exécutez l’application et accédez à l’URL */movies* . Cliquez sur un lien **modifier** . Dans le navigateur, affichez la source de la page. Le code HTML de l’élément Form est indiqué ci-dessous.

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

Les `<input>` éléments se trouvent dans un `<form>` élément HTML dont l' `action` attribut a la valeur publication sur l’URL */movies/Edit* . Les données du formulaire sont publiées sur le serveur lorsque l’utilisateur clique sur le bouton **modifier** .

## <a name="processing-the-post-request"></a>Traitement de la requête POST

Le code suivant montre la version `HttpPost` de la méthode d’action `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

Le [Binder de modèle ASP.NET MVC](https://msdn.microsoft.com/magazine/hh781022.aspx) prend les valeurs de formulaire publiées et crée un `Movie` objet qui est passé comme `movie` paramètre. La méthode `ModelState.IsValid` vérifie que les données envoyées dans le formulaire peuvent être utilisées pour changer (modifier ou mettre à jour) un objet `Movie`. Si les données sont valides, les données de film sont enregistrées dans la `Movies` collection de l' `db(MovieDBContext` instance de. Les nouvelles données de film sont enregistrées dans la base de données en appelant la `SaveChanges` méthode de `MovieDBContext` . Une fois les données enregistrées, le code redirige l’utilisateur vers la `Index` méthode d’action de la `MoviesController` classe, qui affiche le de la collection de films, y compris les modifications que vous venez d’apporter.

Si les valeurs publiées ne sont pas valides, elles sont réaffichées dans le formulaire. Les `Html.ValidationMessageFor` applications auxiliaires dans le modèle de vue *Edit. cshtml* s’occupent de l’affichage des messages d’erreur appropriés.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> pour prendre en charge la validation jQuery pour les paramètres régionaux autres que l’anglais qui utilisent une virgule ( &quot; , &quot; ) pour une virgule décimale, vous devez inclure *globalize.js* et votre fichier de *cultures/globalize.cultures.js* spécifique (à partir de [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) et JavaScript à utiliser `Globalize.parseFloat` . Le code suivant montre les modifications apportées au fichier Views\Movies\Edit.cshtml pour travailler avec la &quot; culture fr-fr &quot; :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Le champ décimal peut nécessiter une virgule, et non une virgule décimale. En guise de correctif temporaire, vous pouvez ajouter l’élément Globalization au fichier web.config racine des projets. Le code suivant montre l’élément Globalization dont la culture est définie sur États-Unis anglais.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

Toutes les `HttpGet` méthodes suivent un modèle similaire. Ils obtiennent un objet Movie (ou une liste d’objets, dans le cas de `Index` ) et transmettent le modèle à la vue. La `Create` méthode passe un objet Movie vide à la vue Create. Toutes les méthodes qui créent, modifient, suppriment ou changent d’une quelconque manière des données le font dans la surcharge `HttpPost` de la méthode. La modification des données dans une méthode HTTP obtient est un risque pour la sécurité, comme décrit dans le billet de blog entrée [ASP.net Conseil MVC #46 – ne pas utiliser les liens de suppression, car ils créent des failles de sécurité](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). La modification des données dans une méthode d’extraction viole également les meilleures pratiques HTTP et le modèle architectural [Rest](http://en.wikipedia.org/wiki/Representational_State_Transfer) , qui spécifie que les demandes d’extraction ne doivent pas modifier l’état de votre application. En d’autres termes, une opération GET doit être sûre, ne doit avoir aucun effet secondaire et ne doit pas modifier vos données persistantes.

## <a name="adding-a-search-method-and-search-view"></a>Ajout d’une méthode de recherche et d’un affichage de recherche

Dans cette section, vous allez ajouter une `SearchIndex` méthode d’action qui vous permet de rechercher des films par genre ou par nom. Celui-ci sera disponible à l’aide de l’URL */movies/SearchIndex* . La requête affiche un formulaire HTML qui contient les éléments d’entrée qu’un utilisateur peut entrer pour rechercher un film. Lorsqu’un utilisateur envoie le formulaire, la méthode d’action obtient les valeurs de recherche publiées par l’utilisateur et utilise les valeurs pour effectuer une recherche dans la base de données.

## <a name="displaying-the-searchindex-form"></a>Affichage du formulaire SearchIndex

Commencez par ajouter une `SearchIndex` méthode d’action à la `MoviesController` classe existante. La méthode retourne une vue qui contient un formulaire HTML. Voici le code :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

La première ligne de la `SearchIndex` méthode crée la requête [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) suivante pour sélectionner les films :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

La requête est définie à ce stade, mais elle n’a pas encore été exécutée sur le magasin de données.

Si le `searchString` paramètre contient une chaîne, la requête de films est modifiée pour filtrer sur la valeur de la chaîne de recherche, à l’aide du code suivant :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

Le code `s => s.Title` ci-dessus est une [expression lambda](https://msdn.microsoft.com/library/bb397687.aspx). Les expressions lambda sont utilisées dans les requêtes [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) basées sur une méthode en tant qu’arguments pour les méthodes d’opérateur de requête standard, telles que la méthode [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) utilisée dans le code ci-dessus. Les requêtes LINQ ne sont pas exécutées quand elles sont définies ou quand elles sont modifiées en appelant une méthode telle que `Where` ou `OrderBy` . Au lieu de cela, l’exécution de la requête est différée, ce qui signifie que l’évaluation d’une expression est retardée jusqu’à ce que sa valeur réalisée soit réellement itérée ou que la [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) méthode soit appelée. Dans l' `SearchIndex` exemple, la requête est exécutée dans la vue SearchIndex. Pour plus d’informations sur l’exécution différée des requêtes, consultez [Exécution de requête](https://msdn.microsoft.com/library/bb738633.aspx).

À présent, vous pouvez implémenter la `SearchIndex` vue qui affichera le formulaire pour l’utilisateur. Cliquez avec le bouton droit à l’intérieur de la `SearchIndex` méthode, puis cliquez sur **Ajouter une vue**. Dans la boîte de dialogue **Ajouter une vue** , spécifiez que vous allez passer un `Movie` objet au modèle de vue en tant que classe de modèle. Dans la liste **modèle de structure** , choisissez **liste**, puis cliquez sur **Ajouter**.

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

Lorsque vous cliquez sur le bouton **Ajouter** , le modèle de vue *Views\Movies\SearchIndex.cshtml* est créé. Étant donné que vous avez sélectionné la **liste** dans la liste de modèles de l' **échafaudage** , Visual Studio a généré automatiquement un balisage par défaut dans la vue. La génération de modèles automatique a créé un formulaire HTML. Il a examiné la `Movie` classe et créé du code pour afficher les `<label>` éléments de chaque propriété de la classe. La liste ci-dessous montre la vue Create qui a été générée :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

Exécutez l’application et accédez à */movies/SearchIndex*. Ajoutez une chaîne de requête comme `?searchString=ghost` à l’URL. Les films filtrés sont affichés.

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

Si vous modifiez la signature de la `SearchIndex` méthode pour qu’elle ait un paramètre nommé `id` , le `id` paramètre correspondra à l' `{id}` espace réservé pour les itinéraires par défaut définis dans le fichier *global. asax* .

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.json)]

La méthode d’origine se `SearchIndex` présente comme suit :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

La méthode modifiée se `SearchIndex` présente comme suit :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

Vous pouvez maintenant passer le titre de la recherche en tant que données de routage (un segment de l’URL) et non pas en tant que valeur de chaîne de requête.

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

Cependant, vous ne pouvez pas attendre des utilisateurs qu’ils modifient l’URL à chaque fois qu’ils veulent rechercher un film. Vous allez maintenant ajouter l’interface utilisateur pour filtrer les films. Si vous avez modifié la signature de la `SearchIndex` méthode pour tester la manière de passer le paramètre d’ID lié à l’itinéraire, modifiez-le de façon à ce que votre `SearchIndex` méthode prenne un paramètre de chaîne nommé `searchString` :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

Ouvrez le fichier *Views\Movies\SearchIndex.cshtml* et, juste après `@Html.ActionLink("Create New", "Create")` , ajoutez ce qui suit :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

L’exemple suivant montre une partie du fichier *Views\Movies\SearchIndex.cshtml* avec le balisage de filtrage ajouté.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

L' `Html.BeginForm` application auxiliaire crée une `<form>` balise d’ouverture. L' `Html.BeginForm` application d’assistance fait en sorte que le formulaire publie sur lui-même lorsque l’utilisateur envoie le formulaire en cliquant sur le bouton de **filtre** .

Exécutez l’application et essayez de rechercher un film.

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

Il n’y a aucune `HttpPost` surcharge de la `SearchIndex` méthode. Vous n’en avez pas besoin, car la méthode ne modifie pas l’état de l’application, il suffit de filtrer les données.

Vous pourriez ajouter la méthode `HttpPost SearchIndex` suivante. Dans ce cas, le demandeur d’action correspondrait à la `HttpPost SearchIndex` méthode et la `HttpPost SearchIndex` méthode s’exécuterait comme indiqué dans l’image ci-dessous.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

Cependant, même si vous ajoutez cette version `HttpPost` de la méthode `SearchIndex`, il existe une limitation dans la façon dont tout ceci a été implémenté. Imaginez que vous voulez insérer un signet pour une recherche spécifique, ou que vous voulez envoyer un lien à vos amis sur lequel ils peuvent cliquer pour afficher la même liste filtrée de films. Notez que l’URL de la requête HTTP POSTÉRIEURe est identique à l’URL de la demande d’extraction (localhost : xxxxx/movies/SearchIndex). il n’y a pas d’informations de recherche dans l’URL elle-même. Pour le moment, les informations de la chaîne de recherche sont envoyées au serveur en tant que valeur de champ de formulaire. Cela signifie que vous ne pouvez pas capturer les informations de recherche dans un signet ou les envoyer à des amis dans une URL.

La solution consiste à utiliser une surcharge de `BeginForm` qui spécifie que la requête de publication doit ajouter les informations de recherche à l’URL et qu’elle doit être routée vers la version HttpGet de la `SearchIndex` méthode. Remplacez la méthode sans paramètre existante `BeginForm` par ce qui suit :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

Désormais, lorsque vous envoyez une recherche, l’URL contient une chaîne de requête de recherche. La recherche accède également à la méthode d’action `HttpGet SearchIndex`, même si vous avez une méthode `HttpPost SearchIndex`.

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>Ajout de la recherche par genre

Si vous avez ajouté la `HttpPost` version de la `SearchIndex` méthode, supprimez-la maintenant.

Ensuite, vous allez ajouter une fonctionnalité pour permettre aux utilisateurs de rechercher des films par genre. Remplacez la méthode `SearchIndex` par le code suivant :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

Cette version de la `SearchIndex` méthode prend un paramètre supplémentaire, à savoir `movieGenre` . Les premières lignes de code créent un `List` objet pour stocker les genres de films à partir de la base de données.

Le code suivant est une requête LINQ qui récupère tous les genres dans la base de données.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

Le code utilise la `AddRange` méthode de la `List` collection générique pour ajouter tous les genres distincts à la liste. (Sans le `Distinct` modificateur, les genres en double seraient ajoutés, par exemple, comédie serait ajouté deux fois dans notre exemple). Le code stocke ensuite la liste des genres dans l' `ViewBag` objet.

Le code suivant montre comment vérifier le `movieGenre` paramètre. S’il n’est pas vide, le code limite davantage la requête de films pour limiter les films sélectionnés au genre spécifié.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Ajout de balises à la vue SearchIndex pour prendre en charge la recherche par genre

Ajoutez une `Html.DropDownList` application d’assistance au fichier *Views\Movies\SearchIndex.cshtml* , juste avant l' `TextBox` application auxiliaire. Le balisage terminé est illustré ci-dessous :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

Exécutez l’application et accédez à */movies/SearchIndex*. Essayez d’effectuer une recherche par genre, par nom de film et les deux critères.

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

Dans cette section, vous avez examiné les méthodes et les vues d’action CRUD générées par l’infrastructure. Vous avez créé une méthode et une vue d’action de recherche qui permettent aux utilisateurs de rechercher par titre et par genre de films. Dans la section suivante, vous allez examiner comment ajouter une propriété au `Movie` modèle et comment ajouter un initialiseur qui créera automatiquement une base de données de test.

> [!div class="step-by-step"]
> [Précédent](accessing-your-models-data-from-a-controller.md) 
>  [Suivant](adding-a-new-field-to-the-movie-model-and-table.md)
