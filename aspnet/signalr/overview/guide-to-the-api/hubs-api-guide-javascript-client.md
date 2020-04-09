---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: ASP.NET SignalR Hubs API Guide - JavaScript Client (fr) Microsoft Docs
author: bradygaster
description: Ce document fournit une introduction à l’utilisation de l’API Hubs pour la version SignalR 2 dans les clients JavaScript, tels que les navigateurs et Windows Store (WinJS) applicat ...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 8befe133c3627dac1f7d011959c68e2054d345da
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676082"
---
# <a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="6965d-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span><span class="sxs-lookup"><span data-stu-id="6965d-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="6965d-104">Ce document fournit une introduction à l’utilisation de l’API Hubs pour la version SignalR 2 dans les clients JavaScript, tels que les navigateurs et les applications Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="6965d-104">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
>
> <span data-ttu-id="6965d-105">L’API SignalR Hubs vous permet de passer des appels de procédure à distance (CRP) d’un serveur à des clients connectés et des clients au serveur.</span><span class="sxs-lookup"><span data-stu-id="6965d-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="6965d-106">Dans le code serveur, vous définissez les méthodes qui peuvent être appelées par les clients, et vous appelez les méthodes qui s’exécutent sur le client.</span><span class="sxs-lookup"><span data-stu-id="6965d-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="6965d-107">Dans le code client, vous définissez les méthodes qui peuvent être appelées à partir du serveur, et vous appelez les méthodes qui s’exécutent sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="6965d-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="6965d-108">SignalR s’occupe de toute la plomberie client-serveur pour vous.</span><span class="sxs-lookup"><span data-stu-id="6965d-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="6965d-109">SignalR offre également une API de niveau inférieur appelée Connexions Persistantes.</span><span class="sxs-lookup"><span data-stu-id="6965d-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="6965d-110">Pour une introduction à SignalR, Hubs, et persistent Connexions, voir [Introduction à SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="6965d-110">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="6965d-111">Versions logicielles utilisées dans ce sujet</span><span class="sxs-lookup"><span data-stu-id="6965d-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="6965d-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="6965d-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="6965d-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6965d-113">.NET 4.5</span></span>
> - <span data-ttu-id="6965d-114">Version SignalR 2</span><span class="sxs-lookup"><span data-stu-id="6965d-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="6965d-115">Versions précédentes de ce sujet</span><span class="sxs-lookup"><span data-stu-id="6965d-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="6965d-116">Pour plus d’informations sur les versions antérieures de SignalR, voir [SignalR Older Versions](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="6965d-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="6965d-117">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="6965d-117">Questions and comments</span></span>
>
> <span data-ttu-id="6965d-118">S’il vous plaît laisser des commentaires sur la façon dont vous avez aimé ce tutoriel et ce que nous pourrions améliorer dans les commentaires au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="6965d-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="6965d-119">Si vous avez des questions qui ne sont pas directement liées au tutoriel, vous pouvez les poster sur le [ASP.NET Forum SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="6965d-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="6965d-120">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="6965d-120">Overview</span></span>

<span data-ttu-id="6965d-121">Ce document contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="6965d-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="6965d-122">Le proxy généré et ce qu’il fait pour vous</span><span class="sxs-lookup"><span data-stu-id="6965d-122">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="6965d-123">Quand utiliser le proxy généré</span><span class="sxs-lookup"><span data-stu-id="6965d-123">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="6965d-124">Configuration du client</span><span class="sxs-lookup"><span data-stu-id="6965d-124">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="6965d-125">Comment faire référence au proxy généré dynamiquement</span><span class="sxs-lookup"><span data-stu-id="6965d-125">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="6965d-126">Comment créer un fichier physique pour le proxy généré par SignalR</span><span class="sxs-lookup"><span data-stu-id="6965d-126">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="6965d-127">Comment établir une connexion</span><span class="sxs-lookup"><span data-stu-id="6965d-127">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="6965d-128">$.connection.hub est le même objet que $.hubConnection() crée</span><span class="sxs-lookup"><span data-stu-id="6965d-128">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="6965d-129">Exécution asynchrone de la méthode de départ</span><span class="sxs-lookup"><span data-stu-id="6965d-129">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="6965d-130">Comment établir une connexion inter-domaines</span><span class="sxs-lookup"><span data-stu-id="6965d-130">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="6965d-131">Comment configurer la connexion</span><span class="sxs-lookup"><span data-stu-id="6965d-131">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="6965d-132">Comment spécifier les paramètres de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="6965d-132">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="6965d-133">Comment spécifier le mode de transport</span><span class="sxs-lookup"><span data-stu-id="6965d-133">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="6965d-134">Comment obtenir un proxy pour une classe Hub</span><span class="sxs-lookup"><span data-stu-id="6965d-134">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="6965d-135">Comment définir les méthodes sur le client que le serveur peut appeler</span><span class="sxs-lookup"><span data-stu-id="6965d-135">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="6965d-136">Comment appeler les méthodes du serveur du client</span><span class="sxs-lookup"><span data-stu-id="6965d-136">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="6965d-137">Comment gérer les événements de connexion à vie</span><span class="sxs-lookup"><span data-stu-id="6965d-137">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="6965d-138">Comment gérer les erreurs</span><span class="sxs-lookup"><span data-stu-id="6965d-138">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="6965d-139">Comment permettre l’enregistrement côté client</span><span class="sxs-lookup"><span data-stu-id="6965d-139">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="6965d-140">Pour obtenir de la documentation sur la façon de programmer le serveur ou les clients .NET, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="6965d-140">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="6965d-141">Guide d’API SignalR Hubs - Serveur</span><span class="sxs-lookup"><span data-stu-id="6965d-141">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="6965d-142">Guide d’API SignalR Hubs - .NET Client</span><span class="sxs-lookup"><span data-stu-id="6965d-142">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="6965d-143">Le composant serveur SignalR 2 n’est disponible que sur .NET 4.5 (bien qu’il existe un client .NET pour SignalR 2 sur .NET 4.0).</span><span class="sxs-lookup"><span data-stu-id="6965d-143">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="6965d-144">Le proxy généré et ce qu’il fait pour vous</span><span class="sxs-lookup"><span data-stu-id="6965d-144">The generated proxy and what it does for you</span></span>

<span data-ttu-id="6965d-145">Vous pouvez programmer un client JavaScript pour communiquer avec un service SignalR avec ou sans proxy que SignalR génère pour vous.</span><span class="sxs-lookup"><span data-stu-id="6965d-145">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="6965d-146">Ce que le proxy fait pour vous est de simplifier la syntaxe du code que vous utilisez pour connecter, écrire des méthodes que le serveur appelle, et les méthodes d’appel sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="6965d-146">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="6965d-147">Lorsque vous écrivez du code pour appeler les méthodes du serveur, le proxy généré vous permet `serverMethod(arg1, arg2)` d’utiliser la syntaxe qui semble exécuter une fonction locale: vous pouvez écrire au lieu de `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="6965d-147">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="6965d-148">La syntaxe proxy générée permet également une erreur immédiate et intelligible côté client si vous maltypé un nom de méthode serveur.</span><span class="sxs-lookup"><span data-stu-id="6965d-148">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="6965d-149">Et si vous créez manuellement le fichier qui définit les procurations, vous pouvez également obtenir le support IntelliSense pour écrire du code qui appelle les méthodes du serveur.</span><span class="sxs-lookup"><span data-stu-id="6965d-149">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="6965d-150">Supposons, par exemple, que vous ayez la classe Hub suivante sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="6965d-150">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="6965d-151">Les exemples de code suivants montrent à quoi `NewContosoChatMessage` ressemble le code JavaScript pour `addContosoChatMessageToPage` invoquer la méthode sur le serveur et recevoir des invocations de la méthode à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="6965d-151">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="6965d-152">**Avec le proxy généré**</span><span class="sxs-lookup"><span data-stu-id="6965d-152">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="6965d-153">**Sans le proxy généré**</span><span class="sxs-lookup"><span data-stu-id="6965d-153">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="6965d-154">Quand utiliser le proxy généré</span><span class="sxs-lookup"><span data-stu-id="6965d-154">When to use the generated proxy</span></span>

<span data-ttu-id="6965d-155">Si vous souhaitez enregistrer plusieurs gestionnaires d’événements pour une méthode client que le serveur appelle, vous ne pouvez pas utiliser le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="6965d-155">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="6965d-156">Sinon, vous pouvez choisir d’utiliser le proxy généré ou non en fonction de votre préférence de codage.</span><span class="sxs-lookup"><span data-stu-id="6965d-156">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="6965d-157">Si vous choisissez de ne pas l’utiliser, vous n’avez pas à `script` référencer l’URL "signaleur/hubs" dans un élément de votre code client.</span><span class="sxs-lookup"><span data-stu-id="6965d-157">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="6965d-158">Configuration cliente</span><span class="sxs-lookup"><span data-stu-id="6965d-158">Client setup</span></span>

<span data-ttu-id="6965d-159">Un client JavaScript nécessite des références à jQuery et au fichier JavaScript de base SignalR.</span><span class="sxs-lookup"><span data-stu-id="6965d-159">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="6965d-160">La version jQuery doit être 1.6.4 ou les versions plus importantes ultérieures, telles que 1.7.2, 1.8.2, ou 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="6965d-160">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="6965d-161">Si vous décidez d’utiliser le proxy généré, vous avez également besoin d’une référence au fichier JavaScript proxy généré par SignalR.</span><span class="sxs-lookup"><span data-stu-id="6965d-161">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="6965d-162">L’exemple suivant montre à quoi pourraient ressembler les références dans une page HTML qui utilise le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="6965d-162">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="6965d-163">Ces références doivent être incluses dans cet ordre : jQuery d’abord, noyau SignalR après cela, et les procurations SignalR durent.</span><span class="sxs-lookup"><span data-stu-id="6965d-163">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="6965d-164">Comment faire référence au proxy généré dynamiquement</span><span class="sxs-lookup"><span data-stu-id="6965d-164">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="6965d-165">Dans l’exemple précédent, la référence au proxy généré par SignalR est de générer dynamiquement le code JavaScript, et non vers un fichier physique.</span><span class="sxs-lookup"><span data-stu-id="6965d-165">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="6965d-166">SignalR crée le code JavaScript pour le proxy à la volée et le sert au client en réponse à l’URL "/signaleur/hubs".</span><span class="sxs-lookup"><span data-stu-id="6965d-166">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="6965d-167">Si vous avez spécifié une URL de base `MapSignalR` différente pour les connexions SignalR sur le serveur dans votre méthode, l’URL du fichier proxy généré dynamiquement est votre URL personnalisée avec des "/hubs" qui lui sont annexés.</span><span class="sxs-lookup"><span data-stu-id="6965d-167">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="6965d-168">Pour les clients JavaScript de Windows 8 (Windows Store), utilisez le fichier proxy physique au lieu de celui généré dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="6965d-168">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="6965d-169">Pour plus d’informations, voir [Comment créer un fichier physique pour le proxy généré par SignalR](#manualproxy) plus tard dans ce sujet.</span><span class="sxs-lookup"><span data-stu-id="6965d-169">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>

<span data-ttu-id="6965d-170">Dans une ASP.NET vue MVC 4 ou 5 Razor, utilisez le tilde pour désigner la racine d’application dans votre référence de fichier proxy :</span><span class="sxs-lookup"><span data-stu-id="6965d-170">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="6965d-171">Pour plus d’informations sur l’utilisation de SignalR dans MVC 5, voir [Getting Started with SignalR et MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="6965d-171">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="6965d-172">Dans une ASP.NET vue de rasoir MVC `Url.Content` 3, utilisez pour votre référence de fichier proxy :</span><span class="sxs-lookup"><span data-stu-id="6965d-172">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="6965d-173">Dans une application ASP.NET Web Forms, utilisez `ResolveClientUrl` votre référence de fichier de procurations ou enregistrez-la via le ScriptManager à l’aide d’un chemin relatif à la racine de l’application (à commencer par un tilde) :</span><span class="sxs-lookup"><span data-stu-id="6965d-173">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="6965d-174">En règle générale, utilisez la même méthode pour spécifier l’URL "/signaleur/hubs" que vous utilisez pour les fichiers CSS ou JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6965d-174">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="6965d-175">Si vous spécifiez une URL sans utiliser de tilde, dans certains scénarios, votre application fonctionnera correctement lorsque vous testez dans Visual Studio à l’aide d’IIS Express, mais échouera avec une erreur 404 lorsque vous déployez à l’IIS complète.</span><span class="sxs-lookup"><span data-stu-id="6965d-175">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="6965d-176">Pour plus d’informations, voir **Résoudre les références aux ressources de niveau racine** dans les serveurs Web dans Visual Studio pour ASP.NET [projets Web](https://msdn.microsoft.com/library/58wxa9w5.aspx) sur le site MSDN.</span><span class="sxs-lookup"><span data-stu-id="6965d-176">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="6965d-177">Lorsque vous exécutez un projet web dans Visual Studio 2017 en mode débogé, et si vous utilisez Internet Explorer comme navigateur, vous pouvez voir le fichier proxy dans **Solution Explorer** sous **Scripts**.</span><span class="sxs-lookup"><span data-stu-id="6965d-177">When you run a web project in Visual Studio 2017 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Scripts**.</span></span>

<span data-ttu-id="6965d-178">Pour voir le contenu du fichier, double clic **hubs**.</span><span class="sxs-lookup"><span data-stu-id="6965d-178">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="6965d-179">Si vous n’utilisez pas Visual Studio 2012 ou 2013 et Internet Explorer, ou si vous n’êtes pas en mode débogé, vous pouvez également obtenir le contenu du fichier en naviguant vers l’URL "/signalR/hubs".</span><span class="sxs-lookup"><span data-stu-id="6965d-179">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="6965d-180">Par exemple, si votre `http://localhost:56699`site est `http://localhost:56699/SignalR/hubs` en cours d’exécution à , aller à dans votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="6965d-180">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="6965d-181">Comment créer un fichier physique pour le proxy généré par SignalR</span><span class="sxs-lookup"><span data-stu-id="6965d-181">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="6965d-182">Comme alternative au proxy généré dynamiquement, vous pouvez créer un fichier physique qui a le code proxy et la référence de ce fichier.</span><span class="sxs-lookup"><span data-stu-id="6965d-182">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="6965d-183">Vous voudrez peut-être le faire pour le contrôle sur la mise en cache ou le comportement de regroupement, ou pour obtenir IntelliSense lorsque vous codez des appels vers des méthodes de serveur.</span><span class="sxs-lookup"><span data-stu-id="6965d-183">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="6965d-184">Pour créer un fichier proxy, effectuez les étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="6965d-184">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="6965d-185">Installez le package [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet.</span><span class="sxs-lookup"><span data-stu-id="6965d-185">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="6965d-186">Ouvrez une invite de commande et naviguez vers le dossier *d’outils* qui contient le fichier SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="6965d-186">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="6965d-187">Le dossier d’outils se trouve à l’endroit suivant :</span><span class="sxs-lookup"><span data-stu-id="6965d-187">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="6965d-188">Entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="6965d-188">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="6965d-189">Le chemin vers votre *.dll* est généralement le dossier *bin* dans votre dossier de projet.</span><span class="sxs-lookup"><span data-stu-id="6965d-189">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="6965d-190">Cette commande crée un fichier nommé *server.js* dans le même dossier que *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="6965d-190">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="6965d-191">Mettez le fichier *server.js* dans un dossier approprié dans votre projet, renommez-le le comme approprié pour votre application, et ajoutez une référence à celui-ci à la place de la référence "signaleur / hubs".</span><span class="sxs-lookup"><span data-stu-id="6965d-191">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="6965d-192">Comment établir une connexion</span><span class="sxs-lookup"><span data-stu-id="6965d-192">How to establish a connection</span></span>

<span data-ttu-id="6965d-193">Avant de pouvoir établir une connexion, vous devez créer un objet de connexion, créer un proxy et enregistrer les gestionnaires d’événements pour les méthodes qui peuvent être appelées à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="6965d-193">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="6965d-194">Lorsque les gestionnaires de procuration et d’événement sont `start` configurés, établissez la connexion en appelant la méthode.</span><span class="sxs-lookup"><span data-stu-id="6965d-194">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="6965d-195">Si vous utilisez le proxy généré, vous n’avez pas à créer l’objet de connexion dans votre propre code parce que le code proxy généré le fait pour vous.</span><span class="sxs-lookup"><span data-stu-id="6965d-195">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="6965d-196">**Établir une connexion (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-196">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="6965d-197">**Établir une connexion (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-197">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="6965d-198">Le code de l’échantillon utilise l’URL par défaut "/signaleur" pour se connecter à votre service SignalR.</span><span class="sxs-lookup"><span data-stu-id="6965d-198">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="6965d-199">Pour plus d’informations sur la façon de spécifier une URL de base différente, voir [ASP.NET Guide d’API De SignalR Hubs - Serveur - L’URL /signalur](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="6965d-199">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="6965d-200">Par défaut, l’emplacement du hub est le serveur actuel; si vous vous connectez à un serveur différent, spécifiez l’URL avant d’appeler la `start` méthode, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="6965d-200">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="6965d-201">Normalement, vous enregistrez les `start` gestionnaires d’événements avant d’appeler la méthode pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="6965d-201">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="6965d-202">Si vous souhaitez enregistrer certains gestionnaires d’événements après avoir établi la connexion, vous pouvez le faire, `start` mais vous devez vous inscrire au moins un de vos gestionnaires d’événements avant d’appeler la méthode.</span><span class="sxs-lookup"><span data-stu-id="6965d-202">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="6965d-203">Une raison pour cela est qu’il peut y avoir de nombreux Hubs dans une application, mais vous ne voudriez pas déclencher l’événement `OnConnected` sur chaque Hub si vous allez seulement utiliser à l’un d’eux.</span><span class="sxs-lookup"><span data-stu-id="6965d-203">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="6965d-204">Lorsque la connexion est établie, la présence d’une méthode client sur le `OnConnected` proxy d’un Hub est ce qui indique à SignalR de déclencher l’événement.</span><span class="sxs-lookup"><span data-stu-id="6965d-204">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="6965d-205">Si vous n’enregistrez pas les `start` gestionnaires d’événements avant d’appeler la méthode, vous `OnConnected` serez en mesure d’invoquer des méthodes sur le Hub, mais la méthode du Hub ne sera pas appelée et aucune méthode client ne sera invoquée à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="6965d-205">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="6965d-206">$.connection.hub est le même objet que $.hubConnection() crée</span><span class="sxs-lookup"><span data-stu-id="6965d-206">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="6965d-207">Comme vous pouvez le voir dans les exemples, lorsque vous utilisez le proxy généré, `$.connection.hub` se réfère à l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="6965d-207">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="6965d-208">C’est le même objet `$.hubConnection()` que vous obtenez en appelant lorsque vous n’utilisez pas le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="6965d-208">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="6965d-209">Le code proxy généré crée la connexion pour vous en exécutant la déclaration suivante :</span><span class="sxs-lookup"><span data-stu-id="6965d-209">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Création d’une connexion dans le fichier proxy généré](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="6965d-211">Lorsque vous utilisez le proxy généré, vous `$.connection.hub` pouvez faire n’importe quoi avec ce que vous pouvez faire avec un objet de connexion lorsque vous n’utilisez pas le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="6965d-211">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="6965d-212">Exécution asynchrone de la méthode de départ</span><span class="sxs-lookup"><span data-stu-id="6965d-212">Asynchronous execution of the start method</span></span>

<span data-ttu-id="6965d-213">La `start` méthode s’exécute asynchronement.</span><span class="sxs-lookup"><span data-stu-id="6965d-213">The `start` method executes asynchronously.</span></span> <span data-ttu-id="6965d-214">Il renvoie un [objet différé jQuery](http://api.jquery.com/category/deferred-object/), ce qui signifie que `pipe`vous `done`pouvez `fail`ajouter des fonctions de rappel en appelant des méthodes telles que , , et .</span><span class="sxs-lookup"><span data-stu-id="6965d-214">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="6965d-215">Si vous avez du code que vous souhaitez exécuter après la mise en place de la connexion, comme un appel à une méthode serveur, mettez ce code dans une fonction de rappel ou appelez-le à partir d’une fonction de rappel.</span><span class="sxs-lookup"><span data-stu-id="6965d-215">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="6965d-216">La `.done` méthode de rappel est exécutée après la connexion a été `OnConnected` établie, et après tout code que vous avez dans votre méthode de gestionnaire d’événement sur le serveur termine l’exécution.</span><span class="sxs-lookup"><span data-stu-id="6965d-216">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="6965d-217">Si vous mettez l’instruction "Maintenant connectée" de l’exemple `start` précédent comme la `.done` prochaine ligne `console.log` de code après l’appel de méthode (pas dans un rappel), la ligne s’exécutera avant que la connexion soit établie, comme indiqué dans l’exemple suivant:</span><span class="sxs-lookup"><span data-stu-id="6965d-217">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Mauvaise façon d’écrire du code qui s’exécute après la connexion est établie](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="6965d-219">Comment établir une connexion inter-domaines</span><span class="sxs-lookup"><span data-stu-id="6965d-219">How to establish a cross-domain connection</span></span>

<span data-ttu-id="6965d-220">Typiquement, si le navigateur `http://contoso.com`charge une page à partir de `http://contoso.com/signalr`, la connexion SignalR est dans le même domaine, à .</span><span class="sxs-lookup"><span data-stu-id="6965d-220">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="6965d-221">Si la `http://contoso.com` page de `http://fabrikam.com/signalr`fait une connexion à , c’est une connexion cross-domain.</span><span class="sxs-lookup"><span data-stu-id="6965d-221">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="6965d-222">Pour des raisons de sécurité, les connexions inter-domaines sont désactivées par défaut.</span><span class="sxs-lookup"><span data-stu-id="6965d-222">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="6965d-223">Dans SignalR 1.x, les demandes de domaine transversal ont été contrôlées par un seul drapeau EnableCrossDomain.</span><span class="sxs-lookup"><span data-stu-id="6965d-223">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="6965d-224">Ce drapeau contrôlait à la fois les demandes de JSONP et de CORS.</span><span class="sxs-lookup"><span data-stu-id="6965d-224">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="6965d-225">Pour une plus grande flexibilité, tout le support CORS a été supprimé de la composante serveur de SignalR (les clients JavaScript utilisent toujours CORS normalement s’il est détecté que le navigateur le prend en charge), et de nouveaux middleware OWIN a été mis à disposition pour prendre en charge ces scénarios.</span><span class="sxs-lookup"><span data-stu-id="6965d-225">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="6965d-226">Si JSONP est nécessaire sur le client (pour prendre en charge les demandes de `EnableJSONP` cross-domain dans les anciens navigateurs), il devra être activé explicitement en définissant sur l’objet `HubConfiguration` à `true`, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="6965d-226">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="6965d-227">JSONP est désactivé par défaut, car il est moins sûr que CORS.</span><span class="sxs-lookup"><span data-stu-id="6965d-227">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="6965d-228">**Ajout de Microsoft.Owin.Cors à votre projet :** Pour installer cette bibliothèque, exécutez la commande suivante dans la console Package Manager :</span><span class="sxs-lookup"><span data-stu-id="6965d-228">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="6965d-229">Cette commande ajoutera la version 2.1.0 du paquet à votre projet.</span><span class="sxs-lookup"><span data-stu-id="6965d-229">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="6965d-230">Appel UseCors</span><span class="sxs-lookup"><span data-stu-id="6965d-230">Calling UseCors</span></span>

 <span data-ttu-id="6965d-231">L’extrait de code suivant montre comment implémenter des connexions inter-domaines dans SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="6965d-231">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span>

<span data-ttu-id="6965d-232">**Mise en œuvre des demandes inter-domaines dans SignalR 2**</span><span class="sxs-lookup"><span data-stu-id="6965d-232">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="6965d-233">Le code suivant montre comment activer CORS ou JSONP dans un projet SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="6965d-233">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="6965d-234">Cet exemple `Map` de `RunSignalR` code `MapSignalR`utilise et au lieu de , de sorte que le middleware CORS fonctionne uniquement `MapSignalR`pour les demandes SignalR qui nécessitent une prise en charge CORS (plutôt que pour tout le trafic sur le chemin spécifié dans .) La carte peut également être utilisée pour tout autre middleware qui doit s’exécuter pour un préfixe URL spécifique, plutôt que pour l’ensemble de l’application.</span><span class="sxs-lookup"><span data-stu-id="6965d-234">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - <span data-ttu-id="6965d-235">Ne définissez `jQuery.support.cors` pas pour vrai dans votre code.</span><span class="sxs-lookup"><span data-stu-id="6965d-235">Don't set `jQuery.support.cors` to true in your code.</span></span>
>
>     ![Ne définissez pas jQuery.support.cors à vrai](hubs-api-guide-javascript-client/_static/image7.png)
>
>     <span data-ttu-id="6965d-237">SignalR gère l’utilisation de CORS.</span><span class="sxs-lookup"><span data-stu-id="6965d-237">SignalR handles the use of CORS.</span></span> <span data-ttu-id="6965d-238">Réglage `jQuery.support.cors` à vrai désactive JSONP parce qu’il provoque SignalR à assumer le navigateur prend en charge CORS.</span><span class="sxs-lookup"><span data-stu-id="6965d-238">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="6965d-239">Lorsque vous vous connectez à une URL locale, Internet Explorer 10 ne le considérera pas comme une connexion cross-domain, de sorte que l’application fonctionnera localement avec IE 10, même si vous n’avez pas activé les connexions cross-domain sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="6965d-239">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="6965d-240">Pour plus d’informations sur l’utilisation de connexions cross-domain avec Internet Explorer 9, voir [ce fil StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="6965d-240">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="6965d-241">Pour plus d’informations sur l’utilisation de connexions cross-domain avec Chrome, consultez [ce thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="6965d-241">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="6965d-242">Le code de l’échantillon utilise l’URL par défaut "/signaleur" pour se connecter à votre service SignalR.</span><span class="sxs-lookup"><span data-stu-id="6965d-242">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="6965d-243">Pour plus d’informations sur la façon de spécifier une URL de base différente, voir [ASP.NET Guide d’API De SignalR Hubs - Serveur - L’URL /signalur](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="6965d-243">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="6965d-244">Comment configurer la connexion</span><span class="sxs-lookup"><span data-stu-id="6965d-244">How to configure the connection</span></span>

<span data-ttu-id="6965d-245">Avant d’établir une connexion, vous pouvez spécifier les paramètres de chaîne de requête ou spécifier le mode de transport.</span><span class="sxs-lookup"><span data-stu-id="6965d-245">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="6965d-246">Comment spécifier les paramètres de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="6965d-246">How to specify query string parameters</span></span>

<span data-ttu-id="6965d-247">Si vous souhaitez envoyer des données au serveur lorsque le client se connecte, vous pouvez ajouter des paramètres de chaîne de requête à l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="6965d-247">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="6965d-248">Les exemples suivants montrent comment définir un paramètre de chaîne de requête dans le code client.</span><span class="sxs-lookup"><span data-stu-id="6965d-248">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="6965d-249">**Définissez une valeur de chaîne de requête avant d’appeler la méthode de démarrage (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-249">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="6965d-250">**Définissez une valeur de chaîne de requête avant d’appeler la méthode de démarrage (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-250">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="6965d-251">L’exemple suivant montre comment lire un paramètre de chaîne de requête dans le code serveur.</span><span class="sxs-lookup"><span data-stu-id="6965d-251">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="6965d-252">Comment spécifier le mode de transport</span><span class="sxs-lookup"><span data-stu-id="6965d-252">How to specify the transport method</span></span>

<span data-ttu-id="6965d-253">Dans le cadre du processus de connexion, un client SignalR négocie normalement avec le serveur pour déterminer le meilleur transport qui est pris en charge par le serveur et le client.</span><span class="sxs-lookup"><span data-stu-id="6965d-253">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="6965d-254">Si vous savez déjà quel transport vous souhaitez utiliser, vous pouvez contourner ce `start` processus de négociation en spécifiant le mode de transport lorsque vous appelez la méthode.</span><span class="sxs-lookup"><span data-stu-id="6965d-254">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="6965d-255">**Code client qui spécifie la méthode de transport (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-255">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="6965d-256">**Code client qui spécifie la méthode de transport (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-256">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="6965d-257">Comme alternative, vous pouvez spécifier plusieurs méthodes de transport dans l’ordre dans lequel vous souhaitez SignalR pour les essayer:</span><span class="sxs-lookup"><span data-stu-id="6965d-257">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="6965d-258">**Code client qui spécifie un système de repli sur mesure des transports (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-258">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="6965d-259">**Code client qui spécifie un système de repli sur mesure des transports (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-259">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="6965d-260">Vous pouvez utiliser les valeurs suivantes pour spécifier le mode de transport :</span><span class="sxs-lookup"><span data-stu-id="6965d-260">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="6965d-261">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="6965d-261">"webSockets"</span></span>
- <span data-ttu-id="6965d-262">"ForeverFrame"</span><span class="sxs-lookup"><span data-stu-id="6965d-262">"foreverFrame"</span></span>
- <span data-ttu-id="6965d-263">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="6965d-263">"serverSentEvents"</span></span>
- <span data-ttu-id="6965d-264">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="6965d-264">"longPolling"</span></span>

<span data-ttu-id="6965d-265">Les exemples suivants montrent comment savoir quelle méthode de transport est utilisée par une connexion.</span><span class="sxs-lookup"><span data-stu-id="6965d-265">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="6965d-266">**Code client qui affiche la méthode de transport utilisée par une connexion (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-266">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="6965d-267">**Code client qui affiche la méthode de transport utilisée par une connexion (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-267">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="6965d-268">Pour plus d’informations sur la façon de vérifier la méthode de transport dans le code serveur, voir [ASP.NET SignalR Hubs API Guide - Server - Comment obtenir des informations sur le client à partir de la propriété Context](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="6965d-268">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="6965d-269">Pour plus d’informations sur les transports et les repli, voir [Introduction à SignalR - Transports et Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="6965d-269">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="6965d-270">Comment obtenir un proxy pour une classe Hub</span><span class="sxs-lookup"><span data-stu-id="6965d-270">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="6965d-271">Chaque objet de connexion que vous créez encapsule des informations sur une connexion à un service SignalR qui contient une ou plusieurs classes Hub.</span><span class="sxs-lookup"><span data-stu-id="6965d-271">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="6965d-272">Pour communiquer avec une classe Hub, vous utilisez un objet proxy que vous créez vous-même (si vous n’utilisez pas le proxy généré) ou qui est généré pour vous.</span><span class="sxs-lookup"><span data-stu-id="6965d-272">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="6965d-273">Sur le client, le nom proxy est une version à dos de chameau du nom de la classe Hub.</span><span class="sxs-lookup"><span data-stu-id="6965d-273">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="6965d-274">SignalR effectue automatiquement cette modification de sorte que le code JavaScript peut se conformer aux conventions JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6965d-274">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="6965d-275">**Classe Hub sur le serveur**</span><span class="sxs-lookup"><span data-stu-id="6965d-275">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="6965d-276">**Obtenez une référence au proxy client généré pour le Hub**</span><span class="sxs-lookup"><span data-stu-id="6965d-276">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="6965d-277">**Créez un proxy client pour la classe Hub (sans proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-277">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="6965d-278">Si vous décorez votre `HubName` classe Hub avec un attribut, utilisez le nom exact sans changer de boîtier.</span><span class="sxs-lookup"><span data-stu-id="6965d-278">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="6965d-279">**Classe Hub sur serveur avec l’attribut HubName**</span><span class="sxs-lookup"><span data-stu-id="6965d-279">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="6965d-280">**Obtenez une référence au proxy client généré pour le Hub**</span><span class="sxs-lookup"><span data-stu-id="6965d-280">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="6965d-281">**Créez un proxy client pour la classe Hub (sans proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-281">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="6965d-282">Comment définir les méthodes sur le client que le serveur peut appeler</span><span class="sxs-lookup"><span data-stu-id="6965d-282">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="6965d-283">Pour définir une méthode que le serveur peut appeler à partir d’un Hub, ajoutez un gestionnaire d’événements au proxy Hub en utilisant la `client` propriété du proxy généré, ou appelez la `on` méthode si vous n’utilisez pas le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="6965d-283">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="6965d-284">Les paramètres peuvent être des objets complexes.</span><span class="sxs-lookup"><span data-stu-id="6965d-284">The parameters can be complex objects.</span></span>

<span data-ttu-id="6965d-285">Ajoutez le gestionnaire d’événements avant d’appeler la `start` méthode pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="6965d-285">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="6965d-286">(Si vous souhaitez ajouter des gestionnaires `start` d’événements après avoir appelé la méthode, voir la note dans [Comment établir une connexion](#establishconnection) plus tôt dans ce document, et utiliser la syntaxe indiquée pour définir une méthode sans utiliser le proxy généré.)</span><span class="sxs-lookup"><span data-stu-id="6965d-286">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="6965d-287">L’appariement du nom de la méthode est insensible aux cas.</span><span class="sxs-lookup"><span data-stu-id="6965d-287">Method name matching is case-insensitive.</span></span> <span data-ttu-id="6965d-288">Par `Clients.All.addContosoChatMessageToPage` exemple, sur le `AddContosoChatMessageToPage` `addContosoChatMessageToPage`serveur `addcontosochatmessagetopage` s’exécute, , ou sur le client.</span><span class="sxs-lookup"><span data-stu-id="6965d-288">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="6965d-289">**Définir la méthode sur le client (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-289">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="6965d-290">**Autre moyen de définir la méthode sur le client (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-290">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="6965d-291">**Définir la méthode sur le client (sans le proxy généré, ou lors de l’ajout après avoir appelé la méthode de démarrage)**</span><span class="sxs-lookup"><span data-stu-id="6965d-291">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="6965d-292">**Code serveur qui appelle la méthode client**</span><span class="sxs-lookup"><span data-stu-id="6965d-292">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="6965d-293">Les exemples suivants incluent un objet complexe comme paramètre de méthode.</span><span class="sxs-lookup"><span data-stu-id="6965d-293">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="6965d-294">**Définir la méthode sur le client qui prend un objet complexe (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-294">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="6965d-295">**Définir la méthode sur le client qui prend un objet complexe (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-295">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="6965d-296">**Code serveur qui définit l’objet complexe**</span><span class="sxs-lookup"><span data-stu-id="6965d-296">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="6965d-297">**Code serveur qui appelle la méthode client à l’aide d’un objet complexe**</span><span class="sxs-lookup"><span data-stu-id="6965d-297">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="6965d-298">Comment appeler les méthodes du serveur du client</span><span class="sxs-lookup"><span data-stu-id="6965d-298">How to call server methods from the client</span></span>

<span data-ttu-id="6965d-299">Pour appeler une méthode de serveur `server` du client, utilisez `invoke` la propriété du proxy généré ou de la méthode sur le proxy Hub si vous n’utilisez pas le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="6965d-299">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="6965d-300">La valeur de retour ou les paramètres peuvent être des objets complexes.</span><span class="sxs-lookup"><span data-stu-id="6965d-300">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="6965d-301">Passez dans une version camel-case du nom de la méthode sur le Hub.</span><span class="sxs-lookup"><span data-stu-id="6965d-301">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="6965d-302">SignalR effectue automatiquement cette modification de sorte que le code JavaScript peut se conformer aux conventions JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6965d-302">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="6965d-303">Les exemples suivants montrent comment appeler une méthode serveur qui n’a pas de valeur de retour et comment appeler une méthode serveur qui a une valeur de retour.</span><span class="sxs-lookup"><span data-stu-id="6965d-303">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="6965d-304">**Méthode de serveur sans attribut HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="6965d-304">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="6965d-305">**Code serveur qui définit l’objet complexe passé dans un paramètre**</span><span class="sxs-lookup"><span data-stu-id="6965d-305">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="6965d-306">**Code client qui invoque la méthode du serveur (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-306">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="6965d-307">**Code client qui invoque la méthode du serveur (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-307">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="6965d-308">Si vous avez décoré la `HubMethodName` méthode Hub avec un attribut, utilisez ce nom sans changer de boîtier.</span><span class="sxs-lookup"><span data-stu-id="6965d-308">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="6965d-309">**Méthode de serveur** avec un attribut HubMethodName</span><span class="sxs-lookup"><span data-stu-id="6965d-309">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="6965d-310">**Code client qui invoque la méthode du serveur (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-310">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="6965d-311">**Code client qui invoque la méthode du serveur (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-311">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="6965d-312">Les exemples précédents montrent comment appeler une méthode serveur qui n’a aucune valeur de retour.</span><span class="sxs-lookup"><span data-stu-id="6965d-312">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="6965d-313">Les exemples suivants montrent comment appeler une méthode serveur qui a une valeur de retour.</span><span class="sxs-lookup"><span data-stu-id="6965d-313">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="6965d-314">**Code serveur pour une méthode qui a une valeur de retour**</span><span class="sxs-lookup"><span data-stu-id="6965d-314">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="6965d-315">**La classe stock utilisée pour la** valeur de retour</span><span class="sxs-lookup"><span data-stu-id="6965d-315">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="6965d-316">**Code client qui invoque la méthode du serveur (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-316">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="6965d-317">**Code client qui invoque la méthode du serveur (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-317">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="6965d-318">Comment gérer les événements de connexion à vie</span><span class="sxs-lookup"><span data-stu-id="6965d-318">How to handle connection lifetime events</span></span>

<span data-ttu-id="6965d-319">SignalR fournit les événements de connexion suivants à vie que vous pouvez gérer :</span><span class="sxs-lookup"><span data-stu-id="6965d-319">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="6965d-320">`starting`: Augmenté avant que des données soient envoyées sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="6965d-320">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="6965d-321">`received`: Augmenté lorsque des données sont reçues sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="6965d-321">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="6965d-322">Fournit les données reçues.</span><span class="sxs-lookup"><span data-stu-id="6965d-322">Provides the received data.</span></span>
- <span data-ttu-id="6965d-323">`connectionSlow`: Élevé lorsque le client détecte une connexion lente ou fréquemment en baisse.</span><span class="sxs-lookup"><span data-stu-id="6965d-323">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="6965d-324">`reconnecting`: Augmenté lorsque le transport sous-jacent commence à se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="6965d-324">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="6965d-325">`reconnected`: Augmenté lorsque le transport sous-jacent s’est reconnecté.</span><span class="sxs-lookup"><span data-stu-id="6965d-325">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="6965d-326">`stateChanged`: Augmenté lorsque l’état de connexion change.</span><span class="sxs-lookup"><span data-stu-id="6965d-326">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="6965d-327">Fournit l’ancien état et le nouvel état (Connecting, Connected, Reconnecting, ou Déconnecté).</span><span class="sxs-lookup"><span data-stu-id="6965d-327">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="6965d-328">`disconnected`: Augmenté lorsque la connexion s’est déconnectée.</span><span class="sxs-lookup"><span data-stu-id="6965d-328">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="6965d-329">Par exemple, si vous souhaitez afficher des messages d’avertissement lorsqu’il y a des problèmes de connexion qui peuvent causer des retards notables, gérez l’événement. `connectionSlow`</span><span class="sxs-lookup"><span data-stu-id="6965d-329">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="6965d-330">**Gérer l’événement connectionSlow (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-330">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="6965d-331">**Gérer l’événement connectionSlow (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-331">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="6965d-332">Pour plus d’informations, voir [Comprendre et gérer les événements à vie de connexion dans SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="6965d-332">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="6965d-333">Comment gérer les erreurs</span><span class="sxs-lookup"><span data-stu-id="6965d-333">How to handle errors</span></span>

<span data-ttu-id="6965d-334">Le client SignalR JavaScript fournit un `error` événement pour lequel vous pouvez ajouter un gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="6965d-334">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="6965d-335">Vous pouvez également utiliser la méthode d’échec pour ajouter un gestionnaire pour les erreurs qui résultent d’une invocation de méthode de serveur.</span><span class="sxs-lookup"><span data-stu-id="6965d-335">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="6965d-336">Si vous n’activez pas explicitement les messages d’erreur détaillés sur le serveur, l’objet d’exception que SignalR renvoie après une erreur contient un minimum d’informations sur l’erreur.</span><span class="sxs-lookup"><span data-stu-id="6965d-336">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="6965d-337">Par exemple, si `newContosoChatMessage` un appel échoue, le message`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`d’erreur de l’objet d’erreur contient « L’envoi de messages d’erreur détaillés aux clients en production n’est pas recommandé pour des raisons de sécurité, mais si vous souhaitez activer des messages d’erreur détaillés à des fins de dépannage, utilisez le code suivant sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="6965d-337">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="6965d-338">L’exemple suivant montre comment ajouter un gestionnaire pour l’événement d’erreur.</span><span class="sxs-lookup"><span data-stu-id="6965d-338">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="6965d-339">**Ajouter un gestionnaire d’erreur (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-339">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="6965d-340">**Ajouter un gestionnaire d’erreur (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-340">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="6965d-341">L’exemple suivant montre comment gérer une erreur à partir d’une invocation de la méthode.</span><span class="sxs-lookup"><span data-stu-id="6965d-341">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="6965d-342">**Gérer une erreur à partir d’une invocation de la méthode (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-342">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="6965d-343">**Gérer une erreur à partir d’une invocation de la méthode (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-343">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="6965d-344">Si une invocation de `error` méthode échoue, l’événement `error` est également soulevé, ainsi votre code dans le gestionnaire de méthode et dans le `.fail` rappel de méthode exécuterait.</span><span class="sxs-lookup"><span data-stu-id="6965d-344">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="6965d-345">Comment permettre l’enregistrement côté client</span><span class="sxs-lookup"><span data-stu-id="6965d-345">How to enable client-side logging</span></span>

<span data-ttu-id="6965d-346">Pour activer la connexion côté client, `logging` définissez la propriété sur `start` l’objet de connexion avant d’appeler la méthode pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="6965d-346">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="6965d-347">**Activez l’enregistrement (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-347">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="6965d-348">**Activez l’enregistrement (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="6965d-348">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="6965d-349">Pour voir les journaux, ouvrez les outils de développeur de votre navigateur et allez à l’onglet Console. Pour un tutoriel qui montre des instructions étape par étape et des captures d’écran qui montrent comment le faire, voir [Server Broadcast avec ASP.NET Signalr - Activez l’enregistrement](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span><span class="sxs-lookup"><span data-stu-id="6965d-349">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span></span>
