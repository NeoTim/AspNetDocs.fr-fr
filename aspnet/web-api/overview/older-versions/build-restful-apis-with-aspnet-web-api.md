---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Générer des API RESTful avec les API Web ASP.NET | Microsoft Docs
author: rick-anderson
description: Ces dernières années, il est clair que HTTP n’est pas simplement pour servir des pages HTML. Il est également une plateforme puissante pour la création d’API Web, à l’aide d’un certain nombre de o...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f1f5ebbf5170f205be331b6402951fb429196046
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58423713"
---
<a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="6ec19-104">Générer des API RESTful avec les API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6ec19-104">Build RESTful APIs with ASP.NET Web API</span></span>
====================
<span data-ttu-id="6ec19-105">par [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="6ec19-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="6ec19-106">Ces dernières années, il est clair que HTTP n’est pas simplement pour servir des pages HTML.</span><span class="sxs-lookup"><span data-stu-id="6ec19-106">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="6ec19-107">Il est également une plateforme puissante pour la création d’API Web, à l’aide d’un petit nombre de verbes (GET, POST et ainsi de suite) ainsi que de quelques concepts simples comme *URI* et *en-têtes*.</span><span class="sxs-lookup"><span data-stu-id="6ec19-107">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="6ec19-108">API Web ASP.NET est un ensemble de composants qui simplifient la programmation de HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ec19-108">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="6ec19-109">Car il est basé sur le runtime ASP.NET MVC, API Web gère automatiquement les détails de bas niveau transport HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ec19-109">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="6ec19-110">En même temps, les API Web expose naturellement le modèle de programmation HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ec19-110">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="6ec19-111">En fait, l’un des objectifs de l’API Web consiste à *pas* clarifient la réalité de HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ec19-111">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="6ec19-112">Par conséquent, les API Web est flexible et facile à étendre.</span><span class="sxs-lookup"><span data-stu-id="6ec19-112">As a result, Web API is both flexible and easy to extend.</span></span> <span data-ttu-id="6ec19-113">Dans cet atelier, vous allez utiliser les API Web pour générer une API REST simple pour une application de gestionnaire de contacts.</span><span class="sxs-lookup"><span data-stu-id="6ec19-113">In this hands-on lab, you will use Web API to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="6ec19-114">Vous allez également générer un client pour utiliser l’API.</span><span class="sxs-lookup"><span data-stu-id="6ec19-114">You will also build a client to consume the API.</span></span> <span data-ttu-id="6ec19-115">Le style architectural REST s’est avérée pour être un moyen efficace pour tirer parti de HTTP - même s’il n’est certainement pas l’approche valide uniquement pour HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ec19-115">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="6ec19-116">Le Gestionnaire de contacts exposera le RESTful pour répertorier, l’ajout et suppression de contacts, entre autres.</span><span class="sxs-lookup"><span data-stu-id="6ec19-116">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> <span data-ttu-id="6ec19-117">Cet atelier nécessite une connaissance élémentaire de HTTP, REST et suppose que vous avez une connaissance de base du code HTML, JavaScript et jQuery.</span><span class="sxs-lookup"><span data-stu-id="6ec19-117">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="6ec19-118">Le site Web ASP.NET a une zone dédiée à l’infrastructure d’API Web ASP.NET à [ https://asp.net/web-api ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="6ec19-118">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="6ec19-119">Ce site continuera de fournir aux dernières informations, des exemples et des informations sur les API Web, par conséquent, archivez-le fréquemment si vous souhaitez en savoir plus sur l’art de créer des API Web personnalisées disponibles pour n’importe quel framework de l’appareil ou de développement.</span><span class="sxs-lookup"><span data-stu-id="6ec19-119">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="6ec19-120">ASP.NET Web API, similaire à ASP.NET MVC 4, a une grande souplesse en termes de séparer la couche de service provenant des contrôleurs de ce qui vous permet d’utiliser plusieurs des infrastructures d’Injection de dépendance disponibles assez facile.</span><span class="sxs-lookup"><span data-stu-id="6ec19-120">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="6ec19-121">Il existe un bon échantillon dans MSDN qui montre comment utiliser Ninject l’injection de dépendances dans un projet d’API Web ASP.NET que vous pouvez le télécharger à partir de [ici](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="6ec19-121">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="6ec19-122">Tous les exemples de code et extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="6ec19-122">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="6ec19-123">Objectifs</span><span class="sxs-lookup"><span data-stu-id="6ec19-123">Objectives</span></span>

<span data-ttu-id="6ec19-124">Dans cet atelier, vous allez apprendre comment :</span><span class="sxs-lookup"><span data-stu-id="6ec19-124">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="6ec19-125">Implémenter une API Web RESTful</span><span class="sxs-lookup"><span data-stu-id="6ec19-125">Implement a RESTful Web API</span></span>
- <span data-ttu-id="6ec19-126">Appeler l’API à partir d’un client HTML</span><span class="sxs-lookup"><span data-stu-id="6ec19-126">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="6ec19-127">Prérequis</span><span class="sxs-lookup"><span data-stu-id="6ec19-127">Prerequisites</span></span>

<span data-ttu-id="6ec19-128">Les éléments suivants sont nécessaire pour terminer ce laboratoire pratique :</span><span class="sxs-lookup"><span data-stu-id="6ec19-128">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="6ec19-129">[Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieure (lire [annexe B](#AppendixB) pour obtenir des instructions sur la façon d’installer).</span><span class="sxs-lookup"><span data-stu-id="6ec19-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="6ec19-130">Installation</span><span class="sxs-lookup"><span data-stu-id="6ec19-130">Setup</span></span>

<span data-ttu-id="6ec19-131">**L’installation des extraits de Code**</span><span class="sxs-lookup"><span data-stu-id="6ec19-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="6ec19-132">Pour des raisons pratiques, une grande partie du code que vous gérez le long de ce laboratoire est disponible en tant que les extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6ec19-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="6ec19-133">Pour installer les extraits de code exécuter **.\Source\Setup\CodeSnippets.vsi** fichier.</span><span class="sxs-lookup"><span data-stu-id="6ec19-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="6ec19-134">Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et que vous souhaitiez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [annexe a : À l’aide d’extraits de Code](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="6ec19-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="6ec19-135">Exercices</span><span class="sxs-lookup"><span data-stu-id="6ec19-135">Exercises</span></span>

<span data-ttu-id="6ec19-136">Ce laboratoire pratique inclut l’exercice suivant :</span><span class="sxs-lookup"><span data-stu-id="6ec19-136">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="6ec19-137">Exercice 1 : Créer une API Web en lecture seule</span><span class="sxs-lookup"><span data-stu-id="6ec19-137">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="6ec19-138">Exercice 2 : Créer une API du Web en lecture/écriture</span><span class="sxs-lookup"><span data-stu-id="6ec19-138">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="6ec19-139">Exercice 3 : Utiliser l’API Web à partir d’un Client HTML</span><span class="sxs-lookup"><span data-stu-id="6ec19-139">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="6ec19-140">Chaque exercice est accompagné par un **fin** dossier contenant la solution obtenue, vous devez obtenir après avoir effectué les exercices.</span><span class="sxs-lookup"><span data-stu-id="6ec19-140">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="6ec19-141">Si vous avez besoin d’aide supplémentaire sur l’utilisation via les exercices, vous pouvez utiliser cette solution comme guide.</span><span class="sxs-lookup"><span data-stu-id="6ec19-141">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="6ec19-142">Durée estimée pour effectuer ce laboratoire : **60 minutes**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-142">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="6ec19-143">Exercice 1 : Créer une API Web en lecture seule</span><span class="sxs-lookup"><span data-stu-id="6ec19-143">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="6ec19-144">Dans cet exercice, vous implémenterez les méthodes GET en lecture seule pour le Gestionnaire de contacts.</span><span class="sxs-lookup"><span data-stu-id="6ec19-144">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="6ec19-145">Tâche 1 - Création du projet d’API</span><span class="sxs-lookup"><span data-stu-id="6ec19-145">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="6ec19-146">Dans cette tâche, vous allez utiliser les nouveaux modèles de projet web ASP.NET pour créer une application de web API Web.</span><span class="sxs-lookup"><span data-stu-id="6ec19-146">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="6ec19-147">Exécutez **Visual Studio 2012 Express pour Web**, pour cela, accédez à **Démarrer** et type **Visual Studio Express pour Web** puis appuyez sur **entrée**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-147">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="6ec19-148">À partir de la **fichier** menu, sélectionnez **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-148">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="6ec19-149">Sélectionnez le **Visual C# | Web** type dans l’arborescence de type de projet du projet, puis sélectionnez le **ASP.NET MVC 4 Web Application** type de projet.</span><span class="sxs-lookup"><span data-stu-id="6ec19-149">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="6ec19-150">Définissez le projet **nom** à *ContactManager* et **nom de la Solution** à *commencer*, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-150">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="6ec19-151">![Création d’un projet Application ASP.NET MVC 4.0 Web](build-restful-apis-with-aspnet-web-api/_static/image1.png "création d’un projet Application ASP.NET MVC 4.0 Web")</span><span class="sxs-lookup"><span data-stu-id="6ec19-151">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="6ec19-152">*Création d’un projet Application ASP.NET MVC 4.0 Web*</span><span class="sxs-lookup"><span data-stu-id="6ec19-152">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="6ec19-153">Dans la boîte de dialogue du type de projet ASP.NET MVC 4, sélectionnez le **API Web** type de projet.</span><span class="sxs-lookup"><span data-stu-id="6ec19-153">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="6ec19-154">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-154">Click **OK**.</span></span>

    <span data-ttu-id="6ec19-155">![Qui spécifie le type de projet API Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "spécifiant le type de projet API Web")</span><span class="sxs-lookup"><span data-stu-id="6ec19-155">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="6ec19-156">*Qui spécifie le type de projet API Web*</span><span class="sxs-lookup"><span data-stu-id="6ec19-156">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="6ec19-157">Tâche 2 : création de contrôleurs d’API de gestionnaire de contacts</span><span class="sxs-lookup"><span data-stu-id="6ec19-157">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="6ec19-158">Dans cette tâche, vous allez créer les classes de contrôleur dans lequel résidera méthodes d’API.</span><span class="sxs-lookup"><span data-stu-id="6ec19-158">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="6ec19-159">Supprimer le fichier nommé **ValuesController.cs** dans **contrôleurs** dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="6ec19-159">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="6ec19-160">Cliquez sur le **contrôleurs** dossier dans le projet, puis sélectionnez **ajouter | Contrôleur** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="6ec19-160">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="6ec19-161">![Ajoutez un nouveau contrôleur au projet](build-restful-apis-with-aspnet-web-api/_static/image3.png "Ajout d’un nouveau contrôleur au projet")</span><span class="sxs-lookup"><span data-stu-id="6ec19-161">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="6ec19-162">*Ajoutez un nouveau contrôleur au projet*</span><span class="sxs-lookup"><span data-stu-id="6ec19-162">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="6ec19-163">Dans le **ajouter un contrôleur** boîte de dialogue qui s’affiche, sélectionnez **contrôleur d’API vide** dans le menu modèle.</span><span class="sxs-lookup"><span data-stu-id="6ec19-163">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="6ec19-164">Nommez la classe de contrôleur **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-164">Name the controller class **ContactController**.</span></span> <span data-ttu-id="6ec19-165">Ensuite, cliquez sur **ajouter.**</span><span class="sxs-lookup"><span data-stu-id="6ec19-165">Then, click **Add.**</span></span>

    <span data-ttu-id="6ec19-166">![À l’aide de la boîte de dialogue Ajouter un contrôleur pour créer un nouveau contrôleur API Web](build-restful-apis-with-aspnet-web-api/_static/image4.png "à l’aide de la boîte de dialogue Ajouter un contrôleur pour créer un nouveau contrôleur API Web")</span><span class="sxs-lookup"><span data-stu-id="6ec19-166">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="6ec19-167">*À l’aide de la boîte de dialogue Ajouter un contrôleur pour créer un nouveau contrôleur API Web*</span><span class="sxs-lookup"><span data-stu-id="6ec19-167">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="6ec19-168">Ajoutez le code suivant à la **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-168">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="6ec19-169">(Code Snippet - *Lab d’API Web - Ex01 - Get, méthode d’API*)</span><span class="sxs-lookup"><span data-stu-id="6ec19-169">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="6ec19-170">Appuyez sur **F5** pour déboguer l’application.</span><span class="sxs-lookup"><span data-stu-id="6ec19-170">Press **F5** to debug the application.</span></span> <span data-ttu-id="6ec19-171">La page d’accueil par défaut pour un projet d’API Web doit apparaître.</span><span class="sxs-lookup"><span data-stu-id="6ec19-171">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="6ec19-172">![La page d’accueil par défaut d’une application ASP.NET Web API](build-restful-apis-with-aspnet-web-api/_static/image5.png "la page d’accueil par défaut d’une application API Web ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="6ec19-172">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="6ec19-173">*La page d’accueil par défaut d’une application API Web ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="6ec19-173">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="6ec19-174">Dans la fenêtre Internet Explorer, appuyez sur la **F12** touche pour ouvrir le **outils de développement** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="6ec19-174">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="6ec19-175">Cliquez sur le **réseau** onglet, puis cliquez sur le **démarrer capture** bouton pour commencer la capture du trafic réseau dans la fenêtre.</span><span class="sxs-lookup"><span data-stu-id="6ec19-175">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="6ec19-176">![Ouverture de l’onglet réseau et à lancer de capture réseau](build-restful-apis-with-aspnet-web-api/_static/image6.png "ouvrant l’onglet réseau et à lancer de capture réseau")</span><span class="sxs-lookup"><span data-stu-id="6ec19-176">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="6ec19-177">*Ouverture de l’onglet réseau et à lancer de capture réseau*</span><span class="sxs-lookup"><span data-stu-id="6ec19-177">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="6ec19-178">Ajoutez l’URL dans la barre d’adresses du navigateur avec **/api/contact** et appuyez sur ENTRÉE.</span><span class="sxs-lookup"><span data-stu-id="6ec19-178">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="6ec19-179">Les détails de transmission apparaîtra dans la fenêtre de capture réseau.</span><span class="sxs-lookup"><span data-stu-id="6ec19-179">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="6ec19-180">Notez que le type MIME de la réponse est **application/json**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-180">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="6ec19-181">Cet exemple montre comment le format de sortie par défaut est JSON.</span><span class="sxs-lookup"><span data-stu-id="6ec19-181">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="6ec19-182">![Affichez la sortie de la demande d’API Web dans la vue du réseau](build-restful-apis-with-aspnet-web-api/_static/image7.png "affichant la sortie de la demande d’API Web dans la vue du réseau")</span><span class="sxs-lookup"><span data-stu-id="6ec19-182">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="6ec19-183">*Affichez la sortie de la demande d’API Web dans la vue du réseau*</span><span class="sxs-lookup"><span data-stu-id="6ec19-183">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="6ec19-184">À ce stade, par défaut d’Internet Explorer 10 sera pour demander si l’utilisateur souhaite enregistrer ou ouvrir le flux résultant de l’appel d’API Web.</span><span class="sxs-lookup"><span data-stu-id="6ec19-184">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="6ec19-185">La sortie sera un fichier texte contenant le résultat JSON de l’appel de l’URL de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="6ec19-185">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="6ec19-186">N’annulez pas la boîte de dialogue afin de pouvoir regarder le contenu de la réponse via la fenêtre outil de développeurs.</span><span class="sxs-lookup"><span data-stu-id="6ec19-186">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="6ec19-187">Cliquez sur le **vue détaillée** bouton pour afficher plus de détails sur la réponse de cet appel d’API.</span><span class="sxs-lookup"><span data-stu-id="6ec19-187">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="6ec19-188">![Basculez vers la vue détaillée](build-restful-apis-with-aspnet-web-api/_static/image8.png "passer en mode Détails")</span><span class="sxs-lookup"><span data-stu-id="6ec19-188">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="6ec19-189">*Basculez vers la vue détaillée*</span><span class="sxs-lookup"><span data-stu-id="6ec19-189">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="6ec19-190">Cliquez sur le **corps de réponse** onglet pour afficher le texte de réponse JSON.</span><span class="sxs-lookup"><span data-stu-id="6ec19-190">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="6ec19-191">![Affichage de l’objet JSON de sortie de texte dans le Moniteur réseau](build-restful-apis-with-aspnet-web-api/_static/image9.png "affichant le code JSON de sortie de texte dans le Moniteur réseau")</span><span class="sxs-lookup"><span data-stu-id="6ec19-191">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="6ec19-192">*Affichage du texte de sortie JSON dans le Moniteur réseau*</span><span class="sxs-lookup"><span data-stu-id="6ec19-192">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="6ec19-193">Tâche 3 : création des modèles de Contact et d’augmenter le contrôleur de Contact</span><span class="sxs-lookup"><span data-stu-id="6ec19-193">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="6ec19-194">Dans cette tâche, vous allez créer les classes de contrôleur dans lequel résidera méthodes d’API.</span><span class="sxs-lookup"><span data-stu-id="6ec19-194">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="6ec19-195">Cliquez sur le **modèles** dossier et sélectionnez **ajouter | Classe...**  dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="6ec19-195">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="6ec19-196">![Ajout d’un nouveau modèle à l’application web](build-restful-apis-with-aspnet-web-api/_static/image10.png "Ajout d’un nouveau modèle à l’application web")</span><span class="sxs-lookup"><span data-stu-id="6ec19-196">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="6ec19-197">*Ajout d’un nouveau modèle à l’application web*</span><span class="sxs-lookup"><span data-stu-id="6ec19-197">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="6ec19-198">Dans le **ajouter un nouvel élément** boîte de dialogue, nommez le nouveau fichier **Contact.cs** et cliquez sur **ajouter.**</span><span class="sxs-lookup"><span data-stu-id="6ec19-198">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="6ec19-199">![Création du nouveau fichier de classe de Contact](build-restful-apis-with-aspnet-web-api/_static/image11.png "création du nouveau fichier de classe de Contact")</span><span class="sxs-lookup"><span data-stu-id="6ec19-199">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="6ec19-200">*Création du nouveau fichier de classe de Contact*</span><span class="sxs-lookup"><span data-stu-id="6ec19-200">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="6ec19-201">Ajoutez le code en surbrillance suivant à la **Contact** classe.</span><span class="sxs-lookup"><span data-stu-id="6ec19-201">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="6ec19-202">(Code Snippet - *classe Contact API Lab - Ex01 - Web*)</span><span class="sxs-lookup"><span data-stu-id="6ec19-202">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="6ec19-203">Dans le **ContactController** de classe, sélectionnez le mot **chaîne** dans la définition de méthode de la **obtenir** (méthode) et tapez le mot *Contact*.</span><span class="sxs-lookup"><span data-stu-id="6ec19-203">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="6ec19-204">Une fois que le mot est tapé dans, un indicateur s’affiche au début du mot **Contact**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-204">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="6ec19-205">Soit maintenez la **Ctrl** clé et appuyez sur la touche point (.) ou cliquez sur l’icône à l’aide de votre souris pour ouvrir la boîte de dialogue d’assistance dans l’éditeur de code, pour renseigner automatiquement le **à l’aide de** directive pour les modèles espace de noms.</span><span class="sxs-lookup"><span data-stu-id="6ec19-205">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![À l’aide de l’assistance Intellisense pour les déclarations d’espace de noms](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="6ec19-207">*À l’aide de l’assistance Intellisense pour les déclarations d’espace de noms*</span><span class="sxs-lookup"><span data-stu-id="6ec19-207">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="6ec19-208">Modifier le code pour le **obtenir** méthode pour qu’elle retourne un tableau d’instances de modèle de Contact.</span><span class="sxs-lookup"><span data-stu-id="6ec19-208">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="6ec19-209">(Code Snippet - *Lab d’API Web - Ex01 - renvoi d’une liste de contacts*)</span><span class="sxs-lookup"><span data-stu-id="6ec19-209">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="6ec19-210">Appuyez sur **F5** pour déboguer l’application web dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="6ec19-210">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="6ec19-211">Pour afficher les modifications apportées à la sortie de réponse de l’API, procédez comme suit.</span><span class="sxs-lookup"><span data-stu-id="6ec19-211">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="6ec19-212">Une fois que le navigateur s’ouvre, appuyez sur **F12** si les outils de développement ne sont pas encore ouverts.</span><span class="sxs-lookup"><span data-stu-id="6ec19-212">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="6ec19-213">Cliquez sur le **réseau** onglet.</span><span class="sxs-lookup"><span data-stu-id="6ec19-213">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="6ec19-214">Appuyez sur la **démarrer capture** bouton.</span><span class="sxs-lookup"><span data-stu-id="6ec19-214">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="6ec19-215">Ajouter le suffixe d’URL **/api/contact** à l’URL dans la barre d’adresses et appuyez sur la **entrée** clé.</span><span class="sxs-lookup"><span data-stu-id="6ec19-215">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="6ec19-216">Appuyez sur la **vue détaillée** bouton.</span><span class="sxs-lookup"><span data-stu-id="6ec19-216">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="6ec19-217">Sélectionnez le **corps de réponse** onglet. Vous devez voir une chaîne JSON représentant la forme sérialisée d’un tableau d’instances de Contact.</span><span class="sxs-lookup"><span data-stu-id="6ec19-217">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="6ec19-218">![JSON sérialisée des résultats d’un appel de méthode d’API Web complex](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON sérialisée des résultats d’un appel de méthode d’API Web complex")</span><span class="sxs-lookup"><span data-stu-id="6ec19-218">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="6ec19-219">*Sortie JSON sérialisé d’un appel de méthode d’API Web complexe*</span><span class="sxs-lookup"><span data-stu-id="6ec19-219">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="6ec19-220">Tâche 4 - extraction de fonctionnalités dans une couche de Service</span><span class="sxs-lookup"><span data-stu-id="6ec19-220">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="6ec19-221">Cette tâche va vous montrer comment extraire des fonctionnalités dans une couche de Service pour faciliter aux développeurs de séparer leurs fonctionnalités de service à partir de la couche de contrôleur, ce qui permet la réutilisation des services qui effectuent réellement le travail.</span><span class="sxs-lookup"><span data-stu-id="6ec19-221">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="6ec19-222">Créez un nouveau dossier dans la racine de la solution et nommez-le **Services**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-222">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="6ec19-223">Pour ce faire, cliquez sur **ContactManager** projet, sélectionnez **ajouter** | **nouveau dossier**, nommez-le *Services*.</span><span class="sxs-lookup"><span data-stu-id="6ec19-223">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="6ec19-224">![Dossier des Services de création](build-restful-apis-with-aspnet-web-api/_static/image14.png "dossier de création de Services")</span><span class="sxs-lookup"><span data-stu-id="6ec19-224">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="6ec19-225">*Création du dossier de Services*</span><span class="sxs-lookup"><span data-stu-id="6ec19-225">*Creating Services folder*</span></span>
2. <span data-ttu-id="6ec19-226">Cliquez sur le **Services** dossier et sélectionnez **ajouter | Classe...**  dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="6ec19-226">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="6ec19-227">![Ajout d’une nouvelle classe vers le dossier Services](build-restful-apis-with-aspnet-web-api/_static/image15.png "ajoutant une nouvelle classe vers le dossier Services")</span><span class="sxs-lookup"><span data-stu-id="6ec19-227">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="6ec19-228">*Ajout d’une nouvelle classe vers le dossier Services*</span><span class="sxs-lookup"><span data-stu-id="6ec19-228">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="6ec19-229">Lorsque le **ajouter un nouvel élément** boîte de dialogue s’affiche, nommez la nouvelle classe **ContactRepository** et cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-229">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="6ec19-230">![Création d’un fichier de classe pour contenir le code de la couche de service de référentiel de Contact](build-restful-apis-with-aspnet-web-api/_static/image16.png "création d’un fichier de classe pour contenir le code de la couche de service de référentiel de Contact")</span><span class="sxs-lookup"><span data-stu-id="6ec19-230">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="6ec19-231">*Création d’un fichier de classe pour contenir le code de la couche de service de référentiel de Contact*</span><span class="sxs-lookup"><span data-stu-id="6ec19-231">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="6ec19-232">Ajoutez une directive pour le **ContactRepository.cs** fichier à inclure l’espace de noms de modèles.</span><span class="sxs-lookup"><span data-stu-id="6ec19-232">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="6ec19-233">Ajoutez le code en surbrillance suivant à la **ContactRepository.cs** fichier pour implémenter la méthode de GetAllContacts.</span><span class="sxs-lookup"><span data-stu-id="6ec19-233">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="6ec19-234">(Code Snippet - *Web de référentiel de Contact API Lab - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="6ec19-234">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="6ec19-235">Ouvrez le **ContactController.cs** fichier s’il n’est pas déjà ouvert.</span><span class="sxs-lookup"><span data-stu-id="6ec19-235">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="6ec19-236">Ajoutez le code suivant à l’aide d’instruction à la section de déclaration d’espace de noms du fichier.</span><span class="sxs-lookup"><span data-stu-id="6ec19-236">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="6ec19-237">Ajoutez le code en surbrillance suivant à la **ContactController.cs** classe pour ajouter un champ privé pour représenter l’instance du référentiel, afin que le reste de la classe les membres peuvent apporter utiliser de l’implémentation de service.</span><span class="sxs-lookup"><span data-stu-id="6ec19-237">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="6ec19-238">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span><span class="sxs-lookup"><span data-stu-id="6ec19-238">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="6ec19-239">Modifier le **obtenir** méthode donc qu’il est utiliser le service de référentiel de contact.</span><span class="sxs-lookup"><span data-stu-id="6ec19-239">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="6ec19-240">(Code Snippet - *Lab d’API Web - Ex01 - renvoi d’une liste de contacts via le référentiel*)</span><span class="sxs-lookup"><span data-stu-id="6ec19-240">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="6ec19-241">Placez un point d’arrêt sur la **ContactController**de **obtenir** définition de méthode.</span><span class="sxs-lookup"><span data-stu-id="6ec19-241">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="6ec19-242">![Ajout de points d’arrêt au contrôleur de contact](build-restful-apis-with-aspnet-web-api/_static/image17.png "ajoutant des points d’arrêt au contrôleur de contact")</span><span class="sxs-lookup"><span data-stu-id="6ec19-242">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="6ec19-243">*Ajout de points d’arrêt au contrôleur de contact*</span><span class="sxs-lookup"><span data-stu-id="6ec19-243">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="6ec19-244">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="6ec19-244">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="6ec19-245">Lorsque le navigateur s’ouvre, appuyez sur **F12** pour ouvrir les outils de développement.</span><span class="sxs-lookup"><span data-stu-id="6ec19-245">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="6ec19-246">Cliquez sur le **réseau** onglet.</span><span class="sxs-lookup"><span data-stu-id="6ec19-246">Click the **Network** tab.</span></span>
14. <span data-ttu-id="6ec19-247">Cliquez sur le **démarrer capture** bouton.</span><span class="sxs-lookup"><span data-stu-id="6ec19-247">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="6ec19-248">Ajoutez l’URL dans la barre d’adresses avec le suffixe **/api/contact** et appuyez sur **entrée** pour charger le contrôleur d’API.</span><span class="sxs-lookup"><span data-stu-id="6ec19-248">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="6ec19-249">Visual Studio 2012 doit s’arrêter une fois **obtenir** méthode commence à s’exécuter.</span><span class="sxs-lookup"><span data-stu-id="6ec19-249">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="6ec19-250">![Avec rupture dans la méthode Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "avec rupture dans la méthode Get")</span><span class="sxs-lookup"><span data-stu-id="6ec19-250">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="6ec19-251">*Avec rupture dans la méthode Get*</span><span class="sxs-lookup"><span data-stu-id="6ec19-251">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="6ec19-252">Appuyez sur **F5** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="6ec19-252">Press **F5** to continue.</span></span>
18. <span data-ttu-id="6ec19-253">Revenez dans Internet Explorer si elle n’est pas déjà le focus.</span><span class="sxs-lookup"><span data-stu-id="6ec19-253">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="6ec19-254">Notez la fenêtre de capture réseau.</span><span class="sxs-lookup"><span data-stu-id="6ec19-254">Note the network capture window.</span></span>

    <span data-ttu-id="6ec19-255">![Affichage dans Internet Explorer affichant les résultats de l’appel d’API Web de réseau](build-restful-apis-with-aspnet-web-api/_static/image19.png "réseau dans Internet Explorer affichant les résultats de l’appel d’API Web")</span><span class="sxs-lookup"><span data-stu-id="6ec19-255">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="6ec19-256">*Affichage du réseau dans Internet Explorer affichant les résultats de l’appel d’API Web*</span><span class="sxs-lookup"><span data-stu-id="6ec19-256">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="6ec19-257">Cliquez sur le **vue détaillée** bouton.</span><span class="sxs-lookup"><span data-stu-id="6ec19-257">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="6ec19-258">Cliquez sur le **corps de réponse** onglet. Notez la sortie JSON de l’appel d’API, et comment il représente les deux contacts récupérées par la couche de service.</span><span class="sxs-lookup"><span data-stu-id="6ec19-258">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="6ec19-259">![Affichage de la sortie JSON à partir de l’API Web dans la fenêtre Outils de développement](build-restful-apis-with-aspnet-web-api/_static/image20.png "affichant la sortie JSON à partir de l’API Web dans la fenêtre d’outils de développeur")</span><span class="sxs-lookup"><span data-stu-id="6ec19-259">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="6ec19-260">*Affichage de la sortie JSON à partir de l’API Web dans la fenêtre d’outils de développeur*</span><span class="sxs-lookup"><span data-stu-id="6ec19-260">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="6ec19-261">Exercice 2 : Créer une API du Web en lecture/écriture</span><span class="sxs-lookup"><span data-stu-id="6ec19-261">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="6ec19-262">Dans cet exercice, vous allez implémenter POST et PUT des méthodes pour le Gestionnaire de contact pour l’activer avec les fonctionnalités de modification de données.</span><span class="sxs-lookup"><span data-stu-id="6ec19-262">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="6ec19-263">Tâche 1 : ouvrez le projet d’API Web</span><span class="sxs-lookup"><span data-stu-id="6ec19-263">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="6ec19-264">Dans cette tâche, vous allez vous préparer à améliorer le projet d’API Web créé dans l’exercice 1 afin qu’il accepte l’entrée d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="6ec19-264">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="6ec19-265">Exécutez **Visual Studio 2012 Express pour Web**, pour cela, accédez à **Démarrer** et type **Visual Studio Express pour Web** puis appuyez sur **entrée**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-265">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="6ec19-266">Ouvrez le **commencer** solution situé dans **/Ex02-ReadWriteWebAPI/début du fichierSource/** dossier.</span><span class="sxs-lookup"><span data-stu-id="6ec19-266">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="6ec19-267">Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="6ec19-267">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="6ec19-268">Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="6ec19-268">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6ec19-269">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-269">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="6ec19-270">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="6ec19-270">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="6ec19-271">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-271">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="6ec19-272">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="6ec19-272">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6ec19-273">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="6ec19-273">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6ec19-274">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="6ec19-274">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="6ec19-275">Ouvrez le **Services/ContactRepository.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="6ec19-275">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="6ec19-276">Tâche 2 : ajout de fonctionnalités de persistance des données à l’implémentation de référentiel de Contact</span><span class="sxs-lookup"><span data-stu-id="6ec19-276">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="6ec19-277">Dans cette tâche, vous augmenterons la classe ContactRepository du projet API Web créé dans l’exercice 1 afin qu’il peut conserver et accepter l’entrée d’utilisateur et de nouvelles instances de Contact.</span><span class="sxs-lookup"><span data-stu-id="6ec19-277">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="6ec19-278">Ajoutez la constante suivante à la **ContactRepository** classe pour représenter le nom de la mémoire cache élément clé nom du serveur web plus loin dans cet exercice.</span><span class="sxs-lookup"><span data-stu-id="6ec19-278">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="6ec19-279">Ajoutez un constructeur à la **ContactRepository** contenant le code suivant.</span><span class="sxs-lookup"><span data-stu-id="6ec19-279">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="6ec19-280">(Code Snippet - *Web constructeur de référentiel de Contact API Lab - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="6ec19-280">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="6ec19-281">Modifier le code pour le **GetAllContacts** méthode comme illustré ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="6ec19-281">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="6ec19-282">(Code Snippet - *Lab d’API Web - Ex02 - obtenir tous les Contacts*)</span><span class="sxs-lookup"><span data-stu-id="6ec19-282">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="6ec19-283">Cet exemple est destiné à des fins de démonstration et utilisera du cache du serveur web en tant qu’un support de stockage, afin que les valeurs seront accessibles à plusieurs clients simultanément, au lieu d’utiliser un mécanisme de stockage de Session ou une durée de vie de stockage de demande.</span><span class="sxs-lookup"><span data-stu-id="6ec19-283">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="6ec19-284">Peut utiliser un Entity Framework, stockage XML ou toute autre variété à la place le cache de serveur web.</span><span class="sxs-lookup"><span data-stu-id="6ec19-284">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="6ec19-285">Implémenter une nouvelle méthode nommée **SaveContact** à la **ContactRepository** classe pour effectuer le travail de l’enregistrement d’un contact.</span><span class="sxs-lookup"><span data-stu-id="6ec19-285">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="6ec19-286">Le **SaveContact** méthode doit prendre un seul **Contact** valeur de paramètre et retourner une valeur booléenne signalant la réussite ou échec.</span><span class="sxs-lookup"><span data-stu-id="6ec19-286">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="6ec19-287">(Code Snippet - *Web API Lab - Ex02 - implémentation de la méthode SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="6ec19-287">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="6ec19-288">Exercice 3 : Utiliser l’API Web à partir d’un Client HTML</span><span class="sxs-lookup"><span data-stu-id="6ec19-288">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="6ec19-289">Dans cet exercice, vous allez créer un client HTML pour appeler l’API Web.</span><span class="sxs-lookup"><span data-stu-id="6ec19-289">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="6ec19-290">Ce client facilite l’échange de données avec l’API Web à l’aide de JavaScript et affiche les résultats dans un navigateur web à l’aide du balisage HTML.</span><span class="sxs-lookup"><span data-stu-id="6ec19-290">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="6ec19-291">Tâche 1 : modification de la vue Index afin de fournir une interface graphique utilisateur pour l’affichage des Contacts</span><span class="sxs-lookup"><span data-stu-id="6ec19-291">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="6ec19-292">Dans cette tâche, vous allez modifier la vue d’Index par défaut de l’application web pour prendre en charge de la configuration requise de l’affichage de la liste des contacts existants dans un navigateur HTML.</span><span class="sxs-lookup"><span data-stu-id="6ec19-292">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="6ec19-293">Ouvrez **Visual Studio 2012 Express pour Web** s’il n’est pas déjà ouvert.</span><span class="sxs-lookup"><span data-stu-id="6ec19-293">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="6ec19-294">Ouvrez le **commencer** solution situé dans **/Ex03-ConsumingWebAPI/début du fichierSource/** dossier.</span><span class="sxs-lookup"><span data-stu-id="6ec19-294">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="6ec19-295">Sinon, vous pouvez continuer à utiliser le **fin** solution obtenu par le biais de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="6ec19-295">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="6ec19-296">Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="6ec19-296">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6ec19-297">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-297">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="6ec19-298">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="6ec19-298">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="6ec19-299">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-299">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="6ec19-300">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="6ec19-300">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6ec19-301">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="6ec19-301">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6ec19-302">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="6ec19-302">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="6ec19-303">Ouvrez le **Index.cshtml** fichier situé dans **vues/accueil** dossier.</span><span class="sxs-lookup"><span data-stu-id="6ec19-303">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="6ec19-304">Remplacez le code HTML au sein de l’élément div avec l’id **corps** afin qu’il ressemble au code suivant.</span><span class="sxs-lookup"><span data-stu-id="6ec19-304">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="6ec19-305">Ajoutez le code Javascript suivant au bas du fichier à exécuter la requête HTTP à l’API Web.</span><span class="sxs-lookup"><span data-stu-id="6ec19-305">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="6ec19-306">Ouvrez le **ContactController.cs** fichier s’il n’est pas déjà ouvert.</span><span class="sxs-lookup"><span data-stu-id="6ec19-306">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="6ec19-307">Placez un point d’arrêt sur la **obtenir** méthode de la **ContactController** classe.</span><span class="sxs-lookup"><span data-stu-id="6ec19-307">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="6ec19-308">![Placer un point d’arrêt sur la méthode Get du contrôleur d’API](build-restful-apis-with-aspnet-web-api/_static/image21.png "placer un point d’arrêt sur la méthode Get du contrôleur d’API")</span><span class="sxs-lookup"><span data-stu-id="6ec19-308">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="6ec19-309">*Placer un point d’arrêt sur la méthode Get du contrôleur d’API*</span><span class="sxs-lookup"><span data-stu-id="6ec19-309">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="6ec19-310">Appuyez sur **F5** pour exécuter le projet.</span><span class="sxs-lookup"><span data-stu-id="6ec19-310">Press **F5** to run the project.</span></span> <span data-ttu-id="6ec19-311">Le navigateur chargera le document HTML.</span><span class="sxs-lookup"><span data-stu-id="6ec19-311">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6ec19-312">Assurez-vous que vous y accédez à l’URL racine de votre application.</span><span class="sxs-lookup"><span data-stu-id="6ec19-312">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="6ec19-313">Une fois que la page se charge et exécute le code JavaScript, le point d’arrêt est atteint et suspend l’exécution du code dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6ec19-313">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="6ec19-314">![Débogage dans les appels d’API Web à l’aide de Visual Studio Express pour Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "débogage dans les appels d’API Web à l’aide de Visual Studio Express pour le Web")</span><span class="sxs-lookup"><span data-stu-id="6ec19-314">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="6ec19-315">*Débogage dans l’appel d’API Web à l’aide de Visual Studio 2012 Express pour le Web*</span><span class="sxs-lookup"><span data-stu-id="6ec19-315">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="6ec19-316">Supprimer le point d’arrêt et appuyez sur **F5** ou de la barre d’outils débogage **continuer** pour continuer le chargement de la vue dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="6ec19-316">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="6ec19-317">Une fois que la fin de l’appel d’API Web devrait être renvoyé à partir de l’API Web appeler affichés sous forme d’éléments de liste dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="6ec19-317">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="6ec19-318">![Résultats de l’appel d’API affichées dans le navigateur en tant qu’éléments de liste](build-restful-apis-with-aspnet-web-api/_static/image23.png "résultats de l’appel d’API affichées dans le navigateur en tant qu’éléments de liste")</span><span class="sxs-lookup"><span data-stu-id="6ec19-318">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="6ec19-319">*Résultats de l’appel d’API affichées dans le navigateur en tant qu’éléments de liste*</span><span class="sxs-lookup"><span data-stu-id="6ec19-319">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="6ec19-320">Arrêter le débogage.</span><span class="sxs-lookup"><span data-stu-id="6ec19-320">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="6ec19-321">Tâche 2 : modification de la vue Index afin de fournir une interface graphique utilisateur pour la création de Contacts</span><span class="sxs-lookup"><span data-stu-id="6ec19-321">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="6ec19-322">Dans cette tâche, vous continuerez à modifier la vue de l’Index de l’application MVC.</span><span class="sxs-lookup"><span data-stu-id="6ec19-322">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="6ec19-323">Un formulaire sera ajouté à la page HTML qui capture l’entrée d’utilisateur et les envoyer à l’API Web pour créer un nouveau Contact et une nouvelle méthode de contrôleur d’API Web sera créée pour collecter la date à partir de l’interface utilisateur graphique.</span><span class="sxs-lookup"><span data-stu-id="6ec19-323">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="6ec19-324">Ouvrez le **ContactController.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="6ec19-324">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="6ec19-325">Ajoutez une nouvelle méthode à la classe de contrôleur nommée **Post** comme indiqué dans le code suivant.</span><span class="sxs-lookup"><span data-stu-id="6ec19-325">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="6ec19-326">(Code Snippet - *API Lab - Ex03 - Post méthode Web*)</span><span class="sxs-lookup"><span data-stu-id="6ec19-326">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="6ec19-327">Ouvrez le **Index.cshtml** de fichiers dans Visual Studio s’il n’est pas déjà ouvert.</span><span class="sxs-lookup"><span data-stu-id="6ec19-327">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="6ec19-328">Ajoutez le code HTML ci-dessous au fichier juste après la liste non triée, que vous avez ajouté dans la tâche précédente.</span><span class="sxs-lookup"><span data-stu-id="6ec19-328">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="6ec19-329">Dans l’élément de script en bas du document, ajoutez le code en surbrillance suivant pour gérer les événements de clic de bouton qui publiera les données à l’API Web à l’aide d’un appel HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="6ec19-329">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="6ec19-330">Dans **ContactController.cs**, placez un point d’arrêt sur la **Post** (méthode).</span><span class="sxs-lookup"><span data-stu-id="6ec19-330">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="6ec19-331">Appuyez sur **F5** pour exécuter l’application dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="6ec19-331">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="6ec19-332">Une fois que la page est chargée dans le navigateur, tapez un nouveau nom de contact et un Id et un clic le **enregistrer** bouton.</span><span class="sxs-lookup"><span data-stu-id="6ec19-332">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="6ec19-333">![Le document HTML clientes chargée dans le navigateur](build-restful-apis-with-aspnet-web-api/_static/image24.png "le document HTML clientes chargée dans le navigateur")</span><span class="sxs-lookup"><span data-stu-id="6ec19-333">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="6ec19-334">*Le document HTML client chargée dans le navigateur*</span><span class="sxs-lookup"><span data-stu-id="6ec19-334">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="6ec19-335">Lorsque la fenêtre du débogueur s’arrête le **Post** (méthode), jetez un œil aux propriétés de la **contacter** paramètre.</span><span class="sxs-lookup"><span data-stu-id="6ec19-335">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="6ec19-336">Les valeurs doivent correspondre les données que vous avez entré dans le formulaire.</span><span class="sxs-lookup"><span data-stu-id="6ec19-336">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="6ec19-337">![L’objet de Contact qui est envoyée à l’API Web depuis le client](build-restful-apis-with-aspnet-web-api/_static/image25.png "objet de Contact The qui est envoyé à l’API Web à partir du client")</span><span class="sxs-lookup"><span data-stu-id="6ec19-337">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="6ec19-338">*L’objet de Contact qui est envoyé à l’API Web à partir du client*</span><span class="sxs-lookup"><span data-stu-id="6ec19-338">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="6ec19-339">Étape via la méthode dans le débogueur jusqu'à ce que le **réponse** variable a été créée.</span><span class="sxs-lookup"><span data-stu-id="6ec19-339">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="6ec19-340">Lors de l’inspection dans la **variables locales** fenêtre du débogueur, vous verrez que toutes les propriétés ont été définies.</span><span class="sxs-lookup"><span data-stu-id="6ec19-340">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="6ec19-341">![La réponse après sa création dans le débogueur](build-restful-apis-with-aspnet-web-api/_static/image26.png "la réponse après sa création dans le débogueur")</span><span class="sxs-lookup"><span data-stu-id="6ec19-341">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="6ec19-342">*La réponse après sa création dans le débogueur*</span><span class="sxs-lookup"><span data-stu-id="6ec19-342">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="6ec19-343">Si vous appuyez sur **F5** ou cliquez sur **continuer** dans le débogueur se termine la demande.</span><span class="sxs-lookup"><span data-stu-id="6ec19-343">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="6ec19-344">Une fois que vous passez au navigateur, le nouveau contact a été ajouté à la liste des contacts stockés par le **ContactRepository** implémentation.</span><span class="sxs-lookup"><span data-stu-id="6ec19-344">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="6ec19-345">![Le navigateur reflète la création réussie de la nouvelle instance de contact](build-restful-apis-with-aspnet-web-api/_static/image27.png "le navigateur reflète la création réussie de la nouvelle instance de contact")</span><span class="sxs-lookup"><span data-stu-id="6ec19-345">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="6ec19-346">*Le navigateur reflète la création réussie de la nouvelle instance de contact*</span><span class="sxs-lookup"><span data-stu-id="6ec19-346">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="6ec19-347">En outre, vous pouvez déployer cette application à Azure suit [annexe c : Publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="6ec19-347">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>


* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="6ec19-348">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="6ec19-348">Summary</span></span>

<span data-ttu-id="6ec19-349">Ce laboratoire vous a présenté à la nouvelle infrastructure d’API Web ASP.NET et à l’implémentation de l’API Web RESTful à l’aide de l’infrastructure.</span><span class="sxs-lookup"><span data-stu-id="6ec19-349">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="6ec19-350">À ce stade, vous pouvez créer un nouveau référentiel qui facilite la persistance des données à l’aide d’un nombre quelconque de mécanismes et associer ce service plutôt que de simples celui fourni à titre d’exemple dans ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="6ec19-350">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="6ec19-351">API Web prend en charge un nombre de fonctionnalités supplémentaires, telles que l’activation des communications depuis les clients non-HTML écrites dans n’importe quel langage qui prend en charge HTTP et JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="6ec19-351">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="6ec19-352">La capacité d’héberger une API Web en dehors d’une application web typique est également possible, mais aussi est la possibilité de créer vos propres formats de sérialisation.</span><span class="sxs-lookup"><span data-stu-id="6ec19-352">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="6ec19-353">Le site Web ASP.NET a une zone dédiée à l’infrastructure d’API Web ASP.NET à [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="6ec19-353">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="6ec19-354">Ce site continuera de fournir aux dernières informations, des exemples et des informations sur les API Web, par conséquent, archivez-le fréquemment si vous souhaitez en savoir plus sur l’art de créer des API Web personnalisées disponibles pour n’importe quel framework de l’appareil ou de développement.</span><span class="sxs-lookup"><span data-stu-id="6ec19-354">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="6ec19-355">Annexe a : À l’aide d’extraits de Code</span><span class="sxs-lookup"><span data-stu-id="6ec19-355">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="6ec19-356">Avec des extraits de code, vous avez tout le code que vous avez besoin à portée de main.</span><span class="sxs-lookup"><span data-stu-id="6ec19-356">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="6ec19-357">Le document de laboratoire vous indiquera exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.</span><span class="sxs-lookup"><span data-stu-id="6ec19-357">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="6ec19-358">![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](build-restful-apis-with-aspnet-web-api/_static/image28.png "extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="6ec19-358">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="6ec19-359">*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="6ec19-359">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="6ec19-360">Pour ajouter un extrait de code à l’aide du clavier (C# uniquement)</span><span class="sxs-lookup"><span data-stu-id="6ec19-360">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="6ec19-361">Placez le curseur où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="6ec19-361">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="6ec19-362">Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).</span><span class="sxs-lookup"><span data-stu-id="6ec19-362">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="6ec19-363">Regarder en tant qu’IntelliSense affiche les noms des extraits correspondants.</span><span class="sxs-lookup"><span data-stu-id="6ec19-363">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="6ec19-364">Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entière est sélectionnée).</span><span class="sxs-lookup"><span data-stu-id="6ec19-364">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="6ec19-365">Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="6ec19-365">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="6ec19-366">![Commencez à taper le nom de l’extrait de code](build-restful-apis-with-aspnet-web-api/_static/image29.png "commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="6ec19-366">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="6ec19-367">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="6ec19-367">*Start typing the snippet name*</span></span>

    <span data-ttu-id="6ec19-368">![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](build-restful-apis-with-aspnet-web-api/_static/image30.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="6ec19-368">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="6ec19-369">*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="6ec19-369">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="6ec19-370">![Appuyez sur Tab à nouveau et l’extrait de code seront développe](build-restful-apis-with-aspnet-web-api/_static/image31.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")</span><span class="sxs-lookup"><span data-stu-id="6ec19-370">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="6ec19-371">*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*</span><span class="sxs-lookup"><span data-stu-id="6ec19-371">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="6ec19-372">Pour ajouter un extrait de code à l’aide de la souris (C#, Visual Basic et XML)</span><span class="sxs-lookup"><span data-stu-id="6ec19-372">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="6ec19-373">Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="6ec19-373">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="6ec19-374">Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-374">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="6ec19-375">Choisissez l’extrait de code approprié dans la liste, en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="6ec19-375">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="6ec19-376">![Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](build-restful-apis-with-aspnet-web-api/_static/image32.png "avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="6ec19-376">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="6ec19-377">*Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="6ec19-377">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="6ec19-378">![Choisissez l’extrait de code approprié dans la liste, en cliquant dessus](build-restful-apis-with-aspnet-web-api/_static/image33.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")</span><span class="sxs-lookup"><span data-stu-id="6ec19-378">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="6ec19-379">*Choisissez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*</span><span class="sxs-lookup"><span data-stu-id="6ec19-379">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="6ec19-380">Annexe b : Installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="6ec19-380">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="6ec19-381">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-381">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="6ec19-382">Les instructions suivantes vous guident dans les étapes requises pour installer *Visual studio Express 2012 pour Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="6ec19-382">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="6ec19-383">Accédez à [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="6ec19-383">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="6ec19-384">Si vous avez déjà installé Web Platform Installer, vous pouvez également ouvrir il et recherchez le produit &quot; <em>Visual Studio Express 2012 pour le Web avec le Kit de développement</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="6ec19-384">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="6ec19-385">Cliquez sur **installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-385">Click on **Install Now**.</span></span> <span data-ttu-id="6ec19-386">Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer en premier.</span><span class="sxs-lookup"><span data-stu-id="6ec19-386">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="6ec19-387">Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="6ec19-387">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="6ec19-388">![Installer Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="6ec19-388">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="6ec19-389">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="6ec19-389">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="6ec19-390">Lisez les licences et les termes du contrat de tous les produits et cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="6ec19-390">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accepter les termes du contrat de licence](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="6ec19-392">*Accepter les termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="6ec19-392">*Accepting the license terms*</span></span>
5. <span data-ttu-id="6ec19-393">Attendez que le processus de téléchargement et l’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="6ec19-393">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="6ec19-395">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="6ec19-395">*Installation progress*</span></span>
6. <span data-ttu-id="6ec19-396">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-396">When the installation completes, click **Finish**.</span></span>

    ![Installation est terminée](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="6ec19-398">*Installation est terminée*</span><span class="sxs-lookup"><span data-stu-id="6ec19-398">*Installation completed*</span></span>
7. <span data-ttu-id="6ec19-399">Cliquez sur **Exit** pour fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="6ec19-399">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="6ec19-400">Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.</span><span class="sxs-lookup"><span data-stu-id="6ec19-400">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pour une vignette de Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="6ec19-402">*VS Express pour une vignette de Web*</span><span class="sxs-lookup"><span data-stu-id="6ec19-402">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="6ec19-403">Annexe c : Publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="6ec19-403">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="6ec19-404">Cette annexe sera vous montrent comment créer un nouveau site web à partir du portail Azure et publiez l’application que vous avez obtenu en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Azure.</span><span class="sxs-lookup"><span data-stu-id="6ec19-404">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="6ec19-405">Tâche 1 : création d’un nouveau Site Web à partir du portail Azure</span><span class="sxs-lookup"><span data-stu-id="6ec19-405">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="6ec19-406">Accédez à la [portail de gestion](https://manage.windowsazure.com/) et connectez-vous en utilisant les informations d’identification Microsoft associées à votre abonnement.</span><span class="sxs-lookup"><span data-stu-id="6ec19-406">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6ec19-407">Avec Azure, vous pouvez héberger 10 Sites Web ASP.NET gratuitement et faites ensuite évoluer que votre trafic augmente.</span><span class="sxs-lookup"><span data-stu-id="6ec19-407">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="6ec19-408">Vous pouvez vous inscrire [ici](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="6ec19-408">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="6ec19-409">![Ouvrez une session sur le portail Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "ouvrez une session sur le portail Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="6ec19-409">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="6ec19-410">*Ouvrez une session sur le portail*</span><span class="sxs-lookup"><span data-stu-id="6ec19-410">*Log on to Portal*</span></span>
2. <span data-ttu-id="6ec19-411">Cliquez sur **New** sur la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="6ec19-411">Click **New** on the command bar.</span></span>

    <span data-ttu-id="6ec19-412">![Création d’un Site Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "création d’un Site Web")</span><span class="sxs-lookup"><span data-stu-id="6ec19-412">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="6ec19-413">*Création d’un Site Web*</span><span class="sxs-lookup"><span data-stu-id="6ec19-413">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="6ec19-414">Cliquez sur **calcul** | **Site Web**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-414">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="6ec19-415">Puis sélectionnez **création rapide** option.</span><span class="sxs-lookup"><span data-stu-id="6ec19-415">Then select **Quick Create** option.</span></span> <span data-ttu-id="6ec19-416">Fournir une URL disponible pour le nouveau site web et cliquez sur **créer un Site Web**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-416">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6ec19-417">Azure est l’hôte pour une application web en cours d’exécution dans le cloud que vous pouvez contrôler et gérer.</span><span class="sxs-lookup"><span data-stu-id="6ec19-417">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="6ec19-418">L’option Création rapide vous permet de déployer une application web terminée sur Azure à partir en dehors du portail.</span><span class="sxs-lookup"><span data-stu-id="6ec19-418">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="6ec19-419">Il n’inclut pas les étapes de configuration d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="6ec19-419">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="6ec19-420">![Création d’un Site Web à l’aide de la création rapide](build-restful-apis-with-aspnet-web-api/_static/image41.png "création d’un Site Web à l’aide de la création rapide")</span><span class="sxs-lookup"><span data-stu-id="6ec19-420">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="6ec19-421">*Création d’un Site Web à l’aide de la création rapide*</span><span class="sxs-lookup"><span data-stu-id="6ec19-421">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="6ec19-422">Attendez que la nouvelle **Site Web** est créé.</span><span class="sxs-lookup"><span data-stu-id="6ec19-422">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="6ec19-423">Une fois que le Site Web est créé, cliquez sur le lien situé sous le **URL** colonne.</span><span class="sxs-lookup"><span data-stu-id="6ec19-423">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="6ec19-424">Vérifiez que le nouveau Site Web fonctionne.</span><span class="sxs-lookup"><span data-stu-id="6ec19-424">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="6ec19-425">![Navigation vers le nouveau site web](build-restful-apis-with-aspnet-web-api/_static/image42.png "exploration vers le nouveau site web")</span><span class="sxs-lookup"><span data-stu-id="6ec19-425">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="6ec19-426">*Navigation vers le nouveau site web*</span><span class="sxs-lookup"><span data-stu-id="6ec19-426">*Browsing to the new web site*</span></span>

    <span data-ttu-id="6ec19-427">![Site Web en cours d’exécution](build-restful-apis-with-aspnet-web-api/_static/image43.png "site Web en cours d’exécution")</span><span class="sxs-lookup"><span data-stu-id="6ec19-427">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="6ec19-428">*Site Web en cours d’exécution*</span><span class="sxs-lookup"><span data-stu-id="6ec19-428">*Web site running*</span></span>
6. <span data-ttu-id="6ec19-429">Revenez au portail et cliquez sur le nom du site web sous le **nom** colonne pour afficher les pages de gestion.</span><span class="sxs-lookup"><span data-stu-id="6ec19-429">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="6ec19-430">![Ouvrir les pages de gestion de site web](build-restful-apis-with-aspnet-web-api/_static/image44.png "ouvrir les pages de gestion de site web")</span><span class="sxs-lookup"><span data-stu-id="6ec19-430">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="6ec19-431">*Ouvrir les pages de gestion de Site Web*</span><span class="sxs-lookup"><span data-stu-id="6ec19-431">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="6ec19-432">Dans le **tableau de bord** page, sous la **aperçu rapide** , cliquez sur le **télécharger le profil de publication** lien.</span><span class="sxs-lookup"><span data-stu-id="6ec19-432">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6ec19-433">Le *le profil de publication* contient toutes les informations requises pour publier une application web à Azure pour chaque méthode de publication est activée.</span><span class="sxs-lookup"><span data-stu-id="6ec19-433">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="6ec19-434">Le profil de publication contient les URL, les informations d’identification utilisateur et les chaînes de base de données requis pour la connexion et l’authentification par rapport à chacun des points de terminaison pour lequel une méthode de publication est activée.</span><span class="sxs-lookup"><span data-stu-id="6ec19-434">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="6ec19-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour Web** et **Microsoft Visual Studio 2012** prise en charge de la lecture des profils pour automatiser la configuration de ces programmes pour de publication publication d’applications web sur Azure.</span><span class="sxs-lookup"><span data-stu-id="6ec19-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="6ec19-436">![Téléchargement du site web le profil de publication](build-restful-apis-with-aspnet-web-api/_static/image45.png "téléchargement du site web le profil de publication")</span><span class="sxs-lookup"><span data-stu-id="6ec19-436">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="6ec19-437">*Téléchargement du Site Web le profil de publication*</span><span class="sxs-lookup"><span data-stu-id="6ec19-437">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="6ec19-438">Télécharger le fichier de profil de publication dans un emplacement connu.</span><span class="sxs-lookup"><span data-stu-id="6ec19-438">Download the publish profile file to a known location.</span></span> <span data-ttu-id="6ec19-439">Davantage dans cet exercice, vous verrez comment utiliser ce fichier pour publier une application web vers Azure à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6ec19-439">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="6ec19-440">![L’enregistrement du fichier de profil de publication](build-restful-apis-with-aspnet-web-api/_static/image46.png "l’enregistrement du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="6ec19-440">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="6ec19-441">*L’enregistrement du fichier de profil de publication*</span><span class="sxs-lookup"><span data-stu-id="6ec19-441">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="6ec19-442">Tâche 2 : configuration du serveur de base de données</span><span class="sxs-lookup"><span data-stu-id="6ec19-442">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="6ec19-443">Si votre application se sert de SQL Server vous devez créer un serveur de base de données SQL des bases de données.</span><span class="sxs-lookup"><span data-stu-id="6ec19-443">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="6ec19-444">Si vous souhaitez déployer une application simple qui n’utilise pas de SQL Server, vous pouvez ignorer cette tâche.</span><span class="sxs-lookup"><span data-stu-id="6ec19-444">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="6ec19-445">Vous devez un serveur de base de données SQL pour stocker la base de données d’application.</span><span class="sxs-lookup"><span data-stu-id="6ec19-445">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="6ec19-446">Vous pouvez afficher les serveurs de base de données SQL à partir de votre abonnement dans le portail de gestion Azure à **bases de données Sql** | **serveurs** | **tableau de bord du serveur**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-446">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="6ec19-447">Si vous n’avez pas un serveur créé, vous pouvez créer un à l’aide du **ajouter** bouton sur la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="6ec19-447">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="6ec19-448">Prenez note de la **nom du serveur et les URL, nom de connexion d’administrateur et mot de passe**, car vous les utiliserez dans les tâches suivantes.</span><span class="sxs-lookup"><span data-stu-id="6ec19-448">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="6ec19-449">Ne créez pas encore, la base de données tel qu’il sera créé dans une étape ultérieure.</span><span class="sxs-lookup"><span data-stu-id="6ec19-449">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="6ec19-450">![Tableau de bord de serveur SQL Database](build-restful-apis-with-aspnet-web-api/_static/image47.png "tableau de bord serveur de base de données SQL")</span><span class="sxs-lookup"><span data-stu-id="6ec19-450">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="6ec19-451">*Tableau de bord serveur de base de données SQL*</span><span class="sxs-lookup"><span data-stu-id="6ec19-451">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="6ec19-452">Dans la tâche suivante, vous allez tester la connexion de base de données à partir de Visual Studio, pour cette raison, vous devez inclure votre adresse IP locale dans la liste du serveur de **adresses IP autorisées**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-452">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="6ec19-453">Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de **adresse IP cliente actuelle** et collez-le sur le **adresse IP de début** et **adresse IP de fin** zones de texte et cliquez sur le ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) bouton.</span><span class="sxs-lookup"><span data-stu-id="6ec19-453">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Ajout d’adresse IP du Client](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="6ec19-455">*Ajout d’adresse IP du Client*</span><span class="sxs-lookup"><span data-stu-id="6ec19-455">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="6ec19-456">Une fois le **adresse IP du Client** est ajouté à le des adresses IP autorisées de liste, cliquez sur **enregistrer** pour confirmer les modifications.</span><span class="sxs-lookup"><span data-stu-id="6ec19-456">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmer les modifications](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="6ec19-458">*Confirmer les modifications*</span><span class="sxs-lookup"><span data-stu-id="6ec19-458">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="6ec19-459">Tâche 3 : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="6ec19-459">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="6ec19-460">Revenez à la solution ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="6ec19-460">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="6ec19-461">Dans le **l’Explorateur de solutions**, cliquez sur le projet de site web et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-461">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="6ec19-462">![Publication de l’Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "publication de l’Application")</span><span class="sxs-lookup"><span data-stu-id="6ec19-462">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="6ec19-463">*Publier le site web*</span><span class="sxs-lookup"><span data-stu-id="6ec19-463">*Publishing the web site*</span></span>
2. <span data-ttu-id="6ec19-464">Importer le profil de publication que vous avez enregistré dans la première tâche.</span><span class="sxs-lookup"><span data-stu-id="6ec19-464">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="6ec19-465">![L’importation du profil de publication](build-restful-apis-with-aspnet-web-api/_static/image52.png "l’importation du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="6ec19-465">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="6ec19-466">*Importation du profil de publication*</span><span class="sxs-lookup"><span data-stu-id="6ec19-466">*Importing publish profile*</span></span>
3. <span data-ttu-id="6ec19-467">Cliquez sur **valider la connexion**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-467">Click **Validate Connection**.</span></span> <span data-ttu-id="6ec19-468">Une fois la Validation terminée. Cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-468">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6ec19-469">La validation est terminée une fois que vous voyez une coche verte apparaît en regard du bouton Valider la connexion.</span><span class="sxs-lookup"><span data-stu-id="6ec19-469">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="6ec19-470">![Validation de la connexion](build-restful-apis-with-aspnet-web-api/_static/image53.png "validation de la connexion")</span><span class="sxs-lookup"><span data-stu-id="6ec19-470">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="6ec19-471">*Validation de la connexion*</span><span class="sxs-lookup"><span data-stu-id="6ec19-471">*Validating connection*</span></span>
4. <span data-ttu-id="6ec19-472">Dans le **paramètres** page, sous la **bases de données** , cliquez sur le bouton en regard de la zone de texte de la connexion de votre base de données (par exemple, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="6ec19-472">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="6ec19-473">![Configuration de déploiement Web](build-restful-apis-with-aspnet-web-api/_static/image54.png "configuration de déploiement Web")</span><span class="sxs-lookup"><span data-stu-id="6ec19-473">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="6ec19-474">*Configuration de déploiement Web*</span><span class="sxs-lookup"><span data-stu-id="6ec19-474">*Web deploy configuration*</span></span>
5. <span data-ttu-id="6ec19-475">Configurez la connexion de base de données comme suit :</span><span class="sxs-lookup"><span data-stu-id="6ec19-475">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="6ec19-476">Dans le **nom du serveur** tapez votre URL de base de données SQL server à l’aide du *tcp :* préfixe.</span><span class="sxs-lookup"><span data-stu-id="6ec19-476">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="6ec19-477">Dans **nom d’utilisateur** tapez votre nom de connexion d’administrateur serveur.</span><span class="sxs-lookup"><span data-stu-id="6ec19-477">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="6ec19-478">Dans **mot de passe** tapez votre mot de passe du compte de connexion administrateur serveur.</span><span class="sxs-lookup"><span data-stu-id="6ec19-478">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="6ec19-479">Tapez un nouveau nom de base de données, par exemple : *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="6ec19-479">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="6ec19-480">![Configuration de chaîne de connexion de destination](build-restful-apis-with-aspnet-web-api/_static/image55.png "configuration de chaîne de connexion de destination")</span><span class="sxs-lookup"><span data-stu-id="6ec19-480">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="6ec19-481">*Configuration de chaîne de connexion de destination*</span><span class="sxs-lookup"><span data-stu-id="6ec19-481">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="6ec19-482">Cliquez ensuite sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-482">Then click **OK**.</span></span> <span data-ttu-id="6ec19-483">Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-483">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="6ec19-484">![Création de la base de données](build-restful-apis-with-aspnet-web-api/_static/image56.png "création de la chaîne de la base de données")</span><span class="sxs-lookup"><span data-stu-id="6ec19-484">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="6ec19-485">*Création de la base de données*</span><span class="sxs-lookup"><span data-stu-id="6ec19-485">*Creating the database*</span></span>
7. <span data-ttu-id="6ec19-486">La chaîne de connexion que vous utiliserez pour vous connecter à la base de données SQL dans Windows Azure est indiquée dans la zone de texte de la connexion par défaut.</span><span class="sxs-lookup"><span data-stu-id="6ec19-486">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="6ec19-487">Cliquez ensuite sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-487">Then click **Next**.</span></span>

    <span data-ttu-id="6ec19-488">![Chaîne de connexion pointant vers la base de données SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "chaîne de connexion pointant vers la base de données SQL")</span><span class="sxs-lookup"><span data-stu-id="6ec19-488">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="6ec19-489">*Chaîne de connexion pointant vers la base de données SQL*</span><span class="sxs-lookup"><span data-stu-id="6ec19-489">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="6ec19-490">Dans le **aperçu** , cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="6ec19-490">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="6ec19-491">![Publication de l’application web](build-restful-apis-with-aspnet-web-api/_static/image58.png "publication de l’application web")</span><span class="sxs-lookup"><span data-stu-id="6ec19-491">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="6ec19-492">*Publication de l’application web*</span><span class="sxs-lookup"><span data-stu-id="6ec19-492">*Publishing the web application*</span></span>
9. <span data-ttu-id="6ec19-493">Une fois le processus de publication terminé, votre navigateur par défaut s’ouvre le site web publié.</span><span class="sxs-lookup"><span data-stu-id="6ec19-493">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="6ec19-494">![Application publiée dans Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application publiée dans Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="6ec19-494">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="6ec19-495">*Application publiée sur Azure*</span><span class="sxs-lookup"><span data-stu-id="6ec19-495">*Application published to Azure*</span></span>
