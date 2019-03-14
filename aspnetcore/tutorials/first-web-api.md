---
title: 'Tutoriel : Créer une API web avec ASP.NET Core MVC'
author: rick-anderson
description: Générer une API web avec ASP.NET Core MVC
ms.author: riande
ms.custom: mvc
ms.date: 02/4/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 686397cd25248ce7b37e505c7129a3b56d4ada1b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57045776"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core-mvc"></a><span data-ttu-id="0d2bc-103">Tutoriel : Créer une API web avec ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="0d2bc-103">Tutorial: Create a web API with ASP.NET Core MVC</span></span>

<span data-ttu-id="0d2bc-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="0d2bc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="0d2bc-105">Ce tutoriel décrit les principes fondamentaux liés à la génération d’une API web avec ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

<span data-ttu-id="0d2bc-106">Dans ce didacticiel, vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0d2bc-107">Créer un projet d’API web.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-107">Create a web API project.</span></span>
> * <span data-ttu-id="0d2bc-108">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="0d2bc-108">Add a model class.</span></span>
> * <span data-ttu-id="0d2bc-109">Créer le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="0d2bc-109">Create the database context.</span></span>
> * <span data-ttu-id="0d2bc-110">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="0d2bc-110">Register the database context.</span></span>
> * <span data-ttu-id="0d2bc-111">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="0d2bc-111">Add a controller.</span></span>
> * <span data-ttu-id="0d2bc-112">Ajouter les méthodes CRUD</span><span class="sxs-lookup"><span data-stu-id="0d2bc-112">Add CRUD methods.</span></span>
> * <span data-ttu-id="0d2bc-113">Configurer le routage et les chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="0d2bc-113">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="0d2bc-114">Spécifier des valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="0d2bc-114">Specify return values.</span></span>
> * <span data-ttu-id="0d2bc-115">Appeler l’API web avec Postman</span><span class="sxs-lookup"><span data-stu-id="0d2bc-115">Call the web API with Postman.</span></span>
> * <span data-ttu-id="0d2bc-116">Appeler l’API web avec jQuery.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-116">Call the web API with jQuery.</span></span>

<span data-ttu-id="0d2bc-117">À la fin, vous disposez d’une API web qui peut gérer des tâches stockées dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-117">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="0d2bc-118">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="0d2bc-118">Overview</span></span>

<span data-ttu-id="0d2bc-119">Ce didacticiel crée l’API suivante :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-119">This tutorial creates the following API:</span></span>

