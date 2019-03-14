---
title: Module ASP.NET Core
author: guardrex
description: Découvrez comment configurer le module ASP.NET Core pour héberger des applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 302cfb00127c223aeb5e51e4d0a9ef3cb69b10eb
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029886"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="24a8e-103">Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24a8e-103">ASP.NET Core Module</span></span>

<span data-ttu-id="24a8e-104">Par [Nowak](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [ Justin Kotalik](https://github.com/jkotalik) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="24a8e-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="24a8e-105">Le module ASP.NET Core est un module IIS natif qui se connecte dans le pipeline IIS pour effectuer l’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="24a8e-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="24a8e-106">Héberger une application ASP.NET Core à l’intérieur du processus de travail IIS (`w3wp.exe`), appelé [modèle d’hébergement in-process](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="24a8e-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="24a8e-107">Transférer les requêtes web à une application ASP.NET Core du back-end exécutant le [serveur Kestrel](xref:fundamentals/servers/kestrel), appelé [modèle d’hébergement out-of-process](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="24a8e-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="24a8e-108">Versions Windows prises en charge :</span><span class="sxs-lookup"><span data-stu-id="24a8e-108">Supported Windows versions:</span></span>

* <span data-ttu-id="24a8e-109">Windows 7 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="24a8e-109">Windows 7 or later</span></span>
* <span data-ttu-id="24a8e-110">Windows Server 2008 R2 ou une version ultérieure</span><span class="sxs-lookup"><span data-stu-id="24a8e-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="24a8e-111">Lors de l’hébergement in-process, le module utilise une implémentation du serveur in-process pour IIS, nommée serveur HTTP IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="24a8e-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="24a8e-112">Lors de l’hébergement out-of-process, le module fonctionne uniquement avec Kestrel.</span><span class="sxs-lookup"><span data-stu-id="24a8e-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="24a8e-113">Le module n’est pas compatible avec [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="24a8e-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="24a8e-114">Modèles d'hébergement</span><span class="sxs-lookup"><span data-stu-id="24a8e-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="24a8e-115">Modèle d’hébergement in-process</span><span class="sxs-lookup"><span data-stu-id="24a8e-115">In-process hosting model</span></span>

<span data-ttu-id="24a8e-116">Pour configurer l’hébergement in-process dans une application, ajoutez la propriété `<AspNetCoreHostingModel>` au fichier projet de l’application en lui attribuant la valeur `InProcess` (l’hébergement out-of-process est défini sur `OutOfProcess`) :</span><span class="sxs-lookup"><span data-stu-id="24a8e-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="24a8e-117">Le modèle d’hébergement in-process n’est pas pris en charge pour les applications ASP.NET Core qui ciblent le .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="24a8e-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="24a8e-118">Si la propriété `<AspNetCoreHostingModel>` n’est pas présente dans le fichier, la valeur par défaut est `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="24a8e-119">Les caractéristiques suivantes s’appliquent lors de l’hébergement in-process :</span><span class="sxs-lookup"><span data-stu-id="24a8e-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="24a8e-120">Le serveur HTTP IIS (`IISHttpServer`) est utilisé à la place du serveur [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="24a8e-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="24a8e-121">Pour in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) appelle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> pour :</span><span class="sxs-lookup"><span data-stu-id="24a8e-121">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="24a8e-122">Enregistrer `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-122">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="24a8e-123">Configurer le port et le chemin de base sur lesquels le serveur doit écouter lors de l’exécution derrière le module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="24a8e-123">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="24a8e-124">Configurer l’hôte pour capturer des erreurs de démarrage.</span><span class="sxs-lookup"><span data-stu-id="24a8e-124">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="24a8e-125">L’[attribut requestTimeout](#attributes-of-the-aspnetcore-element) ne s’applique pas à l’hébergement in-process.</span><span class="sxs-lookup"><span data-stu-id="24a8e-125">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="24a8e-126">Le partage d’un pool d’applications entre les applications n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="24a8e-126">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="24a8e-127">Utilisez un pool d’applications par application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-127">Use one app pool per app.</span></span>

* <span data-ttu-id="24a8e-128">Lorsque vous utilisez [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) ou que vous placez manuellement un [fichier app_offline.htm dans le déploiement](xref:host-and-deploy/iis/index#locked-deployment-files), l’application peut ne pas s’arrêter immédiatement si une connexion est ouverte.</span><span class="sxs-lookup"><span data-stu-id="24a8e-128">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="24a8e-129">Par exemple, une connexion websocket peut retarder l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-129">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="24a8e-130">L’architecture (nombre de bits) de l’application et le runtime installé (x64 ou x86) doivent correspondre à l’architecture du pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="24a8e-130">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="24a8e-131">Si vous configurez manuellement l’hôte de l’application avec `WebHostBuilder` (et non [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)), et que l’application est exécutée directement sur le serveur Kestrel (auto-hébergée), appelez `UseKestrel` avant d’appeler `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-131">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="24a8e-132">Si l’ordre est inversé, l’hôte ne parvient pas à démarrer.</span><span class="sxs-lookup"><span data-stu-id="24a8e-132">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="24a8e-133">Les déconnexions du client sont détectées.</span><span class="sxs-lookup"><span data-stu-id="24a8e-133">Client disconnects are detected.</span></span> <span data-ttu-id="24a8e-134">Le jeton d’annulation [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) est annulé quand le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="24a8e-134">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="24a8e-135">Dans ASP.NET Core version 2.2.1 ou antérieure, <xref:System.IO.Directory.GetCurrentDirectory*> retourne le répertoire de travail du processus démarré par IIS, et non le répertoire de l’application (par exemple, *C:\Windows\System32\inetsrv* pour *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="24a8e-135">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="24a8e-136">Pour connaître l’exemple de code qui définit le répertoire actuel de l’application, consultez [CurrentDirectoryHelpers, classe](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="24a8e-136">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="24a8e-137">Appelez la méthode `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-137">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="24a8e-138">Les appels suivants à <xref:System.IO.Directory.GetCurrentDirectory*> fournissent le répertoire de l’application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-138">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>
  
* <span data-ttu-id="24a8e-139">Dans le cas d’un hébergement in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> n’est pas appelé en interne pour initialiser un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="24a8e-139">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="24a8e-140">Par conséquent, une implémentation de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> utilisée pour transformer les revendications après chaque authentification n’est pas activée par défaut.</span><span class="sxs-lookup"><span data-stu-id="24a8e-140">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="24a8e-141">Si vous transformez les revendications avec une implémentation de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, appelez <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> pour ajouter des services d’authentification :</span><span class="sxs-lookup"><span data-stu-id="24a8e-141">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

  ```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
      services.AddAuthentication(IISServerDefaults.AuthenticationScheme);
  }
  
  public void Configure(IApplicationBuilder app)
  {
      app.UseAuthentication();
  }
  ```

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="24a8e-142">Modèle d’hébergement out-of-process</span><span class="sxs-lookup"><span data-stu-id="24a8e-142">Out-of-process hosting model</span></span>

<span data-ttu-id="24a8e-143">Pour configurer une application pour un hébergement out-of-process, utilisez l’une des approches suivantes dans le fichier de projet :</span><span class="sxs-lookup"><span data-stu-id="24a8e-143">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="24a8e-144">Ne spécifiez pas la propriété `<AspNetCoreHostingModel>`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-144">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="24a8e-145">Si la propriété `<AspNetCoreHostingModel>` n’est pas présente dans le fichier, la valeur par défaut est `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-145">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="24a8e-146">Définissez la valeur de la propriété `<AspNetCoreHostingModel>` sur `OutOfProcess` (l’hébergement in-process est défini avec `InProcess`) :</span><span class="sxs-lookup"><span data-stu-id="24a8e-146">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="24a8e-147">Le serveur [Kestrel](xref:fundamentals/servers/kestrel) est utilisé à la place du serveur HTTP IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="24a8e-147">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="24a8e-148">Pour out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) appelle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> pour :</span><span class="sxs-lookup"><span data-stu-id="24a8e-148">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="24a8e-149">Configurer le port et le chemin de base sur lesquels le serveur doit écouter lors de l’exécution derrière le module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="24a8e-149">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="24a8e-150">Configurer l’hôte pour capturer des erreurs de démarrage.</span><span class="sxs-lookup"><span data-stu-id="24a8e-150">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="24a8e-151">Modifications du modèle d’hébergement</span><span class="sxs-lookup"><span data-stu-id="24a8e-151">Hosting model changes</span></span>

<span data-ttu-id="24a8e-152">Si le paramètre `hostingModel` est modifié dans le fichier *web.config* (comme expliqué dans la section [Configuration avec web.config](#configuration-with-webconfig)), le module recycle le processus de travail pour IIS.</span><span class="sxs-lookup"><span data-stu-id="24a8e-152">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="24a8e-153">Pour IIS Express, le module ne recycle pas le processus de travail, mais déclenche plutôt un arrêt approprié du processus IIS Express en cours.</span><span class="sxs-lookup"><span data-stu-id="24a8e-153">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="24a8e-154">La requête suivante à l’application génère un nouveau processus IIS Express.</span><span class="sxs-lookup"><span data-stu-id="24a8e-154">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="24a8e-155">Nom du processus</span><span class="sxs-lookup"><span data-stu-id="24a8e-155">Process name</span></span>

<span data-ttu-id="24a8e-156">`Process.GetCurrentProcess().ProcessName` signale `w3wp`/`iisexpress` (in-process) ou `dotnet` (out-of-process).</span><span class="sxs-lookup"><span data-stu-id="24a8e-156">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="24a8e-157">Le module ASP.NET Core est un module IIS natif qui se connecte dans le pipeline IIS pour transférer les requêtes web aux applications ASP.NET Core du back-end.</span><span class="sxs-lookup"><span data-stu-id="24a8e-157">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="24a8e-158">Versions Windows prises en charge :</span><span class="sxs-lookup"><span data-stu-id="24a8e-158">Supported Windows versions:</span></span>

* <span data-ttu-id="24a8e-159">Windows 7 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="24a8e-159">Windows 7 or later</span></span>
* <span data-ttu-id="24a8e-160">Windows Server 2008 R2 ou une version ultérieure</span><span class="sxs-lookup"><span data-stu-id="24a8e-160">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="24a8e-161">Le module fonctionne seulement avec Kestrel.</span><span class="sxs-lookup"><span data-stu-id="24a8e-161">The module only works with Kestrel.</span></span> <span data-ttu-id="24a8e-162">Le module n’est pas compatible avec [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="24a8e-162">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="24a8e-163">Étant donné que les applications ASP.NET Core s’exécutent dans un processus distinct du processus de travail IIS, le module traite également la gestion des processus.</span><span class="sxs-lookup"><span data-stu-id="24a8e-163">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="24a8e-164">Le module démarre le processus de l’application ASP.NET Core quand la première requête arrive et redémarre l’application si elle plante.</span><span class="sxs-lookup"><span data-stu-id="24a8e-164">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="24a8e-165">Il s’agit essentiellement du même comportement que celui des applications ASP.NET 4.x qui s’exécutent in-process dans IIS et sont gérées par le [service WAS (Windows Activation Service)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="24a8e-165">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="24a8e-166">Le schéma suivant illustre la relation entre IIS, le module ASP.NET Core et une application :</span><span class="sxs-lookup"><span data-stu-id="24a8e-166">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Module ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="24a8e-168">Les requêtes arrivent du web au pilote HTTP.sys en mode noyau.</span><span class="sxs-lookup"><span data-stu-id="24a8e-168">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="24a8e-169">Le pilote route les requêtes vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="24a8e-169">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="24a8e-170">Le module transfère les requêtes à Kestrel sur un port aléatoire pour l’application, qui n’est ni le port 80 ni le port 443.</span><span class="sxs-lookup"><span data-stu-id="24a8e-170">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="24a8e-171">Le module spécifie le port via une variable d’environnement au démarrage, et le middleware d’intégration IIS configure le serveur pour qu’il écoute sur `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-171">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="24a8e-172">Des vérifications supplémentaires sont effectuées, et les requêtes qui ne proviennent pas du module sont rejetées.</span><span class="sxs-lookup"><span data-stu-id="24a8e-172">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="24a8e-173">Le module ne prend pas en charge le transfert HTTPS : les requêtes sont donc transférées via HTTP, même si IIS les reçoit via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="24a8e-173">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="24a8e-174">Dès que Kestrel sélectionne la requête dans le module, celle-ci est envoyée (push) dans le pipeline de middlewares d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="24a8e-174">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="24a8e-175">Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-175">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="24a8e-176">Le middleware ajouté par l’intégration d’IIS met à jour le schéma, l’adresse IP distante et la base du chemin pour prendre en compte le transfert de la requête à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="24a8e-176">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="24a8e-177">La réponse de l’application est ensuite repassée à IIS, qui la renvoie au client HTTP à l’origine de la requête.</span><span class="sxs-lookup"><span data-stu-id="24a8e-177">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="24a8e-178">De nombreux modules natifs, comme l’authentification Windows, restent actifs.</span><span class="sxs-lookup"><span data-stu-id="24a8e-178">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="24a8e-179">Pour obtenir plus d’informations sur les modules IIS actifs avec le module ASP.NET Core, consultez <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="24a8e-179">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="24a8e-180">Le module ASP.NET Core peut également  :</span><span class="sxs-lookup"><span data-stu-id="24a8e-180">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="24a8e-181">Définir des variables d’environnement pour le processus de travail.</span><span class="sxs-lookup"><span data-stu-id="24a8e-181">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="24a8e-182">Journaliser la sortie de stdout dans un stockage de fichiers pour résoudre les problèmes de démarrage.</span><span class="sxs-lookup"><span data-stu-id="24a8e-182">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="24a8e-183">Transférer des jetons d’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="24a8e-183">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="24a8e-184">Comment installer et utiliser le module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24a8e-184">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="24a8e-185">Pour obtenir des instructions sur l’installation et l’utilisation du module ASP.NET Core, consultez <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="24a8e-185">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="24a8e-186">Configuration avec web.config</span><span class="sxs-lookup"><span data-stu-id="24a8e-186">Configuration with web.config</span></span>

<span data-ttu-id="24a8e-187">Le module ASP.NET Core est configuré avec la section `aspNetCore` du nœud `system.webServer` dans le fichier *web.config* du site.</span><span class="sxs-lookup"><span data-stu-id="24a8e-187">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="24a8e-188">Le fichier *web.config* suivant est publié pour un [déploiement dépendant du framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) et configure le module ASP.NET Core pour gérer les demandes du site :</span><span class="sxs-lookup"><span data-stu-id="24a8e-188">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet" 
                  arguments=".\MyApp.dll" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="24a8e-189">Le fichier *web.config* suivant est publié pour un [déploiement autonome](/dotnet/articles/core/deploying/#self-contained-deployments-scd) :</span><span class="sxs-lookup"><span data-stu-id="24a8e-189">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="24a8e-190">La propriété <xref:System.Configuration.SectionInformation.InheritInChildApplications*> est définie sur `false` pour indiquer que les paramètres spécifiés dans l’élément [\<emplacement>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) ne sont pas hérités par les applications qui se trouvent dans un sous-répertoire de l’application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-190">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="24a8e-191">Lorsqu’une application est déployée sur [Azure App Service](https://azure.microsoft.com/services/app-service/), le chemin d’accès `stdoutLogFile` est défini sur `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-191">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="24a8e-192">Le chemin d’accès enregistre les journaux stdout dans le dossier *LogFiles*, un emplacement automatiquement créé par le service.</span><span class="sxs-lookup"><span data-stu-id="24a8e-192">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="24a8e-193">Pour plus d’informations sur la configuration d’une sous-application IIS, consultez <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="24a8e-193">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="24a8e-194">Attributs de l’élément aspNetCore</span><span class="sxs-lookup"><span data-stu-id="24a8e-194">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="24a8e-195">Attribut</span><span class="sxs-lookup"><span data-stu-id="24a8e-195">Attribute</span></span> | <span data-ttu-id="24a8e-196">Description</span><span class="sxs-lookup"><span data-stu-id="24a8e-196">Description</span></span> | <span data-ttu-id="24a8e-197">Par défaut</span><span class="sxs-lookup"><span data-stu-id="24a8e-197">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="24a8e-198">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-198">Optional string attribute.</span></span></p><p><span data-ttu-id="24a8e-199">Arguments pour l’exécutable spécifié dans **processPath**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-199">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="24a8e-200">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-200">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="24a8e-201">Si la valeur est true, la page **502.5 - Échec du processus** est supprimée, et la page de code d’état 502 configurée dans le fichier *web.config* est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="24a8e-201">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="24a8e-202">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-202">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="24a8e-203">Si la valeur est true, le jeton est transmis au processus enfant qui écoute sur %ASPNETCORE_PORT% sous la forme d’un en-tête 'MS-ASPNETCORE-WINAUTHTOKEN' par demande.</span><span class="sxs-lookup"><span data-stu-id="24a8e-203">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="24a8e-204">Il incombe à ce processus d’appeler CloseHandle sur ce jeton par demande.</span><span class="sxs-lookup"><span data-stu-id="24a8e-204">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="24a8e-205">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-205">Optional string attribute.</span></span></p><p><span data-ttu-id="24a8e-206">Spécifie le modèle d’hébergement comme étant in-process (`InProcess`) ou out-of-process (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="24a8e-206">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="24a8e-207">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-207">Optional integer attribute.</span></span></p><p><span data-ttu-id="24a8e-208">Spécifie le nombre d’instances du processus indiqué dans le paramètre **processPath** qui peuvent être lancées par application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-208">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="24a8e-209">&dagger;Pour l’hébergement in-process, la valeur est limitée à `1`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-209">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="24a8e-210">Il est déconseillé de définir `processesPerApplication`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-210">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="24a8e-211">Cet attribut sera supprimé dans une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="24a8e-211">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="24a8e-212">Par défaut : `1`</span><span class="sxs-lookup"><span data-stu-id="24a8e-212">Default: `1`</span></span><br><span data-ttu-id="24a8e-213">Min : `1`</span><span class="sxs-lookup"><span data-stu-id="24a8e-213">Min: `1`</span></span><br><span data-ttu-id="24a8e-214">Max : `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="24a8e-214">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="24a8e-215">Attribut de chaîne requis.</span><span class="sxs-lookup"><span data-stu-id="24a8e-215">Required string attribute.</span></span></p><p><span data-ttu-id="24a8e-216">Chemin d’accès au fichier exécutable lançant un processus d’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24a8e-216">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="24a8e-217">Les chemins d’accès relatifs sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="24a8e-217">Relative paths are supported.</span></span> <span data-ttu-id="24a8e-218">Si le chemin d’accès commence par `.`, il est considéré comme étant relatif par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="24a8e-218">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="24a8e-219">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-219">Optional integer attribute.</span></span></p><p><span data-ttu-id="24a8e-220">Indique le nombre de fois où le processus spécifié dans **processPath** est autorisé à se bloquer par minute.</span><span class="sxs-lookup"><span data-stu-id="24a8e-220">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="24a8e-221">Si cette limite est dépassée, le module arrête le lancement du processus pour le reste de la minute.</span><span class="sxs-lookup"><span data-stu-id="24a8e-221">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="24a8e-222">Non pris en charge avec hébergement in-process.</span><span class="sxs-lookup"><span data-stu-id="24a8e-222">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="24a8e-223">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="24a8e-223">Default: `10`</span></span><br><span data-ttu-id="24a8e-224">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="24a8e-224">Min: `0`</span></span><br><span data-ttu-id="24a8e-225">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="24a8e-225">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="24a8e-226">Attribut timespan facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-226">Optional timespan attribute.</span></span></p><p><span data-ttu-id="24a8e-227">Spécifie la durée pendant laquelle le module ASP.NET Core attend une réponse de la part du processus à l’écoute sur %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="24a8e-227">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="24a8e-228">Dans les versions du module ASP.NET Core fournies avec ASP.NET Core 2.1 ou version ultérieure, la valeur `requestTimeout` est spécifiée en heures, minutes et secondes.</span><span class="sxs-lookup"><span data-stu-id="24a8e-228">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="24a8e-229">Ne s’applique pas à l’hébergement in-process.</span><span class="sxs-lookup"><span data-stu-id="24a8e-229">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="24a8e-230">Pour l’hébergement in-process, le module attend que l’application traite la requête.</span><span class="sxs-lookup"><span data-stu-id="24a8e-230">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="24a8e-231">Par défaut : `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="24a8e-231">Default: `00:02:00`</span></span><br><span data-ttu-id="24a8e-232">Min : `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="24a8e-232">Min: `00:00:00`</span></span><br><span data-ttu-id="24a8e-233">Max : `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="24a8e-233">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="24a8e-234">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-234">Optional integer attribute.</span></span></p><p><span data-ttu-id="24a8e-235">Durée en secondes pendant laquelle le module attend que l’exécutable s’arrête normalement lorsque le fichier *app_offline.htm* est détecté.</span><span class="sxs-lookup"><span data-stu-id="24a8e-235">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="24a8e-236">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="24a8e-236">Default: `10`</span></span><br><span data-ttu-id="24a8e-237">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="24a8e-237">Min: `0`</span></span><br><span data-ttu-id="24a8e-238">Max : `600`</span><span class="sxs-lookup"><span data-stu-id="24a8e-238">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="24a8e-239">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-239">Optional integer attribute.</span></span></p><p><span data-ttu-id="24a8e-240">Durée en secondes pendant laquelle le module attend que le fichier exécutable démarre un processus à l’écoute sur le port.</span><span class="sxs-lookup"><span data-stu-id="24a8e-240">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="24a8e-241">Si cette limite de temps est dépassée, le module met fin au processus.</span><span class="sxs-lookup"><span data-stu-id="24a8e-241">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="24a8e-242">Le module tente de relancer le processus lorsqu’il reçoit une nouvelle requête, puis continue d’essayer de redémarrer le processus pour les requêtes entrantes suivantes, sauf si l’application ne démarre pas **rapidFailsPerMinute** un certain nombre de fois au cours de la dernière minute.</span><span class="sxs-lookup"><span data-stu-id="24a8e-242">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="24a8e-243">La valeur 0 (zéro) n’est **pas** considérée comme un délai infini.</span><span class="sxs-lookup"><span data-stu-id="24a8e-243">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="24a8e-244">Par défaut : `120`</span><span class="sxs-lookup"><span data-stu-id="24a8e-244">Default: `120`</span></span><br><span data-ttu-id="24a8e-245">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="24a8e-245">Min: `0`</span></span><br><span data-ttu-id="24a8e-246">Max : `3600`</span><span class="sxs-lookup"><span data-stu-id="24a8e-246">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="24a8e-247">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-247">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="24a8e-248">Si la valeur est true, les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont redirigés vers le fichier spécifié dans **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-248">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="24a8e-249">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-249">Optional string attribute.</span></span></p><p><span data-ttu-id="24a8e-250">Spécifie le chemin d’accès relatif ou absolu pour lequel les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont consignés.</span><span class="sxs-lookup"><span data-stu-id="24a8e-250">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="24a8e-251">Les chemins d’accès relatifs sont relatifs par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="24a8e-251">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="24a8e-252">Tout chemin d’accès commençant par `.` est relatif par rapport à la racine du site et tous les autres chemins d’accès sont traités comme des chemins d’accès absolus.</span><span class="sxs-lookup"><span data-stu-id="24a8e-252">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="24a8e-253">Tous les dossiers fournis dans le chemin sont créés par le module au moment de la création du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="24a8e-253">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="24a8e-254">Si vous utilisez des délimiteurs de trait de soulignement, un horodatage, un ID de processus et une extension de fichier (*.log*) sont ajoutés au dernier segment du chemin d'accès **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-254">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="24a8e-255">Si `.\logs\stdout` est fourni en tant que valeur, un exemple de journal stdout est enregistré en tant que *stdout_20180205194132_1934.log* dans le dossier *journaux* avec un enregistrement effectué le 05/02/2018 à 19:41:32 et un ID de processus de 1934.</span><span class="sxs-lookup"><span data-stu-id="24a8e-255">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="24a8e-256">Attribut</span><span class="sxs-lookup"><span data-stu-id="24a8e-256">Attribute</span></span> | <span data-ttu-id="24a8e-257">Description</span><span class="sxs-lookup"><span data-stu-id="24a8e-257">Description</span></span> | <span data-ttu-id="24a8e-258">Par défaut</span><span class="sxs-lookup"><span data-stu-id="24a8e-258">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="24a8e-259">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-259">Optional string attribute.</span></span></p><p><span data-ttu-id="24a8e-260">Arguments pour l’exécutable spécifié dans **processPath**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-260">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="24a8e-261">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-261">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="24a8e-262">Si la valeur est true, la page **502.5 - Échec du processus** est supprimée, et la page de code d’état 502 configurée dans le fichier *web.config* est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="24a8e-262">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="24a8e-263">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-263">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="24a8e-264">Si la valeur est true, le jeton est transmis au processus enfant qui écoute sur %ASPNETCORE_PORT% sous la forme d’un en-tête 'MS-ASPNETCORE-WINAUTHTOKEN' par demande.</span><span class="sxs-lookup"><span data-stu-id="24a8e-264">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="24a8e-265">Il incombe à ce processus d’appeler CloseHandle sur ce jeton par demande.</span><span class="sxs-lookup"><span data-stu-id="24a8e-265">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="24a8e-266">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-266">Optional integer attribute.</span></span></p><p><span data-ttu-id="24a8e-267">Spécifie le nombre d’instances du processus indiqué dans le paramètre **processPath** qui peuvent être lancées par application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-267">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="24a8e-268">Il est déconseillé de définir `processesPerApplication`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-268">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="24a8e-269">Cet attribut sera supprimé dans une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="24a8e-269">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="24a8e-270">Par défaut : `1`</span><span class="sxs-lookup"><span data-stu-id="24a8e-270">Default: `1`</span></span><br><span data-ttu-id="24a8e-271">Min : `1`</span><span class="sxs-lookup"><span data-stu-id="24a8e-271">Min: `1`</span></span><br><span data-ttu-id="24a8e-272">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="24a8e-272">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="24a8e-273">Attribut de chaîne requis.</span><span class="sxs-lookup"><span data-stu-id="24a8e-273">Required string attribute.</span></span></p><p><span data-ttu-id="24a8e-274">Chemin d’accès au fichier exécutable lançant un processus d’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24a8e-274">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="24a8e-275">Les chemins d’accès relatifs sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="24a8e-275">Relative paths are supported.</span></span> <span data-ttu-id="24a8e-276">Si le chemin d’accès commence par `.`, il est considéré comme étant relatif par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="24a8e-276">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="24a8e-277">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-277">Optional integer attribute.</span></span></p><p><span data-ttu-id="24a8e-278">Indique le nombre de fois où le processus spécifié dans **processPath** est autorisé à se bloquer par minute.</span><span class="sxs-lookup"><span data-stu-id="24a8e-278">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="24a8e-279">Si cette limite est dépassée, le module arrête le lancement du processus pour le reste de la minute.</span><span class="sxs-lookup"><span data-stu-id="24a8e-279">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="24a8e-280">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="24a8e-280">Default: `10`</span></span><br><span data-ttu-id="24a8e-281">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="24a8e-281">Min: `0`</span></span><br><span data-ttu-id="24a8e-282">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="24a8e-282">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="24a8e-283">Attribut timespan facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-283">Optional timespan attribute.</span></span></p><p><span data-ttu-id="24a8e-284">Spécifie la durée pendant laquelle le module ASP.NET Core attend une réponse de la part du processus à l’écoute sur %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="24a8e-284">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="24a8e-285">Dans les versions du module ASP.NET Core fournies avec ASP.NET Core 2.1 ou version ultérieure, la valeur `requestTimeout` est spécifiée en heures, minutes et secondes.</span><span class="sxs-lookup"><span data-stu-id="24a8e-285">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="24a8e-286">Par défaut : `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="24a8e-286">Default: `00:02:00`</span></span><br><span data-ttu-id="24a8e-287">Min : `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="24a8e-287">Min: `00:00:00`</span></span><br><span data-ttu-id="24a8e-288">Max : `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="24a8e-288">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="24a8e-289">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-289">Optional integer attribute.</span></span></p><p><span data-ttu-id="24a8e-290">Durée en secondes pendant laquelle le module attend que l’exécutable s’arrête normalement lorsque le fichier *app_offline.htm* est détecté.</span><span class="sxs-lookup"><span data-stu-id="24a8e-290">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="24a8e-291">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="24a8e-291">Default: `10`</span></span><br><span data-ttu-id="24a8e-292">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="24a8e-292">Min: `0`</span></span><br><span data-ttu-id="24a8e-293">Max : `600`</span><span class="sxs-lookup"><span data-stu-id="24a8e-293">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="24a8e-294">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-294">Optional integer attribute.</span></span></p><p><span data-ttu-id="24a8e-295">Durée en secondes pendant laquelle le module attend que le fichier exécutable démarre un processus à l’écoute sur le port.</span><span class="sxs-lookup"><span data-stu-id="24a8e-295">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="24a8e-296">Si cette limite de temps est dépassée, le module met fin au processus.</span><span class="sxs-lookup"><span data-stu-id="24a8e-296">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="24a8e-297">Le module tente de relancer le processus lorsqu’il reçoit une nouvelle requête, puis continue d’essayer de redémarrer le processus pour les requêtes entrantes suivantes, sauf si l’application ne démarre pas **rapidFailsPerMinute** un certain nombre de fois au cours de la dernière minute.</span><span class="sxs-lookup"><span data-stu-id="24a8e-297">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="24a8e-298">La valeur 0 (zéro) n’est **pas** considérée comme un délai infini.</span><span class="sxs-lookup"><span data-stu-id="24a8e-298">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="24a8e-299">Par défaut : `120`</span><span class="sxs-lookup"><span data-stu-id="24a8e-299">Default: `120`</span></span><br><span data-ttu-id="24a8e-300">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="24a8e-300">Min: `0`</span></span><br><span data-ttu-id="24a8e-301">Max : `3600`</span><span class="sxs-lookup"><span data-stu-id="24a8e-301">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="24a8e-302">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-302">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="24a8e-303">Si la valeur est true, les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont redirigés vers le fichier spécifié dans **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-303">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="24a8e-304">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-304">Optional string attribute.</span></span></p><p><span data-ttu-id="24a8e-305">Spécifie le chemin d’accès relatif ou absolu pour lequel les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont consignés.</span><span class="sxs-lookup"><span data-stu-id="24a8e-305">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="24a8e-306">Les chemins d’accès relatifs sont relatifs par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="24a8e-306">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="24a8e-307">Tout chemin d’accès commençant par `.` est relatif par rapport à la racine du site et tous les autres chemins d’accès sont traités comme des chemins d’accès absolus.</span><span class="sxs-lookup"><span data-stu-id="24a8e-307">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="24a8e-308">Tous les dossiers spécifiés dans le chemin d’accès doivent exister pour permettre au module de créer le fichier journal.</span><span class="sxs-lookup"><span data-stu-id="24a8e-308">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="24a8e-309">Si vous utilisez des délimiteurs de trait de soulignement, un horodatage, un ID de processus et une extension de fichier (*.log*) sont ajoutés au dernier segment du chemin d'accès **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-309">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="24a8e-310">Si `.\logs\stdout` est fourni en tant que valeur, un exemple de journal stdout est enregistré en tant que *stdout_20180205194132_1934.log* dans le dossier *journaux* avec un enregistrement effectué le 05/02/2018 à 19:41:32 et un ID de processus de 1934.</span><span class="sxs-lookup"><span data-stu-id="24a8e-310">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="24a8e-311">Attribut</span><span class="sxs-lookup"><span data-stu-id="24a8e-311">Attribute</span></span> | <span data-ttu-id="24a8e-312">Description</span><span class="sxs-lookup"><span data-stu-id="24a8e-312">Description</span></span> | <span data-ttu-id="24a8e-313">Par défaut</span><span class="sxs-lookup"><span data-stu-id="24a8e-313">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="24a8e-314">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-314">Optional string attribute.</span></span></p><p><span data-ttu-id="24a8e-315">Arguments pour l’exécutable spécifié dans **processPath**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-315">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="24a8e-316">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-316">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="24a8e-317">Si la valeur est true, la page **502.5 - Échec du processus** est supprimée, et la page de code d’état 502 configurée dans le fichier *web.config* est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="24a8e-317">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="24a8e-318">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-318">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="24a8e-319">Si la valeur est true, le jeton est transmis au processus enfant qui écoute sur %ASPNETCORE_PORT% sous la forme d’un en-tête 'MS-ASPNETCORE-WINAUTHTOKEN' par demande.</span><span class="sxs-lookup"><span data-stu-id="24a8e-319">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="24a8e-320">Il incombe à ce processus d’appeler CloseHandle sur ce jeton par demande.</span><span class="sxs-lookup"><span data-stu-id="24a8e-320">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="24a8e-321">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-321">Optional integer attribute.</span></span></p><p><span data-ttu-id="24a8e-322">Spécifie le nombre d’instances du processus indiqué dans le paramètre **processPath** qui peuvent être lancées par application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-322">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="24a8e-323">Il est déconseillé de définir `processesPerApplication`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-323">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="24a8e-324">Cet attribut sera supprimé dans une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="24a8e-324">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="24a8e-325">Par défaut : `1`</span><span class="sxs-lookup"><span data-stu-id="24a8e-325">Default: `1`</span></span><br><span data-ttu-id="24a8e-326">Min : `1`</span><span class="sxs-lookup"><span data-stu-id="24a8e-326">Min: `1`</span></span><br><span data-ttu-id="24a8e-327">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="24a8e-327">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="24a8e-328">Attribut de chaîne requis.</span><span class="sxs-lookup"><span data-stu-id="24a8e-328">Required string attribute.</span></span></p><p><span data-ttu-id="24a8e-329">Chemin d’accès au fichier exécutable lançant un processus d’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24a8e-329">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="24a8e-330">Les chemins d’accès relatifs sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="24a8e-330">Relative paths are supported.</span></span> <span data-ttu-id="24a8e-331">Si le chemin d’accès commence par `.`, il est considéré comme étant relatif par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="24a8e-331">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="24a8e-332">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-332">Optional integer attribute.</span></span></p><p><span data-ttu-id="24a8e-333">Indique le nombre de fois où le processus spécifié dans **processPath** est autorisé à se bloquer par minute.</span><span class="sxs-lookup"><span data-stu-id="24a8e-333">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="24a8e-334">Si cette limite est dépassée, le module arrête le lancement du processus pour le reste de la minute.</span><span class="sxs-lookup"><span data-stu-id="24a8e-334">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="24a8e-335">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="24a8e-335">Default: `10`</span></span><br><span data-ttu-id="24a8e-336">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="24a8e-336">Min: `0`</span></span><br><span data-ttu-id="24a8e-337">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="24a8e-337">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="24a8e-338">Attribut timespan facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-338">Optional timespan attribute.</span></span></p><p><span data-ttu-id="24a8e-339">Spécifie la durée pendant laquelle le module ASP.NET Core attend une réponse de la part du processus à l’écoute sur %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="24a8e-339">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="24a8e-340">Dans les versions du module ASP.NET Core fournies avec ASP.NET Core 2.0 ou version antérieure, la durée `requestTimeout` doit être spécifiée en minutes entières, sinon la valeur par défaut est de 2 minutes.</span><span class="sxs-lookup"><span data-stu-id="24a8e-340">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="24a8e-341">Par défaut : `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="24a8e-341">Default: `00:02:00`</span></span><br><span data-ttu-id="24a8e-342">Min : `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="24a8e-342">Min: `00:00:00`</span></span><br><span data-ttu-id="24a8e-343">Max : `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="24a8e-343">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="24a8e-344">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-344">Optional integer attribute.</span></span></p><p><span data-ttu-id="24a8e-345">Durée en secondes pendant laquelle le module attend que l’exécutable s’arrête normalement lorsque le fichier *app_offline.htm* est détecté.</span><span class="sxs-lookup"><span data-stu-id="24a8e-345">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="24a8e-346">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="24a8e-346">Default: `10`</span></span><br><span data-ttu-id="24a8e-347">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="24a8e-347">Min: `0`</span></span><br><span data-ttu-id="24a8e-348">Max : `600`</span><span class="sxs-lookup"><span data-stu-id="24a8e-348">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="24a8e-349">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-349">Optional integer attribute.</span></span></p><p><span data-ttu-id="24a8e-350">Durée en secondes pendant laquelle le module attend que le fichier exécutable démarre un processus à l’écoute sur le port.</span><span class="sxs-lookup"><span data-stu-id="24a8e-350">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="24a8e-351">Si cette limite de temps est dépassée, le module met fin au processus.</span><span class="sxs-lookup"><span data-stu-id="24a8e-351">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="24a8e-352">Le module tente de relancer le processus lorsqu’il reçoit une nouvelle requête, puis continue d’essayer de redémarrer le processus pour les requêtes entrantes suivantes, sauf si l’application ne démarre pas **rapidFailsPerMinute** un certain nombre de fois au cours de la dernière minute.</span><span class="sxs-lookup"><span data-stu-id="24a8e-352">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="24a8e-353">Par défaut : `120`</span><span class="sxs-lookup"><span data-stu-id="24a8e-353">Default: `120`</span></span><br><span data-ttu-id="24a8e-354">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="24a8e-354">Min: `0`</span></span><br><span data-ttu-id="24a8e-355">Max : `3600`</span><span class="sxs-lookup"><span data-stu-id="24a8e-355">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="24a8e-356">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-356">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="24a8e-357">Si la valeur est true, les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont redirigés vers le fichier spécifié dans **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-357">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="24a8e-358">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="24a8e-358">Optional string attribute.</span></span></p><p><span data-ttu-id="24a8e-359">Spécifie le chemin d’accès relatif ou absolu pour lequel les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont consignés.</span><span class="sxs-lookup"><span data-stu-id="24a8e-359">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="24a8e-360">Les chemins d’accès relatifs sont relatifs par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="24a8e-360">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="24a8e-361">Tout chemin d’accès commençant par `.` est relatif par rapport à la racine du site et tous les autres chemins d’accès sont traités comme des chemins d’accès absolus.</span><span class="sxs-lookup"><span data-stu-id="24a8e-361">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="24a8e-362">Tous les dossiers spécifiés dans le chemin d’accès doivent exister pour permettre au module de créer le fichier journal.</span><span class="sxs-lookup"><span data-stu-id="24a8e-362">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="24a8e-363">Si vous utilisez des délimiteurs de trait de soulignement, un horodatage, un ID de processus et une extension de fichier (*.log*) sont ajoutés au dernier segment du chemin d'accès **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-363">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="24a8e-364">Si `.\logs\stdout` est fourni en tant que valeur, un exemple de journal stdout est enregistré en tant que *stdout_20180205194132_1934.log* dans le dossier *journaux* avec un enregistrement effectué le 05/02/2018 à 19:41:32 et un ID de processus de 1934.</span><span class="sxs-lookup"><span data-stu-id="24a8e-364">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="24a8e-365">Définition des variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="24a8e-365">Setting environment variables</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="24a8e-366">Des variables d’environnement peuvent être spécifiées pour le processus dans l’attribut `processPath`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-366">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="24a8e-367">Spécifiez une variable d’environnement à l’aide de l’élément enfant `<environmentVariable>` d’un élément de collection `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-367">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="24a8e-368">Les variables d’environnement définies dans cette section prévalent sur les variables d’environnement système.</span><span class="sxs-lookup"><span data-stu-id="24a8e-368">Environment variables set in this section take precedence over system environment variables.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="24a8e-369">Des variables d’environnement peuvent être spécifiées pour le processus dans l’attribut `processPath`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-369">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="24a8e-370">Spécifiez une variable d’environnement à l’aide de l’élément enfant `<environmentVariable>` d’un élément de collection `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-370">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="24a8e-371">Les variables d’environnement définies dans cette section sont en conflit avec les variables d’environnement système qui ont le même nom.</span><span class="sxs-lookup"><span data-stu-id="24a8e-371">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="24a8e-372">Si une variable d’environnement est définie à la fois dans le fichier *web.config* et au niveau du système dans Windows, la valeur provenant du fichier *web.config* est ajoutée à la valeur de la variable d’environnement système (par exemple, `ASPNETCORE_ENVIRONMENT: Development;Development`), ce qui empêche le démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-372">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

::: moniker-end

<span data-ttu-id="24a8e-373">L’exemple suivant définit deux variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="24a8e-373">The following example sets two environment variables.</span></span> <span data-ttu-id="24a8e-374">`ASPNETCORE_ENVIRONMENT` définit l’environnement de l’application sur `Development`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-374">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="24a8e-375">Un développeur peut définir temporairement cette valeur dans le fichier *web.config* afin de forcer le chargement de la [Page d’exception de développeur](xref:fundamentals/error-handling) lors du débogage d’une exception d’application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-375">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="24a8e-376">`CONFIG_DIR` est un exemple de variable d’environnement définie par l’utilisateur, dans laquelle le développeur a écrit du code qui lit la valeur de démarrage afin de former un chemin d’accès pour le chargement du fichier de configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-376">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="24a8e-377">Une alternative à la définition directe de l’environnement dans le fichier *web.config* consiste à inclure la propriété `<EnvironmentName>` dans le profil de publication (*.pubxml*) ou le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="24a8e-377">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="24a8e-378">Cette approche définit l’environnement dans *web.config* lorsque le projet est publié :</span><span class="sxs-lookup"><span data-stu-id="24a8e-378">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="24a8e-379">Affectez uniquement `Development` à la variable d’environnement `ASPNETCORE_ENVIRONMENT` sur les serveurs de test et de préproduction non accessibles aux réseaux non approuvés, par exemple Internet.</span><span class="sxs-lookup"><span data-stu-id="24a8e-379">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="24a8e-380">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="24a8e-380">app_offline.htm</span></span>

<span data-ttu-id="24a8e-381">Si un fichier portant le nom *app_offline.htm* est détecté dans le répertoire racine d’une application, le module ASP.NET Core tente d’arrêter normalement l’application, puis interrompt le traitement des requêtes entrantes.</span><span class="sxs-lookup"><span data-stu-id="24a8e-381">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="24a8e-382">Si l’application est toujours active après le nombre de secondes défini dans `shutdownTimeLimit`, le module ASP.NET Core met fin au processus en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="24a8e-382">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="24a8e-383">Tant que le fichier *app_offline.htm* est présent, le module ASP.NET Core répond aux requêtes en renvoyant le contenu du fichier *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="24a8e-383">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="24a8e-384">Lorsque le fichier *app_offline.htm* est supprimé, la requête suivante démarre l’application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-384">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="24a8e-385">Lorsque vous utilisez le modèle d’hébergement out-of-process, l’application peut ne pas s’arrêter immédiatement s’il existe une connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="24a8e-385">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="24a8e-386">Par exemple, une connexion websocket peut retarder l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-386">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="24a8e-387">Page d’erreur de démarrage</span><span class="sxs-lookup"><span data-stu-id="24a8e-387">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="24a8e-388">Les hébergements in-process et out-of-process produisent des pages d’erreurs personnalisées quand ils ne parviennent pas à démarrer l’application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-388">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="24a8e-389">Si le module ASP.NET Core ne trouve pas le gestionnaire de requêtes in-process ou out-of-process, une page de code d’état *500.0 - Échec de chargement du gestionnaire in-process/out-of-process* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="24a8e-389">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="24a8e-390">Pour l’hébergement in-process, si le module ASP.NET Core ne peut pas démarrer l’application, une page de code d’état *500.30 - Échec de démarrage* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="24a8e-390">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="24a8e-391">Pour l’hébergement out-of-process, si le module ASP.NET Core ne peut pas lancer le processus backend ou que ce dernier démarre mais ne parvient pas à écouter sur le port configuré, une page de code d’état *502.5 - Échec du processus* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="24a8e-391">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="24a8e-392">Pour supprimer cette page et revenir à la page IIS de codes d’état 5xx par défaut, utilisez l’attribut `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-392">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="24a8e-393">Pour plus d’informations sur la configuration des messages d’erreur personnalisés, consultez [Erreurs HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="24a8e-393">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="24a8e-394">Si le module ASP.NET Core ne peut pas lancer le processus backend ou que ce dernier démarre mais ne parvient pas à écouter sur le port configuré, une page de code d’état *502.5 - Échec du processus* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="24a8e-394">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="24a8e-395">Pour supprimer cette page et revenir à la page IIS de code d’état 502 par défaut, utilisez l’attribut `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="24a8e-395">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="24a8e-396">Pour plus d’informations sur la configuration des messages d’erreur personnalisés, consultez [Erreurs HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="24a8e-396">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Page de code d’état 502.5 Échec du processus](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="24a8e-398">Création et redirection de journal</span><span class="sxs-lookup"><span data-stu-id="24a8e-398">Log creation and redirection</span></span>

<span data-ttu-id="24a8e-399">Le module ASP.NET Core redirige la sortie de console stdout et stderr vers le disque si les attributs `stdoutLogEnabled` et `stdoutLogFile` de l’élément `aspNetCore` sont définis.</span><span class="sxs-lookup"><span data-stu-id="24a8e-399">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="24a8e-400">Tous les dossiers du chemin `stdoutLogFile` sont créés par le module au moment de la création du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="24a8e-400">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="24a8e-401">Le pool d’applications doit avoir un accès en écriture à l’emplacement auquel les journaux sont écrits (utilisez `IIS AppPool\<app_pool_name>` pour fournir les autorisations d’écriture).</span><span class="sxs-lookup"><span data-stu-id="24a8e-401">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="24a8e-402">Aucune rotation n’est appliquée aux journaux, sauf en cas de recyclage/redémarrage du processus.</span><span class="sxs-lookup"><span data-stu-id="24a8e-402">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="24a8e-403">Il incombe à l’hébergeur de limiter l’espace disque utilisé par les journaux.</span><span class="sxs-lookup"><span data-stu-id="24a8e-403">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="24a8e-404">L’utilisation du journal stdout est uniquement recommandée pour résoudre les problèmes de démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-404">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="24a8e-405">N’utilisez pas le journal stdout à des fins de journalisation d’application générale.</span><span class="sxs-lookup"><span data-stu-id="24a8e-405">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="24a8e-406">Pour journaliser la routine d’une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et appliquez une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="24a8e-406">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="24a8e-407">Pour plus d’informations, consultez [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="24a8e-407">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="24a8e-408">Un horodatage et une extension de fichier sont ajoutés automatiquement quand le fichier journal est créé.</span><span class="sxs-lookup"><span data-stu-id="24a8e-408">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="24a8e-409">Le nom du fichier journal est créé en ajoutant l’horodatage, un ID de processus et une extension de fichier (*.log*) au dernier segment du chemin d'accès `stdoutLogFile` (généralement *stdout*), séparés par des traits de soulignement.</span><span class="sxs-lookup"><span data-stu-id="24a8e-409">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="24a8e-410">Si le chemin d'accès `stdoutLogFile` se termine par *stdout*, un journal pour une application avec un PID de 1934 créé le 5/2/2018 à 19:42:32 affiche le nom de fichier *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="24a8e-410">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="24a8e-411">Si `stdoutLogEnabled` a la valeur false, les erreurs qui se produisent au moment du démarrage de l’application sont capturées et émises dans le journal des événements (30 Ko maximum).</span><span class="sxs-lookup"><span data-stu-id="24a8e-411">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="24a8e-412">Après le démarrage, tous les journaux supplémentaires sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="24a8e-412">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="24a8e-413">L’exemple d’élément `aspNetCore` suivant configure la journalisation stdout pour une application hébergée dans Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="24a8e-413">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="24a8e-414">Un chemin d’accès local ou un chemin de partage réseau peut être utilisé pour une journalisation locale.</span><span class="sxs-lookup"><span data-stu-id="24a8e-414">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="24a8e-415">Vérifiez que l’identité de l’utilisateur AppPool à l’autorisation d’écrire dans le chemin d’accès fourni.</span><span class="sxs-lookup"><span data-stu-id="24a8e-415">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="24a8e-416">Journaux de diagnostic améliorés</span><span class="sxs-lookup"><span data-stu-id="24a8e-416">Enhanced diagnostic logs</span></span>

<span data-ttu-id="24a8e-417">Le module ASP.NET Core est configurable pour proposer des journaux de diagnostic améliorés.</span><span class="sxs-lookup"><span data-stu-id="24a8e-417">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="24a8e-418">Ajoutez l’élément `<handlerSettings>` à l’élément `<aspNetCore>` dans *web.config*. L’affectation de la valeur `TRACE` à `debugLevel` expose une fidélité plus élevée des informations de diagnostic :</span><span class="sxs-lookup"><span data-stu-id="24a8e-418">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="24a8e-419">Les valeurs du niveau de débogage (`debugLevel`) peuvent inclure à la fois le niveau et l’emplacement.</span><span class="sxs-lookup"><span data-stu-id="24a8e-419">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="24a8e-420">Niveaux (dans l’ordre du moins au plus détaillé) :</span><span class="sxs-lookup"><span data-stu-id="24a8e-420">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="24a8e-421">ERROR</span><span class="sxs-lookup"><span data-stu-id="24a8e-421">ERROR</span></span>
* <span data-ttu-id="24a8e-422">WARNING</span><span class="sxs-lookup"><span data-stu-id="24a8e-422">WARNING</span></span>
* <span data-ttu-id="24a8e-423">INFO</span><span class="sxs-lookup"><span data-stu-id="24a8e-423">INFO</span></span>
* <span data-ttu-id="24a8e-424">TRACE</span><span class="sxs-lookup"><span data-stu-id="24a8e-424">TRACE</span></span>

<span data-ttu-id="24a8e-425">Emplacements (plusieurs emplacements sont autorisés) :</span><span class="sxs-lookup"><span data-stu-id="24a8e-425">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="24a8e-426">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="24a8e-426">CONSOLE</span></span>
* <span data-ttu-id="24a8e-427">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="24a8e-427">EVENTLOG</span></span>
* <span data-ttu-id="24a8e-428">FICHIER</span><span class="sxs-lookup"><span data-stu-id="24a8e-428">FILE</span></span>

<span data-ttu-id="24a8e-429">Les paramètres de gestionnaire peuvent également être fournis par le biais de variables d’environnement :</span><span class="sxs-lookup"><span data-stu-id="24a8e-429">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="24a8e-430">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Chemin du fichier journal de débogage.</span><span class="sxs-lookup"><span data-stu-id="24a8e-430">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="24a8e-431">(Par défaut : *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="24a8e-431">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="24a8e-432">`ASPNETCORE_MODULE_DEBUG` &ndash; Paramètre du niveau de débogage.</span><span class="sxs-lookup"><span data-stu-id="24a8e-432">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="24a8e-433">Ne laissez **pas** la journalisation du débogage activée dans le déploiement plus longtemps que nécessaire pour résoudre un problème.</span><span class="sxs-lookup"><span data-stu-id="24a8e-433">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="24a8e-434">La taille du journal n’est pas limitée.</span><span class="sxs-lookup"><span data-stu-id="24a8e-434">The size of the log isn't limited.</span></span> <span data-ttu-id="24a8e-435">Si vous laissez la journalisation du débogage activée, vous risquez d’épuiser l’espace disque disponible et de bloquer le serveur ou le service d’application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-435">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="24a8e-436">Consultez [Configuration avec web.config](#configuration-with-webconfig) pour obtenir un exemple d’élément `aspNetCore` dans le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="24a8e-436">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="24a8e-437">La configuration du proxy utilise le protocole HTTP et un jeton d’appariement</span><span class="sxs-lookup"><span data-stu-id="24a8e-437">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="24a8e-438">*S’applique uniquement à l’hébergement out-of-process.*</span><span class="sxs-lookup"><span data-stu-id="24a8e-438">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="24a8e-439">Le proxy créé entre le module ASP.NET Core et Kestrel utilise le protocole HTTP.</span><span class="sxs-lookup"><span data-stu-id="24a8e-439">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="24a8e-440">Utiliser HTTP est une optimisation des performances où le trafic entre le module et Kestrel a lieu sur une adresse de bouclage hors de l’interface réseau.</span><span class="sxs-lookup"><span data-stu-id="24a8e-440">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="24a8e-441">Il n’existe aucun risque d’écoute clandestine du trafic entre le module et Kestrel à partir d’un emplacement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="24a8e-441">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="24a8e-442">Un jeton d’appariement est utilisé pour garantir que les demandes reçues par Kestrel ont été traitées par IIS et ne proviennent pas d’une autre source.</span><span class="sxs-lookup"><span data-stu-id="24a8e-442">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="24a8e-443">Le jeton d’appariement est créé et défini dans une variable d’environnement (`ASPNETCORE_TOKEN`) par le module.</span><span class="sxs-lookup"><span data-stu-id="24a8e-443">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="24a8e-444">Le jeton d’appariement est également défini dans un en-tête (`MS-ASPNETCORE-TOKEN`) sur toutes les demandes traitées.</span><span class="sxs-lookup"><span data-stu-id="24a8e-444">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="24a8e-445">L'intergiciel IIS vérifie chaque demande qu’il reçoit pour confirmer que la valeur d’en-tête du jeton d'appariement correspond à la valeur de la variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="24a8e-445">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="24a8e-446">Si les valeurs de jeton ne correspondent pas, la demande est connectée et rejetée.</span><span class="sxs-lookup"><span data-stu-id="24a8e-446">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="24a8e-447">La variable d’environnement du jeton d'appariement et le trafic entre le module et Kestrel ne sont pas accessibles à partir d’un emplacement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="24a8e-447">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="24a8e-448">Sans connaître la valeur du jeton d'appariement, une personne malveillante ne peut pas soumettre des demandes qui contournent la vérification de l’intergiciel IIS.</span><span class="sxs-lookup"><span data-stu-id="24a8e-448">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="24a8e-449">Module ASP.NET Core avec une configuration partagée IIS</span><span class="sxs-lookup"><span data-stu-id="24a8e-449">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="24a8e-450">Le programme d’installation du module ASP.NET Core s’exécute avec les privilèges du compte **TrustedInstaller**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-450">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="24a8e-451">Comme le compte système local n’a pas l’autorisation de modifier le chemin de partage utilisé par la configuration partagée IIS, le programme d’installation génère une erreur d’accès refusé pendant la tentative de configuration des paramètres du module dans le fichier *applicationHost.config* du partage.</span><span class="sxs-lookup"><span data-stu-id="24a8e-451">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="24a8e-452">Quand une configuration partagée IIS est utilisée sur la même machine que l’installation IIS, il convient d’exécuter le programme d’installation du bundle d’hébergement ASP.NET Core avec le paramètre `OPT_NO_SHARED_CONFIG_CHECK` défini sur `1` :</span><span class="sxs-lookup"><span data-stu-id="24a8e-452">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="24a8e-453">Quand le chemin de la configuration partagée ne se trouve pas sur la même machine que l’installation IIS, effectuez ces étapes :</span><span class="sxs-lookup"><span data-stu-id="24a8e-453">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="24a8e-454">Désactivez la configuration partagée IIS.</span><span class="sxs-lookup"><span data-stu-id="24a8e-454">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="24a8e-455">Exécutez le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="24a8e-455">Run the installer.</span></span>
1. <span data-ttu-id="24a8e-456">Exportez le fichier *applicationHost.config* mis à jour vers le partage.</span><span class="sxs-lookup"><span data-stu-id="24a8e-456">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="24a8e-457">Réactivez la configuration partagée IIS.</span><span class="sxs-lookup"><span data-stu-id="24a8e-457">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="24a8e-458">Lorsque vous utilisez une configuration partagée IIS, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="24a8e-458">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="24a8e-459">Désactivez la configuration partagée IIS.</span><span class="sxs-lookup"><span data-stu-id="24a8e-459">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="24a8e-460">Exécutez le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="24a8e-460">Run the installer.</span></span>
1. <span data-ttu-id="24a8e-461">Exportez le fichier *applicationHost.config* mis à jour vers le partage.</span><span class="sxs-lookup"><span data-stu-id="24a8e-461">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="24a8e-462">Réactivez la configuration partagée IIS.</span><span class="sxs-lookup"><span data-stu-id="24a8e-462">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="application-initialization"></a><span data-ttu-id="24a8e-463">Initialisation d’application</span><span class="sxs-lookup"><span data-stu-id="24a8e-463">Application Initialization</span></span>

<span data-ttu-id="24a8e-464">[L’Initialisation d’application IIS](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) est une fonctionnalité IIS qui envoie une requête HTTP à l’application lorsque le pool d’applications démarre ou est recyclé.</span><span class="sxs-lookup"><span data-stu-id="24a8e-464">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="24a8e-465">La requête déclenche le démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-465">The request triggers the app to start.</span></span> <span data-ttu-id="24a8e-466">L’Initialisation de l’application peut être utilisée à la fois par le [modèle d’hébergement in-process](xref:fundamentals/servers/index#in-process-hosting-model) et ke [modèle d’hébergement out-of-process](xref:fundamentals/servers/index#out-of-process-hosting-model) avec le module ASP.NET Core version 2.</span><span class="sxs-lookup"><span data-stu-id="24a8e-466">Application Initialization can be used by both the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model) and [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model) with the ASP.NET Core Module version 2.</span></span>

<span data-ttu-id="24a8e-467">Pour activer l’Initialisation d’application :</span><span class="sxs-lookup"><span data-stu-id="24a8e-467">To enable Application Initialization:</span></span>

1. <span data-ttu-id="24a8e-468">Vérifiez que la fonctionnalité de rôle Initialisation d’application IIS est activée :</span><span class="sxs-lookup"><span data-stu-id="24a8e-468">Confirm that the IIS Application Initialization role feature in enabled:</span></span>
   * <span data-ttu-id="24a8e-469">Sur Windows 7 ou version ultérieure : Accédez à **Panneau de configuration** > **Programmes** > **Programmes et fonctionnalités** > **Activer ou désactiver des fonctionnalités Windows** (à gauche de l’écran).</span><span class="sxs-lookup"><span data-stu-id="24a8e-469">On Windows 7 or later: Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="24a8e-470">Ouvrez **Internet Information Services** > **Services World Wide Web** > **Fonctionnalités de développement d’applications**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-470">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="24a8e-471">Cochez la case **Initialisation d’application**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-471">Select the check box for **Application Initialization**.</span></span>
   * <span data-ttu-id="24a8e-472">Sur Windows Server 2008 R2 ou version ultérieure, ouvrez **l’assistant Ajouter des rôles et des fonctionnalités**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-472">On Windows Server 2008 R2 or later, open the **Add Roles and Features Wizard**.</span></span> <span data-ttu-id="24a8e-473">Lorsque vous atteignez le panneau **Sélectionner des services de rôle**, ouvrez le nœud **Développement d’applications** et cochez la case **Initialisation d’application**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-473">When you reach the **Select role services** panel, open the **Application Development** node and select the **Application Initialization** check box.</span></span>
1. <span data-ttu-id="24a8e-474">Dans IIS Manager, sélectionnez **Pools d’applications** dans le volet **Connexions**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-474">In IIS Manager, select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="24a8e-475">Sélectionnez le pool d’applications de l’application dans la liste.</span><span class="sxs-lookup"><span data-stu-id="24a8e-475">Select the app's app pool in the list.</span></span>
1. <span data-ttu-id="24a8e-476">Sélectionnez **Paramètres avancés** sous **Modifier le pool d’applications** dans le volet **Actions**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-476">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="24a8e-477">Définissez **Mode de démarrage** sur **AlwaysRunning**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-477">Set **Start Mode** to **AlwaysRunning**.</span></span>
1. <span data-ttu-id="24a8e-478">Ouvrez le nœud **Sites** dans le panneau **Connexions**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-478">Open the **Sites** node in the **Connections** panel.</span></span>
1. <span data-ttu-id="24a8e-479">Sélectionnez l’application.</span><span class="sxs-lookup"><span data-stu-id="24a8e-479">Select the app.</span></span>
1. <span data-ttu-id="24a8e-480">Sélectionnez **Paramètres avancés** sous **Gérer le site web** dans le volet **Actions**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-480">Select **Advanced Settings** under **Manage Website** in the **Actions** panel.</span></span>
1. <span data-ttu-id="24a8e-481">Définissez **Préchargement activé** sur **True**.</span><span class="sxs-lookup"><span data-stu-id="24a8e-481">Set **Preload Enabled** to **True**.</span></span>

<span data-ttu-id="24a8e-482">Pour plus d’informations, consultez [Initialisation d’application IIS 8.0](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization).</span><span class="sxs-lookup"><span data-stu-id="24a8e-482">For more information, see [IIS 8.0 Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization).</span></span>

<span data-ttu-id="24a8e-483">Les applications qui utilisent le [modèle d’hébergement out-of-process](xref:fundamentals/servers/index#out-of-process-hosting-model) doivent utiliser un service externe pour effectuer régulièrement un test ping de l’application afin de garantir son fonctionnement.</span><span class="sxs-lookup"><span data-stu-id="24a8e-483">Apps that use the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model) must use an external service to periodically ping the app in order to keep it running.</span></span>

::: moniker-end

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="24a8e-484">Version du module et journaux du programme d’installation du bundle d’hébergement</span><span class="sxs-lookup"><span data-stu-id="24a8e-484">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="24a8e-485">Pour déterminer la version du module ASP.NET Core installé :</span><span class="sxs-lookup"><span data-stu-id="24a8e-485">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="24a8e-486">Sur le système hôte, accédez à *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="24a8e-486">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="24a8e-487">Recherchez le fichier *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="24a8e-487">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="24a8e-488">Cliquez avec le bouton droit sur le fichier, puis sélectionnez **Propriétés** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="24a8e-488">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="24a8e-489">Sélectionnez l'onglet **Détails**. Le **version du fichier** et la **version du produit** représentent la version installée du module.</span><span class="sxs-lookup"><span data-stu-id="24a8e-489">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="24a8e-490">Les journaux du programme d’installation du bundle d’hébergement du module se trouvent dans *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Le fichier est nommé *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="24a8e-490">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="24a8e-491">Emplacements des fichiers du module, du schéma et de configuration</span><span class="sxs-lookup"><span data-stu-id="24a8e-491">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="24a8e-492">Module</span><span class="sxs-lookup"><span data-stu-id="24a8e-492">Module</span></span>

<span data-ttu-id="24a8e-493">**IIS (x86/amd64) :**</span><span class="sxs-lookup"><span data-stu-id="24a8e-493">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="24a8e-494">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="24a8e-494">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="24a8e-495">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="24a8e-495">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="24a8e-496">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="24a8e-496">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="24a8e-497">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="24a8e-497">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="24a8e-498">**IIS Express (x86/amd64) :**</span><span class="sxs-lookup"><span data-stu-id="24a8e-498">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="24a8e-499">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="24a8e-499">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="24a8e-500">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="24a8e-500">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="24a8e-501">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="24a8e-501">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="24a8e-502">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="24a8e-502">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="24a8e-503">Schéma</span><span class="sxs-lookup"><span data-stu-id="24a8e-503">Schema</span></span>

<span data-ttu-id="24a8e-504">**IIS**</span><span class="sxs-lookup"><span data-stu-id="24a8e-504">**IIS**</span></span>

   * <span data-ttu-id="24a8e-505">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="24a8e-505">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="24a8e-506">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="24a8e-506">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="24a8e-507">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="24a8e-507">**IIS Express**</span></span>

   * <span data-ttu-id="24a8e-508">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="24a8e-508">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="24a8e-509">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="24a8e-509">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="24a8e-510">Configuration</span><span class="sxs-lookup"><span data-stu-id="24a8e-510">Configuration</span></span>

<span data-ttu-id="24a8e-511">**IIS**</span><span class="sxs-lookup"><span data-stu-id="24a8e-511">**IIS**</span></span>

   * <span data-ttu-id="24a8e-512">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="24a8e-512">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="24a8e-513">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="24a8e-513">**IIS Express**</span></span>

   * <span data-ttu-id="24a8e-514">Visual Studio : {RACINE DE L’APPLICATION}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="24a8e-514">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>
   
   * <span data-ttu-id="24a8e-515">*iisexpress.exe* CLI : %PROFILUTILISATEUR%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="24a8e-515">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="24a8e-516">Vous trouverez les fichiers en recherchant *aspnetcore* dans le fichier *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="24a8e-516">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="24a8e-517">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="24a8e-517">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="24a8e-518">Dépôt GitHub du module ASP.NET Core (source de référence)</span><span class="sxs-lookup"><span data-stu-id="24a8e-518">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
