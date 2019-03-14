---
title: Tag Helper Partial dans ASP.NET Core
author: scottaddie
description: Découvrez le Tag Helper Partial ASP.NET Core et le rôle de ses attributs dans le rendu d’une vue partielle.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/25/2018
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: d56df549d845b1f83ec4a5ec97ce6b44438f725a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048006"
---
# <a name="partial-tag-helper-in-aspnet-core"></a><span data-ttu-id="d9758-103">Tag Helper Partial dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9758-103">Partial Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="d9758-104">Par [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="d9758-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="d9758-105">Pour avoir une vue d’ensemble des Tag Helpers, consultez <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="d9758-105">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="d9758-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d9758-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview"></a><span data-ttu-id="d9758-107">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="d9758-107">Overview</span></span>

<span data-ttu-id="d9758-108">Le Tag Helper Partial est utilisé dans le cadre du rendu d’une [vue partielle](xref:mvc/views/partial) dans les pages Razor et les applications MVC.</span><span class="sxs-lookup"><span data-stu-id="d9758-108">The Partial Tag Helper is used for rendering a [partial view](xref:mvc/views/partial) in Razor Pages and MVC apps.</span></span> <span data-ttu-id="d9758-109">Tenez compte des points suivants :</span><span class="sxs-lookup"><span data-stu-id="d9758-109">Consider that it:</span></span>

