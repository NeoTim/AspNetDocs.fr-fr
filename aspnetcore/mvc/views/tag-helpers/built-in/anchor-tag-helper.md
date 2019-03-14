---
title: Tag Helper Ancre dans ASP.NET Core
author: pkellner
description: Découvrez les attributs ASP.NET Core Tag Helper Ancre et le rôle joué par chaque attribut dans l’extension du comportement de la balise d’ancrage HTML.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/18/2018
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 60fa0c00e40878a8227ca2bc8bdb0bc2bf9f8336
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57033126"
---
# <a name="anchor-tag-helper-in-aspnet-core"></a>Tag Helper Ancre dans ASP.NET Core

Par [Peter Kellner](http://peterkellner.net) et [Scott Addie](https://github.com/scottaddie)

Le [Tag Helper Ancre](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper) améliore la balise d’ancrage HTML standard (`<a ... ></a>`) en ajoutant de nouveaux attributs. Par convention, les noms d’attribut commencent par `asp-`. La valeur d’attribut de l’élément d’ancrage rendu `href` est déterminée par les valeurs des attributs `asp-`.

Pour avoir une vue d’ensemble des Tag Helpers, consultez <xref:mvc/views/tag-helpers/intro>.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

*SpeakerController* est utilisé dans les exemples dans ce document :

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

Un inventaire des attributs `asp-` vient ensuite.

## <a name="asp-controller"></a>asp-controller

L’attribut [asp-controller](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Controller*) assigne le contrôleur utilisé pour générer l’URL. Le balisage suivant répertorie tous les présentateurs :

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

Code HTML généré :

```html
<a href="/Speaker">All Speakers</a>
```

Si l’attribut `asp-controller` est spécifié et que `asp-action` ne l’est pas, la valeur par défaut `asp-action` est l’action du contrôleur associée à la vue en cours d’exécution. Si `asp-action` est omis du balisage précédent, et le Tag Helper Ancre est utilisé dans la vue *Index* de *HomeController* (*/Home*), le code HTML généré est :

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a>asp-action

La valeur de l’attribut [asp-action](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Action*) représente le nom d’action du contrôleur inclus dans l’attribut `href` généré. Le balisage suivant définit la valeur de l’attribut `href` généré sur la page d’évaluations du présentateur :

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

Code HTML généré :

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Si aucun attribut `asp-controller` n’est spécifié, le contrôleur par défaut qui appelle la vue exécutant la vue actuelle est utilisé.

Si la valeur de l’attribut `asp-action` est `Index`, aucune action n’est ajoutée à l’URL, ce qui aboutit à l’appel de l’action `Index` par défaut. L’action spécifiée (ou par défaut) doit exister dans le contrôleur référencé dans `asp-controller`.

## <a name="asp-route-value"></a>asp-route-{value}

L’attribut [asp-route-{value}](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.RouteValues*) active un préfixe d’itinéraire générique. Toute valeur occupant l’espace réservé `{value}` est interprétée comme un paramètre d’itinéraire potentiel. Si aucune route par défaut n’est trouvée, ce préfixe de routage est ajouté à l’attribut `href` généré en tant que valeur et paramètre de requête. Dans le cas contraire, il est remplacé dans le modèle de routage.

Examinons l’action du contrôleur ci-dessous :

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

Avec un modèle d’itinéraire par défaut défini dans *Startup.Configure* :

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

La vue MVC utilise le modèle, fourni par l’action, comme suit :

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

L’espace réservé `{id?}` de l’itinéraire par défaut a été mis en correspondance. Code HTML généré :

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

Supposons que le préfixe d’itinéraire ne fait pas partie du modèle de routage correspondant, comme dans la vue MVC suivante :

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail"
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

Le code HTML suivant est généré, car `speakerid` n’a pas été trouvé dans l’itinéraire correspondant :

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

Si `asp-controller` ou `asp-action` ne sont pas spécifiés, le même traitement par défaut est appliqué que dans l’attribut `asp-route`.

## <a name="asp-route"></a>asp-route

L’attribut [asp-route](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Route*) est utilisé pour créer une URL reliant directement à un itinéraire nommé. À l’aide des [attributs de routage](xref:mvc/controllers/routing#attribute-routing), un itinéraire peut être nommé comme indiqué dans `SpeakerController` et utilisé dans son action `Evaluations` :

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

Dans le balisage suivant, l’attribut `asp-route` fait référence à l’itinéraire nommé :

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

Le Tag Helper Ancre génère un itinéraire directement vers cette action de contrôleur à l’aide de l’URL */Speaker/Evaluations*. Code HTML généré :

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Si `asp-controller` ou `asp-action` est spécifié en plus de `asp-route`, la route générée ne correspond peut-être pas à ce que vous attendez. `asp-route` ne doit pas être utilisé avec les attributs `asp-controller` ou `asp-action` afin d’éviter un conflit de routage.

## <a name="asp-all-route-data"></a>asp-all-route-data

L’attribut [asp-all-route-data](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.RouteValues*) prend en charge la création d’un dictionnaire de paires clé-valeur. La clé est le nom du paramètre et la valeur est la valeur du paramètre.

Dans l’exemple suivant, un dictionnaire est initialisé et transmis à une vue Razor. Les données peuvent également être transmises avec votre modèle.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

Le code précédent génère le code HTML suivant :

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

Le dictionnaire `asp-all-route-data` est aplati pour produire une chaîne de requête conforme aux exigences de l’action `Evaluations` surchargée :

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

Si des clés dans le dictionnaire correspondent aux paramètres d’itinéraire, ces valeurs sont substituées dans l’itinéraire comme il convient. Les autres valeurs non correspondantes sont générées en tant que paramètres de la requête.

## <a name="asp-fragment"></a>asp-fragment

L’attribut [asp-fragment](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Fragment*) définit un fragment d’URL à ajouter à l’URL. Le Tag Helper Ancre ajoute le caractère de hachage (#). Examinons le balisage suivant :

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

Code HTML généré :

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

Les balises de hachage sont utiles lors de la création des applications côté client. Elles peuvent servir à faciliter le marquage et la recherche en JavaScript, par exemple.

## <a name="asp-area"></a>asp-area

L’attribut [asp-area](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Area*) définit le nom de la zone utilisé pour définir l’itinéraire approprié. Les exemples suivants décrivent la façon dont l’attribut `asp-area` entraîne un remappage des itinéraires.

### <a name="usage-in-razor-pages"></a>Utilisation dans les pages Razor

Les zones de pages Razor sont prises en charge dans ASP.NET Core 2.1 ou les versions ultérieures.

Considérez la hiérarchie de répertoires suivante :

* **{Nom du projet}**
  * **wwwroot**
  * **Les zones (areas)**
    * **Sessions**
      * **Pages**
        * *\_ViewStart.cshtml*
        * *Index.cshtml*
        * *Index.cshtml.cs*
  * **Pages**

Le balisage pour faire référence à la zone *Sessions* de la page Razor *Index* est :

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAreaRazorPages)]

Code HTML généré :

```html
<a href="/Sessions">View Sessions</a>
```

> [!TIP]
> Pour prendre en charge des zones dans une application Razor Pages, effectuez l’une des opérations suivantes dans `Startup.ConfigureServices`:
>
> * Définir la [version de compatibilité](xref:mvc/compatibility-version) sur 2.1 ou au-dessus.
> * Définir la propriété [RazorPagesOptions.AllowAreas](xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.AllowAreas*) sur `true`:
>
>   [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_AllowAreas)]

