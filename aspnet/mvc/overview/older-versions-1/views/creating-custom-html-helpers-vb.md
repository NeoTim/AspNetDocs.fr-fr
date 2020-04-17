---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Création d’aides HTML personnalisées (VB) Microsoft Docs
author: rick-anderson
description: Le but de ce tutoriel est de démontrer comment vous pouvez créer des aides HTML personnalisées que vous pouvez utiliser dans vos vues MVC. En profitant de HTML Helper...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 52f16bf666edc9f1c95c01faf004e303fcb6a0b2
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542506"
---
# <a name="creating-custom-html-helpers-vb"></a><span data-ttu-id="a0c35-104">Création de helpers HTML personnalisés (VB)</span><span class="sxs-lookup"><span data-stu-id="a0c35-104">Creating Custom HTML Helpers (VB)</span></span>

<span data-ttu-id="a0c35-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a0c35-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="a0c35-106">Télécharger le PDF</span><span class="sxs-lookup"><span data-stu-id="a0c35-106">Download PDF</span></span>](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> <span data-ttu-id="a0c35-107">Le but de ce tutoriel est de démontrer comment vous pouvez créer des aides HTML personnalisées que vous pouvez utiliser dans vos vues MVC.</span><span class="sxs-lookup"><span data-stu-id="a0c35-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="a0c35-108">En profitant de HTML Helpers, vous pouvez réduire la quantité de dactylographie fastidieuse des balises HTML que vous devez effectuer pour créer une page HTML standard.</span><span class="sxs-lookup"><span data-stu-id="a0c35-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="a0c35-109">Le but de ce tutoriel est de démontrer comment vous pouvez créer des aides HTML personnalisées que vous pouvez utiliser dans vos vues MVC.</span><span class="sxs-lookup"><span data-stu-id="a0c35-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="a0c35-110">En profitant de HTML Helpers, vous pouvez réduire la quantité de dactylographie fastidieuse des balises HTML que vous devez effectuer pour créer une page HTML standard.</span><span class="sxs-lookup"><span data-stu-id="a0c35-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="a0c35-111">Dans la première partie de ce tutoriel, je décris certains des aides HTML existants inclus avec le cadre ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a0c35-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="a0c35-112">Ensuite, je décris deux méthodes de création d’aides HTML personnalisées: j’explique comment créer des aides HTML personnalisées en créant une méthode partagée et en créant une méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="a0c35-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a shared method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="a0c35-113">Comprendre les aides HTML</span><span class="sxs-lookup"><span data-stu-id="a0c35-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="a0c35-114">Un assistant HTML est juste une méthode qui renvoie une chaîne.</span><span class="sxs-lookup"><span data-stu-id="a0c35-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="a0c35-115">La chaîne peut représenter n’importe quel type de contenu que vous voulez.</span><span class="sxs-lookup"><span data-stu-id="a0c35-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="a0c35-116">Par exemple, vous pouvez utiliser DES aides HTML `<input>` `<img>` pour rendre les balises HTML standard comme HTML et les balises.</span><span class="sxs-lookup"><span data-stu-id="a0c35-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="a0c35-117">Vous pouvez également utiliser des aides HTML pour rendre un contenu plus complexe, comme une bande d’onglet ou une table HTML de données de base de données.</span><span class="sxs-lookup"><span data-stu-id="a0c35-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="a0c35-118">Le cadre ASP.NET MVC comprend l’ensemble suivant d’aides HTML standard (ce n’est pas une liste complète):</span><span class="sxs-lookup"><span data-stu-id="a0c35-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="a0c35-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="a0c35-119">Html.ActionLink()</span></span>
- <span data-ttu-id="a0c35-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="a0c35-120">Html.BeginForm()</span></span>
- <span data-ttu-id="a0c35-121">Html.CheckBox ()</span><span class="sxs-lookup"><span data-stu-id="a0c35-121">Html.CheckBox()</span></span>
- <span data-ttu-id="a0c35-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="a0c35-122">Html.DropDownList()</span></span>
- <span data-ttu-id="a0c35-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="a0c35-123">Html.EndForm()</span></span>
- <span data-ttu-id="a0c35-124">Html.Hidden ()</span><span class="sxs-lookup"><span data-stu-id="a0c35-124">Html.Hidden()</span></span>
- <span data-ttu-id="a0c35-125">Html.ListBox ()</span><span class="sxs-lookup"><span data-stu-id="a0c35-125">Html.ListBox()</span></span>
- <span data-ttu-id="a0c35-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="a0c35-126">Html.Password()</span></span>
- <span data-ttu-id="a0c35-127">Html.RadioButton ()</span><span class="sxs-lookup"><span data-stu-id="a0c35-127">Html.RadioButton()</span></span>
- <span data-ttu-id="a0c35-128">Html.TextArea ()</span><span class="sxs-lookup"><span data-stu-id="a0c35-128">Html.TextArea()</span></span>
- <span data-ttu-id="a0c35-129">Html.TextBox ()</span><span class="sxs-lookup"><span data-stu-id="a0c35-129">Html.TextBox()</span></span>