* <span data-ttu-id="d9758-110">Il nécessite ASP.NET Core 2.1 ou ultérieur.</span><span class="sxs-lookup"><span data-stu-id="d9758-110">Requires ASP.NET Core 2.1 or later.</span></span>
* <span data-ttu-id="d9758-111">Il constitue une alternative à la [syntaxe HTML Helper](xref:mvc/views/partial#reference-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="d9758-111">Is an alternative to [HTML Helper syntax](xref:mvc/views/partial#reference-a-partial-view).</span></span>
* <span data-ttu-id="d9758-112">Il affiche la vue partielle de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="d9758-112">Renders the partial view asynchronously.</span></span>

<span data-ttu-id="d9758-113">Parmi les options HTML Helper utilisées pour le rendu d’une vue partielle, citons les suivantes :</span><span class="sxs-lookup"><span data-stu-id="d9758-113">The HTML Helper options for rendering a partial view include:</span></span>

* [<span data-ttu-id="d9758-114">@await Html.PartialAsync</span><span class="sxs-lookup"><span data-stu-id="d9758-114">@await Html.PartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [<span data-ttu-id="d9758-115">@await Html.RenderPartialAsync</span><span class="sxs-lookup"><span data-stu-id="d9758-115">@await Html.RenderPartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

<span data-ttu-id="d9758-116">Le modèle *Product* est utilisé dans les exemples tout au long de ce document :</span><span class="sxs-lookup"><span data-stu-id="d9758-116">The *Product* model is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

<span data-ttu-id="d9758-117">Voici l’inventaire des attributs du Tag Helper Partial.</span><span class="sxs-lookup"><span data-stu-id="d9758-117">An inventory of the Partial Tag Helper attributes follows.</span></span>

## <a name="name"></a><span data-ttu-id="d9758-118">name</span><span class="sxs-lookup"><span data-stu-id="d9758-118">name</span></span>

<span data-ttu-id="d9758-119">L'attribut `name` est obligatoire.</span><span class="sxs-lookup"><span data-stu-id="d9758-119">The `name` attribute is required.</span></span> <span data-ttu-id="d9758-120">Il indique le nom ou le chemin de la vue partielle à afficher.</span><span class="sxs-lookup"><span data-stu-id="d9758-120">It indicates the name or the path of the partial view to be rendered.</span></span> <span data-ttu-id="d9758-121">Quand un nom de vue partielle est fourni, le processus de [découverte de vue](xref:mvc/views/overview#view-discovery) est lancé.</span><span class="sxs-lookup"><span data-stu-id="d9758-121">When a partial view name is provided, the [view discovery](xref:mvc/views/overview#view-discovery) process is initiated.</span></span> <span data-ttu-id="d9758-122">Ce processus est ignoré quand un chemin explicite est fourni.</span><span class="sxs-lookup"><span data-stu-id="d9758-122">That process is bypassed when an explicit path is provided.</span></span> <span data-ttu-id="d9758-123">Pour connaître toutes les valeurs `name` acceptables, consultez [Découverte des vues partielles](xref:mvc/views/partial#partial-view-discovery).</span><span class="sxs-lookup"><span data-stu-id="d9758-123">For all acceptable `name` values, see [Partial view discovery](xref:mvc/views/partial#partial-view-discovery).</span></span>

<span data-ttu-id="d9758-124">Le balisage suivant utilise un chemin explicite indiquant que le fichier *_ProductPartial.cshtml* doit être chargé à partir du dossier *Shared*.</span><span class="sxs-lookup"><span data-stu-id="d9758-124">The following markup uses an explicit path, indicating that *_ProductPartial.cshtml* is to be loaded from the *Shared* folder.</span></span> <span data-ttu-id="d9758-125">À l’aide de l’attribut [for](#for), un modèle est passé à la vue partielle pour liaison.</span><span class="sxs-lookup"><span data-stu-id="d9758-125">Using the [for](#for) attribute, a model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a><span data-ttu-id="d9758-126">for</span><span class="sxs-lookup"><span data-stu-id="d9758-126">for</span></span>

<span data-ttu-id="d9758-127">L’attribut `for` affecte un [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) à évaluer par rapport au modèle actif.</span><span class="sxs-lookup"><span data-stu-id="d9758-127">The `for` attribute assigns a [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) to be evaluated against the current model.</span></span> <span data-ttu-id="d9758-128">`ModelExpression` déduit la syntaxe `@Model.`.</span><span class="sxs-lookup"><span data-stu-id="d9758-128">A `ModelExpression` infers the `@Model.` syntax.</span></span> <span data-ttu-id="d9758-129">Par exemple, `for="Product"` peut être utilisé à la place de `for="@Model.Product"`.</span><span class="sxs-lookup"><span data-stu-id="d9758-129">For example, `for="Product"` can be used instead of `for="@Model.Product"`.</span></span> <span data-ttu-id="d9758-130">Pour substituer ce comportement d’inférence par défaut, utilisez le symbole `@` pour définir une expression inline.</span><span class="sxs-lookup"><span data-stu-id="d9758-130">This default inference behavior is overridden by using the `@` symbol to define an inline expression.</span></span>

<span data-ttu-id="d9758-131">Le balisage suivant charge *_ProductPartial.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="d9758-131">The following markup loads *_ProductPartial.cshtml*:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

<span data-ttu-id="d9758-132">La vue partielle est liée à la propriété `Product` du modèle de page associé :</span><span class="sxs-lookup"><span data-stu-id="d9758-132">The partial view is bound to the associated page model's `Product` property:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a><span data-ttu-id="d9758-133">modèle</span><span class="sxs-lookup"><span data-stu-id="d9758-133">model</span></span>

<span data-ttu-id="d9758-134">L’attribut `model` affecte une instance de modèle à passer à la vue partielle.</span><span class="sxs-lookup"><span data-stu-id="d9758-134">The `model` attribute assigns a model instance to pass to the partial view.</span></span> <span data-ttu-id="d9758-135">L’attribut `model` ne peut pas être utilisé avec l’attribut [for](#for).</span><span class="sxs-lookup"><span data-stu-id="d9758-135">The `model` attribute can't be used with the [for](#for) attribute.</span></span>

<span data-ttu-id="d9758-136">Dans le balisage suivant, un nouvel objet `Product` est instancié et passé à l’attribut `model` pour liaison :</span><span class="sxs-lookup"><span data-stu-id="d9758-136">In the following markup, a new `Product` object is instantiated and passed to the `model` attribute for binding:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a><span data-ttu-id="d9758-137">view-data</span><span class="sxs-lookup"><span data-stu-id="d9758-137">view-data</span></span>

<span data-ttu-id="d9758-138">L’attribut `view-data` affecte un [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) à passer à la vue partielle.</span><span class="sxs-lookup"><span data-stu-id="d9758-138">The `view-data` attribute assigns a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to pass to the partial view.</span></span> <span data-ttu-id="d9758-139">Le balisage suivant rend l’ensemble de la collection ViewData accessible à la vue partielle :</span><span class="sxs-lookup"><span data-stu-id="d9758-139">The following markup makes the entire ViewData collection accessible to the partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

<span data-ttu-id="d9758-140">Dans le code précédent, la valeur de clé `IsNumberReadOnly` est `true` et ajoutée à la collection ViewData.</span><span class="sxs-lookup"><span data-stu-id="d9758-140">In the preceding code, the `IsNumberReadOnly` key value is set to `true` and added to the ViewData collection.</span></span> <span data-ttu-id="d9758-141">`ViewData["IsNumberReadOnly"]` est donc accessible au sein de la vue partielle suivante :</span><span class="sxs-lookup"><span data-stu-id="d9758-141">Consequently, `ViewData["IsNumberReadOnly"]` is made accessible within the following partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

<span data-ttu-id="d9758-142">Dans cet exemple, la valeur de `ViewData["IsNumberReadOnly"]` détermine si le champ *Number* s’affiche en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="d9758-142">In this example, the value of `ViewData["IsNumberReadOnly"]` determines whether the *Number* field is displayed as read only.</span></span>

## <a name="migrate-from-an-html-helper"></a><span data-ttu-id="d9758-143">Migrer à partir d’une assistance HTML</span><span class="sxs-lookup"><span data-stu-id="d9758-143">Migrate from an HTML Helper</span></span>

<span data-ttu-id="d9758-144">Prenons l’exemple d’assistance HTML asynchrone suivante.</span><span class="sxs-lookup"><span data-stu-id="d9758-144">Consider the following asynchronous HTML Helper example.</span></span> <span data-ttu-id="d9758-145">Une collection de produits est parcourue et affichée.</span><span class="sxs-lookup"><span data-stu-id="d9758-145">A collection of products is iterated and displayed.</span></span> <span data-ttu-id="d9758-146">Conformément au premier paramètre de la méthode `PartialAsync`, la vue partielle *_ProductPartial.cshtml* est chargée.</span><span class="sxs-lookup"><span data-stu-id="d9758-146">Per the `PartialAsync` method's first parameter, the *_ProductPartial.cshtml* partial view is loaded.</span></span> <span data-ttu-id="d9758-147">Une instance du modèle `Product` est passée à la vue partielle pour la liaison.</span><span class="sxs-lookup"><span data-stu-id="d9758-147">An instance of the `Product` model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Products.cshtml?name=snippet_HtmlHelper&highlight=3)]

<span data-ttu-id="d9758-148">Le Tag Helper Partial suivant permet d’obtenir le même comportement de rendu asynchrone que l’assistance HTML `PartialAsync`.</span><span class="sxs-lookup"><span data-stu-id="d9758-148">The following Partial Tag Helper achieves the same asynchronous rendering behavior as the `PartialAsync` HTML Helper.</span></span> <span data-ttu-id="d9758-149">Une instance de modèle `Product` est assignée à l’attribut `model` pour la liaison à la vue partielle.</span><span class="sxs-lookup"><span data-stu-id="d9758-149">The `model` attribute is assigned a `Product` model instance for binding to the partial view.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Products.cshtml?name=snippet_TagHelper&highlight=3)]

## <a name="additional-resources"></a><span data-ttu-id="d9758-150">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d9758-150">Additional resources</span></span>

* <xref:mvc/views/partial>
* <xref:mvc/views/overview#weakly-typed-data-viewdata-viewdata-attribute-and-viewbag>
