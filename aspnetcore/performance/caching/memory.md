---
title: Mettre en cache en mémoire dans ASP.NET Core
author: rick-anderson
description: Découvrez comment mettre en cache les données en mémoire dans ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
uid: performance/caching/memory
ms.openlocfilehash: 9a7727ad41a05f39d74877af3c8f2e3f7a620c7d
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050036"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="bfa2e-103">Mettre en cache en mémoire dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bfa2e-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="bfa2e-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="bfa2e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="bfa2e-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bfa2e-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="bfa2e-106">Principes fondamentaux de la mise en cache</span><span class="sxs-lookup"><span data-stu-id="bfa2e-106">Caching basics</span></span>

<span data-ttu-id="bfa2e-107">La mise en cache peut améliorer considérablement les performances et l’évolutivité d’une application en réduisant le travail requis pour générer le contenu.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="bfa2e-108">Elle est idéale pour les données qui ne sont pas souvent modifiées.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="bfa2e-109">Elle effectue une copie des données, qui sont ainsi renvoyées beaucoup plus rapidement qu’à partir de la source d’origine.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="bfa2e-110">L'application doit être écrite et testée de façon à ne jamais dépendre des données en cache.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="bfa2e-111">ASP.NET Core prend en charge plusieurs caches différents.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="bfa2e-112">Le cache le plus simple est basé sur le [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), qui représente un cache stocké dans la mémoire du serveur web.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="bfa2e-113">Les applications qui s’exécutent sur une batterie de serveurs de plusieurs serveurs doivent vérifier que les sessions sont rémanentes lors de l’utilisation du cache en mémoire.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="bfa2e-114">Les sessions rémanentes garantissent que les demandes d’un client vont vers le même serveur.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="bfa2e-115">Par exemple, les applications web Azure utilisent [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) pour router toutes les demandes vers le même serveur.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="bfa2e-116">Les sessions non rémanentes dans une batterie de serveurs web nécessitent un [cache distribué](distributed.md) pour éviter les problèmes de cohérence du cache.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="bfa2e-117">Pour certaines applications, un cache distribué peut prendre en charge un sclae-out plus important qu'un cache en mémoire.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-117">For some apps, a distributed cache can support higher scale-out than an in-memory cache.</span></span> <span data-ttu-id="bfa2e-118">Il permet de décharger la mémoire cache vers un processus externe.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bfa2e-119">Le cache `IMemoryCache` supprime les entrées du cache mémoire insuffisante, sauf si la [priorité de cache](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) est définie à `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="bfa2e-120">Vous pouvez définir le `CacheItemPriority` pour ajuster la priorité avec laquelle le cache évince des éléments avec une mémoire insuffisante.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

::: moniker-end

<span data-ttu-id="bfa2e-121">Le cache en mémoire permet de stocker n’importe quel objet ; l’interface de cache distribué est limitée à `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span> <span data-ttu-id="bfa2e-122">Les éléments de cache de magasin de cache en mémoire et distribuée en tant que paires clé-valeur.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-122">The in-memory and distributed cache store cache items as key-value pairs.</span></span>

## <a name="systemruntimecachingmemorycache"></a><span data-ttu-id="bfa2e-123">System.Runtime.Caching/MemoryCache</span><span class="sxs-lookup"><span data-stu-id="bfa2e-123">System.Runtime.Caching/MemoryCache</span></span>

