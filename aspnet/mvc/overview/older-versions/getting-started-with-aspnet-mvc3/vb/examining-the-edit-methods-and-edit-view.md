---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
title: Examen des méthodes de modification et de la vue Edit (VB) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est en cours...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 5cb3c59b-1e96-464b-b3a8-c55607201872
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 509f362a15990486cc5fa4f2f666c3d0de2434dc
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57055966"
---
<a name="examining-the-edit-methods-and-edit-view-vb"></a>Examen des méthodes de modification et de la vue de modification (VB)
====================
par [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Ce didacticiel vous apprend les notions de base de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis listés ci-dessous. Vous pouvez installer tous les en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les conditions préalables à l’aide des liens suivants :
> 
> - [Prérequis pour le Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mettre à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + outils prennent en charge)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet de Visual Web Developer avec le code source VB.NET est disponible pour accompagner cette rubrique. [Téléchargez la version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez c#, basculez vers le [c# version](../cs/examining-the-edit-methods-and-edit-view.md) de ce didacticiel.


Dans cette section, vous allez examiner les méthodes d’action généré et les vues pour le contrôleur de film. Ensuite, vous allez ajouter une page de recherche personnalisée.

Exécutez l’application et accédez à la `Movies` contrôleur en ajoutant */Movies* à l’URL dans la barre d’adresses de votre navigateur. Maintenez le pointeur de la souris sur un **modifier** lien pour afficher l’URL auxquelles il est lié.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Le **modifier** lien a été généré par le `Html.ActionLink` méthode dans le *Views\Movies\Index.vbhtml* vue :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image3.png)](examining-the-edit-methods-and-edit-view/_static/image2.png)

Le `Html` objet est une application d’assistance qui est exposée à l’aide d’une propriété sur le `WebViewPage` classe de base. Le `ActionLink` méthode de l’application d’assistance facilite l’utilisation générer dynamiquement le code HTML des liens hypertexte aux méthodes d’action sur les contrôleurs. Le premier argument de la `ActionLink` méthode est le texte de lien à afficher (par exemple, `<a>Edit Me</a>`). Le deuxième argument est le nom de la méthode d’action à appeler. L’argument final est un [objet anonyme](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) qui génère les données d’itinéraire (dans ce cas, l’ID de 4).

Le lien généré indiqué dans l’image précédente est `http://localhost:xxxxx/Movies/Edit/4`. L’itinéraire par défaut prend le modèle d’URL `{controller}/{action}/{id}`. Par conséquent, ASP.NET se traduit par `http://localhost:xxxxx/Movies/Edit/4` en une requête à la `Edit` méthode d’action de la `Movies` contrôleur avec le paramètre `ID` égal à 4.

Vous pouvez également transmettre des paramètres de méthode d’action à l’aide d’une chaîne de requête. Par exemple, l’URL `http://localhost:xxxxx/Movies/Edit?ID=4` passe également le paramètre `ID` de 4 processeurs à la `Edit` méthode d’action de la `Movies` contrôleur.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image5.png)](examining-the-edit-methods-and-edit-view/_static/image4.png)

Ouvrez la `Movies` contrôleur. Les deux `Edit` méthodes d’action sont présentées ci-dessous.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample3.vb)]

Notez que la deuxième méthode d’action `Edit` est précédée de l’attribut `HttpPost`. Cet attribut spécifie cette surcharge de la `Edit` méthode peut être appelée uniquement pour les requêtes POST. Vous pouvez appliquer la `HttpGet` modifier la méthode de l’attribut à la première, mais qui n’est pas nécessaire car il s’agit de la valeur par défaut. (Nous allons nous référer aux méthodes d’action attribués de manière implicite le `HttpGet` sous la forme `HttpGet` méthodes.)

Le `HttpGet` `Edit` méthode prend le paramètre ID de film, recherche le film à l’aide d’Entity Framework `Find` (méthode) et retourne le film sélectionné à la vue Edit. Quand le système de génération de modèles automatique a créé la vue Edit, il a examiné la classe `Movie` et a créé le code pour restituer les éléments `<label>` et `<input>` de chaque propriété de la classe. L’exemple suivant montre la vue Edit qui a été générée :

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.vbhtml)]

