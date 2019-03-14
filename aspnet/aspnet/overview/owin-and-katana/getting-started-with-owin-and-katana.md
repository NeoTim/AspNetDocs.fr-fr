---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Bien démarrer avec OWIN et Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 9920861da0e67d9304a944cacfb8ff8685267cd6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57052006"
---
<a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="1b31b-102">Bien démarrer avec OWIN et Katana</span><span class="sxs-lookup"><span data-stu-id="1b31b-102">Getting Started with OWIN and Katana</span></span>
====================
<span data-ttu-id="1b31b-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1b31b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1b31b-104">[Open Web Interface pour .NET (OWIN)](http://owin.org/) définit une abstraction entre les serveurs web de .NET et des applications web.</span><span class="sxs-lookup"><span data-stu-id="1b31b-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="1b31b-105">En séparant le serveur web à partir de l’application, OWIN rend plus facile de créer des intergiciels (middleware) pour le développement web .NET.</span><span class="sxs-lookup"><span data-stu-id="1b31b-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="1b31b-106">En outre, OWIN facilite pour porter les applications web vers d’autres hôtes&#8212;auto-hébergement, par exemple, dans un service Windows ou tout autre processus.</span><span class="sxs-lookup"><span data-stu-id="1b31b-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="1b31b-107">OWIN est une spécification appartenant à la Communauté, pas une implémentation.</span><span class="sxs-lookup"><span data-stu-id="1b31b-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="1b31b-108">Le projet Katana est un ensemble de composants OWIN open source développée par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1b31b-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="1b31b-109">Pour obtenir une vue d’ensemble de OWIN et Katana, consultez [une vue d’ensemble du projet Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="1b31b-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="1b31b-110">Dans cet article, j’ai sera lancez-vous dans le code pour commencer.</span><span class="sxs-lookup"><span data-stu-id="1b31b-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="1b31b-111">Ce didacticiel utilise [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), mais vous pouvez également utiliser Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="1b31b-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="1b31b-112">Quelques étapes sont différentes dans Visual Studio 2012, je remarque ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="1b31b-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="1b31b-113">Héberger OWIN dans IIS</span><span class="sxs-lookup"><span data-stu-id="1b31b-113">Host OWIN in IIS</span></span>

<span data-ttu-id="1b31b-114">Dans cette section, nous allons héberger OWIN dans IIS.</span><span class="sxs-lookup"><span data-stu-id="1b31b-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="1b31b-115">Cette option vous donne la flexibilité et la composabilité d’un pipeline OWIN avec l’ensemble des fonctionnalités matures de IIS.</span><span class="sxs-lookup"><span data-stu-id="1b31b-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="1b31b-116">Utilisez cette option, l’application OWIN s’exécute dans le pipeline de demande ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1b31b-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="1b31b-117">Tout d’abord, créez un nouveau projet d’Application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1b31b-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="1b31b-118">(Dans Visual Studio 2012, utilisez le type de projet d’Application Web ASP.NET vide).</span><span class="sxs-lookup"><span data-stu-id="1b31b-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="1b31b-119">Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **vide** modèle.</span><span class="sxs-lookup"><span data-stu-id="1b31b-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="1b31b-120">Ajout de Packages NuGet</span><span class="sxs-lookup"><span data-stu-id="1b31b-120">Add NuGet Packages</span></span>

<span data-ttu-id="1b31b-121">Ensuite, ajoutez les packages NuGet nécessaires.</span><span class="sxs-lookup"><span data-stu-id="1b31b-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="1b31b-122">À partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="1b31b-122">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="1b31b-123">Dans la fenêtre de Console du Gestionnaire de Package, tapez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="1b31b-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="1b31b-124">Ajoutez une classe de démarrage</span><span class="sxs-lookup"><span data-stu-id="1b31b-124">Add a Startup Class</span></span>

<span data-ttu-id="1b31b-125">Ensuite, ajoutez une classe de démarrage OWIN.</span><span class="sxs-lookup"><span data-stu-id="1b31b-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="1b31b-126">Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **ajouter**, puis sélectionnez **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="1b31b-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="1b31b-127">Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez **classe de démarrage Owin**.</span><span class="sxs-lookup"><span data-stu-id="1b31b-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="1b31b-128">Pour plus d’informations sur la configuration de la classe de démarrage, consultez [détection de classe de démarrage OWIN](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="1b31b-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="1b31b-129">Ajoutez le code suivant à la méthode `Startup1.Configuration` :</span><span class="sxs-lookup"><span data-stu-id="1b31b-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="1b31b-130">Ce code ajoute un simple morceau de l’intergiciel (middleware) au pipeline OWIN, implémenté comme une fonction qui reçoit un **Microsoft.Owin.IOwinContext** instance.</span><span class="sxs-lookup"><span data-stu-id="1b31b-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="1b31b-131">Lorsque le serveur reçoit une requête HTTP, le pipeline OWIN appelle l’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="1b31b-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="1b31b-132">L’intergiciel (middleware) définit le type de contenu pour la réponse et écrit le corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="1b31b-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="1b31b-133">Le modèle de classe de démarrage OWIN est disponible dans Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="1b31b-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="1b31b-134">Si vous utilisez Visual Studio 2012, ajoutez simplement une classe vide nommée `Startup1`et collez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="1b31b-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="1b31b-135">Exécution de l'application</span><span class="sxs-lookup"><span data-stu-id="1b31b-135">Run the Application</span></span>

<span data-ttu-id="1b31b-136">Appuyez sur F5 pour commencer le débogage.</span><span class="sxs-lookup"><span data-stu-id="1b31b-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="1b31b-137">Visual Studio ouvre une fenêtre de navigateur à `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="1b31b-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="1b31b-138">La page doit ressembler à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="1b31b-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="1b31b-139">L’auto-hébergement OWIN dans une Application Console</span><span class="sxs-lookup"><span data-stu-id="1b31b-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="1b31b-140">Il est facile de convertir cette application à partir de l’hébergement IIS pour l’auto-hébergement dans un processus personnalisé.</span><span class="sxs-lookup"><span data-stu-id="1b31b-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="1b31b-141">Avec hébergement IIS, IIS sert à la fois le serveur HTTP et que le processus qui héberge le service.</span><span class="sxs-lookup"><span data-stu-id="1b31b-141">With IIS hosting, IIS acts as both the HTTP server and as the process that hosts the service.</span></span> <span data-ttu-id="1b31b-142">Avec l’auto-hébergement, votre application crée le processus et utilise le **HttpListener** classe en tant que le serveur HTTP.</span><span class="sxs-lookup"><span data-stu-id="1b31b-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="1b31b-143">Dans Visual Studio, créez une nouvelle application de console.</span><span class="sxs-lookup"><span data-stu-id="1b31b-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="1b31b-144">Dans la fenêtre de Console du Gestionnaire de Package, tapez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="1b31b-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="1b31b-145">Ajouter un `Startup1` classe à partir de la partie 1 de ce didacticiel au projet.</span><span class="sxs-lookup"><span data-stu-id="1b31b-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="1b31b-146">Vous n’avez pas besoin de modifier cette classe.</span><span class="sxs-lookup"><span data-stu-id="1b31b-146">You don't need to modify this class.</span></span>

<span data-ttu-id="1b31b-147">Implémenter l’application `Main` méthode comme suit.</span><span class="sxs-lookup"><span data-stu-id="1b31b-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="1b31b-148">Lorsque vous exécutez l’application console, le serveur commence à écouter les `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="1b31b-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="1b31b-149">Si vous accédez à cette adresse dans un navigateur web, vous verrez la page « Hello world ».</span><span class="sxs-lookup"><span data-stu-id="1b31b-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="1b31b-150">Ajouter des tests de diagnostic OWIN</span><span class="sxs-lookup"><span data-stu-id="1b31b-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="1b31b-151">Le package Microsoft.Owin.Diagnostics contient un intergiciel (middleware) qui intercepte les exceptions non gérées et affiche une page HTML avec les détails de l’erreur.</span><span class="sxs-lookup"><span data-stu-id="1b31b-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="1b31b-152">Cette page même manière que la page d’erreur ASP.NET qui est parfois appelée le «[écran jaune de la mort](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)» (YSOD).</span><span class="sxs-lookup"><span data-stu-id="1b31b-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="1b31b-153">Comme le YSOD, la page d’erreur Katana est utile lors du développement, mais il est conseillé de le désactiver en mode de production.</span><span class="sxs-lookup"><span data-stu-id="1b31b-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="1b31b-154">Pour installer le package de diagnostic dans votre projet, tapez la commande suivante dans la fenêtre de Console du Gestionnaire de Package :</span><span class="sxs-lookup"><span data-stu-id="1b31b-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="1b31b-155">Modifier le code dans votre `Startup1.Configuration` méthode comme suit :</span><span class="sxs-lookup"><span data-stu-id="1b31b-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="1b31b-156">Maintenant utiliser CTRL + F5 pour exécuter l’application sans débogage, afin que Visual Studio s’arrête pas sur l’exception.</span><span class="sxs-lookup"><span data-stu-id="1b31b-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="1b31b-157">L’application comporte comme avant, jusqu'à ce que vous accédez à `http://localhost/fail`, moment où l’application lève l’exception.</span><span class="sxs-lookup"><span data-stu-id="1b31b-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="1b31b-158">L’intergiciel (middleware) page d’erreur interceptera l’exception et afficher une page HTML avec des informations sur l’erreur.</span><span class="sxs-lookup"><span data-stu-id="1b31b-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="1b31b-159">Vous pouvez cliquer sur les onglets pour afficher la pile de chaîne de requête, les cookies, en-tête de demande et les variables d’environnement OWIN.</span><span class="sxs-lookup"><span data-stu-id="1b31b-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="1b31b-160">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="1b31b-160">Next Steps</span></span>

- [<span data-ttu-id="1b31b-161">Détection de classe de démarrage OWIN</span><span class="sxs-lookup"><span data-stu-id="1b31b-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="1b31b-162">Utiliser OWIN pour auto-héberger l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1b31b-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="1b31b-163">Utiliser OWIN pour auto-héberger SignalR</span><span class="sxs-lookup"><span data-stu-id="1b31b-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