<span data-ttu-id="bfa2e-124"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([Package NuGet](https://www.nuget.org/packages/System.Runtime.Caching/)) peut être utilisé avec :</span><span class="sxs-lookup"><span data-stu-id="bfa2e-124"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([NuGet package](https://www.nuget.org/packages/System.Runtime.Caching/)) can be used with:</span></span>

* <span data-ttu-id="bfa2e-125">.NET standard 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-125">.NET Standard 2.0 or later.</span></span>
* <span data-ttu-id="bfa2e-126">N’importe quel [implémentation .NET](/dotnet/standard/net-standard#net-implementation-support) qui cible .NET Standard 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-126">Any [.NET implementation](/dotnet/standard/net-standard#net-implementation-support) that targets .NET Standard 2.0 or later.</span></span> <span data-ttu-id="bfa2e-127">Par exemple, ASP.NET Core 2.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-127">For example, ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="bfa2e-128">.NET framework 4.5 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-128">.NET Framework 4.5 or later.</span></span>

<span data-ttu-id="bfa2e-129">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/`IMemoryCache` est recommandé par rapport à `System.Runtime.Caching`/`MemoryCache` , car il est mieux intégré dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-129">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/`IMemoryCache` (described in this topic) is recommended over `System.Runtime.Caching`/`MemoryCache` because it's better integrated into ASP.NET Core.</span></span> <span data-ttu-id="bfa2e-130">Par exemple, `IMemoryCache` fonctionne en mode natif avec [l’injection de dépendances](xref:fundamentals/dependency-injection) ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-130">For example, `IMemoryCache` works natively with ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="bfa2e-131">Utilisez `System.Runtime.Caching` / `MemoryCache` comme un pont de compatibilité lors du portage du code à partir d’ASP.NET 4.x vers ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-131">Use `System.Runtime.Caching`/`MemoryCache` as a compatibility bridge when porting code from ASP.NET 4.x to ASP.NET Core.</span></span>

## <a name="cache-guidelines"></a><span data-ttu-id="bfa2e-132">Recommandations de cache</span><span class="sxs-lookup"><span data-stu-id="bfa2e-132">Cache guidelines</span></span>

* <span data-ttu-id="bfa2e-133">Code doit systématiquement posséder une option de secours pour extraire des données et **pas** dépendent d’une valeur mise en cache qui est disponible.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-133">Code should always have a fallback option to fetch data and **not** depend on a cached value being available.</span></span>
* <span data-ttu-id="bfa2e-134">Le cache utilise une ressource rare, la mémoire.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-134">The cache uses a scarce resource, memory.</span></span> <span data-ttu-id="bfa2e-135">Limiter la croissance de cache :</span><span class="sxs-lookup"><span data-stu-id="bfa2e-135">Limit cache growth:</span></span>
  * <span data-ttu-id="bfa2e-136">Faire **pas** utiliser entrée externe en tant que clés de cache.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-136">Do **not** use external input as cache keys.</span></span>
  * <span data-ttu-id="bfa2e-137">Utiliser des délais d’expiration pour limiter la croissance du cache.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-137">Use expirations to limit cache growth.</span></span>
  * [<span data-ttu-id="bfa2e-138">Permet de limiter la taille du cache SetSize, la taille et SizeLimit</span><span class="sxs-lookup"><span data-stu-id="bfa2e-138">Use SetSize, Size, and SizeLimit to limit cache size</span></span>](#use-setsize-size-and-sizelimit-to-limit-cache-size)

## <a name="using-imemorycache"></a><span data-ttu-id="bfa2e-139">À l’aide de IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="bfa2e-139">Using IMemoryCache</span></span>

<span data-ttu-id="bfa2e-140">La mise en cache en mémoire est un *service* qui est référencé à partir de votre application à l’aide de l'[Injection de dépendance](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="bfa2e-140">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="bfa2e-141">Appelez `AddMemoryCache` dans `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="bfa2e-141">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="bfa2e-142">Faire appel à l'instance `IMemoryCache` dans le constructeur :</span><span class="sxs-lookup"><span data-stu-id="bfa2e-142">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bfa2e-143">`IMemoryCache` nécessite le package NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span><span class="sxs-lookup"><span data-stu-id="bfa2e-143">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bfa2e-144">`IMemoryCache` nécessite le package NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), qui est disponible dans le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="bfa2e-144">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="bfa2e-145">`IMemoryCache` nécessite le package NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), qui est disponible dans le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bfa2e-145">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="bfa2e-146">Le code suivant utilise [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) pour vérifier si une heure est dans le cache.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-146">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="bfa2e-147">Si une heure n’est pas mis en cache, une nouvelle entrée est créée et ajoutée au cache avec [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span><span class="sxs-lookup"><span data-stu-id="bfa2e-147">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="bfa2e-148">L’heure actuelle et l’heure de mise en cache s’affichent :</span><span class="sxs-lookup"><span data-stu-id="bfa2e-148">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="bfa2e-149">La valeur `DateTime` mise en cache reste dans le cache tant qu'il existe des demandes dans le délai d’expiration (et aucune suppression en raison d’une sollicitation de la mémoire).</span><span class="sxs-lookup"><span data-stu-id="bfa2e-149">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="bfa2e-150">L’illustration ci-dessous indique l’heure actuelle et une heure antérieure récupérées du cache :</span><span class="sxs-lookup"><span data-stu-id="bfa2e-150">The following image shows the current time and an older time retrieved from the cache:</span></span>

![Vue index avec deux heures différentes affichées](memory/_static/time.png)

<span data-ttu-id="bfa2e-152">Le code suivant utilise [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) et [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) pour mettre en cache des données.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-152">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="bfa2e-153">Le code suivant appelle [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) pour extraire l’heure de mise en cache :</span><span class="sxs-lookup"><span data-stu-id="bfa2e-153">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="bfa2e-154"><xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*> , <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, et [obtenir](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) font partie de méthodes d’extension de la [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) classe qui étend les fonctionnalités de <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-154"><xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*> , <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, and [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) are extension methods part of the [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) class that extends the capability of <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="bfa2e-155">Consultez [IMemoryCache méthodes](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) et [CacheExtensions méthodes](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) pour obtenir une description des autres méthodes de cache.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-155">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of other cache methods.</span></span>

## <a name="memorycacheentryoptions"></a><span data-ttu-id="bfa2e-156">MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="bfa2e-156">MemoryCacheEntryOptions</span></span>

<span data-ttu-id="bfa2e-157">L’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="bfa2e-157">The following sample:</span></span>

- <span data-ttu-id="bfa2e-158">Définit l’heure d’expiration absolue.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-158">Sets the absolute expiration time.</span></span> <span data-ttu-id="bfa2e-159">Ceci est la durée maximale pendant laquelle l’entrée peut être mise en cache et empêche l’élément de devenir trop périmé quand l’expiration glissante est constamment renouvelée.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-159">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="bfa2e-160">Définit un délai d’expiration glissant.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-160">Sets a sliding expiration time.</span></span> <span data-ttu-id="bfa2e-161">Les requêtes qui accèdent à cet élément de mise en cache réinitialisent l’horloge d’expiration glissante.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-161">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="bfa2e-162">Définit la priorité de cache sur `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-162">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
- <span data-ttu-id="bfa2e-163">Définit un [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) qui est appelé après la suppression de l’entrée du cache.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-163">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="bfa2e-164">Le rappel est exécuté sur un thread différent du code qui supprime l’élément à partir du cache.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-164">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a><span data-ttu-id="bfa2e-165">Utiliser SetSize, Size et SizeLimit pour limiter la taille du cache</span><span class="sxs-lookup"><span data-stu-id="bfa2e-165">Use SetSize, Size, and SizeLimit to limit cache size</span></span>

<span data-ttu-id="bfa2e-166">Une instance `MemoryCache` peut éventuellement spécifier et appliquer une limite de taille.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-166">A `MemoryCache` instance may optionally specify and enforce a size limit.</span></span> <span data-ttu-id="bfa2e-167">La limite de taille de mémoire n’a pas d’unité de mesure, car le cache dispose d’aucun mécanisme pour mesurer le nombre d’entrées.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-167">The memory size limit does not have a defined unit of measure because the cache has no mechanism to measure the size of entries.</span></span> <span data-ttu-id="bfa2e-168">Si la limite de taille de mémoire cache est définie, toutes les entrées doivent spécifier taille.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-168">If the cache memory size limit is set, all entries must specify size.</span></span> <span data-ttu-id="bfa2e-169">La taille spécifiée est exprimé en unités choisies par le développeur.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-169">The size specified is in units the developer chooses.</span></span>

<span data-ttu-id="bfa2e-170">Exemple :</span><span class="sxs-lookup"><span data-stu-id="bfa2e-170">For example:</span></span>

* <span data-ttu-id="bfa2e-171">Si l’application web met en cache principalement les chaînes, chaque taille d’entrée du cache peut être la longueur de chaîne.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-171">If the web app was primarily caching strings, each cache entry size could be the string length.</span></span>
* <span data-ttu-id="bfa2e-172">L’application peut spécifier la taille de toutes les entrées en tant que 1 et la limite de taille est le nombre d’entrées.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-172">The app could specify the size of all entries as 1, and the size limit is the count of entries.</span></span>

<span data-ttu-id="bfa2e-173">Le code suivant crée un [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) de taille fixe sans unité, accessible par [l’injection de dépendances](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="bfa2e-173">The following code creates a unitless fixed size [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) accessible by [dependency injection](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

<span data-ttu-id="bfa2e-174">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) n’a pas d’unités.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-174">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) does not have units.</span></span> <span data-ttu-id="bfa2e-175">Les entrées mises en cache doivent indiquer la taille dans les unités jugées les plus appropriées si la taille de la mémoire cache a été définie.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-175">Cached entries must specify size in whatever units they deem most appropriate if the cache memory size has been set.</span></span> <span data-ttu-id="bfa2e-176">Tous les utilisateurs d’une instance de cache doivent utiliser le même système d’unité.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-176">All users of a cache instance should use the same unit system.</span></span> <span data-ttu-id="bfa2e-177">Une entrée n’est pas mise en cache si la somme des tailles des entrées mises en cache dépasse la valeur spécifiée par `SizeLimit`.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-177">An entry will not be cached if the sum of the cached entry sizes exceeds the value specified by `SizeLimit`.</span></span> <span data-ttu-id="bfa2e-178">Si aucune limite de taille du cache n’est définie, la taille de cache définie sur l’entrée est ignorée.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-178">If no cache size limit is set, the cache size set on the entry will be ignored.</span></span>

<span data-ttu-id="bfa2e-179">Le code suivant inscrit `MyMemoryCache` avec le conteneur [d’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="bfa2e-179">The following code registers `MyMemoryCache` with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span>

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="bfa2e-180">`MyMemoryCache` est créé comme cache de mémoire indépendant pour les composants qui sont informés de la taille limitée du cache et qui ont la capacité de définir une taille d’entrée de cache en conséquence.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-180">`MyMemoryCache` is created as an independent memory cache for components that are aware of this size limited cache and know how to set cache entry size appropriately.</span></span>

<span data-ttu-id="bfa2e-181">Le code suivant utilise `MyMemoryCache`:</span><span class="sxs-lookup"><span data-stu-id="bfa2e-181">The following code uses `MyMemoryCache`:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

<span data-ttu-id="bfa2e-182">La taille de l’entrée de cache peut être définie par [Size](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) ou par la méthode d’extension [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) :</span><span class="sxs-lookup"><span data-stu-id="bfa2e-182">The size of the cache entry can be set by [Size](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) or the [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) extension method:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a><span data-ttu-id="bfa2e-183">Dépendances de cache</span><span class="sxs-lookup"><span data-stu-id="bfa2e-183">Cache dependencies</span></span>

<span data-ttu-id="bfa2e-184">L’exemple suivant montre comment expirer une entrée de cache si une entrée dépendante expire.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-184">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="bfa2e-185">Un `CancellationChangeToken` est ajouté à l’élément mis en cache.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-185">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="bfa2e-186">Lorsque `Cancel` est appelée sur le `CancellationTokenSource`, les deux entrées du cache sont supprimées.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-186">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="bfa2e-187">Utiliser un `CancellationTokenSource` permet à plusieurs entrées de cache d'être supprimées en tant que groupe.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-187">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="bfa2e-188">Avec le modèle `using` dans le code ci-dessus, les entrées de cache créées à l’intérieur du bloc `using` hériteront des déclencheurs et des paramètres d’expiration.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-188">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="bfa2e-189">Remarques supplémentaires</span><span class="sxs-lookup"><span data-stu-id="bfa2e-189">Additional notes</span></span>

- <span data-ttu-id="bfa2e-190">Lorsque vous utilisez un rappel pour remplir un élément de cache :</span><span class="sxs-lookup"><span data-stu-id="bfa2e-190">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="bfa2e-191">Plusieurs demandes peuvent trouver la valeur de clé mise en cache vide étant donné que le rappel n’est pas terminé.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-191">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  - <span data-ttu-id="bfa2e-192">Il peut en résulter que plusieurs threads remplissent l’élément mis en cache.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-192">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="bfa2e-193">Lorsqu’une entrée de cache est utilisée pour en créer une autre, l’enfant copie les jetons d’expiration et les paramètres d’expiration basés sur le temps de l’entrée parente.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-193">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="bfa2e-194">L’enfant n’est pas expiré par la suppression manuelle ou à la mise à jour de l’entrée parente.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-194">The child isn't expired by manual removal or updating of the parent entry.</span></span>

- <span data-ttu-id="bfa2e-195">Utilisez [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) pour définir que les rappels sont déclenchés une fois que l’entrée de cache est supprimée du cache.</span><span class="sxs-lookup"><span data-stu-id="bfa2e-195">Use [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) to set the callbacks that will be fired after the cache entry is evicted from the cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bfa2e-196">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="bfa2e-196">Additional resources</span></span>

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
