---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: Ajout de contenu dynamique à une page cache (VB) Microsoft Docs
author: rick-anderson
description: Apprenez à mélanger du contenu dynamique et mis en cache dans la même page. La substitution post-cache vous permet d’afficher du contenu dynamique, comme des bannières publicitaires o...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 1ed022fc560becbd21722b94f2428cf7b32f2635
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542259"
---
# <a name="adding-dynamic-content-to-a-cached-page-vb"></a>Ajout de contenu dynamique à une page mise en cache (VB)

par [Microsoft](https://github.com/microsoft)

> Apprenez à mélanger du contenu dynamique et mis en cache dans la même page. La substitution post-cache vous permet d’afficher du contenu dynamique, comme des bannières publicitaires ou des articles de nouvelles, dans une page qui a été mise en cache.

En profitant de la mise en cache de sortie, vous pouvez améliorer considérablement les performances d’une application ASP.NET MVC. Au lieu de régénérer une page chaque fois que la page est demandée, la page peut être générée une fois et mise en cache en mémoire pour plusieurs utilisateurs.

Mais il y a un problème. Que se passe-t-il si vous devez afficher du contenu dynamique dans la page ? Par exemple, imaginez que vous souhaitez afficher une bannière publicitaire dans la page. Vous ne voulez pas que la bannière annonce soit mise en cache de sorte que chaque utilisateur voit la même publicité. Tu ne gagnerais pas d’argent comme ça !

Heureusement, il existe une solution facile. Vous pouvez profiter d’une fonctionnalité du cadre ASP.NET appelé *substitution post-cache*. La substitution post-cache vous permet de remplacer le contenu dynamique dans une page qui a été mise en cache dans la mémoire.

Normalement, lorsque vous parez cachez &lt;une&gt; page en utilisant l’attribut OutputCache, la page est mise en cache sur le serveur et le client (le navigateur Web). Lorsque vous utilisez la substitution post-cache, une page n’est mise en cache que sur le serveur.

#### <a name="using-post-cache-substitution"></a>Utilisation de la substitution post-Cache

L’utilisation de la substitution post-cache nécessite deux étapes. Tout d’abord, vous devez définir une méthode qui renvoie une chaîne qui représente le contenu dynamique que vous souhaitez afficher dans la page mise en cache. Ensuite, vous appelez la méthode HttpResponse.WriteSubstitution() pour injecter le contenu dynamique dans la page.

Imaginez, par exemple, que vous souhaitez afficher au hasard différents éléments de nouvelles dans une page mise en cache. La classe dans La liste 1 expose une seule méthode, nommée RenderNews (), qui renvoie au hasard un article de nouvelles à partir d’une liste de trois nouvelles.

**Liste 1 - Models-News.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

Pour profiter de la substitution post-cache, vous appelez la méthode HttpResponse.WriteSubstitution() . La méthode WriteSubstitution() définit le code pour remplacer une région de la page mise en cache par un contenu dynamique. La méthode WriteSubstitution () est utilisée pour afficher l’élément de nouvelles aléatoire dans la vue dans la liste 2.

**Liste 2 - Views-Home.Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

La méthode RenderNews est transmise à la méthode WriteSubstitution() . Notez que la méthode RenderNews n’est pas appelée. Au lieu de cela, une référence à la méthode est transmise à WriteSubstitution() avec l’aide de l’opérateur AddressOf.

La vue index est mise en cache. La vue est retournée par le contrôleur dans la liste 3. Notez que l’action Index() &lt;est&gt; décorée d’un attribut OutputCache qui fait en cache la vue index pendant 60 secondes.

**Liste 3 - Controllers-HomeController.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

Même si la vue Index est mise en cache, différents éléments aléatoires de nouvelles sont affichés lorsque vous demandez la page Index. Lorsque vous demandez la page Index, le temps affiché par la page ne change pas pendant 60 secondes (voir la figure 1). Le fait que le temps ne change pas prouve que la page est mise en cache. Cependant, le contenu injecté par la méthode WriteSubstitution () - l’élément de nouvelles aléatoires - change à chaque demande .

**Figure 1 - Injecter des nouvelles dynamiques dans une page en cache**

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Utilisation de la substitution post-cache dans les méthodes d’aide

Un moyen plus facile de profiter de la substitution post-cache est d’encapsuler l’appel à la méthode WriteSubstitution () dans une méthode d’aide personnalisée. Cette approche est illustrée par la méthode de l’aide dans la liste 4.

**Liste 4 - Helpers-AdHelper.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

La liste 4 contient un module de base visuelle qui expose deux méthodes : RenderBanner() et RenderBannerInternal(). La méthode RenderBanner() représente la méthode réelle de l’aide. Cette méthode étend la norme ASP.NET classe HtmlHelper MVC afin que vous puissiez appeler Html.RenderBanner() dans une vue comme toute autre méthode d’aide.

La méthode RenderBanner () appelle la méthode HttpResponse.WriteSubstitution() passant la méthode RenderBannerInternal () à la méthode WriteSubstitution() .

La méthode RenderBannerInternal() est une méthode privée. Cette méthode ne sera pas exposée comme une méthode d’aide. La méthode RenderBannerInternal() renvoie au hasard une bannière publicitaire d’une liste de trois bannières publicitaires.

La vue index modifiée dans la liste 5 illustre comment vous pouvez utiliser la méthode d’aide RenderBanner(). Notez qu’une directive supplémentaire &lt;de %&gt; d’importation est incluse en haut de l’affiche pour importer l’espace de nom MvcApplication1.Helpers. Si vous négligez d’importer cet espace de nom, alors la méthode RenderBanner() n’apparaîtra pas comme une méthode sur la propriété Html.

**Liste 5 - Views-Home.Index.aspx (avec RenderBanner() méthode)**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

Lorsque vous demandez la page rendue par la vue dans la liste 5, une bannière publicitaire différente est affichée à chaque demande (voir la figure 2). La page est mise en cache, mais la bannière publicitaire est injectée dynamiquement par la méthode d’aide RenderBanner().

**Figure 2 - La vue de l’index affichant une bannière publicitaire aléatoire**

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a>Récapitulatif

Ce tutoriel a expliqué comment vous pouvez mettre à jour dynamiquement le contenu dans une page mise en cache. Vous avez appris à utiliser la méthode HttpResponse.WriteSubstitution() pour permettre l’injection de contenu dynamique dans une page mise en cache. Vous avez également appris à encapsuler l’appel à la méthode WriteSubstitution() dans une méthode d’aide HTML.

Profitez de la mise en cache dans la mesure du possible, elle peut avoir un impact dramatique sur les performances de vos applications Web. Comme expliqué dans ce tutoriel, vous pouvez profiter de la mise en cache, même lorsque vous avez besoin d’afficher du contenu dynamique dans vos pages.

> [!div class="step-by-step"]
> [Suivant précédent](improving-performance-with-output-caching-vb.md)
> [Next](creating-a-controller-vb.md)
