---
uid: mvc/overview/getting-started/introduction/adding-search
title: Recherche | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/17/2019
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: ada125c917656f3a83524ff39e53b4cfc041a497
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029696"
---
<a name="search"></a>Rechercher
====================

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>Ajout d’une méthode de recherche et de la vue de la recherche

Dans cette section, vous ajouterez des possibilités de recherche le `Index` méthode d’action qui vous permet de rechercher des films par genre ou le nom.

## <a name="prerequisites"></a>Prérequis

Pour faire correspondre des captures d’écran de cette section, vous devez exécuter l’application (F5) et ajouter les films suivantes à la base de données.

| Titre | Date de publication | Genre | Prix |
| ----- | ------------ | ----- | ----- |
| Ghostbusters | 6/8/1984 | Comédie | 6.99 |
| Ghostbusters II | 6/16/1989 | Comédie | 6.99 |
| Mondiale des singes | 3/27/1986 | Action | 5.99 |


## <a name="updating-the-index-form"></a>La mise à jour de la forme d’Index

Commencez par la mise à jour le `Index` méthode d’action à l’objet existant `MoviesController` classe. Voici le code :

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

La première ligne de la `Index` méthode crée les éléments suivants [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) requête pour sélectionner les films :

[!code-csharp[Main](adding-search/samples/sample2.cs)]

La requête est définie à ce stade, mais n’a pas encore été exécutée par rapport à la base de données.

Si le `searchString` paramètre contient une chaîne, la requête de films est modifiée pour filtrer sur la valeur de la chaîne de recherche, en utilisant le code suivant :

[!code-csharp[Main](adding-search/samples/sample3.cs)]

