---
title: Accéder à HttpContext dans ASP.NET Core
author: coderandhiker
description: Découvrez comment accéder à HttpContext dans ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/27/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: 446882297524af3cbaed3ba7f941935debf5e7f4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57026426"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="4a3a5-103">Accéder à HttpContext dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a3a5-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="4a3a5-104">Les applications ASP.NET Core accèdent à `HttpContext` via l’interface [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) et son implémentation par défaut [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span><span class="sxs-lookup"><span data-stu-id="4a3a5-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span> <span data-ttu-id="4a3a5-105">Il est seulement nécessaire d’utiliser `IHttpContextAccessor` quand vous avez besoin d’accéder à `HttpContext` à l’intérieur d’un service.</span><span class="sxs-lookup"><span data-stu-id="4a3a5-105">It's only necessary to use `IHttpContextAccessor` when you need access to the `HttpContext` inside a service.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="4a3a5-106">Utiliser HttpContext à partir de Razor Pages</span><span class="sxs-lookup"><span data-stu-id="4a3a5-106">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="4a3a5-107">[PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) de Razor Pages expose la propriété [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) :</span><span class="sxs-lookup"><span data-stu-id="4a3a5-107">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

```csharp
public class AboutModel : PageModel
{
    public string Message { get; set; }

    public void OnGet()
    {
        Message = HttpContext.Request.PathBase;
    }
}
```

::: moniker-end

## <a name="use-httpcontext-from-a-razor-view"></a><span data-ttu-id="4a3a5-108">Utiliser HttpContext à partir d’une vue Razor</span><span class="sxs-lookup"><span data-stu-id="4a3a5-108">Use HttpContext from a Razor view</span></span>

<span data-ttu-id="4a3a5-109">Les vues Razor exposent `HttpContext` directement par l’intermédiaire d’une propriété [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) de la vue.</span><span class="sxs-lookup"><span data-stu-id="4a3a5-109">Razor views expose the `HttpContext` directly via a [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) property on the view.</span></span> <span data-ttu-id="4a3a5-110">L’exemple suivant récupère le nom d’utilisateur actif dans une application Intranet en utilisant l’authentification Windows :</span><span class="sxs-lookup"><span data-stu-id="4a3a5-110">The following example retrieves the current username in an Intranet app using Windows Authentication:</span></span>

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="4a3a5-111">Utiliser HttpContext à partir d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="4a3a5-111">Use HttpContext from a controller</span></span>

<span data-ttu-id="4a3a5-112">Les contrôleurs exposent la propriété [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) :</span><span class="sxs-lookup"><span data-stu-id="4a3a5-112">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult About()
    {
        var pathBase = HttpContext.Request.PathBase;
        // Do something with the PathBase.

        return View();
    }
}
```

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="4a3a5-113">Utiliser HttpContext à partir d’un intergiciel</span><span class="sxs-lookup"><span data-stu-id="4a3a5-113">Use HttpContext from middleware</span></span>

<span data-ttu-id="4a3a5-114">Lorsque vous travaillez avec des composants d’intergiciel personnalisés, `HttpContext` est transmis à la méthode `Invoke` ou `InvokeAsync` et accessible lorsque l’intergiciel est configuré :</span><span class="sxs-lookup"><span data-stu-id="4a3a5-114">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="4a3a5-115">Utiliser HttpContext à partir de composants personnalisés</span><span class="sxs-lookup"><span data-stu-id="4a3a5-115">Use HttpContext from custom components</span></span>

<span data-ttu-id="4a3a5-116">Pour les autres infrastructures et les composants personnalisés qui requièrent l’accès à `HttpContext`, l’approche recommandée consiste à inscrire une dépendance à l’aide du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) intégré.</span><span class="sxs-lookup"><span data-stu-id="4a3a5-116">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="4a3a5-117">Le conteneur d’injection de dépendances fournit le `IHttpContextAccessor` à toutes les classes qui le déclarent en tant que dépendance dans leurs constructeurs.</span><span class="sxs-lookup"><span data-stu-id="4a3a5-117">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc()
         .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc();
     services.AddSingleton<IHttpContextAccessor, HttpContextAccessor>();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

<span data-ttu-id="4a3a5-118">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="4a3a5-118">In the following example:</span></span>

* <span data-ttu-id="4a3a5-119">`UserRepository` déclare sa dépendance par rapport à `IHttpContextAccessor`.</span><span class="sxs-lookup"><span data-stu-id="4a3a5-119">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="4a3a5-120">Il y a dépendance lorsque l’instance de dépendances résout la chaîne de dépendance et crée une instance de `UserRepository`.</span><span class="sxs-lookup"><span data-stu-id="4a3a5-120">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

```csharp
public class UserRepository : IUserRepository
{
    private readonly IHttpContextAccessor _httpContextAccessor;