|<span data-ttu-id="0d2bc-120">API</span><span class="sxs-lookup"><span data-stu-id="0d2bc-120">API</span></span> | <span data-ttu-id="0d2bc-121">Description</span><span class="sxs-lookup"><span data-stu-id="0d2bc-121">Description</span></span> | <span data-ttu-id="0d2bc-122">Corps de la requête</span><span class="sxs-lookup"><span data-stu-id="0d2bc-122">Request body</span></span> | <span data-ttu-id="0d2bc-123">Corps de réponse</span><span class="sxs-lookup"><span data-stu-id="0d2bc-123">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="0d2bc-124">GET /api/todo</span><span class="sxs-lookup"><span data-stu-id="0d2bc-124">GET /api/todo</span></span> | <span data-ttu-id="0d2bc-125">Obtenir toutes les tâches</span><span class="sxs-lookup"><span data-stu-id="0d2bc-125">Get all to-do items</span></span> | <span data-ttu-id="0d2bc-126">Aucun.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-126">None</span></span> | <span data-ttu-id="0d2bc-127">Tableau de tâches</span><span class="sxs-lookup"><span data-stu-id="0d2bc-127">Array of to-do items</span></span>|
|<span data-ttu-id="0d2bc-128">GET /api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="0d2bc-128">GET /api/todo/{id}</span></span> | <span data-ttu-id="0d2bc-129">Obtenir un élément par ID</span><span class="sxs-lookup"><span data-stu-id="0d2bc-129">Get an item by ID</span></span> | <span data-ttu-id="0d2bc-130">Aucun.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-130">None</span></span> | <span data-ttu-id="0d2bc-131">Tâche</span><span class="sxs-lookup"><span data-stu-id="0d2bc-131">To-do item</span></span>|
|<span data-ttu-id="0d2bc-132">POST /api/todo</span><span class="sxs-lookup"><span data-stu-id="0d2bc-132">POST /api/todo</span></span> | <span data-ttu-id="0d2bc-133">Ajouter un nouvel élément</span><span class="sxs-lookup"><span data-stu-id="0d2bc-133">Add a new item</span></span> | <span data-ttu-id="0d2bc-134">Tâche</span><span class="sxs-lookup"><span data-stu-id="0d2bc-134">To-do item</span></span> | <span data-ttu-id="0d2bc-135">Tâche</span><span class="sxs-lookup"><span data-stu-id="0d2bc-135">To-do item</span></span> |
|<span data-ttu-id="0d2bc-136">PUT /api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="0d2bc-136">PUT /api/todo/{id}</span></span> | <span data-ttu-id="0d2bc-137">Mettre à jour un élément existant &nbsp;</span><span class="sxs-lookup"><span data-stu-id="0d2bc-137">Update an existing item &nbsp;</span></span> | <span data-ttu-id="0d2bc-138">Tâche</span><span class="sxs-lookup"><span data-stu-id="0d2bc-138">To-do item</span></span> | <span data-ttu-id="0d2bc-139">Aucun.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-139">None</span></span> |
|<span data-ttu-id="0d2bc-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="0d2bc-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="0d2bc-141">Supprimer un élément &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="0d2bc-141">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="0d2bc-142">Aucun.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-142">None</span></span> | <span data-ttu-id="0d2bc-143">Aucun.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-143">None</span></span>|

<span data-ttu-id="0d2bc-144">Le diagramme suivant illustre la conception de l’application.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-144">The following diagram shows the design of the app.</span></span>

