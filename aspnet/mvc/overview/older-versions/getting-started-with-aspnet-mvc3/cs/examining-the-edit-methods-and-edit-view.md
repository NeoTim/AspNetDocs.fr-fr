---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
title: Examen des méthodes de modification et de la vue de modification (C#) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 1d266bf0-a61e-423b-a3d2-13773d7dafe2
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: baa300c66bfae9fe602a8fe597e21b0abbaf3a63
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89044985"
---
# <a name="examining-the-edit-methods-and-edit-view-c"></a>Examen des méthodes de modification et de la vue de modification (C#)

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../../getting-started/introduction/getting-started.md) et utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sûr, bien plus simple à suivre et qui illustre davantage de fonctionnalités.
> 
> 
> Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis ci-dessous. Vous pouvez les installer en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les composants requis à l’aide des liens suivants :
> 
> - [Conditions préalables pour Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mise à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(prise en charge de Runtime + Tools)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 Prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet Visual Web Developer avec du code source C# est disponible pour accompagner cette rubrique. [Téléchargez la version C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez Visual Basic, passez à la [version Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de ce didacticiel.

Dans cette section, vous allez examiner les méthodes et les vues d’action générées pour le contrôleur de films. Vous allez ensuite ajouter une page de recherche personnalisée.

Exécutez l’application et accédez au `Movies` contrôleur en ajoutant */movies* à l’URL dans la barre d’adresses de votre navigateur. Maintenez le pointeur de la souris sur un lien **modifier** pour afficher l’URL à laquelle il est lié.

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image2.png)](examining-the-edit-methods-and-edit-view/_static/image1.png)

Le lien **Edit** a été généré par la `Html.ActionLink` méthode dans la vue *Views\Movies\Index.cshtml* :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![Html. ActionLink](examining-the-edit-methods-and-edit-view/_static/image4.png)](examining-the-edit-methods-and-edit-view/_static/image3.png)

L' `Html` objet est une application d’assistance exposée à l’aide d’une propriété sur la `WebViewPage` classe de base. La `ActionLink` méthode de l’application auxiliaire facilite la génération dynamique des liens hypertexte HTML qui sont liés aux méthodes d’action sur les contrôleurs. Le premier argument de la `ActionLink` méthode est le texte de lien à afficher (par exemple, `<a>Edit Me</a>` ). Le deuxième argument est le nom de la méthode d’action à appeler. Le dernier argument est un [objet anonyme](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) qui génère les données d’itinéraire (dans le cas présent, l’ID 4).

Le lien généré affiché dans l’image précédente est `http://localhost:xxxxx/Movies/Edit/4` . L’itinéraire par défaut prend le modèle d’URL `{controller}/{action}/{id}` . Par conséquent, ASP.NET se traduit par `http://localhost:xxxxx/Movies/Edit/4` une requête à la `Edit` méthode d’action du `Movies` contrôleur avec le paramètre `ID` égal à 4.

Vous pouvez également passer des paramètres de méthode d’action à l’aide d’une chaîne de requête. Par exemple, l’URL `http://localhost:xxxxx/Movies/Edit?ID=4` passe également le paramètre `ID` de 4 à la `Edit` méthode d’action du `Movies` contrôleur.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image6.png)](examining-the-edit-methods-and-edit-view/_static/image5.png)

Ouvrez le `Movies` contrôleur. Les deux `Edit` méthodes d’action sont présentées ci-dessous.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Notez que la deuxième méthode d’action `Edit` est précédée de l’attribut `HttpPost`. Cet attribut spécifie que la surcharge de la `Edit` méthode peut être appelée uniquement pour les requêtes de publication. Vous pouvez appliquer l' `HttpGet` attribut à la première méthode Edit, mais cela n’est pas nécessaire, car il s’agit de la valeur par défaut. (Nous reviendrons sur les méthodes d’action qui sont assignées de manière implicite à l' `HttpGet` attribut en tant que `HttpGet` méthodes.)

La `HttpGet` `Edit` méthode prend le paramètre Movie ID, recherche le film à l’aide de la `Find` méthode Entity Framework et retourne le film sélectionné à la vue Edit. Quand le système de génération de modèles automatique a créé la vue Edit, il a examiné la classe `Movie` et a créé le code pour restituer les éléments `<label>` et `<input>` de chaque propriété de la classe. L’exemple suivant montre la vue Edit qui a été générée :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

Notez que le modèle de vue contient une `@model MvcMovie.Models.Movie` instruction en haut du fichier, ce qui indique que la vue s’attend à ce que le modèle du modèle de vue soit de type `Movie` .

