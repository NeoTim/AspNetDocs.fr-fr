---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: Examen des méthodes de modification et de la vue Edit | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : Une version mise à jour de ce didacticiel est disponible ici qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et de démonstration...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 1223e110ce98c5b511312de42bc2992045a4a40b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062136"
---
<a name="examining-the-edit-methods-and-edit-view"></a>Examen des méthodes de modification et de la vue de modification
====================
par [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et illustre plusieurs fonctionnalités.


Dans cette section, vous allez examiner les méthodes d’action généré et les vues pour le contrôleur de film. Ensuite, vous allez ajouter une page de recherche personnalisée.

Exécutez l’application et accédez à la `Movies` contrôleur en ajoutant */Movies* à l’URL dans la barre d’adresses de votre navigateur. Maintenez le pointeur de la souris sur un **modifier** lien pour afficher l’URL auxquelles il est lié.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Le **modifier** lien a été généré par le `Html.ActionLink` méthode dans le *Views\Movies\Index.cshtml* vue :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

Le `Html` objet est une application d’assistance qui est exposée à l’aide d’une propriété sur le [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) classe de base. Le `ActionLink` méthode de l’application d’assistance facilite l’utilisation générer dynamiquement le code HTML des liens hypertexte aux méthodes d’action sur les contrôleurs. Le premier argument de la `ActionLink` méthode est le texte de lien à afficher (par exemple, `<a>Edit Me</a>`). Le deuxième argument est le nom de la méthode d’action à appeler. L’argument final est un [objet anonyme](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) qui génère les données d’itinéraire (dans ce cas, l’ID de 4).

Le lien généré indiqué dans l’image précédente est `http://localhost:xxxxx/Movies/Edit/4`. L’itinéraire par défaut (établie dans *application\_start\routeconfig*) prend le modèle d’URL `{controller}/{action}/{id}`. Par conséquent, ASP.NET se traduit par `http://localhost:xxxxx/Movies/Edit/4` en une requête à la `Edit` méthode d’action de la `Movies` contrôleur avec le paramètre `ID` égal à 4. Examinez le code suivant à partir de la *application\_start\routeconfig* fichier.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Vous pouvez également transmettre des paramètres de méthode d’action à l’aide d’une chaîne de requête. Par exemple, l’URL `http://localhost:xxxxx/Movies/Edit?ID=4` passe également le paramètre `ID` de 4 processeurs à la `Edit` méthode d’action de la `Movies` contrôleur.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Ouvrez la `Movies` contrôleur. Les deux `Edit` méthodes d’action sont présentées ci-dessous.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

Notez que la deuxième méthode d’action `Edit` est précédée de l’attribut `HttpPost`. Cet attribut spécifie cette surcharge de la `Edit` méthode peut être appelée uniquement pour les requêtes POST. Vous pouvez appliquer la `HttpGet` modifier la méthode de l’attribut à la première, mais qui n’est pas nécessaire car il s’agit de la valeur par défaut. (Nous allons nous référer aux méthodes d’action attribués de manière implicite le `HttpGet` sous la forme `HttpGet` méthodes.)

Le `HttpGet` `Edit` méthode prend le paramètre ID de film, recherche le film à l’aide d’Entity Framework `Find` (méthode) et retourne le film sélectionné à la vue Edit. Le paramètre ID spécifie un [valeur par défaut](https://msdn.microsoft.com/library/dd264739.aspx) de zéro si le `Edit` méthode est appelée sans aucun paramètre. Si un film est introuvable, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) est retourné. Quand le système de génération de modèles automatique a créé la vue Edit, il a examiné la classe `Movie` et a créé le code pour restituer les éléments `<label>` et `<input>` de chaque propriété de la classe. L’exemple suivant montre la vue Edit qui a été générée :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

Notez comment le modèle de vue a un `@model MvcMovie.Models.Movie` instruction en haut du fichier : Spécifie que la vue prévoit que le modèle pour le modèle de vue soit de type `Movie`.

Le code généré automatiquement utilise plusieurs *méthodes d’assistance* pour rationaliser le balisage HTML. Le [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) helper affiche le nom du champ (&quot;titre&quot;, &quot;ReleaseDate&quot;, &quot;Genre&quot;, ou &quot;prix &quot;). Le [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) helper restitue un élément HTML `<input>` élément. Le [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) helper affiche les messages de validation associés à cette propriété.

Exécutez l’application et accédez à la */Movies* URL. Cliquez sur un lien **Edit**. Dans le navigateur, affichez la source de la page. Vous trouverez ci-dessous le code HTML pour l’élément de formulaire.

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

Le `<input>` sont des éléments dans le code HTML `<form>` élément dont `action` attribut a la valeur à valider dans le */Movies/Edit* URL. Les données du formulaire seront publiées sur le serveur lors de la **modifier** bouton.

## <a name="processing-the-post-request"></a>Traitement de la requête POST

Le code suivant montre la version `HttpPost` de la méthode d’action `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

Le [binder de modèle ASP.NET MVC](https://msdn.microsoft.com/magazine/hh781022.aspx) prend les valeurs de formulaire publiées et crée un `Movie` objet qui est passé en tant que le `movie` paramètre. La méthode `ModelState.IsValid` vérifie que les données envoyées dans le formulaire peuvent être utilisées pour changer (modifier ou mettre à jour) un objet `Movie`. Si les données sont valides, les données de film sont enregistrées dans le `Movies` collection de la `db(MovieDBContext` instance). Les nouvelles données de film sont enregistrées dans la base de données en appelant le `SaveChanges` méthode de `MovieDBContext`. Après avoir enregistré les données, le code redirige l’utilisateur vers le `Index` méthode d’action de la `MoviesController` classe, qui affiche le de collection de films, y compris les modifications apportées uniquement.

Si les valeurs publiées ne sont pas valides, ils réapparaissent dans le formulaire. Le `Html.ValidationMessageFor` helpers dans les *Edit.cshtml* vue modèle de prendre en charge d’affichage des messages d’erreur appropriés.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> pour prendre en charge la validation jQuery pour les paramètres régionaux non anglais qui utilisent une virgule (&quot;,&quot;) pour une décimale, vous devez inclure *globalize.js* et votre propre *cultures/globalize.cultures.js* fichier (à partir de [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) et JavaScript pour utiliser `Globalize.parseFloat`. Le code suivant montre les modifications au fichier Views\Movies\Edit.cshtml pour travailler avec le &quot;fr-FR&quot; culture :


[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Le champ décimal peut nécessiter une virgule, pas une virgule décimale. Comme solution temporaire, vous pouvez ajouter l’élément de la globalisation au fichier web.config racine projets. Le code suivant montre l’élément de globalisation avec la culture définie pour l’anglais des États-Unis.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

Tous les `HttpGet` méthodes suivent un modèle similaire. Ils obtiennent un objet de film (ou une liste d’objets, dans le cas de `Index`) et passer le modèle à la vue. Le `Create` méthode passe un objet de film vide à la vue de créer. Toutes les méthodes qui créent, modifient, suppriment ou changent d’une quelconque manière des données le font dans la surcharge `HttpPost` de la méthode. Modification des données dans une méthode HTTP GET est un risque de sécurité, comme décrit dans le billet de blog [ASP.NET MVC Conseil #46 : n’utilisez pas supprimer les liens, car ils créent des failles de sécurité](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modification des données dans une méthode GET enfreint également les bonnes pratiques HTTP et l’architecture [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) de modèle, qui spécifie que les requêtes GET ne doivent pas changer l’état de votre application. En d’autres termes, une opération GET doit être sûre, ne doit avoir aucun effet secondaire et ne doit pas modifier vos données persistantes.

## <a name="adding-a-search-method-and-search-view"></a>Ajout d’une méthode de recherche et de la vue de la recherche

Dans cette section, vous ajouterez un `SearchIndex` méthode d’action qui vous permet de rechercher des films par genre ou le nom. Ce sera disponible en utilisant le */Movies/SearchIndex* URL. La requête affiche un formulaire HTML qui contient les éléments d’entrée qu’un utilisateur peut entrer afin de rechercher un film. Lorsqu’un utilisateur soumet le formulaire, la méthode d’action sera obtenir les valeurs de recherche publiés par l’utilisateur et utiliser les valeurs à rechercher la base de données.

## <a name="displaying-the-searchindex-form"></a>Afficher le formulaire SearchIndex

Commencez par ajouter un `SearchIndex` méthode d’action à l’objet existant `MoviesController` classe. La méthode retourne une vue qui contient un formulaire HTML. Voici le code :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

La première ligne de la `SearchIndex` méthode crée les éléments suivants [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) requête pour sélectionner les films :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

La requête est définie à ce stade, mais n’a pas encore été exécutée sur le magasin de données.

Si le `searchString` paramètre contient une chaîne, la requête de films est modifiée pour filtrer sur la valeur de la chaîne de recherche, en utilisant le code suivant :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

Le code `s => s.Title` ci-dessus est une [expression lambda](https://msdn.microsoft.com/library/bb397687.aspx). Les expressions lambda sont utilisées dans fondées sur une méthode [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) de requêtes en tant qu’arguments aux méthodes d’opérateur de requête standard telles que la [où](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) méthode utilisée dans le code ci-dessus. Requêtes LINQ ne sont pas exécutées quand elles sont définies ou quand elles sont modifiées en appelant une méthode telle que `Where` ou `OrderBy`. Au lieu de cela, l’exécution de requête est différée, ce qui signifie que l’évaluation d’une expression est retardée jusqu'à ce que sa valeur réalisée est une itération réelle ou le [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) méthode est appelée. Dans le `SearchIndex` exemple, la requête est exécutée dans la vue SearchIndex. Pour plus d’informations sur l’exécution différée des requêtes, consultez [Exécution de requête](https://msdn.microsoft.com/library/bb738633.aspx).

Maintenant que vous pouvez implémenter la `SearchIndex` vue qui affiche le formulaire à l’utilisateur. Avec le bouton droit à l’intérieur de la `SearchIndex` (méthode), puis cliquez sur **ajouter une vue**. Dans le **ajouter une vue** boîte de dialogue, spécifiez que vous allez passer un `Movie` objet pour le modèle de vue en tant que sa classe de modèle. Dans le **modèle de structure** , choisissez **liste**, puis cliquez sur **ajouter**.

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

Lorsque vous cliquez sur le **ajouter** bouton, le *Views\Movies\SearchIndex.cshtml* afficher le modèle est créé. Étant donné que vous avez sélectionné **liste** dans le **modèle de structure** répertorier, Visual Studio généré automatiquement (généré automatiquement) des balises par défaut dans la vue. La génération de modèles automatique créé un formulaire HTML. Elle a étudié les `Movie` classe et a créé le code pour restituer `<label>` éléments pour chaque propriété de la classe. La liste ci-dessous affiche la vue Create qui a été générée :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

Exécutez l’application et accédez à */Movies/SearchIndex*. Ajoutez une chaîne de requête comme `?searchString=ghost` à l’URL. Les films filtrés sont affichés.

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

Si vous modifiez la signature de la `SearchIndex` méthode d’avoir un paramètre nommé `id`, le `id` paramètre correspond à la `{id}` espace réservé pour la valeur par défaut achemine ensemble dans le *Global.asax* fichier.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.json)]

La version d’origine `SearchIndex` méthode ressemble à ceci :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

Modifié `SearchIndex` méthode se présente comme suit :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

Vous pouvez maintenant passer le titre de la recherche en tant que données de routage (un segment de l’URL) et non pas en tant que valeur de chaîne de requête.

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

Cependant, vous ne pouvez pas attendre des utilisateurs qu’ils modifient l’URL à chaque fois qu’ils veulent rechercher un film. Maintenant vous vous allez ajouter une interface utilisateur pour aider à les filtrer les films. Si vous avez modifié la signature de la `SearchIndex` méthode pour tester comment passer le paramètre ID de la limite de l’itinéraire, modifiez-le pour que votre `SearchIndex` méthode prend un paramètre de chaîne nommé `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

Ouvrez le *Views\Movies\SearchIndex.cshtml* de fichiers et juste après `@Html.ActionLink("Create New", "Create")`, ajoutez le code suivant :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

L’exemple suivant montre une partie de la *Views\Movies\SearchIndex.cshtml* fichier avec la balise de filtrage ajoutée.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

Le `Html.BeginForm` helper crée une ouverture `<form>` balise. Le `Html.BeginForm` helper, le formulaire à publier vers lui-même lorsque l’utilisateur envoie le formulaire en cliquant sur le **filtre** bouton.

Exécutez l’application et essayez de rechercher un film.

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

Il existe aucune `HttpPost` surcharge de la `SearchIndex` (méthode). Vous n’en avez besoin, car la méthode ne change pas l’état de l’application, filtrage des données.

Vous pourriez ajouter la méthode `HttpPost SearchIndex` suivante. Dans ce cas, le demandeur d’action correspondrait à la `HttpPost SearchIndex` (méthode) et le `HttpPost SearchIndex` méthode s’exécuterait comme indiqué dans l’image ci-dessous.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

Cependant, même si vous ajoutez cette version `HttpPost` de la méthode `SearchIndex`, il existe une limitation dans la façon dont tout ceci a été implémenté. Imaginez que vous voulez insérer un signet pour une recherche spécifique, ou que vous voulez envoyer un lien à vos amis sur lequel ils peuvent cliquer pour afficher la même liste filtrée de films. Notez que l’URL de la requête HTTP POST est identique à l’URL de la requête GET (localhost : XXXXX/Movies/SearchIndex) : il n’existe aucune information de recherche dans l’URL proprement dite. Droite désormais, les informations de chaîne de recherche sont envoyées au serveur en tant que valeur d’un champ de formulaire. Cela signifie que vous ne pouvez pas capturer ces informations de recherche pour créer un signet ou envoyer à vos amis dans une URL.

La solution consiste à utiliser une surcharge de `BeginForm` qui spécifie que la requête POST doit ajouter les informations de recherche à l’URL et qu’il doit être routé vers la version HttpGet de la `SearchIndex` (méthode). Remplacer l’existant sans paramètre `BeginForm` méthode par le code suivant :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

Maintenant lorsque vous soumettez une recherche, l’URL contient une chaîne de requête de recherche. La recherche accède également à la méthode d’action `HttpGet SearchIndex`, même si vous avez une méthode `HttpPost SearchIndex`.

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>Ajout de la recherche par Genre

Si vous avez ajouté le `HttpPost` version de la `SearchIndex` (méthode), supprimez maintenant.

Ensuite, vous allez ajouter une fonctionnalité pour permettre aux utilisateurs de rechercher des films par genre. Remplacez la méthode `SearchIndex` par le code suivant :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

Cette version de la `SearchIndex` méthode prend un paramètre supplémentaire, à savoir `movieGenre`. Les premières lignes de code créent un `List` objet pour contenir les genres de film à partir de la base de données.

Le code suivant est une requête LINQ qui récupère tous les genres dans la base de données.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

Le code utilise le `AddRange` méthode du générique `List` collection pour ajouter tous les genres distincts à la liste. (Sans le `Distinct` modificateur, genres en doublon seraient ajoutés, par exemple, comédie est ajoutée à deux reprises dans notre exemple). Le code stocke ensuite la liste des genres dans le `ViewBag` objet.

Le code suivant montre comment vérifier la `movieGenre` paramètre. S’il n’est pas vide, le code plus contraint la requête de films pour limiter les films sélectionnés pour le genre spécifié.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Ajout de balisage à la vue SearchIndex pour prendre en charge la recherche par Genre

Ajouter un `Html.DropDownList` helper pour le *Views\Movies\SearchIndex.cshtml* de fichiers, juste avant la `TextBox` helper. Le balisage complet est indiqué ci-dessous :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

Exécutez l’application et accédez à */Movies/SearchIndex*. Essayez une recherche par genre, par le nom du film et par les deux critères.

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

Dans cette section, vous avez examiné les méthodes d’action CRUD et les vues générées par le framework. Vous avez créé une méthode d’action de recherche et de la vue qui permettre aux utilisateurs de rechercher par titre de film et genre. Dans la section suivante, vous explorerez comment ajouter une propriété à la `Movie` modèle et comment ajouter un initialiseur qui crée automatiquement une base de données de test.

> [!div class="step-by-step"]
> [Précédent](accessing-your-models-data-from-a-controller.md)
> [Suivant](adding-a-new-field-to-the-movie-model-and-table.md)
