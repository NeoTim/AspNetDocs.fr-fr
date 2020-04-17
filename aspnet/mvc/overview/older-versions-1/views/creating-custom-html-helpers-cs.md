---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Création d’aides HTML personnalisées (C) Microsoft Docs
author: rick-anderson
description: Le but de ce tutoriel est de démontrer comment vous pouvez créer des aides HTML personnalisées que vous pouvez utiliser dans vos vues MVC. En profitant de HTML Helper...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 82e4118fd404051b891489b62d531169e83f450d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542558"
---
# <a name="creating-custom-html-helpers-c"></a><span data-ttu-id="8f252-104">Création de helpers HTML personnalisés (C#)</span><span class="sxs-lookup"><span data-stu-id="8f252-104">Creating Custom HTML Helpers (C#)</span></span>

<span data-ttu-id="8f252-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="8f252-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="8f252-106">Télécharger le PDF</span><span class="sxs-lookup"><span data-stu-id="8f252-106">Download PDF</span></span>](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> <span data-ttu-id="8f252-107">Le but de ce tutoriel est de démontrer comment vous pouvez créer des aides HTML personnalisées que vous pouvez utiliser dans vos vues MVC.</span><span class="sxs-lookup"><span data-stu-id="8f252-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="8f252-108">En profitant de HTML Helpers, vous pouvez réduire la quantité de dactylographie fastidieuse des balises HTML que vous devez effectuer pour créer une page HTML standard.</span><span class="sxs-lookup"><span data-stu-id="8f252-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="8f252-109">Le but de ce tutoriel est de démontrer comment vous pouvez créer des aides HTML personnalisées que vous pouvez utiliser dans vos vues MVC.</span><span class="sxs-lookup"><span data-stu-id="8f252-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="8f252-110">En profitant de HTML Helpers, vous pouvez réduire la quantité de dactylographie fastidieuse des balises HTML que vous devez effectuer pour créer une page HTML standard.</span><span class="sxs-lookup"><span data-stu-id="8f252-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="8f252-111">Dans la première partie de ce tutoriel, je décris certains des aides HTML existants inclus avec le cadre ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8f252-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="8f252-112">Ensuite, je décris deux méthodes de création d’aides HTML personnalisés: j’explique comment créer des aides HTML personnalisées en créant une méthode statique et en créant une méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="8f252-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a static method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="8f252-113">Comprendre les aides HTML</span><span class="sxs-lookup"><span data-stu-id="8f252-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="8f252-114">Un assistant HTML est juste une méthode qui renvoie une chaîne.</span><span class="sxs-lookup"><span data-stu-id="8f252-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="8f252-115">La chaîne peut représenter n’importe quel type de contenu que vous voulez.</span><span class="sxs-lookup"><span data-stu-id="8f252-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="8f252-116">Par exemple, vous pouvez utiliser DES aides HTML `<input>` `<img>` pour rendre les balises HTML standard comme HTML et les balises.</span><span class="sxs-lookup"><span data-stu-id="8f252-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="8f252-117">Vous pouvez également utiliser des aides HTML pour rendre un contenu plus complexe, comme une bande d’onglet ou une table HTML de données de base de données.</span><span class="sxs-lookup"><span data-stu-id="8f252-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="8f252-118">Le cadre ASP.NET MVC comprend l’ensemble suivant d’aides HTML standard (ce n’est pas une liste complète):</span><span class="sxs-lookup"><span data-stu-id="8f252-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="8f252-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="8f252-119">Html.ActionLink()</span></span>
- <span data-ttu-id="8f252-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="8f252-120">Html.BeginForm()</span></span>
- <span data-ttu-id="8f252-121">Html.CheckBox ()</span><span class="sxs-lookup"><span data-stu-id="8f252-121">Html.CheckBox()</span></span>
- <span data-ttu-id="8f252-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="8f252-122">Html.DropDownList()</span></span>
- <span data-ttu-id="8f252-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="8f252-123">Html.EndForm()</span></span>
- <span data-ttu-id="8f252-124">Html.Hidden ()</span><span class="sxs-lookup"><span data-stu-id="8f252-124">Html.Hidden()</span></span>
- <span data-ttu-id="8f252-125">Html.ListBox ()</span><span class="sxs-lookup"><span data-stu-id="8f252-125">Html.ListBox()</span></span>
- <span data-ttu-id="8f252-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="8f252-126">Html.Password()</span></span>
- <span data-ttu-id="8f252-127">Html.RadioButton ()</span><span class="sxs-lookup"><span data-stu-id="8f252-127">Html.RadioButton()</span></span>
- <span data-ttu-id="8f252-128">Html.TextArea ()</span><span class="sxs-lookup"><span data-stu-id="8f252-128">Html.TextArea()</span></span>
- <span data-ttu-id="8f252-129">Html.TextBox ()</span><span class="sxs-lookup"><span data-stu-id="8f252-129">Html.TextBox()</span></span>

<span data-ttu-id="8f252-130">Par exemple, considérez le formulaire dans la liste 1.</span><span class="sxs-lookup"><span data-stu-id="8f252-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="8f252-131">Ce formulaire est rendu avec l’aide de deux des aides HTML standard (voir la figure 1).</span><span class="sxs-lookup"><span data-stu-id="8f252-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="8f252-132">Ce formulaire `Html.BeginForm()` utilise `Html.TextBox()` les méthodes et helper pour rendre un formulaire HTML simple.</span><span class="sxs-lookup"><span data-stu-id="8f252-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods to render a simple HTML form.</span></span>

<span data-ttu-id="8f252-133">[![Page rendue avec les aides HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8f252-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="8f252-134">**Figure 01**: Page rendue avec des aides HTML ([Cliquez pour voir l’image grandeur nature](creating-custom-html-helpers-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8f252-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image3.png))</span></span>

<span data-ttu-id="8f252-135">**Liste 1`Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="8f252-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

<span data-ttu-id="8f252-136">La méthode Html.BeginForm() Helper est utilisée pour `<form>` créer les balises HTML d’ouverture et de fermeture.</span><span class="sxs-lookup"><span data-stu-id="8f252-136">The Html.BeginForm() Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="8f252-137">Notez `Html.BeginForm()` que la méthode est appelée dans une instruction à l’aide.</span><span class="sxs-lookup"><span data-stu-id="8f252-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="8f252-138">L’instruction d’utilisation `<form>` garantit que l’étiquette est fermée à la fin du bloc d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="8f252-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="8f252-139">Si vous préférez, au lieu de créer un bloc d’utilisation, vous pouvez `<form>` appeler la méthode Html.EndForm() Helper pour fermer l’étiquette.</span><span class="sxs-lookup"><span data-stu-id="8f252-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="8f252-140">Utilisez n’importe quelle approche `<form>` pour créer une balise d’ouverture et de fermeture qui vous semble la plus intuitive.</span><span class="sxs-lookup"><span data-stu-id="8f252-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="8f252-141">Les `Html.TextBox()` méthodes Helper sont utilisées dans `<input>` la liste 1 pour rendre les balises HTML.</span><span class="sxs-lookup"><span data-stu-id="8f252-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="8f252-142">Si vous sélectionnez la source d’afficher dans votre navigateur, vous voyez la source HTML dans la liste 2.</span><span class="sxs-lookup"><span data-stu-id="8f252-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="8f252-143">Notez que la source contient des balises HTML standard.</span><span class="sxs-lookup"><span data-stu-id="8f252-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8f252-144">remarquez `Html.TextBox()`que l’aide -HTML `<%= %>` est `<% %>` rendu avec des balises au lieu de balises.</span><span class="sxs-lookup"><span data-stu-id="8f252-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="8f252-145">Si vous n’incluez pas le signe égal, alors rien n’est rendu au navigateur.</span><span class="sxs-lookup"><span data-stu-id="8f252-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="8f252-146">Le cadre MVC ASP.NET contient un petit ensemble d’aides.</span><span class="sxs-lookup"><span data-stu-id="8f252-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="8f252-147">Très probablement, vous devrez étendre le cadre MVC avec des aides HTML personnalisées.</span><span class="sxs-lookup"><span data-stu-id="8f252-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="8f252-148">Dans le reste de ce tutoriel, vous apprenez deux méthodes de création d’aides HTML personnalisées.</span><span class="sxs-lookup"><span data-stu-id="8f252-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="8f252-149">**Liste 2`Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="8f252-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a><span data-ttu-id="8f252-150">Création d’aides HTML avec des méthodes statiques</span><span class="sxs-lookup"><span data-stu-id="8f252-150">Creating HTML Helpers with Static Methods</span></span>

<span data-ttu-id="8f252-151">La façon la plus simple de créer un nouvel assistant HTML est de créer une méthode statique qui renvoie une chaîne.</span><span class="sxs-lookup"><span data-stu-id="8f252-151">The easiest way to create a new HTML Helper is to create a static method that returns a string.</span></span> <span data-ttu-id="8f252-152">Imaginez, par exemple, que vous décidiez de créer `<label>` un nouvel assistant HTML qui rend une balise HTML.</span><span class="sxs-lookup"><span data-stu-id="8f252-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="8f252-153">Vous pouvez utiliser la classe dans `<label>` la liste 2 pour rendre un .</span><span class="sxs-lookup"><span data-stu-id="8f252-153">You can use the class in Listing 2 to render a `<label>` .</span></span>

<span data-ttu-id="8f252-154">**Liste 2`Helpers\LabelHelper.cs`**</span><span class="sxs-lookup"><span data-stu-id="8f252-154">**Listing 2 – `Helpers\LabelHelper.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

<span data-ttu-id="8f252-155">Il n’y a rien de spécial dans la classe dans la liste 2.</span><span class="sxs-lookup"><span data-stu-id="8f252-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="8f252-156">La `Label()` méthode renvoie simplement une chaîne.</span><span class="sxs-lookup"><span data-stu-id="8f252-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="8f252-157">La vue index modifiée dans `LabelHelper` la liste `<label>` 3 utilise les balises HTML pour rendre.</span><span class="sxs-lookup"><span data-stu-id="8f252-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="8f252-158">Notez que le `<%@ imports %>` point de `Application1.Helpers` vue comprend une directive qui importe l’espace nom.</span><span class="sxs-lookup"><span data-stu-id="8f252-158">Notice that the view includes an `<%@ imports %>` directive that imports the `Application1.Helpers` namespace.</span></span>

<span data-ttu-id="8f252-159">**Liste 2`Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="8f252-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="8f252-160">Création d’aides HTML avec des méthodes d’extension</span><span class="sxs-lookup"><span data-stu-id="8f252-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="8f252-161">Si vous souhaitez créer des aides HTML qui fonctionnent comme les aides HTML standard incluses dans le cadre MVC ASP.NET, alors vous devez créer des méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="8f252-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="8f252-162">Les méthodes d’extension vous permettent d’ajouter de nouvelles méthodes à une classe existante.</span><span class="sxs-lookup"><span data-stu-id="8f252-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="8f252-163">Lors de la création d’une méthode d’assistance HTML, vous ajoutez de nouvelles méthodes à la classe HtmlHelper représentée par la propriété Html d’une vue.</span><span class="sxs-lookup"><span data-stu-id="8f252-163">When creating an HTML Helper method, you add new methods to the HtmlHelper class represented by a view's Html property.</span></span>

<span data-ttu-id="8f252-164">La classe dans La liste 3 `HtmlHelper` ajoute `Label()`une méthode d’extension à la classe nommée .</span><span class="sxs-lookup"><span data-stu-id="8f252-164">The class in Listing 3 adds an extension method to the `HtmlHelper` class named `Label()`.</span></span> <span data-ttu-id="8f252-165">Il ya un couple de choses que vous devriez remarquer à propos de cette classe.</span><span class="sxs-lookup"><span data-stu-id="8f252-165">There are a couple of things that you should notice about this class.</span></span> <span data-ttu-id="8f252-166">Tout d’abord, notez que la classe est une classe statique.</span><span class="sxs-lookup"><span data-stu-id="8f252-166">First, notice that the class is a static class.</span></span> <span data-ttu-id="8f252-167">Vous devez définir une méthode d’extension avec une classe statique.</span><span class="sxs-lookup"><span data-stu-id="8f252-167">You must define an extension method with a static class.</span></span>

<span data-ttu-id="8f252-168">Deuxièmement, notez que `Label()` le premier paramètre `this`de la méthode est précédé par le mot clé .</span><span class="sxs-lookup"><span data-stu-id="8f252-168">Second, notice that the first parameter of the `Label()` method is preceded by the keyword `this`.</span></span> <span data-ttu-id="8f252-169">Le premier paramètre d’une méthode d’extension indique la classe que la méthode d’extension s’étend.</span><span class="sxs-lookup"><span data-stu-id="8f252-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="8f252-170">**Liste 3`Helpers\LabelExtensions.cs`**</span><span class="sxs-lookup"><span data-stu-id="8f252-170">**Listing 3 – `Helpers\LabelExtensions.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

<span data-ttu-id="8f252-171">Après avoir créé une méthode d’extension, et de construire votre application avec succès, la méthode d’extension apparaît dans Visual Studio Intellisense comme toutes les autres méthodes d’une classe (voir la figure 2).</span><span class="sxs-lookup"><span data-stu-id="8f252-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="8f252-172">La seule différence est que les méthodes d’extension apparaissent avec un symbole spécial à côté d’eux (une icône d’une flèche vers le bas).</span><span class="sxs-lookup"><span data-stu-id="8f252-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>

<span data-ttu-id="8f252-173">[![Utilisation de la méthode d’extension Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="8f252-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span></span>

<span data-ttu-id="8f252-174">**Figure 02**: Utilisation de la méthode d’extension Html.Label()[(Cliquez pour voir l’image grandeur nature](creating-custom-html-helpers-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="8f252-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image6.png))</span></span>

<span data-ttu-id="8f252-175">La vue index modifiée dans La liste 4 utilise la méthode `<label>` d’extension Html.Label() pour rendre toutes ses balises.</span><span class="sxs-lookup"><span data-stu-id="8f252-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its `<label>` tags.</span></span>

<span data-ttu-id="8f252-176">**Liste 4`Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="8f252-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="8f252-177">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="8f252-177">Summary</span></span>

<span data-ttu-id="8f252-178">Dans ce tutoriel, vous avez appris deux méthodes de création d’aides HTML personnalisées.</span><span class="sxs-lookup"><span data-stu-id="8f252-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="8f252-179">Tout d’abord, vous `Label()` avez appris à créer un assistant HTML personnalisé en créant une méthode statique qui renvoie une chaîne.</span><span class="sxs-lookup"><span data-stu-id="8f252-179">First, you learned how to create a custom `Label()` HTML Helper by creating a static method that returns a string.</span></span> <span data-ttu-id="8f252-180">Ensuite, vous avez appris `Label()` à créer une méthode d’assistance `HtmlHelper` HTML personnalisée en créant une méthode d’extension sur la classe.</span><span class="sxs-lookup"><span data-stu-id="8f252-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="8f252-181">Dans ce tutoriel, je me suis concentré sur la construction d’une méthode d’aide HTML extrêmement simple.</span><span class="sxs-lookup"><span data-stu-id="8f252-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="8f252-182">Réalisez qu’un assistant HTML peut être aussi compliqué que vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="8f252-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="8f252-183">Vous pouvez créer des aides HTML qui rendent un contenu riche comme des vues d’arbres, des menus ou des tableaux de données de base de données.</span><span class="sxs-lookup"><span data-stu-id="8f252-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8f252-184">[Suivant précédent](asp-net-mvc-views-overview-cs.md)
> [Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span><span class="sxs-lookup"><span data-stu-id="8f252-184">[Previous](asp-net-mvc-views-overview-cs.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span></span>