Le code `s => s.Title` ci-dessus est une [expression lambda](https://msdn.microsoft.com/library/bb397687.aspx). Les expressions lambda sont utilisées dans fondées sur une méthode [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) de requêtes en tant qu’arguments aux méthodes d’opérateur de requête standard telles que la [où](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) méthode utilisée dans le code ci-dessus. Requêtes LINQ ne sont pas exécutées quand elles sont définies ou quand elles sont modifiées en appelant une méthode telle que `Where` ou `OrderBy`. Au lieu de cela, l’exécution de requête est différée, ce qui signifie que l’évaluation d’une expression est retardée jusqu'à ce que sa valeur réalisée est une itération réelle ou le [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) méthode est appelée. Dans le `Search` exemple, la requête est exécutée dans le *Index.cshtml* vue. Pour plus d’informations sur l’exécution différée des requêtes, consultez [Exécution de requête](https://msdn.microsoft.com/library/bb738633.aspx).

> [!NOTE]
> Le [Contains](https://msdn.microsoft.com/library/bb155125.aspx) méthode est exécutée sur la base de données, pas le code c# ci-dessus. Sur la base de données, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) est mappé à [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), qui respecte la casse.

Maintenant vous pouvez mettre à jour le `Index` vue qui affiche le formulaire à l’utilisateur.

Exécutez l’application et accédez à */Movies/Index*. Ajoutez une chaîne de requête comme `?searchString=ghost` à l’URL. Les films filtrés sont affichés.

![SearchQryStr](adding-search/_static/image1.png)

Si vous modifiez la signature de la `Index` méthode d’avoir un paramètre nommé `id`, le `id` paramètre correspond à la `{id}` espace réservé pour la valeur par défaut achemine ensemble dans le *application\_Start RouteConfig.cs* fichier.

[!code-json[Main](adding-search/samples/sample4.json)]

La version d’origine `Index` méthode ressemble à ceci :

[!code-csharp[Main](adding-search/samples/sample5.cs)]

Modifié `Index` méthode se présente comme suit :

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

Vous pouvez maintenant passer le titre de la recherche en tant que données de routage (un segment de l’URL) et non pas en tant que valeur de chaîne de requête.

![](adding-search/_static/image2.png)

Cependant, vous ne pouvez pas attendre des utilisateurs qu’ils modifient l’URL à chaque fois qu’ils veulent rechercher un film. Vous allez donc maintenant ajouter une interface utilisateur pour les aider à filtrer les films. Si vous avez modifié la signature de la `Index` méthode pour tester comment passer le paramètre ID de la limite de l’itinéraire, modifiez-le pour que votre `Index` méthode prend un paramètre de chaîne nommé `searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

Ouvrez le *Views\Movies\Index.cshtml* de fichiers et juste après `@Html.ActionLink("Create New", "Create")`, ajoutez le balisage de formulaire mis en surbrillance ci-dessous :

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

Le `Html.BeginForm` helper crée une ouverture `<form>` balise. Le `Html.BeginForm` helper, le formulaire à publier vers lui-même lorsque l’utilisateur envoie le formulaire en cliquant sur le **filtre** bouton.

Visual Studio 2013 a une amélioration appréciable lors de l’affichage et modification des fichiers de vue. Lorsque vous exécutez l’application avec un fichier d’affichage ouvert, Visual Studio 2013 appelle la méthode d’action de contrôleur correct pour afficher la vue.

![](adding-search/_static/image3.png)

Avec la vue Index ouvert dans Visual Studio (comme indiqué dans l’image ci-dessus), appuyez sur CTRL F5 ou F5 pour exécuter l’application et essayez de rechercher un film.

![](adding-search/_static/image4.png)

Il existe aucune `HttpPost` surcharge de la `Index` (méthode). Vous n’en avez besoin, car la méthode ne change pas l’état de l’application, filtrage des données.

Vous pourriez ajouter la méthode `HttpPost Index` suivante. Dans ce cas, le demandeur d’action correspondrait à la `HttpPost Index` (méthode) et le `HttpPost Index` méthode s’exécuterait comme indiqué dans l’image ci-dessous.

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

Cependant, même si vous ajoutez cette version `HttpPost` de la méthode `Index`, il existe une limitation dans la façon dont tout ceci a été implémenté. Imaginez que vous voulez insérer un signet pour une recherche spécifique, ou que vous voulez envoyer un lien à vos amis sur lequel ils peuvent cliquer pour afficher la même liste filtrée de films. Notez que l’URL de la requête HTTP POST est identique à l’URL de la requête GET (localhost : XXXXX/Movies/Index) : il n’existe aucune information de recherche dans l’URL proprement dite. Droite désormais, les informations de chaîne de recherche sont envoyées au serveur en tant que valeur d’un champ de formulaire. Cela signifie que vous ne pouvez pas capturer ces informations de recherche pour créer un signet ou envoyer à vos amis dans une URL.

La solution consiste à utiliser une surcharge de `BeginForm` qui spécifie que la requête POST doit ajouter les informations de recherche à l’URL et qu’il doit être acheminé vers le `HttpGet` version de la `Index` (méthode). Remplacer l’existant sans paramètre `BeginForm` méthode avec le balisage suivant :

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

Maintenant lorsque vous soumettez une recherche, l’URL contient une chaîne de requête de recherche. La recherche accède également à la méthode d’action `HttpGet Index`, même si vous avez une méthode `HttpPost Index`.

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>Ajout de la recherche par Genre

Si vous avez ajouté le `HttpPost` version de la `Index` (méthode), supprimez maintenant.

Ensuite, vous allez ajouter une fonctionnalité pour permettre aux utilisateurs de rechercher des films par genre. Remplacez la méthode `Index` par le code suivant :

[!code-csharp[Main](adding-search/samples/sample11.cs)]

Cette version de la `Index` méthode prend un paramètre supplémentaire, à savoir `movieGenre`. Les premières lignes de code créent un `List` objet pour contenir les genres de film à partir de la base de données.

Le code suivant est une requête LINQ qui récupère tous les genres dans la base de données.

[!code-csharp[Main](adding-search/samples/sample12.cs)]

Le code utilise le `AddRange` méthode du générique `List` collection pour ajouter tous les genres distincts à la liste. (Sans le `Distinct` modificateur, genres en doublon seraient ajoutés, par exemple, comédie est ajoutée à deux reprises dans notre exemple). Le code stocke ensuite la liste des genres dans le `ViewBag.MovieGenre` objet. Stockage des données de catégorie (tel un film genres) en tant qu’un [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) de l’objet dans un `ViewBag`, puis accéder aux données de catégorie dans une zone de liste déroulante est une approche courante pour les applications MVC.

Le code suivant montre comment vérifier la `movieGenre` paramètre. S’il n’est pas vide, le code plus contraint la requête de films pour limiter les films sélectionnés pour le genre spécifié.

[!code-csharp[Main](adding-search/samples/sample13.cs)]

Comme indiqué précédemment, la requête n’est pas exécutée sur la base de données jusqu'à ce que la liste de films est itérée (ce qui se passe dans la vue, après le `Index` retourne de la méthode d’action).

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>Ajout de balisage à la vue Index pour prendre en charge la recherche par Genre

Ajouter un `Html.DropDownList` helper pour le *Views\Movies\Index.cshtml* de fichiers, juste avant la `TextBox` helper. Le balisage complet est indiqué ci-dessous :

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

Dans le code suivant :

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

Le paramètre « MovieGenre » fournit la clé pour le `DropDownList` helper pour rechercher un `IEnumerable<SelectListItem>` dans le `ViewBag`. Le `ViewBag` a été renseigné dans la méthode d’action :

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

Le paramètre « All » fournit une étiquette d’option. Si vous examinez ce choix dans votre navigateur, vous verrez que son attribut de « valeur » est vide. Étant donné que notre contrôleur filtre uniquement `if` la chaîne n’est pas `null` ou est vide, l’envoi d’une valeur vide pour `movieGenre` montre tous les genres.

Vous pouvez également définir une option pour être sélectionnée par défaut. Si vous souhaitiez « Comédie » comme option de valeur par défaut, vous modifieriez le code dans le contrôleur comme suit :

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

Exécutez l’application et accédez à */Movies/Index*. Essayez une recherche par genre, par le nom du film et par les deux critères.

![](adding-search/_static/image8.png)

Dans cette section, vous avez créé une méthode d’action de recherche et de la vue qui permettre aux utilisateurs de rechercher par titre de film et genre. Dans la section suivante, vous explorerez comment ajouter une propriété à la `Movie` modèle et comment ajouter un initialiseur qui crée automatiquement une base de données de test.

> [!div class="step-by-step"]
> [Précédent](examining-the-edit-methods-and-edit-view.md)
> [Suivant](adding-a-new-field.md)
