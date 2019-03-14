---
title: Disposition dans ASP.NET Core
author: ardalis
description: Apprenez à utiliser des dispositions communes, à partager des directives et à exécuter le code commun avant d’afficher les vues dans une application ASP.NET Core.
ms.author: riande
ms.date: 02/26/2019
uid: mvc/views/layout
ms.openlocfilehash: 7a60ee15e688d6f0e531302457604fa759213758
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57036086"
---
# <a name="layout-in-aspnet-core"></a><span data-ttu-id="6fb73-103">Disposition dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6fb73-103">Layout in ASP.NET Core</span></span>

<span data-ttu-id="6fb73-104">Article rédigé par [Steve Smith](https://ardalis.com/) et [Dave Brock](https://twitter.com/daveabrock)</span><span class="sxs-lookup"><span data-stu-id="6fb73-104">By [Steve Smith](https://ardalis.com/) and [Dave Brock](https://twitter.com/daveabrock)</span></span>

<span data-ttu-id="6fb73-105">Les pages et les vues ont souvent des éléments visuels et programmatiques en commun.</span><span class="sxs-lookup"><span data-stu-id="6fb73-105">Pages and views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="6fb73-106">Cet article montre comment :</span><span class="sxs-lookup"><span data-stu-id="6fb73-106">This article demonstrates how to:</span></span>

* <span data-ttu-id="6fb73-107">Utiliser des dispositions communes.</span><span class="sxs-lookup"><span data-stu-id="6fb73-107">Use common layouts.</span></span>
* <span data-ttu-id="6fb73-108">Partager des directives.</span><span class="sxs-lookup"><span data-stu-id="6fb73-108">Share directives.</span></span>
* <span data-ttu-id="6fb73-109">Exécuter du code commun avant d’afficher des pages ou des vues.</span><span class="sxs-lookup"><span data-stu-id="6fb73-109">Run common code before rendering pages or views.</span></span>

<span data-ttu-id="6fb73-110">Ce document traite des dispositions pour les deux approches différentes d’ASP.NET Core MVC : Razor Pages et les contrôleurs avec vues.</span><span class="sxs-lookup"><span data-stu-id="6fb73-110">This document discusses layouts for the two different approaches to ASP.NET Core MVC: Razor Pages and controllers with views.</span></span> <span data-ttu-id="6fb73-111">Pour cette rubrique, les différences sont minimes :</span><span class="sxs-lookup"><span data-stu-id="6fb73-111">For this topic, the differences are minimal:</span></span>

* <span data-ttu-id="6fb73-112">Razor Pages se trouve dans le dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="6fb73-112">Razor Pages are in the *Pages* folder.</span></span>
* <span data-ttu-id="6fb73-113">Les contrôleurs avec vues utilisent un dossier *Views* pour les vues.</span><span class="sxs-lookup"><span data-stu-id="6fb73-113">Controllers with views uses a *Views* folder for views.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="6fb73-114">Qu’est-ce qu’une disposition ?</span><span class="sxs-lookup"><span data-stu-id="6fb73-114">What is a Layout</span></span>

<span data-ttu-id="6fb73-115">La plupart des applications web ont une disposition commune pour offrir aux utilisateurs une expérience homogène quand ils naviguent de page en page.</span><span class="sxs-lookup"><span data-stu-id="6fb73-115">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="6fb73-116">En général, la disposition inclut des éléments d’interface utilisateur communs à toute l’application, tels que l’en-tête, des éléments de menu ou de navigation et le pied de page.</span><span class="sxs-lookup"><span data-stu-id="6fb73-116">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Exemple de disposition de page](layout/_static/page-layout.png)

<span data-ttu-id="6fb73-118">Les structures HTML communes comme les scripts et les feuilles de style sont aussi fréquemment utilisées par bon nombre de pages dans une application.</span><span class="sxs-lookup"><span data-stu-id="6fb73-118">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="6fb73-119">Tous ces éléments partagés peuvent être définis dans un fichier de *disposition*, qui peut ensuite être référencé par n’importe quelle vue utilisée dans l’application.</span><span class="sxs-lookup"><span data-stu-id="6fb73-119">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="6fb73-120">Les dispositions réduisent les doublons de code dans les vues.</span><span class="sxs-lookup"><span data-stu-id="6fb73-120">Layouts reduce duplicate code in views.</span></span>