Le code de génération de modèles automatique utilise plusieurs *méthodes d’assistance* pour simplifier le balisage HTML. L' [`Html.LabelFor`](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) application auxiliaire affiche le nom du champ (« title », « Released », « genre » ou « Price »). L' [`Html.EditorFor`](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) application auxiliaire affiche un `<input>` élément HTML. L' [`Html.ValidationMessageFor`](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) application auxiliaire affiche tous les messages de validation associés à cette propriété.

Exécutez l’application et accédez à l’URL */movies* . Cliquez sur un lien **modifier** . Dans le navigateur, affichez la source de la page. Le code HTML de la page ressemble à l’exemple suivant. (Le balisage de menu a été exclu par souci de clarté.)

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample4.html)]

Les `<input>` éléments se trouvent dans un `<form>` élément HTML dont l' `action` attribut a la valeur publication sur l’URL */movies/Edit* . Les données du formulaire sont publiées sur le serveur lorsque l’utilisateur clique sur le bouton **modifier** .

## <a name="processing-the-post-request"></a>Traitement de la requête POST

Le code suivant montre la version `HttpPost` de la méthode d’action `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs)]

Le Binder de modèle ASP.NET Framework prend les valeurs de formulaire publiées et crée un `Movie` objet qui est passé comme `movie` paramètre. La `ModelState.IsValid` vérification dans le code vérifie que les données envoyées dans le formulaire peuvent être utilisées pour modifier un `Movie` objet. Si les données sont valides, le code enregistre les données de film dans la `Movies` collection de l' `MovieDBContext` instance. Le code enregistre ensuite les nouvelles données de film dans la base de données en appelant la `SaveChanges` méthode de `MovieDBContext` , qui rend persistantes les modifications apportées à la base de données. Une fois les données enregistrées, le code redirige l’utilisateur vers la `Index` méthode d’action de la `MoviesController` classe, ce qui entraîne l’affichage du film mis à jour dans la liste des films.

Si les valeurs publiées ne sont pas valides, elles sont réaffichées dans le formulaire. Les `Html.ValidationMessageFor` applications auxiliaires dans le modèle de vue *Edit. cshtml* s’occupent de l’affichage des messages d’erreur appropriés.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image8.png)](examining-the-edit-methods-and-edit-view/_static/image7.png)

> **Remarque à propos des paramètres régionaux** Si vous travaillez normalement avec des paramètres régionaux autres que l’anglais, consultez [prise en charge de la Validation ASP.NET MVC 3 avec des paramètres régionaux non anglais.](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)

## <a name="making-the-edit-method-more-robust"></a>Rendre la méthode Edit plus robuste

La `HttpGet` `Edit` méthode générée par le système de génération de modèles automatique ne vérifie pas si l’ID qui lui est passé est valide. Si un utilisateur supprime le segment d’ID de l’URL ( `http://localhost:xxxxx/Movies/Edit` ), l’erreur suivante s’affiche :

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image10.png)](examining-the-edit-methods-and-edit-view/_static/image9.png)

Un utilisateur peut également passer un ID qui n’existe pas dans la base de données, par exemple `http://localhost:xxxxx/Movies/Edit/1234` . Vous pouvez apporter deux modifications à la `HttpGet` `Edit` méthode d’action pour résoudre cette limitation. Tout d’abord, modifiez le `ID` paramètre pour avoir une valeur par défaut de zéro lorsqu’un ID n’est pas explicitement passé. Vous pouvez également vérifier que la `Find` méthode a réellement trouvé un film avant de retourner l’objet film au modèle de vue. La méthode mise à jour `Edit` est illustrée ci-dessous.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

Si aucun film n’est trouvé, la `HttpNotFound` méthode est appelée.