<span data-ttu-id="a0c35-130">Par exemple, considérez le formulaire dans la liste 1.</span><span class="sxs-lookup"><span data-stu-id="a0c35-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="a0c35-131">Ce formulaire est rendu avec l’aide de deux des aides HTML standard (voir la figure 1).</span><span class="sxs-lookup"><span data-stu-id="a0c35-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="a0c35-132">Cette forme `Html.BeginForm()` utilise `Html.TextBox()` les méthodes et aides.</span><span class="sxs-lookup"><span data-stu-id="a0c35-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods.</span></span>

<span data-ttu-id="a0c35-133">[![Page rendue avec les aides HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a0c35-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="a0c35-134">**Figure 01**: Page rendue avec des aides HTML ([Cliquez pour voir l’image grandeur nature](creating-custom-html-helpers-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a0c35-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image3.png))</span></span>

<span data-ttu-id="a0c35-135">**Liste 1`Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="a0c35-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

<span data-ttu-id="a0c35-136">La `Html.BeginForm()` méthode Helper est utilisée pour `<form>` créer les balises HTML d’ouverture et de fermeture.</span><span class="sxs-lookup"><span data-stu-id="a0c35-136">The `Html.BeginForm()` Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="a0c35-137">Notez `Html.BeginForm()` que la méthode est appelée dans une instruction à l’aide.</span><span class="sxs-lookup"><span data-stu-id="a0c35-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="a0c35-138">L’instruction d’utilisation `<form>` garantit que l’étiquette est fermée à la fin du bloc d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="a0c35-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="a0c35-139">Si vous préférez, au lieu de créer un bloc d’utilisation, vous pouvez `<form>` appeler la méthode Html.EndForm() Helper pour fermer l’étiquette.</span><span class="sxs-lookup"><span data-stu-id="a0c35-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="a0c35-140">Utilisez n’importe quelle approche `<form>` pour créer une balise d’ouverture et de fermeture qui vous semble la plus intuitive.</span><span class="sxs-lookup"><span data-stu-id="a0c35-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="a0c35-141">Les `Html.TextBox()` méthodes Helper sont utilisées dans `<input>` la liste 1 pour rendre les balises HTML.</span><span class="sxs-lookup"><span data-stu-id="a0c35-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="a0c35-142">Si vous sélectionnez la source d’afficher dans votre navigateur, vous voyez la source HTML dans la liste 2.</span><span class="sxs-lookup"><span data-stu-id="a0c35-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="a0c35-143">Notez que la source contient des balises HTML standard.</span><span class="sxs-lookup"><span data-stu-id="a0c35-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a0c35-144">remarquez `Html.TextBox()`que l’aide -HTML `<%= %>` est `<% %>` rendu avec des balises au lieu de balises.</span><span class="sxs-lookup"><span data-stu-id="a0c35-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="a0c35-145">Si vous n’incluez pas le signe égal, alors rien n’est rendu au navigateur.</span><span class="sxs-lookup"><span data-stu-id="a0c35-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="a0c35-146">Le cadre MVC ASP.NET contient un petit ensemble d’aides.</span><span class="sxs-lookup"><span data-stu-id="a0c35-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="a0c35-147">Très probablement, vous devrez étendre le cadre MVC avec des aides HTML personnalisées.</span><span class="sxs-lookup"><span data-stu-id="a0c35-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="a0c35-148">Dans le reste de ce tutoriel, vous apprenez deux méthodes de création d’aides HTML personnalisées.</span><span class="sxs-lookup"><span data-stu-id="a0c35-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="a0c35-149">**Liste 2`Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="a0c35-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a><span data-ttu-id="a0c35-150">Création d’aides HTML avec des méthodes partagées</span><span class="sxs-lookup"><span data-stu-id="a0c35-150">Creating HTML Helpers with Shared Methods</span></span>

<span data-ttu-id="a0c35-151">La façon la plus simple de créer un nouvel assistant HTML est de créer une méthode partagée qui renvoie une chaîne.</span><span class="sxs-lookup"><span data-stu-id="a0c35-151">The easiest way to create a new HTML Helper is to create a shared method that returns a string.</span></span> <span data-ttu-id="a0c35-152">Imaginez, par exemple, que vous décidiez de créer `<label>` un nouvel assistant HTML qui rend une balise HTML.</span><span class="sxs-lookup"><span data-stu-id="a0c35-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="a0c35-153">Vous pouvez utiliser la classe dans `<label>`la liste 2 pour rendre un .</span><span class="sxs-lookup"><span data-stu-id="a0c35-153">You can use the class in Listing 2 to render a `<label>`.</span></span>

<span data-ttu-id="a0c35-154">**Liste 2`Helpers\LabelHelper.vb`**</span><span class="sxs-lookup"><span data-stu-id="a0c35-154">**Listing 2 – `Helpers\LabelHelper.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

<span data-ttu-id="a0c35-155">Il n’y a rien de spécial dans la classe dans la liste 2.</span><span class="sxs-lookup"><span data-stu-id="a0c35-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="a0c35-156">La `Label()` méthode renvoie simplement une chaîne.</span><span class="sxs-lookup"><span data-stu-id="a0c35-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="a0c35-157">La vue index modifiée dans `LabelHelper` la liste `<label>` 3 utilise les balises HTML pour rendre.</span><span class="sxs-lookup"><span data-stu-id="a0c35-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="a0c35-158">Notez que le `<%@ imports %>` point de vue comprend une directive qui importe l’espace de nom Application1.Helpers.</span><span class="sxs-lookup"><span data-stu-id="a0c35-158">Notice that the view includes an `<%@ imports %>` directive that imports the Application1.Helpers namespace.</span></span>

<span data-ttu-id="a0c35-159">**Liste 2`Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="a0c35-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="a0c35-160">Création d’aides HTML avec des méthodes d’extension</span><span class="sxs-lookup"><span data-stu-id="a0c35-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="a0c35-161">Si vous souhaitez créer des aides HTML qui fonctionnent comme les aides HTML standard incluses dans le cadre MVC ASP.NET, alors vous devez créer des méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="a0c35-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="a0c35-162">Les méthodes d’extension vous permettent d’ajouter de nouvelles méthodes à une classe existante.</span><span class="sxs-lookup"><span data-stu-id="a0c35-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="a0c35-163">Lors de la création d’une méthode `HtmlHelper` d’assistance HTML, vous ajoutez de nouvelles méthodes à la classe représentée par la propriété Html d’une vue.</span><span class="sxs-lookup"><span data-stu-id="a0c35-163">When creating an HTML Helper method, you add new methods to the `HtmlHelper` class represented by a view's Html property.</span></span>

<span data-ttu-id="a0c35-164">Le module Visual Basic dans La liste `Label()` 3 ajoute une méthode d’extension nommée à la `HtmlHelper` classe.</span><span class="sxs-lookup"><span data-stu-id="a0c35-164">The Visual Basic module in Listing 3 adds an extension method named `Label()` to the `HtmlHelper` class.</span></span> <span data-ttu-id="a0c35-165">Il ya un couple de choses que vous devriez remarquer à propos de ce module.</span><span class="sxs-lookup"><span data-stu-id="a0c35-165">There are a couple of things that you should notice about this module.</span></span> <span data-ttu-id="a0c35-166">Tout d’abord, notez que `<Extension()>` le module est décoré avec l’attribut.</span><span class="sxs-lookup"><span data-stu-id="a0c35-166">First, notice that the module is decorated with the `<Extension()>` attribute.</span></span> <span data-ttu-id="a0c35-167">Afin d’utiliser cet attribut, `System.Runtime.CompilerServices` vous devez importer l’espace nom</span><span class="sxs-lookup"><span data-stu-id="a0c35-167">In order to use this attribute, you must import the `System.Runtime.CompilerServices` namespace</span></span>

<span data-ttu-id="a0c35-168">Deuxièmement, notez que `Label()` le premier `HtmlHelper` paramètre de la méthode représente la classe.</span><span class="sxs-lookup"><span data-stu-id="a0c35-168">Second, notice that the first parameter of the `Label()` method represents the `HtmlHelper` class.</span></span> <span data-ttu-id="a0c35-169">Le premier paramètre d’une méthode d’extension indique la classe que la méthode d’extension s’étend.</span><span class="sxs-lookup"><span data-stu-id="a0c35-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="a0c35-170">**Liste 3`Helpers\LabelExtensions.vb`**</span><span class="sxs-lookup"><span data-stu-id="a0c35-170">**Listing 3 – `Helpers\LabelExtensions.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

<span data-ttu-id="a0c35-171">Après avoir créé une méthode d’extension, et de construire votre application avec succès, la méthode d’extension apparaît dans Visual Studio Intellisense comme toutes les autres méthodes d’une classe (voir la figure 2).</span><span class="sxs-lookup"><span data-stu-id="a0c35-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="a0c35-172">La seule différence est que les méthodes d’extension apparaissent avec un symbole spécial à côté d’eux (une icône d’une flèche vers le bas).</span><span class="sxs-lookup"><span data-stu-id="a0c35-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>

<span data-ttu-id="a0c35-173">[![Utilisation de la méthode d’extension Html.Label()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="a0c35-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span></span>

<span data-ttu-id="a0c35-174">**Figure 02**: Utilisation de la méthode d’extension Html.Label()[(Cliquez pour voir l’image grandeur nature](creating-custom-html-helpers-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a0c35-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image6.png))</span></span>

<span data-ttu-id="a0c35-175">La vue index modifiée dans Listing 4 utilise la méthode d’extension Html.Label() pour rendre toutes ses &lt;étiquettes d’étiquette.&gt;</span><span class="sxs-lookup"><span data-stu-id="a0c35-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its &lt;label&gt; tags.</span></span>

<span data-ttu-id="a0c35-176">**Liste 4`Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="a0c35-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="a0c35-177">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="a0c35-177">Summary</span></span>

<span data-ttu-id="a0c35-178">Dans ce tutoriel, vous avez appris deux méthodes de création d’aides HTML personnalisées.</span><span class="sxs-lookup"><span data-stu-id="a0c35-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="a0c35-179">Tout d’abord, vous `Label()` avez appris à créer un assistant HTML personnalisé en créant une méthode partagée qui renvoie une chaîne.</span><span class="sxs-lookup"><span data-stu-id="a0c35-179">First, you learned how to create a custom `Label()` HTML Helper by creating a shared method that returns a string.</span></span> <span data-ttu-id="a0c35-180">Ensuite, vous avez appris `Label()` à créer une méthode d’assistance `HtmlHelper` HTML personnalisée en créant une méthode d’extension sur la classe.</span><span class="sxs-lookup"><span data-stu-id="a0c35-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="a0c35-181">Dans ce tutoriel, je me suis concentré sur la construction d’une méthode d’aide HTML extrêmement simple.</span><span class="sxs-lookup"><span data-stu-id="a0c35-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="a0c35-182">Réalisez qu’un assistant HTML peut être aussi compliqué que vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="a0c35-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="a0c35-183">Vous pouvez créer des aides HTML qui rendent un contenu riche comme des vues d’arbres, des menus ou des tableaux de données de base de données.</span><span class="sxs-lookup"><span data-stu-id="a0c35-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a0c35-184">[Suivant précédent](asp-net-mvc-views-overview-vb.md)
> [Next](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a0c35-184">[Previous](asp-net-mvc-views-overview-vb.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span></span>
