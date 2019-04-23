---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: À l’aide de la classe TagBuilder pour générer des Helpers HTML (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther vous présente une classe d’utilitaire utile dans l’infrastructure ASP.NET MVC nommé la classe TagBuilder. Vous pouvez utiliser la classe TagBuilder pour facilement...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 4fe34858aadb705ffb59e06ba805493d89aa4028
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59403205"
---
# <a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a><span data-ttu-id="8c2cf-104">À l’aide de la classe TagBuilder pour générer des Helpers HTML (VB)</span><span class="sxs-lookup"><span data-stu-id="8c2cf-104">Using the TagBuilder Class to Build HTML Helpers (VB)</span></span>

<span data-ttu-id="8c2cf-105">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="8c2cf-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="8c2cf-106">Stephen Walther vous présente une classe d’utilitaire utile dans l’infrastructure ASP.NET MVC nommé la classe TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="8c2cf-107">Vous pouvez utiliser la classe TagBuilder pour créer facilement des balises HTML.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-107">You can use the TagBuilder class to easily build HTML tags.</span></span>


<span data-ttu-id="8c2cf-108">L’infrastructure ASP.NET MVC inclut une classe d’utilitaire utile nommée la classe TagBuilder que vous pouvez utiliser lors de la création de programmes d’assistance HTML.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="8c2cf-109">La classe TagBuilder, comme le suggère le nom de la classe, vous permet de créer facilement des balises HTML.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="8c2cf-110">Dans ce bref didacticiel, vous sont fournis avec une vue d’ensemble de la classe TagBuilder et vous allez apprendre à utiliser cette classe lors de la création d’un programme d’assistance HTML simple qui effectue le rendu HTML &lt;img&gt; balises.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="8c2cf-111">Vue d’ensemble de la classe TagBuilder</span><span class="sxs-lookup"><span data-stu-id="8c2cf-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="8c2cf-112">La classe TagBuilder est contenue dans l’espace de noms System.Web.Mvc.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="8c2cf-113">Il a cinq méthodes :</span><span class="sxs-lookup"><span data-stu-id="8c2cf-113">It has five methods:</span></span>

- <span data-ttu-id="8c2cf-114">AddCssClass() – vous permet d’ajouter une nouvelle *classe = « »* à une balise d’attribut.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-114">AddCssClass() – Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="8c2cf-115">GenerateId() – permet d’ajouter un attribut id à une balise.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-115">GenerateId() – Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="8c2cf-116">Cette méthode remplace automatiquement les points dans l’id (par défaut, les points sont remplacés par des traits de soulignement)</span><span class="sxs-lookup"><span data-stu-id="8c2cf-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="8c2cf-117">MergeAttribute() – permet d’ajouter des attributs à une balise.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-117">MergeAttribute() – Enables you to add attributes to a tag.</span></span> <span data-ttu-id="8c2cf-118">Il existe plusieurs surcharges de cette méthode.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="8c2cf-119">SetInnerText() – vous permet de définir le texte interne de la balise.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-119">SetInnerText() – Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="8c2cf-120">Le texte interne est automatiquement encoder en HTML.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="8c2cf-121">ToString() – permet d’afficher la balise.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-121">ToString() – Enables you to render the tag.</span></span> <span data-ttu-id="8c2cf-122">Vous pouvez spécifier si vous souhaitez créer une balise normale, une balise de début, une balise de fin ou une balise de fermeture automatique.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>
  

<span data-ttu-id="8c2cf-123">La classe TagBuilder a quatre propriétés importantes :</span><span class="sxs-lookup"><span data-stu-id="8c2cf-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="8c2cf-124">Attributs : représente tous les attributs de la balise.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-124">Attributes – Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="8c2cf-125">IdAttributeDotReplacement – représente le caractère utilisé par la méthode GenerateId() pour remplacer les points (la valeur par défaut est un trait de soulignement).</span><span class="sxs-lookup"><span data-stu-id="8c2cf-125">IdAttributeDotReplacement – Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="8c2cf-126">InnerHTML – représente le contenu interne de la balise.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-126">InnerHTML – Represents the inner contents of the tag.</span></span> <span data-ttu-id="8c2cf-127">Assignez une chaîne à cette propriété *pas* encoder en HTML la chaîne.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="8c2cf-128">TagName – représente le nom de la balise.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-128">TagName – Represents the name of the tag.</span></span>

<span data-ttu-id="8c2cf-129">Ces méthodes et propriétés vous donnent toutes les méthodes de base et les propriétés dont vous avez besoin pour créer une balise HTML.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="8c2cf-130">Vous n’avez pas vraiment besoin d’utiliser la classe TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="8c2cf-131">Vous pouvez utiliser une classe StringBuilder à la place.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="8c2cf-132">Toutefois, la classe TagBuilder simplifie la vie un peu.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="8c2cf-133">Création d’une Image HTML Helper</span><span class="sxs-lookup"><span data-stu-id="8c2cf-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="8c2cf-134">Lorsque vous créez une instance de la classe TagBuilder, vous passez le nom de la balise que vous souhaitez générer au constructeur TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="8c2cf-135">Ensuite, vous pouvez appeler des méthodes, telles que les méthodes AddCssClass et MergeAttribute() pour modifier les attributs de la balise.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="8c2cf-136">Enfin, vous appelez la méthode ToString() pour afficher la balise.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="8c2cf-137">Par exemple, le Listing 1 contient une application d’assistance HTML de l’Image.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="8c2cf-138">L’application d’assistance de l’Image est implémentée en interne avec un TagBuilder qui représente un élément HTML &lt;img&gt; balise.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="8c2cf-139">**Liste 1 – Helpers\ImageHelper.vb**</span><span class="sxs-lookup"><span data-stu-id="8c2cf-139">**Listing 1 – Helpers\ImageHelper.vb**</span></span>

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

<span data-ttu-id="8c2cf-140">Le module dans le Listing 1 contient deux méthodes surchargées nommés Image().</span><span class="sxs-lookup"><span data-stu-id="8c2cf-140">The module in Listing 1 contains two overloaded methods named Image().</span></span> <span data-ttu-id="8c2cf-141">Lorsque vous appelez la méthode Image(), vous pouvez passer un objet qui représente un ensemble d’attributs HTML ou non.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="8c2cf-142">Notez comment la méthode TagBuilder.MergeAttribute() est utilisée pour ajouter des attributs individuels tels que l’attribut src la TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="8c2cf-143">Notez que, en outre, comment la méthode TagBuilder.MergeAttributes() est utilisée pour ajouter une collection d’attributs à la TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="8c2cf-144">La méthode MergeAttributes() accepte un dictionnaire&lt;chaîne, objet&gt; paramètre.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="8c2cf-145">La classe RouteValueDictionary est utilisée pour convertir l’objet qui représente la collection d’attributs dans un dictionnaire&lt;chaîne, objet&gt;.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-145">The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="8c2cf-146">Après avoir créé l’application d’assistance d’Image, vous pouvez utiliser l’application d’assistance dans vos vues ASP.NET MVC comme une les autres assistances HTML standards.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="8c2cf-147">La vue dans la liste 2 utilise l’application d’assistance de l’Image pour afficher la même image d’une console Xbox deux fois (voir Figure 1).</span><span class="sxs-lookup"><span data-stu-id="8c2cf-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="8c2cf-148">Le programme d’assistance Image() est appelée avec et sans une collection d’attributs HTML.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="8c2cf-149">**Listing 2 – Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="8c2cf-149">**Listing 2 – Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]


<span data-ttu-id="8c2cf-150">[![La boîte de dialogue Nouveau projet](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8c2cf-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="8c2cf-151">**Figure 01**: À l’aide du programme d’assistance d’Image ([cliquez pour afficher l’image en taille réelle](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="8c2cf-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))</span></span>


<span data-ttu-id="8c2cf-152">Notez que vous devez importer l’espace de noms associé à l’application d’assistance de l’Image en haut de la vue Index.aspx.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="8c2cf-153">Le programme d’assistance est importé avec la directive suivante :</span><span class="sxs-lookup"><span data-stu-id="8c2cf-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

<span data-ttu-id="8c2cf-154">Dans une application Visual Basic, l’espace de noms par défaut est le même que le nom de l’application.</span><span class="sxs-lookup"><span data-stu-id="8c2cf-154">In a Visual Basic application, the default namespace is the same as the name of the application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8c2cf-155">[Précédent](creating-custom-html-helpers-vb.md)
> [Suivant](creating-page-layouts-with-view-master-pages-vb.md)</span><span class="sxs-lookup"><span data-stu-id="8c2cf-155">[Previous](creating-custom-html-helpers-vb.md)
[Next](creating-page-layouts-with-view-master-pages-vb.md)</span></span>