Notez comment le modèle de vue a un `@ModelType MvcMovie.Models.Movie` instruction en haut du fichier : Spécifie que la vue prévoit que le modèle pour le modèle de vue soit de type `Movie`.

Le code généré automatiquement utilise plusieurs *méthodes d’assistance* pour rationaliser le balisage HTML. Le [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) helper affiche le nom du champ (&quot;titre&quot;, &quot;ReleaseDate&quot;, &quot;Genre&quot;, ou &quot;prix &quot;). Le [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) helper affiche HTML `<input>` élément. Le [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) helper affiche les messages de validation associés à cette propriété.

Exécutez l’application et accédez à la */Movies* URL. Cliquez sur un lien **Edit**. Dans le navigateur, affichez la source de la page. Le code HTML dans la page ressemble à l’exemple suivant. (Le balisage de menu a été exclu par souci de clarté).

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html)]

Le `<input>` sont des éléments dans le code HTML `<form>` élément dont `action` attribut a la valeur à valider dans le */Movies/Edit* URL. Les données du formulaire seront publiées sur le serveur lors de la **modifier** bouton.

## <a name="processing-the-post-request"></a>Traitement de la requête POST

Le code suivant montre la version `HttpPost` de la méthode d’action `Edit`.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample6.vb)]

Le binder de modèle ASP.NET framework prend les valeurs de formulaire publiées et crée un `Movie` objet qui est passé en tant que le `movie` paramètre. Le `ModelState.IsValid` contrôle dans le code vérifie que les données envoyées dans le formulaire peuvent être utilisées pour modifier un `Movie` objet. Si les données sont valides, le code enregistre les données de film à la `Movies` collection de la `MovieDBContext` instance. Le code enregistre ensuite les nouvelles données de film à la base de données en appelant le `SaveChanges` méthode de `MovieDBContext`, ce qui rend les modifications de la base de données. Après avoir enregistré les données, le code redirige l’utilisateur vers le `Index` méthode d’action de la `MoviesController` (classe), ce qui provoque le film mis à jour à afficher dans la liste de films.

Si les valeurs publiées ne sont pas valides, ils réapparaissent dans le formulaire. Le `Html.ValidationMessageFor` helpers dans les *Edit.vbhtml* vue modèle de prendre en charge d’affichage des messages d’erreur appropriés.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image7.png)](examining-the-edit-methods-and-edit-view/_static/image6.png)

> **Remarque sur les paramètres régionaux** si vous travaillez généralement avec des paramètres régionaux autres que l’anglais, consultez [prenant en charge la Validation ASP.NET MVC 3 avec les paramètres régionaux Non anglais.](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>Rendre la méthode Edit plus robuste

Le `HttpGet` `Edit` méthode générée par le système de génération de modèles automatique ne vérifie pas que l’ID qui lui est passé est valide. Si un utilisateur supprime le segment de l’ID de l’URL (`http://localhost:xxxxx/Movies/Edit`), l’erreur suivante s’affiche :

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image9.png)](examining-the-edit-methods-and-edit-view/_static/image8.png)

Un utilisateur peut également passer un ID qui n’existe pas dans la base de données, tel que `http://localhost:xxxxx/Movies/Edit/1234`. Vous pouvez apporter deux modifications à la `HttpGet` `Edit` méthode d’action pour contourner cette limitation. Tout d’abord, modifiez le `ID` paramètre possède une valeur par défaut de zéro lorsqu’un ID n’est pas passé explicitement. Vous pouvez également vérifier que le `Find` méthode trouvée en fait un film avant de retourner l’objet de film pour le modèle de vue. La mise à jour `Edit` méthode est indiquée ci-dessous.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample7.vb)]

Si aucun film n’est trouvé, le `HttpNotFound` méthode est appelée.

