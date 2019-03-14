---
title: Prise en charge des WebSockets dans ASP.NET Core
author: rick-anderson
description: Découvrez comment commencer à utiliser les WebSockets dans ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/17/2019
uid: fundamentals/websockets
ms.openlocfilehash: 76acb9c96ed5e8bbbaf39eeb6cb23307bb44fb8d
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57031406"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="15d33-103">Prise en charge des WebSockets dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15d33-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="15d33-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="15d33-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="15d33-105">Cet article explique comment commencer avec les WebSockets dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="15d33-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="15d33-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) est un protocole qui autorise des canaux de communication persistants bidirectionnels sur les connexions TCP.</span><span class="sxs-lookup"><span data-stu-id="15d33-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="15d33-107">Il est utilisé dans les applications qui bénéficient de communications rapides et en temps réel, comme les applications de conversation, de tableau de bord et de jeu.</span><span class="sxs-lookup"><span data-stu-id="15d33-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="15d33-108">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="15d33-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="15d33-109">Pour plus d’informations, consultez la section [Étapes suivantes](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="15d33-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15d33-110">Prérequis</span><span class="sxs-lookup"><span data-stu-id="15d33-110">Prerequisites</span></span>

* <span data-ttu-id="15d33-111">ASP.NET Core 1.1 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="15d33-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="15d33-112">Tout système d’exploitation prenant en charge ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="15d33-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="15d33-113">Windows 7 / Windows Server 2008 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="15d33-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="15d33-114">Linux</span><span class="sxs-lookup"><span data-stu-id="15d33-114">Linux</span></span>
  * <span data-ttu-id="15d33-115">macOS</span><span class="sxs-lookup"><span data-stu-id="15d33-115">macOS</span></span>
  
* <span data-ttu-id="15d33-116">Si l’application s’exécute sur Windows avec IIS :</span><span class="sxs-lookup"><span data-stu-id="15d33-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="15d33-117">Windows 8/Windows Server 2012 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="15d33-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="15d33-118">IIS 8/IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="15d33-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="15d33-119">WebSockets doit être activé (consultez la section [Prise en charge d’IIS/IIS Express](#iisiis-express-support).)</span><span class="sxs-lookup"><span data-stu-id="15d33-119">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="15d33-120">Si l’application s’exécute sur [HTTP.sys](xref:fundamentals/servers/httpsys) :</span><span class="sxs-lookup"><span data-stu-id="15d33-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="15d33-121">Windows 8/Windows Server 2012 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="15d33-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="15d33-122">Pour les navigateurs pris en charge, consultez https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="15d33-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="15d33-123">Quand utiliser WebSocket</span><span class="sxs-lookup"><span data-stu-id="15d33-123">When to use WebSockets</span></span>

<span data-ttu-id="15d33-124">Utilisez WebSocket pour travailler directement avec une connexion de socket.</span><span class="sxs-lookup"><span data-stu-id="15d33-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="15d33-125">Par exemple, utilisez WebSocket de façon à obtenir les meilleures performances possibles pour un jeu en temps réel.</span><span class="sxs-lookup"><span data-stu-id="15d33-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="15d33-126">[ASP.NET Core SignalR](xref:signalr/introduction) est une bibliothèque qui simplifie l’ajout de fonctionnalités web en temps réel aux applications.</span><span class="sxs-lookup"><span data-stu-id="15d33-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="15d33-127">Elle utilise des WebSockets dans la mesure du possible.</span><span class="sxs-lookup"><span data-stu-id="15d33-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="15d33-128">Comment utiliser des WebSockets ?</span><span class="sxs-lookup"><span data-stu-id="15d33-128">How to use WebSockets</span></span>

* <span data-ttu-id="15d33-129">Installez le package [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="15d33-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="15d33-130">Configurez l’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="15d33-130">Configure the middleware.</span></span>
* <span data-ttu-id="15d33-131">Acceptez les requêtes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="15d33-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="15d33-132">Envoyez et recevez des messages.</span><span class="sxs-lookup"><span data-stu-id="15d33-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="15d33-133">Configurer l’intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="15d33-133">Configure the middleware</span></span>

<span data-ttu-id="15d33-134">Ajoutez le middleware WebSocket dans la méthode `Configure` de la classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="15d33-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="15d33-135">Les paramètres suivants peuvent être configurés :</span><span class="sxs-lookup"><span data-stu-id="15d33-135">The following settings can be configured:</span></span>

* <span data-ttu-id="15d33-136">`KeepAliveInterval` : fréquence d’envoi de frames de « ping » au client pour garantir que les proxys maintiennent la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="15d33-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="15d33-137">La valeur par défaut est deux minutes.</span><span class="sxs-lookup"><span data-stu-id="15d33-137">The default is two minutes.</span></span>
* <span data-ttu-id="15d33-138">`ReceiveBufferSize` : taille de la mémoire tampon utilisée pour recevoir des données.</span><span class="sxs-lookup"><span data-stu-id="15d33-138">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="15d33-139">Seuls les utilisateurs avancés peuvent être amenés à changer ce paramètre, pour l’optimisation des performances en fonction de la taille des données.</span><span class="sxs-lookup"><span data-stu-id="15d33-139">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="15d33-140">La valeur par défaut est 4 Ko.</span><span class="sxs-lookup"><span data-stu-id="15d33-140">The default is 4 KB.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="15d33-141">Les paramètres suivants peuvent être configurés :</span><span class="sxs-lookup"><span data-stu-id="15d33-141">The following settings can be configured:</span></span>

* <span data-ttu-id="15d33-142">`KeepAliveInterval` : fréquence d’envoi de frames de « ping » au client pour garantir que les proxys maintiennent la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="15d33-142">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="15d33-143">La valeur par défaut est deux minutes.</span><span class="sxs-lookup"><span data-stu-id="15d33-143">The default is two minutes.</span></span>
* <span data-ttu-id="15d33-144">`ReceiveBufferSize` : taille de la mémoire tampon utilisée pour recevoir des données.</span><span class="sxs-lookup"><span data-stu-id="15d33-144">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="15d33-145">Seuls les utilisateurs avancés peuvent être amenés à changer ce paramètre, pour l’optimisation des performances en fonction de la taille des données.</span><span class="sxs-lookup"><span data-stu-id="15d33-145">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="15d33-146">La valeur par défaut est 4 Ko.</span><span class="sxs-lookup"><span data-stu-id="15d33-146">The default is 4 KB.</span></span>
* <span data-ttu-id="15d33-147">`AllowedOrigins` : liste des valeurs d’en-tête Origin autorisées pour les requêtes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="15d33-147">`AllowedOrigins` - A list of allowed Origin header values for WebSocket requests.</span></span> <span data-ttu-id="15d33-148">Par défaut, toutes les origines sont autorisées.</span><span class="sxs-lookup"><span data-stu-id="15d33-148">By default, all origins are allowed.</span></span> <span data-ttu-id="15d33-149">Pour plus d’informations, consultez « Restriction d’origine WebSocket » ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="15d33-149">See "WebSocket origin restriction" below for details.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a><span data-ttu-id="15d33-150">Accepter les requêtes WebSocket</span><span class="sxs-lookup"><span data-stu-id="15d33-150">Accept WebSocket requests</span></span>

<span data-ttu-id="15d33-151">Ultérieurement dans le cycle de vie de la requête (plus loin dans la méthode `Configure` ou dans une action MVC, par exemple), vérifiez s’il s’agit d’une requête WebSocket et acceptez-la.</span><span class="sxs-lookup"><span data-stu-id="15d33-151">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="15d33-152">L’exemple suivant est extrait d’une partie ultérieure de la méthode `Configure` :</span><span class="sxs-lookup"><span data-stu-id="15d33-152">The following example is from later in the `Configure` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

<span data-ttu-id="15d33-153">Une requête WebSocket peut figurer sur toute URL, mais cet exemple de code accepte uniquement les requêtes pour `/ws`.</span><span class="sxs-lookup"><span data-stu-id="15d33-153">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="15d33-154">Envoyer et recevoir des messages</span><span class="sxs-lookup"><span data-stu-id="15d33-154">Send and receive messages</span></span>

<span data-ttu-id="15d33-155">La méthode `AcceptWebSocketAsync` met à niveau la connexion TCP vers une connexion WebSocket et fournit un objet [WebSocket](/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="15d33-155">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="15d33-156">Utilisez l’objet `WebSocket` pour envoyer et recevoir des messages.</span><span class="sxs-lookup"><span data-stu-id="15d33-156">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="15d33-157">Le code montré précédemment qui accepte la requête WebSocket passe l’objet `WebSocket` à une méthode `Echo`.</span><span class="sxs-lookup"><span data-stu-id="15d33-157">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="15d33-158">Le code reçoit un message et renvoie immédiatement le même message.</span><span class="sxs-lookup"><span data-stu-id="15d33-158">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="15d33-159">Les messages sont envoyés et reçus dans une boucle jusqu’à ce que le client ferme la connexion :</span><span class="sxs-lookup"><span data-stu-id="15d33-159">Messages are sent and received in a loop until the client closes the connection:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

<span data-ttu-id="15d33-160">Quand vous acceptez le WebSocket avant de commencer cette boucle, le pipeline de middlewares se termine.</span><span class="sxs-lookup"><span data-stu-id="15d33-160">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="15d33-161">À la fermeture du socket, le pipeline se déroule.</span><span class="sxs-lookup"><span data-stu-id="15d33-161">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="15d33-162">Autrement dit, la requête cesse d’avancer dans le pipeline quand le WebSocket est accepté.</span><span class="sxs-lookup"><span data-stu-id="15d33-162">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="15d33-163">Quand la boucle est terminée et que le socket est fermé, la requête recule dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="15d33-163">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="handle-client-disconnects"></a><span data-ttu-id="15d33-164">Gérer la déconnexion du client</span><span class="sxs-lookup"><span data-stu-id="15d33-164">Handle client disconnects</span></span>

<span data-ttu-id="15d33-165">Le serveur n’est pas automatiquement informé lorsque le client se déconnecte en raison d’une perte de connectivité.</span><span class="sxs-lookup"><span data-stu-id="15d33-165">The server is not automatically informed when the client disconnects due to loss of connectivity.</span></span> <span data-ttu-id="15d33-166">Le serveur reçoit un message de déconnexion uniquement si le client l’envoie, ce qui n’est pas possible si la connexion Internet est perdue.</span><span class="sxs-lookup"><span data-stu-id="15d33-166">The server receives a disconnect message only if the client sends it, which can't be done if the internet connection is lost.</span></span> <span data-ttu-id="15d33-167">Si vous souhaitez prendre des mesures quand cela se produit, définissez un délai d’attente lorsque rien n’est reçu du client dans un certain laps de temps.</span><span class="sxs-lookup"><span data-stu-id="15d33-167">If you want to take some action when that happens, set a timeout after nothing is received from the client within a certain time window.</span></span>

<span data-ttu-id="15d33-168">Si le client n’envoie toujours pas de messages et si vous ne souhaitez pas appliquer le délai d’attente uniquement parce que la connexion devient inactive, demandez au client d’utiliser un minuteur pour envoyer un message ping toutes les X secondes.</span><span class="sxs-lookup"><span data-stu-id="15d33-168">If the client isn't always sending messages and you don't want to timeout just because the connection goes idle, have the client use a timer to send a ping message every X seconds.</span></span> <span data-ttu-id="15d33-169">Sur le serveur, si un message n’arrive pas dans un délai de 2\*X secondes après le précédent, mettez fin à la connexion et signalez que le client s’est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="15d33-169">On the server, if a message hasn't arrived within 2\*X seconds after the previous one, terminate the connection and report that the client disconnected.</span></span> <span data-ttu-id="15d33-170">Attendez deux fois la durée de l’intervalle de temps attendu pour autoriser les délais réseau qui peuvent retarder le message ping.</span><span class="sxs-lookup"><span data-stu-id="15d33-170">Wait for twice the expected time interval to leave extra time for network delays that might hold up the ping message.</span></span>

### <a name="websocket-origin-restriction"></a><span data-ttu-id="15d33-171">Restriction d’origine WebSocket</span><span class="sxs-lookup"><span data-stu-id="15d33-171">WebSocket origin restriction</span></span>

<span data-ttu-id="15d33-172">Les protections fournies par CORS ne s’appliquent pas aux WebSockets.</span><span class="sxs-lookup"><span data-stu-id="15d33-172">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="15d33-173">Les navigateurs **:**</span><span class="sxs-lookup"><span data-stu-id="15d33-173">Browsers do **not**:</span></span>

* <span data-ttu-id="15d33-174">n’effectuent pas de requêtes préalables CORS ;</span><span class="sxs-lookup"><span data-stu-id="15d33-174">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="15d33-175">respectent les restrictions spécifiées dans les en-têtes `Access-Control` quand ils effectuent des requêtes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="15d33-175">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="15d33-176">Toutefois, les navigateurs envoient l’en-tête `Origin` au moment de l’émission des requêtes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="15d33-176">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="15d33-177">Les applications doivent être configurées de manière à valider ces en-têtes, le but étant de vérifier que seuls les WebSockets provenant des origines attendues sont autorisés.</span><span class="sxs-lookup"><span data-stu-id="15d33-177">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="15d33-178">Si vous hébergez votre serveur sur « https://server.com» et votre client sur « https://client.com», ajoutez « https://client.com» à la liste `AllowedOrigins` pour autoriser les WebSockets.</span><span class="sxs-lookup"><span data-stu-id="15d33-178">If you're hosting your server on "https://server.com" and hosting your client on "https://client.com", add "https://client.com" to the `AllowedOrigins` list for WebSockets to verify.</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptionsAO&highlight=6-7)]

> [!NOTE]
> <span data-ttu-id="15d33-179">L’en-tête `Origin` est contrôlé par le client et, comme l’en-tête `Referer`, peut être falsifié.</span><span class="sxs-lookup"><span data-stu-id="15d33-179">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="15d33-180">**N’utilisez pas** ces en-têtes comme mécanisme d’authentification.</span><span class="sxs-lookup"><span data-stu-id="15d33-180">Do **not** use these headers as an authentication mechanism.</span></span>

::: moniker-end

## <a name="iisiis-express-support"></a><span data-ttu-id="15d33-181">Prise en charge d’IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="15d33-181">IIS/IIS Express support</span></span>

<span data-ttu-id="15d33-182">Windows Server 2012 ou ultérieur et Windows 8 ou ultérieur avec IIS/IIS Express 8 ou ultérieure prennent en charge le protocole WebSocket.</span><span class="sxs-lookup"><span data-stu-id="15d33-182">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="15d33-183">WebSockets sont toujours activés lorsque vous utilisez IIS Express.</span><span class="sxs-lookup"><span data-stu-id="15d33-183">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="15d33-184">Activation de WebSockets sur IIS</span><span class="sxs-lookup"><span data-stu-id="15d33-184">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="15d33-185">Pour activer la prise en charge du protocole WebSocket sur Windows Server 2012 ou ultérieur :</span><span class="sxs-lookup"><span data-stu-id="15d33-185">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="15d33-186">Ces étapes ne sont pas nécessaires si vous utilisez IIS Express</span><span class="sxs-lookup"><span data-stu-id="15d33-186">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="15d33-187">Utilisez l’Assistant **Ajouter des rôles et des fonctionnalités** par le biais du menu **Gérer** ou du lien dans **Gestionnaire de serveur**.</span><span class="sxs-lookup"><span data-stu-id="15d33-187">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="15d33-188">Sélectionnez **Installation basée sur un rôle ou une fonctionnalité**.</span><span class="sxs-lookup"><span data-stu-id="15d33-188">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="15d33-189">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="15d33-189">Select **Next**.</span></span>
1. <span data-ttu-id="15d33-190">Sélectionnez le serveur approprié (le serveur local est sélectionné par défaut).</span><span class="sxs-lookup"><span data-stu-id="15d33-190">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="15d33-191">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="15d33-191">Select **Next**.</span></span>
1. <span data-ttu-id="15d33-192">Développez **Serveur web (IIS)** dans l’arborescence **Rôles**, développez **Serveur Web**, puis développez **Développement d’applications**.</span><span class="sxs-lookup"><span data-stu-id="15d33-192">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="15d33-193">Sélectionnez **Protocole WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="15d33-193">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="15d33-194">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="15d33-194">Select **Next**.</span></span>
1. <span data-ttu-id="15d33-195">Si vous n’avez pas besoin d’autres fonctionnalités, sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="15d33-195">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="15d33-196">Cliquez sur **Installer**.</span><span class="sxs-lookup"><span data-stu-id="15d33-196">Select **Install**.</span></span>
1. <span data-ttu-id="15d33-197">Une fois l’installation terminée, sélectionnez **Fermer** pour quitter l’Assistant.</span><span class="sxs-lookup"><span data-stu-id="15d33-197">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="15d33-198">Pour activer la prise en charge du protocole WebSocket sur Windows 8 ou ultérieur :</span><span class="sxs-lookup"><span data-stu-id="15d33-198">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="15d33-199">Ces étapes ne sont pas nécessaires si vous utilisez IIS Express</span><span class="sxs-lookup"><span data-stu-id="15d33-199">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="15d33-200">Accédez à **Panneau de configuration** > **Programmes** > **Programmes et fonctionnalités** > **Activer ou désactiver des fonctionnalités Windows** (à gauche de l’écran).</span><span class="sxs-lookup"><span data-stu-id="15d33-200">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="15d33-201">Ouvrez les nœuds suivants : **Internet Information Services** > **Services World Wide Web** > **Fonctionnalités de développement d’applications**.</span><span class="sxs-lookup"><span data-stu-id="15d33-201">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="15d33-202">Sélectionnez la fonctionnalité **Protocole WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="15d33-202">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="15d33-203">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="15d33-203">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="15d33-204">Désactiver WebSocket lors de l’utilisation de socket.io sur Node.js</span><span class="sxs-lookup"><span data-stu-id="15d33-204">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="15d33-205">Si vous utilisez la prise en charge de WebSocket dans [socket.io](https://socket.io/) sur [Node.js](https://nodejs.org/), désactivez le module WebSocket IIS par défaut en utilisant l’élément `webSocket` dans *web.config* ou dans *applicationHost.config*. Si cette étape n’est pas effectuée, le module WebSocket IIS tente de gérer la communication WebSocket, au lieu de celle de Node.js et de l’application.</span><span class="sxs-lookup"><span data-stu-id="15d33-205">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="15d33-206">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="15d33-206">Next steps</span></span>

<span data-ttu-id="15d33-207">[L’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) qui accompagne cet article est une application d’écho.</span><span class="sxs-lookup"><span data-stu-id="15d33-207">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="15d33-208">Elle comporte une page web qui établit des connexions WebSocket, et le serveur renvoie au client les messages qu’il reçoit.</span><span class="sxs-lookup"><span data-stu-id="15d33-208">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="15d33-209">Exécutez l’application à partir d’une invite de commandes (elle n’est pas configurée pour s’exécuter à partir de Visual Studio avec IIS Express) et accédez à http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="15d33-209">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="15d33-210">La page web affiche l’état de la connexion en haut à gauche :</span><span class="sxs-lookup"><span data-stu-id="15d33-210">The web page shows the connection status in the upper left:</span></span>

![État initial de la page web](websockets/_static/start.png)

<span data-ttu-id="15d33-212">Sélectionnez **Connect** (Connexion) pour envoyer une requête WebSocket à l’URL affichée.</span><span class="sxs-lookup"><span data-stu-id="15d33-212">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="15d33-213">Entrez un message de test, puis sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="15d33-213">Enter a test message and select **Send**.</span></span> <span data-ttu-id="15d33-214">Quand vous avez terminé, sélectionnez **Close Socket** (Fermer le socket).</span><span class="sxs-lookup"><span data-stu-id="15d33-214">When done, select **Close Socket**.</span></span> <span data-ttu-id="15d33-215">La section **Communication Log** (Journal des communications) signale chaque action d’ouverture, d’envoi et de fermeture en temps réel.</span><span class="sxs-lookup"><span data-stu-id="15d33-215">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![État initial de la page web](websockets/_static/end.png)
