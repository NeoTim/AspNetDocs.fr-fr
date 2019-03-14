---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Réutilisation de l’interface utilisateur à l’aide de Pages maîtres et des vues partielles | Microsoft Docs
author: microsoft
description: Étape 7 examine les façons que nous pouvons appliquer le principe DRY au sein de nos modèles de vue pour éliminer les doublons de code, à l’aide de pages maîtres et des modèles de vue partielle.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 0da32e6ac38f10df6e581517989b3b1fd2f2328c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57055996"
---
<a name="re-use-ui-using-master-pages-and-partials"></a><span data-ttu-id="0dabc-103">Réutiliser l’interface utilisateur avec des pages maîtres et des vues partielles</span><span class="sxs-lookup"><span data-stu-id="0dabc-103">Re-use UI Using Master Pages and Partials</span></span>
====================
<span data-ttu-id="0dabc-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0dabc-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="0dabc-105">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="0dabc-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="0dabc-106">Il s’agit d’étape 7 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui présente en détail comment créer un petit mais terminé, l’application web à l’aide d’ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="0dabc-106">This is step 7 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="0dabc-107">Étape 7 examine les façons que nous pouvons appliquer le « principe DRY » au sein de nos modèles de vue pour éliminer les doublons de code, à l’aide de pages maîtres et des modèles de vue partielle.</span><span class="sxs-lookup"><span data-stu-id="0dabc-107">Step 7 looks at ways we can apply the "DRY Principle" within our view templates to eliminate code duplication, using partial view templates and master pages.</span></span>
> 
> <span data-ttu-id="0dabc-108">Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.</span><span class="sxs-lookup"><span data-stu-id="0dabc-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-7-partials-and-master-pages"></a><span data-ttu-id="0dabc-109">NerdDinner étape 7 : Vues partielles et des Pages maîtres</span><span class="sxs-lookup"><span data-stu-id="0dabc-109">NerdDinner Step 7: Partials and Master Pages</span></span>

<span data-ttu-id="0dabc-110">Une des philosophies de conception QU'ASP.NET MVC prend en compte est le principe « Faire pas se répéter » (communément appelé « Sec »).</span><span class="sxs-lookup"><span data-stu-id="0dabc-110">One of the design philosophies ASP.NET MVC embraces is the "Do Not Repeat Yourself" principle (commonly referred to as "DRY").</span></span> <span data-ttu-id="0dabc-111">Une conception DRY permet d’éviter la duplication de code et de logique, ce qui rend finalement les applications plus rapidement à générer et plus faciles à gérer.</span><span class="sxs-lookup"><span data-stu-id="0dabc-111">A DRY design helps eliminate the duplication of code and logic, which ultimately makes applications faster to build and easier to maintain.</span></span>

<span data-ttu-id="0dabc-112">Nous avons déjà vu le principe DRY appliqué dans plusieurs de nos scénarios NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="0dabc-112">We've already seen the DRY principle applied in several of our NerdDinner scenarios.</span></span> <span data-ttu-id="0dabc-113">Voici quelques exemples : notre logique de validation est implémentée au sein de notre couche de modèle, ce qui lui permet d’être appliquées au sein de ces deux modifier et créer des scénarios dans notre contrôleur ; Nous ré-utilisons le modèle de vue « NotFound » pour toutes les méthodes d’action Edit, détails et Delete ; Nous utilisons un convention d’affectation de noms modèle avec nos modèles de vue, ce qui vous évite de devoir spécifier explicitement le nom lorsque nous appelons la méthode d’assistance View() ; et nous ré-utilisez la classe DinnerFormViewModel pour à la fois modifier et créer des scénarios d’action.</span><span class="sxs-lookup"><span data-stu-id="0dabc-113">A few examples: our validation logic is implemented within our model layer, which enables it to be enforced across both edit and create scenarios in our controller; we are re-using the "NotFound" view template across the Edit, Details and Delete action methods; we are using a convention- naming pattern with our view templates, which eliminates the need to explicitly specify the name when we call the View() helper method; and we are re-using the DinnerFormViewModel class for both Edit and Create action scenarios.</span></span>