Toutes les `HttpGet` méthodes suivent un modèle similaire. Ils obtiennent un objet Movie (ou une liste d’objets, dans le cas de `Index` ) et transmettent le modèle à la vue. La `Create` méthode passe un objet Movie vide à la vue Create. Toutes les méthodes qui créent, modifient, suppriment ou changent d’une quelconque manière des données le font dans la surcharge `HttpPost` de la méthode. La modification des données dans une méthode HTTP obtient est un risque pour la sécurité, comme décrit dans le billet de blog entrée [ASP.net Conseil MVC #46 – ne pas utiliser les liens de suppression, car ils créent des failles de sécurité](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). La modification des données dans une méthode d’extraction viole également les meilleures pratiques HTTP et le modèle architectural REST, qui spécifie que les demandes d’extraction ne doivent pas modifier l’état de votre application. En d’autres termes, l’exécution d’une opération d’extraction doit être une opération sécurisée qui n’a aucun effet secondaire.

## <a name="adding-a-search-method-and-search-view"></a>Ajout d’une méthode de recherche et d’un affichage de recherche

Dans cette section, vous allez ajouter une `SearchIndex` méthode d’action qui vous permet de rechercher des films par genre ou par nom. Celui-ci sera disponible à l’aide de l’URL */movies/SearchIndex* . La requête affiche un formulaire HTML qui contient les éléments d’entrée qu’un utilisateur peut remplir afin de rechercher un film. Lorsqu’un utilisateur envoie le formulaire, la méthode d’action obtient les valeurs de recherche publiées par l’utilisateur et utilise les valeurs pour effectuer une recherche dans la base de données.

## <a name="displaying-the-searchindex-form"></a>Affichage du formulaire SearchIndex

Commencez par ajouter une `SearchIndex` méthode d’action à la `MoviesController` classe existante. La méthode retourne une vue qui contient un formulaire HTML. Voici le code :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cs)]

La première ligne de la `SearchIndex` méthode crée la requête [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) suivante pour sélectionner les films :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cs)]

La requête est définie à ce stade, mais elle n’a pas encore été exécutée sur le magasin de données.

Si le `searchString` paramètre contient une chaîne, la requête de films est modifiée pour filtrer sur la valeur de la chaîne de recherche, à l’aide du code suivant :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

Les requêtes LINQ ne sont pas exécutées quand elles sont définies ou quand elles sont modifiées en appelant une méthode telle que `Where` ou `OrderBy` . Au lieu de cela, l’exécution de la requête est différée, ce qui signifie que l’évaluation d’une expression est retardée jusqu’à ce que sa valeur réalisée soit réellement itérée ou que la [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) méthode soit appelée. Dans l' `SearchIndex` exemple, la requête est exécutée dans la vue SearchIndex. Pour plus d’informations sur l’exécution différée des requêtes, consultez [Exécution de requête](https://msdn.microsoft.com/library/bb738633.aspx).

À présent, vous pouvez implémenter la `SearchIndex` vue qui affichera le formulaire pour l’utilisateur. Cliquez avec le bouton droit à l’intérieur de la `SearchIndex` méthode, puis cliquez sur **Ajouter une vue**. Dans la boîte de dialogue **Ajouter une vue** , spécifiez que vous allez passer un `Movie` objet au modèle de vue en tant que classe de modèle. Dans la liste **modèle de structure** , choisissez **liste**, puis cliquez sur **Ajouter**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Lorsque vous cliquez sur le bouton **Ajouter** , le modèle de vue *Views\Movies\SearchIndex.cshtml* est créé. Dans la mesure où vous avez sélectionné la **liste** dans la liste **modèle de structure** , Visual Web Developer a généré automatiquement un contenu par défaut dans la vue. La génération de modèles automatique a créé un formulaire HTML. Il a examiné la `Movie` classe et créé du code pour afficher les `<label>` éléments de chaque propriété de la classe. La liste ci-dessous montre la vue Create qui a été générée :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Exécutez l’application et accédez à */movies/SearchIndex*. Ajoutez une chaîne de requête comme `?searchString=ghost` à l’URL. Les films filtrés sont affichés.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Si vous modifiez la signature de la `SearchIndex` méthode pour qu’elle ait un paramètre nommé `id` , le `id` paramètre correspondra à l' `{id}` espace réservé pour les itinéraires par défaut définis dans le fichier *global. asax* .

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.txt)]

La méthode modifiée se `SearchIndex` présente comme suit :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cs)]

Vous pouvez maintenant passer le titre de la recherche en tant que données de routage (un segment de l’URL) et non pas en tant que valeur de chaîne de requête.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

Cependant, vous ne pouvez pas attendre des utilisateurs qu’ils modifient l’URL à chaque fois qu’ils veulent rechercher un film. Vous allez maintenant ajouter l’interface utilisateur pour filtrer les films. Si vous avez modifié la signature de la `SearchIndex` méthode pour tester la manière de passer le paramètre d’ID lié à l’itinéraire, modifiez-le de façon à ce que votre `SearchIndex` méthode prenne un paramètre de chaîne nommé `searchString` :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample13.cs)]

Ouvrez le fichier *Views\Movies\SearchIndex.cshtml* et, juste après `@Html.ActionLink("Create New", "Create")` , ajoutez ce qui suit :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cshtml)]

L’exemple suivant montre une partie du fichier *Views\Movies\SearchIndex.cshtml* avec le balisage de filtrage ajouté.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cshtml)]