    public UserRepository(IHttpContextAccessor httpContextAccessor)
    {
        _httpContextAccessor = httpContextAccessor;
    }

    public void LogCurrentUser()
    {
        var username = _httpContextAccessor.HttpContext.User.Identity.Name;
        service.LogAccessRequest(username);
    }
}
```

## <a name="httpcontext-access-from-a-background-thread"></a><span data-ttu-id="4a3a5-121">Accès de HttpContext à partir d’un thread en arrière-plan</span><span class="sxs-lookup"><span data-stu-id="4a3a5-121">HttpContext access from a background thread</span></span>

<span data-ttu-id="4a3a5-122">`HttpContext` n’est pas thread‑safe.</span><span class="sxs-lookup"><span data-stu-id="4a3a5-122">`HttpContext` is not thread-safe.</span></span> <span data-ttu-id="4a3a5-123">La lecture ou l’écriture de propriétés de `HttpContext` en dehors du traitement d’une requête peut entraîner une exception `NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="4a3a5-123">Reading or writing properties of the `HttpContext` outside of processing a request can result in a `NullReferenceException`.</span></span>

> [!NOTE]
> <span data-ttu-id="4a3a5-124">L’utilisation de `HttpContext` en dehors du traitement d’une requête entraîne souvent une exception `NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="4a3a5-124">Using `HttpContext` outside of processing a request often results in a `NullReferenceException`.</span></span> <span data-ttu-id="4a3a5-125">Si votre application génère des `NullReferenceException` sporadiques, examinez les parties du code qui démarrent le traitement en arrière-plan ou qui continue le traitement après qu’une requête a abouti.</span><span class="sxs-lookup"><span data-stu-id="4a3a5-125">If your app generates sporadic `NullReferenceException`s , review parts of the code that start background processing, or that continue processing after a request completes.</span></span> <span data-ttu-id="4a3a5-126">Recherchez des erreurs comme une méthode de contrôleur qui serait définie en tant que `async void`.</span><span class="sxs-lookup"><span data-stu-id="4a3a5-126">Look for a mistakes like defining a controller method as `async void`.</span></span>

<span data-ttu-id="4a3a5-127">Pour effectuer un travail en arrière-plan en toute sécurité avec des données `HttpContext` :</span><span class="sxs-lookup"><span data-stu-id="4a3a5-127">To safely perform background work with `HttpContext` data:</span></span>

* <span data-ttu-id="4a3a5-128">Copiez les données nécessaires pendant le traitement de la requête.</span><span class="sxs-lookup"><span data-stu-id="4a3a5-128">Copy the required data during request processing.</span></span>
* <span data-ttu-id="4a3a5-129">Transmettez les données copiées à une tâche en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="4a3a5-129">Pass the copied data to a background task.</span></span>

<span data-ttu-id="4a3a5-130">Pour éviter un code unsafe, ne transmettez jamais `HttpContext` dans une méthode qui effectue un travail en arrière-plan. Transmettez plutôt les données nécessaires.</span><span class="sxs-lookup"><span data-stu-id="4a3a5-130">To avoid unsafe code, never pass the `HttpContext` into a method that does background work - pass the data you need instead.</span></span>

```csharp
public class EmailController
{
    public ActionResult SendEmail(string email)
    {
        var correlationId = HttpContext.Request.Headers["x-correlation-id"].ToString();

        // Starts sending an email, but doesn't wait for it to complete
        _ = SendEmailCore(correlationId);
        return View();
    }

    private async Task SendEmailCore(string correlationId)
    {
        // send the email
    }
}