### <a name="usage-in-mvc"></a>Utilisation dans MVC

Considérez la hiérarchie de répertoires suivante :

* **{Nom du projet}**
  * **wwwroot**
  * **Les zones (areas)**
    * **Blogs**
      * **Contrôleurs**
        * *HomeController.cs*
      * **Vues**
        * **Accueil**
          * *AboutBlog.cshtml*
          * *Index.cshtml*
        * *\_ViewStart.cshtml*
  * **Contrôleurs**

L’affectation de la valeur « Blogs » à `asp-area` préfixe le répertoire *Areas/Blogs* dans les itinéraires des vues et contrôleurs associés pour cette balise d’ancrage. Le balisage pour faire référence à la vue *AboutBlog* est :

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

Code HTML généré :

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> Pour prendre en charge les zones dans une application MVC, le modèle de routage doit inclure une référence à la zone si elle existe. Ce modèle est représenté par le deuxième paramètre de l’appel de méthode `routes.MapRoute` dans *Startup.Configure* :
>
> [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a>asp-protocol

L’attribut [asp-protocol](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Protocol*) permet de spécifier un protocole (tel que `https`) dans l’URL. Exemple :

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

Code HTML généré :

```html
<a href="https://localhost/Home/About">About</a>
```

Le nom d’hôte dans l’exemple est localhost. Le Tag Helper Ancre utilise le domaine public du site web lors de la génération de l’URL.

## <a name="asp-host"></a>asp-host

L’attribut [asp-host](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Host*) est destiné à spécifier un nom d’hôte dans votre URL. Exemple :

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

Code HTML généré :

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a>asp-page

L’attribut [asp-page](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Page*) est utilisé avec les Pages Razor. Utilisez-le pour définir la valeur d’attribut `href` d’une balise d’ancrage sur une page spécifique. En ajoutant une barre oblique (« / ») comme préfixe au nom de la page, vous créez l’URL.

L’exemple suivant pointe vers la Page Razor du participant :

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

Code HTML généré :

```html
<a href="/Attendee">All Attendees</a>
```

L’attribut `asp-page` et les attributs `asp-route`, `asp-controller` et `asp-action` s’excluent mutuellement. Toutefois, `asp-page` peut être utilisé avec `asp-route-{value}` pour contrôler le routage, comme illustré dans le balisage suivant :

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

Code HTML généré :

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a>asp-page-handler

L’attribut [asp-page-handler](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.PageHandler*) est utilisé avec les Pages Razor. Il est destiné à la liaison à des gestionnaires de page spécifiques.

Prenons le gestionnaire de page suivant :

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

Le balisage associé au modèle de page est lié au gestionnaire de page `OnGetProfile`. Notez que le préfixe `On<Verb>` du nom de la méthode du gestionnaire de page est omis dans la valeur d’attribut `asp-page-handler`. Lorsque la méthode est asynchrone, le suffixe `Async` est également omis.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

Code HTML généré :

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