Tous les `HttpGet` méthodes suivent un modèle similaire. Ils obtiennent un objet de film (ou une liste d’objets, dans le cas de `Index`) et passer le modèle à la vue. Le `Create` méthode passe un objet de film vide à la vue de créer. Toutes les méthodes qui créent, modifient, suppriment ou changent d’une quelconque manière des données le font dans la surcharge `HttpPost` de la méthode. Modification des données dans une méthode HTTP GET est un risque de sécurité, comme décrit dans le billet de blog [ASP.NET MVC Conseil #46 : n’utilisez pas supprimer les liens, car ils créent des failles de sécurité](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modification des données dans une méthode GET enfreint également les bonnes pratiques HTTP et le modèle d’architecture REST, laquelle spécifie que les requêtes GET ne doivent pas changer l’état de votre application. En d’autres termes, une opération GET doit être une opération sûre qui n’a aucun effet secondaire.

## <a name="adding-a-search-method-and-search-view"></a>Ajout d’une méthode de recherche et de la vue de la recherche

Dans cette section, vous ajouterez un `SearchIndex` méthode d’action qui vous permet de rechercher des films par genre ou le nom. Ce sera disponible en utilisant le */Movies/SearchIndex* URL. La requête affiche un formulaire HTML qui contient les éléments d’entrée qu’un utilisateur peut remplir dans afin de rechercher un film. Lorsqu’un utilisateur soumet le formulaire, la méthode d’action sera obtenir les valeurs de recherche publiés par l’utilisateur et utiliser les valeurs à rechercher la base de données.

![SearchIndx_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

## <a name="displaying-the-searchindex-form"></a>Afficher le formulaire SearchIndex

Commencez par ajouter un `SearchIndex` méthode d’action à l’objet existant `MoviesController` classe. La méthode retourne une vue qui contient un formulaire HTML. Voici le code :

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample8.vb)]

La première ligne de la `SearchIndex` méthode crée les éléments suivants [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) requête pour sélectionner les films :

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample9.vb)]

La requête est définie à ce stade, mais n’a pas encore été exécutée sur le magasin de données.

Si le `searchString` paramètre contient une chaîne, la requête de films est modifiée pour filtrer sur la valeur de la chaîne de recherche, en utilisant le code suivant :

Si ce n’est pas String.IsNullOrEmpty(searchString) puis   
 films = films. Où (ou les fonctions s.Title.Contains(searchString))   
 End If

Requêtes LINQ ne sont pas exécutées quand elles sont définies ou quand elles sont modifiées en appelant une méthode telle que `Where` ou `OrderBy`. Au lieu de cela, l’exécution de requête est différée, ce qui signifie que l’évaluation d’une expression est retardée jusqu'à ce que sa valeur réalisée est une itération réelle ou le [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) méthode est appelée. Dans le `SearchIndex` exemple, la requête est exécutée dans la vue SearchIndex. Pour plus d’informations sur l’exécution différée des requêtes, consultez [Exécution de requête](https://msdn.microsoft.com/library/bb738633.aspx).

Maintenant que vous pouvez implémenter la `SearchIndex` vue qui affiche le formulaire à l’utilisateur. Avec le bouton droit à l’intérieur de la `SearchIndex` (méthode), puis cliquez sur **ajouter une vue**. Dans le **ajouter une vue** boîte de dialogue, spécifiez que vous allez passer un `Movie` objet pour le modèle de vue en tant que sa classe de modèle. Dans le **modèle de structure** , choisissez **liste**, puis cliquez sur **ajouter**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Lorsque vous cliquez sur le **ajouter** bouton, le *Views\Movies\SearchIndex.vbhtml* afficher le modèle est créé. Étant donné que vous avez sélectionné **liste** dans le **modèle de structure** répertorier, Visual Web Developer généré automatiquement (généré automatiquement) du contenu par défaut dans la vue. La génération de modèles automatique créé un formulaire HTML. Elle a étudié les `Movie` classe et a créé le code pour restituer `<label>` éléments pour chaque propriété de la classe. La liste ci-dessous affiche la vue Create qui a été générée :

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.vbhtml)]

Exécutez l’application et accédez à */Movies/SearchIndex*. Ajoutez une chaîne de requête comme `?searchString=ghost` à l’URL. Les films filtrés sont affichés.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Si vous modifiez la signature de la `SearchIndex` méthode d’avoir un paramètre nommé `id`, le `id` paramètre correspond à la `{id}` espace réservé pour la valeur par défaut achemine ensemble dans le *Global.asax* fichier.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