<span data-ttu-id="6fb73-121">Par convention, la disposition par défaut d’une application ASP.NET Core se nomme *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6fb73-121">By convention, the default layout for an ASP.NET Core app is named *_Layout.cshtml*.</span></span> <span data-ttu-id="6fb73-122">Le fichier de disposition pour les projets ASP.NET Core créés avec les modèles :</span><span class="sxs-lookup"><span data-stu-id="6fb73-122">The layout file for new ASP.NET Core projects created with the templates:</span></span>

* <span data-ttu-id="6fb73-123">Pages Razor : *Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6fb73-123">Razor Pages: *Pages/Shared/_Layout.cshtml*</span></span>

  ![Dossier Pages dans l’Explorateur de solutions](layout/_static/rp-web-project-views.png)

* <span data-ttu-id="6fb73-125">Contrôleur avec vues : *Views/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6fb73-125">Controller with views: *Views/Shared/_Layout.cshtml*</span></span>

 ![Dossier Vues dans l’Explorateur de solutions](layout/_static/mvc-web-project-views.png)

<span data-ttu-id="6fb73-127">La disposition définit un modèle général pour les vues dans l’application.</span><span class="sxs-lookup"><span data-stu-id="6fb73-127">The layout defines a top level template for views in the app.</span></span> <span data-ttu-id="6fb73-128">Les applications ne nécessitent pas de disposition.</span><span class="sxs-lookup"><span data-stu-id="6fb73-128">Apps don't require a layout.</span></span> <span data-ttu-id="6fb73-129">Les applications peuvent définir plusieurs dispositions, avec des vues différentes spécifiant des dispositions différentes.</span><span class="sxs-lookup"><span data-stu-id="6fb73-129">Apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="6fb73-130">Le code suivant montre le fichier de disposition pour un projet créé avec un modèle, avec un contrôleur et des vues :</span><span class="sxs-lookup"><span data-stu-id="6fb73-130">The following code shows the layout file for a template created project with a controller and views:</span></span>

[!code-cshtml[](~/common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=44,72)]

## <a name="specifying-a-layout"></a><span data-ttu-id="6fb73-131">Spécification d’une disposition</span><span class="sxs-lookup"><span data-stu-id="6fb73-131">Specifying a Layout</span></span>

