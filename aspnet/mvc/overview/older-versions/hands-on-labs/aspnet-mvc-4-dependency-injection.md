---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Injection de dépendance ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Remarque : ce laboratoire pratique suppose que vous avez une connaissance de base des filtres ASP.NET MVC et ASP.NET MVC 4. Si vous n’avez pas utilisé de filtres ASP.NET MVC 4 avant, nous avons reporté...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 15c9d4dcb9e2c6b9f6adf54d65d15737b32cca3b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78560612"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="aa11a-104">Injection de dépendances d’ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="aa11a-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="aa11a-105">par l' [équipe Web camps](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="aa11a-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="aa11a-106">Télécharger le kit de formation Web camps</span><span class="sxs-lookup"><span data-stu-id="aa11a-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="aa11a-107">Ce laboratoire pratique suppose que vous avez une connaissance de base des filtres **ASP.NET MVC** et **ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="aa11a-108">Si vous n’avez pas encore utilisé les **filtres ASP.NET MVC 4** , nous vous recommandons de passer au-dessus de l’atelier pratique des **filtres d’action personnalisée ASP.NET MVC** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="aa11a-109">Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible dans les [versions Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="aa11a-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="aa11a-110">Le projet propre à ce laboratoire est disponible sur l' [injection de dépendance ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="aa11a-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="aa11a-111">Dans le paradigme de **programmation orientée objet** , les objets fonctionnent ensemble dans un modèle de collaboration où il existe des contributeurs et des consommateurs.</span><span class="sxs-lookup"><span data-stu-id="aa11a-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="aa11a-112">Naturellement, ce modèle de communication génère des dépendances entre les objets et les composants, ce qui devient difficile à gérer lorsque la complexité augmente.</span><span class="sxs-lookup"><span data-stu-id="aa11a-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="aa11a-113">![Dépendances de classes et complexité du modèle](aspnet-mvc-4-dependency-injection/_static/image1.png "Dépendances de classes et complexité du modèle")</span><span class="sxs-lookup"><span data-stu-id="aa11a-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="aa11a-114">*Dépendances de classes et complexité du modèle*</span><span class="sxs-lookup"><span data-stu-id="aa11a-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="aa11a-115">Vous avez probablement entendu parler du **modèle de fabrique** et de la séparation entre l’interface et l’implémentation à l’aide de services, où les objets clients sont souvent responsables de l’emplacement du service.</span><span class="sxs-lookup"><span data-stu-id="aa11a-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="aa11a-116">Le modèle d’injection de dépendances est une implémentation particulière d’inversion de contrôle.</span><span class="sxs-lookup"><span data-stu-id="aa11a-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="aa11a-117">**Inversion de contrôle (IOC, inversion of Control)** signifie que les objets ne créent pas d’autres objets sur lesquels ils s’appuient pour effectuer leur travail.</span><span class="sxs-lookup"><span data-stu-id="aa11a-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="aa11a-118">Au lieu de cela, ils obtiennent les objets dont ils ont besoin à partir d’une source externe (par exemple, un fichier de configuration XML).</span><span class="sxs-lookup"><span data-stu-id="aa11a-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="aa11a-119">L' **injection de dépendance (di)** signifie que cette opération est effectuée sans l’intervention de l’objet, généralement par un composant de l’infrastructure qui passe les paramètres du constructeur et définit les propriétés.</span><span class="sxs-lookup"><span data-stu-id="aa11a-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="aa11a-120">Le modèle de conception d’injection de dépendances (DI)</span><span class="sxs-lookup"><span data-stu-id="aa11a-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="aa11a-121">À un niveau élevé, l’objectif de l’injection de dépendances est qu’une classe de client (par exemple, *le golfeur*) a besoin d’une interface qui satisfait une interface (par exemple, *iClub*).</span><span class="sxs-lookup"><span data-stu-id="aa11a-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="aa11a-122">Il ne se soucie pas du type concret (par exemple *, WoodClub, IronClub, WedgeClub* ou *PutterClub*), il veut que quelqu’un d’autre le gère (par exemple, un bon *chariot*).</span><span class="sxs-lookup"><span data-stu-id="aa11a-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="aa11a-123">Le résolveur de dépendance dans ASP.NET MVC peut vous permettre d’inscrire votre logique de dépendance ailleurs (par exemple, un conteneur ou un *sac de trèfle*).</span><span class="sxs-lookup"><span data-stu-id="aa11a-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="aa11a-124">![Diagramme d’injection de dépendances](aspnet-mvc-4-dependency-injection/_static/image2.png "Illustration de l’injection de dépendances")</span><span class="sxs-lookup"><span data-stu-id="aa11a-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="aa11a-125">*Injection de dépendances-analogie de golf*</span><span class="sxs-lookup"><span data-stu-id="aa11a-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="aa11a-126">Les avantages de l’utilisation du modèle d’injection de dépendances et de l’inversion de contrôle sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="aa11a-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="aa11a-127">Réduit le couplage de classe</span><span class="sxs-lookup"><span data-stu-id="aa11a-127">Reduces class coupling</span></span>
- <span data-ttu-id="aa11a-128">Augmente la réutilisation du code</span><span class="sxs-lookup"><span data-stu-id="aa11a-128">Increases code reusing</span></span>
- <span data-ttu-id="aa11a-129">Améliore la maintenabilité du code</span><span class="sxs-lookup"><span data-stu-id="aa11a-129">Improves code maintainability</span></span>
- <span data-ttu-id="aa11a-130">Améliore les tests d’application</span><span class="sxs-lookup"><span data-stu-id="aa11a-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="aa11a-131">L’injection de dépendances est parfois comparée au modèle de conception de fabrique abstraite, mais il existe une légère différence entre les deux approches.</span><span class="sxs-lookup"><span data-stu-id="aa11a-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="aa11a-132">L’injection de dépendance a une infrastructure qui fonctionne en arrière-plan pour résoudre les dépendances en appelant les fabriques et les services inscrits.</span><span class="sxs-lookup"><span data-stu-id="aa11a-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>

<span data-ttu-id="aa11a-133">Maintenant que vous comprenez le modèle d’injection de dépendances, vous allez apprendre dans ce laboratoire comment l’appliquer dans ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="aa11a-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="aa11a-134">Vous allez commencer à utiliser l’injection de dépendances dans les **contrôleurs** pour inclure un service d’accès à la base de données.</span><span class="sxs-lookup"><span data-stu-id="aa11a-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="aa11a-135">Ensuite, vous allez appliquer l’injection de dépendances aux **vues** pour utiliser un service et afficher des informations.</span><span class="sxs-lookup"><span data-stu-id="aa11a-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="aa11a-136">Enfin, vous allez étendre les filtres DI à ASP.NET MVC 4, en injectant un filtre d’action personnalisé dans la solution.</span><span class="sxs-lookup"><span data-stu-id="aa11a-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="aa11a-137">Dans ce laboratoire pratique, vous allez apprendre à :</span><span class="sxs-lookup"><span data-stu-id="aa11a-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="aa11a-138">Intégrer ASP.NET MVC 4 avec Unity pour l’injection de dépendances à l’aide de packages NuGet</span><span class="sxs-lookup"><span data-stu-id="aa11a-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="aa11a-139">Utiliser l’injection de dépendances dans un contrôleur MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="aa11a-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="aa11a-140">Utiliser l’injection de dépendances dans une vue MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="aa11a-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="aa11a-141">Utiliser l’injection de dépendances à l’intérieur d’un filtre d’action MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="aa11a-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="aa11a-142">Ce laboratoire utilise le package NuGet Unity. Mvc3 pour la résolution des dépendances, mais il est possible d’adapter n’importe quel Framework d’injection de dépendances pour qu’il fonctionne avec ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="aa11a-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="aa11a-143">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="aa11a-143">Prerequisites</span></span>

<span data-ttu-id="aa11a-144">Pour effectuer ce laboratoire, vous devez disposer des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="aa11a-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="aa11a-145">[Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieur (lisez [l’annexe A](#AppendixA) pour obtenir des instructions sur son installation).</span><span class="sxs-lookup"><span data-stu-id="aa11a-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="aa11a-146">Installation</span><span class="sxs-lookup"><span data-stu-id="aa11a-146">Setup</span></span>

<span data-ttu-id="aa11a-147">**Installation d’extraits de code**</span><span class="sxs-lookup"><span data-stu-id="aa11a-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="aa11a-148">Pour plus de commodité, la majeure partie du code que vous allez gérer dans le cadre de ce laboratoire est disponible sous forme d’extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aa11a-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="aa11a-149">Pour installer les extraits de code, exécutez le fichier **.\Source\Setup\CodeSnippets.vsi** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="aa11a-150">Si vous n’êtes pas familiarisé avec les extraits de Visual Studio Code et que vous souhaitez apprendre à les utiliser, vous pouvez vous référer à l’annexe de ce document &quot;l' [annexe B : utilisation d’extraits de Code](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="aa11a-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="aa11a-151">Exercices</span><span class="sxs-lookup"><span data-stu-id="aa11a-151">Exercises</span></span>

<span data-ttu-id="aa11a-152">Ce laboratoire pratique est constitué des exercices suivants :</span><span class="sxs-lookup"><span data-stu-id="aa11a-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="aa11a-153">Exercice 1 : injection d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="aa11a-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="aa11a-154">Exercice 2 : injection d’une vue</span><span class="sxs-lookup"><span data-stu-id="aa11a-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="aa11a-155">Exercice 3 : injection de filtres</span><span class="sxs-lookup"><span data-stu-id="aa11a-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="aa11a-156">Chaque exercice est accompagné d’un dossier de **fin** contenant la solution obtenue à obtenir après avoir effectué les exercices.</span><span class="sxs-lookup"><span data-stu-id="aa11a-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="aa11a-157">Vous pouvez utiliser cette solution comme guide si vous avez besoin d’aide supplémentaire pour suivre les exercices.</span><span class="sxs-lookup"><span data-stu-id="aa11a-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="aa11a-158">Durée estimée pour effectuer ce laboratoire : **30 minutes**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="aa11a-159">Exercice 1 : injection d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="aa11a-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="aa11a-160">Dans cet exercice, vous allez apprendre à utiliser l’injection de dépendances dans les contrôleurs MVC ASP.NET en intégrant Unity à l’aide d’un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="aa11a-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="aa11a-161">Pour cette raison, vous allez inclure des services dans vos contrôleurs MvcMusicStore pour séparer la logique de l’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="aa11a-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="aa11a-162">Les services créeront une nouvelle dépendance dans le constructeur du contrôleur, qui sera résolu à l’aide de l’injection de dépendances à l’aide d' **Unity**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="aa11a-163">Cette approche vous montrera comment générer des applications moins couplées, qui sont plus flexibles et plus faciles à gérer et à tester.</span><span class="sxs-lookup"><span data-stu-id="aa11a-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="aa11a-164">Vous allez également apprendre à intégrer ASP.NET MVC avec Unity.</span><span class="sxs-lookup"><span data-stu-id="aa11a-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="aa11a-165">À propos du service StoreManager</span><span class="sxs-lookup"><span data-stu-id="aa11a-165">About StoreManager Service</span></span>

<span data-ttu-id="aa11a-166">Le magasin de musique MVC fourni dans la solution Begin comprend maintenant un service qui gère les données du contrôleur de magasin nommé **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="aa11a-167">Vous trouverez ci-dessous l’implémentation du service Store.</span><span class="sxs-lookup"><span data-stu-id="aa11a-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="aa11a-168">Notez que toutes les méthodes retournent des entités de modèle.</span><span class="sxs-lookup"><span data-stu-id="aa11a-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="aa11a-169">**StoreController** à partir de la solution Begin consomme désormais **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="aa11a-170">Toutes les références de données ont été supprimées de **StoreController**, et il est maintenant possible de modifier le fournisseur d’accès aux données actuel sans modifier une méthode qui consomme **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="aa11a-171">Vous trouverez ci-dessous que l’implémentation de **StoreController** a une dépendance avec **StoreService** à l’intérieur du constructeur de classe.</span><span class="sxs-lookup"><span data-stu-id="aa11a-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="aa11a-172">La dépendance introduite dans cet exercice est liée à l' **inversion de contrôle** (IOC).</span><span class="sxs-lookup"><span data-stu-id="aa11a-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="aa11a-173">Le constructeur de classe **StoreController** reçoit un paramètre de type **IStoreService** , qui est essentiel pour effectuer des appels de service à l’intérieur de la classe.</span><span class="sxs-lookup"><span data-stu-id="aa11a-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="aa11a-174">Toutefois, **StoreController** n’implémente pas le constructeur par défaut (sans paramètres) que n’importe quel contrôleur doit avoir pour fonctionner avec ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="aa11a-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="aa11a-175">Pour résoudre la dépendance, le contrôleur doit être créé par une fabrique abstraite (une classe qui retourne un objet du type spécifié).</span><span class="sxs-lookup"><span data-stu-id="aa11a-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="aa11a-176">Vous obtiendrez une erreur lorsque la classe tentera de créer le StoreController sans envoyer l’objet de service, car aucun constructeur sans paramètre n’est déclaré.</span><span class="sxs-lookup"><span data-stu-id="aa11a-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="aa11a-177">Tâche 1 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="aa11a-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="aa11a-178">Dans cette tâche, vous allez exécuter l’application Begin, qui comprend le service dans le contrôleur de magasin qui sépare l’accès aux données de la logique d’application.</span><span class="sxs-lookup"><span data-stu-id="aa11a-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="aa11a-179">Lors de l’exécution de l’application, vous recevez une exception, car le service de contrôleur n’est pas transmis en tant que paramètre par défaut :</span><span class="sxs-lookup"><span data-stu-id="aa11a-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="aa11a-180">Ouvrez la solution **Begin** située dans **Source\Ex01-injecting Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="aa11a-181">Vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="aa11a-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="aa11a-182">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="aa11a-183">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="aa11a-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="aa11a-184">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="aa11a-185">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="aa11a-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="aa11a-186">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="aa11a-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="aa11a-187">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="aa11a-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="aa11a-188">Appuyez sur **CTRL + F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="aa11a-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="aa11a-189">Vous obtiendrez le message d’erreur &quot;**aucun constructeur sans paramètre défini pour cet objet**&quot;:</span><span class="sxs-lookup"><span data-stu-id="aa11a-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="aa11a-190">![Erreur lors de l’exécution de ASP.NET MVC Begin application](aspnet-mvc-4-dependency-injection/_static/image3.png "Erreur lors de l’exécution de ASP.NET MVC Begin application")</span><span class="sxs-lookup"><span data-stu-id="aa11a-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="aa11a-191">*Erreur lors de l’exécution de ASP.NET MVC Begin application*</span><span class="sxs-lookup"><span data-stu-id="aa11a-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="aa11a-192">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="aa11a-192">Close the browser.</span></span>

<span data-ttu-id="aa11a-193">Dans les étapes suivantes, vous allez travailler sur la solution Store Music pour injecter la dépendance dont ce contrôleur a besoin.</span><span class="sxs-lookup"><span data-stu-id="aa11a-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="aa11a-194">Tâche 2 : inclure Unity dans la solution MvcMusicStore</span><span class="sxs-lookup"><span data-stu-id="aa11a-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="aa11a-195">Dans cette tâche, vous allez inclure **Unity. Mvc3** NuGet package à la solution.</span><span class="sxs-lookup"><span data-stu-id="aa11a-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="aa11a-196">Le package Unity. Mvc3 a été conçu pour ASP.NET MVC 3, mais il est entièrement compatible avec ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="aa11a-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="aa11a-197">Unity est un conteneur d’injection de dépendances léger et extensible avec prise en charge facultative de l’interception d’instances et de types.</span><span class="sxs-lookup"><span data-stu-id="aa11a-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="aa11a-198">Il s’agit d’un conteneur à usage général à utiliser dans n’importe quel type d’application .NET.</span><span class="sxs-lookup"><span data-stu-id="aa11a-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="aa11a-199">Il fournit toutes les fonctionnalités communes disponibles dans les mécanismes d’injection de dépendances, notamment la création d’objets, l’abstraction des exigences en spécifiant des dépendances au moment de l’exécution et la flexibilité, en reportant la configuration du composant dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="aa11a-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>

1. <span data-ttu-id="aa11a-200">Installez le package NuGet **Unity. Mvc3** dans le projet **MvcMusicStore** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="aa11a-201">Pour ce faire, ouvrez la **console du gestionnaire de package** à partir de l' **affichage** | **autres fenêtres**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="aa11a-202">Exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="aa11a-202">Run the following command.</span></span>

    <span data-ttu-id="aa11a-203">PMC</span><span class="sxs-lookup"><span data-stu-id="aa11a-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="aa11a-204">![Installation du package NuGet Unity. Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Installation du package NuGet Unity. Mvc3")</span><span class="sxs-lookup"><span data-stu-id="aa11a-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="aa11a-205">*Installation du package NuGet Unity. Mvc3*</span><span class="sxs-lookup"><span data-stu-id="aa11a-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="aa11a-206">Une fois le package **Unity. Mvc3** installé, explorez les fichiers et les dossiers qu’il ajoute automatiquement afin de simplifier la configuration Unity.</span><span class="sxs-lookup"><span data-stu-id="aa11a-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="aa11a-207">![Package Unity. Mvc3 installé](aspnet-mvc-4-dependency-injection/_static/image5.png "Package Unity. Mvc3 installé")</span><span class="sxs-lookup"><span data-stu-id="aa11a-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="aa11a-208">*Package Unity. Mvc3 installé*</span><span class="sxs-lookup"><span data-stu-id="aa11a-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-application_start"></a><span data-ttu-id="aa11a-209">Tâche 3 : enregistrement d’Unity dans l’application Global.asax.cs\_démarrer</span><span class="sxs-lookup"><span data-stu-id="aa11a-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="aa11a-210">Dans cette tâche, vous allez mettre à jour l' **Application\_méthode Start** située dans **global.asax.cs** pour appeler l’initialiseur de programme d’amorçage Unity, puis mettre à jour le fichier du programme d’amorçage en inscrivant le service et le contrôleur que vous utiliserez pour l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="aa11a-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="aa11a-211">À présent, vous allez raccorder le programme d’amorçage qui est le fichier qui initialise le conteneur Unity et le résolveur de dépendance.</span><span class="sxs-lookup"><span data-stu-id="aa11a-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="aa11a-212">Pour ce faire, ouvrez **global.asax.cs** et ajoutez le code en surbrillance suivant au sein de l' **application\_méthode Start** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="aa11a-213">(Extrait de code- *laboratoire d’injection de dépendances ASP.net-Ex01-Initialize Unity*)</span><span class="sxs-lookup"><span data-stu-id="aa11a-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="aa11a-214">Ouvrez le fichier **Bootstrapper.cs** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="aa11a-215">Incluez les espaces de noms suivants : **MvcMusicStore. services** et **MusicStore. Controllers**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="aa11a-216">(Extrait de code- *ASP.net injection de dépendances Lab-Ex01-programme d’amorçage ajoutant des espaces de noms*)</span><span class="sxs-lookup"><span data-stu-id="aa11a-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="aa11a-217">Remplacez le contenu de la méthode **BuildUnityContainer** par le code suivant qui enregistre le contrôleur de magasin et le service de banque d’informations.</span><span class="sxs-lookup"><span data-stu-id="aa11a-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="aa11a-218">(Extrait de code- *ASP.net injection de dépendances Lab-Ex01-Register Store Controller and service*)</span><span class="sxs-lookup"><span data-stu-id="aa11a-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="aa11a-219">Tâche 4 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="aa11a-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="aa11a-220">Dans cette tâche, vous allez exécuter l’application pour vérifier qu’elle peut maintenant être chargée après l’intégration d’Unity.</span><span class="sxs-lookup"><span data-stu-id="aa11a-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="aa11a-221">Appuyez sur **F5** pour exécuter l’application, l’application doit maintenant être chargée sans qu’aucun message d’erreur ne s’affiche.</span><span class="sxs-lookup"><span data-stu-id="aa11a-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="aa11a-222">![Exécution de l’application avec injection de dépendances](aspnet-mvc-4-dependency-injection/_static/image6.png "Exécution de l’application avec injection de dépendances")</span><span class="sxs-lookup"><span data-stu-id="aa11a-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="aa11a-223">*Exécution de l’application avec injection de dépendances*</span><span class="sxs-lookup"><span data-stu-id="aa11a-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="aa11a-224">Accédez à **/Store**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-224">Browse to **/Store**.</span></span> <span data-ttu-id="aa11a-225">Cela appellera **StoreController**, qui est maintenant créé à l’aide d' **Unity**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="aa11a-226">![Magasin de musique MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "Magasin de musique MVC")</span><span class="sxs-lookup"><span data-stu-id="aa11a-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="aa11a-227">*Magasin de musique MVC*</span><span class="sxs-lookup"><span data-stu-id="aa11a-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="aa11a-228">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="aa11a-228">Close the browser.</span></span>