<span data-ttu-id="0dabc-114">Voyons maintenant les façons que nous pouvons appliquer le « principe DRY » au sein de nos modèles de vue pour éliminer les doublons de code il également.</span><span class="sxs-lookup"><span data-stu-id="0dabc-114">Let's now look at ways we can apply the "DRY Principle" within our view templates to eliminate code duplication there as well.</span></span>

### <a name="re-visiting-our-edit-and-create-view-templates"></a><span data-ttu-id="0dabc-115">Revisiter notre modifier et créer des modèles de vue</span><span class="sxs-lookup"><span data-stu-id="0dabc-115">Re-visiting our Edit and Create View Templates</span></span>

<span data-ttu-id="0dabc-116">Actuellement, nous utilisons deux modèles de vue différents – « Edit.aspx » et « Create.aspx » – pour afficher notre formulaire dîner l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0dabc-116">Currently we are using two different view templates – "Edit.aspx" and "Create.aspx" – to display our Dinner form UI.</span></span> <span data-ttu-id="0dabc-117">Une comparaison rapide visual d'entre eux met en évidence le degré de similitude qu’ils le sont.</span><span class="sxs-lookup"><span data-stu-id="0dabc-117">A quick visual comparison of them highlights how similar they are.</span></span> <span data-ttu-id="0dabc-118">Voici à quoi ressemble le formulaire de création :</span><span class="sxs-lookup"><span data-stu-id="0dabc-118">Below is what the create form looks like:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

<span data-ttu-id="0dabc-119">Et Voici à quoi ressemble notre formulaire « Modifier » :</span><span class="sxs-lookup"><span data-stu-id="0dabc-119">And here is what our "Edit" form looks like:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

<span data-ttu-id="0dabc-120">Pas beaucoup de différence y a-t-il ?</span><span class="sxs-lookup"><span data-stu-id="0dabc-120">Not much of a difference is there?</span></span> <span data-ttu-id="0dabc-121">Autre que le texte de titre et l’en-tête, les contrôles de disposition et l’entrée de formulaire sont identiques.</span><span class="sxs-lookup"><span data-stu-id="0dabc-121">Other than the title and header text, the form layout and input controls are identical.</span></span>

<span data-ttu-id="0dabc-122">Si nous ouvrons le « Edit.aspx » et « Create.aspx » les modèles de vue, vous trouverez qui ils contiennent code de contrôle de disposition et entrée forme identique.</span><span class="sxs-lookup"><span data-stu-id="0dabc-122">If we open up the "Edit.aspx" and "Create.aspx" view templates we'll find that they contain identical form layout and input control code.</span></span> <span data-ttu-id="0dabc-123">Cette duplication signifie que nous nous retrouvons avoir à apporter des modifications deux fois à chaque fois que nous introduire ou modifier une nouvelle propriété dîner - qui n’est pas bon.</span><span class="sxs-lookup"><span data-stu-id="0dabc-123">This duplication means we end up having to make changes twice anytime we introduce or change a new Dinner property - which is not good.</span></span>

### <a name="using-partial-view-templates"></a><span data-ttu-id="0dabc-124">À l’aide de modèles de vue partielle</span><span class="sxs-lookup"><span data-stu-id="0dabc-124">Using Partial View Templates</span></span>

<span data-ttu-id="0dabc-125">ASP.NET MVC prend en charge la possibilité de définir des modèles de « vue partielle » qui peuvent être utilisées pour encapsuler la logique de rendu de vue pour une partie secondaire d’une page.</span><span class="sxs-lookup"><span data-stu-id="0dabc-125">ASP.NET MVC supports the ability to define "partial view" templates that can be used to encapsulate view rendering logic for a sub-portion of a page.</span></span> <span data-ttu-id="0dabc-126">« Partiels » permettent de définir une logique de rendu vue une seule fois, puis réutiliser dans plusieurs endroits dans une application.</span><span class="sxs-lookup"><span data-stu-id="0dabc-126">"Partials" provide a useful way to define view rendering logic once, and then re-use it in multiple places across an application.</span></span>