<span data-ttu-id="6fb73-132">Les vues Razor ont une propriété `Layout`.</span><span class="sxs-lookup"><span data-stu-id="6fb73-132">Razor views have a `Layout` property.</span></span> <span data-ttu-id="6fb73-133">Chaque vue spécifie une disposition en définissant cette propriété :</span><span class="sxs-lookup"><span data-stu-id="6fb73-133">Individual views specify a layout by setting this property:</span></span>

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="6fb73-134">La disposition spécifiée peut utiliser un chemin complet (par exemple, */Pages/Shared/_Layout.cshtml* ou */Views/Shared/_Layout.cshtml*) ou un nom partiel (exemple : `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="6fb73-134">The layout specified can use a full path (for example, */Pages/Shared/_Layout.cshtml* or */Views/Shared/_Layout.cshtml*) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="6fb73-135">Quand un nom partiel est fourni, le moteur de vue Razor recherche le fichier de disposition en utilisant son processus de détection habituel.</span><span class="sxs-lookup"><span data-stu-id="6fb73-135">When a partial name is provided, the Razor view engine searches for the layout file using its standard discovery process.</span></span> <span data-ttu-id="6fb73-136">Le dossier où se trouve la méthode de gestionnaire (ou contrôleur) est parcouru en premier, suivi du dossier *Shared*.</span><span class="sxs-lookup"><span data-stu-id="6fb73-136">The folder where the handler method (or controller) exists is searched first, followed by the *Shared* folder.</span></span> <span data-ttu-id="6fb73-137">Ce processus de détection est le même que celui utilisé pour détecter les [vues partielles](xref:mvc/views/partial#partial-view-discovery).</span><span class="sxs-lookup"><span data-stu-id="6fb73-137">This discovery process is identical to the process used to discover [partial views](xref:mvc/views/partial#partial-view-discovery).</span></span>

<span data-ttu-id="6fb73-138">Par défaut, chaque disposition doit appeler `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="6fb73-138">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="6fb73-139">À chaque appel de `RenderBody`, le contenu de la vue est affiché.</span><span class="sxs-lookup"><span data-stu-id="6fb73-139">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="6fb73-140">Sections</span><span class="sxs-lookup"><span data-stu-id="6fb73-140">Sections</span></span>

<span data-ttu-id="6fb73-141">Une disposition peut éventuellement faire référence à une ou plusieurs *sections*, en appelant `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="6fb73-141">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="6fb73-142">Les sections sont un moyen d’organiser certains éléments dans la page.</span><span class="sxs-lookup"><span data-stu-id="6fb73-142">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="6fb73-143">Chaque appel à `RenderSection` peut spécifier si cette section est obligatoire ou facultative :</span><span class="sxs-lookup"><span data-stu-id="6fb73-143">Each call to `RenderSection` can specify whether that section is required or optional:</span></span>

```html
@section Scripts {
    @RenderSection("Scripts", required: false)
}
```

<span data-ttu-id="6fb73-144">Si une section obligatoire est introuvable, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="6fb73-144">If a required section isn't found, an exception is thrown.</span></span> <span data-ttu-id="6fb73-145">Chacune des vues spécifient le contenu à afficher dans une section à l’aide de la syntaxe Razor `@section`.</span><span class="sxs-lookup"><span data-stu-id="6fb73-145">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="6fb73-146">Si une page ou vue définit une section, elle doit être affichée (sinon, une erreur se produit).</span><span class="sxs-lookup"><span data-stu-id="6fb73-146">If a page or view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="6fb73-147">Exemple de définition `@section` dans une vue Razor Pages :</span><span class="sxs-lookup"><span data-stu-id="6fb73-147">An example `@section` definition in Razor Pages view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
}
```

<span data-ttu-id="6fb73-148">Dans le code précédent, *scripts/main.js* est ajouté à la section `scripts` sur une page ou vue.</span><span class="sxs-lookup"><span data-stu-id="6fb73-148">In the preceding code, *scripts/main.js* is added to the `scripts` section on a page or view.</span></span> <span data-ttu-id="6fb73-149">Il est possible que les autres pages ou vues de la même application ne nécessitent pas ce script et ne définissent pas de section de scripts.</span><span class="sxs-lookup"><span data-stu-id="6fb73-149">Other pages or views in the same app might not require this script and wouldn't define a scripts section.</span></span>

<span data-ttu-id="6fb73-150">Le balisage suivant utilise le [Tag Helper Partial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) pour afficher *_ValidationScriptsPartial.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6fb73-150">The following markup uses the [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) to render  *_ValidationScriptsPartial.cshtml*:</span></span>

```html
@section Scripts {
    <partial name="_ValidationScriptsPartial" />
}
```

<span data-ttu-id="6fb73-151">Le balisage précédent était le résultat de la [génération d’un modèle automatique d’identité](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="6fb73-151">The preceding markup was generated by [scaffolding Identity](xref:security/authentication/scaffold-identity).</span></span>

<span data-ttu-id="6fb73-152">Les sections définies dans une page ou vue sont disponibles uniquement dans sa page de disposition la plus proche.</span><span class="sxs-lookup"><span data-stu-id="6fb73-152">Sections defined in a page or view are available only in its immediate layout page.</span></span> <span data-ttu-id="6fb73-153">Elles ne peuvent pas être référencées à partir de vues partielles, de composants de vue ou d’autres parties du système de vue.</span><span class="sxs-lookup"><span data-stu-id="6fb73-153">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="6fb73-154">Ignorer des sections</span><span class="sxs-lookup"><span data-stu-id="6fb73-154">Ignoring sections</span></span>

<span data-ttu-id="6fb73-155">Par défaut, le corps et toutes les sections dans une page de contenu doivent intégralement être affichés par la page de disposition.</span><span class="sxs-lookup"><span data-stu-id="6fb73-155">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="6fb73-156">Le moteur de vue Razor s’assure que c’est bien le cas en vérifiant que le corps et chaque section ont été affichés.</span><span class="sxs-lookup"><span data-stu-id="6fb73-156">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="6fb73-157">Pour indiquer au moteur de vue d’ignorer le corps ou les sections, appelez les méthodes `IgnoreBody` et `IgnoreSection`.</span><span class="sxs-lookup"><span data-stu-id="6fb73-157">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="6fb73-158">Le corps et toutes les sections dans une page Razor doivent être soit affichés, soit ignorés.</span><span class="sxs-lookup"><span data-stu-id="6fb73-158">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="6fb73-159">Importation de directives partagées</span><span class="sxs-lookup"><span data-stu-id="6fb73-159">Importing Shared Directives</span></span>

<span data-ttu-id="6fb73-160">Les vues et les pages peuvent utiliser des directives Razor pour importer des espaces de noms et utiliser [l’injection de dépendances](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="6fb73-160">Views and pages can use Razor directives to importing namespaces and use [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="6fb73-161">Les directives partagées par plusieurs vues peuvent être spécifiées dans un fichier *_ViewImports.cshtml* commun.</span><span class="sxs-lookup"><span data-stu-id="6fb73-161">Directives shared by many views may be specified in a common *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="6fb73-162">Le fichier `_ViewImports` prend en charge les directives suivantes :</span><span class="sxs-lookup"><span data-stu-id="6fb73-162">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`
* `@removeTagHelper`
* `@tagHelperPrefix`
* `@using`
* `@model`
* `@inherits`
* `@inject`

<span data-ttu-id="6fb73-163">Le fichier ne prend pas en charge les autres fonctionnalités Razor, telles que les fonctions et les définitions de section.</span><span class="sxs-lookup"><span data-stu-id="6fb73-163">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="6fb73-164">Exemple de fichier `_ViewImports.cshtml` :</span><span class="sxs-lookup"><span data-stu-id="6fb73-164">A sample `_ViewImports.cshtml` file:</span></span>

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="6fb73-165">Le fichier *_ViewImports.cshtml* pour une application ASP.NET Core MVC est généralement créé dans le dossier *Pages* (ou *Views*).</span><span class="sxs-lookup"><span data-stu-id="6fb73-165">The *_ViewImports.cshtml* file for an ASP.NET Core MVC app is typically placed in the *Pages* (or *Views*) folder.</span></span> <span data-ttu-id="6fb73-166">Un fichier *_ViewImports.cshtml* peut être créé dans un autre dossier ; dans ce cas, il s’applique uniquement aux pages ou vues contenues dans ce dossier et ses sous-dossiers.</span><span class="sxs-lookup"><span data-stu-id="6fb73-166">A *_ViewImports.cshtml* file can be placed within any folder, in which case it will only be applied to pages or views within that folder and its subfolders.</span></span> <span data-ttu-id="6fb73-167">Les fichiers `_ViewImports` sont traités à partir de la racine, puis pour chaque dossier conduisant à l’emplacement de la page ou vue elle-même.</span><span class="sxs-lookup"><span data-stu-id="6fb73-167">`_ViewImports` files are processed starting at the root level and then for each folder leading up to the location of the page or view itself.</span></span> <span data-ttu-id="6fb73-168">Les paramètres `_ViewImports` spécifiés au niveau de la racine peuvent être remplacés au niveau du dossier.</span><span class="sxs-lookup"><span data-stu-id="6fb73-168">`_ViewImports` settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="6fb73-169">Par exemple, supposons que :</span><span class="sxs-lookup"><span data-stu-id="6fb73-169">For example, suppose:</span></span>

* <span data-ttu-id="6fb73-170">Le fichier *_ViewImports.cshtml* au niveau de la racine inclut `@model MyModel1` et `@addTagHelper *, MyTagHelper1`.</span><span class="sxs-lookup"><span data-stu-id="6fb73-170">The  root level *_ViewImports.cshtml* file includes `@model MyModel1` and `@addTagHelper *, MyTagHelper1`.</span></span>
* <span data-ttu-id="6fb73-171">Le fichier *_ViewImports.cshtml* dans un sous-dossier inclut `@model MyModel2` et `@addTagHelper *, MyTagHelper2`.</span><span class="sxs-lookup"><span data-stu-id="6fb73-171">A subfolder  *_ViewImports.cshtml* file includes `@model MyModel2` and `@addTagHelper *, MyTagHelper2`.</span></span>

<span data-ttu-id="6fb73-172">Les pages et vues dans le sous-dossier ont accès aux Tag Helpers et au modèle `MyModel2`.</span><span class="sxs-lookup"><span data-stu-id="6fb73-172">Pages and views in the subfolder will have access to both Tag Helpers and the `MyModel2` model.</span></span>

<span data-ttu-id="6fb73-173">Si la hiérarchie des fichiers comprend plusieurs fichiers *_ViewImports.cshtml*, le comportement combiné des directives est le suivant :</span><span class="sxs-lookup"><span data-stu-id="6fb73-173">If multiple *_ViewImports.cshtml* files are found in the file hierarchy, the combined behavior of the directives are:</span></span>

* <span data-ttu-id="6fb73-174">`@addTagHelper`, `@removeTagHelper` : les deux directives sont exécutées, dans l’ordre</span><span class="sxs-lookup"><span data-stu-id="6fb73-174">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>
* <span data-ttu-id="6fb73-175">`@tagHelperPrefix` : la directive la plus proche de la vue se substitue aux autres</span><span class="sxs-lookup"><span data-stu-id="6fb73-175">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>
* <span data-ttu-id="6fb73-176">`@model` : la directive la plus proche de la vue se substitue aux autres</span><span class="sxs-lookup"><span data-stu-id="6fb73-176">`@model`: the closest one to the view overrides any others</span></span>
* <span data-ttu-id="6fb73-177">`@inherits` : la directive la plus proche de la vue se substitue aux autres</span><span class="sxs-lookup"><span data-stu-id="6fb73-177">`@inherits`: the closest one to the view overrides any others</span></span>
* <span data-ttu-id="6fb73-178">`@using` : toutes les directives sont incluses ; les doublons sont ignorés</span><span class="sxs-lookup"><span data-stu-id="6fb73-178">`@using`: all are included; duplicates are ignored</span></span>
* <span data-ttu-id="6fb73-179">`@inject` : pour chaque propriété, la plus proche de la vue se substitue aux autres propriétés ayant le même nom</span><span class="sxs-lookup"><span data-stu-id="6fb73-179">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="6fb73-180">Exécution du code avant chaque vue</span><span class="sxs-lookup"><span data-stu-id="6fb73-180">Running Code Before Each View</span></span>

<span data-ttu-id="6fb73-181">Le code qui doit s’exécuter avant chaque vue ou page doit être placé dans le fichier *_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6fb73-181">Code that needs to run before each view or page should be placed in the *_ViewStart.cshtml* file.</span></span> <span data-ttu-id="6fb73-182">Par convention, le fichier *_ViewStart.cshtml* se trouve dans le dossier *Pages* (ou *Views*).</span><span class="sxs-lookup"><span data-stu-id="6fb73-182">By convention, the *_ViewStart.cshtml* file is located in the *Pages* (or *Views*) folder.</span></span> <span data-ttu-id="6fb73-183">Les instructions contenues dans *_ViewStart.cshtml* sont exécutées avant chaque vue complète (donc hors dispositions et vues partielles).</span><span class="sxs-lookup"><span data-stu-id="6fb73-183">The statements listed in *_ViewStart.cshtml* are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="6fb73-184">Comme [ViewImports.cshtml](xref:mvc/views/layout#viewimports), *_ViewStart.cshtml* est hiérarchique.</span><span class="sxs-lookup"><span data-stu-id="6fb73-184">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), *_ViewStart.cshtml* is hierarchical.</span></span> <span data-ttu-id="6fb73-185">Si un fichier *_ViewStart.cshtml* est défini dans le dossier des vues ou des pages, il est exécuté après celui qui est défini à la racine du dossier *Pages* (ou *Views*) (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="6fb73-185">If a *_ViewStart.cshtml* file is defined in the view or pages folder, it will be run after the one defined in the root of the *Pages* (or *Views*) folder (if any).</span></span>

<span data-ttu-id="6fb73-186">Exemple de fichier *_ViewStart.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6fb73-186">A sample *_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="6fb73-187">Le fichier ci-dessus spécifie que toutes les vues doivent utiliser la disposition *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6fb73-187">The file above specifies that all views will use the *_Layout.cshtml* layout.</span></span>

<span data-ttu-id="6fb73-188">*_ViewStart.cshtml* et *_ViewImports.cshtml* **ne sont pas** généralement placés dans le dossier */Pages/Shared* (ou */Views/Shared*).</span><span class="sxs-lookup"><span data-stu-id="6fb73-188">*_ViewStart.cshtml* and *_ViewImports.cshtml* are **not** typically placed in the */Pages/Shared* (or */Views/Shared*) folder.</span></span> <span data-ttu-id="6fb73-189">Les versions de ces fichiers qui sont au niveau de l’application doivent être placées directement dans le dossier */Pages* (ou */Views*).</span><span class="sxs-lookup"><span data-stu-id="6fb73-189">The app-level versions of these files should be placed directly in the */Pages* (or */Views*) folder.</span></span>
