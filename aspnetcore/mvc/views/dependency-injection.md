---
title: Injection de dépendances dans les vues dans ASP.NET Core
author: ardalis
description: Découvrez comment ASP.NET Core prend en charge l’injection de dépendances dans les vues MVC.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/dependency-injection
ms.openlocfilehash: dfadafe9ebb5799b45ef68653f20c5fc1a2506b5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57025456"
---
# <a name="dependency-injection-into-views-in-aspnet-core"></a><span data-ttu-id="36d84-103">Injection de dépendances dans les vues dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="36d84-103">Dependency injection into views in ASP.NET Core</span></span>

<span data-ttu-id="36d84-104">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="36d84-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="36d84-105">ASP.NET Core prend en charge l’[injection de dépendances](xref:fundamentals/dependency-injection) dans les vues.</span><span class="sxs-lookup"><span data-stu-id="36d84-105">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="36d84-106">Cette fonctionnalité peut être utile pour les services spécifiques à une vue, notamment la localisation ou les données requises uniquement pour remplir les éléments de la vue.</span><span class="sxs-lookup"><span data-stu-id="36d84-106">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="36d84-107">Vous devez essayer de respecter le principe de [séparation des préoccupations](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) entre les contrôleurs et les vues.</span><span class="sxs-lookup"><span data-stu-id="36d84-107">You should try to maintain [separation of concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) between your controllers and views.</span></span> <span data-ttu-id="36d84-108">La plupart des données affichées dans vos vues doivent être passées par le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="36d84-108">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="36d84-109">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="36d84-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="a-simple-example"></a><span data-ttu-id="36d84-110">Exemple simple</span><span class="sxs-lookup"><span data-stu-id="36d84-110">A Simple Example</span></span>