<span data-ttu-id="0dabc-127">Pour aider à « Sec-up », notre Edit.aspx et duplication de modèle de vue Create.aspx, nous pouvons créer un modèle de vue partielle nommé « DinnerForm.ascx » qui encapsule la disposition du formulaire et les éléments d’entrée communs aux deux.</span><span class="sxs-lookup"><span data-stu-id="0dabc-127">To help "DRY-up" our Edit.aspx and Create.aspx view template duplication, we can create a partial view template named "DinnerForm.ascx" that encapsulates the form layout and input elements common to both.</span></span> <span data-ttu-id="0dabc-128">Nous aborderons ce point en cliquant sur notre annuaire dîners/vues/et en choisissant la « Add -&gt;vue « commande de menu :</span><span class="sxs-lookup"><span data-stu-id="0dabc-128">We'll do this by right-clicking on our /Views/Dinners directory and choosing the "Add-&gt;View" menu command:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

<span data-ttu-id="0dabc-129">Ceci affichera la boîte de dialogue « Ajouter une vue ».</span><span class="sxs-lookup"><span data-stu-id="0dabc-129">This will display the "Add View" dialog.</span></span> <span data-ttu-id="0dabc-130">Nous allons nommer la nouvelle vue créer des « DinnerForm », sélectionnez la case à cocher « Créer une vue partielle » dans la boîte de dialogue et nous indiquer que nous transmettons il une classe DinnerFormViewModel :</span><span class="sxs-lookup"><span data-stu-id="0dabc-130">We'll name the new view we want to create "DinnerForm", select the "Create a partial view" checkbox within the dialog, and indicate that we will pass it a DinnerFormViewModel class:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

<span data-ttu-id="0dabc-131">Lorsque nous cliquons sur le bouton « Ajouter », Visual Studio crée un nouveau modèle de vue « DinnerForm.ascx » pour nous dans le répertoire « \Views\Dinners ».</span><span class="sxs-lookup"><span data-stu-id="0dabc-131">When we click the "Add" button, Visual Studio will create a new "DinnerForm.ascx" view template for us within the "\Views\Dinners" directory.</span></span>

<span data-ttu-id="0dabc-132">Nous pouvons ensuite copier/coller la disposition du formulaire en double d’entrée de code de contrôle à partir de nos modèles de vue Edit.aspx/ Create.aspx dans notre nouveau modèle de vue partielle « DinnerForm.ascx » :</span><span class="sxs-lookup"><span data-stu-id="0dabc-132">We can then copy/paste the duplicate form layout / input control code from our Edit.aspx/ Create.aspx view templates into our new "DinnerForm.ascx" partial view template:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

<span data-ttu-id="0dabc-133">Nous pouvons ensuite mettre à jour nos modèles de vue de modification et de création pour appeler le modèle partiels DinnerForm et éviter la duplication du formulaire.</span><span class="sxs-lookup"><span data-stu-id="0dabc-133">We can then update our Edit and Create view templates to call the DinnerForm partial template and eliminate the form duplication.</span></span> <span data-ttu-id="0dabc-134">Nous pouvons faire cela en appelant Html.RenderPartial("DinnerForm") nos modèles de vue :</span><span class="sxs-lookup"><span data-stu-id="0dabc-134">We can do this by calling Html.RenderPartial("DinnerForm") within our view templates:</span></span>

##### <a name="createaspx"></a><span data-ttu-id="0dabc-135">Create.aspx</span><span class="sxs-lookup"><span data-stu-id="0dabc-135">Create.aspx</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a><span data-ttu-id="0dabc-136">Edit.aspx</span><span class="sxs-lookup"><span data-stu-id="0dabc-136">Edit.aspx</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

