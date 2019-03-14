---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Bien démarrer avec ASP.NET Web API 2 (c#)
author: MikeWasson
description: HTTP n’est pas simplement pour servir des pages web. Il est également une plateforme puissante pour la création d’API qui exposent des services et données. HTTP est simple, flexible et ubiq...
ms.author: riande
ms.date: 11/28/2017
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7bec95af4532535f0d620bfe6862958907466874
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57060186"
---
<a name="get-started-with-aspnet-web-api-2-c"></a><span data-ttu-id="79a29-105">Bien démarrer avec ASP.NET Web API 2 (c#)</span><span class="sxs-lookup"><span data-stu-id="79a29-105">Get Started with ASP.NET Web API 2 (C#)</span></span>
====================
<span data-ttu-id="79a29-106">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="79a29-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="79a29-107">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="79a29-107">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

<span data-ttu-id="79a29-108">HTTP n’est pas simplement pour servir des pages web.</span><span class="sxs-lookup"><span data-stu-id="79a29-108">HTTP is not just for serving up web pages.</span></span> <span data-ttu-id="79a29-109">HTTP est également une plateforme puissante pour la création d’API qui exposent des services et données.</span><span class="sxs-lookup"><span data-stu-id="79a29-109">HTTP is also a powerful platform for building APIs that expose services and data.</span></span> <span data-ttu-id="79a29-110">HTTP est simple, flexible et omniprésent.</span><span class="sxs-lookup"><span data-stu-id="79a29-110">HTTP is simple, flexible, and ubiquitous.</span></span> <span data-ttu-id="79a29-111">Presque n’importe quelle plateforme qui vous pouvez considérer a une bibliothèque HTTP, pour les services HTTP peuvent atteindre un large éventail de clients, y compris les navigateurs, les appareils mobiles et les applications de bureau traditionnelles.</span><span class="sxs-lookup"><span data-stu-id="79a29-111">Almost any platform that you can think of has an HTTP library, so HTTP services can reach a broad range of clients, including browsers, mobile devices, and traditional desktop applications.</span></span>

<span data-ttu-id="79a29-112">API Web ASP.NET est une infrastructure pour la création d’API web basées sur le .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="79a29-112">ASP.NET Web API is a framework for building web APIs on top of the .NET Framework.</span></span> <span data-ttu-id="79a29-113">Dans ce didacticiel, vous utiliserez des API Web ASP.NET pour créer une API web qui retourne une liste de produits.</span><span class="sxs-lookup"><span data-stu-id="79a29-113">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span>

## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="79a29-114">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="79a29-114">Software versions used in the tutorial</span></span>

- [<span data-ttu-id="79a29-115">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="79a29-115">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- <span data-ttu-id="79a29-116">Web API 2</span><span class="sxs-lookup"><span data-stu-id="79a29-116">Web API 2</span></span>

<span data-ttu-id="79a29-117">Consultez [créer une API web avec ASP.NET Core et Visual Studio pour Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) pour une version plus récente de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="79a29-117">See [Create a web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) for a newer version of this tutorial.</span></span>

## <a name="create-a-web-api-project"></a><span data-ttu-id="79a29-118">Créer un projet d’API Web</span><span class="sxs-lookup"><span data-stu-id="79a29-118">Create a Web API Project</span></span>

<span data-ttu-id="79a29-119">Dans ce didacticiel, vous utiliserez des API Web ASP.NET pour créer une API web qui retourne une liste de produits.</span><span class="sxs-lookup"><span data-stu-id="79a29-119">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span> <span data-ttu-id="79a29-120">La page web frontal utilise jQuery pour afficher les résultats.</span><span class="sxs-lookup"><span data-stu-id="79a29-120">The front-end web page uses jQuery to display the results.</span></span>

![](tutorial-your-first-web-api/_static/image1.png)

<span data-ttu-id="79a29-121">Démarrez Visual Studio et sélectionnez **nouveau projet** à partir de la **Démarrer** page.</span><span class="sxs-lookup"><span data-stu-id="79a29-121">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="79a29-122">Ou, à partir de la **fichier** menu, sélectionnez **New** , puis **projet**.</span><span class="sxs-lookup"><span data-stu-id="79a29-122">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="79a29-123">Dans le **modèles** volet, sélectionnez **modèles installés** et développez le **Visual C#** nœud.</span><span class="sxs-lookup"><span data-stu-id="79a29-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="79a29-124">Sous **Visual C#**, sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="79a29-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="79a29-125">Dans la liste des modèles de projet, sélectionnez **Application Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="79a29-125">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="79a29-126">Nommez le projet « ProductsApp » et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="79a29-126">Name the project "ProductsApp" and click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image2.png)

<span data-ttu-id="79a29-127">Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **vide** modèle.</span><span class="sxs-lookup"><span data-stu-id="79a29-127">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="79a29-128">Sous &quot;ajouter des dossiers et les références principales pour&quot;, vérifiez **API Web**.</span><span class="sxs-lookup"><span data-stu-id="79a29-128">Under &quot;Add folders and core references for&quot;, check **Web API**.</span></span> <span data-ttu-id="79a29-129">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="79a29-129">Click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="79a29-130">Vous pouvez également créer un projet d’API Web en utilisant le &quot;API Web&quot; modèle.</span><span class="sxs-lookup"><span data-stu-id="79a29-130">You can also create a Web API project using the &quot;Web API&quot; template.</span></span> <span data-ttu-id="79a29-131">Le modèle API Web utilise ASP.NET MVC pour fournir des pages d’aide API.</span><span class="sxs-lookup"><span data-stu-id="79a29-131">The Web API template uses ASP.NET MVC to provide API help pages.</span></span> <span data-ttu-id="79a29-132">J’utilise le modèle vide pour ce didacticiel, car je veux vous montrer les API Web sans MVC.</span><span class="sxs-lookup"><span data-stu-id="79a29-132">I'm using the Empty template for this tutorial because I want to show Web API without MVC.</span></span> <span data-ttu-id="79a29-133">En règle générale, vous n’avez pas besoin de savoir ASP.NET MVC pour utiliser l’API Web.</span><span class="sxs-lookup"><span data-stu-id="79a29-133">In general, you don't need to know ASP.NET MVC to use Web API.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="79a29-134">Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="79a29-134">Adding a Model</span></span>

<span data-ttu-id="79a29-135">Un *modèle* est un objet qui représente les données dans votre application.</span><span class="sxs-lookup"><span data-stu-id="79a29-135">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="79a29-136">API Web ASP.NET peut sérialiser automatiquement votre modèle JSON, XML ou un autre format, puis écrire les données sérialisées dans le corps du message de réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="79a29-136">ASP.NET Web API can automatically serialize your model to JSON, XML, or some other format, and then write the serialized data into the body of the HTTP response message.</span></span> <span data-ttu-id="79a29-137">Tant qu’un client peut lire le format de sérialisation, il peut désérialiser l’objet.</span><span class="sxs-lookup"><span data-stu-id="79a29-137">As long as a client can read the serialization format, it can deserialize the object.</span></span> <span data-ttu-id="79a29-138">La plupart des clients peut analyser XML ou JSON.</span><span class="sxs-lookup"><span data-stu-id="79a29-138">Most clients can parse either XML or JSON.</span></span> <span data-ttu-id="79a29-139">En outre, le client peut indiquer quel format qu’il veut en définissant l’en-tête Accept dans le message de demande HTTP.</span><span class="sxs-lookup"><span data-stu-id="79a29-139">Moreover, the client can indicate which format it wants by setting the Accept header in the HTTP request message.</span></span>

<span data-ttu-id="79a29-140">Commençons par créer un modèle simple qui représente un produit.</span><span class="sxs-lookup"><span data-stu-id="79a29-140">Let's start by creating a simple model that represents a product.</span></span>

<span data-ttu-id="79a29-141">Si l’Explorateur de solutions n’est pas déjà visible, cliquez sur le **vue** menu et sélectionnez **l’Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="79a29-141">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="79a29-142">Dans l’Explorateur de solutions, cliquez sur le dossier de modèles.</span><span class="sxs-lookup"><span data-stu-id="79a29-142">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="79a29-143">Dans le menu contextuel, sélectionnez **ajouter** puis sélectionnez **classe**.</span><span class="sxs-lookup"><span data-stu-id="79a29-143">From the context menu, select **Add** then select **Class**.</span></span>

![](tutorial-your-first-web-api/_static/image4.png)

<span data-ttu-id="79a29-144">Nommez la classe &quot;produit&quot;.</span><span class="sxs-lookup"><span data-stu-id="79a29-144">Name the class &quot;Product&quot;.</span></span> <span data-ttu-id="79a29-145">Ajoutez les propriétés suivantes à la `Product` classe.</span><span class="sxs-lookup"><span data-stu-id="79a29-145">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a><span data-ttu-id="79a29-146">Ajour d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="79a29-146">Adding a Controller</span></span>

<span data-ttu-id="79a29-147">Dans l’API Web, un *contrôleur* est un objet qui gère les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="79a29-147">In Web API, a *controller* is an object that handles HTTP requests.</span></span> <span data-ttu-id="79a29-148">Nous allons ajouter un contrôleur qui peut retourner une liste de produits ou un produit unique spécifié par ID.</span><span class="sxs-lookup"><span data-stu-id="79a29-148">We'll add a controller that can return either a list of products or a single product specified by ID.</span></span>

> [!NOTE]
> <span data-ttu-id="79a29-149">Si vous avez utilisé ASP.NET MVC, vous êtes déjà familiarisé avec les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="79a29-149">If you have used ASP.NET MVC, you are already familiar with controllers.</span></span> <span data-ttu-id="79a29-150">Contrôleurs d’API Web sont similaires aux contrôleurs MVC, mais héritent le **ApiController** classe au lieu du **contrôleur** classe.</span><span class="sxs-lookup"><span data-stu-id="79a29-150">Web API controllers are similar to MVC controllers, but inherit the **ApiController** class instead of the **Controller** class.</span></span>

<span data-ttu-id="79a29-151">Dans **l’Explorateur de solutions**, cliquez sur le dossier contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="79a29-151">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="79a29-152">Sélectionnez **ajouter** , puis sélectionnez **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="79a29-152">Select **Add** and then select **Controller**.</span></span>

![](tutorial-your-first-web-api/_static/image5.png)

<span data-ttu-id="79a29-153">Dans le **ajouter une structure** boîte de dialogue, sélectionnez **contrôleur d’API Web - vide**.</span><span class="sxs-lookup"><span data-stu-id="79a29-153">In the **Add Scaffold** dialog, select **Web API Controller - Empty**.</span></span> <span data-ttu-id="79a29-154">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="79a29-154">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image6.png)

<span data-ttu-id="79a29-155">Dans le **ajouter un contrôleur** boîte de dialogue, nommez le contrôleur &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="79a29-155">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="79a29-156">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="79a29-156">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image7.png)

<span data-ttu-id="79a29-157">La génération de modèles automatique crée un fichier nommé ProductsController.cs dans le dossier contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="79a29-157">The scaffolding creates a file named ProductsController.cs in the Controllers folder.</span></span>

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> <span data-ttu-id="79a29-158">Vous n’avez pas besoin de placer vos contrôleurs dans un dossier nommé contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="79a29-158">You don't need to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="79a29-159">Le nom du dossier est simplement un moyen pratique d’organiser vos fichiers sources.</span><span class="sxs-lookup"><span data-stu-id="79a29-159">The folder name is just a convenient way to organize your source files.</span></span>


<span data-ttu-id="79a29-160">Si ce fichier n’est pas déjà ouvert, double-cliquez sur le fichier pour l’ouvrir.</span><span class="sxs-lookup"><span data-stu-id="79a29-160">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="79a29-161">Remplacez le code dans ce fichier avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="79a29-161">Replace the code in this file with the following:</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

<span data-ttu-id="79a29-162">Pour simplifier l’exemple, les produits sont stockés dans un tableau fixe à l’intérieur de la classe de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="79a29-162">To keep the example simple, products are stored in a fixed array inside the controller class.</span></span> <span data-ttu-id="79a29-163">Bien sûr, dans une application réelle, vous serez interroger une base de données ou utiliser une autre source de données externe.</span><span class="sxs-lookup"><span data-stu-id="79a29-163">Of course, in a real application, you would query a database or use some other external data source.</span></span>

<span data-ttu-id="79a29-164">Le contrôleur définit deux méthodes qui retournent des produits :</span><span class="sxs-lookup"><span data-stu-id="79a29-164">The controller defines two methods that return products:</span></span>

- <span data-ttu-id="79a29-165">Le `GetAllProducts` méthode retourne la liste complète des produits comme une **IEnumerable&lt;produit&gt;**  type.</span><span class="sxs-lookup"><span data-stu-id="79a29-165">The `GetAllProducts` method returns the entire list of products as an **IEnumerable&lt;Product&gt;** type.</span></span>
- <span data-ttu-id="79a29-166">Le `GetProduct` méthode recherche un produit unique par son ID.</span><span class="sxs-lookup"><span data-stu-id="79a29-166">The `GetProduct` method looks up a single product by its ID.</span></span>

<span data-ttu-id="79a29-167">C’est tout !</span><span class="sxs-lookup"><span data-stu-id="79a29-167">That's it!</span></span> <span data-ttu-id="79a29-168">Vous avez une API de travail web.</span><span class="sxs-lookup"><span data-stu-id="79a29-168">You have a working web API.</span></span> <span data-ttu-id="79a29-169">Chaque méthode sur le contrôleur correspond à un ou plusieurs URI :</span><span class="sxs-lookup"><span data-stu-id="79a29-169">Each method on the controller corresponds to one or more URIs:</span></span>

| <span data-ttu-id="79a29-170">Méthode de contrôleur</span><span class="sxs-lookup"><span data-stu-id="79a29-170">Controller Method</span></span> | <span data-ttu-id="79a29-171">URI</span><span class="sxs-lookup"><span data-stu-id="79a29-171">URI</span></span> |
| --- | --- |
| <span data-ttu-id="79a29-172">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="79a29-172">GetAllProducts</span></span> | <span data-ttu-id="79a29-173">/ api/produits</span><span class="sxs-lookup"><span data-stu-id="79a29-173">/api/products</span></span> |
| <span data-ttu-id="79a29-174">GetProduct</span><span class="sxs-lookup"><span data-stu-id="79a29-174">GetProduct</span></span> | <span data-ttu-id="79a29-175">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="79a29-175">/api/products/*id*</span></span> |

<span data-ttu-id="79a29-176">Pour le `GetProduct` (méthode), le *id* dans l’URI est un espace réservé.</span><span class="sxs-lookup"><span data-stu-id="79a29-176">For the `GetProduct` method, the *id* in the URI is a placeholder.</span></span> <span data-ttu-id="79a29-177">Par exemple, pour obtenir le produit avec l’ID de 5, l’URI est `api/products/5`.</span><span class="sxs-lookup"><span data-stu-id="79a29-177">For example, to get the product with ID of 5, the URI is `api/products/5`.</span></span>

<span data-ttu-id="79a29-178">Pour plus d’informations sur comment les API Web achemine les requêtes HTTP aux méthodes de contrôleur, consultez [routage dans ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="79a29-178">For more information about how Web API routes HTTP requests to controller methods, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="calling-the-web-api-with-javascript-and-jquery"></a><span data-ttu-id="79a29-179">Appel de l’API Web avec Javascript et jQuery</span><span class="sxs-lookup"><span data-stu-id="79a29-179">Calling the Web API with Javascript and jQuery</span></span>

<span data-ttu-id="79a29-180">Dans cette section, nous allons ajouter une page HTML qui utilise AJAX pour appeler l’API web.</span><span class="sxs-lookup"><span data-stu-id="79a29-180">In this section, we'll add an HTML page that uses AJAX to call the web API.</span></span> <span data-ttu-id="79a29-181">Nous allons utiliser jQuery pour effectuer les appels AJAX et pour mettre à jour de la page avec les résultats.</span><span class="sxs-lookup"><span data-stu-id="79a29-181">We'll use jQuery to make the AJAX calls and also to update the page with the results.</span></span>

<span data-ttu-id="79a29-182">Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **ajouter**, puis sélectionnez **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="79a29-182">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span>

![](tutorial-your-first-web-api/_static/image9.png)

<span data-ttu-id="79a29-183">Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez le **Web** nœud sous **Visual C#**, puis sélectionnez le **HTML Page** élément.</span><span class="sxs-lookup"><span data-stu-id="79a29-183">In the **Add New Item** dialog, select the **Web** node under **Visual C#**, and then select the **HTML Page** item.</span></span> <span data-ttu-id="79a29-184">Nommez la page &quot;index.html&quot;.</span><span class="sxs-lookup"><span data-stu-id="79a29-184">Name the page &quot;index.html&quot;.</span></span>

![](tutorial-your-first-web-api/_static/image10.png)

<span data-ttu-id="79a29-185">Remplacez tous les éléments de ce fichier avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="79a29-185">Replace everything in this file with the following:</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

<span data-ttu-id="79a29-186">Il existe plusieurs façons d’obtenir jQuery.</span><span class="sxs-lookup"><span data-stu-id="79a29-186">There are several ways to get jQuery.</span></span> <span data-ttu-id="79a29-187">Dans cet exemple, j’ai utilisé le [CDN Microsoft Ajax](../../../ajax/cdn/overview.md).</span><span class="sxs-lookup"><span data-stu-id="79a29-187">In this example, I used the [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span></span> <span data-ttu-id="79a29-188">Vous pouvez également le télécharger à partir de [ http://jquery.com/ ](http://jquery.com/)et ASP.NET « API Web » modèle de projet inclut également de jQuery.</span><span class="sxs-lookup"><span data-stu-id="79a29-188">You can also download it from [http://jquery.com/](http://jquery.com/), and the ASP.NET "Web API" project template includes jQuery as well.</span></span>

### <a name="getting-a-list-of-products"></a><span data-ttu-id="79a29-189">Obtention d’une liste de produits</span><span class="sxs-lookup"><span data-stu-id="79a29-189">Getting a List of Products</span></span>

<span data-ttu-id="79a29-190">Pour obtenir une liste de produits, envoyez une requête HTTP GET à &quot;/api/produits&quot;.</span><span class="sxs-lookup"><span data-stu-id="79a29-190">To get a list of products, send an HTTP GET request to &quot;/api/products&quot;.</span></span>

<span data-ttu-id="79a29-191">Le jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) fonction envoie une requête AJAX.</span><span class="sxs-lookup"><span data-stu-id="79a29-191">The jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) function sends an AJAX request.</span></span> <span data-ttu-id="79a29-192">Pour la réponse contient un tableau d’objets JSON.</span><span class="sxs-lookup"><span data-stu-id="79a29-192">For response contains array of JSON objects.</span></span> <span data-ttu-id="79a29-193">Le `done` fonction spécifie un rappel qui est appelé si la demande réussit.</span><span class="sxs-lookup"><span data-stu-id="79a29-193">The `done` function specifies a callback that is called if the request succeeds.</span></span> <span data-ttu-id="79a29-194">Dans le rappel, nous mettre à jour le modèle DOM avec les informations de produit.</span><span class="sxs-lookup"><span data-stu-id="79a29-194">In the callback, we update the DOM with the product information.</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a><span data-ttu-id="79a29-195">Obtention d’un produit par ID</span><span class="sxs-lookup"><span data-stu-id="79a29-195">Getting a Product By ID</span></span>

<span data-ttu-id="79a29-196">Pour obtenir un produit par ID, envoyez une requête HTTP GET à &quot;/API/produits/*id*&quot;, où *id* est l’ID de produit.</span><span class="sxs-lookup"><span data-stu-id="79a29-196">To get a product by ID, send an HTTP GET request to &quot;/api/products/*id*&quot;, where *id* is the product ID.</span></span>

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

<span data-ttu-id="79a29-197">Nous appelons toujours `getJSON` pour envoyer la requête AJAX, mais cette fois, nous avons placé le code dans l’URI de demande.</span><span class="sxs-lookup"><span data-stu-id="79a29-197">We still call `getJSON` to send the AJAX request, but this time we put the ID in the request URI.</span></span> <span data-ttu-id="79a29-198">La réponse à partir de cette demande est une représentation JSON d’un produit unique.</span><span class="sxs-lookup"><span data-stu-id="79a29-198">The response from this request is a JSON representation of a single product.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="79a29-199">Exécution de l'application</span><span class="sxs-lookup"><span data-stu-id="79a29-199">Running the Application</span></span>

<span data-ttu-id="79a29-200">Appuyez sur F5 pour démarrer le débogage de l’application.</span><span class="sxs-lookup"><span data-stu-id="79a29-200">Press F5 to start debugging the application.</span></span> <span data-ttu-id="79a29-201">La page web doit ressembler à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="79a29-201">The web page should look like the following:</span></span>

![](tutorial-your-first-web-api/_static/image11.png)

<span data-ttu-id="79a29-202">Pour obtenir un produit par ID, entrez l’ID et cliquez sur Rechercher :</span><span class="sxs-lookup"><span data-stu-id="79a29-202">To get a product by ID, enter the ID and click Search:</span></span>

![](tutorial-your-first-web-api/_static/image12.png)

<span data-ttu-id="79a29-203">Si vous entrez un ID non valide, le serveur renvoie une erreur HTTP :</span><span class="sxs-lookup"><span data-stu-id="79a29-203">If you enter an invalid ID, the server returns an HTTP error:</span></span>

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a><span data-ttu-id="79a29-204">À l’aide de la touche F12 pour afficher la requête et réponse HTTP</span><span class="sxs-lookup"><span data-stu-id="79a29-204">Using F12 to View the HTTP Request and Response</span></span>

<span data-ttu-id="79a29-205">Lorsque vous travaillez avec un service HTTP, il peut être très utile pour voir la requête HTTP et les messages de demande.</span><span class="sxs-lookup"><span data-stu-id="79a29-205">When you are working with an HTTP service, it can be very useful to see the HTTP request and request messages.</span></span> <span data-ttu-id="79a29-206">Vous pouvez le faire en utilisant les outils de développement F12 dans Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="79a29-206">You can do this by using the F12 developer tools in Internet Explorer 9.</span></span> <span data-ttu-id="79a29-207">À partir d’Internet Explorer 9, appuyez sur **F12** pour ouvrir les outils.</span><span class="sxs-lookup"><span data-stu-id="79a29-207">From Internet Explorer 9, press **F12** to open the tools.</span></span> <span data-ttu-id="79a29-208">Cliquez sur le **réseau** onglet, puis appuyez sur **démarrer capture**.</span><span class="sxs-lookup"><span data-stu-id="79a29-208">Click the **Network** tab and press **Start Capturing**.</span></span> <span data-ttu-id="79a29-209">Revenez maintenant à la page web et appuyez sur **F5** pour recharger la page web.</span><span class="sxs-lookup"><span data-stu-id="79a29-209">Now go back to the web page and press **F5** to reload the web page.</span></span> <span data-ttu-id="79a29-210">Internet Explorer capture le trafic HTTP entre le navigateur et le serveur web.</span><span class="sxs-lookup"><span data-stu-id="79a29-210">Internet Explorer will capture the HTTP traffic between the browser and the web server.</span></span> <span data-ttu-id="79a29-211">La vue résumée affiche tout le trafic réseau pour une page :</span><span class="sxs-lookup"><span data-stu-id="79a29-211">The summary view shows all the network traffic for a page:</span></span>

![](tutorial-your-first-web-api/_static/image14.png)

<span data-ttu-id="79a29-212">Recherchez l’entrée pour l’URI relative « api/products / ».</span><span class="sxs-lookup"><span data-stu-id="79a29-212">Locate the entry for the relative URI "api/products/".</span></span> <span data-ttu-id="79a29-213">Sélectionnez cette entrée et cliquez sur **vue détaillée**.</span><span class="sxs-lookup"><span data-stu-id="79a29-213">Select this entry and click **Go to detailed view**.</span></span> <span data-ttu-id="79a29-214">Dans la vue détail, il existe onglets pour afficher les en-têtes de demande et de réponse et un corps.</span><span class="sxs-lookup"><span data-stu-id="79a29-214">In the detail view, there are tabs to view the request and response headers and bodies.</span></span> <span data-ttu-id="79a29-215">Par exemple, si vous cliquez sur le **en-têtes de demande** onglet, vous pouvez voir que le client a demandé &quot;application/json&quot; dans l’en-tête Accept.</span><span class="sxs-lookup"><span data-stu-id="79a29-215">For example, if you click the **Request headers** tab, you can see that the client requested &quot;application/json&quot; in the Accept header.</span></span>

![](tutorial-your-first-web-api/_static/image15.png)

<span data-ttu-id="79a29-216">Si vous cliquez sur l’onglet du corps de réponse, vous pouvez voir comment la liste des produits a été sérialisée en JSON.</span><span class="sxs-lookup"><span data-stu-id="79a29-216">If you click the Response body tab, you can see how the product list was serialized to JSON.</span></span> <span data-ttu-id="79a29-217">D’autres navigateurs ont des fonctionnalités similaires.</span><span class="sxs-lookup"><span data-stu-id="79a29-217">Other browsers have similar functionality.</span></span> <span data-ttu-id="79a29-218">Est un autre outil utile [Fiddler](http://www.fiddler2.com/fiddler2/), un proxy de débogage web.</span><span class="sxs-lookup"><span data-stu-id="79a29-218">Another useful tool is [Fiddler](http://www.fiddler2.com/fiddler2/), a web debugging proxy.</span></span> <span data-ttu-id="79a29-219">Vous pouvez utiliser Fiddler pour afficher le trafic HTTP et également pour composer des requêtes HTTP, ce qui vous donne un contrôle total sur les en-têtes HTTP dans la demande.</span><span class="sxs-lookup"><span data-stu-id="79a29-219">You can use Fiddler to view your HTTP traffic, and also to compose HTTP requests, which gives you full control over the HTTP headers in the request.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="79a29-220">Consultez cette application en cours d’exécution sur Azure</span><span class="sxs-lookup"><span data-stu-id="79a29-220">See this App Running on Azure</span></span>

<span data-ttu-id="79a29-221">Vous souhaitez voir le site terminé en cours d’exécution en tant qu’une application web en direct ?</span><span class="sxs-lookup"><span data-stu-id="79a29-221">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="79a29-222">Vous pouvez déployer une version complète de l’application à votre compte Azure en cliquant simplement sur le bouton suivant.</span><span class="sxs-lookup"><span data-stu-id="79a29-222">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

<span data-ttu-id="79a29-223">Vous avez besoin d’un compte Azure pour déployer cette solution sur Azure.</span><span class="sxs-lookup"><span data-stu-id="79a29-223">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="79a29-224">Si vous n’avez pas déjà un compte, vous disposez des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="79a29-224">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="79a29-225">[Ouvrir un compte Azure gratuit](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -vous bénéficiez de crédits que vous pouvez utiliser pour tester les services Azure payants et même lorsqu’ils sont épuisés, vous pouvez conserver le compte et utiliser les services Azure gratuits.</span><span class="sxs-lookup"><span data-stu-id="79a29-225">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="79a29-226">[Activer les avantages d’abonnement MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.</span><span class="sxs-lookup"><span data-stu-id="79a29-226">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="79a29-227">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="79a29-227">Next Steps</span></span>

- <span data-ttu-id="79a29-228">Pour obtenir un exemple plus complet d’un service HTTP qui prend en charge les actions POST, PUT et DELETE et écrit dans une base de données, consultez [à l’aide de Web API 2 avec Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span><span class="sxs-lookup"><span data-stu-id="79a29-228">For a more complete example of an HTTP service that supports POST, PUT, and DELETE actions and writes to a database, see [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span></span>
- <span data-ttu-id="79a29-229">Pour plus d’informations sur la création d’applications web fluides et réactives en haut d’un service HTTP, consultez [Application à Page unique ASP.NET](../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="79a29-229">For more about creating fluid and responsive web applications on top of an HTTP service, see [ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="79a29-230">Pour plus d’informations sur la façon de déployer un projet web Visual Studio sur Azure App Service, consultez [créer une application web ASP.NET dans Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="79a29-230">For information about how to deploy a Visual Studio web project to Azure App Service, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