![Le client, représenté par une zone située à gauche, soumet une requête et reçoit une réponse de l’application, représentée par une zone dessinée sur la droite.](first-web-api/_static/architecture.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="0d2bc-149">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="0d2bc-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0d2bc-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d2bc-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0d2bc-151">Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="0d2bc-152">Sélectionnez le modèle **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-152">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="0d2bc-153">Nommez le projet *TodoApi*, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-153">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="0d2bc-154">Dans la boîte de dialogue **Nouvelle application web ASP.NET Core - TodoApi**, choisissez la version ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-154">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="0d2bc-155">Sélectionnez le modèle **API** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-155">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="0d2bc-156">Ne sélectionnez **pas** **Activer la prise en charge de Docker**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-156">Do **not** select **Enable Docker Support**.</span></span>

![Boîte de dialogue de nouveau projet dans VS](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0d2bc-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0d2bc-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0d2bc-159">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="0d2bc-160">Définissez les répertoires (`cd`) sur le dossier destiné à contenir le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-160">Change directories (`cd`) to the folder which will contain the project folder.</span></span>
* <span data-ttu-id="0d2bc-161">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="0d2bc-162">Ces commandes créent un projet d’API web et ouvrent une nouvelle instance de Visual Studio Code dans le nouveau dossier de projet.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-162">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="0d2bc-163">Quand une boîte de dialogue vous demande si vous souhaitez ajouter les composants nécessaires au projet, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-163">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0d2bc-164">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="0d2bc-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0d2bc-165">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-165">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="0d2bc-167">Sélectionnez **Application .NET Core** > **API web ASP.NET Core** > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-167">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="0d2bc-169">Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, acceptez la valeur par défaut \**.NET Core 2.2* pour **Framework cible**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-169">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="0d2bc-170">Entrez *TodoApi* comme **Nom du projet**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-170">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![boîte de dialogue de configuration](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="0d2bc-172">Tester l’API</span><span class="sxs-lookup"><span data-stu-id="0d2bc-172">Test the API</span></span>

<span data-ttu-id="0d2bc-173">Le modèle de projet crée une API `values`.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-173">The project template creates a `values` API.</span></span> <span data-ttu-id="0d2bc-174">Appelez la méthode `Get` à partir d’un navigateur pour tester l’application.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-174">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0d2bc-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d2bc-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0d2bc-176">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-176">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="0d2bc-177">Visual Studio lance un navigateur et accède à `https://localhost:<port>/api/values`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-177">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="0d2bc-178">Si une boîte de dialogue apparaît vous demandant si vous devez approuver le certificat IIS Express, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-178">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="0d2bc-179">Dans la boîte de dialogue **Avertissement de sécurité** qui s’affiche ensuite, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-179">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0d2bc-180">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0d2bc-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0d2bc-181">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-181">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="0d2bc-182">Dans un navigateur, accédez à l’URL suivante : [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-182">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0d2bc-183">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="0d2bc-183">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0d2bc-184">Sélectionnez **Exécuter** > **Démarrer avec débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-184">Select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="0d2bc-185">Visual Studio pour Mac lance un navigateur et accède à `https://localhost:<port>`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-185">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="0d2bc-186">Une erreur HTTP 404 (introuvable) est retournée.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-186">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="0d2bc-187">Ajoutez `/api/values` à l’URL (définissez-la sur `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-187">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="0d2bc-188">Le code JSON suivant est retourné :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-188">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="0d2bc-189">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="0d2bc-189">Add a model class</span></span>

<span data-ttu-id="0d2bc-190">Un *modèle* est un ensemble de classes qui représentent les données gérées par l’application.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-190">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="0d2bc-191">Le modèle pour cette application est une classe `TodoItem` unique.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-191">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0d2bc-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d2bc-192">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0d2bc-193">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-193">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="0d2bc-194">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-194">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="0d2bc-195">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-195">Name the folder *Models*.</span></span>

* <span data-ttu-id="0d2bc-196">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-196">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="0d2bc-197">Nommez la classe *TodoItem* et sélectionnez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-197">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="0d2bc-198">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-198">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0d2bc-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0d2bc-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0d2bc-200">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-200">Add a folder named *Models*.</span></span>

* <span data-ttu-id="0d2bc-201">Ajoutez une classe `TodoItem` au dossier *Modèles* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-201">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0d2bc-202">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="0d2bc-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0d2bc-203">Cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-203">Right-click the project.</span></span> <span data-ttu-id="0d2bc-204">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-204">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="0d2bc-205">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-205">Name the folder *Models*.</span></span>

  ![nouveau dossier](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="0d2bc-207">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Nouveau fichier** > **Général** > **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-207">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="0d2bc-208">Nommez la classe *TodoItem* et cliquez sur **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-208">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="0d2bc-209">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-209">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="0d2bc-210">La propriété `Id` fonctionne comme la clé unique dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-210">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="0d2bc-211">Vous pouvez placer des classes de modèle n’importe où dans le projet, mais le dossier *Models* est utilisé par convention.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-211">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="0d2bc-212">Ajouter un contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="0d2bc-212">Add a database context</span></span>

<span data-ttu-id="0d2bc-213">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-213">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="0d2bc-214">Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-214">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0d2bc-215">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d2bc-215">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0d2bc-216">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-216">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="0d2bc-217">Nommez la classe *TodoContext* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-217">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0d2bc-218">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="0d2bc-218">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="0d2bc-219">Ajoutez une classe `TodoContext` au dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-219">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="0d2bc-220">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-220">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="0d2bc-221">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="0d2bc-221">Register the database context</span></span>

<span data-ttu-id="0d2bc-222">Dans ASP.NET Core, les services tels que le contexte de base de données doivent être inscrits auprès du [conteneur d’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-222">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="0d2bc-223">Le conteneur fournit le service aux contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-223">The container provides the service to controllers.</span></span>

<span data-ttu-id="0d2bc-224">Mettez à jour *Startup.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-224">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="0d2bc-225">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-225">The preceding code:</span></span>

* <span data-ttu-id="0d2bc-226">Supprime les déclarations `using` inutilisées.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-226">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="0d2bc-227">Ajoute le contexte de base de données au conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-227">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="0d2bc-228">Spécifie que le contexte de base de données utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-228">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="0d2bc-229">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="0d2bc-229">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0d2bc-230">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d2bc-230">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0d2bc-231">Cliquez avec le bouton droit sur le dossier *Contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-231">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="0d2bc-232">Sélectionnez **Ajouter** > **Nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-232">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="0d2bc-233">Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez le modèle **Classe de contrôleur d’API**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-233">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="0d2bc-234">Nommez la classe *TodoController* et sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-234">Name the class *TodoController*, and select **Add**.</span></span>

  ![Boîte de dialogue Ajouter un nouvel élément avec contrôleur dans la zone de recherche et le contrôleur des API web sélectionné](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0d2bc-236">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="0d2bc-236">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="0d2bc-237">Dans le dossier *Contrôleurs*, créez une classe nommée `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-237">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="0d2bc-238">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-238">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="0d2bc-239">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-239">The preceding code:</span></span>

* <span data-ttu-id="0d2bc-240">Définit une classe de contrôleur d’API sans méthodes.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-240">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="0d2bc-241">Décorez la classe avec l’attribut [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-241">Decorates the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="0d2bc-242">Cet attribut indique que le contrôleur répond aux requêtes de l’API web.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-242">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="0d2bc-243">Pour plus d’informations sur les comportements spécifiques que permet l’attribut, consultez [Annotation avec attribut ApiController](xref:web-api/index#annotation-with-apicontroller-attribute).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-243">For information about specific behaviors that the attribute enables, see [Annotation with ApiController attribute](xref:web-api/index#annotation-with-apicontroller-attribute).</span></span>
* <span data-ttu-id="0d2bc-244">Utilise l’injection de dépendances pour injecter le contexte de base de données (`TodoContext`) dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-244">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="0d2bc-245">Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-245">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="0d2bc-246">Ajoute un élément nommé `Item1` à la base de données si celle-ci est vide.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-246">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="0d2bc-247">Ce code se trouvant dans le constructeur, il s’exécute chaque fois qu’une nouvelle requête HTTP existe.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-247">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="0d2bc-248">Si vous supprimez tous les éléments, le constructeur recrée `Item1` au prochain appel d’une méthode d’API.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-248">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="0d2bc-249">Ainsi, il peut vous sembler à tort que la suppression n’a pas fonctionné.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-249">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="0d2bc-250">Ajouter des méthodes Get</span><span class="sxs-lookup"><span data-stu-id="0d2bc-250">Add Get methods</span></span>

<span data-ttu-id="0d2bc-251">Pour fournir une API qui récupère les tâches, ajoutez les méthodes suivantes à la classe `TodoController` :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-251">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="0d2bc-252">Ces méthodes implémentent deux points de terminaison GET :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-252">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="0d2bc-253">Testez l’application en appelant les deux points de terminaison à partir d’un navigateur.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-253">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="0d2bc-254">Exemple :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-254">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="0d2bc-255">La réponse HTTP suivante est générée par l’appel à `GetTodoItems` :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-255">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="0d2bc-256">Routage et chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="0d2bc-256">Routing and URL paths</span></span>

<span data-ttu-id="0d2bc-257">L’attribut [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) désigne une méthode qui répond à une requête HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-257">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="0d2bc-258">Le chemin d’URL pour chaque méthode est construit comme suit :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-258">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="0d2bc-259">Partez de la chaîne de modèle dans l’attribut `Route` du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-259">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="0d2bc-260">Remplacez `[controller]` par le nom du contrôleur qui, par convention, est le nom de la classe du contrôleur sans le suffixe « Controller ».</span><span class="sxs-lookup"><span data-stu-id="0d2bc-260">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="0d2bc-261">Pour cet exemple, le nom de classe du contrôleur étant **Todo**Controller, le nom du contrôleur est « todo ».</span><span class="sxs-lookup"><span data-stu-id="0d2bc-261">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="0d2bc-262">Le [routage](xref:mvc/controllers/routing) d’ASP.NET Core ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-262">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="0d2bc-263">Si l’attribut `[HttpGet]` a un modèle de route (par exemple, `[HttpGet("products")]`), ajoutez-le au chemin.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-263">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="0d2bc-264">Cet exemple n’utilise pas de modèle.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-264">This sample doesn't use a template.</span></span> <span data-ttu-id="0d2bc-265">Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-265">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="0d2bc-266">Dans la méthode `GetTodoItem` suivante, `"{id}"` est une variable d’espace réservé pour l’identificateur unique de la tâche.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-266">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="0d2bc-267">Quand `GetTodoItem` est appelée, la valeur de `"{id}"` dans l’URL est fournie à la méthode dans son paramètre `id`.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-267">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="0d2bc-268">Valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="0d2bc-268">Return values</span></span>

<span data-ttu-id="0d2bc-269">Le type de retour des méthodes `GetTodoItems` et `GetTodoItem` est [type ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-269">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="0d2bc-270">ASP.NET Core sérialise automatiquement l’objet en [JSON](https://www.json.org/) et écrit le JSON dans le corps du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-270">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="0d2bc-271">Le code de réponse pour ce type de retour est 200, en supposant qu’il n’existe pas d’exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-271">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="0d2bc-272">Les exceptions non gérées sont converties en erreurs 5xx.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-272">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="0d2bc-273">Les types de retour `ActionResult` peuvent représenter une large plage de codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-273">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="0d2bc-274">Par exemple, `GetTodoItem` peut retourner deux valeurs d’état différentes :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-274">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="0d2bc-275">Si aucun élément ne correspond à l’ID demandé, la méthode retourne un code d’erreur 404 [introuvable](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-275">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="0d2bc-276">Sinon, la méthode retourne 200 avec un corps de réponse JSON.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-276">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="0d2bc-277">Le retour de `item` entraîne une réponse HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-277">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="0d2bc-278">Tester la méthode GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="0d2bc-278">Test the GetTodoItems method</span></span>

<span data-ttu-id="0d2bc-279">Ce tutoriel utilise Postman pour tester l’API web.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-279">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="0d2bc-280">Installez [Postman](https://www.getpostman.com/apps).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-280">Install [Postman](https://www.getpostman.com/apps)</span></span>
* <span data-ttu-id="0d2bc-281">Démarrez l’application web.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-281">Start the web app.</span></span>
* <span data-ttu-id="0d2bc-282">Démarrez Postman.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-282">Start Postman.</span></span>
* <span data-ttu-id="0d2bc-283">Désactivez la **vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-283">Disable **SSL certificate verification**</span></span>
  
  * <span data-ttu-id="0d2bc-284">À partir de **Fichier > Paramètres** (onglet \**Général*), désactivez **Vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-284">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="0d2bc-285">Réactivez la vérification du certificat SSL après avoir testé le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-285">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="0d2bc-286">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-286">Create a new request.</span></span>
  * <span data-ttu-id="0d2bc-287">Définissez la méthode HTTP sur **GET**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-287">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="0d2bc-288">Définissez l’URL de la requête sur `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-288">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="0d2bc-289">Par exemple, `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-289">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="0d2bc-290">Définissez l’**affichage à deux volets** dans Postman.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-290">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="0d2bc-291">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-291">Select **Send**.</span></span>

![Postman avec requête Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="0d2bc-293">Ajouter une méthode Create</span><span class="sxs-lookup"><span data-stu-id="0d2bc-293">Add a Create method</span></span>

<span data-ttu-id="0d2bc-294">Ajoutez la méthode `PostTodoItem` suivante :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-294">Add the following `PostTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="0d2bc-295">Le code précédent est une méthode HTTP POST, comme indiqué par l’attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-295">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="0d2bc-296">La méthode obtient la valeur de la tâche dans le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-296">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="0d2bc-297">La méthode `CreatedAtAction` :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-297">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="0d2bc-298">Retourne un code d’état HTTP 201 en cas de réussite.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-298">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="0d2bc-299">HTTP 201 est la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-299">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="0d2bc-300">Ajoute un en-tête `Location` à la réponse.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-300">Adds a `Location` header to the response.</span></span> <span data-ttu-id="0d2bc-301">L’en-tête `Location` spécifie l’URI de l’élément d’action qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-301">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="0d2bc-302">Pour plus d’informations, consultez la section [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-302">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="0d2bc-303">Fait référence à l’action `GetTodoItem` pour créer l’URI `Location` de l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-303">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="0d2bc-304">Le mot clé `nameof` C# est utilisé pour éviter de coder en dur le nom de l’action dans l’appel `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-304">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="0d2bc-305">Tester la méthode PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="0d2bc-305">Test the PostTodoItem method</span></span>

* <span data-ttu-id="0d2bc-306">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-306">Build the project.</span></span>
* <span data-ttu-id="0d2bc-307">Dans Postman, définissez la méthode HTTP sur `POST`.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-307">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="0d2bc-308">Sélectionnez l’onglet **Body** (Corps).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-308">Select the **Body** tab.</span></span>
* <span data-ttu-id="0d2bc-309">Sélectionnez la case d’option **raw** (données brutes).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-309">Select the **raw** radio button.</span></span>
* <span data-ttu-id="0d2bc-310">Définissez le type sur **JSON (application/json)**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-310">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="0d2bc-311">Dans le corps de la demande, entrez la syntaxe JSON d’une tâche :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-311">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="0d2bc-312">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-312">Select **Send**.</span></span>

  ![Postman avec requête de création](first-web-api/_static/create.png)

  <span data-ttu-id="0d2bc-314">Si vous obtenez une erreur 405 Méthode non autorisée, il est probable que le projet n’ait pas été compilé après l’ajout de la méthode `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-314">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="0d2bc-315">Tester l’URI de l’en-tête d’emplacement</span><span class="sxs-lookup"><span data-stu-id="0d2bc-315">Test the location header URI</span></span>

* <span data-ttu-id="0d2bc-316">Sélectionnez l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-316">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="0d2bc-317">Copiez la valeur d’en-tête **Location** (Emplacement) :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-317">Copy the **Location** header value:</span></span>

  ![Onglet Headers de la console Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="0d2bc-319">Définissez la méthode sur GET.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-319">Set the method to GET.</span></span>
* <span data-ttu-id="0d2bc-320">Collez l’URI (par exemple, `https://localhost:5001/api/Todo/2`).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-320">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="0d2bc-321">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-321">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="0d2bc-322">Ajouter une méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="0d2bc-322">Add a PutTodoItem method</span></span>

<span data-ttu-id="0d2bc-323">Ajoutez la méthode `PutTodoItem` suivante :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-323">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="0d2bc-324">`PutTodoItem` est similaire à `PostTodoItem`, à la différence près qu’il utilise HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-324">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="0d2bc-325">La réponse est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-325">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="0d2bc-326">D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les changements.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-326">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="0d2bc-327">Pour prendre en charge les mises à jour partielles, utilisez [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-327">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="0d2bc-328">Si vous obtenez une erreur en appelant `PutTodoItem`, appelez `GET` pour vérifier que la base de données contient un élément.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-328">I you get an error calling `PutTodoItem`, call `GET` to ensure there is a an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="0d2bc-329">Tester la méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="0d2bc-329">Test the PutTodoItem method</span></span>

<span data-ttu-id="0d2bc-330">Cet exemple utilise une base de données en mémoire qui doit être initialisée à chaque démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-330">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="0d2bc-331">La base de données doit contenir un élément avant que vous ne passiez un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-331">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="0d2bc-332">Appelez GET afin de garantir qu’un élément existe dans la base de données avant d’effectuer un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-332">Call GET to insure there is an item in the database before making a PUT call.</span></span>

<span data-ttu-id="0d2bc-333">Mettez à jour la tâche dont l’id est 1 en définissant son nom sur « feed fish » :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-333">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="0d2bc-334">L’image suivante montre la mise à jour Postman :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-334">The following image shows the Postman update:</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="0d2bc-336">Ajouter une méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="0d2bc-336">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="0d2bc-337">Ajoutez la méthode `DeleteTodoItem` suivante :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-337">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="0d2bc-338">La réponse `DeleteTodoItem` est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-338">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="0d2bc-339">Tester la méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="0d2bc-339">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="0d2bc-340">Utilisez Postman pour supprimer une tâche :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-340">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="0d2bc-341">Définissez la méthode sur `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-341">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="0d2bc-342">Définissez l’URI de l’objet à supprimer, par exemple `https://localhost:5001/api/todo/1`.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-342">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="0d2bc-343">Sélectionnez **Send**.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-343">Select **Send**</span></span>

<span data-ttu-id="0d2bc-344">L’exemple d’application vous permet de supprimer tous les éléments, mais quand le dernier élément est supprimé, un nouveau est créé par le constructeur de classe de modèle au prochain appel de l’API.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-344">The sample app allows you to delete all the items, but when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="0d2bc-345">Appeler l’API avec jQuery</span><span class="sxs-lookup"><span data-stu-id="0d2bc-345">Call the API with jQuery</span></span>

<span data-ttu-id="0d2bc-346">Dans cette section, une page HTML qui utilise jQuery pour appeler l’API web est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-346">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="0d2bc-347">jQuery lance la requête et met à jour la page avec les détails de la réponse de l’API.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-347">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="0d2bc-348">Configurez l’application pour [traiter les fichiers statiques](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [activer le mappage de fichier par défaut](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-348">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_):</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

::: moniker range=">= aspnetcore-2.2"
<span data-ttu-id="0d2bc-349">Créez un dossier *wwwroot* dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-349">Create a *wwwroot* folder in the project directory.</span></span>
::: moniker-end

<span data-ttu-id="0d2bc-350">Ajoutez un fichier HTML nommé *index.html* au répertoire *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-350">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="0d2bc-351">Remplacez son contenu par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-351">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="0d2bc-352">Ajoutez un fichier JavaScript nommé *site.js* au répertoire *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-352">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="0d2bc-353">Remplacez son contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-353">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="0d2bc-354">Vous devrez peut-être changer les paramètres de lancement du projet ASP.NET Core pour tester la page HTML localement :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-354">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="0d2bc-355">Ouvrez *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-355">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="0d2bc-356">Supprimez la propriété `launchUrl` pour forcer l’utilisation du fichier *index.html* (fichier par défaut du projet) à l’ouverture de l’application.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-356">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="0d2bc-357">Il existe plusieurs façons d’obtenir jQuery.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-357">There are several ways to get jQuery.</span></span> <span data-ttu-id="0d2bc-358">Dans l’extrait précédent, la bibliothèque est chargée à partir d’un CDN.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-358">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="0d2bc-359">Cet exemple appelle toutes les méthodes CRUD de l’API.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-359">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="0d2bc-360">Les explications suivantes traitent des appels à l’API.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-360">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="0d2bc-361">Obtenir une liste de tâches</span><span class="sxs-lookup"><span data-stu-id="0d2bc-361">Get a list of to-do items</span></span>

<span data-ttu-id="0d2bc-362">La fonction JQuery [ajax](https://api.jquery.com/jquery.ajax/) envoie une requête `GET` à l’API, qui retourne du code JSON représentant un tableau de tâches.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-362">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="0d2bc-363">La fonction de rappel `success` est appelée si la requête réussit.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-363">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="0d2bc-364">Dans le rappel, le DOM est mis à jour avec les informations des tâches.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-364">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="0d2bc-365">Ajouter une tâche</span><span class="sxs-lookup"><span data-stu-id="0d2bc-365">Add a to-do item</span></span>

<span data-ttu-id="0d2bc-366">La fonction [ajax](https://api.jquery.com/jquery.ajax/) envoie une requête `POST` dont le corps indique la tâche.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-366">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="0d2bc-367">Les options `accepts` et `contentType` sont définies avec la valeur `application/json` pour spécifier le type de média qui est reçu et envoyé.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-367">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="0d2bc-368">La tâche est convertie au format JSON à l’aide de [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-368">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="0d2bc-369">Quand l’API retourne un code d’état de réussite, la fonction `getData` est appelée pour mettre à jour la table HTML.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-369">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="0d2bc-370">Mettre à jour une tâche</span><span class="sxs-lookup"><span data-stu-id="0d2bc-370">Update a to-do item</span></span>

<span data-ttu-id="0d2bc-371">La mise à jour d’une tâche est similaire à l’ajout d’une tâche.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-371">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="0d2bc-372">L’identificateur unique de la tâche est ajouté à l’`url` et le `type` est `PUT`.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-372">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="0d2bc-373">Supprimer une tâche</span><span class="sxs-lookup"><span data-stu-id="0d2bc-373">Delete a to-do item</span></span>

<span data-ttu-id="0d2bc-374">Pour supprimer une tâche, vous devez définir le `type` sur l’appel AJAX avec la valeur `DELETE` et spécifier l’identificateur unique de l’élément dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="0d2bc-374">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

## <a name="additional-resources"></a><span data-ttu-id="0d2bc-375">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0d2bc-375">Additional resources</span></span>

<span data-ttu-id="0d2bc-376">[Affichez ou téléchargez l’exemple de code de ce tutoriel](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-376">[View or download sample code for this tutorial](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="0d2bc-377">Consultez [Guide pratique pour télécharger](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="0d2bc-377">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="0d2bc-378">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-378">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>

## <a name="next-steps"></a><span data-ttu-id="0d2bc-379">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="0d2bc-379">Next steps</span></span>

<span data-ttu-id="0d2bc-380">Dans ce didacticiel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-380">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0d2bc-381">Créer un projet d’API web</span><span class="sxs-lookup"><span data-stu-id="0d2bc-381">Create a web api project.</span></span>
> * <span data-ttu-id="0d2bc-382">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="0d2bc-382">Add a model class.</span></span>
> * <span data-ttu-id="0d2bc-383">Créer le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="0d2bc-383">Create the database context.</span></span>
> * <span data-ttu-id="0d2bc-384">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="0d2bc-384">Register the database context.</span></span>
> * <span data-ttu-id="0d2bc-385">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="0d2bc-385">Add a controller.</span></span>
> * <span data-ttu-id="0d2bc-386">Ajouter les méthodes CRUD</span><span class="sxs-lookup"><span data-stu-id="0d2bc-386">Add CRUD methods.</span></span>
> * <span data-ttu-id="0d2bc-387">Configurer le routage et les chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="0d2bc-387">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="0d2bc-388">Spécifier des valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="0d2bc-388">Specify return values.</span></span>
> * <span data-ttu-id="0d2bc-389">Appeler l’API web avec Postman</span><span class="sxs-lookup"><span data-stu-id="0d2bc-389">Call the web API with Postman.</span></span>
> * <span data-ttu-id="0d2bc-390">Appeler l’API web avec jQuery</span><span class="sxs-lookup"><span data-stu-id="0d2bc-390">Call the web api with jQuery.</span></span>

<span data-ttu-id="0d2bc-391">Passez au tutoriel suivant pour apprendre à générer des pages d’aide d’API :</span><span class="sxs-lookup"><span data-stu-id="0d2bc-391">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
