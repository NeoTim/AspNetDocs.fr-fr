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
ms.openlocfilehash: f6d6d32a648fed453be924790a1b55698c9cf209
ms.sourcegitcommit: 0d583ed9253103f3e50b6d729276e667591cdd41
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86211475"
---
# <a name="search"></a>Recherche

[!INCLUDE [Tutorial Note](index.md)]

## <a name="adding-a-search-method-and-search-view"></a>Ajout d’une méthode de recherche et d’un affichage de recherche

Dans cette section, vous allez ajouter la fonctionnalité de recherche à la `Index` méthode d’action qui vous permet de rechercher des films par genre ou par nom.

## <a name="prerequisites"></a>Prérequis

Pour faire correspondre les captures d’écran de cette section, vous devez exécuter l’application (F5) et ajouter les films suivants à la base de données.

| Titre | Date de sortie | Genre | Prix |
| ----- | ------------ | ----- | ----- |
| Ghostbusters | 6/8/1984 | Comédie | 6,99 |
| Ghostbusters II | 6/16/1989 | Comédie | 6,99 |
| Planète du singes | 3/27/1986 | Action | 5,99 |

## <a name="updating-the-index-form"></a>Mise à jour du formulaire d’index

Commencez par mettre à jour la `Index` méthode d’action à la `MoviesController` classe existante. Voici le code :

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

La première ligne de la `Index` méthode crée la requête [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) suivante pour sélectionner les films :

[!code-csharp[Main](adding-search/samples/sample2.cs)]

La requête est définie à ce stade, mais elle n’a pas encore été exécutée sur la base de données.

Si le `searchString` paramètre contient une chaîne, la requête de films est modifiée pour filtrer sur la valeur de la chaîne de recherche, à l’aide du code suivant :

[!code-csharp[Main](adding-search/samples/sample3.cs)]

