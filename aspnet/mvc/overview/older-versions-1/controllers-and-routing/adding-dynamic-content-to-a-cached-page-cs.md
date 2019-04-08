---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Ajout de contenu dynamique à une Page mise en cache (C#) | Microsoft Docs
author: microsoft
description: Découvrez comment combiner le contenu dynamique et mise en cache dans la même page. Post-cache vous permet d’afficher le contenu dynamique, tels que bannière publications o...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 26e40ff9659a4b8552b2a087c7c948c9f1f1554c
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58424168"
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a><span data-ttu-id="f7f26-104">Ajout de contenu dynamique à une page mise en cache (C#)</span><span class="sxs-lookup"><span data-stu-id="f7f26-104">Adding Dynamic Content to a Cached Page (C#)</span></span>
====================
<span data-ttu-id="f7f26-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f7f26-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f7f26-106">Découvrez comment combiner le contenu dynamique et mise en cache dans la même page.</span><span class="sxs-lookup"><span data-stu-id="f7f26-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="f7f26-107">Post-cache vous permet d’afficher le contenu dynamique, tels que les publications de bannière ou de nouveaux éléments au sein d’une page qui a été une sortie mise en cache.</span><span class="sxs-lookup"><span data-stu-id="f7f26-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="f7f26-108">En tirant parti de la mise en cache de sortie, vous pouvez améliorer considérablement les performances d’une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f7f26-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="f7f26-109">Au lieu de la régénération d’une page chaque fois que la page est demandée, la page peut être générée une fois et mis en cache en mémoire pour plusieurs utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="f7f26-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="f7f26-110">Mais il existe un problème.</span><span class="sxs-lookup"><span data-stu-id="f7f26-110">But there is a problem.</span></span> <span data-ttu-id="f7f26-111">Que se passe-t-il si vous devez afficher le contenu dynamique dans la page ?</span><span class="sxs-lookup"><span data-stu-id="f7f26-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="f7f26-112">Par exemple, imaginez que vous souhaitez afficher une publicité de bannière dans la page.</span><span class="sxs-lookup"><span data-stu-id="f7f26-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="f7f26-113">Vous ne voulez pas la publication de la bannière doit être mis en cache afin que chaque utilisateur voit la même publication.</span><span class="sxs-lookup"><span data-stu-id="f7f26-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="f7f26-114">Vous n’aurait pas gagner de l’argent de cette façon !</span><span class="sxs-lookup"><span data-stu-id="f7f26-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="f7f26-115">Heureusement, il existe une solution facile.</span><span class="sxs-lookup"><span data-stu-id="f7f26-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="f7f26-116">Vous pouvez tirer parti d’une fonctionnalité de l’infrastructure ASP.NET appelée *post-cache*.</span><span class="sxs-lookup"><span data-stu-id="f7f26-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="f7f26-117">Post-cache vous permet de remplacer le contenu dynamique dans une page qui a été mis en cache en mémoire.</span><span class="sxs-lookup"><span data-stu-id="f7f26-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="f7f26-118">Normalement, lors de la sortie du cache une page à l’aide de l’attribut [OutputCache], la page est mise en cache sur le serveur et le client (navigateur web).</span><span class="sxs-lookup"><span data-stu-id="f7f26-118">Normally, when you output cache a page by using the [OutputCache] attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="f7f26-119">Lorsque vous utilisez post-cache, une page est mise en cache uniquement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f7f26-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="f7f26-120">À l’aide de la substitution post-cache</span><span class="sxs-lookup"><span data-stu-id="f7f26-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="f7f26-121">À l’aide de la substitution post-cache nécessite deux étapes.</span><span class="sxs-lookup"><span data-stu-id="f7f26-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="f7f26-122">Tout d’abord, vous devez définir une méthode qui retourne une chaîne qui représente le contenu dynamique que vous souhaitez afficher dans la page mise en cache.</span><span class="sxs-lookup"><span data-stu-id="f7f26-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="f7f26-123">Ensuite, vous appelez la méthode HttpResponse.WriteSubstitution() pour injecter le contenu dynamique dans la page.</span><span class="sxs-lookup"><span data-stu-id="f7f26-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="f7f26-124">Par exemple, imaginez que vous souhaitez afficher au hasard des éléments d’informations différente dans une page mise en cache.</span><span class="sxs-lookup"><span data-stu-id="f7f26-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="f7f26-125">La classe dans le Listing 1 expose une méthode unique, nommée RenderNews(), qui renvoie au hasard un élément d’information dans une liste de trois éléments d’actualités.</span><span class="sxs-lookup"><span data-stu-id="f7f26-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="f7f26-126">**Liste 1 – Models\News.cs**</span><span class="sxs-lookup"><span data-stu-id="f7f26-126">**Listing 1 – Models\News.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

<span data-ttu-id="f7f26-127">Pour tirer parti de la substitution post-cache, vous appelez la méthode HttpResponse.WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="f7f26-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="f7f26-128">La méthode WriteSubstitution() définit le code pour remplacer une région de la page mise en cache avec un contenu dynamique.</span><span class="sxs-lookup"><span data-stu-id="f7f26-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="f7f26-129">La méthode WriteSubstitution() est utilisée pour afficher l’article aléatoire dans la vue dans la liste 2.</span><span class="sxs-lookup"><span data-stu-id="f7f26-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="f7f26-130">**Listing 2 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="f7f26-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

<span data-ttu-id="f7f26-131">La méthode RenderNews est passée à la méthode WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="f7f26-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="f7f26-132">Notez que la méthode RenderNews n’est pas appelée (il existe sans parenthèses).</span><span class="sxs-lookup"><span data-stu-id="f7f26-132">Notice that the RenderNews method is not called (there are no parentheses).</span></span> <span data-ttu-id="f7f26-133">Au lieu de cela, une référence à la méthode est passée à WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="f7f26-133">Instead a reference to the method is passed to WriteSubstitution().</span></span>

<span data-ttu-id="f7f26-134">La vue Index est mis en cache.</span><span class="sxs-lookup"><span data-stu-id="f7f26-134">The Index view is cached.</span></span> <span data-ttu-id="f7f26-135">La vue est retournée par le contrôleur dans le Listing 3.</span><span class="sxs-lookup"><span data-stu-id="f7f26-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="f7f26-136">Notez que l’action Index() est décorée avec un attribut [OutputCache] qui provoque l’affichage de l’Index doit être mis en cache pendant 60 secondes.</span><span class="sxs-lookup"><span data-stu-id="f7f26-136">Notice that the Index() action is decorated with an [OutputCache] attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="f7f26-137">**Liste 3 – Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="f7f26-137">**Listing 3 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

<span data-ttu-id="f7f26-138">Même si la vue de l’Index est mis en cache, les éléments de news aléatoires différents sont affichés lorsque vous demandez la page d’Index.</span><span class="sxs-lookup"><span data-stu-id="f7f26-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="f7f26-139">Lorsque vous demandez la page d’Index, l’heure affichée par la page ne change pas pendant 60 secondes (voir Figure 1).</span><span class="sxs-lookup"><span data-stu-id="f7f26-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="f7f26-140">Le fait que l’heure ne change pas prouve que la page est mise en cache.</span><span class="sxs-lookup"><span data-stu-id="f7f26-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="f7f26-141">Toutefois, le contenu injectées par les modifications de la méthode – aléatoire actualité – WriteSubstitution() avec chaque requête.</span><span class="sxs-lookup"><span data-stu-id="f7f26-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="f7f26-142">**Figure 1 – injectant des actualités dynamique dans une page mise en cache**</span><span class="sxs-lookup"><span data-stu-id="f7f26-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="f7f26-144">À l’aide de la substitution post-cache dans les méthodes d’assistance</span><span class="sxs-lookup"><span data-stu-id="f7f26-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="f7f26-145">Un moyen plus simple pour tirer parti de la substitution post-cache est d’encapsuler l’appel à la méthode WriteSubstitution() au sein d’une méthode d’assistance personnalisés.</span><span class="sxs-lookup"><span data-stu-id="f7f26-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="f7f26-146">Cette approche est illustrée par la méthode d’assistance dans la liste 4.</span><span class="sxs-lookup"><span data-stu-id="f7f26-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="f7f26-147">**Liste 4 – AdHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="f7f26-147">**Listing 4 – AdHelper.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

<span data-ttu-id="f7f26-148">Liste 4 contient une classe statique qui expose deux méthodes : RenderBanner() et RenderBannerInternal().</span><span class="sxs-lookup"><span data-stu-id="f7f26-148">Listing 4 contains a static class that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="f7f26-149">La méthode RenderBanner() représente la méthode d’assistance réelle.</span><span class="sxs-lookup"><span data-stu-id="f7f26-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="f7f26-150">Cette méthode étend la classe HtmlHelper de MVC ASP.NET standard, afin que vous pouvez appeler Html.RenderBanner() dans une vue comme toute autre méthode d’assistance.</span><span class="sxs-lookup"><span data-stu-id="f7f26-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="f7f26-151">La méthode RenderBanner() appelle la méthode de HttpResponse.WriteSubstitution() en passant de la méthode RenderBannerInternal() à la méthode WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="f7f26-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubstitution() method.</span></span>

<span data-ttu-id="f7f26-152">La méthode RenderBannerInternal() est une méthode privée.</span><span class="sxs-lookup"><span data-stu-id="f7f26-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="f7f26-153">Cette méthode ne soient pas exposée en tant qu’une méthode d’assistance.</span><span class="sxs-lookup"><span data-stu-id="f7f26-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="f7f26-154">La méthode RenderBannerInternal() retourne au hasard une image de bannière la publication dans une liste de trois images de publication de bannière.</span><span class="sxs-lookup"><span data-stu-id="f7f26-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="f7f26-155">La vue Index modifiée dans la liste 5 illustre comment vous pouvez utiliser la méthode d’assistance RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="f7f26-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="f7f26-156">Notez que supplémentaires &lt;% @ Import %&gt; directive est incluse en haut de la vue pour importer l’espace de noms MvcApplication1.Helpers.</span><span class="sxs-lookup"><span data-stu-id="f7f26-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="f7f26-157">Si vous oubliez d’importer cet espace de noms, la méthode RenderBanner() ne s’affiche en tant que méthode sur la propriété Html.</span><span class="sxs-lookup"><span data-stu-id="f7f26-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="f7f26-158">**Liste 5 – Views\Home\Index.aspx (avec la méthode RenderBanner())**</span><span class="sxs-lookup"><span data-stu-id="f7f26-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

<span data-ttu-id="f7f26-159">Lorsque vous demandez la page rendue par la vue dans la liste 5, une publication de bannière différent est affichée avec chaque requête (voir Figure 2).</span><span class="sxs-lookup"><span data-stu-id="f7f26-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="f7f26-160">La page est mise en cache, mais la publication de la bannière est injectée dynamiquement par la méthode d’assistance RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="f7f26-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="f7f26-161">**Figure 2 – la vue Index affichant une publication de bannière aléatoire**</span><span class="sxs-lookup"><span data-stu-id="f7f26-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="f7f26-163">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="f7f26-163">Summary</span></span>

<span data-ttu-id="f7f26-164">Ce didacticiel vous a expliqué comment vous pouvez mettre à jour dynamiquement le contenu dans une page mise en cache.</span><span class="sxs-lookup"><span data-stu-id="f7f26-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="f7f26-165">Vous avez appris comment utiliser la méthode HttpResponse.WriteSubstitution() pour permettre à injecter dans une page mise en cache du contenu dynamique.</span><span class="sxs-lookup"><span data-stu-id="f7f26-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="f7f26-166">Vous avez également appris comment encapsuler l’appel à la méthode WriteSubstitution() dans une méthode d’assistance HTML.</span><span class="sxs-lookup"><span data-stu-id="f7f26-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="f7f26-167">Tirer parti de la mise en cache si possible : il peut avoir un impact considérable sur les performances de vos applications web.</span><span class="sxs-lookup"><span data-stu-id="f7f26-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="f7f26-168">Comme expliqué dans ce didacticiel, vous pouvez tirer parti de la mise en cache même lorsque vous avez besoin afficher le contenu dynamique dans vos pages.</span><span class="sxs-lookup"><span data-stu-id="f7f26-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

## 

## 

> [!div class="step-by-step"]
> <span data-ttu-id="f7f26-169">[Précédent](improving-performance-with-output-caching-cs.md)
> [Suivant](creating-a-controller-cs.md)</span><span class="sxs-lookup"><span data-stu-id="f7f26-169">[Previous](improving-performance-with-output-caching-cs.md)
[Next](creating-a-controller-cs.md)</span></span>