<span data-ttu-id="0dabc-137">Vous pouvez qualifier explicitement le chemin d’accès du modèle partiels lors de l’appel Html.RenderPartial (par exemple : ~ Views/Dinners/DinnerForm.ascx »).</span><span class="sxs-lookup"><span data-stu-id="0dabc-137">You can explicitly qualify the path of the partial template you want when calling Html.RenderPartial (for example: ~Views/Dinners/DinnerForm.ascx").</span></span> <span data-ttu-id="0dabc-138">Dans notre code ci-dessus, cependant, nous sommes en tirant parti du modèle d’affectation de noms basé sur une convention dans ASP.NET MVC et simplement spécifier « DinnerForm » comme nom de la partielle à restituer.</span><span class="sxs-lookup"><span data-stu-id="0dabc-138">In our code above, though, we are taking advantage of the convention-based naming pattern within ASP.NET MVC, and just specifying "DinnerForm" as the name of the partial to render.</span></span> <span data-ttu-id="0dabc-139">Lorsque nous faisons cela ASP.NET MVC recherche premier dans le répertoire views conventionnelle (DinnersController cela serait dîners/vues /).</span><span class="sxs-lookup"><span data-stu-id="0dabc-139">When we do this ASP.NET MVC will look first in the convention-based views directory (for DinnersController this would be /Views/Dinners).</span></span> <span data-ttu-id="0dabc-140">S’il ne trouve pas le modèle partiels il il sera recherchez-le dans le répertoire /Views/Shared.</span><span class="sxs-lookup"><span data-stu-id="0dabc-140">If it doesn't find the partial template there it will then look for it in the /Views/Shared directory.</span></span>

<span data-ttu-id="0dabc-141">Lorsque Html.RenderPartial() est appelée uniquement avec le nom de la vue partielle, ASP.NET MVC passe à la vue partielle même modèle et ViewData dictionnaire objets utilisés par le modèle de vue appelant.</span><span class="sxs-lookup"><span data-stu-id="0dabc-141">When Html.RenderPartial() is called with just the name of the partial view, ASP.NET MVC will pass to the partial view the same Model and ViewData dictionary objects used by the calling view template.</span></span> <span data-ttu-id="0dabc-142">Vous pouvez également, il existe des versions surchargées de Html.RenderPartial() qui vous permettent de passer un autre objet de modèle et/ou de dictionnaire ViewData de la vue partielle à utiliser.</span><span class="sxs-lookup"><span data-stu-id="0dabc-142">Alternatively, there are overloaded versions of Html.RenderPartial() that enable you to pass an alternate Model object and/or ViewData dictionary for the partial view to use.</span></span> <span data-ttu-id="0dabc-143">Cela est utile pour les scénarios où vous souhaitez uniquement passer un sous-ensemble du modèle complet/ViewModel.</span><span class="sxs-lookup"><span data-stu-id="0dabc-143">This is useful for scenarios where you only want to pass a subset of the full Model/ViewModel.</span></span>

| <span data-ttu-id="0dabc-144">**Rubrique de côté : Pourquoi &lt;%%&gt; au lieu de &lt;% = %&gt;?**</span><span class="sxs-lookup"><span data-stu-id="0dabc-144">**Side Topic: Why &lt;% %&gt; instead of &lt;%= %&gt;?**</span></span> |
| --- |
| <span data-ttu-id="0dabc-145">L’une des choses subtiles que vous avez peut-être remarqué avec le code ci-dessus est que nous utilisons un &lt;%%&gt; bloquer au lieu d’un &lt;% = %&gt; bloquer lors de l’appel Html.RenderPartial().</span><span class="sxs-lookup"><span data-stu-id="0dabc-145">One of the subtle things you might have noticed with the code above is that we are using a &lt;% %&gt; block instead of a &lt;%= %&gt; block when calling Html.RenderPartial().</span></span> <span data-ttu-id="0dabc-146">&lt;% = %&gt; blocs dans ASP.NET indiquent qu’un développeur veut afficher une valeur spécifiée (par exemple : &lt;% = « Hello » %&gt; affichant « Hello »).</span><span class="sxs-lookup"><span data-stu-id="0dabc-146">&lt;%= %&gt; blocks in ASP.NET indicate that a developer wants to render a specified value (for example: &lt;%= "Hello" %&gt; would render "Hello").</span></span> <span data-ttu-id="0dabc-147">&lt;%%&gt; blocs indiquent plutôt que le développeur souhaite exécuter du code, et que tout restitué de sortie au sein de celles-ci doit être effectué explicitement (par exemple : &lt;Response.Write("Hello") %&gt;.</span><span class="sxs-lookup"><span data-stu-id="0dabc-147">&lt;% %&gt; blocks instead indicate that the developer wants to execute code, and that any rendered output within them must be done explicitly (for example: &lt;% Response.Write("Hello") %&gt;.</span></span> <span data-ttu-id="0dabc-148">La raison pour laquelle nous utilisons un &lt;%%&gt; bloc avec notre code Html.RenderPartial ci-dessus est, car la méthode Html.RenderPartial() ne retourne pas une chaîne et qu’il génère à la place le contenu directement à l’appelant afficher le modèle de flux de sortie.</span><span class="sxs-lookup"><span data-stu-id="0dabc-148">The reason we are using a &lt;% %&gt; block with our Html.RenderPartial code above is because the Html.RenderPartial() method doesn't return a string, and instead outputs the content directly to the calling view template's output stream.</span></span> <span data-ttu-id="0dabc-149">Il procède ainsi pour des raisons de l’efficacité de performances et en procédant ainsi, elle évite d’avoir à créer un objet chaîne temporaire (potentiellement très grande).</span><span class="sxs-lookup"><span data-stu-id="0dabc-149">It does this for performance efficiency reasons, and by doing so it avoids the need to create a (potentially very large) temporary string object.</span></span> <span data-ttu-id="0dabc-150">Cela réduit l’utilisation de la mémoire et améliore le débit global d’applications.</span><span class="sxs-lookup"><span data-stu-id="0dabc-150">This reduces memory usage and improves overall application throughput.</span></span> <span data-ttu-id="0dabc-151">Une erreur courante lors de l’utilisation de Html.RenderPartial() est d’oublier d’ajouter un point-virgule à la fin de l’appel quand il est dans un &lt;%%&gt; bloc.</span><span class="sxs-lookup"><span data-stu-id="0dabc-151">One common mistake when using Html.RenderPartial() is to forget to add a semi-colon at the end of the call when it is within a &lt;% %&gt; block.</span></span> <span data-ttu-id="0dabc-152">Par exemple, ce code entraîne une erreur du compilateur : &lt;Html.RenderPartial("DinnerForm") %&gt; vous devez plutôt écrire : &lt;% Html.RenderPartial("DinnerForm") ; %&gt; effet, &lt;%%&gt; blocs sont des instructions de code autonome et lorsque vous utilisez C# les instructions de code doivent se terminer par un point-virgule.</span><span class="sxs-lookup"><span data-stu-id="0dabc-152">For example, this code will cause a compiler error: &lt;% Html.RenderPartial("DinnerForm") %&gt; You instead need to write: &lt;% Html.RenderPartial("DinnerForm"); %&gt; This is because &lt;% %&gt; blocks are self-contained code statements, and when using C# code statements need to be terminated with a semi-colon.</span></span> |

### <a name="using-partial-view-templates-to-clarify-code"></a><span data-ttu-id="0dabc-153">À l’aide de modèles de vue partielle à clarifier le Code</span><span class="sxs-lookup"><span data-stu-id="0dabc-153">Using Partial View Templates to Clarify Code</span></span>

<span data-ttu-id="0dabc-154">Nous avons créé le modèle de vue partielle « DinnerForm » pour éviter de dupliquer la logique de rendu d’affichage à plusieurs endroits.</span><span class="sxs-lookup"><span data-stu-id="0dabc-154">We created the "DinnerForm" partial view template to avoid duplicating view rendering logic in multiple places.</span></span> <span data-ttu-id="0dabc-155">Il s’agit de la raison la plus courante pour créer des modèles de vue partielle.</span><span class="sxs-lookup"><span data-stu-id="0dabc-155">This is the most common reason to create partial view templates.</span></span>

<span data-ttu-id="0dabc-156">Parfois, il est toujours judicieux pour créer des vues partielles même lorsqu’elles sont appelées uniquement dans un emplacement unique.</span><span class="sxs-lookup"><span data-stu-id="0dabc-156">Sometimes it still makes sense to create partial views even when they are only being called in a single place.</span></span> <span data-ttu-id="0dabc-157">Modèles de vue très compliqué peuvent devenir beaucoup plus faciles à lire lorsque leur logique de rendu de vue est extrait et partitionnée en une seule ou plus bien nommé modèles partielles.</span><span class="sxs-lookup"><span data-stu-id="0dabc-157">Very complicated view templates can often become much easier to read when their view rendering logic is extracted and partitioned into one or more well named partial templates.</span></span>

<span data-ttu-id="0dabc-158">Par exemple, considérez le ci-dessous extrait de code à partir du fichier Site.master dans notre projet (ce qui nous nous allons examiner sous peu).</span><span class="sxs-lookup"><span data-stu-id="0dabc-158">For example, consider the below code-snippet from the Site.master file in our project (which we will be looking at shortly).</span></span> <span data-ttu-id="0dabc-159">Le code est relativement simple à lire : en partie parce que la logique pour afficher une connexion/déconnexion situé en haut à droite de l’écran est encapsulée dans partielle « LogOnUserControl » :</span><span class="sxs-lookup"><span data-stu-id="0dabc-159">The code is relatively straight-forward to read – partly because the logic to display a login/logout link at the top right of the screen is encapsulated within the "LogOnUserControl" partial:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

<span data-ttu-id="0dabc-160">Chaque fois que vous êtes amené à confondre essayez de comprendre le balisage html/code au sein d’un modèle de vue, envisagez de si il ne serait plus clair si certaines de ces a été extraite et refactorisés dans les vues partielles bien nommés.</span><span class="sxs-lookup"><span data-stu-id="0dabc-160">Whenever you find yourself getting confused trying to understand the html/code markup within a view-template, consider whether it wouldn't be clearer if some of it was extracted and refactored into well-named partial views.</span></span>

### <a name="master-pages"></a><span data-ttu-id="0dabc-161">Pages maîtres</span><span class="sxs-lookup"><span data-stu-id="0dabc-161">Master Pages</span></span>

<span data-ttu-id="0dabc-162">La prise en charge des vues partielles, ASP.NET MVC prend également en charge la possibilité de créer des modèles de « page maître » qui peuvent être utilisées pour définir la mise en page commune et le html de niveau supérieur d’un site.</span><span class="sxs-lookup"><span data-stu-id="0dabc-162">In addition to supporting partial views, ASP.NET MVC also supports the ability to create "master page" templates that can be used to define the common layout and top-level html of a site.</span></span> <span data-ttu-id="0dabc-163">Espace réservé de contrôles peuvent ensuite être ajoutés à la page maître pour identifier les zones remplaçables qui peuvent être remplacées ou « remplis » par les vues de contenu.</span><span class="sxs-lookup"><span data-stu-id="0dabc-163">Content placeholder controls can then be added to the master page to identify replaceable regions that can be overridden or "filled in" by views.</span></span> <span data-ttu-id="0dabc-164">Cela fournit un moyen très efficace (et sec) pour appliquer une disposition courante sur une application.</span><span class="sxs-lookup"><span data-stu-id="0dabc-164">This provides a very effective (and DRY) way to apply a common layout across an application.</span></span>

<span data-ttu-id="0dabc-165">Par défaut, les nouveaux projets ASP.NET MVC ont un modèle de page maître automatiquement ajouté.</span><span class="sxs-lookup"><span data-stu-id="0dabc-165">By default, new ASP.NET MVC projects have a master page template automatically added to them.</span></span> <span data-ttu-id="0dabc-166">Cette page maître est nommée « Site.master » et se trouve dans le dossier \Views\Shared\ :</span><span class="sxs-lookup"><span data-stu-id="0dabc-166">This master page is named "Site.master" and lives within the \Views\Shared\ folder:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

<span data-ttu-id="0dabc-167">Le fichier Site.master par défaut se présente comme suit.</span><span class="sxs-lookup"><span data-stu-id="0dabc-167">The default Site.master file looks like below.</span></span> <span data-ttu-id="0dabc-168">Il définit le code html externe du site, ainsi que d’un menu de navigation en haut.</span><span class="sxs-lookup"><span data-stu-id="0dabc-168">It defines the outer html of the site, along with a menu for navigation at the top.</span></span> <span data-ttu-id="0dabc-169">Il contient deux contrôles remplaçable par l’espace réservé de contenu : un pour le titre et l’autre pour où le contenu principal d’une page doit être remplacé :</span><span class="sxs-lookup"><span data-stu-id="0dabc-169">It contains two replaceable content placeholder controls – one for the title, and the other for where the primary content of a page should be replaced:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

<span data-ttu-id="0dabc-170">Tous les modèles de vue que nous avons créé pour notre application NerdDinner (« List », « Details », « Modifier », « Créer », « NotFound », etc.) ont été basés sur ce modèle de Site.master.</span><span class="sxs-lookup"><span data-stu-id="0dabc-170">All of the view templates we've created for our NerdDinner application ("List", "Details", "Edit", "Create", "NotFound", etc) have been based on this Site.master template.</span></span> <span data-ttu-id="0dabc-171">Cela est indiqué par le biais de l’attribut « MasterPageFile » qui a été ajouté par défaut vers le haut &lt;% @ Page %&gt; directive lorsque nous avons créé notre vues à l’aide de la boîte de dialogue « Ajouter une vue » :</span><span class="sxs-lookup"><span data-stu-id="0dabc-171">This is indicated via the "MasterPageFile" attribute that was added by default to the top &lt;% @ Page %&gt; directive when we created our views using the "Add View" dialog:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

<span data-ttu-id="0dabc-172">Cela signifie que nous pouvons modifier le contenu Site.master et ont les modifications automatiquement appliquées et utilisé lorsque nous affichons un de nos modèles de vue.</span><span class="sxs-lookup"><span data-stu-id="0dabc-172">What this means is that we can change the Site.master content, and have the changes automatically be applied and used when we render any of our view templates.</span></span>

<span data-ttu-id="0dabc-173">Nous allons mettre à jour section d’en-tête de notre Site.master afin que l’en-tête de notre application est « NerdDinner » au lieu de « My Application MVC ».</span><span class="sxs-lookup"><span data-stu-id="0dabc-173">Let's update our Site.master's header section so that the header of our application is "NerdDinner" instead of "My MVC Application".</span></span> <span data-ttu-id="0dabc-174">Nous allons également mettre à jour notre menu de navigation afin que le premier onglet est « Rechercher un dîner » (gérées par la méthode d’action du HomeController Index()) et nous allons ajouter un nouvel onglet appelé « Hôte un dîner » (gérées par la méthode d’action de la DinnersController Create()) :</span><span class="sxs-lookup"><span data-stu-id="0dabc-174">Let's also update our navigation menu so that the first tab is "Find a Dinner" (handled by the HomeController's Index() action method), and let's add a new tab called "Host a Dinner" (handled by the DinnersController's Create() action method):</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

<span data-ttu-id="0dabc-175">Lorsque nous enregistrons le fichier Site.master et actualiser notre navigateur, nous allons voir notre en-tête changements soient répercutés à toutes les vues au sein de notre application.</span><span class="sxs-lookup"><span data-stu-id="0dabc-175">When we save the Site.master file and refresh our browser we'll see our header changes show up across all views within our application.</span></span> <span data-ttu-id="0dabc-176">Exemple :</span><span class="sxs-lookup"><span data-stu-id="0dabc-176">For example:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

<span data-ttu-id="0dabc-177">Et avec le */Dinners/modification / [id]* URL :</span><span class="sxs-lookup"><span data-stu-id="0dabc-177">And with the */Dinners/Edit/[id]* URL:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a><span data-ttu-id="0dabc-178">Étape suivante</span><span class="sxs-lookup"><span data-stu-id="0dabc-178">Next Step</span></span>

<span data-ttu-id="0dabc-179">Vues partielles et les pages maîtres fournissent des options très flexibles qui vous permettent d’organiser correctement les vues.</span><span class="sxs-lookup"><span data-stu-id="0dabc-179">Partials and master pages provide very flexible options that enable you to cleanly organize views.</span></span> <span data-ttu-id="0dabc-180">Vous trouverez qu’ils permettent de vous éviter de dupliquer la vue de contenu / code et facilitent la lecture et la maintenance de vos modèles de vue.</span><span class="sxs-lookup"><span data-stu-id="0dabc-180">You'll find that they help you avoid duplicating view content/ code, and make your view templates easier to read and maintain.</span></span>

<span data-ttu-id="0dabc-181">Nous allons maintenant revisiter l’annonce scénario que nous avons créé précédemment et activer la prise en charge de la pagination évolutive.</span><span class="sxs-lookup"><span data-stu-id="0dabc-181">Let's now revisit the listing scenario we built earlier and enable scalable paging support.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0dabc-182">[Précédent](use-viewdata-and-implement-viewmodel-classes.md)
> [Suivant](implement-efficient-data-paging.md)</span><span class="sxs-lookup"><span data-stu-id="0dabc-182">[Previous](use-viewdata-and-implement-viewmodel-classes.md)
[Next](implement-efficient-data-paging.md)</span></span>