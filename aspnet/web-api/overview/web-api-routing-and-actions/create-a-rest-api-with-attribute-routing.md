---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Créer une API REST avec routage d’attribut dans API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: f6ff5fa18a44b3e6717ec0141ebe101bcdc0bee4
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89045180"
---
# <a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="1cf9a-102">Créer une API REST avec routage d’attribut dans API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="1cf9a-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>

<span data-ttu-id="1cf9a-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1cf9a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1cf9a-104">L’API Web 2 prend en charge un nouveau type de routage, appelé *routage d’attribut*.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="1cf9a-105">Pour obtenir une vue d’ensemble générale du routage d’attributs, consultez [routage d’attributs dans l’API Web 2](attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="1cf9a-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="1cf9a-106">Dans ce didacticiel, vous allez utiliser le routage d’attributs pour créer une API REST pour une collection de livres.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="1cf9a-107">L’API prend en charge les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="1cf9a-107">The API will support the following actions:</span></span>

| <span data-ttu-id="1cf9a-108">Action</span><span class="sxs-lookup"><span data-stu-id="1cf9a-108">Action</span></span> | <span data-ttu-id="1cf9a-109">Exemple d’URI</span><span class="sxs-lookup"><span data-stu-id="1cf9a-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="1cf9a-110">Obtenir la liste de tous les livres.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-110">Get a list of all books.</span></span> | <span data-ttu-id="1cf9a-111">/api/books</span><span class="sxs-lookup"><span data-stu-id="1cf9a-111">/api/books</span></span> |
| <span data-ttu-id="1cf9a-112">Obtenir un livre par ID.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-112">Get a book by ID.</span></span> | <span data-ttu-id="1cf9a-113">/api/books/1</span><span class="sxs-lookup"><span data-stu-id="1cf9a-113">/api/books/1</span></span> |
| <span data-ttu-id="1cf9a-114">Obtenir les détails d’un livre.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-114">Get the details of a book.</span></span> | <span data-ttu-id="1cf9a-115">/api/books/1/details</span><span class="sxs-lookup"><span data-stu-id="1cf9a-115">/api/books/1/details</span></span> |
| <span data-ttu-id="1cf9a-116">Obtient une liste de livres par genre.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-116">Get a list of books by genre.</span></span> | <span data-ttu-id="1cf9a-117">/api/books/fantasy</span><span class="sxs-lookup"><span data-stu-id="1cf9a-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="1cf9a-118">Obtenir une liste de livres par date de publication.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="1cf9a-119">/API/Books/date/2013-02-16/API/Books/date/2013/02/16 (autre forme)</span><span class="sxs-lookup"><span data-stu-id="1cf9a-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="1cf9a-120">Obtenir la liste des livres d’un auteur donné.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="1cf9a-121">/api/authors/1/books</span><span class="sxs-lookup"><span data-stu-id="1cf9a-121">/api/authors/1/books</span></span> |

<span data-ttu-id="1cf9a-122">Toutes les méthodes sont en lecture seule (requêtes HTTP d’extraction).</span><span class="sxs-lookup"><span data-stu-id="1cf9a-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="1cf9a-123">Pour la couche de données, nous allons utiliser Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="1cf9a-124">Les enregistrements de livres comporteront les champs suivants :</span><span class="sxs-lookup"><span data-stu-id="1cf9a-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="1cf9a-125">ID</span><span class="sxs-lookup"><span data-stu-id="1cf9a-125">ID</span></span>
- <span data-ttu-id="1cf9a-126">Titre</span><span class="sxs-lookup"><span data-stu-id="1cf9a-126">Title</span></span>
- <span data-ttu-id="1cf9a-127">Genre</span><span class="sxs-lookup"><span data-stu-id="1cf9a-127">Genre</span></span>
- <span data-ttu-id="1cf9a-128">Date de publication</span><span class="sxs-lookup"><span data-stu-id="1cf9a-128">Publication date</span></span>
- <span data-ttu-id="1cf9a-129">Prix</span><span class="sxs-lookup"><span data-stu-id="1cf9a-129">Price</span></span>
- <span data-ttu-id="1cf9a-130">Description</span><span class="sxs-lookup"><span data-stu-id="1cf9a-130">Description</span></span>
- <span data-ttu-id="1cf9a-131">Réfaut-réfaut (clé étrangère à une table authors)</span><span class="sxs-lookup"><span data-stu-id="1cf9a-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="1cf9a-132">Toutefois, pour la plupart des requêtes, l’API retourne un sous-ensemble de ces données (titre, auteur et genre).</span><span class="sxs-lookup"><span data-stu-id="1cf9a-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="1cf9a-133">Pour obtenir l’enregistrement complet, le client demande `/api/books/{id}/details` .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1cf9a-134">Prérequis</span><span class="sxs-lookup"><span data-stu-id="1cf9a-134">Prerequisites</span></span>

<span data-ttu-id="1cf9a-135">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional ou Enterprise Edition.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-135">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="1cf9a-136">Créer le projet Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1cf9a-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="1cf9a-137">Commencez par exécuter Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-137">Start by running Visual Studio.</span></span> <span data-ttu-id="1cf9a-138">Dans le menu **File** (Fichier), sélectionnez **New** (Nouveau), puis **Project** (Projet).</span><span class="sxs-lookup"><span data-stu-id="1cf9a-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="1cf9a-139">Développez **Installed**la  >  catégorie**Visual C#** installée.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-139">Expand the **Installed** > **Visual C#** category.</span></span> <span data-ttu-id="1cf9a-140">Sous **Visual C#**, sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="1cf9a-141">Dans la liste des modèles de projet, sélectionnez **application Web ASP.net (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-141">In the list of project templates, select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="1cf9a-142">Nommez le projet &quot; BooksAPI &quot; .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="1cf9a-143">Dans la boîte de dialogue **nouvelle application Web ASP.net** , sélectionnez le modèle **vide** .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-143">In the **New ASP.NET Web Application** dialog, select the **Empty** template.</span></span> <span data-ttu-id="1cf9a-144">Sous « ajouter des dossiers et des références principales pour », activez la case à cocher **API Web** .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="1cf9a-145">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-145">Click **OK**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="1cf9a-146">Cela crée un projet squelette configuré pour la fonctionnalité de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="1cf9a-147">Modèles de domaine</span><span class="sxs-lookup"><span data-stu-id="1cf9a-147">Domain Models</span></span>

<span data-ttu-id="1cf9a-148">Ensuite, ajoutez des classes pour les modèles de domaine.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-148">Next, add classes for domain models.</span></span> <span data-ttu-id="1cf9a-149">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier Modèles.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="1cf9a-150">Sélectionnez **Ajouter**, puis **classe**.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="1cf9a-151">Nommez la classe `Author`.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="1cf9a-152">Remplacez le code dans Author.cs par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="1cf9a-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="1cf9a-153">Ajoutez maintenant une autre classe nommée `Book` .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="1cf9a-154">Ajouter un contrôleur d’API Web</span><span class="sxs-lookup"><span data-stu-id="1cf9a-154">Add a Web API Controller</span></span>

<span data-ttu-id="1cf9a-155">Dans cette étape, nous allons ajouter un contrôleur d’API Web qui utilise Entity Framework comme couche de données.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="1cf9a-156">Appuyez sur CTRL+MAJ+B pour générer le projet.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="1cf9a-157">Entity Framework utilise la réflexion pour découvrir les propriétés des modèles. il requiert donc un assembly compilé pour créer le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="1cf9a-158">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier Contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="1cf9a-159">Sélectionnez **Ajouter**, puis **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="1cf9a-160">Dans la boîte de dialogue **Ajouter une structure** , sélectionnez **contrôleur d’API Web 2 avec actions, à l’aide de Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-160">In the **Add Scaffold** dialog, select **Web API 2 Controller with actions, using Entity Framework**.</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="1cf9a-161">Dans la boîte de dialogue **Ajouter un contrôleur** , pour **nom du contrôleur**, entrez &quot; BooksController &quot; .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="1cf9a-162">Cochez la &quot; case utiliser les actions du contrôleur Async &quot; .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="1cf9a-163">Pour **classe de modèle**, sélectionnez &quot; livre &quot; .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="1cf9a-164">(Si vous ne voyez pas la `Book` classe dans la liste déroulante, assurez-vous que vous avez créé le projet.) Cliquez ensuite sur le bouton « + ».</span><span class="sxs-lookup"><span data-stu-id="1cf9a-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="1cf9a-165">Cliquez sur **Ajouter** dans la boîte de dialogue **nouveau contexte de données** .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="1cf9a-166">Cliquez sur **Ajouter** dans la boîte de dialogue **Ajouter un contrôleur** .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="1cf9a-167">La génération de modèles automatique ajoute une classe nommée `BooksController` qui définit le contrôleur d’API.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="1cf9a-168">Elle ajoute également une classe nommée `BooksAPIContext` dans le dossier Models, qui définit le contexte de données pour Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="1cf9a-169">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="1cf9a-169">Seed the Database</span></span>

<span data-ttu-id="1cf9a-170">Dans le menu Outils, sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-170">From the Tools menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="1cf9a-171">Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="1cf9a-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="1cf9a-172">Cette commande crée un dossier migrations et ajoute un nouveau fichier de code nommé Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="1cf9a-173">Ouvrez ce fichier et ajoutez le code suivant à la `Configuration.Seed` méthode.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="1cf9a-174">Dans la fenêtre console du gestionnaire de package, tapez les commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="1cf9a-175">Ces commandes créent une base de données locale et appellent la méthode Seed pour remplir la base de données.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="1cf9a-176">Ajouter des classes DTO</span><span class="sxs-lookup"><span data-stu-id="1cf9a-176">Add DTO Classes</span></span>

<span data-ttu-id="1cf9a-177">Si vous exécutez l’application maintenant et envoyez une requête d’extraction à/API/Books/1, la réponse ressemble à ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="1cf9a-178">(J’ai ajouté une mise en retrait pour une meilleure lisibilité.)</span><span class="sxs-lookup"><span data-stu-id="1cf9a-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="1cf9a-179">Au lieu de cela, je souhaite que cette requête retourne un sous-ensemble des champs.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="1cf9a-180">En outre, je souhaite qu’elle retourne le nom de l’auteur plutôt que l’ID de l’auteur.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="1cf9a-181">Pour ce faire, nous allons modifier les méthodes de contrôleur pour retourner un *objet de transfert de données* (DTO) à la place du modèle EF.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="1cf9a-182">Un DTO est un objet conçu uniquement pour transporter des données.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="1cf9a-183">Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter**  |  **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="1cf9a-184">Nommez le dossier &quot; DTO &quot; .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="1cf9a-185">Ajoutez une classe nommée `BookDto` au dossier DTO, avec la définition suivante :</span><span class="sxs-lookup"><span data-stu-id="1cf9a-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="1cf9a-186">Ajoutez une autre classe nommée `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="1cf9a-187">Ensuite, mettez à jour la `BooksController` classe pour retourner des `BookDto` instances.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="1cf9a-188">Nous allons utiliser la méthode [Queryable. Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) pour projeter des `Book` instances sur des instances `BookDto` .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="1cf9a-189">Voici le code mis à jour pour la classe Controller.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="1cf9a-190">J’ai supprimé `PutBook` les `PostBook` méthodes, et `DeleteBook` , car elles ne sont pas nécessaires pour ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>

<span data-ttu-id="1cf9a-191">Maintenant, si vous exécutez l’application et demandez/API/Books/1, le corps de la réponse doit se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="1cf9a-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="1cf9a-192">Ajouter des attributs de route</span><span class="sxs-lookup"><span data-stu-id="1cf9a-192">Add Route Attributes</span></span>

<span data-ttu-id="1cf9a-193">Ensuite, nous allons convertir le contrôleur pour utiliser le routage d’attribut.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="1cf9a-194">Tout d’abord, ajoutez un attribut **RoutePrefix** au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="1cf9a-195">Cet attribut définit les segments d’URI initiaux pour toutes les méthodes sur ce contrôleur.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="1cf9a-196">Ajoutez ensuite les attributs **[route]** aux actions du contrôleur, comme suit :</span><span class="sxs-lookup"><span data-stu-id="1cf9a-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="1cf9a-197">Le modèle de routage pour chaque méthode de contrôleur est le préfixe plus la chaîne spécifiée dans l’attribut de **routage** .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="1cf9a-198">Pour la `GetBook` méthode, le modèle de routage inclut la chaîne paramétrable &quot; {ID : int} &quot; , qui correspond si le segment d’URI contient une valeur entière.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="1cf9a-199">Méthode</span><span class="sxs-lookup"><span data-stu-id="1cf9a-199">Method</span></span> | <span data-ttu-id="1cf9a-200">Modèle de routage</span><span class="sxs-lookup"><span data-stu-id="1cf9a-200">Route Template</span></span> | <span data-ttu-id="1cf9a-201">Exemple d’URI</span><span class="sxs-lookup"><span data-stu-id="1cf9a-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="1cf9a-202">« API/livres »</span><span class="sxs-lookup"><span data-stu-id="1cf9a-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="1cf9a-203">« API/livres/{ID : int} »</span><span class="sxs-lookup"><span data-stu-id="1cf9a-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="1cf9a-204">Récupérer les détails du livre</span><span class="sxs-lookup"><span data-stu-id="1cf9a-204">Get Book Details</span></span>

<span data-ttu-id="1cf9a-205">Pour obtenir les détails du livre, le client envoie une requête d’extraction à `/api/books/{id}/details` , où *{ID}* est l’ID du livre.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="1cf9a-206">Ajoutez la méthode suivante à la classe `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="1cf9a-207">Si vous demandez `/api/books/1/details` , la réponse ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="1cf9a-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="1cf9a-208">Récupérer des livres par genre</span><span class="sxs-lookup"><span data-stu-id="1cf9a-208">Get Books By Genre</span></span>

<span data-ttu-id="1cf9a-209">Pour obtenir une liste de livres dans un genre spécifique, le client envoie une requête d’extraction à `/api/books/genre` , où *genre* est le nom du genre.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="1cf9a-210">(Par exemple, `/api/books/fantasy`.)</span><span class="sxs-lookup"><span data-stu-id="1cf9a-210">(For example, `/api/books/fantasy`.)</span></span>

<span data-ttu-id="1cf9a-211">Ajoutez la méthode suivante à `BooksController` .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="1cf9a-212">Ici, nous définissons un itinéraire qui contient un paramètre {genre} dans le modèle d’URI.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="1cf9a-213">Notez que l’API Web est en mesure de distinguer ces deux URI et de les acheminer vers différentes méthodes :</span><span class="sxs-lookup"><span data-stu-id="1cf9a-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="1cf9a-214">Cela est dû au fait que la `GetBook` méthode comprend une contrainte indiquant que le segment « ID » doit être une valeur entière :</span><span class="sxs-lookup"><span data-stu-id="1cf9a-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="1cf9a-215">Si vous demandez/API/Books/Fantasy, la réponse ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="1cf9a-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="1cf9a-216">Recevoir des livres par auteur</span><span class="sxs-lookup"><span data-stu-id="1cf9a-216">Get Books By Author</span></span>

<span data-ttu-id="1cf9a-217">Pour obtenir la liste des livres d’un auteur donné, le client envoie une requête d’extraction à `/api/authors/id/books` , où *ID* est l’ID de l’auteur.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="1cf9a-218">Ajoutez la méthode suivante à `BooksController` .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="1cf9a-219">Cet exemple est intéressant, &quot; car &quot; la documentation est traitée comme une ressource enfant des &quot; auteurs &quot; .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="1cf9a-220">Ce modèle est assez courant dans les API RESTful.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="1cf9a-221">Le tilde (~) dans le modèle de routage remplace le préfixe d’itinéraire dans l’attribut **RoutePrefix** .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="1cf9a-222">Récupérer les livres par date de publication</span><span class="sxs-lookup"><span data-stu-id="1cf9a-222">Get Books By Publication Date</span></span>

<span data-ttu-id="1cf9a-223">Pour obtenir une liste de livres par date de publication, le client envoie une requête d’extraction à `/api/books/date/yyyy-mm-dd` , où *aaaa-mm-jj* est la date.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="1cf9a-224">Voici une façon de procéder :</span><span class="sxs-lookup"><span data-stu-id="1cf9a-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="1cf9a-225">Le `{pubdate:datetime}` paramètre est restreint pour correspondre à une valeur **DateTime** .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="1cf9a-226">Cela fonctionne, mais il est en fait plus permissif que ce que nous aimerions.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="1cf9a-227">Par exemple, ces URI correspondent également à l’itinéraire :</span><span class="sxs-lookup"><span data-stu-id="1cf9a-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="1cf9a-228">Il n’y a rien de mal à autoriser ces URI.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="1cf9a-229">Toutefois, vous pouvez limiter l’itinéraire à un format particulier en ajoutant une contrainte d’expression régulière au modèle de routage :</span><span class="sxs-lookup"><span data-stu-id="1cf9a-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="1cf9a-230">À présent, seules les dates au format &quot; aaaa-mm-jj &quot; correspondent.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="1cf9a-231">Notez que nous n’utilisons pas l’expression régulière pour confirmer que nous obtenons une date réelle.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="1cf9a-232">Qui est géré lorsque l’API Web tente de convertir le segment d’URI en une instance **DateTime** .</span><span class="sxs-lookup"><span data-stu-id="1cf9a-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="1cf9a-233">Une date non valide telle que « 2012-47-99 » ne sera pas convertie et le client obtiendra une erreur 404.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="1cf9a-234">Vous pouvez également prendre en charge un séparateur de barre oblique ( `/api/books/date/yyyy/mm/dd` ) en ajoutant un autre attribut **[route]** avec une expression régulière différente.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="1cf9a-235">Il y a un détail subtil mais important ici.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="1cf9a-236">Le deuxième modèle de routage comporte un caractère générique ( \* ) au début du paramètre {pubDate} :</span><span class="sxs-lookup"><span data-stu-id="1cf9a-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.txt)]

<span data-ttu-id="1cf9a-237">Cela indique au moteur de routage que {pubDate} doit correspondre au reste de l’URI.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="1cf9a-238">Par défaut, un paramètre de modèle correspond à un segment d’URI unique.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="1cf9a-239">Dans ce cas, nous souhaitons que {pubDate} couvre plusieurs segments d’URI :</span><span class="sxs-lookup"><span data-stu-id="1cf9a-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="1cf9a-240">Code du contrôleur</span><span class="sxs-lookup"><span data-stu-id="1cf9a-240">Controller Code</span></span>

<span data-ttu-id="1cf9a-241">Voici le code complet de la classe BooksController.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="1cf9a-242">Résumé</span><span class="sxs-lookup"><span data-stu-id="1cf9a-242">Summary</span></span>

<span data-ttu-id="1cf9a-243">Le routage des attributs vous offre davantage de contrôle et une plus grande flexibilité lors de la conception des URI de votre API.</span><span class="sxs-lookup"><span data-stu-id="1cf9a-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
