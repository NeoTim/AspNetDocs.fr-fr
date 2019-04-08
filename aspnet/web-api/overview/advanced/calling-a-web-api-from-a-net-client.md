---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Appeler une API Web à partir d’un Client .NET (C#) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 11/24/2017
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 0c360f580285967c8fab8d33ccbb9557a7316ee1
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58423136"
---
<a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="d1eac-102">Appeler une API Web à partir d’un Client .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="d1eac-102">Call a Web API From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="d1eac-103">par [Mike Wasson](https://github.com/MikeWasson) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d1eac-103">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d1eac-104">[Télécharger le projet terminé](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span><span class="sxs-lookup"><span data-stu-id="d1eac-104">[Download Completed Project](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="d1eac-105">[Télécharger les instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="d1eac-105">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="d1eac-106">Ce didacticiel montre comment appeler une API web à partir d’une application .NET, à l’aide de [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="d1eac-106">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="d1eac-107">Dans ce didacticiel, une application cliente est écrit qui consomme l’API web suivante :</span><span class="sxs-lookup"><span data-stu-id="d1eac-107">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="d1eac-108">Action</span><span class="sxs-lookup"><span data-stu-id="d1eac-108">Action</span></span> | <span data-ttu-id="d1eac-109">Méthode HTTP</span><span class="sxs-lookup"><span data-stu-id="d1eac-109">HTTP method</span></span> | <span data-ttu-id="d1eac-110">URI relatif</span><span class="sxs-lookup"><span data-stu-id="d1eac-110">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d1eac-111">Obtenir un produit par ID</span><span class="sxs-lookup"><span data-stu-id="d1eac-111">Get a product by ID</span></span> | <span data-ttu-id="d1eac-112">GET</span><span class="sxs-lookup"><span data-stu-id="d1eac-112">GET</span></span> | <span data-ttu-id="d1eac-113">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="d1eac-113">/api/products/*id*</span></span> |
| <span data-ttu-id="d1eac-114">Création d’un produit</span><span class="sxs-lookup"><span data-stu-id="d1eac-114">Create a new product</span></span> | <span data-ttu-id="d1eac-115">PUBLIER</span><span class="sxs-lookup"><span data-stu-id="d1eac-115">POST</span></span> | <span data-ttu-id="d1eac-116">/ api/produits</span><span class="sxs-lookup"><span data-stu-id="d1eac-116">/api/products</span></span> |
| <span data-ttu-id="d1eac-117">Mettre à jour un produit</span><span class="sxs-lookup"><span data-stu-id="d1eac-117">Update a product</span></span> | <span data-ttu-id="d1eac-118">PUT</span><span class="sxs-lookup"><span data-stu-id="d1eac-118">PUT</span></span> | <span data-ttu-id="d1eac-119">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="d1eac-119">/api/products/*id*</span></span> |
| <span data-ttu-id="d1eac-120">Supprimer un produit</span><span class="sxs-lookup"><span data-stu-id="d1eac-120">Delete a product</span></span> | <span data-ttu-id="d1eac-121">SUPPR</span><span class="sxs-lookup"><span data-stu-id="d1eac-121">DELETE</span></span> | <span data-ttu-id="d1eac-122">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="d1eac-122">/api/products/*id*</span></span> |

<span data-ttu-id="d1eac-123">Pour savoir comment implémenter cette API avec API Web ASP.NET, consultez [d’une API Web qui prend en charge les opérations CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="d1eac-123">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="d1eac-124">Par souci de simplicité, l’application cliente dans ce didacticiel est une application de console Windows.</span><span class="sxs-lookup"><span data-stu-id="d1eac-124">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="d1eac-125">**HttpClient** est également pris en charge pour les applications Windows Phone et Windows Store.</span><span class="sxs-lookup"><span data-stu-id="d1eac-125">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="d1eac-126">Pour plus d’informations, consultez [écriture Web API Client de Code pour plusieurs plateformes utilisant les bibliothèques portables](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="d1eac-126">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="d1eac-127">Créer l’Application de Console</span><span class="sxs-lookup"><span data-stu-id="d1eac-127">Create the Console Application</span></span>

<span data-ttu-id="d1eac-128">Dans Visual Studio, créez une nouvelle application de console Windows nommée **HttpClientSample** et collez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="d1eac-128">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="d1eac-129">Le code précédent est l’application cliente complète.</span><span class="sxs-lookup"><span data-stu-id="d1eac-129">The preceding code is the complete client app.</span></span>

<span data-ttu-id="d1eac-130">`RunAsync` exécutions et bloque jusqu'à ce qu’elle se termine.</span><span class="sxs-lookup"><span data-stu-id="d1eac-130">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="d1eac-131">La plupart des **HttpClient** méthodes sont asynchrones, car ils effectuent des e/s réseau.</span><span class="sxs-lookup"><span data-stu-id="d1eac-131">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="d1eac-132">Toutes les tâches async sont effectuées à l’intérieur de `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="d1eac-132">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="d1eac-133">Normalement une application ne bloque pas le thread principal, mais aucune interaction n’autorise pas cette application.</span><span class="sxs-lookup"><span data-stu-id="d1eac-133">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="d1eac-134">Installer les bibliothèques de Client de l’API Web</span><span class="sxs-lookup"><span data-stu-id="d1eac-134">Install the Web API Client Libraries</span></span>

<span data-ttu-id="d1eac-135">Utilisez le Gestionnaire de Package NuGet pour installer le package de bibliothèques de Client d’API Web.</span><span class="sxs-lookup"><span data-stu-id="d1eac-135">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="d1eac-136">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="d1eac-136">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="d1eac-137">Dans le Package Manager de la console, tapez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d1eac-137">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="d1eac-138">La commande précédente ajoute les packages NuGet suivants au projet :</span><span class="sxs-lookup"><span data-stu-id="d1eac-138">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="d1eac-139">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="d1eac-139">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="d1eac-140">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="d1eac-140">Newtonsoft.Json</span></span>

<span data-ttu-id="d1eac-141">Json.NET est une infrastructure JSON de hautes performances populaire pour .NET.</span><span class="sxs-lookup"><span data-stu-id="d1eac-141">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="d1eac-142">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="d1eac-142">Add a Model Class</span></span>

<span data-ttu-id="d1eac-143">Examiner la classe `Product` :</span><span class="sxs-lookup"><span data-stu-id="d1eac-143">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="d1eac-144">Cette classe correspond au modèle de données utilisé par l’API web.</span><span class="sxs-lookup"><span data-stu-id="d1eac-144">This class matches the data model used by the web API.</span></span> <span data-ttu-id="d1eac-145">Une application peut utiliser **HttpClient** pour lire un `Product` instance à partir d’une réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="d1eac-145">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="d1eac-146">L’application n’a pas à écrire de code de la désérialisation.</span><span class="sxs-lookup"><span data-stu-id="d1eac-146">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="d1eac-147">Créer et initialiser HttpClient</span><span class="sxs-lookup"><span data-stu-id="d1eac-147">Create and Initialize HttpClient</span></span>

<span data-ttu-id="d1eac-148">Examinez la méthode statique **HttpClient** propriété :</span><span class="sxs-lookup"><span data-stu-id="d1eac-148">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="d1eac-149">**HttpClient** est destinée à être instancié une seule fois et réutilisées tout au long de la durée de vie d’une application.</span><span class="sxs-lookup"><span data-stu-id="d1eac-149">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="d1eac-150">Les conditions suivantes peuvent entraîner **SocketException** erreurs :</span><span class="sxs-lookup"><span data-stu-id="d1eac-150">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="d1eac-151">Création d’un nouveau **HttpClient** instance par demande.</span><span class="sxs-lookup"><span data-stu-id="d1eac-151">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="d1eac-152">Serveur sous une charge importante.</span><span class="sxs-lookup"><span data-stu-id="d1eac-152">Server under heavy load.</span></span>

<span data-ttu-id="d1eac-153">Création d’un nouveau **HttpClient** instance par demande peut épuiser les sockets disponibles.</span><span class="sxs-lookup"><span data-stu-id="d1eac-153">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="d1eac-154">Le code suivant initialise le **HttpClient** instance :</span><span class="sxs-lookup"><span data-stu-id="d1eac-154">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="d1eac-155">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="d1eac-155">The preceding code:</span></span>

* <span data-ttu-id="d1eac-156">Définit l’URI de base pour les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="d1eac-156">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="d1eac-157">Modifier le numéro de port au port utilisé dans l’application serveur.</span><span class="sxs-lookup"><span data-stu-id="d1eac-157">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="d1eac-158">L’application ne fonctionne pas si le port de l’application serveur est utilisé.</span><span class="sxs-lookup"><span data-stu-id="d1eac-158">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="d1eac-159">Définit l’en-tête Accept par « application/json ».</span><span class="sxs-lookup"><span data-stu-id="d1eac-159">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="d1eac-160">Définition de cet en-tête indique le serveur pour envoyer des données au format JSON.</span><span class="sxs-lookup"><span data-stu-id="d1eac-160">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="d1eac-161">Envoie une requête GET pour récupérer une ressource</span><span class="sxs-lookup"><span data-stu-id="d1eac-161">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="d1eac-162">Le code suivant envoie une demande GET pour un produit :</span><span class="sxs-lookup"><span data-stu-id="d1eac-162">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="d1eac-163">Le **GetAsync** méthode envoie la requête HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="d1eac-163">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="d1eac-164">Quand la méthode se termine, elle retourne un **HttpResponseMessage** qui contient la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="d1eac-164">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="d1eac-165">Si le code d’état dans la réponse est un code de réussite, le corps de réponse contient la représentation JSON d’un produit.</span><span class="sxs-lookup"><span data-stu-id="d1eac-165">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="d1eac-166">Appelez **ReadAsAsync** pour désérialiser la charge utile JSON pour un `Product` instance.</span><span class="sxs-lookup"><span data-stu-id="d1eac-166">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="d1eac-167">Le **ReadAsAsync** méthode est asynchrone, car le corps de réponse peut être arbitrairement grand.</span><span class="sxs-lookup"><span data-stu-id="d1eac-167">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="d1eac-168">**HttpClient** ne lève pas d’exception lors de la réponse HTTP contient un code d’erreur.</span><span class="sxs-lookup"><span data-stu-id="d1eac-168">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="d1eac-169">Au lieu de cela, le **IsSuccessStatusCode** propriété est **false** si l’état est un code d’erreur.</span><span class="sxs-lookup"><span data-stu-id="d1eac-169">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="d1eac-170">Si vous préférez traiter les codes d’erreur HTTP en tant qu’exceptions, appelez [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) sur l’objet de réponse.</span><span class="sxs-lookup"><span data-stu-id="d1eac-170">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="d1eac-171">`EnsureSuccessStatusCode` lève une exception si le code d’état se situe en dehors de la plage 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="d1eac-171">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="d1eac-172">Notez que **HttpClient** peut lever des exceptions pour d’autres raisons &mdash; par exemple, si la demande expire.</span><span class="sxs-lookup"><span data-stu-id="d1eac-172">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="d1eac-173">Formateurs de Type de média à désérialiser</span><span class="sxs-lookup"><span data-stu-id="d1eac-173">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="d1eac-174">Lorsque **ReadAsAsync** est appelée sans paramètres, il utilise un ensemble par défaut de *formateurs de médias* pour lire le corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="d1eac-174">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="d1eac-175">Les formateurs par défaut prend en charge JSON, XML et codée en url de formulaire de données.</span><span class="sxs-lookup"><span data-stu-id="d1eac-175">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="d1eac-176">Au lieu d’utiliser les formateurs par défaut, vous pouvez fournir une liste de formateurs à la **ReadAsAsync** (méthode).</span><span class="sxs-lookup"><span data-stu-id="d1eac-176">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="d1eac-177">À l’aide d’une liste de formateurs est utile si vous avez un formateur personnalisé de type de média :</span><span class="sxs-lookup"><span data-stu-id="d1eac-177">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="d1eac-178">Pour plus d’informations, consultez [formateurs de médias dans ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="d1eac-178">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="d1eac-179">Envoi d’une demande POST pour créer une ressource</span><span class="sxs-lookup"><span data-stu-id="d1eac-179">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="d1eac-180">Le code suivant envoie une demande POST qui contient un `Product` instance au format JSON :</span><span class="sxs-lookup"><span data-stu-id="d1eac-180">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="d1eac-181">Le **PostAsJsonAsync** méthode :</span><span class="sxs-lookup"><span data-stu-id="d1eac-181">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="d1eac-182">Sérialise un objet au format JSON.</span><span class="sxs-lookup"><span data-stu-id="d1eac-182">Serializes an object to JSON.</span></span>
* <span data-ttu-id="d1eac-183">Envoie la charge utile JSON dans une requête POST.</span><span class="sxs-lookup"><span data-stu-id="d1eac-183">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="d1eac-184">Si la demande réussit :</span><span class="sxs-lookup"><span data-stu-id="d1eac-184">If the request succeeds:</span></span>

* <span data-ttu-id="d1eac-185">Elle doit retourner la réponse 201 (créé).</span><span class="sxs-lookup"><span data-stu-id="d1eac-185">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="d1eac-186">La réponse doit inclure l’URL, les ressources créées dans l’en-tête Location.</span><span class="sxs-lookup"><span data-stu-id="d1eac-186">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="d1eac-187">Envoi d’une demande PUT pour mettre à jour une ressource</span><span class="sxs-lookup"><span data-stu-id="d1eac-187">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="d1eac-188">Le code suivant envoie une demande PUT pour mettre à jour un produit :</span><span class="sxs-lookup"><span data-stu-id="d1eac-188">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="d1eac-189">Le **PutAsJsonAsync** méthode fonctionne comme **PostAsJsonAsync**, sauf qu’il envoie une demande PUT au lieu de POST.</span><span class="sxs-lookup"><span data-stu-id="d1eac-189">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="d1eac-190">Envoi d’une demande de suppression pour supprimer une ressource</span><span class="sxs-lookup"><span data-stu-id="d1eac-190">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="d1eac-191">Le code suivant envoie une demande de suppression pour supprimer un produit :</span><span class="sxs-lookup"><span data-stu-id="d1eac-191">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="d1eac-192">Comme GET, une demande de suppression n’a pas un corps de demande.</span><span class="sxs-lookup"><span data-stu-id="d1eac-192">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="d1eac-193">Vous n’avez pas besoin de spécifier le format JSON ou XML quand la suppression.</span><span class="sxs-lookup"><span data-stu-id="d1eac-193">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="d1eac-194">Tester l’exemple</span><span class="sxs-lookup"><span data-stu-id="d1eac-194">Test the sample</span></span>

<span data-ttu-id="d1eac-195">Pour tester l’application cliente :</span><span class="sxs-lookup"><span data-stu-id="d1eac-195">To test the client app:</span></span>

1. <span data-ttu-id="d1eac-196">[Télécharger](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) et exécuter l’application serveur.</span><span class="sxs-lookup"><span data-stu-id="d1eac-196">[Download](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="d1eac-197">[Télécharger les instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="d1eac-197">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="d1eac-198">Vérifiez que l’application serveur fonctionne.</span><span class="sxs-lookup"><span data-stu-id="d1eac-198">Verify the server app is working.</span></span> <span data-ttu-id="d1eac-199">Par exemple, `http://localhost:64195/api/products` doit retourner une liste de produits.</span><span class="sxs-lookup"><span data-stu-id="d1eac-199">For example, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="d1eac-200">Définir l’URI de base pour les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="d1eac-200">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="d1eac-201">Modifier le numéro de port au port utilisé dans l’application serveur.</span><span class="sxs-lookup"><span data-stu-id="d1eac-201">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="d1eac-202">Exécutez l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="d1eac-202">Run the client app.</span></span> <span data-ttu-id="d1eac-203">La sortie suivante est produite :</span><span class="sxs-lookup"><span data-stu-id="d1eac-203">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
