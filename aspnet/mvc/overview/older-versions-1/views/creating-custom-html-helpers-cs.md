---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Création de Helpers HTML personnalisés (c#) | Microsoft Docs
author: microsoft
description: L’objectif de ce didacticiel consiste à montrer comment vous pouvez créer des programmes d’assistance HTML personnalisé que vous pouvez utiliser dans vos vues MVC. En tirant parti du programme d’assistance HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 41306a7f09b830e0ee88135326a48beaadcfb28c
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65126647"
---
# <a name="creating-custom-html-helpers-c"></a><span data-ttu-id="0ffbd-104">Création de helpers HTML personnalisés (C#)</span><span class="sxs-lookup"><span data-stu-id="0ffbd-104">Creating Custom HTML Helpers (C#)</span></span>

<span data-ttu-id="0ffbd-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0ffbd-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="0ffbd-106">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="0ffbd-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> <span data-ttu-id="0ffbd-107">L’objectif de ce didacticiel consiste à montrer comment vous pouvez créer des programmes d’assistance HTML personnalisé que vous pouvez utiliser dans vos vues MVC.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="0ffbd-108">En tirant parti des programmes d’assistance HTML, vous pouvez réduire la quantité de frappe fastidieux de balises HTML que vous devez effectuer pour créer une page HTML standard.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="0ffbd-109">L’objectif de ce didacticiel consiste à montrer comment vous pouvez créer des programmes d’assistance HTML personnalisé que vous pouvez utiliser dans vos vues MVC.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="0ffbd-110">En tirant parti des programmes d’assistance HTML, vous pouvez réduire la quantité de frappe fastidieux de balises HTML que vous devez effectuer pour créer une page HTML standard.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="0ffbd-111">Dans la première partie de ce didacticiel, je décris certaines des programmes d’assistance HTML existant inclus avec l’infrastructure ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="0ffbd-112">Ensuite, je décris les deux méthodes de création de programmes d’assistance HTML personnalisée : J’explique comment créer des programmes d’assistance HTML personnalisée en créant une méthode statique et en créant une méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a static method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="0ffbd-113">Présentation des programmes d’assistance HTML</span><span class="sxs-lookup"><span data-stu-id="0ffbd-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="0ffbd-114">Une application d’assistance HTML est simplement une méthode qui retourne une chaîne.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="0ffbd-115">La chaîne peut représenter n’importe quel type de contenu que vous souhaitez.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="0ffbd-116">Par exemple, vous pouvez utiliser des programmes d’assistance HTML pour restituer des balises HTML standard comme HTML `<input>` et `<img>` balises.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="0ffbd-117">Vous pouvez également utiliser assistances HTML à afficher le contenu comme un contrôle onglet ou une table HTML de base de données plus complexe.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="0ffbd-118">L’infrastructure ASP.NET MVC inclut l’ensemble des programmes d’assistance HTML standard (cela n’est pas une liste complète) suivantes :</span><span class="sxs-lookup"><span data-stu-id="0ffbd-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="0ffbd-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="0ffbd-119">Html.ActionLink()</span></span>
- <span data-ttu-id="0ffbd-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="0ffbd-120">Html.BeginForm()</span></span>
- <span data-ttu-id="0ffbd-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="0ffbd-121">Html.CheckBox()</span></span>
- <span data-ttu-id="0ffbd-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="0ffbd-122">Html.DropDownList()</span></span>
- <span data-ttu-id="0ffbd-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="0ffbd-123">Html.EndForm()</span></span>
- <span data-ttu-id="0ffbd-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="0ffbd-124">Html.Hidden()</span></span>
- <span data-ttu-id="0ffbd-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="0ffbd-125">Html.ListBox()</span></span>
- <span data-ttu-id="0ffbd-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="0ffbd-126">Html.Password()</span></span>
- <span data-ttu-id="0ffbd-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="0ffbd-127">Html.RadioButton()</span></span>
- <span data-ttu-id="0ffbd-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="0ffbd-128">Html.TextArea()</span></span>
- <span data-ttu-id="0ffbd-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="0ffbd-129">Html.TextBox()</span></span>

<span data-ttu-id="0ffbd-130">Par exemple, considérez le formulaire dans le Listing 1.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="0ffbd-131">Ce formulaire est restitué à l’aide de deux des programmes d’assistance HTML standard (voir Figure 1).</span><span class="sxs-lookup"><span data-stu-id="0ffbd-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="0ffbd-132">Ce formulaire utilise la `Html.BeginForm()` et `Html.TextBox()` les méthodes d’assistance pour restituer un formulaire HTML simple.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods to render a simple HTML form.</span></span>

<span data-ttu-id="0ffbd-133">[![Page rendue avec des programmes d’assistance HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0ffbd-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="0ffbd-134">**Figure 01**: Page rendue avec des programmes d’assistance HTML ([cliquez pour afficher l’image en taille réelle](creating-custom-html-helpers-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0ffbd-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image3.png))</span></span>

<span data-ttu-id="0ffbd-135">**Liste 1 : `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="0ffbd-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

<span data-ttu-id="0ffbd-136">La méthode d’assistance Html.BeginForm() est utilisée pour créer le code HTML ouvrant et fermant `<form>` balises.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-136">The Html.BeginForm() Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="0ffbd-137">Notez que le `Html.BeginForm()` méthode est appelée au sein d’un à l’aide de déclaration.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="0ffbd-138">L’instruction using assure que le `<form>` balise se ferme à la fin de l’à l’aide de bloc.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="0ffbd-139">Si vous préférez, au lieu de créer un à l’aide de bloc, vous pouvez appeler la méthode d’assistance Html.EndForm() pour fermer la `<form>` balise.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="0ffbd-140">Utiliser l’approche de création d’ouverture et fermeture `<form>` balise qui semble la plus intuitive.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="0ffbd-141">Le `Html.TextBox()` méthodes d’assistance sont utilisés dans le Listing 1 pour le rendu HTML `<input>` balises.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="0ffbd-142">Si vous sélectionnez Afficher la source dans votre navigateur, vous verrez la source HTML dans le Listing 2.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="0ffbd-143">Notez que la source contient des balises HTML standard.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0ffbd-144">Notez que le `Html.TextBox()`-HTML Helper est rendue avec `<%= %>` balises au lieu de `<% %>` balises.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="0ffbd-145">Si vous n’incluez pas le signe égal, rien n’est rendu dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="0ffbd-146">L’infrastructure ASP.NET MVC contient un petit ensemble de programmes d’assistance.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="0ffbd-147">Vous devrez très probablement, étendre l’infrastructure MVC avec des programmes d’assistance HTML personnalisée.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="0ffbd-148">Dans le reste de ce didacticiel, vous allez les deux méthodes de création de programmes d’assistance HTML personnalisée.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="0ffbd-149">**Listing 2 : `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="0ffbd-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a><span data-ttu-id="0ffbd-150">Création de Helpers HTML avec des méthodes statiques</span><span class="sxs-lookup"><span data-stu-id="0ffbd-150">Creating HTML Helpers with Static Methods</span></span>

<span data-ttu-id="0ffbd-151">Pour créer un nouveau programme d’assistance HTML, le plus simple consiste à créer une méthode statique qui retourne une chaîne.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-151">The easiest way to create a new HTML Helper is to create a static method that returns a string.</span></span> <span data-ttu-id="0ffbd-152">Par exemple, imaginez que vous décidez de créer un nouveau programme d’assistance HTML qui assure un rendu HTML `<label>` balise.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="0ffbd-153">Vous pouvez utiliser la classe dans la liste 2 pour restituer un `<label>` .</span><span class="sxs-lookup"><span data-stu-id="0ffbd-153">You can use the class in Listing 2 to render a `<label>` .</span></span>

<span data-ttu-id="0ffbd-154">**Listing 2 : `Helpers\LabelHelper.cs`**</span><span class="sxs-lookup"><span data-stu-id="0ffbd-154">**Listing 2 – `Helpers\LabelHelper.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

<span data-ttu-id="0ffbd-155">Il n’a rien de spécial à la classe dans le Listing 2.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="0ffbd-156">Le `Label()` méthode retourne simplement une chaîne.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="0ffbd-157">La vue Index modifiée dans la liste 3 utilise le `LabelHelper` pour le rendu HTML `<label>` balises.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="0ffbd-158">Notez que la vue inclut une `<%@ imports %>` directive qui importe le `Application1.Helpers` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-158">Notice that the view includes an `<%@ imports %>` directive that imports the `Application1.Helpers` namespace.</span></span>

<span data-ttu-id="0ffbd-159">**Listing 2 : `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="0ffbd-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="0ffbd-160">Création de Helpers HTML avec les méthodes d’Extension</span><span class="sxs-lookup"><span data-stu-id="0ffbd-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="0ffbd-161">Si vous souhaitez créer des programmes d’assistance HTML qui fonctionnent comme les programmes d’assistance HTML standard inclus dans l’infrastructure ASP.NET MVC, vous devez créer des méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="0ffbd-162">Méthodes d’extension permettent d’ajouter de nouvelles méthodes à une classe existante.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="0ffbd-163">Lorsque vous créez une méthode d’assistance HTML, vous ajoutez de nouvelles méthodes à la classe HtmlHelper représentée par la propriété de Html d’un affichage.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-163">When creating an HTML Helper method, you add new methods to the HtmlHelper class represented by a view's Html property.</span></span>

<span data-ttu-id="0ffbd-164">La classe dans le Listing 3 ajoute une méthode d’extension pour le `HtmlHelper` classe nommée `Label()`.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-164">The class in Listing 3 adds an extension method to the `HtmlHelper` class named `Label()`.</span></span> <span data-ttu-id="0ffbd-165">Il existe plusieurs choses que vous devriez remarquer sur cette classe.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-165">There are a couple of things that you should notice about this class.</span></span> <span data-ttu-id="0ffbd-166">Tout d’abord, notez que la classe est une classe statique.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-166">First, notice that the class is a static class.</span></span> <span data-ttu-id="0ffbd-167">Vous devez définir une méthode d’extension avec une classe statique.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-167">You must define an extension method with a static class.</span></span>

<span data-ttu-id="0ffbd-168">En second lieu, notez que le premier paramètre de la `Label()` méthode est précédée par le mot clé `this`.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-168">Second, notice that the first parameter of the `Label()` method is preceded by the keyword `this`.</span></span> <span data-ttu-id="0ffbd-169">Le premier paramètre d’une méthode d’extension indique la classe qui étend la méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="0ffbd-170">**Liste 3 : `Helpers\LabelExtensions.cs`**</span><span class="sxs-lookup"><span data-stu-id="0ffbd-170">**Listing 3 – `Helpers\LabelExtensions.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

<span data-ttu-id="0ffbd-171">Après avoir créé une méthode d’extension et que vous générez votre application avec succès, la méthode d’extension s’affiche dans Intellisense dans Visual Studio comme tous les autres méthodes d’une classe (voir Figure 2).</span><span class="sxs-lookup"><span data-stu-id="0ffbd-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="0ffbd-172">La seule différence est qu’extension méthodes apparaissent avec un symbole spécial en regard (il s’agit d’une icône d’une flèche vers le bas).</span><span class="sxs-lookup"><span data-stu-id="0ffbd-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>

<span data-ttu-id="0ffbd-173">[![À l’aide de la méthode d’extension Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="0ffbd-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span></span>

<span data-ttu-id="0ffbd-174">**Figure 02**: À l’aide de la méthode d’extension Html.Label() ([cliquez pour afficher l’image en taille réelle](creating-custom-html-helpers-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="0ffbd-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image6.png))</span></span>

<span data-ttu-id="0ffbd-175">La vue Index modifiée sur la liste 4 utilise la méthode d’extension Html.Label() pour restituer tous ses `<label>` balises.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its `<label>` tags.</span></span>

<span data-ttu-id="0ffbd-176">**Liste 4 – `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="0ffbd-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="0ffbd-177">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="0ffbd-177">Summary</span></span>

<span data-ttu-id="0ffbd-178">Dans ce didacticiel, vous avez appris de deux méthodes de création de programmes d’assistance HTML personnalisée.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="0ffbd-179">Tout d’abord, vous avez appris à créer un personnalisé `Label()` programme d’assistance HTML en créant une méthode statique qui retourne une chaîne.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-179">First, you learned how to create a custom `Label()` HTML Helper by creating a static method that returns a string.</span></span> <span data-ttu-id="0ffbd-180">Ensuite, vous avez appris à créer un personnalisé `Label()` méthode d’assistance HTML en créant une méthode d’extension sur la `HtmlHelper` classe.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="0ffbd-181">Dans ce didacticiel, je me suis concentré sur la création d’une méthode d’assistance HTML extrêmement simple.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="0ffbd-182">Notez qu’une application d’assistance HTML peuvent être aussi complexe que vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="0ffbd-183">Vous pouvez créer des programmes d’assistance HTML qui restituent contenu riche, tels que les vues de l’arborescence, des menus ou des tables de base de données.</span><span class="sxs-lookup"><span data-stu-id="0ffbd-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0ffbd-184">[Précédent](asp-net-mvc-views-overview-cs.md)
> [Suivant](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span><span class="sxs-lookup"><span data-stu-id="0ffbd-184">[Previous](asp-net-mvc-views-overview-cs.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span></span>