<span data-ttu-id="36d84-111">Vous pouvez injecter un service dans une vue à l’aide de la directive `@inject`.</span><span class="sxs-lookup"><span data-stu-id="36d84-111">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="36d84-112">L’utilisation de la directive `@inject` revient à ajouter une propriété à la vue, puis à remplir cette propriété à l’aide de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="36d84-112">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="36d84-113">Syntaxe de la directive `@inject` : `@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="36d84-113">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="36d84-114">Exemple d’exécution de la directive `@inject` :</span><span class="sxs-lookup"><span data-stu-id="36d84-114">An example of `@inject` in action:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="36d84-115">Cette vue affiche une liste d’instances `ToDoItem` et un récapitulatif de statistiques générales.</span><span class="sxs-lookup"><span data-stu-id="36d84-115">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="36d84-116">Le récapitulatif est rempli avec les données du service `StatisticsService` injecté.</span><span class="sxs-lookup"><span data-stu-id="36d84-116">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="36d84-117">Ce service est inscrit pour l’injection de dépendances sous `ConfigureServices` dans *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="36d84-117">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="36d84-118">`StatisticsService` effectue des calculs sur l’ensemble des instances `ToDoItem`, auquel il accède par le biais d’un référentiel :</span><span class="sxs-lookup"><span data-stu-id="36d84-118">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,25)]

<span data-ttu-id="36d84-119">Le référentiel de l’exemple utilise une collection en mémoire.</span><span class="sxs-lookup"><span data-stu-id="36d84-119">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="36d84-120">L’implémentation illustrée ci-dessus (qui s’applique à toutes les données en mémoire) n’est pas recommandée pour les jeux de données volumineux et accessibles à distance.</span><span class="sxs-lookup"><span data-stu-id="36d84-120">The implementation shown above (which operates on all of the data in memory) isn't recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="36d84-121">L’exemple affiche les données fournies par le modèle lié à la vue et le service injecté dans la vue :</span><span class="sxs-lookup"><span data-stu-id="36d84-121">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![La vue des tâches To Do affiche le nombre total de tâches et de tâches terminées, la priorité moyenne, et une liste des tâches avec leur niveau de priorité et une valeur booléenne indiquant leur statut.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="36d84-123">Remplissage des données de recherche</span><span class="sxs-lookup"><span data-stu-id="36d84-123">Populating Lookup Data</span></span>

<span data-ttu-id="36d84-124">L’injection dans les vues peut être utile pour remplir certaines options dans les éléments d’interface utilisateur, telles que les listes déroulantes.</span><span class="sxs-lookup"><span data-stu-id="36d84-124">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="36d84-125">Prenons l’exemple d’un formulaire de profil utilisateur qui comporte des options permettant de spécifier le sexe, l’État et d’autres préférences.</span><span class="sxs-lookup"><span data-stu-id="36d84-125">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="36d84-126">Pour afficher un formulaire de ce type selon une approche MVC standard, il faudrait que le contrôleur demande l’accès des données aux services pour chacune des options du formulaire, puis qu’il remplisse un modèle ou `ViewBag` avec chaque ensemble d’options à lier.</span><span class="sxs-lookup"><span data-stu-id="36d84-126">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="36d84-127">Une autre approche consiste à injecter les services directement dans la vue pour obtenir les options.</span><span class="sxs-lookup"><span data-stu-id="36d84-127">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="36d84-128">Cela réduit la quantité de code requis par le contrôleur, car la logique de construction de cet élément de vue est déplacée dans la vue proprement dite.</span><span class="sxs-lookup"><span data-stu-id="36d84-128">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="36d84-129">Pour afficher un formulaire de modification de profil, il suffit ainsi au contrôleur de passer l’instance de profil au formulaire :</span><span class="sxs-lookup"><span data-stu-id="36d84-129">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="36d84-130">Le formulaire HTML utilisé pour mettre à jour ces préférences inclut des listes déroulantes pour trois des propriétés :</span><span class="sxs-lookup"><span data-stu-id="36d84-130">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![Vue de mise à jour du profil, avec un formulaire de saisie d’informations (nom, sexe, État et couleur préférée).](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="36d84-132">Ces listes sont remplies par un service qui a été injecté dans la vue :</span><span class="sxs-lookup"><span data-stu-id="36d84-132">These lists are populated by a service that has been injected into the view:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="36d84-133">`ProfileOptionsService` est un service au niveau de l’interface utilisateur qui est conçu pour fournir uniquement les données nécessaires dans ce formulaire :</span><span class="sxs-lookup"><span data-stu-id="36d84-133">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

> [!IMPORTANT]
> <span data-ttu-id="36d84-134">N’oubliez pas d’inscrire les types à demander par le biais de l’injection de dépendances dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="36d84-134">Don't forget to register types you request through dependency injection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="36d84-135">Un type non inscrit lève une exception au moment de l’exécution, car le fournisseur de services est interrogé en interne par le biais de [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).</span><span class="sxs-lookup"><span data-stu-id="36d84-135">An unregistered type throws an exception at runtime because the service provider is internally queried via [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).</span></span>

## <a name="overriding-services"></a><span data-ttu-id="36d84-136">Substitution de services</span><span class="sxs-lookup"><span data-stu-id="36d84-136">Overriding Services</span></span>

<span data-ttu-id="36d84-137">L’injection de services peut être utilisée pour injecter de nouveaux services, mais également pour substituer des services ayant déjà été injectés dans une page.</span><span class="sxs-lookup"><span data-stu-id="36d84-137">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="36d84-138">La capture d’écran ci-dessous montre tous les champs disponibles dans la page utilisée dans le premier exemple :</span><span class="sxs-lookup"><span data-stu-id="36d84-138">The figure below shows all of the fields available on the page used in the first example:</span></span>

![Menu contextuel IntelliSense pour un symbole @ entré qui affiche les champs Html, Component, StatsService et Url](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="36d84-140">Comme vous pouvez le voir, les champs par défaut incluent `Html`, `Component` et `Url` (mais aussi `StatsService` que nous avons injecté).</span><span class="sxs-lookup"><span data-stu-id="36d84-140">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="36d84-141">Si vous souhaitez, par exemple, remplacer les HTML Helpers par défaut par les vôtres, vous pouvez facilement le faire en utilisant `@inject` :</span><span class="sxs-lookup"><span data-stu-id="36d84-141">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="36d84-142">Si vous souhaitez étendre des services existants, vous pouvez simplement utiliser cette technique en héritant de ou en incluant dans un wrapper l’implémentation existante avec la vôtre.</span><span class="sxs-lookup"><span data-stu-id="36d84-142">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="36d84-143">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="36d84-143">See Also</span></span>

* <span data-ttu-id="36d84-144">Blog de Simon Timms : [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="36d84-144">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