<span data-ttu-id="aa11a-229">Dans les exercices suivants, vous allez apprendre à étendre l’étendue de l’injection de dépendances pour l’utiliser dans les vues ASP.NET MVC et les filtres d’action.</span><span class="sxs-lookup"><span data-stu-id="aa11a-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="aa11a-230">Exercice 2 : injection d’une vue</span><span class="sxs-lookup"><span data-stu-id="aa11a-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="aa11a-231">Dans cet exercice, vous allez apprendre à utiliser l’injection de dépendances dans une vue avec les nouvelles fonctionnalités de ASP.NET MVC 4 pour l’intégration Unity.</span><span class="sxs-lookup"><span data-stu-id="aa11a-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="aa11a-232">Pour ce faire, vous allez appeler un service personnalisé à l’intérieur de la vue de navigation du magasin, qui affiche un message et une image ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="aa11a-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="aa11a-233">Ensuite, vous allez intégrer le projet à Unity et créer un programme de résolution de dépendance personnalisé pour injecter les dépendances.</span><span class="sxs-lookup"><span data-stu-id="aa11a-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="aa11a-234">Tâche 1 : création d’une vue qui consomme un service</span><span class="sxs-lookup"><span data-stu-id="aa11a-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="aa11a-235">Dans cette tâche, vous allez créer une vue qui effectue un appel de service pour générer une nouvelle dépendance.</span><span class="sxs-lookup"><span data-stu-id="aa11a-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="aa11a-236">Le service est constitué d’un simple service de messagerie inclus dans cette solution.</span><span class="sxs-lookup"><span data-stu-id="aa11a-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="aa11a-237">Ouvrez la solution **Begin** située dans le dossier **Source\Ex02-injecting View\Begin** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="aa11a-238">Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="aa11a-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="aa11a-239">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="aa11a-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="aa11a-240">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="aa11a-241">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="aa11a-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="aa11a-242">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="aa11a-243">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="aa11a-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="aa11a-244">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="aa11a-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="aa11a-245">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="aa11a-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="aa11a-246">Pour plus d’informations, consultez cet article : [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="aa11a-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="aa11a-247">Incluez les classes **MessageService.cs** et **IMessageService.cs** situées dans le dossier **\Assets source** dans **/services**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="aa11a-248">Pour ce faire, cliquez avec le bouton droit sur le dossier **services** et sélectionnez **Ajouter un élément existant**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="aa11a-249">Accédez à l’emplacement des fichiers et incluez-les.</span><span class="sxs-lookup"><span data-stu-id="aa11a-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="aa11a-250">![Ajout du service de message et de l’interface de service](aspnet-mvc-4-dependency-injection/_static/image8.png "Ajout du service de message et de l’interface de service")</span><span class="sxs-lookup"><span data-stu-id="aa11a-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="aa11a-251">*Ajout du service de message et de l’interface de service*</span><span class="sxs-lookup"><span data-stu-id="aa11a-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="aa11a-252">L’interface **IMessageService** définit deux propriétés implémentées par la classe **MessageService** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="aa11a-253">Ces propriétés-**message** et **ImageUrl**-stockent le message et l’URL de l’image à afficher.</span><span class="sxs-lookup"><span data-stu-id="aa11a-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="aa11a-254">Créez le dossier **/pages** dans le dossier racine du projet, puis ajoutez la classe **MyBasePage.cs** existante à partir de **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="aa11a-255">La page de base à partir de laquelle héritera a la structure suivante.</span><span class="sxs-lookup"><span data-stu-id="aa11a-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="aa11a-256">![Dossier pages](aspnet-mvc-4-dependency-injection/_static/image9.png "Dossier Pages")</span><span class="sxs-lookup"><span data-stu-id="aa11a-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="aa11a-257">Ouvrez la vue **Browse. cshtml** à partir du dossier **/views/Store** et faites en sorte qu’elle hérite de **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="aa11a-258">Dans l’affichage **Parcourir** , ajoutez un appel à **MessageService** pour afficher une image et un message récupéré par le service.</span><span class="sxs-lookup"><span data-stu-id="aa11a-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="aa11a-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="aa11a-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="aa11a-260">Tâche 2 : inclure un résolveur de dépendance personnalisé et un activateur de page de vue personnalisé</span><span class="sxs-lookup"><span data-stu-id="aa11a-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="aa11a-261">Dans la tâche précédente, vous avez injecté une nouvelle dépendance à l’intérieur d’une vue pour effectuer un appel de service à l’intérieur de celle-ci.</span><span class="sxs-lookup"><span data-stu-id="aa11a-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="aa11a-262">À présent, vous allez résoudre cette dépendance en implémentant les interfaces d’injection de dépendances MVC ASP.NET **IViewPageActivator** et **IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="aa11a-263">Vous allez inclure dans la solution une implémentation de **IDependencyResolver** qui traitera la récupération du service à l’aide d’Unity.</span><span class="sxs-lookup"><span data-stu-id="aa11a-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="aa11a-264">Ensuite, vous allez inclure une autre implémentation personnalisée de l’interface **IViewPageActivator** qui résoudra la création des vues.</span><span class="sxs-lookup"><span data-stu-id="aa11a-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="aa11a-265">Depuis ASP.NET MVC 3, l’implémentation pour l’injection de dépendances avait simplifié les interfaces pour inscrire les services.</span><span class="sxs-lookup"><span data-stu-id="aa11a-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="aa11a-266">**IDependencyResolver** et **IViewPageActivator** font partie des fonctionnalités ASP.NET MVC 3 pour l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="aa11a-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="aa11a-267">**-** L’interface IDependencyResolver remplace le IMvcServiceLocator précédent.</span><span class="sxs-lookup"><span data-stu-id="aa11a-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="aa11a-268">Les implémenteurs de IDependencyResolver doivent retourner une instance du service ou une collection de services.</span><span class="sxs-lookup"><span data-stu-id="aa11a-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="aa11a-269">**-** L’interface IViewPageActivator fournit un contrôle plus précis sur la façon dont les pages de vue sont instanciées via l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="aa11a-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="aa11a-270">Les classes qui implémentent l’interface **IViewPageActivator** peuvent créer des instances de vue à l’aide des informations de contexte.</span><span class="sxs-lookup"><span data-stu-id="aa11a-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]

