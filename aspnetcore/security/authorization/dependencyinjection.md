---
title: Injection de dépendances dans les gestionnaires d’exigences dans ASP.NET Core
author: rick-anderson
description: Découvrez comment injecter des gestionnaires d’exigences d’autorisation dans une application ASP.NET Core à l’aide de l’injection de dépendances.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029896"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a><span data-ttu-id="09ace-103">Injection de dépendances dans les gestionnaires d’exigences dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09ace-103">Dependency injection in requirement handlers in ASP.NET Core</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="09ace-104">[Les gestionnaires d’autorisation doivent être inscrit](xref:security/authorization/policies#handler-registration) dans la collection de service lors de la configuration (à l’aide de [l’injection de dépendances](xref:fundamentals/dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="09ace-104">[Authorization handlers must be registered](xref:security/authorization/policies#handler-registration) in the service collection during configuration (using [dependency injection](xref:fundamentals/dependency-injection)).</span></span>

<span data-ttu-id="09ace-105">Supposons que vous ayez un référentiel de règles que vous souhaitez évaluer à l’intérieur d’un gestionnaire d’autorisation, et ce référentiel a été inscrit dans la collection de service.</span><span class="sxs-lookup"><span data-stu-id="09ace-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="09ace-106">L’autorisation sera résoudre et qui injecter dans votre constructeur.</span><span class="sxs-lookup"><span data-stu-id="09ace-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="09ace-107">Par exemple, si vous souhaitez utiliser ASP. NET d’enregistrement d’infrastructure, vous pouvez injecter `ILoggerFactory` dans votre gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="09ace-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="09ace-108">Ce gestionnaire peut ressembler :</span><span class="sxs-lookup"><span data-stu-id="09ace-108">Such a handler might look like:</span></span>

```csharp
public class LoggingAuthorizationHandler : AuthorizationHandler<MyRequirement>
   {
       ILogger _logger;

       public LoggingAuthorizationHandler(ILoggerFactory loggerFactory)
       {
           _logger = loggerFactory.CreateLogger(this.GetType().FullName);
       }

       protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MyRequirement requirement)
       {
           _logger.LogInformation("Inside my handler");
           // Check if the requirement is fulfilled.
           return Task.CompletedTask;
       }
   }
   ```

<span data-ttu-id="09ace-109">Vous devez enregistrer le gestionnaire avec `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="09ace-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="09ace-110">Une instance du gestionnaire va être créé au démarrage de votre application et est de l’injection de dépendances injecter inscrit `ILoggerFactory` dans votre constructeur.</span><span class="sxs-lookup"><span data-stu-id="09ace-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="09ace-111">Les gestionnaires qui utilisent Entity Framework ne doit pas être enregistrés en tant que singletons.</span><span class="sxs-lookup"><span data-stu-id="09ace-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
