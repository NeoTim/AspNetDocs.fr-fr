---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: ASP.NET SignalR Hubs API Guide - Server (C) Microsoft Docs
author: bradygaster
description: Ce document fournit une introduction à la programmation du côté serveur de l’API ASP.NET SignalR Hubs pour la version SignalR 2, avec des échantillons de code démontrant...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c681b104b15bfc4a04587c7abf685dcf20def2ca
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676187"
---
# <a name="aspnet-signalr-hubs-api-guide---server-c"></a><span data-ttu-id="a7b15-103">ASP.NET SignalR Hubs API Guide - Serveur (C)</span><span class="sxs-lookup"><span data-stu-id="a7b15-103">ASP.NET SignalR Hubs API Guide - Server (C#)</span></span>

<span data-ttu-id="a7b15-104">par [Patrick Fletcher](https://github.com/pfletcher), Tom [Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a7b15-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="a7b15-105">Ce document fournit une introduction à la programmation du côté serveur de l’API ASP.NET SignalR Hubs pour la version SignalR 2, avec des échantillons de code démontrant des options communes.</span><span class="sxs-lookup"><span data-stu-id="a7b15-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 2, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="a7b15-106">L’API SignalR Hubs vous permet de passer des appels de procédure à distance (CRP) d’un serveur à des clients connectés et des clients au serveur.</span><span class="sxs-lookup"><span data-stu-id="a7b15-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="a7b15-107">Dans le code serveur, vous définissez les méthodes qui peuvent être appelées par les clients, et vous appelez les méthodes qui s’exécutent sur le client.</span><span class="sxs-lookup"><span data-stu-id="a7b15-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="a7b15-108">Dans le code client, vous définissez les méthodes qui peuvent être appelées à partir du serveur, et vous appelez les méthodes qui s’exécutent sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="a7b15-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="a7b15-109">SignalR s’occupe de toute la plomberie client-serveur pour vous.</span><span class="sxs-lookup"><span data-stu-id="a7b15-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="a7b15-110">SignalR offre également une API de niveau inférieur appelée Connexions Persistantes.</span><span class="sxs-lookup"><span data-stu-id="a7b15-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="a7b15-111">Pour une introduction à SignalR, Hubs, et persistent Connexions, voir [Introduction à SignalR 2](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="a7b15-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR 2](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="a7b15-112">Versions logicielles utilisées dans ce sujet</span><span class="sxs-lookup"><span data-stu-id="a7b15-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="a7b15-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a7b15-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="a7b15-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="a7b15-114">.NET 4.5</span></span>
> - <span data-ttu-id="a7b15-115">Version SignalR 2</span><span class="sxs-lookup"><span data-stu-id="a7b15-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="topic-versions"></a><span data-ttu-id="a7b15-116">Versions thématiques</span><span class="sxs-lookup"><span data-stu-id="a7b15-116">Topic versions</span></span>
> 
> <span data-ttu-id="a7b15-117">Pour plus d’informations sur les versions antérieures de SignalR, voir [SignalR Older Versions](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="a7b15-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="a7b15-118">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="a7b15-118">Questions and comments</span></span>
> 
> <span data-ttu-id="a7b15-119">S’il vous plaît laisser des commentaires sur la façon dont vous avez aimé ce tutoriel et ce que nous pourrions améliorer dans les commentaires au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="a7b15-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="a7b15-120">Si vous avez des questions qui ne sont pas directement liées au tutoriel, vous pouvez les poster sur le [ASP.NET Forum SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="a7b15-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="a7b15-121">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="a7b15-121">Overview</span></span>

<span data-ttu-id="a7b15-122">Ce document contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="a7b15-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="a7b15-123">Comment enregistrer SignalR middleware</span><span class="sxs-lookup"><span data-stu-id="a7b15-123">How to register SignalR middleware</span></span>](#route)

    - [<span data-ttu-id="a7b15-124">L’URL /signaleur</span><span class="sxs-lookup"><span data-stu-id="a7b15-124">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="a7b15-125">Configuration des options SignalR</span><span class="sxs-lookup"><span data-stu-id="a7b15-125">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="a7b15-126">Comment créer et utiliser les classes Hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-126">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="a7b15-127">Durée de vie de l’objet Hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-127">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="a7b15-128">Camel-casing des noms Hub dans les clients JavaScript</span><span class="sxs-lookup"><span data-stu-id="a7b15-128">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="a7b15-129">Centres multiples</span><span class="sxs-lookup"><span data-stu-id="a7b15-129">Multiple Hubs</span></span>](#multiplehubs)
    - [<span data-ttu-id="a7b15-130">Hubs fortement typés</span><span class="sxs-lookup"><span data-stu-id="a7b15-130">Strongly-Typed Hubs</span></span>](#stronglytypedhubs)
- [<span data-ttu-id="a7b15-131">Comment définir les méthodes de la classe Hub que les clients peuvent appeler</span><span class="sxs-lookup"><span data-stu-id="a7b15-131">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="a7b15-132">Camélion-casage de noms de méthode dans les clients JavaScript</span><span class="sxs-lookup"><span data-stu-id="a7b15-132">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="a7b15-133">Quand exécuter asynchroneously</span><span class="sxs-lookup"><span data-stu-id="a7b15-133">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="a7b15-134">Définir les surcharges</span><span class="sxs-lookup"><span data-stu-id="a7b15-134">Defining overloads</span></span>](#overloads)
    - [<span data-ttu-id="a7b15-135">Signaler les progrès réalisés par rapport aux invocations de la méthode du hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-135">Reporting progress from hub method invocations</span></span>](#progress)
- [<span data-ttu-id="a7b15-136">Comment appeler les méthodes client de la classe Hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-136">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="a7b15-137">Sélection des clients qui recevront le RPC</span><span class="sxs-lookup"><span data-stu-id="a7b15-137">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="a7b15-138">Aucune validation de temps de compilation pour les noms de méthode</span><span class="sxs-lookup"><span data-stu-id="a7b15-138">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="a7b15-139">Nom de méthode insensible au cas correspondant</span><span class="sxs-lookup"><span data-stu-id="a7b15-139">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="a7b15-140">Exécution asynchrone</span><span class="sxs-lookup"><span data-stu-id="a7b15-140">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="a7b15-141">Comment gérer l’adhésion du groupe de la classe Hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-141">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="a7b15-142">Exécution asynchrone des méthodes Add and Remove</span><span class="sxs-lookup"><span data-stu-id="a7b15-142">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="a7b15-143">Persistance de l’adhésion au groupe</span><span class="sxs-lookup"><span data-stu-id="a7b15-143">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="a7b15-144">Groupes d’utilisateurs uniques</span><span class="sxs-lookup"><span data-stu-id="a7b15-144">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="a7b15-145">Comment gérer les événements de connexion à vie dans la classe Hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-145">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="a7b15-146">Lorsque OnConnected, OnDisconnected et OnReconnected sont appelés</span><span class="sxs-lookup"><span data-stu-id="a7b15-146">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="a7b15-147">État appelant non peuplé</span><span class="sxs-lookup"><span data-stu-id="a7b15-147">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="a7b15-148">Comment obtenir des informations sur le client à partir de la propriété Context</span><span class="sxs-lookup"><span data-stu-id="a7b15-148">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="a7b15-149">Comment passer l’état entre les clients et la classe Hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-149">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="a7b15-150">Comment gérer les erreurs dans la classe Hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-150">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="a7b15-151">Comment appeler les méthodes des clients et gérer les groupes de l’extérieur de la classe Hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-151">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="a7b15-152">Appeler les méthodes clientes</span><span class="sxs-lookup"><span data-stu-id="a7b15-152">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="a7b15-153">Gestion des membres du groupe</span><span class="sxs-lookup"><span data-stu-id="a7b15-153">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="a7b15-154">Comment activer le traçage</span><span class="sxs-lookup"><span data-stu-id="a7b15-154">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="a7b15-155">Comment personnaliser le pipeline Hubs</span><span class="sxs-lookup"><span data-stu-id="a7b15-155">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="a7b15-156">Pour obtenir de la documentation sur la façon de programmer les clients, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="a7b15-156">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="a7b15-157">Guide d’API SignalR Hubs - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="a7b15-157">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)
- [<span data-ttu-id="a7b15-158">Guide d’API SignalR Hubs - .NET Client</span><span class="sxs-lookup"><span data-stu-id="a7b15-158">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="a7b15-159">Les composants du serveur pour SignalR 2 ne sont disponibles que dans .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="a7b15-159">The server components for SignalR 2 are only available in .NET 4.5.</span></span> <span data-ttu-id="a7b15-160">Les serveurs fonctionnant en cours d’exécution .NET 4.0 doivent utiliser SignalR v1.x.</span><span class="sxs-lookup"><span data-stu-id="a7b15-160">Servers running .NET 4.0 must use SignalR v1.x.</span></span>

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a><span data-ttu-id="a7b15-161">Comment enregistrer SignalR middleware</span><span class="sxs-lookup"><span data-stu-id="a7b15-161">How to register SignalR middleware</span></span>

<span data-ttu-id="a7b15-162">Pour définir l’itinéraire que les clients utiliseront `MapSignalR` pour vous connecter à votre Hub, appelez la méthode lorsque l’application démarre.</span><span class="sxs-lookup"><span data-stu-id="a7b15-162">To define the route that clients will use to connect to your Hub, call the `MapSignalR` method when the application starts.</span></span> <span data-ttu-id="a7b15-163">`MapSignalR`est une méthode `OwinExtensions` [d’extension](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) pour la classe.</span><span class="sxs-lookup"><span data-stu-id="a7b15-163">`MapSignalR` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `OwinExtensions` class.</span></span> <span data-ttu-id="a7b15-164">L’exemple suivant montre comment définir l’itinéraire SignalR Hubs à l’aide d’une classe de démarrage OWIN.</span><span class="sxs-lookup"><span data-stu-id="a7b15-164">The following example shows how to define the SignalR Hubs route using an OWIN startup class.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

<span data-ttu-id="a7b15-165">Si vous ajoutez la fonctionnalité SignalR à une application MVC ASP.NET, assurez-vous que l’itinéraire SignalR est ajouté avant les autres itinéraires.</span><span class="sxs-lookup"><span data-stu-id="a7b15-165">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="a7b15-166">Pour plus d’informations, voir [Tutorial: Getting Started with SignalR 2 et MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="a7b15-166">For more information, see [Tutorial: Getting Started with SignalR 2 and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="a7b15-167">L’URL /signaleur</span><span class="sxs-lookup"><span data-stu-id="a7b15-167">The /signalr URL</span></span>

<span data-ttu-id="a7b15-168">Par défaut, l’URL d’itinéraire que les clients utiliseront pour se connecter à votre Hub est "/signalur".</span><span class="sxs-lookup"><span data-stu-id="a7b15-168">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="a7b15-169">(Ne confondez pas cette URL avec l’URL "/signaleur/hubs", qui est pour le fichier JavaScript généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="a7b15-169">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="a7b15-170">Pour plus d’informations sur le proxy généré, voir [SignalR Hubs API Guide - JavaScript Client - Le proxy généré et ce qu’il fait pour vous](hubs-api-guide-javascript-client.md#genproxy).)</span><span class="sxs-lookup"><span data-stu-id="a7b15-170">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).)</span></span>

<span data-ttu-id="a7b15-171">Il pourrait y avoir des circonstances extraordinaires qui rendent cette URL de base non utilisable pour SignalR; par exemple, vous avez un dossier dans votre projet nommé *signaleur* et vous ne voulez pas changer le nom.</span><span class="sxs-lookup"><span data-stu-id="a7b15-171">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="a7b15-172">Dans ce cas, vous pouvez modifier l’URL de base, comme indiqué dans les exemples suivants (remplacer "/signaleur" dans le code de l’échantillon avec votre URL désirée).</span><span class="sxs-lookup"><span data-stu-id="a7b15-172">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="a7b15-173">**Code serveur qui spécifie l’URL**</span><span class="sxs-lookup"><span data-stu-id="a7b15-173">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

<span data-ttu-id="a7b15-174">**Code client JavaScript qui spécifie l’URL (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="a7b15-174">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

<span data-ttu-id="a7b15-175">**Code client JavaScript qui spécifie l’URL (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="a7b15-175">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="a7b15-176">**.NET code client qui spécifie l’URL**</span><span class="sxs-lookup"><span data-stu-id="a7b15-176">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="a7b15-177">Configurer les options SignalR</span><span class="sxs-lookup"><span data-stu-id="a7b15-177">Configuring SignalR Options</span></span>

<span data-ttu-id="a7b15-178">Les surcharges `MapSignalR` de la méthode vous permettent de spécifier une URL personnalisée, un résolveur de dépendance personnalisé et les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="a7b15-178">Overloads of the `MapSignalR` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="a7b15-179">Activez les appels inter-domaines à l’aide de CORS ou de JSONP auprès des clients du navigateur.</span><span class="sxs-lookup"><span data-stu-id="a7b15-179">Enable cross-domain calls using CORS or JSONP from browser clients.</span></span>

    <span data-ttu-id="a7b15-180">Typiquement, si le navigateur `http://contoso.com`charge une page à partir de `http://contoso.com/signalr`, la connexion SignalR est dans le même domaine, à .</span><span class="sxs-lookup"><span data-stu-id="a7b15-180">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="a7b15-181">Si la `http://contoso.com` page de `http://fabrikam.com/signalr`fait une connexion à , c’est une connexion cross-domain.</span><span class="sxs-lookup"><span data-stu-id="a7b15-181">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="a7b15-182">Pour des raisons de sécurité, les connexions inter-domaines sont désactivées par défaut.</span><span class="sxs-lookup"><span data-stu-id="a7b15-182">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="a7b15-183">Pour plus d’informations, voir [ASP.NET Guide API SignalR Hubs - JavaScript Client - Comment établir une connexion cross-domain](hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="a7b15-183">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](hubs-api-guide-javascript-client.md#crossdomain).</span></span>
- <span data-ttu-id="a7b15-184">Activez des messages d’erreur détaillés.</span><span class="sxs-lookup"><span data-stu-id="a7b15-184">Enable detailed error messages.</span></span>

    <span data-ttu-id="a7b15-185">Lorsque des erreurs se produisent, le comportement par défaut de SignalR est d’envoyer aux clients un message de notification sans plus de détails sur ce qui s’est passé.</span><span class="sxs-lookup"><span data-stu-id="a7b15-185">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="a7b15-186">L’envoi d’informations détaillées sur les erreurs aux clients n’est pas recommandé dans la production, car les utilisateurs malveillants peuvent être en mesure d’utiliser les informations lors d’attaques contre votre application.</span><span class="sxs-lookup"><span data-stu-id="a7b15-186">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="a7b15-187">Pour le dépannage, vous pouvez utiliser cette option pour activer temporairement des rapports d’erreurs plus informatifs.</span><span class="sxs-lookup"><span data-stu-id="a7b15-187">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="a7b15-188">Désactiver les fichiers proxy JavaScript générés automatiquement.</span><span class="sxs-lookup"><span data-stu-id="a7b15-188">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="a7b15-189">Par défaut, un fichier JavaScript avec des procurations pour vos classes Hub est généré en réponse à l’URL "/signalur/hubs".</span><span class="sxs-lookup"><span data-stu-id="a7b15-189">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="a7b15-190">Si vous ne souhaitez pas utiliser les proxys JavaScript, ou si vous souhaitez générer ce fichier manuellement et vous référer à un fichier physique dans vos clients, vous pouvez utiliser cette option pour désactiver la génération proxy.</span><span class="sxs-lookup"><span data-stu-id="a7b15-190">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="a7b15-191">Pour plus d’informations, voir [SignalR Hubs API Guide - JavaScript Client - Comment créer un fichier physique pour le proxy généré SignalR](hubs-api-guide-javascript-client.md#manualproxy).</span><span class="sxs-lookup"><span data-stu-id="a7b15-191">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).</span></span>

<span data-ttu-id="a7b15-192">L’exemple suivant montre comment spécifier l’URL de `MapSignalR` connexion SignalR et ces options dans un appel à la méthode.</span><span class="sxs-lookup"><span data-stu-id="a7b15-192">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapSignalR` method.</span></span> <span data-ttu-id="a7b15-193">Pour spécifier une URL personnalisée, remplacez "/signaleur" dans l’exemple avec l’URL que vous souhaitez utiliser.</span><span class="sxs-lookup"><span data-stu-id="a7b15-193">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="a7b15-194">Comment créer et utiliser les classes Hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-194">How to create and use Hub classes</span></span>

<span data-ttu-id="a7b15-195">Pour créer un Hub, créez une classe dérivée de [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="a7b15-195">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="a7b15-196">L’exemple suivant montre une classe Hub simple pour une application de chat.</span><span class="sxs-lookup"><span data-stu-id="a7b15-196">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

<span data-ttu-id="a7b15-197">Dans cet exemple, un client `NewContosoChatMessage` connecté peut appeler la méthode, et quand il le fait, les données reçues sont diffusées à tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="a7b15-197">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="a7b15-198">Durée de vie de l’objet Hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-198">Hub object lifetime</span></span>

<span data-ttu-id="a7b15-199">Vous n’avez pas instantané la classe Hub ou n’appelez pas ses méthodes à partir de votre propre code sur le serveur ; tout ce qui est fait pour vous par le pipeline SignalR Hubs.</span><span class="sxs-lookup"><span data-stu-id="a7b15-199">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="a7b15-200">SignalR crée une nouvelle instance de votre classe Hub chaque fois qu’il a besoin de gérer une opération Hub telle que lorsqu’un client se connecte, se déconnecte ou fait un appel de méthode au serveur.</span><span class="sxs-lookup"><span data-stu-id="a7b15-200">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="a7b15-201">Étant donné que les instances de la classe Hub sont transitoires, vous ne pouvez pas les utiliser pour maintenir l’état d’un appel de méthode à l’autre.</span><span class="sxs-lookup"><span data-stu-id="a7b15-201">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="a7b15-202">Chaque fois que le serveur reçoit un appel de méthode d’un client, une nouvelle instance de votre classe Hub traite le message.</span><span class="sxs-lookup"><span data-stu-id="a7b15-202">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="a7b15-203">Pour maintenir l’état par le biais de connexions multiples et d’appels de méthode, utilisez une autre `Hub`méthode telle qu’une base de données, ou une variable statique sur la classe Hub, ou une classe différente qui ne dérive pas de .</span><span class="sxs-lookup"><span data-stu-id="a7b15-203">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="a7b15-204">Si vous persistez les données dans la mémoire, en utilisant une méthode telle qu’une variable statique sur la classe Hub, les données seront perdues lorsque le domaine de l’application recycle.</span><span class="sxs-lookup"><span data-stu-id="a7b15-204">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="a7b15-205">Si vous souhaitez envoyer des messages aux clients à partir de votre propre code qui s’exécute en dehors de la classe Hub, vous ne pouvez pas le faire en instantanéisant une instance de classe Hub, mais vous pouvez le faire en obtenant une référence à l’objet de contexte SignalR pour votre classe Hub.</span><span class="sxs-lookup"><span data-stu-id="a7b15-205">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="a7b15-206">Pour plus d’informations, voir [Comment appeler les méthodes des clients et gérer les groupes de l’extérieur de la classe Hub](#callfromoutsidehub) plus tard dans ce sujet.</span><span class="sxs-lookup"><span data-stu-id="a7b15-206">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="a7b15-207">Camel-casing des noms Hub dans les clients JavaScript</span><span class="sxs-lookup"><span data-stu-id="a7b15-207">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="a7b15-208">Par défaut, les clients JavaScript se réfèrent à Hubs en utilisant une version à dos de chameau du nom de la classe.</span><span class="sxs-lookup"><span data-stu-id="a7b15-208">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="a7b15-209">SignalR effectue automatiquement cette modification de sorte que le code JavaScript peut se conformer aux conventions JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a7b15-209">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="a7b15-210">L’exemple précédent serait appelé `contosoChatHub` dans le code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a7b15-210">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="a7b15-211">**Serveur**</span><span class="sxs-lookup"><span data-stu-id="a7b15-211">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

<span data-ttu-id="a7b15-212">**JavaScript client à l’aide de proxy généré**</span><span class="sxs-lookup"><span data-stu-id="a7b15-212">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

<span data-ttu-id="a7b15-213">Si vous souhaitez spécifier un nom `HubName` différent pour les clients à utiliser, ajoutez l’attribut.</span><span class="sxs-lookup"><span data-stu-id="a7b15-213">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="a7b15-214">Lorsque vous `HubName` utilisez un attribut, il n’y a pas de changement de nom à étui à chameaux sur les clients JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a7b15-214">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="a7b15-215">**Serveur**</span><span class="sxs-lookup"><span data-stu-id="a7b15-215">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

<span data-ttu-id="a7b15-216">**JavaScript client à l’aide de proxy généré**</span><span class="sxs-lookup"><span data-stu-id="a7b15-216">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="a7b15-217">Centres multiples</span><span class="sxs-lookup"><span data-stu-id="a7b15-217">Multiple Hubs</span></span>

<span data-ttu-id="a7b15-218">Vous pouvez définir plusieurs classes Hub dans une application.</span><span class="sxs-lookup"><span data-stu-id="a7b15-218">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="a7b15-219">Lorsque vous faites cela, la connexion est partagée, mais les groupes sont séparés:</span><span class="sxs-lookup"><span data-stu-id="a7b15-219">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="a7b15-220">Tous les clients utiliseront la même URL pour établir une connexion SignalR avec votre service ("/signaleur" ou votre URL personnalisée si vous en avez spécifié un), et cette connexion est utilisée pour tous les Hubs définis par le service.</span><span class="sxs-lookup"><span data-stu-id="a7b15-220">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="a7b15-221">Il n’y a pas de différence de performances pour plusieurs Hubs par rapport à la définition de toutes les fonctionnalités Hub en une seule classe.</span><span class="sxs-lookup"><span data-stu-id="a7b15-221">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="a7b15-222">Tous les Hubs obtiennent les mêmes informations de demande HTTP.</span><span class="sxs-lookup"><span data-stu-id="a7b15-222">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="a7b15-223">Étant donné que tous les Hubs partagent la même connexion, les seules informations de demande HTTP que le serveur obtient est ce qui vient dans la demande https://version originale qui établit la connexion SignalR.</span><span class="sxs-lookup"><span data-stu-id="a7b15-223">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="a7b15-224">Si vous utilisez la demande de connexion pour transmettre des informations du client au serveur en spécifiant une chaîne de requête, vous ne pouvez pas fournir différentes chaînes de requête à différents Hubs.</span><span class="sxs-lookup"><span data-stu-id="a7b15-224">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="a7b15-225">Tous les Hubs recevront les mêmes informations.</span><span class="sxs-lookup"><span data-stu-id="a7b15-225">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="a7b15-226">Le fichier javaScript des mandataires généré contiendra des procurations pour tous les Hubs dans un seul fichier.</span><span class="sxs-lookup"><span data-stu-id="a7b15-226">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="a7b15-227">Pour plus d’informations sur les proxys JavaScript, voir [SignalR Hubs API Guide - JavaScript Client - Le proxy généré et ce qu’il fait pour vous](hubs-api-guide-javascript-client.md#genproxy).</span><span class="sxs-lookup"><span data-stu-id="a7b15-227">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).</span></span>
- <span data-ttu-id="a7b15-228">Les groupes sont définis au sein des Hubs.</span><span class="sxs-lookup"><span data-stu-id="a7b15-228">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="a7b15-229">Dans SignalR, vous pouvez définir les groupes nommés à diffuser à des sous-ensembles de clients connectés.</span><span class="sxs-lookup"><span data-stu-id="a7b15-229">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="a7b15-230">Les groupes sont maintenus séparément pour chaque Hub.</span><span class="sxs-lookup"><span data-stu-id="a7b15-230">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="a7b15-231">Par exemple, un groupe nommé « Administrateurs » comprendrait un ensemble de clients pour votre `ContosoChatHub` classe, `StockTickerHub` et le même nom de groupe ferait référence à un ensemble différent de clients pour votre classe.</span><span class="sxs-lookup"><span data-stu-id="a7b15-231">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a><span data-ttu-id="a7b15-232">Hubs fortement typés</span><span class="sxs-lookup"><span data-stu-id="a7b15-232">Strongly-Typed Hubs</span></span>

<span data-ttu-id="a7b15-233">Pour définir une interface pour vos méthodes de moyeu que votre client peut référencer (et activer Intellisense sur vos méthodes de moyeu), dérivez votre hub (introduit `Hub<T>` dans SignalR 2.1) plutôt que `Hub`:</span><span class="sxs-lookup"><span data-stu-id="a7b15-233">To define an interface for your hub methods that your client can reference (and enable Intellisense on your hub methods), derive your hub from `Hub<T>` (introduced in SignalR 2.1) rather than `Hub`:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="a7b15-234">Comment définir les méthodes de la classe Hub que les clients peuvent appeler</span><span class="sxs-lookup"><span data-stu-id="a7b15-234">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="a7b15-235">Pour exposer une méthode sur le Hub que vous souhaitez être callable du client, déclarez une méthode publique, comme le montrent les exemples suivants.</span><span class="sxs-lookup"><span data-stu-id="a7b15-235">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="a7b15-236">Vous pouvez spécifier un type de retour et des paramètres, y compris des types et des tableaux complexes, comme vous le feriez dans n’importe quelle méthode C.</span><span class="sxs-lookup"><span data-stu-id="a7b15-236">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="a7b15-237">Toutes les données que vous recevez dans les paramètres ou le retour à l’appelant sont communiquées entre le client et le serveur en utilisant JSON, et SignalR gère automatiquement la liaison d’objets complexes et de tableaux d’objets.</span><span class="sxs-lookup"><span data-stu-id="a7b15-237">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="a7b15-238">Camélion-casage de noms de méthode dans les clients JavaScript</span><span class="sxs-lookup"><span data-stu-id="a7b15-238">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="a7b15-239">Par défaut, les clients JavaScript se réfèrent aux méthodes Hub en utilisant une version à dos de chameau du nom de la méthode.</span><span class="sxs-lookup"><span data-stu-id="a7b15-239">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="a7b15-240">SignalR effectue automatiquement cette modification de sorte que le code JavaScript peut se conformer aux conventions JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a7b15-240">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="a7b15-241">**Serveur**</span><span class="sxs-lookup"><span data-stu-id="a7b15-241">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="a7b15-242">**JavaScript client à l’aide de proxy généré**</span><span class="sxs-lookup"><span data-stu-id="a7b15-242">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="a7b15-243">Si vous souhaitez spécifier un nom `HubMethodName` différent pour les clients à utiliser, ajoutez l’attribut.</span><span class="sxs-lookup"><span data-stu-id="a7b15-243">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="a7b15-244">**Serveur**</span><span class="sxs-lookup"><span data-stu-id="a7b15-244">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="a7b15-245">**JavaScript client à l’aide de proxy généré**</span><span class="sxs-lookup"><span data-stu-id="a7b15-245">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="a7b15-246">Quand exécuter asynchroneously</span><span class="sxs-lookup"><span data-stu-id="a7b15-246">When to execute asynchronously</span></span>

<span data-ttu-id="a7b15-247">Si la méthode sera longue ou doit faire un travail qui impliquerait l’attente, comme une recherche de base de données ou un `void` appel de service Web, rendre la méthode Hub asynchrone en retournant une [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (en lieu de retour) ou [l’objet&lt;de la tâche T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) (au lieu du type de `T` retour).</span><span class="sxs-lookup"><span data-stu-id="a7b15-247">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="a7b15-248">Lorsque vous `Task` retournez un objet de la `Task` méthode, SignalR attend que le devait se terminer, puis il renvoie le résultat non emballé au client, de sorte qu’il n’y a aucune différence dans la façon dont vous codez l’appel de méthode dans le client.</span><span class="sxs-lookup"><span data-stu-id="a7b15-248">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="a7b15-249">Rendre une méthode Hub asynchrone évite de bloquer la connexion lorsqu’elle utilise le transport WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a7b15-249">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="a7b15-250">Lorsqu’une méthode Hub s’exécute de manière synchrone et que le transport est WebSocket, les invocations ultérieures de méthodes sur le Hub à partir du même client sont bloquées jusqu’à ce que la méthode Hub se termine.</span><span class="sxs-lookup"><span data-stu-id="a7b15-250">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="a7b15-251">L’exemple suivant montre la même méthode codée pour s’exécuter de façon synchrone ou asynchrone, suivie par le code client JavaScript qui fonctionne pour appeler l’une ou l’autre version.</span><span class="sxs-lookup"><span data-stu-id="a7b15-251">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="a7b15-252">**Synchrone**</span><span class="sxs-lookup"><span data-stu-id="a7b15-252">**Synchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="a7b15-253">**Asynchrone**</span><span class="sxs-lookup"><span data-stu-id="a7b15-253">**Asynchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="a7b15-254">**JavaScript client à l’aide de proxy généré**</span><span class="sxs-lookup"><span data-stu-id="a7b15-254">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="a7b15-255">Pour plus d’informations sur la façon d’utiliser des méthodes asynchrones dans ASP.NET 4.5, voir [Using Asynchroneous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="a7b15-255">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="a7b15-256">Définir les surcharges</span><span class="sxs-lookup"><span data-stu-id="a7b15-256">Defining Overloads</span></span>

<span data-ttu-id="a7b15-257">Si vous souhaitez définir les surcharges d’une méthode, le nombre de paramètres dans chaque surcharge doit être différent.</span><span class="sxs-lookup"><span data-stu-id="a7b15-257">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="a7b15-258">Si vous différenciez une surcharge simplement en spécifiant différents types de paramètres, votre classe Hub se compilera, mais le service SignalR fera une exception au moment de l’exécution lorsque les clients essaient d’appeler l’une des surcharges.</span><span class="sxs-lookup"><span data-stu-id="a7b15-258">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a><span data-ttu-id="a7b15-259">Signaler les progrès réalisés par rapport aux invocations de la méthode du hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-259">Reporting progress from hub method invocations</span></span>

<span data-ttu-id="a7b15-260">SignalR 2.1 ajoute un soutien pour le [modèle de rapports d’étape](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduit dans .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="a7b15-260">SignalR 2.1 adds support for the [progress reporting pattern](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduced in .NET 4.5.</span></span> <span data-ttu-id="a7b15-261">Pour mettre en œuvre des rapports d’étape, définissez un `IProgress<T>` paramètre pour votre méthode de hub à laquelle votre client peut accéder :</span><span class="sxs-lookup"><span data-stu-id="a7b15-261">To implement progress reporting, define an `IProgress<T>` parameter for your hub method that your client can access:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

<span data-ttu-id="a7b15-262">Lors de l’écriture d’une méthode serveur de longue durée, il est important d’utiliser un modèle de programmation asynchrone comme Async / Attendez plutôt que de bloquer le fil hub.</span><span class="sxs-lookup"><span data-stu-id="a7b15-262">When writing a long-running server method, it is important to use an asynchronous programming pattern like Async/ Await rather than blocking the hub thread.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="a7b15-263">Comment appeler les méthodes client de la classe Hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-263">How to call client methods from the Hub class</span></span>

<span data-ttu-id="a7b15-264">Pour appeler les méthodes client `Clients` du serveur, utilisez la propriété dans une méthode de votre classe Hub.</span><span class="sxs-lookup"><span data-stu-id="a7b15-264">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="a7b15-265">L’exemple suivant montre `addNewMessageToPage` le code serveur qui appelle tous les clients connectés, et le code client qui définit la méthode dans un client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a7b15-265">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="a7b15-266">**Serveur**</span><span class="sxs-lookup"><span data-stu-id="a7b15-266">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

<span data-ttu-id="a7b15-267">Invoquer une méthode client est une opération asynchrone et renvoie un `Task`.</span><span class="sxs-lookup"><span data-stu-id="a7b15-267">Invoking a client method is an asynchronous operation and returns a `Task`.</span></span> <span data-ttu-id="a7b15-268">Utilisation `await`:</span><span class="sxs-lookup"><span data-stu-id="a7b15-268">Use `await`:</span></span>

* <span data-ttu-id="a7b15-269">Pour s’assurer que le message est envoyé sans erreur.</span><span class="sxs-lookup"><span data-stu-id="a7b15-269">To ensure the message is sent without error.</span></span> 
* <span data-ttu-id="a7b15-270">Pour permettre des erreurs de capture et de manipulation dans un bloc d’essai-catch.</span><span class="sxs-lookup"><span data-stu-id="a7b15-270">To enable catching and handling errors in a try-catch block.</span></span>

<span data-ttu-id="a7b15-271">**JavaScript client à l’aide de proxy généré**</span><span class="sxs-lookup"><span data-stu-id="a7b15-271">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

<span data-ttu-id="a7b15-272">Vous ne pouvez pas obtenir une valeur de retour d’une méthode client; syntaxe `int x = Clients.All.add(1,1)` telle que ne fonctionne pas.</span><span class="sxs-lookup"><span data-stu-id="a7b15-272">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="a7b15-273">Vous pouvez spécifier des types et des tableaux complexes pour les paramètres.</span><span class="sxs-lookup"><span data-stu-id="a7b15-273">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="a7b15-274">L’exemple suivant transmet un type complexe au client dans un paramètre de méthode.</span><span class="sxs-lookup"><span data-stu-id="a7b15-274">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="a7b15-275">**Code serveur qui appelle une méthode client à l’aide d’un objet complexe**</span><span class="sxs-lookup"><span data-stu-id="a7b15-275">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

<span data-ttu-id="a7b15-276">**Code serveur qui définit l’objet complexe**</span><span class="sxs-lookup"><span data-stu-id="a7b15-276">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

<span data-ttu-id="a7b15-277">**JavaScript client à l’aide de proxy généré**</span><span class="sxs-lookup"><span data-stu-id="a7b15-277">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="a7b15-278">Sélection des clients qui recevront le RPC</span><span class="sxs-lookup"><span data-stu-id="a7b15-278">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="a7b15-279">La propriété Clients renvoie un objet [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) qui offre plusieurs options pour spécifier quels clients recevront le RPC :</span><span class="sxs-lookup"><span data-stu-id="a7b15-279">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="a7b15-280">Tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="a7b15-280">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="a7b15-281">Seulement le client appelant.</span><span class="sxs-lookup"><span data-stu-id="a7b15-281">Only the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="a7b15-282">Tous les clients sauf le client appelant.</span><span class="sxs-lookup"><span data-stu-id="a7b15-282">All clients except the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- <span data-ttu-id="a7b15-283">Un client spécifique identifié par id de connexion.</span><span class="sxs-lookup"><span data-stu-id="a7b15-283">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    <span data-ttu-id="a7b15-284">Cet exemple `addContosoChatMessageToPage` appelle le client appelant et `Clients.Caller`a le même effet que l’utilisation .</span><span class="sxs-lookup"><span data-stu-id="a7b15-284">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="a7b15-285">Tous les clients connectés, à l’exception des clients spécifiés, identifiés par id de connexion.</span><span class="sxs-lookup"><span data-stu-id="a7b15-285">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- <span data-ttu-id="a7b15-286">Tous les clients connectés dans un groupe spécifié.</span><span class="sxs-lookup"><span data-stu-id="a7b15-286">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- <span data-ttu-id="a7b15-287">Tous les clients connectés dans un groupe spécifié, à l’exception des clients spécifiés, identifiés par id de connexion.</span><span class="sxs-lookup"><span data-stu-id="a7b15-287">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- <span data-ttu-id="a7b15-288">Tous les clients connectés dans un groupe spécifié, à l’exception du client appelant.</span><span class="sxs-lookup"><span data-stu-id="a7b15-288">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- <span data-ttu-id="a7b15-289">Un utilisateur spécifique, identifié par userId.</span><span class="sxs-lookup"><span data-stu-id="a7b15-289">A specific user, identified by userId.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    <span data-ttu-id="a7b15-290">Par défaut, `IPrincipal.Identity.Name`c’est , mais cela peut être changé [en enregistrant une mise en œuvre de IUserIdProvider avec l’hôte mondial](mapping-users-to-connections.md#IUserIdProvider).</span><span class="sxs-lookup"><span data-stu-id="a7b15-290">By default, this is `IPrincipal.Identity.Name`, but this can be changed by [registering an implementation of IUserIdProvider with the global host](mapping-users-to-connections.md#IUserIdProvider).</span></span>
- <span data-ttu-id="a7b15-291">Tous les clients et groupes dans une liste d’ID de connexion.</span><span class="sxs-lookup"><span data-stu-id="a7b15-291">All clients and groups in a list of connection IDs.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- <span data-ttu-id="a7b15-292">Une liste de groupes.</span><span class="sxs-lookup"><span data-stu-id="a7b15-292">A list of groups.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- <span data-ttu-id="a7b15-293">Un utilisateur par son nom.</span><span class="sxs-lookup"><span data-stu-id="a7b15-293">A user by name.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- <span data-ttu-id="a7b15-294">Une liste de noms d’utilisateur (introduit dans SignalR 2.1).</span><span class="sxs-lookup"><span data-stu-id="a7b15-294">A list of user names (introduced in SignalR 2.1).</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="a7b15-295">Aucune validation de temps de compilation pour les noms de méthode</span><span class="sxs-lookup"><span data-stu-id="a7b15-295">No compile-time validation for method names</span></span>

<span data-ttu-id="a7b15-296">Le nom de méthode que vous spécifiez est interprété comme un objet dynamique, ce qui signifie qu’il n’y a pas d’IntelliSense ou de validation de temps de compilation pour elle.</span><span class="sxs-lookup"><span data-stu-id="a7b15-296">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="a7b15-297">L’expression est évaluée au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="a7b15-297">The expression is evaluated at run time.</span></span> <span data-ttu-id="a7b15-298">Lorsque l’appel de méthode s’exécute, SignalR envoie le nom de la méthode et les valeurs de paramètre au client, et si le client a une méthode qui correspond au nom, cette méthode est appelée et les valeurs de paramètres sont transmises à elle.</span><span class="sxs-lookup"><span data-stu-id="a7b15-298">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="a7b15-299">Si aucune méthode de correspondance n’est trouvée sur le client, aucune erreur n’est soulevée.</span><span class="sxs-lookup"><span data-stu-id="a7b15-299">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="a7b15-300">Pour plus d’informations sur le format des données que SignalR transmet au client dans les coulisses lorsque vous appelez une méthode client, voir [Introduction à SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="a7b15-300">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="a7b15-301">Nom de méthode insensible au cas correspondant</span><span class="sxs-lookup"><span data-stu-id="a7b15-301">Case-insensitive method name matching</span></span>

<span data-ttu-id="a7b15-302">L’appariement du nom de la méthode est insensible aux cas.</span><span class="sxs-lookup"><span data-stu-id="a7b15-302">Method name matching is case-insensitive.</span></span> <span data-ttu-id="a7b15-303">Par `Clients.All.addContosoChatMessageToPage` exemple, sur le `AddContosoChatMessageToPage` `addcontosochatmessagetopage`serveur `addContosoChatMessageToPage` s’exécute, , ou sur le client.</span><span class="sxs-lookup"><span data-stu-id="a7b15-303">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="a7b15-304">Exécution asynchrone</span><span class="sxs-lookup"><span data-stu-id="a7b15-304">Asynchronous execution</span></span>

<span data-ttu-id="a7b15-305">La méthode que vous appelez exécute asynchronement.</span><span class="sxs-lookup"><span data-stu-id="a7b15-305">The method that you call executes asynchronously.</span></span> <span data-ttu-id="a7b15-306">Tout code qui vient après un appel de méthode à un client s’exécute immédiatement sans attendre que SignalR finisse la transmission de données aux clients, sauf si vous spécifiez que les lignes de code ultérieures doivent attendre l’achèvement de la méthode.</span><span class="sxs-lookup"><span data-stu-id="a7b15-306">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="a7b15-307">L’exemple de code suivant montre comment exécuter deux méthodes clientes de façon séquentielle.</span><span class="sxs-lookup"><span data-stu-id="a7b15-307">The following code example shows how to execute two client methods sequentially.</span></span>

<span data-ttu-id="a7b15-308">**Utilisation d’attente (.NET 4.5)**</span><span class="sxs-lookup"><span data-stu-id="a7b15-308">**Using Await (.NET 4.5)**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="a7b15-309">Si vous `await` attendez qu’une méthode client se termine avant l’exécution de la prochaine ligne de code, cela ne signifie pas que les clients recevront effectivement le message avant l’exécution de la prochaine ligne de code.</span><span class="sxs-lookup"><span data-stu-id="a7b15-309">If you use `await` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="a7b15-310">« L’achèvement » d’un appel de méthode client signifie seulement que SignalR a fait tout le nécessaire pour envoyer le message.</span><span class="sxs-lookup"><span data-stu-id="a7b15-310">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="a7b15-311">Si vous avez besoin de vérification que les clients ont reçu le message, vous devez programmer ce mécanisme vous-même.</span><span class="sxs-lookup"><span data-stu-id="a7b15-311">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="a7b15-312">Par exemple, vous `MessageReceived` pouvez coder une méthode `addContosoChatMessageToPage` sur le Hub, `MessageReceived` et dans la méthode sur le client, vous pouvez appeler après avoir fait tout le travail que vous devez faire sur le client.</span><span class="sxs-lookup"><span data-stu-id="a7b15-312">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="a7b15-313">Dans `MessageReceived` le Hub, vous pouvez faire tout le travail dépend de la réception réelle des clients et le traitement de l’appel de méthode d’origine.</span><span class="sxs-lookup"><span data-stu-id="a7b15-313">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="a7b15-314">Comment utiliser une variable de chaîne comme nom de méthode</span><span class="sxs-lookup"><span data-stu-id="a7b15-314">How to use a string variable as the method name</span></span>

<span data-ttu-id="a7b15-315">Si vous souhaitez invoquer une méthode client en utilisant `Clients.All` une `Clients.Others` `Clients.Caller`variable de chaîne `IClientProxy` comme nom de méthode, moulé (ou , , etc) et ensuite appeler [Invoke (méthodeName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="a7b15-315">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="a7b15-316">Comment gérer l’adhésion du groupe de la classe Hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-316">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="a7b15-317">Les groupes de SignalR fournissent une méthode de diffusion de messages à des sous-ensembles spécifiés de clients connectés.</span><span class="sxs-lookup"><span data-stu-id="a7b15-317">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="a7b15-318">Un groupe peut avoir n’importe quel nombre de clients, et un client peut être membre d’un certain nombre de groupes.</span><span class="sxs-lookup"><span data-stu-id="a7b15-318">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="a7b15-319">Pour gérer l’adhésion au groupe, utilisez `Groups` les méthodes [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) fournies par la propriété de la classe Hub.</span><span class="sxs-lookup"><span data-stu-id="a7b15-319">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="a7b15-320">L’exemple suivant `Groups.Add` `Groups.Remove` montre les méthodes et les méthodes utilisées dans les méthodes Hub qui sont appelées par le code client, suivie par le code client JavaScript qui les appelle.</span><span class="sxs-lookup"><span data-stu-id="a7b15-320">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="a7b15-321">**Serveur**</span><span class="sxs-lookup"><span data-stu-id="a7b15-321">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

<span data-ttu-id="a7b15-322">**JavaScript client à l’aide de proxy généré**</span><span class="sxs-lookup"><span data-stu-id="a7b15-322">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

<span data-ttu-id="a7b15-323">Vous n’avez pas à créer explicitement des groupes.</span><span class="sxs-lookup"><span data-stu-id="a7b15-323">You don't have to explicitly create groups.</span></span> <span data-ttu-id="a7b15-324">En effet, un groupe est automatiquement créé la première `Groups.Add`fois que vous spécifiez son nom dans un appel à , et il est supprimé lorsque vous supprimez la dernière connexion de l’adhésion à celui-ci.</span><span class="sxs-lookup"><span data-stu-id="a7b15-324">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="a7b15-325">Il n’y a pas d’API pour obtenir une liste d’adhésion de groupe ou une liste de groupes.</span><span class="sxs-lookup"><span data-stu-id="a7b15-325">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="a7b15-326">SignalR envoie des messages aux clients et aux groupes en fonction d’un [pub/sous-modèle,](http://en.wikipedia.org/wiki/Publish/subscribe)et le serveur ne tient pas de listes de groupes ou d’adhésions de groupe.</span><span class="sxs-lookup"><span data-stu-id="a7b15-326">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="a7b15-327">Cela permet de maximiser l’évolutivité, parce que chaque fois que vous ajoutez un nœud à une ferme web, tout état que SignalR maintient doit être propagé au nouveau nœud.</span><span class="sxs-lookup"><span data-stu-id="a7b15-327">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="a7b15-328">Exécution asynchrone des méthodes Add and Remove</span><span class="sxs-lookup"><span data-stu-id="a7b15-328">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="a7b15-329">Les `Groups.Add` `Groups.Remove` méthodes et les méthodes s’exécutent asynchronement.</span><span class="sxs-lookup"><span data-stu-id="a7b15-329">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="a7b15-330">Si vous souhaitez ajouter un client à un groupe et envoyer immédiatement un message au `Groups.Add` client en utilisant le groupe, vous devez vous assurer que la méthode se termine en premier.</span><span class="sxs-lookup"><span data-stu-id="a7b15-330">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="a7b15-331">L’exemple de code suivant montre comment le faire.</span><span class="sxs-lookup"><span data-stu-id="a7b15-331">The following code example shows how to do that.</span></span>

<span data-ttu-id="a7b15-332">**Ajout d’un client à un groupe, puis messagerie de ce client**</span><span class="sxs-lookup"><span data-stu-id="a7b15-332">**Adding a client to a group and then messaging that client**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="a7b15-333">Persistance de l’adhésion au groupe</span><span class="sxs-lookup"><span data-stu-id="a7b15-333">Group membership persistence</span></span>

<span data-ttu-id="a7b15-334">SignalR suit les connexions, pas les utilisateurs, donc si vous voulez qu’un utilisateur soit `Groups.Add` dans le même groupe chaque fois que l’utilisateur établit une connexion, vous devez appeler chaque fois que l’utilisateur établit une nouvelle connexion.</span><span class="sxs-lookup"><span data-stu-id="a7b15-334">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="a7b15-335">Après une perte temporaire de connectivité, signalR peut parfois restaurer la connexion automatiquement.</span><span class="sxs-lookup"><span data-stu-id="a7b15-335">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="a7b15-336">Dans ce cas, SignalR rétablit la même connexion, n’établit pas de nouvelle connexion, et donc l’adhésion du groupe du client est automatiquement restaurée.</span><span class="sxs-lookup"><span data-stu-id="a7b15-336">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="a7b15-337">Cela est possible même lorsque la pause temporaire est le résultat d’un redémarrage ou d’une défaillance du serveur, car l’état de connexion pour chaque client, y compris les adhésions de groupe, est trébuché vers le client.</span><span class="sxs-lookup"><span data-stu-id="a7b15-337">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="a7b15-338">Si un serveur tombe en panne et est remplacé par un nouveau serveur avant la sortie des temps de connexion, un client peut se reconnecter automatiquement au nouveau serveur et ré-inscrire dans les groupes dont il est membre.</span><span class="sxs-lookup"><span data-stu-id="a7b15-338">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="a7b15-339">Lorsqu’une connexion ne peut pas être restaurée automatiquement après une perte de connectivité, ou lorsque la connexion se déconnecte ou lorsque le client se déconnecte (par exemple, lorsqu’un navigateur navigue vers une nouvelle page), les abonnements du groupe sont perdus.</span><span class="sxs-lookup"><span data-stu-id="a7b15-339">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="a7b15-340">La prochaine fois que l’utilisateur se connecte sera une nouvelle connexion.</span><span class="sxs-lookup"><span data-stu-id="a7b15-340">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="a7b15-341">Pour maintenir les adhésions de groupe lorsque le même utilisateur établit une nouvelle connexion, votre application doit suivre les associations entre les utilisateurs et les groupes, et restaurer les adhésions de groupe chaque fois qu’un utilisateur établit une nouvelle connexion.</span><span class="sxs-lookup"><span data-stu-id="a7b15-341">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="a7b15-342">Pour plus d’informations sur les connexions et les reconnexions, voir [Comment gérer les événements de connexion à vie dans la classe Hub](#connectionlifetime) plus tard dans ce sujet.</span><span class="sxs-lookup"><span data-stu-id="a7b15-342">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="a7b15-343">Groupes d’utilisateurs uniques</span><span class="sxs-lookup"><span data-stu-id="a7b15-343">Single-user groups</span></span>

<span data-ttu-id="a7b15-344">Les applications qui utilisent SignalR doivent généralement suivre les associations entre les utilisateurs et les connexions afin de savoir quel utilisateur a envoyé un message et quel utilisateur(s) doit recevoir un message.</span><span class="sxs-lookup"><span data-stu-id="a7b15-344">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="a7b15-345">Les groupes sont utilisés dans l’un des deux modèles couramment utilisés pour ce faire.</span><span class="sxs-lookup"><span data-stu-id="a7b15-345">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="a7b15-346">Groupes d’utilisateurs uniques.</span><span class="sxs-lookup"><span data-stu-id="a7b15-346">Single-user groups.</span></span>

    <span data-ttu-id="a7b15-347">Vous pouvez spécifier le nom d’utilisateur comme nom de groupe, et ajouter l’ID de connexion en cours au groupe chaque fois que l’utilisateur se connecte ou se reconnecte.</span><span class="sxs-lookup"><span data-stu-id="a7b15-347">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="a7b15-348">Pour envoyer des messages à l’utilisateur que vous envoyez au groupe.</span><span class="sxs-lookup"><span data-stu-id="a7b15-348">To send messages to the user you send to the group.</span></span> <span data-ttu-id="a7b15-349">Un inconvénient de cette méthode est que le groupe ne vous fournit pas un moyen de savoir si l’utilisateur est en ligne ou hors ligne.</span><span class="sxs-lookup"><span data-stu-id="a7b15-349">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="a7b15-350">Suivre les associations entre les noms d’utilisateur et les identifiants de connexion.</span><span class="sxs-lookup"><span data-stu-id="a7b15-350">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="a7b15-351">Vous pouvez stocker une association entre chaque nom d’utilisateur et une ou plusieurs identifiants de connexion dans un dictionnaire ou une base de données, et mettre à jour les données stockées chaque fois que l’utilisateur se connecte ou se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="a7b15-351">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="a7b15-352">Pour envoyer des messages à l’utilisateur, vous spécifiez les identifiants de connexion.</span><span class="sxs-lookup"><span data-stu-id="a7b15-352">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="a7b15-353">Un inconvénient de cette méthode est qu’il faut plus de mémoire.</span><span class="sxs-lookup"><span data-stu-id="a7b15-353">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="a7b15-354">Comment gérer les événements de connexion à vie dans la classe Hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-354">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="a7b15-355">Les raisons typiques de la gestion des événements de connexion à vie sont de garder une trace de savoir si un utilisateur est connecté ou non, et de garder une trace de l’association entre les noms d’utilisateur et les identifiants de connexion.</span><span class="sxs-lookup"><span data-stu-id="a7b15-355">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="a7b15-356">Pour exécuter votre propre code lorsque les `OnConnected`clients `OnDisconnected`se `OnReconnected` connectent ou se déconnectent, remplacez les méthodes, et virtuelles de la classe Hub, comme indiqué dans l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="a7b15-356">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="a7b15-357">Lorsque OnConnected, OnDisconnected et OnReconnected sont appelés</span><span class="sxs-lookup"><span data-stu-id="a7b15-357">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="a7b15-358">Chaque fois qu’un navigateur navigue vers une nouvelle page, une nouvelle `OnDisconnected` connexion doit `OnConnected` être établie, ce qui signifie que SignalR exécutera la méthode suivie par la méthode.</span><span class="sxs-lookup"><span data-stu-id="a7b15-358">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="a7b15-359">SignalR crée toujours une nouvelle connexion ID lorsqu’une nouvelle connexion est établie.</span><span class="sxs-lookup"><span data-stu-id="a7b15-359">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="a7b15-360">La `OnReconnected` méthode est appelée lorsqu’il y a eu une rupture temporaire de connectivité dont SignalR peut automatiquement récupérer, par exemple lorsqu’un câble est temporairement déconnecté et reconnecté avant que la connexion ne soit désactivée. La `OnDisconnected` méthode est appelée lorsque le client est déconnecté et SignalR ne peut pas automatiquement se reconnecter, par exemple quand un navigateur navigue vers une nouvelle page.</span><span class="sxs-lookup"><span data-stu-id="a7b15-360">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="a7b15-361">Par conséquent, une séquence possible d’événements pour un client donné est `OnConnected`, `OnReconnected`, `OnDisconnected`; ou `OnConnected` `OnDisconnected`, .</span><span class="sxs-lookup"><span data-stu-id="a7b15-361">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="a7b15-362">Vous ne verrez pas `OnConnected` `OnDisconnected`la `OnReconnected` séquence , , pour une connexion donnée.</span><span class="sxs-lookup"><span data-stu-id="a7b15-362">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="a7b15-363">La `OnDisconnected` méthode n’est pas appelée dans certains scénarios, comme lorsqu’un serveur tombe en panne ou que le domaine de l’application est recyclé.</span><span class="sxs-lookup"><span data-stu-id="a7b15-363">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="a7b15-364">Lorsqu’un autre serveur est en ligne ou que le domaine d’application `OnReconnected` complète son recyclage, certains clients peuvent être en mesure de se reconnecter et de tirer l’événement.</span><span class="sxs-lookup"><span data-stu-id="a7b15-364">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="a7b15-365">Pour plus d’informations, voir [Comprendre et gérer les événements à vie de connexion dans SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="a7b15-365">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="a7b15-366">État appelant non peuplé</span><span class="sxs-lookup"><span data-stu-id="a7b15-366">Caller state not populated</span></span>

<span data-ttu-id="a7b15-367">Les méthodes de gestionnaire d’événements à vie de connexion sont `state` appelées à partir du serveur, `Caller` ce qui signifie que tout état que vous mettez dans l’objet sur le client ne sera pas peuplé dans la propriété sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="a7b15-367">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="a7b15-368">Pour plus `state` d’informations `Caller` sur l’objet et la propriété, voir [Comment passer l’état entre les clients et la classe Hub](#passstate) plus tard dans ce sujet.</span><span class="sxs-lookup"><span data-stu-id="a7b15-368">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="a7b15-369">Comment obtenir des informations sur le client à partir de la propriété Context</span><span class="sxs-lookup"><span data-stu-id="a7b15-369">How to get information about the client from the Context property</span></span>

<span data-ttu-id="a7b15-370">Pour obtenir des informations sur `Context` le client, utilisez la propriété de la classe Hub.</span><span class="sxs-lookup"><span data-stu-id="a7b15-370">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="a7b15-371">La `Context` propriété renvoie un objet [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) qui donne accès aux informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a7b15-371">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="a7b15-372">ID de connexion du client appelant.</span><span class="sxs-lookup"><span data-stu-id="a7b15-372">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    <span data-ttu-id="a7b15-373">L’ID de connexion est un GUID qui est attribué par SignalR (vous ne pouvez pas spécifier la valeur de votre propre code).</span><span class="sxs-lookup"><span data-stu-id="a7b15-373">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="a7b15-374">Il y a un ID de connexion pour chaque connexion, et le même IDENTIFIANT de connexion est utilisé par tous les Hubs si vous avez plusieurs Hubs dans votre application.</span><span class="sxs-lookup"><span data-stu-id="a7b15-374">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="a7b15-375">DONNÉES d’en-tête HTTP.</span><span class="sxs-lookup"><span data-stu-id="a7b15-375">HTTP header data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="a7b15-376">Vous pouvez également obtenir `Context.Headers`des en-têtes HTTP de .</span><span class="sxs-lookup"><span data-stu-id="a7b15-376">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="a7b15-377">La raison de multiples références à `Context.Headers` la même `Context.Request` chose est qui `Context.Headers` a été créé en premier, la propriété a été ajoutée plus tard, et a été conservé pour la compatibilité arrière.</span><span class="sxs-lookup"><span data-stu-id="a7b15-377">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="a7b15-378">Requête des données de chaîne.</span><span class="sxs-lookup"><span data-stu-id="a7b15-378">Query string data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    <span data-ttu-id="a7b15-379">Vous pouvez également obtenir des `Context.QueryString`données de chaîne de requête à partir de .</span><span class="sxs-lookup"><span data-stu-id="a7b15-379">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="a7b15-380">La chaîne de requête que vous obtenez dans cette propriété est celle qui a été utilisée avec la demande HTTP qui a établi la connexion SignalR.</span><span class="sxs-lookup"><span data-stu-id="a7b15-380">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="a7b15-381">Vous pouvez ajouter des paramètres de chaîne de requête dans le client en configurant la connexion, ce qui est un moyen pratique de transmettre des données sur le client du client au serveur.</span><span class="sxs-lookup"><span data-stu-id="a7b15-381">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="a7b15-382">L’exemple suivant montre une façon d’ajouter une chaîne de requête dans un client JavaScript lorsque vous utilisez le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="a7b15-382">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    <span data-ttu-id="a7b15-383">Pour plus d’informations sur le réglage des paramètres de chaîne de requête, consultez les guides API pour les clients [JavaScript](hubs-api-guide-javascript-client.md) et [.NET.](hubs-api-guide-net-client.md)</span><span class="sxs-lookup"><span data-stu-id="a7b15-383">For more information about setting query string parameters, see the API guides for the [JavaScript](hubs-api-guide-javascript-client.md) and [.NET](hubs-api-guide-net-client.md) clients.</span></span>

    <span data-ttu-id="a7b15-384">Vous pouvez trouver la méthode de transport utilisée pour la connexion dans les données de chaîne de requête, ainsi que d’autres valeurs utilisées en interne par SignalR :</span><span class="sxs-lookup"><span data-stu-id="a7b15-384">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    <span data-ttu-id="a7b15-385">La valeur `transportMethod` de sera "webSockets", "serverSentEvents", "foreverFrame" ou "longPolling".</span><span class="sxs-lookup"><span data-stu-id="a7b15-385">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="a7b15-386">Notez que si vous `OnConnected` vérifiez cette valeur dans la méthode de gestionnaire d’événements, dans certains scénarios, vous pourriez d’abord obtenir une valeur de transport qui n’est pas la méthode de transport négociée finale pour la connexion.</span><span class="sxs-lookup"><span data-stu-id="a7b15-386">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="a7b15-387">Dans ce cas, la méthode jettera une exception et sera appelée à nouveau plus tard lorsque le mode de transport final sera établi.</span><span class="sxs-lookup"><span data-stu-id="a7b15-387">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="a7b15-388">Cookies.</span><span class="sxs-lookup"><span data-stu-id="a7b15-388">Cookies.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    <span data-ttu-id="a7b15-389">Vous pouvez également `Context.RequestCookies`obtenir des cookies de .</span><span class="sxs-lookup"><span data-stu-id="a7b15-389">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="a7b15-390">informations utilisateur.</span><span class="sxs-lookup"><span data-stu-id="a7b15-390">User information.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- <span data-ttu-id="a7b15-391">L’objet HttpContext pour la demande :</span><span class="sxs-lookup"><span data-stu-id="a7b15-391">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    <span data-ttu-id="a7b15-392">Utilisez cette méthode `HttpContext.Current` au lieu `HttpContext` d’obtenir l’objet pour la connexion SignalR.</span><span class="sxs-lookup"><span data-stu-id="a7b15-392">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="a7b15-393">Comment passer l’état entre les clients et la classe Hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-393">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="a7b15-394">Le proxy client `state` fournit un objet dans lequel vous pouvez stocker les données que vous souhaitez être transmises au serveur à chaque appel de méthode.</span><span class="sxs-lookup"><span data-stu-id="a7b15-394">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="a7b15-395">Sur le serveur, vous pouvez `Clients.Caller` accéder à ces données dans la propriété dans les méthodes Hub qui sont appelés par les clients.</span><span class="sxs-lookup"><span data-stu-id="a7b15-395">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="a7b15-396">La `Clients.Caller` propriété n’est pas peuplée pour `OnConnected` `OnDisconnected`les `OnReconnected`méthodes de gestionnaire d’événements à vie de connexion , , et .</span><span class="sxs-lookup"><span data-stu-id="a7b15-396">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="a7b15-397">La création ou `state` la mise `Clients.Caller` à jour de données dans l’objet et la propriété fonctionne dans les deux directions.</span><span class="sxs-lookup"><span data-stu-id="a7b15-397">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="a7b15-398">Vous pouvez mettre à jour les valeurs dans le serveur et elles sont transmises au client.</span><span class="sxs-lookup"><span data-stu-id="a7b15-398">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="a7b15-399">L’exemple suivant montre le code client JavaScript qui stocke l’état pour la transmission au serveur avec chaque appel de méthode.</span><span class="sxs-lookup"><span data-stu-id="a7b15-399">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

<span data-ttu-id="a7b15-400">L’exemple suivant affiche le code équivalent dans un client .NET.</span><span class="sxs-lookup"><span data-stu-id="a7b15-400">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

<span data-ttu-id="a7b15-401">Dans votre classe Hub, vous pouvez `Clients.Caller` accéder à ces données dans la propriété.</span><span class="sxs-lookup"><span data-stu-id="a7b15-401">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="a7b15-402">L’exemple suivant montre le code qui récupère l’état mentionné dans l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="a7b15-402">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="a7b15-403">Ce mécanisme pour l’état persistant n’est pas destiné à `state` `Clients.Caller` de grandes quantités de données, puisque tout ce que vous mettez dans la propriété ou est trébuché avec chaque invocation de méthode.</span><span class="sxs-lookup"><span data-stu-id="a7b15-403">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="a7b15-404">Il est utile pour les petits éléments tels que les noms d’utilisateur ou les compteurs.</span><span class="sxs-lookup"><span data-stu-id="a7b15-404">It's useful for smaller items such as user names or counters.</span></span>

<span data-ttu-id="a7b15-405">Dans VB.NET ou dans un moyeu fortement typé, l’objet d’état de l’appelant ne peut pas être consulté par ; `Clients.Caller` au lieu `Clients.CallerState` de cela, l’utilisation (introduite dans SignalR 2.1) :</span><span class="sxs-lookup"><span data-stu-id="a7b15-405">In VB.NET or in a strongly-typed hub, the caller state object can't be accessed through `Clients.Caller`; instead, use `Clients.CallerState` (introduced in SignalR 2.1):</span></span>

<span data-ttu-id="a7b15-406">**Utilisation de CallerState en C #**</span><span class="sxs-lookup"><span data-stu-id="a7b15-406">**Using CallerState in C#**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

<span data-ttu-id="a7b15-407">**Utilisation de CallerState dans Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="a7b15-407">**Using CallerState in Visual Basic**</span></span>

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="a7b15-408">Comment gérer les erreurs dans la classe Hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-408">How to handle errors in the Hub class</span></span>

<span data-ttu-id="a7b15-409">Pour gérer les erreurs qui se produisent dans vos méthodes de classe Hub, assurez-vous d’abord `await`d'"observer" toutes les exceptions des opérations async (comme l’invocation des méthodes clientes) en utilisant .</span><span class="sxs-lookup"><span data-stu-id="a7b15-409">To handle errors that occur in your Hub class methods, first ensure you "observe" any exceptions from async operations (such as invoking client methods) by using `await`.</span></span> <span data-ttu-id="a7b15-410">Ensuite, utilisez une ou plusieurs des méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="a7b15-410">Then use one or more of the following methods:</span></span>

- <span data-ttu-id="a7b15-411">Enveloppez votre code de méthode dans des blocs d’essai et connectez l’objet d’exception.</span><span class="sxs-lookup"><span data-stu-id="a7b15-411">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="a7b15-412">Pour des raisons de débogage, vous pouvez envoyer l’exception au client, mais pour des raisons de sécurité l’envoi d’informations détaillées aux clients en production n’est pas recommandé.</span><span class="sxs-lookup"><span data-stu-id="a7b15-412">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="a7b15-413">Créez un module de pipeline Hubs qui gère la méthode [OnIncomingError.](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="a7b15-413">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="a7b15-414">L’exemple suivant montre un module de pipeline qui enregistre les erreurs, suivi d’un code en Startup.cs qui injecte le module dans le pipeline Hubs.</span><span class="sxs-lookup"><span data-stu-id="a7b15-414">The following example shows a pipeline module that logs errors, followed by code in Startup.cs that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- <span data-ttu-id="a7b15-415">Utilisez `HubException` la classe (introduite dans SignalR 2).</span><span class="sxs-lookup"><span data-stu-id="a7b15-415">Use the `HubException` class (introduced in SignalR 2).</span></span> <span data-ttu-id="a7b15-416">Cette erreur peut être jetée de n’importe quelle invocation de hub.</span><span class="sxs-lookup"><span data-stu-id="a7b15-416">This error can be thrown from any hub invocation.</span></span> <span data-ttu-id="a7b15-417">Le `HubError` constructeur prend un message de chaîne, et un objet pour stocker des données d’erreur supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="a7b15-417">The `HubError` constructor takes a string message, and an object for storing extra error data.</span></span> <span data-ttu-id="a7b15-418">SignalR va auto-sérialiser l’exception et l’envoyer au client, où il sera utilisé pour rejeter ou échouer l’invocation de la méthode de moyeu.</span><span class="sxs-lookup"><span data-stu-id="a7b15-418">SignalR will auto-serialize the exception and send it to the client, where it will be used to reject or fail the hub method invocation.</span></span>

    <span data-ttu-id="a7b15-419">Les échantillons de code suivants `HubException` montrent comment lancer une invocation au cours d’un Hub, et comment gérer l’exception sur les clients JavaScript et .NET.</span><span class="sxs-lookup"><span data-stu-id="a7b15-419">The following code samples demonstrate how to throw a `HubException` during a Hub invocation, and how to handle the exception on JavaScript and .NET clients.</span></span>

    <span data-ttu-id="a7b15-420">**Code serveur démontrant la classe HubException**</span><span class="sxs-lookup"><span data-stu-id="a7b15-420">**Server code demonstrating the HubException class**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    <span data-ttu-id="a7b15-421">**Code client JavaScript démontrant la réponse à la création d’un HubException dans un hub**</span><span class="sxs-lookup"><span data-stu-id="a7b15-421">**JavaScript client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    <span data-ttu-id="a7b15-422">**.NET code client démontrant la réponse à jeter un HubException dans un hub**</span><span class="sxs-lookup"><span data-stu-id="a7b15-422">**.NET client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

<span data-ttu-id="a7b15-423">Pour plus d’informations sur les modules de pipeline Hub, voir [comment personnaliser le pipeline Hubs](#hubpipeline) plus tard dans ce sujet.</span><span class="sxs-lookup"><span data-stu-id="a7b15-423">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="a7b15-424">Comment activer le traçage</span><span class="sxs-lookup"><span data-stu-id="a7b15-424">How to enable tracing</span></span>

<span data-ttu-id="a7b15-425">Pour activer le traçage côté serveur, ajoutez un élément system.diagnostics à votre fichier Web.config, tel qu’indiqué dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="a7b15-425">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

<span data-ttu-id="a7b15-426">Lorsque vous exécutez l’application dans Visual Studio, vous pouvez afficher les journaux dans la fenêtre **Output.**</span><span class="sxs-lookup"><span data-stu-id="a7b15-426">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="a7b15-427">Comment appeler les méthodes des clients et gérer les groupes de l’extérieur de la classe Hub</span><span class="sxs-lookup"><span data-stu-id="a7b15-427">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="a7b15-428">Pour appeler les méthodes client d’une classe différente de votre classe Hub, obtenez une référence à l’objet de contexte SignalR pour le Hub et utilisez-le pour appeler des méthodes sur le client ou gérer des groupes.</span><span class="sxs-lookup"><span data-stu-id="a7b15-428">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="a7b15-429">La classe `StockTicker` d’échantillon suivante obtient l’objet de contexte, le stocke dans un cas de la classe, stocke l’instance de classe dans une propriété statique, et utilise le contexte de l’instance de classe singleton pour appeler la méthode sur les `updateStockPrice` clients qui sont connectés à un Hub nommé `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="a7b15-429">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

<span data-ttu-id="a7b15-430">Si vous avez besoin d’utiliser le contexte plusieurs fois dans un objet à longue durée de vie, obtenir la référence une fois et l’enregistrer plutôt que de l’obtenir à nouveau à chaque fois.</span><span class="sxs-lookup"><span data-stu-id="a7b15-430">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="a7b15-431">L’obtention du contexte une fois garantit que SignalR envoie des messages aux clients dans la même séquence dans laquelle vos méthodes Hub font des invocations de la méthode client.</span><span class="sxs-lookup"><span data-stu-id="a7b15-431">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="a7b15-432">Pour un tutoriel qui montre comment utiliser le contexte SignalR pour un hub, voir [Server Broadcast avec ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="a7b15-432">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="a7b15-433">Appeler les méthodes clientes</span><span class="sxs-lookup"><span data-stu-id="a7b15-433">Calling client methods</span></span>

<span data-ttu-id="a7b15-434">Vous pouvez spécifier quels clients recevront le RPC, mais vous avez moins d’options que lorsque vous appelez à partir d’une classe Hub.</span><span class="sxs-lookup"><span data-stu-id="a7b15-434">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="a7b15-435">La raison en est que le contexte n’est pas associé à un appel particulier d’un `Clients.Others`client, de sorte que toutes les méthodes qui nécessitent une connaissance de l’ID de connexion actuelle, tels que , ou `Clients.Caller`, ou `Clients.OthersInGroup`, ne sont pas disponibles.</span><span class="sxs-lookup"><span data-stu-id="a7b15-435">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="a7b15-436">Les options suivantes sont disponibles :</span><span class="sxs-lookup"><span data-stu-id="a7b15-436">The following options are available:</span></span>

- <span data-ttu-id="a7b15-437">Tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="a7b15-437">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- <span data-ttu-id="a7b15-438">Un client spécifique identifié par id de connexion.</span><span class="sxs-lookup"><span data-stu-id="a7b15-438">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- <span data-ttu-id="a7b15-439">Tous les clients connectés, à l’exception des clients spécifiés, identifiés par id de connexion.</span><span class="sxs-lookup"><span data-stu-id="a7b15-439">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- <span data-ttu-id="a7b15-440">Tous les clients connectés dans un groupe spécifié.</span><span class="sxs-lookup"><span data-stu-id="a7b15-440">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- <span data-ttu-id="a7b15-441">Tous les clients connectés dans un groupe spécifié, à l’exception des clients spécifiés, identifiés par id de connexion.</span><span class="sxs-lookup"><span data-stu-id="a7b15-441">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

<span data-ttu-id="a7b15-442">Si vous appelez dans votre classe non-Hub à partir de méthodes dans votre classe `Clients.Client`Hub, vous pouvez passer dans l’ID de connexion actuelle et l’utiliser avec `Clients.AllExcept`, , ou `Clients.Group` pour simuler `Clients.Caller`, `Clients.Others`, ou `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="a7b15-442">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="a7b15-443">Dans l’exemple `MoveShapeHub` suivant, la classe `Broadcaster` transmet l’ID de connexion à la classe afin que la `Broadcaster` classe puisse simuler `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="a7b15-443">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="a7b15-444">Gestion des membres du groupe</span><span class="sxs-lookup"><span data-stu-id="a7b15-444">Managing group membership</span></span>

<span data-ttu-id="a7b15-445">Pour gérer des groupes, vous avez les mêmes options que vous dans une classe Hub.</span><span class="sxs-lookup"><span data-stu-id="a7b15-445">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="a7b15-446">Ajouter un client à un groupe</span><span class="sxs-lookup"><span data-stu-id="a7b15-446">Add a client to a group</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- <span data-ttu-id="a7b15-447">Retirez un client d’un groupe</span><span class="sxs-lookup"><span data-stu-id="a7b15-447">Remove a client from a group</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="a7b15-448">Comment personnaliser le pipeline Hubs</span><span class="sxs-lookup"><span data-stu-id="a7b15-448">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="a7b15-449">SignalR vous permet d’injecter votre propre code dans le pipeline Hub.</span><span class="sxs-lookup"><span data-stu-id="a7b15-449">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="a7b15-450">L’exemple suivant montre un module de pipeline Hub personnalisé qui enregistre chaque appel de méthode entrant reçu du client et l’appel de méthode sortant invoqué sur le client :</span><span class="sxs-lookup"><span data-stu-id="a7b15-450">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

<span data-ttu-id="a7b15-451">Le code suivant dans le fichier *Startup.cs* enregistre le module pour fonctionner dans le pipeline Hub :</span><span class="sxs-lookup"><span data-stu-id="a7b15-451">The following code in the *Startup.cs* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

<span data-ttu-id="a7b15-452">Il existe de nombreuses méthodes différentes que vous pouvez remplacer.</span><span class="sxs-lookup"><span data-stu-id="a7b15-452">There are many different methods that you can override.</span></span> <span data-ttu-id="a7b15-453">Pour une liste complète, voir [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="a7b15-453">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
