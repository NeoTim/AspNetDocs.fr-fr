---
ms.openlocfilehash: 93bda587eebc438e5da36b07cb7e4a37df8a91eb
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062716"
---
# <a name="aspnet-core-response-caching-sample"></a><span data-ttu-id="b4035-101">Exemple de la mise en cache de réponse ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b4035-101">ASP.NET Core Response Caching Sample</span></span>

<span data-ttu-id="b4035-102">Cet exemple illustre l’utilisation d’ASP.NET Core [intergiciel de mise en cache des réponses](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).</span><span class="sxs-lookup"><span data-stu-id="b4035-102">This sample illustrates the usage of ASP.NET Core [Response Caching Middleware](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).</span></span>

<span data-ttu-id="b4035-103">L’application répond avec sa page d’Index, y compris un `Cache-Control` en-tête pour configurer le comportement de mise en cache.</span><span class="sxs-lookup"><span data-stu-id="b4035-103">The app responds with its Index page, including a `Cache-Control` header to configure caching behavior.</span></span> <span data-ttu-id="b4035-104">L’application définit également la `Vary` en-tête pour configurer le cache pour répondre à la réponse uniquement si le `Accept-Encoding` en-tête des demandes suivantes correspondent à ceux de la demande d’origine.</span><span class="sxs-lookup"><span data-stu-id="b4035-104">The app also sets the `Vary` header to configure the cache to serve the response only if the `Accept-Encoding` header of subsequent requests matches that from the original request.</span></span>

<span data-ttu-id="b4035-105">Lorsque vous exécutez l’exemple, la page d’Index est pris en charge à partir du cache lorsque stockés et mis en cache pendant 10 secondes.</span><span class="sxs-lookup"><span data-stu-id="b4035-105">When running the sample, the Index page is served from cache when stored and cached for up to 10 seconds.</span></span>