Le code `s => s.Title` ci-dessus est une [expression lambda](https://msdn.microsoft.com/library/bb397687.aspx). Les expressions lambda sont utilisées dans les requêtes [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) basées sur une méthode en tant qu’arguments pour les méthodes d’opérateur de requête standard, telles que la méthode [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) utilisée dans le code ci-dessus. Les requêtes LINQ ne sont pas exécutées quand elles sont définies ou quand elles sont modifiées en appelant une méthode telle que `Where` ou `OrderBy` . Au lieu de cela, l’exécution de la requête est différée, ce qui signifie que l’évaluation d’une expression est retardée jusqu’à ce que sa valeur réalisée soit réellement itérée ou que la [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) méthode soit appelée. Dans l' `Search` exemple, la requête est exécutée dans la vue *index. cshtml* . Pour plus d’informations sur l’exécution différée des requêtes, consultez [Exécution de requête](https://msdn.microsoft.com/library/bb738633.aspx).

> [!NOTE]
> La méthode [Contains](https://msdn.microsoft.com/library/bb155125.aspx) est exécutée sur la base de données, et non pas dans le code c# ci-dessus. Sur la base de données, [contient](https://msdn.microsoft.com/library/bb155125.aspx) des mappages à [SQL comme](https://msdn.microsoft.com/library/ms179859.aspx), ce qui ne respecte pas la casse.

À présent, vous pouvez mettre à jour la `Index` vue qui affichera le formulaire pour l’utilisateur.

Exécutez l’application et accédez à */movies/index*. Ajoutez une chaîne de requête comme `?searchString=ghost` à l’URL. Les films filtrés sont affichés.

![SearchQryStr](adding-search/_static/image1.png)

Si vous modifiez la signature de la `Index` méthode pour qu’elle ait un paramètre nommé `id` , le `id` paramètre correspondra à l' `{id}` espace réservé pour les itinéraires par défaut définis dans le fichier *app \_ Start\RouteConfig.cs* .

[!code-json[Main](adding-search/samples/sample4.json)]

La méthode d’origine se `Index` présente comme suit :

[!code-csharp[Main](adding-search/samples/sample5.cs)]

La méthode modifiée se `Index` présente comme suit :

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

Vous pouvez maintenant passer le titre de la recherche en tant que données de routage (un segment de l’URL) et non pas en tant que valeur de chaîne de requête.

![](adding-search/_static/image2.png)

Cependant, vous ne pouvez pas attendre des utilisateurs qu’ils modifient l’URL à chaque fois qu’ils veulent rechercher un film. Vous allez donc maintenant ajouter une interface utilisateur pour les aider à filtrer les films. Si vous avez modifié la signature de la `Index` méthode pour tester la manière de passer le paramètre d’ID lié à l’itinéraire, modifiez-le de façon à ce que votre `Index` méthode prenne un paramètre de chaîne nommé `searchString` :

[!code-csharp[Main](adding-search/samples/sample7.cs)]

Ouvrez le fichier *Views\Movies\Index.cshtml* et juste après `@Html.ActionLink("Create New", "Create")` , ajoutez le balisage de formulaire en surbrillance ci-dessous :

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

L' `Html.BeginForm` application auxiliaire crée une `<form>` balise d’ouverture. L' `Html.BeginForm` application d’assistance fait en sorte que le formulaire publie sur lui-même lorsque l’utilisateur envoie le formulaire en cliquant sur le bouton de **filtre** .

Visual Studio 2013 a une amélioration intéressante lors de l’affichage et de la modification des fichiers de vue. Quand vous exécutez l’application avec un fichier de vue ouvert, Visual Studio 2013 appelle la méthode d’action de contrôleur appropriée pour afficher la vue.

![](adding-search/_static/image3.png)

Avec la vue d’index ouverte dans Visual Studio (comme indiqué dans l’image ci-dessus), appuyez sur CTR F5 ou F5 pour exécuter l’application, puis essayez de rechercher un film.

![](adding-search/_static/image4.png)

Il n’y a aucune `HttpPost` surcharge de la `Index` méthode. Vous n’en avez pas besoin, car la méthode ne modifie pas l’état de l’application, il suffit de filtrer les données.

Vous pourriez ajouter la méthode `HttpPost Index` suivante. Dans ce cas, le demandeur d’action correspondrait à la `HttpPost Index` méthode et la `HttpPost Index` méthode s’exécuterait comme indiqué dans l’image ci-dessous.

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

Cependant, même si vous ajoutez cette version `HttpPost` de la méthode `Index`, il existe une limitation dans la façon dont tout ceci a été implémenté. Imaginez que vous voulez insérer un signet pour une recherche spécifique, ou que vous voulez envoyer un lien à vos amis sur lequel ils peuvent cliquer pour afficher la même liste filtrée de films. Notez que l’URL de la requête HTTP POSTÉRIEURe est identique à l’URL de la demande d’extraction (localhost : xxxxx/movies/index)--il n’y a pas d’informations de recherche dans l’URL proprement dite. Pour le moment, les informations de la chaîne de recherche sont envoyées au serveur en tant que valeur de champ de formulaire. Cela signifie que vous ne pouvez pas capturer les informations de recherche dans un signet ou les envoyer à des amis dans une URL.

La solution consiste à utiliser une surcharge de `BeginForm` qui spécifie que la requête de publication doit ajouter les informations de recherche à l’URL et qu’elle doit être routée vers la `HttpGet` version de la `Index` méthode. Remplacez la méthode sans paramètre existante `BeginForm` par le balisage suivant :

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

Désormais, lorsque vous envoyez une recherche, l’URL contient une chaîne de requête de recherche. La recherche accède également à la méthode d’action `HttpGet Index`, même si vous avez une méthode `HttpPost Index`.

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>Ajout de la recherche par genre

Si vous avez ajouté la `HttpPost` version de la `Index` méthode, supprimez-la maintenant.

Ensuite, vous allez ajouter une fonctionnalité pour permettre aux utilisateurs de rechercher des films par genre. Remplacez la méthode `Index` par le code suivant :

[!code-csharp[Main](adding-search/samples/sample11.cs)]

Cette version de la `Index` méthode prend un paramètre supplémentaire, à savoir `movieGenre` . Les premières lignes de code créent un `List` objet pour stocker les genres de films à partir de la base de données.

Le code suivant est une requête LINQ qui récupère tous les genres dans la base de données.

[!code-csharp[Main](adding-search/samples/sample12.cs)]

Le code utilise la `AddRange` méthode de la `List` collection générique pour ajouter tous les genres distincts à la liste. (Sans le `Distinct` modificateur, les genres en double seraient ajoutés, par exemple, comédie serait ajouté deux fois dans notre exemple). Le code stocke ensuite la liste des genres dans l' `ViewBag.MovieGenre` objet. Le stockage des données de catégorie (telles que les genres de films) en tant qu’objet [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) dans un `ViewBag` , l’accès aux données de catégorie dans une zone de liste déroulante est une approche typique pour les applications MVC.

Le code suivant montre comment vérifier le `movieGenre` paramètre. S’il n’est pas vide, le code limite davantage la requête de films pour limiter les films sélectionnés au genre spécifié.

[!code-csharp[Main](adding-search/samples/sample13.cs)]

Comme indiqué précédemment, la requête n’est pas exécutée sur la base de données tant que la liste de films n’a pas été itérée (ce qui se produit dans la vue, après le retour de la `Index` méthode d’action).

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>Ajout de balises à la vue d’index pour prendre en charge la recherche par genre

Ajoutez une `Html.DropDownList` application d’assistance au fichier *Views\Movies\Index.cshtml* , juste avant l' `TextBox` application auxiliaire. Le balisage terminé est illustré ci-dessous :

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

Dans le code suivant :

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

Le paramètre « MovieGenre » fournit la clé pour que l' `DropDownList` application d’assistance trouve un `IEnumerable<SelectListItem>` dans le `ViewBag` . Le `ViewBag` a été rempli dans la méthode d’action :

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

Le paramètre « All » fournit une étiquette d’option. Si vous examinez ce choix dans votre navigateur, vous verrez que son attribut « value » est vide. Étant donné que notre contrôleur filtre uniquement `if` la chaîne n’est pas `null` ou vide, l’envoi d’une valeur vide pour `movieGenre` affiche tous les genres.

Vous pouvez également définir une option à sélectionner par défaut. Si vous souhaitez utiliser « comédie » comme option par défaut, vous devez modifier le code du contrôleur de la manière suivante :

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

Exécutez l’application et accédez à */movies/index*. Essayez d’effectuer une recherche par genre, par nom de film et les deux critères.

![](adding-search/_static/image8.png)

Dans cette section, vous avez créé une méthode et une vue d’action de recherche qui permettent aux utilisateurs de rechercher par titre et par genre de films. Dans la section suivante, vous allez examiner comment ajouter une propriété au `Movie` modèle et comment ajouter un initialiseur qui créera automatiquement une base de données de test.

> [!div class="step-by-step"]
> [Précédent](examining-the-edit-methods-and-edit-view.md) 
>  [Suivant](adding-a-new-field.md)