1. <span data-ttu-id="aa11a-271">Créez le dossier/**usines** dans le dossier racine du projet.</span><span class="sxs-lookup"><span data-stu-id="aa11a-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="aa11a-272">Incluez **CustomViewPageActivator.cs** à votre solution à partir de **/sources/Assets/** vers le dossier **usines** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="aa11a-273">Pour ce faire, cliquez avec le bouton droit sur le dossier **/Factories** , puis sélectionnez **Ajouter | Élément existant** , puis sélectionnez **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="aa11a-274">Cette classe implémente l’interface **IViewPageActivator** pour contenir le conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="aa11a-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="aa11a-275">**CustomViewPageActivator** est responsable de la gestion de la création d’une vue à l’aide d’un conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="aa11a-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="aa11a-276">Incluez le fichier **UnityDependencyResolver.cs** de **/sources/Assets** dans le dossier **/Factories** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="aa11a-277">Pour ce faire, cliquez avec le bouton droit sur le dossier **/Factories** , puis sélectionnez **Ajouter | Élément existant** , puis sélectionnez le fichier **UnityDependencyResolver.cs** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="aa11a-278">La classe **UnityDependencyResolver** est un DependencyResolver personnalisé pour Unity.</span><span class="sxs-lookup"><span data-stu-id="aa11a-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="aa11a-279">Lorsqu’un service est introuvable dans le conteneur Unity, le programme de résolution de base est invocated.</span><span class="sxs-lookup"><span data-stu-id="aa11a-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="aa11a-280">Dans la tâche suivante, les deux implémentations sont inscrites pour permettre au modèle de connaître l’emplacement des services et les vues.</span><span class="sxs-lookup"><span data-stu-id="aa11a-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="aa11a-281">Tâche 3 : inscription pour l’injection de dépendances dans un conteneur Unity</span><span class="sxs-lookup"><span data-stu-id="aa11a-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="aa11a-282">Dans cette tâche, vous allez regrouper toutes les opérations précédentes pour rendre le travail d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="aa11a-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="aa11a-283">Jusqu’à présent, votre solution comporte les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="aa11a-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="aa11a-284">Vue de **navigation** qui hérite de **MyBaseClass** et consomme **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="aa11a-285">Classe intermédiaire-**MyBaseClass**-qui a une injection de dépendances déclarée pour l’interface de service.</span><span class="sxs-lookup"><span data-stu-id="aa11a-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="aa11a-286">Service- **MessageService** -et son interface **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="aa11a-287">Un programme de résolution des dépendances personnalisé pour Unity- **UnityDependencyResolver** , qui concerne la récupération du service.</span><span class="sxs-lookup"><span data-stu-id="aa11a-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="aa11a-288">Une page de vue Activator- **CustomViewPageActivator** -qui crée la page.</span><span class="sxs-lookup"><span data-stu-id="aa11a-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="aa11a-289">Pour injecter la vue **Parcourir** , vous allez maintenant inscrire le résolveur de dépendance personnalisé dans le conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="aa11a-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="aa11a-290">Ouvrez le fichier **Bootstrapper.cs** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="aa11a-291">Inscrivez une instance de **MessageService** dans le conteneur Unity pour initialiser le service :</span><span class="sxs-lookup"><span data-stu-id="aa11a-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="aa11a-292">(Extrait de code- *ASP.net d’injection de dépendances Lab-Ex02-Register message service*)</span><span class="sxs-lookup"><span data-stu-id="aa11a-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="aa11a-293">Ajoutez une référence à l’espace de noms **MvcMusicStore. Factory** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="aa11a-294">(Extrait de code- *ASP.net injection de dépendances Lab-Ex02-Factory*)</span><span class="sxs-lookup"><span data-stu-id="aa11a-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="aa11a-295">Inscrivez **CustomViewPageActivator** comme activateur de page de vue dans le conteneur Unity :</span><span class="sxs-lookup"><span data-stu-id="aa11a-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="aa11a-296">(Extrait de code- *laboratoire d’injection de dépendances ASP.net-Ex02-Register CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="aa11a-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="aa11a-297">Remplacez le résolveur de dépendance par défaut ASP.NET MVC 4 par une instance de **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="aa11a-298">Pour ce faire, remplacez **initialiser** le contenu de la méthode par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="aa11a-298">To do this, replace **Initialize** method content with the following code:</span></span>

    <span data-ttu-id="aa11a-299">(Extrait de code- *laboratoire d’injection de dépendances ASP.net-Ex02-mettre à jour le*programme de résolution de dépendance)</span><span class="sxs-lookup"><span data-stu-id="aa11a-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="aa11a-300">ASP.NET MVC fournit une classe de programme de résolution de dépendance par défaut.</span><span class="sxs-lookup"><span data-stu-id="aa11a-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="aa11a-301">Pour utiliser les programmes de résolution de dépendance personnalisés comme celui que nous avons créé pour Unity, ce programme de résolution doit être remplacé.</span><span class="sxs-lookup"><span data-stu-id="aa11a-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="aa11a-302">Tâche 4 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="aa11a-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="aa11a-303">Au cours de cette tâche, vous allez exécuter l’application pour vérifier que l’Explorateur Windows Store consomme le service et affiche l’image et le message récupérés :</span><span class="sxs-lookup"><span data-stu-id="aa11a-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="aa11a-304">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="aa11a-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="aa11a-305">Cliquez sur **Rock** dans le menu genres pour voir comment le **MessageService** a été injecté dans la vue et chargé le message d’accueil et l’image.</span><span class="sxs-lookup"><span data-stu-id="aa11a-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="aa11a-306">Dans cet exemple, nous allons entrer dans &quot;&quot;**Rock** :</span><span class="sxs-lookup"><span data-stu-id="aa11a-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="aa11a-307">![Magasin de musique MVC-afficher l’injection](aspnet-mvc-4-dependency-injection/_static/image10.png "Magasin de musique MVC-afficher l’injection")</span><span class="sxs-lookup"><span data-stu-id="aa11a-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="aa11a-308">*Magasin de musique MVC-afficher l’injection*</span><span class="sxs-lookup"><span data-stu-id="aa11a-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="aa11a-309">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="aa11a-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="aa11a-310">Exercice 3 : injection de filtres d’action</span><span class="sxs-lookup"><span data-stu-id="aa11a-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="aa11a-311">Dans les **filtres d’action personnalisée** de laboratoire pratiques précédents, vous avez travaillé avec des filtres de personnalisation et d’injection.</span><span class="sxs-lookup"><span data-stu-id="aa11a-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="aa11a-312">Dans cet exercice, vous allez apprendre à injecter des filtres avec l’injection de dépendances à l’aide du conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="aa11a-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="aa11a-313">Pour ce faire, vous allez ajouter à la solution Store Music un filtre d’action personnalisé qui effectuera le suivi de l’activité du site.</span><span class="sxs-lookup"><span data-stu-id="aa11a-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="aa11a-314">Tâche 1 : inclure le filtre de suivi dans la solution</span><span class="sxs-lookup"><span data-stu-id="aa11a-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="aa11a-315">Dans cette tâche, vous allez inclure dans le magasin musical un filtre d’action personnalisé pour suivre les événements.</span><span class="sxs-lookup"><span data-stu-id="aa11a-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="aa11a-316">Comme les concepts de filtre d’action personnalisée sont déjà traités dans le laboratoire précédent &quot;les filtres d’action personnalisés&quot;, vous incluez simplement la classe de filtre dans le dossier composants de ce Lab, puis vous créez un fournisseur de filtres pour Unity :</span><span class="sxs-lookup"><span data-stu-id="aa11a-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="aa11a-317">Ouvrez la solution **Begin** située dans le dossier **Source\Ex03-injecting action Filter\Begin** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="aa11a-318">Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="aa11a-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="aa11a-319">Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="aa11a-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="aa11a-320">Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="aa11a-321">Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="aa11a-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="aa11a-322">Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="aa11a-323">L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="aa11a-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="aa11a-324">Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet.</span><span class="sxs-lookup"><span data-stu-id="aa11a-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="aa11a-325">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="aa11a-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="aa11a-326">Pour plus d’informations, consultez cet article : [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="aa11a-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="aa11a-327">Incluez le fichier **TraceActionFilter.cs** de **/sources/Assets** dans le dossier **/Filters** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="aa11a-328">Ce filtre d’action personnalisé effectue le suivi ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="aa11a-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="aa11a-329">Pour plus d’informations, consultez &quot;filtres d’action locaux et dynamiques ASP.NET MVC 4&quot; Lab.</span><span class="sxs-lookup"><span data-stu-id="aa11a-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="aa11a-330">Ajoutez la classe vide **FilterProvider.cs** au projet dans le dossier **/Filters.**</span><span class="sxs-lookup"><span data-stu-id="aa11a-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="aa11a-331">Ajoutez les espaces de noms **System. Web. Mvc** et **Microsoft. practices. unity** dans **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="aa11a-332">(Extrait de code- *ASP.net d’injection de dépendances Lab-Ex03-fournisseur de filtre ajoutant des espaces de noms*)</span><span class="sxs-lookup"><span data-stu-id="aa11a-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="aa11a-333">Faites en sorte que la classe hérite de l’interface **IFilterProvider** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="aa11a-334">Ajoutez une propriété **IUnityContainer** dans la classe **FilterProvider** , puis créez un constructeur de classe pour assigner le conteneur.</span><span class="sxs-lookup"><span data-stu-id="aa11a-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="aa11a-335">(Extrait de code- *test d’injection de dépendance ASP.net-Lab-Ex03-constructeur du fournisseur de filtre*)</span><span class="sxs-lookup"><span data-stu-id="aa11a-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="aa11a-336">Le constructeur de classe du fournisseur de filtres ne crée pas un **nouvel** objet à l’intérieur de.</span><span class="sxs-lookup"><span data-stu-id="aa11a-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="aa11a-337">Le conteneur est passé en tant que paramètre et la dépendance est résolue par Unity.</span><span class="sxs-lookup"><span data-stu-id="aa11a-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="aa11a-338">Dans la classe **FilterProvider** , implémentez la méthode **GetFilters** à partir de l’interface **IFilterProvider** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="aa11a-339">(Extrait de code- *laboratoire d’injection de dépendances ASP.net-Ex03-Provider GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="aa11a-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="aa11a-340">Tâche 2 : inscription et activation du filtre</span><span class="sxs-lookup"><span data-stu-id="aa11a-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="aa11a-341">Dans cette tâche, vous allez activer le suivi de site.</span><span class="sxs-lookup"><span data-stu-id="aa11a-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="aa11a-342">Pour ce faire, vous allez inscrire le filtre dans la méthode **Bootstrapper.cs BuildUnityContainer** pour démarrer le suivi :</span><span class="sxs-lookup"><span data-stu-id="aa11a-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="aa11a-343">Ouvrez le **fichier Web. config** situé dans la racine du projet et activez le suivi des traces sur le groupe System. Web.</span><span class="sxs-lookup"><span data-stu-id="aa11a-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="aa11a-344">Ouvrez **Bootstrapper.cs** à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="aa11a-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="aa11a-345">Ajoutez une référence à l’espace de noms **MvcMusicStore. filters** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="aa11a-346">(Extrait de code- *ASP.net injection de dépendances Lab-Ex03-programme d’amorçage ajoutant des espaces de noms*)</span><span class="sxs-lookup"><span data-stu-id="aa11a-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="aa11a-347">Sélectionnez la méthode **BuildUnityContainer** et inscrivez le filtre dans le conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="aa11a-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="aa11a-348">Vous devez inscrire le fournisseur de filtres et le filtre d’action.</span><span class="sxs-lookup"><span data-stu-id="aa11a-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="aa11a-349">(Extrait de code- *laboratoire d’injection de dépendances ASP.net-Ex03-Register FilterProvider et ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="aa11a-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="aa11a-350">Tâche 3 : exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="aa11a-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="aa11a-351">Dans cette tâche, vous allez exécuter l’application et vérifier que le filtre d’action personnalisé effectue le suivi de l’activité :</span><span class="sxs-lookup"><span data-stu-id="aa11a-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="aa11a-352">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="aa11a-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="aa11a-353">Cliquez sur **Rock** dans le menu genres.</span><span class="sxs-lookup"><span data-stu-id="aa11a-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="aa11a-354">Vous pouvez accéder à plus de genres si vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="aa11a-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="aa11a-355">![Magasin de musique](aspnet-mvc-4-dependency-injection/_static/image11.png "Magasin de musique")</span><span class="sxs-lookup"><span data-stu-id="aa11a-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="aa11a-356">*Magasin de musique*</span><span class="sxs-lookup"><span data-stu-id="aa11a-356">*Music Store*</span></span>
3. <span data-ttu-id="aa11a-357">Accédez à **/trace.axd** pour afficher la page de trace de l’application, puis cliquez sur **afficher les détails**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="aa11a-358">![Journal de suivi d’application](aspnet-mvc-4-dependency-injection/_static/image12.png "Journal de suivi d’application")</span><span class="sxs-lookup"><span data-stu-id="aa11a-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="aa11a-359">*Journal de suivi d’application*</span><span class="sxs-lookup"><span data-stu-id="aa11a-359">*Application Trace Log*</span></span>

    <span data-ttu-id="aa11a-360">![Suivi de l’application-détails de la demande](aspnet-mvc-4-dependency-injection/_static/image13.png "Suivi de l’application-détails de la demande")</span><span class="sxs-lookup"><span data-stu-id="aa11a-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="aa11a-361">*Suivi de l’application-détails de la demande*</span><span class="sxs-lookup"><span data-stu-id="aa11a-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="aa11a-362">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="aa11a-362">Close the browser.</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="aa11a-363">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="aa11a-363">Summary</span></span>

<span data-ttu-id="aa11a-364">En effectuant ce laboratoire pratique, vous avez appris à utiliser l’injection de dépendances dans ASP.NET MVC 4 en intégrant Unity à l’aide d’un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="aa11a-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="aa11a-365">Pour ce faire, vous avez utilisé l’injection de dépendances dans les contrôleurs, les vues et les filtres d’action.</span><span class="sxs-lookup"><span data-stu-id="aa11a-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="aa11a-366">Les concepts suivants ont été abordés :</span><span class="sxs-lookup"><span data-stu-id="aa11a-366">The following concepts were covered:</span></span>

- <span data-ttu-id="aa11a-367">Fonctionnalités d’injection de dépendances ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="aa11a-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="aa11a-368">Intégration Unity utilisant le package NuGet Unity. Mvc3</span><span class="sxs-lookup"><span data-stu-id="aa11a-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="aa11a-369">Injection de dépendances dans les contrôleurs</span><span class="sxs-lookup"><span data-stu-id="aa11a-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="aa11a-370">Injection de dépendances dans les vues</span><span class="sxs-lookup"><span data-stu-id="aa11a-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="aa11a-371">Injection de dépendances de filtres d’action</span><span class="sxs-lookup"><span data-stu-id="aa11a-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="aa11a-372">Annexe A : installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="aa11a-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="aa11a-373">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="aa11a-374">Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="aa11a-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="aa11a-375">Accédez à [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="aa11a-375">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="aa11a-376">Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;<em>Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="aa11a-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="aa11a-377">Cliquez sur **Installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-377">Click on **Install Now**.</span></span> <span data-ttu-id="aa11a-378">Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.</span><span class="sxs-lookup"><span data-stu-id="aa11a-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="aa11a-379">Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.</span><span class="sxs-lookup"><span data-stu-id="aa11a-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="aa11a-380">![Installer Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="aa11a-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="aa11a-381">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="aa11a-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="aa11a-382">Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="aa11a-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acceptation des termes du contrat de licence](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="aa11a-384">*Acceptation des termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="aa11a-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="aa11a-385">Attendez que le processus de téléchargement et d’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="aa11a-385">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="aa11a-387">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="aa11a-387">*Installation progress*</span></span>
6. <span data-ttu-id="aa11a-388">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-388">When the installation completes, click **Finish**.</span></span>

    ![Installation terminée](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="aa11a-390">*Installation terminée*</span><span class="sxs-lookup"><span data-stu-id="aa11a-390">*Installation completed*</span></span>
7. <span data-ttu-id="aa11a-391">Cliquez sur **quitter** pour fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="aa11a-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="aa11a-392">Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .</span><span class="sxs-lookup"><span data-stu-id="aa11a-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Vignette VS Express pour le Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="aa11a-394">*Vignette VS Express pour le Web*</span><span class="sxs-lookup"><span data-stu-id="aa11a-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="aa11a-395">Annexe B : utilisation d’extraits de code</span><span class="sxs-lookup"><span data-stu-id="aa11a-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="aa11a-396">Avec les extraits de code, vous avez tout le code dont vous avez besoin à portée de main.</span><span class="sxs-lookup"><span data-stu-id="aa11a-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="aa11a-397">Le document Lab vous indique exactement quand vous pouvez les utiliser, comme illustré dans la figure suivante.</span><span class="sxs-lookup"><span data-stu-id="aa11a-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="aa11a-398">![Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-dependency-injection/_static/image19.png "Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="aa11a-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="aa11a-399">*Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="aa11a-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="aa11a-400">***Pour ajouter un extrait de code à l’aideC# du clavier (uniquement)***</span><span class="sxs-lookup"><span data-stu-id="aa11a-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="aa11a-401">Placez le curseur à l’endroit où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="aa11a-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="aa11a-402">Commencez à taper le nom de l’extrait de code (sans espaces ni traits d’Union).</span><span class="sxs-lookup"><span data-stu-id="aa11a-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="aa11a-403">Regarder comme IntelliSense affiche les noms des extraits correspondants.</span><span class="sxs-lookup"><span data-stu-id="aa11a-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="aa11a-404">Sélectionnez l’extrait de code approprié (ou continuez à taper jusqu’à ce que le nom de l’extrait entier soit sélectionné).</span><span class="sxs-lookup"><span data-stu-id="aa11a-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="aa11a-405">Appuyez deux fois sur la touche Tab pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="aa11a-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="aa11a-406">![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-dependency-injection/_static/image20.png "Commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="aa11a-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="aa11a-407">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="aa11a-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="aa11a-408">![Appuyez sur Tab pour sélectionner l’extrait en surbrillance](aspnet-mvc-4-dependency-injection/_static/image21.png "Appuyez sur Tab pour sélectionner l’extrait en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="aa11a-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="aa11a-409">*Appuyez sur Tab pour sélectionner l’extrait en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="aa11a-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="aa11a-410">![Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé](aspnet-mvc-4-dependency-injection/_static/image22.png "Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé")</span><span class="sxs-lookup"><span data-stu-id="aa11a-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="aa11a-411">*Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé*</span><span class="sxs-lookup"><span data-stu-id="aa11a-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="aa11a-412">***Pour ajouter un extrait de code à l’aideC#de la souris (, Visual Basic et XML)*** 1,0.</span><span class="sxs-lookup"><span data-stu-id="aa11a-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="aa11a-413">Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="aa11a-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="aa11a-414">Sélectionnez **Insérer un extrait** suivi de **mes extraits de code**.</span><span class="sxs-lookup"><span data-stu-id="aa11a-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="aa11a-415">Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="aa11a-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="aa11a-416">![Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait](aspnet-mvc-4-dependency-injection/_static/image23.png "Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="aa11a-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="aa11a-417">*Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="aa11a-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="aa11a-418">![Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.](aspnet-mvc-4-dependency-injection/_static/image24.png "Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.")</span><span class="sxs-lookup"><span data-stu-id="aa11a-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="aa11a-419">*Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.*</span><span class="sxs-lookup"><span data-stu-id="aa11a-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