L' `Html.BeginForm` application auxiliaire crée une `<form>` balise d’ouverture. L' `Html.BeginForm` application d’assistance fait en sorte que le formulaire publie sur lui-même lorsque l’utilisateur envoie le formulaire en cliquant sur le bouton de **filtre** .

Exécutez l’application et essayez de rechercher un film.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

Il n’y a aucune `HttpPost` surcharge de la `SearchIndex` méthode. Vous n’en avez pas besoin, car la méthode ne modifie pas l’état de l’application, il suffit de filtrer les données.

Vous pourriez ajouter la méthode `HttpPost SearchIndex` suivante. Dans ce cas, le demandeur d’action correspondrait à la `HttpPost SearchIndex` méthode et la `HttpPost SearchIndex` méthode s’exécuterait comme indiqué dans l’image ci-dessous.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

Cependant, même si vous ajoutez cette version `HttpPost` de la méthode `SearchIndex`, il existe une limitation dans la façon dont tout ceci a été implémenté. Imaginez que vous voulez insérer un signet pour une recherche spécifique, ou que vous voulez envoyer un lien à vos amis sur lequel ils peuvent cliquer pour afficher la même liste filtrée de films. Notez que l’URL de la requête HTTP POSTÉRIEURe est identique à l’URL de la demande d’extraction (localhost : xxxxx/movies/SearchIndex). il n’y a pas d’informations de recherche dans l’URL elle-même. Pour le moment, les informations de la chaîne de recherche sont envoyées au serveur en tant que valeur de champ de formulaire. Cela signifie que vous ne pouvez pas capturer les informations de recherche dans un signet ou les envoyer à des amis dans une URL.

La solution consiste à utiliser une surcharge de `BeginForm` qui spécifie que la requête de publication doit ajouter les informations de recherche à l’URL et qui doit être acheminée vers la version HttpGet de la `SearchIndex` méthode. Remplacez la méthode sans paramètre existante `BeginForm` par ce qui suit :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml)]

[![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image22.png)](examining-the-edit-methods-and-edit-view/_static/image21.png)

Désormais, lorsque vous envoyez une recherche, l’URL contient une chaîne de requête de recherche. La recherche accède également à la méthode d’action `HttpGet SearchIndex`, même si vous avez une méthode `HttpPost SearchIndex`.

[![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image24.png)](examining-the-edit-methods-and-edit-view/_static/image23.png)

## <a name="adding-search-by-genre"></a>Ajout de la recherche par genre

Si vous avez ajouté la `HttpPost` version de la `SearchIndex` méthode, supprimez-la maintenant.

Ensuite, vous allez ajouter une fonctionnalité pour permettre aux utilisateurs de rechercher des films par genre. Remplacez la méthode `SearchIndex` par le code suivant :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cs)]

Cette version de la `SearchIndex` méthode prend un paramètre supplémentaire, à savoir `movieGenre` . Les premières lignes de code créent un `List` objet pour stocker les genres de films à partir de la base de données.

Le code suivant est une requête LINQ qui récupère tous les genres dans la base de données.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

Le code utilise la `AddRange` méthode de la `List` collection générique pour ajouter tous les genres distincts à la liste. (Sans le `Distinct` modificateur, les genres en double seraient ajoutés, par exemple, comédie serait ajouté deux fois dans notre exemple). Le code stocke ensuite la liste des genres dans l' `ViewBag` objet.

Le code suivant montre comment vérifier le `movieGenre` paramètre. Si ce n’est pas le cas, le code limite davantage la requête movies pour limiter les films sélectionnés au genre spécifié.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Ajout de balises à la vue SearchIndex pour prendre en charge la recherche par genre

Ajoutez une `Html.DropDownList` application d’assistance au fichier *Views\Movies\SearchIndex.cshtml* , juste avant l' `TextBox` application auxiliaire. Le balisage terminé est illustré ci-dessous :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cshtml)]

Exécutez l’application et accédez à */movies/SearchIndex*. Essayez d’effectuer une recherche par genre, par nom de film et les deux critères.

Dans cette section, vous avez examiné les méthodes et les vues d’action CRUD générées par l’infrastructure. Vous avez créé une méthode et une vue d’action de recherche qui permettent aux utilisateurs de rechercher par titre et par genre de films. Dans la section suivante, vous allez examiner comment ajouter une propriété au `Movie` modèle et comment ajouter un initialiseur qui créera automatiquement une base de données de test.

> [!div class="step-by-step"]
> [Précédent](accessing-your-models-data-from-a-controller.md) 
>  [Suivant](adding-a-new-field.md)
