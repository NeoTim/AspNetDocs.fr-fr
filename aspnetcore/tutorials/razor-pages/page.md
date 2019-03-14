---
title: Pages Razor obtenues par génération de modèles automatique dans ASP.NET Core
author: rick-anderson
description: Décrit l’obtention de pages Razor par génération de modèles automatique.
ms.author: riande
ms.date: 12/4/2018
uid: tutorials/razor-pages/page
ms.openlocfilehash: ad87e3da72c3dd6adf8cf55d16da58fa47ed5542
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57055506"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a>Pages Razor obtenues par génération de modèles automatique dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce didacticiel décrit les pages Razor créées par génération de modèles automatique dans le didacticiel précédent.

[Affichez ou téléchargez](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) l’exemple.

## <a name="the-create-delete-details-and-edit-pages"></a>Pages Create, Delete, Details et Edit

Examinez le modèle de page *Pages/Movies/Index.cshtml.cs* :

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

Les pages Razor sont dérivées de `PageModel`. Par convention, la classe dérivée de `PageModel` s’appelle `<PageName>Model`. Le constructeur utilise l’[injection de dépendances](xref:fundamentals/dependency-injection) pour ajouter `RazorPagesMovieContext` à la page. Toutes les pages obtenues par génération de modèles automatique suivent ce modèle. Consultez [Code asynchrone](xref:data/ef-rp/intro#asynchronous-code) pour plus d’informations sur la programmation asynchrone avec Entity Framework.

Quand une requête est effectuée pour la page, la méthode `OnGetAsync` retourne une liste de films à la page Razor. `OnGetAsync` ou `OnGet` est appelé sur une page Razor pour initialiser l’état de la page. Dans ce cas, `OnGetAsync` obtient une liste de films et les affiche.

Si `OnGet` retourne `void` ou que `OnGetAsync` retourne `Task`, aucune méthode de retour n’est utilisée. Lorsque le type de retour est `IActionResult` ou `Task<IActionResult>`, une instruction de retour doit être spécifiée. Par exemple, la méthode *Pages/Movies/Create.cshtml.cs* `OnPostAsync` :

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> Examinez la page Razor *Pages/Movies/Index.cshtml* :

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor peut passer du HTML au C# ou à des balises spécifiques à Razor. Quand un symbole `@` est suivi d’un [mot clé réservé Razor](xref:mvc/views/razor#razor-reserved-keywords), il est converti en balise spécifique à Razor. Sinon, il est converti en C#.

La directive Razor `@page` transforme le fichier en une action MVC, ce qui lui permet de prendre en charge des requêtes. `@page` doit être la première directive Razor sur une page. `@page` est un exemple de conversion en balise spécifique à Razor. Pour plus d’informations, consultez [Syntaxe Razor](xref:mvc/views/razor#razor-syntax).

Examinez l’expression lambda utilisée dans le HTML Helper suivant :

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

Le HTML Helper `DisplayNameFor` inspecte la propriété `Title` référencée dans l’expression lambda pour déterminer le nom d’affichage. L’expression lambda est inspectée plutôt qu’évaluée. Cela signifie qu’il n’existe pas de violation d’accès quand `model`, `model.Movie` ou `model.Movie[0]` ont une valeur `null` ou vide. Quand l’expression lambda est évaluée (par exemple avec `@Html.DisplayFor(modelItem => item.Title)`), les valeurs de propriété du modèle sont évaluées.

<a name="md"></a>
### <a name="the-model-directive"></a>Directive @model 

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

La directive `@model` spécifie le type du modèle passé à la page Razor. Dans l’exemple précédent, la ligne `@model` rend la classe dérivée `PageModel` accessible à la page Razor. Le modèle est utilisé dans les [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` et `@Html.DisplayFor` de la page.

### <a name="the-layout-page"></a>La page de disposition

Sélectionnez les liens du menu (**RazorPagesMovie**, **Accueil** et **Confidentialité**). Chaque page affiche la même disposition de menu. La disposition du menu est implémentée dans le fichier *Pages/Shared/_Layout.cshtml*. Ouvrez le fichier *Pages/Shared/_Layout.cshtml*.

Les modèles de [disposition](xref:mvc/views/layout) vous permettent de spécifier la disposition du conteneur HTML de votre site dans un emplacement unique, puis de l’appliquer sur plusieurs pages de votre site. Recherchez la ligne `@RenderBody()`. `RenderBody` est un espace réservé dans lequel toutes les vues spécifiques à une page que vous créez s’affichent, *encapsulées* dans la page de disposition. Par exemple, si vous sélectionnez le lien **Confidentialité**, la vue **Pages/Privacy.cshtml** est restituée dans la méthode `RenderBody`.

<a name="vd"></a>
### <a name="viewdata-and-layout"></a>ViewData et disposition

Considérez le code suivant du fichier *Pages/Movies/Index.cshtml*:

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

Le code précédent en surbrillance est un exemple de passage de Razor au C#. Les caractères `{` et `}` délimitent un bloc de code C#.

La classe de base `PageModel` a une propriété de dictionnaire `ViewData` qui permet d’ajouter des données à passer à une vue. Vous pouvez ajouter des objets au dictionnaire `ViewData` à l’aide d’un modèle clé/valeur. Dans l’exemple précédent, la propriété « Title » est ajoutée au dictionnaire `ViewData`. 

La propriété « Title » est utilisée dans le fichier *Pages/Shared/_Layout.cshtml*. La balise suivante montre les premières lignes du fichier *_Layout.cshtml*.

<!-- we need a snapshot copy of layout because we are
changing in in the next step. 
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

La ligne `@*Markup removed for brevity.*@` est un commentaire Razor qui n’apparaît pas dans votre fichier de disposition. Contrairement aux commentaires HTML (`<!-- -->`), les commentaires Razor ne sont pas envoyés au client.

### <a name="update-the-layout"></a>Mettre à jour la disposition

Changez l’élément `<title>` dans le fichier *Pages/Shared/_Layout.cshtml* pour afficher **Movie** au lieu de **RazorPagesMovie**.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]


Recherchez l’élément anchor suivant dans le fichier *Pages/Shared/_Layout.cshtml*.

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

Remplacez l’élément précédent par le code suivant.

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

L’élément anchor précédent est un [Tag Helper](xref:mvc/views/tag-helpers/intro). Dans le cas présent, il s’agit du [Tag Helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper). L’attribut et la valeur du Tag Helper `asp-page="/Movies/Index"` créent un lien vers la page Razor `/Movies/Index`. La valeur de l’attribut `asp-area` est vide : la zone n’est donc pas utilisée dans le lien. Pour plus d’informations, consultez [Zones](xref:mvc/controllers/areas).

Enregistrez vos changements, puis testez l’application en cliquant sur le lien **RpMovie**. Consultez le fichier [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) dans GitHub si vous rencontrez des problèmes.

Testez les autres liens (**Home**, **RpMovie**, **Create**, **Edit** et **Delete**). Chaque page définit le titre, que vous pouvez voir dans l’onglet du navigateur. Quand vous définissez un signet pour une page, le titre est affecté au signet.

> [!NOTE]
> Vous ne pourrez peut-être pas entrer de virgules décimales dans le champ `Price`. Pour prendre en charge la [validation jQuery](https://jqueryvalidation.org/) pour les paramètres régionaux autres que l’anglais qui utilisent une virgule (« , ») comme décimale et des formats de date autres que l’anglais des États-Unis, vous devez effectuer des étapes pour localiser votre application. Consultez la page [GitHub problème 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) pour savoir comment ajouter une virgule décimale.

La propriété `Layout` est définie dans le fichier *Pages/_ViewStart.cshtml* :

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

Le code précédent définit le fichier de disposition *Pages/Shared/_Layout.cshtml* pour tous les fichiers Razor du dossier *Pages*. Pour plus d’informations, consultez [Disposition](xref:razor-pages/index#layout).

### <a name="the-create-page-model"></a>Le modèle de page Create

Examinez le modèle de page *Pages/Movies/Create.cshtml.cs* :

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

La méthode `OnGet` initialise l’état nécessaire pour la page. La page Create n’ayant aucun état à initialiser, `Page` est retourné. Ce tutoriel illustre plus loin l’initialisation de l’état par la méthode `OnGet`. La méthode `Page` crée un objet `PageResult` qui affiche la page *Create.cshtml*.

La propriété `Movie` utilise l’attribut `[BindProperty]` pour adhérer à la [liaison de données](xref:mvc/models/model-binding). Quand le formulaire Create publie les valeurs de formulaire, le runtime ASP.NET Core lie les valeurs publiées au modèle `Movie`.

La méthode `OnPostAsync` est exécutée quand la page publie les données de formulaire :

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

S’il existe des erreurs liées au modèle, le formulaire est réaffiché, ainsi que toutes les données de formulaire publiées. La plupart des erreurs de modèle peuvent être interceptées côté client avant la publication du formulaire. Voici un exemple d’erreur de modèle : la publication pour le champ de date d’une valeur qui ne peut pas être convertie en date. La validation côté client et la validation du modèle sont présentées plus loin dans le tutoriel.

S’il n’existe pas d’erreurs de modèle, les données sont enregistrées et le navigateur est redirigé vers la page Index.

### <a name="the-create-razor-page"></a>Page Razor Create

Examinez le fichier de la page Razor *Pages/Movies/Create.cshtml* :

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio affiche la balise `<form method="post">` dans une police différenciée en gras utilisée pour les Tag Helpers :

![Vue VS17 de la page Create.cshtml](page/_static/th.png)
<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Pour plus d’informations sur les Tag Helpers, comme `<form method="post">`, consultez [Tag Helpers dans ASP.NET Core](xref:mvc/views/tag-helpers/intro).

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

Visual Studio pour Mac affiche la balise `<form method="post">` dans une police différenciée en gras utilisée pour les Tag Helpers.

---  
<!-- End of VS tabs -->

L’élément `<form method="post">` est un [Tag Helper de formulaire](xref:mvc/views/working-with-forms#the-form-tag-helper). Le Tag Helper de formulaire inclut automatiquement un [jeton de protection contre les falsifications](xref:security/anti-request-forgery).

Le moteur de génération de modèles automatique crée le code Razor pour chaque champ du modèle (sauf l’ID) de la manière suivante :

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

Les [Tag Helpers de validation](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` et ` <span asp-validation-for`) affichent des erreurs de validation. La validation est traitée de manière plus détaillée plus loin dans cette série.

Le [Tag Helper d’étiquette](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) génère la légende de l’étiquette et l’attribut `for` pour la propriété `Title`.

Le [Tag Helper d’entrée](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) utilise les attributs [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) et produit les attributs HTML nécessaires à la validation jQuery côté client.

> [!div class="step-by-step"]
> [Précédent : Ajout d’un modèle](xref:tutorials/razor-pages/model)
> [Suivant : Base de données](xref:tutorials/razor-pages/sql)