Modifié `SearchIndex` méthode se présente comme suit :

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample12.vb)]

Vous pouvez maintenant passer le titre de la recherche en tant que données de routage (un segment de l’URL) et non pas en tant que valeur de chaîne de requête.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

Cependant, vous ne pouvez pas attendre des utilisateurs qu’ils modifient l’URL à chaque fois qu’ils veulent rechercher un film. Maintenant vous vous allez ajouter une interface utilisateur pour aider à les filtrer les films. Si vous avez modifié la signature de la `SearchIndex` méthode pour tester comment passer le paramètre ID de la limite de l’itinéraire, modifiez-le pour que votre `SearchIndex` méthode prend un paramètre de chaîne nommé `searchString`:

Ouvrez le *Views\Movies\SearchIndex.vbhtml* de fichiers et juste après `@Html.ActionLink("Create New", "Create")`, ajoutez le code suivant :

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample13.vbhtml)]

Le `Html.BeginForm` helper crée une ouverture `<form>` balise. Le `Html.BeginForm` helper, le formulaire à publier vers lui-même lorsque l’utilisateur envoie le formulaire en cliquant sur le **filtre** bouton.

Exécutez l’application et essayez de rechercher un film.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

Il existe aucune `HttpPost` surcharge de la `SearchIndex` (méthode). Vous n’en avez besoin, car la méthode ne change pas l’état de l’application, filtrage des données. Si vous avez ajouté les éléments suivants `HttpPost` `SearchIndex` (méthode), le demandeur d’action correspondrait à la `HttpPost` `SearchIndex` (méthode) et le `HttpPost` `SearchIndex` méthode s’exécuterait comme indiqué dans l’image ci-dessous.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample14.vb)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

## <a name="adding-search-by-genre"></a>Ajout de la recherche par Genre

Si vous avez ajouté le `HttpPost` version de la `SearchIndex` (méthode), supprimez maintenant.

Ensuite, vous allez ajouter une fonctionnalité pour permettre aux utilisateurs de rechercher des films par genre. Remplacez la méthode `SearchIndex` par le code suivant :

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample15.vb)]

Cette version de la `SearchIndex` méthode prend un paramètre supplémentaire, à savoir `movieGenre`. Les premières lignes de code créent un `List` objet pour contenir les genres de film à partir de la base de données.

Le code suivant est une requête LINQ qui récupère tous les genres dans la base de données.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample16.vb)]

Le code utilise le `AddRange` méthode du générique `List` collection pour ajouter tous les genres distincts à la liste. (Sans le `Distinct` modificateur, genres en doublon seraient ajoutés, par exemple, comédie est ajoutée à deux reprises dans notre exemple). Le code stocke ensuite la liste des genres dans le `ViewBag` objet.

Le code suivant montre comment vérifier la `movieGenre` paramètre. S’il n’est pas vide le code plus contraint la requête de films pour limiter les films sélectionnés pour le genre spécifié.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample17.vb)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Ajout de balisage à la vue SearchIndex pour prendre en charge la recherche par Genre

Ajouter un `Html.DropDownList` helper pour le *Views\Movies\SearchIndex.vbhtml* de fichiers, juste avant la `TextBox` helper. Le balisage complet est indiqué ci-dessous :

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.vbhtml)]

Exécutez l’application et accédez à */Movies/SearchIndex*. Essayez une recherche par genre, par le nom du film et par les deux critères.

Dans cette section, vous avez examiné les méthodes d’action CRUD et les vues générées par le framework. Vous avez créé une méthode d’action de recherche et de la vue qui permettre aux utilisateurs de rechercher par titre de film et genre. Dans la section suivante, vous explorerez comment ajouter une propriété à la `Movie` modèle et comment ajouter un initialiseur qui crée automatiquement une base de données de test.

> [!div class="step-by-step"]
> [Précédent](accessing-your-models-data-from-a-controller.md)
> [Suivant](adding-a-new-field.md)
