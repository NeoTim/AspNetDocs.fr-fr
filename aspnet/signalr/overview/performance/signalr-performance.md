---
uid: signalr/overview/performance/signalr-performance
title: Performance Signalr Microsoft Docs
author: bradygaster
description: Performances de SignalR
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: b8a44f4c924c94cdfff1ce7630539b45fe269bbf
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676089"
---
# <a name="signalr-performance"></a><span data-ttu-id="0206e-103">Performances de SignalR</span><span class="sxs-lookup"><span data-stu-id="0206e-103">SignalR Performance</span></span>

<span data-ttu-id="0206e-104">par [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="0206e-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="0206e-105">Ce sujet décrit comment concevoir, mesurer et améliorer les performances dans une application SignalR.</span><span class="sxs-lookup"><span data-stu-id="0206e-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="0206e-106">Versions logicielles utilisées dans ce sujet</span><span class="sxs-lookup"><span data-stu-id="0206e-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="0206e-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0206e-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="0206e-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="0206e-108">.NET 4.5</span></span>
> - <span data-ttu-id="0206e-109">Version SignalR 2</span><span class="sxs-lookup"><span data-stu-id="0206e-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="0206e-110">Versions précédentes de ce sujet</span><span class="sxs-lookup"><span data-stu-id="0206e-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="0206e-111">Pour plus d’informations sur les versions antérieures de SignalR, voir [SignalR Older Versions](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="0206e-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="0206e-112">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="0206e-112">Questions and comments</span></span>
>
> <span data-ttu-id="0206e-113">S’il vous plaît laisser des commentaires sur la façon dont vous avez aimé ce tutoriel et ce que nous pourrions améliorer dans les commentaires au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="0206e-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="0206e-114">Si vous avez des questions qui ne sont pas directement liées au tutoriel, vous pouvez les poster sur le [ASP.NET Forum SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="0206e-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="0206e-115">Pour une présentation récente sur la performance SignalR et la mise à l’échelle, voir [L’échelle du Web en temps réel avec ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="0206e-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="0206e-116">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="0206e-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="0206e-117">Remarques relatives à la conception</span><span class="sxs-lookup"><span data-stu-id="0206e-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="0206e-118">Réglage de votre serveur SignalR pour les performances</span><span class="sxs-lookup"><span data-stu-id="0206e-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="0206e-119">Résolution des problèmes de performances</span><span class="sxs-lookup"><span data-stu-id="0206e-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="0206e-120">Utilisation de compteurs de performance SignalR</span><span class="sxs-lookup"><span data-stu-id="0206e-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="0206e-121">Utilisation d’autres compteurs de performance</span><span class="sxs-lookup"><span data-stu-id="0206e-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="0206e-122">Autres ressources</span><span class="sxs-lookup"><span data-stu-id="0206e-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="0206e-123">Remarques relatives à la conception</span><span class="sxs-lookup"><span data-stu-id="0206e-123">Design considerations</span></span>

<span data-ttu-id="0206e-124">Cette section décrit les modèles qui peuvent être mis en œuvre lors de la conception d’une application SignalR, afin de s’assurer que les performances ne sont pas entravées par la génération de trafic réseau inutile.</span><span class="sxs-lookup"><span data-stu-id="0206e-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="0206e-125">Fréquence de message de limitation</span><span class="sxs-lookup"><span data-stu-id="0206e-125">Throttling message frequency</span></span>

<span data-ttu-id="0206e-126">Même dans une application qui envoie des messages à haute fréquence (comme une application de jeu en temps réel), la plupart des applications n’ont pas besoin d’envoyer plus de quelques messages par seconde.</span><span class="sxs-lookup"><span data-stu-id="0206e-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="0206e-127">Pour réduire la quantité de trafic que chaque client génère, une boucle de message peut être implémentée que les files d’attente et envoie des messages pas plus fréquemment qu’un taux fixe (c’est-à-dire que jusqu’à un certain nombre de messages seront envoyés chaque seconde, s’il y a des messages dans cet intervalle de temps à envoyer).</span><span class="sxs-lookup"><span data-stu-id="0206e-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="0206e-128">Pour une application d’échantillon qui limite les messages à un certain taux (à partir du client et du serveur), voir [High-Frequency Realtime avec SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="0206e-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="0206e-129">Réduire la taille du message</span><span class="sxs-lookup"><span data-stu-id="0206e-129">Reducing message size</span></span>

<span data-ttu-id="0206e-130">Vous pouvez réduire la taille d’un message SignalR en réduisant la taille de vos objets sérialisés.</span><span class="sxs-lookup"><span data-stu-id="0206e-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="0206e-131">Dans le code serveur, si vous envoyez un objet qui contient des propriétés qui n’ont `JsonIgnore` pas besoin d’être transmises, empêcher ces propriétés d’être sérialisées en utilisant l’attribut.</span><span class="sxs-lookup"><span data-stu-id="0206e-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="0206e-132">Les noms des propriétés sont également stockés dans le message; les noms des propriétés peuvent `JsonProperty` être raccourcis à l’aide de l’attribut.</span><span class="sxs-lookup"><span data-stu-id="0206e-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="0206e-133">L’échantillon de code suivant montre comment exclure une propriété d’être envoyée au client, et comment raccourcir les noms de propriété :</span><span class="sxs-lookup"><span data-stu-id="0206e-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="0206e-134">**.NET code serveur qui démontre l’attribut JsonIgnore pour exclure les données d’être envoyé au client, et l’attribut JsonProperty pour réduire la taille du message**</span><span class="sxs-lookup"><span data-stu-id="0206e-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="0206e-135">Afin de conserver la lisibilité/la maintenance du code client, les noms de propriétés abrégées peuvent être rembpés sur des noms adaptés à l’homme après la réception du message.</span><span class="sxs-lookup"><span data-stu-id="0206e-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="0206e-136">L’échantillon de code suivant démontre un moyen possible de réapprevrer les noms raccourcis `reMap` à des noms plus longs, en définissant un contrat de message (cartographie), et en utilisant la fonction pour appliquer le contrat à la classe de message optimisée :</span><span class="sxs-lookup"><span data-stu-id="0206e-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="0206e-137">**Code JavaScript côté client qui remaps raccourci les noms de propriété à des noms lisibles par l’homme**</span><span class="sxs-lookup"><span data-stu-id="0206e-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="0206e-138">Les noms peuvent également être raccourcis dans les messages du client au serveur, en utilisant la même méthode.</span><span class="sxs-lookup"><span data-stu-id="0206e-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="0206e-139">La réduction de l’empreinte mémoire (c’est-à-dire la quantité de mémoire utilisée pour le message) de l’objet du message peut également améliorer les performances.</span><span class="sxs-lookup"><span data-stu-id="0206e-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="0206e-140">Par exemple, si la `int` gamme complète d’un n’est pas nécessaire, a `short` ou `byte` peut être utilisé à la place.</span><span class="sxs-lookup"><span data-stu-id="0206e-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="0206e-141">Étant donné que les messages sont stockés dans le bus de message dans la mémoire du serveur, la réduction de la taille des messages peut également résoudre les problèmes de mémoire du serveur.</span><span class="sxs-lookup"><span data-stu-id="0206e-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="0206e-142">Réglage de votre serveur SignalR pour les performances</span><span class="sxs-lookup"><span data-stu-id="0206e-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="0206e-143">Les paramètres de configuration suivants peuvent être utilisés pour régler votre serveur pour de meilleures performances dans une application SignalR.</span><span class="sxs-lookup"><span data-stu-id="0206e-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="0206e-144">Pour plus d’informations générales sur la façon d’améliorer les performances dans une application ASP.NET, voir [Améliorer ASP.NET performance](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="0206e-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="0206e-145">**Paramètres de configuration SignalR**</span><span class="sxs-lookup"><span data-stu-id="0206e-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="0206e-146">**DefaultMessageBufferSize**: Par défaut, SignalR conserve 1000 messages dans la mémoire par hub par connexion.</span><span class="sxs-lookup"><span data-stu-id="0206e-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="0206e-147">Si de gros messages sont utilisés, cela peut créer des problèmes de mémoire qui peuvent être atténués en réduisant cette valeur.</span><span class="sxs-lookup"><span data-stu-id="0206e-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="0206e-148">Ce paramètre peut `Application_Start` être défini dans le gestionnaire d’événements `Configuration` dans une application ASP.NET, ou dans la méthode d’une classe de démarrage OWIN dans une application auto-hébergée.</span><span class="sxs-lookup"><span data-stu-id="0206e-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="0206e-149">L’échantillon suivant montre comment réduire cette valeur afin de réduire l’empreinte mémoire de votre application afin de réduire la quantité de mémoire du serveur utilisée :</span><span class="sxs-lookup"><span data-stu-id="0206e-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="0206e-150">**.NET code serveur en Startup.cs pour la diminution de la taille de mémoire tampon message par défaut**</span><span class="sxs-lookup"><span data-stu-id="0206e-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="0206e-151">**Paramètres de configuration IIS**</span><span class="sxs-lookup"><span data-stu-id="0206e-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="0206e-152">**Demandes simultanées maximales par application**: L’augmentation du nombre de demandes d’IIS simultanées augmentera les ressources du serveur disponibles pour les demandes de service.</span><span class="sxs-lookup"><span data-stu-id="0206e-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="0206e-153">La valeur par défaut est de 5000; pour augmenter ce paramètre, exécutez les commandes suivantes dans une invite de commande élevée :</span><span class="sxs-lookup"><span data-stu-id="0206e-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="0206e-154">**ApplicationPool QueueLength**: Il s’agit du nombre maximum de demandes que Http.sys files d’attente pour le pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="0206e-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="0206e-155">Lorsque la file d’attente est complète, les nouvelles demandes reçoivent une réponse 503 « Service Indisponible ».</span><span class="sxs-lookup"><span data-stu-id="0206e-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="0206e-156">La valeur par défaut est 1000.</span><span class="sxs-lookup"><span data-stu-id="0206e-156">The default value is 1000.</span></span>

    <span data-ttu-id="0206e-157">Le raccourcissement de la longueur de file d’attente pour le processus de travail dans le pool d’applications hébergeant votre application permettra de conserver les ressources de mémoire.</span><span class="sxs-lookup"><span data-stu-id="0206e-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="0206e-158">Pour plus d’informations, voir [Gérer, Tuning et Configurer les pools d’applications](https://technet.microsoft.com/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="0206e-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="0206e-159">**ASP.NET paramètres de configuration**</span><span class="sxs-lookup"><span data-stu-id="0206e-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="0206e-160">Cette section comprend les paramètres de `aspnet.config` configuration qui peuvent être définis dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="0206e-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="0206e-161">Ce fichier se trouve dans l’un des deux emplacements, selon la plate-forme:</span><span class="sxs-lookup"><span data-stu-id="0206e-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="0206e-162">ASP.NET paramètres qui peuvent améliorer les performances de SignalR comprennent les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="0206e-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="0206e-163">**Demandes simultanées maximales par processeur**: L’augmentation de ce paramètre peut atténuer les goulots d’étranglement des performances.</span><span class="sxs-lookup"><span data-stu-id="0206e-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="0206e-164">Pour augmenter ce paramètre, ajoutez `aspnet.config` le paramètre de configuration suivant au fichier :</span><span class="sxs-lookup"><span data-stu-id="0206e-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="0206e-165">**Limite de file d’attente**de demande `maxConcurrentRequestsPerCPU` : Lorsque le nombre total de connexions dépasse le paramètre, ASP.NET commencera à étrangler les demandes à l’aide d’une file d’attente.</span><span class="sxs-lookup"><span data-stu-id="0206e-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="0206e-166">Pour augmenter la taille de la `requestQueueLimit` file d’attente, vous pouvez augmenter le réglage.</span><span class="sxs-lookup"><span data-stu-id="0206e-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="0206e-167">Pour ce faire, ajoutez le `processModel` réglage de `config/machine.config` configuration `aspnet.config`suivant au nœud (plutôt que ):</span><span class="sxs-lookup"><span data-stu-id="0206e-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="0206e-168">Résolution des problèmes de performances</span><span class="sxs-lookup"><span data-stu-id="0206e-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="0206e-169">Cette section décrit les moyens de trouver des goulots d’étranglement de performance dans votre application.</span><span class="sxs-lookup"><span data-stu-id="0206e-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="0206e-170">Vérifier que WebSocket est utilisé</span><span class="sxs-lookup"><span data-stu-id="0206e-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="0206e-171">Bien que SignalR puisse utiliser une variété de transports pour la communication entre le client et le serveur, WebSocket offre un avantage de performance significatif, et doit être utilisé si le client et le serveur le prennent en charge.</span><span class="sxs-lookup"><span data-stu-id="0206e-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="0206e-172">Pour déterminer si votre client et votre serveur répondent aux exigences de WebSocket, consultez [Transports et Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="0206e-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="0206e-173">Pour déterminer quel transport est utilisé dans votre application, vous pouvez utiliser les outils de développeur de navigateur, et examiner les journaux pour voir quel transport est utilisé pour la connexion.</span><span class="sxs-lookup"><span data-stu-id="0206e-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="0206e-174">Pour plus d’informations sur l’utilisation des outils de développement du navigateur dans Internet Explorer et Chrome, voir [Transports et Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="0206e-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="0206e-175">Utilisation de compteurs de performance SignalR</span><span class="sxs-lookup"><span data-stu-id="0206e-175">Using SignalR performance counters</span></span>

<span data-ttu-id="0206e-176">Cette section décrit comment activer et utiliser les compteurs de performance SignalR, que l’on trouve dans l’emballage. `Microsoft.AspNet.SignalR.Utils`</span><span class="sxs-lookup"><span data-stu-id="0206e-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="0206e-177">Installation signalur.exe</span><span class="sxs-lookup"><span data-stu-id="0206e-177">Installing signalr.exe</span></span>

<span data-ttu-id="0206e-178">Les compteurs de performance peuvent être ajoutés au serveur à l’aide d’un utilitaire appelé SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="0206e-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="0206e-179">Pour installer cet utilitaire, suivez ces étapes :</span><span class="sxs-lookup"><span data-stu-id="0206e-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="0206e-180">Dans Visual Studio, sélectionnez **Tools** > **NuGet Package Manager** > **Gérer les paquets NuGet pour la solution**</span><span class="sxs-lookup"><span data-stu-id="0206e-180">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="0206e-181">Recherchez **signalr.utils**, et sélectionnez Installer.</span><span class="sxs-lookup"><span data-stu-id="0206e-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="0206e-182">Accepter l’accord de licence pour installer le paquet.</span><span class="sxs-lookup"><span data-stu-id="0206e-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="0206e-183">SignalR.exe sera installé `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`pour .</span><span class="sxs-lookup"><span data-stu-id="0206e-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="0206e-184">Installation de compteurs de performance avec SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="0206e-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="0206e-185">Pour installer des compteurs de performance SignalR, exécutez SignalR.exe dans une invite de commande élevée avec le paramètre suivant :</span><span class="sxs-lookup"><span data-stu-id="0206e-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="0206e-186">Pour supprimer les compteurs de performance SignalR, exécutez SignalR.exe dans une invite de commande élevée avec le paramètre suivant :</span><span class="sxs-lookup"><span data-stu-id="0206e-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="0206e-187">Compteurs SignalR Performance</span><span class="sxs-lookup"><span data-stu-id="0206e-187">SignalR Performance counters</span></span>

<span data-ttu-id="0206e-188">Le paquet utilitaires installe les compteurs de performance suivants.</span><span class="sxs-lookup"><span data-stu-id="0206e-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="0206e-189">Les compteurs "Total" mesurent le nombre d’événements depuis le dernier pool d’applications ou le redémarrage du serveur.</span><span class="sxs-lookup"><span data-stu-id="0206e-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="0206e-190">**Métriques de connexion**</span><span class="sxs-lookup"><span data-stu-id="0206e-190">**Connection metrics**</span></span>

<span data-ttu-id="0206e-191">Les mesures suivantes mesurent les événements de la durée de vie des connexions qui se produisent.</span><span class="sxs-lookup"><span data-stu-id="0206e-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="0206e-192">Pour plus d’informations, voir [Comprendre et gérer les événements à vie de connexion](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="0206e-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="0206e-193">**Connexions connectées**</span><span class="sxs-lookup"><span data-stu-id="0206e-193">**Connections Connected**</span></span>
- <span data-ttu-id="0206e-194">**Connexions reconnectées**</span><span class="sxs-lookup"><span data-stu-id="0206e-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="0206e-195">**Connexions déconnectées**</span><span class="sxs-lookup"><span data-stu-id="0206e-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="0206e-196">**Connexions actuelles**</span><span class="sxs-lookup"><span data-stu-id="0206e-196">**Connections Current**</span></span>

<span data-ttu-id="0206e-197">**Métriques de message**</span><span class="sxs-lookup"><span data-stu-id="0206e-197">**Message metrics**</span></span>

<span data-ttu-id="0206e-198">Les mesures suivantes mesurent le trafic de messages généré par SignalR.</span><span class="sxs-lookup"><span data-stu-id="0206e-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="0206e-199">**Messages de connexion reçus Total**</span><span class="sxs-lookup"><span data-stu-id="0206e-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="0206e-200">**Messages de connexion envoyés Total**</span><span class="sxs-lookup"><span data-stu-id="0206e-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="0206e-201">**Messages de connexion reçus/Sec**</span><span class="sxs-lookup"><span data-stu-id="0206e-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="0206e-202">**Messages de connexion Envoyés/Sec**</span><span class="sxs-lookup"><span data-stu-id="0206e-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="0206e-203">**Mesures d’autobus de message**</span><span class="sxs-lookup"><span data-stu-id="0206e-203">**Message bus metrics**</span></span>

<span data-ttu-id="0206e-204">Les mesures suivantes mesurent le trafic à travers le bus à messages SignalR interne, la file d’attente dans laquelle tous les messages SignalR entrants et sortants sont placés.</span><span class="sxs-lookup"><span data-stu-id="0206e-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="0206e-205">Un message est **publié** lorsqu’il est envoyé ou diffusé.</span><span class="sxs-lookup"><span data-stu-id="0206e-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="0206e-206">Un **abonné** dans ce contexte est un abonnement sur le bus message; cela devrait égaler le nombre de clients plus le serveur lui-même.</span><span class="sxs-lookup"><span data-stu-id="0206e-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="0206e-207">Un **travailleur alloué** est un composant qui envoie des données à des connexions actives; un **travailleur occupé** est un travailleur qui envoie activement un message.</span><span class="sxs-lookup"><span data-stu-id="0206e-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="0206e-208">**Messages d’autobus reçus Total**</span><span class="sxs-lookup"><span data-stu-id="0206e-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="0206e-209">**Messages d’autobus reçus/Sec**</span><span class="sxs-lookup"><span data-stu-id="0206e-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="0206e-210">**Messages d’autobus publiés Total**</span><span class="sxs-lookup"><span data-stu-id="0206e-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="0206e-211">**Messages d’autobus publiés/Sec**</span><span class="sxs-lookup"><span data-stu-id="0206e-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="0206e-212">**Abonnés Message Bus En cours**</span><span class="sxs-lookup"><span data-stu-id="0206e-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="0206e-213">**Abonnés Message Bus Total**</span><span class="sxs-lookup"><span data-stu-id="0206e-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="0206e-214">**Message Bus Abonnés/Sec**</span><span class="sxs-lookup"><span data-stu-id="0206e-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="0206e-215">**Message Bus Allocated Workers**</span><span class="sxs-lookup"><span data-stu-id="0206e-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="0206e-216">**Message Bus Travailleurs occupés**</span><span class="sxs-lookup"><span data-stu-id="0206e-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="0206e-217">**Sujets d’autobus de message actuels**</span><span class="sxs-lookup"><span data-stu-id="0206e-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="0206e-218">**Mesures d’erreur**</span><span class="sxs-lookup"><span data-stu-id="0206e-218">**Error metrics**</span></span>

<span data-ttu-id="0206e-219">Les mesures suivantes mesurent les erreurs générées par le trafic de messages SignalR.</span><span class="sxs-lookup"><span data-stu-id="0206e-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="0206e-220">**Les** erreurs de résolution du Hub se produisent lorsqu’une méthode de moyeu ou de moyeu ne peut pas être résolue.</span><span class="sxs-lookup"><span data-stu-id="0206e-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="0206e-221">Les erreurs **d’invocation du Hub** sont des exceptions lancées lors de l’invocation d’une méthode de moyeu.</span><span class="sxs-lookup"><span data-stu-id="0206e-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="0206e-222">**Les** erreurs de transport sont des erreurs de connexion lancées lors d’une demande ou d’une réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="0206e-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="0206e-223">**Erreurs: Tous les total**</span><span class="sxs-lookup"><span data-stu-id="0206e-223">**Errors: All Total**</span></span>
- <span data-ttu-id="0206e-224">**Erreurs: All/Sec**</span><span class="sxs-lookup"><span data-stu-id="0206e-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="0206e-225">**Erreurs: Résolution du hub Total**</span><span class="sxs-lookup"><span data-stu-id="0206e-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="0206e-226">**Erreurs: Résolution du Hub/Sec**</span><span class="sxs-lookup"><span data-stu-id="0206e-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="0206e-227">**Erreurs: Hub Invocation Total**</span><span class="sxs-lookup"><span data-stu-id="0206e-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="0206e-228">**Erreurs: Hub Invocation/Sec**</span><span class="sxs-lookup"><span data-stu-id="0206e-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="0206e-229">**Erreurs: Transport Total**</span><span class="sxs-lookup"><span data-stu-id="0206e-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="0206e-230">**Erreurs: Transport/Sec**</span><span class="sxs-lookup"><span data-stu-id="0206e-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="0206e-231">**Mesures d’échelle**</span><span class="sxs-lookup"><span data-stu-id="0206e-231">**Scaleout metrics**</span></span>

<span data-ttu-id="0206e-232">Les mesures suivantes mesurent le trafic et les erreurs générées par le fournisseur de scaleout.</span><span class="sxs-lookup"><span data-stu-id="0206e-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="0206e-233">Un **flux** dans ce contexte est une unité d’échelle utilisée par le fournisseur d’échelle; il s’agit d’une table si SQL Server est utilisé, un sujet si Service Bus est utilisé, et un abonnement si Redis est utilisé.</span><span class="sxs-lookup"><span data-stu-id="0206e-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="0206e-234">Chaque flux assure la lecture et l’écriture des opérations commandées; un seul flux est un goulot d’étranglement à l’échelle potentielle, de sorte que le nombre de flux peut être augmenté pour aider à réduire ce goulot d’étranglement.</span><span class="sxs-lookup"><span data-stu-id="0206e-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="0206e-235">Si plusieurs flux sont utilisés, SignalR distribuera automatiquement des messages (shard) sur ces flux d’une manière qui garantit que les messages envoyés à partir d’une connexion donnée sont en ordre.</span><span class="sxs-lookup"><span data-stu-id="0206e-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="0206e-236">Le paramètre [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) contrôle la longueur de la file d’attente d’envoi d’échelle entretenue par SignalR.</span><span class="sxs-lookup"><span data-stu-id="0206e-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="0206e-237">Le définir à une valeur supérieure à 0 placera tous les messages dans une file d’attente d’envoi pour être envoyé un à la fois à l’avion de messagerie configuré.</span><span class="sxs-lookup"><span data-stu-id="0206e-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="0206e-238">Si la taille de la file d’attente dépasse la longueur configurée, les appels ultérieurs à envoyer échoueront immédiatement avec une [analyse d’opération invalideEnlaçation](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) jusqu’à ce que le nombre de messages dans la file d’attente soit à nouveau inférieur au réglage.</span><span class="sxs-lookup"><span data-stu-id="0206e-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="0206e-239">La file d’attente est désactivée par défaut parce que les backplanes mis en œuvre ont généralement leur propre file d’attente ou de contrôle du débit en place.</span><span class="sxs-lookup"><span data-stu-id="0206e-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="0206e-240">Dans le cas de SQL Server, la mise en commun des connexions limite effectivement le nombre d’envois en cours à tout moment.</span><span class="sxs-lookup"><span data-stu-id="0206e-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="0206e-241">Par défaut, un seul flux est utilisé pour SQL Server et Redis, cinq flux sont utilisés pour Service Bus, et la file d’attente est désactivée, mais ces paramètres peuvent être modifiés grâce à la configuration sur SQL Server et Service Bus:</span><span class="sxs-lookup"><span data-stu-id="0206e-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="0206e-242">**.NET Server Code pour configurer le nombre de tables et la longueur de file d’attente pour l’avion de fond SQL Server**</span><span class="sxs-lookup"><span data-stu-id="0206e-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="0206e-243">**.NET Code serveur pour configurer le nombre de sujets et la longueur de file d’attente pour l’avion de soutien de bus de service**</span><span class="sxs-lookup"><span data-stu-id="0206e-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="0206e-244">Un flux **tamponné** est celui qui est entré dans un état défectueux; lorsque le flux est dans l’état défectueux, tous les messages envoyés à l’avion de fond échoueront immédiatement jusqu’à ce que le flux ne soit plus défectueux.</span><span class="sxs-lookup"><span data-stu-id="0206e-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="0206e-245">La **durée de file d’attente d’envoi** est le nombre de messages qui ont été postés mais pas encore envoyés.</span><span class="sxs-lookup"><span data-stu-id="0206e-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="0206e-246">**Messages d’autobus à l’échelle reçus/Sec**</span><span class="sxs-lookup"><span data-stu-id="0206e-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="0206e-247">**Scaleout Streams Total**</span><span class="sxs-lookup"><span data-stu-id="0206e-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="0206e-248">**Streams Scaleout Ouvert**</span><span class="sxs-lookup"><span data-stu-id="0206e-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="0206e-249">**Scaleout Streams Buffering**</span><span class="sxs-lookup"><span data-stu-id="0206e-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="0206e-250">**Erreurs d’échelle Total**</span><span class="sxs-lookup"><span data-stu-id="0206e-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="0206e-251">**Erreurs d’échelle/Sec**</span><span class="sxs-lookup"><span data-stu-id="0206e-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="0206e-252">**Scaleout Envoyer la longueur de file d’attente**</span><span class="sxs-lookup"><span data-stu-id="0206e-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="0206e-253">Pour plus d’informations sur ce que ces compteurs mesurent, voir [SignalR Scaleout avec Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="0206e-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="0206e-254">Utilisation d’autres compteurs de performance</span><span class="sxs-lookup"><span data-stu-id="0206e-254">Using other performance counters</span></span>

<span data-ttu-id="0206e-255">Les compteurs de performances suivants peuvent également être utiles pour surveiller les performances de votre application.</span><span class="sxs-lookup"><span data-stu-id="0206e-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="0206e-256">**Mémoire**</span><span class="sxs-lookup"><span data-stu-id="0206e-256">**Memory**</span></span>

- <span data-ttu-id="0206e-257">.NET CLR\\Memory ' octets dans tous les tas (pour w3wp)</span><span class="sxs-lookup"><span data-stu-id="0206e-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="0206e-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="0206e-258">**ASP.NET**</span></span>

- <span data-ttu-id="0206e-259">ASP.NET-Demandes actuelles</span><span class="sxs-lookup"><span data-stu-id="0206e-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="0206e-260">ASP.NET-File</span><span class="sxs-lookup"><span data-stu-id="0206e-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="0206e-261">ASP.NET-Rejeté</span><span class="sxs-lookup"><span data-stu-id="0206e-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="0206e-262">**UC**</span><span class="sxs-lookup"><span data-stu-id="0206e-262">**CPU**</span></span>

- <span data-ttu-id="0206e-263">Informations processeur-Processeur Temps</span><span class="sxs-lookup"><span data-stu-id="0206e-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="0206e-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="0206e-264">**TCP/IP**</span></span>

- <span data-ttu-id="0206e-265">TCPv6/Connexions établies</span><span class="sxs-lookup"><span data-stu-id="0206e-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="0206e-266">TCPv4/Connexions établies</span><span class="sxs-lookup"><span data-stu-id="0206e-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="0206e-267">**Web Service**</span><span class="sxs-lookup"><span data-stu-id="0206e-267">**Web Service**</span></span>

- <span data-ttu-id="0206e-268">Service Web-Connexions actuelles</span><span class="sxs-lookup"><span data-stu-id="0206e-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="0206e-269">Service Web-Connexions maximales</span><span class="sxs-lookup"><span data-stu-id="0206e-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="0206e-270">**Thread**</span><span class="sxs-lookup"><span data-stu-id="0206e-270">**Threading**</span></span>

- <span data-ttu-id="0206e-271">.NET CLR Locks\\And Threads - de threads logiques actuels</span><span class="sxs-lookup"><span data-stu-id="0206e-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="0206e-272">.NET CLR Locks\\And Threads - de threads physiques actuels</span><span class="sxs-lookup"><span data-stu-id="0206e-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="0206e-273">Autres ressources</span><span class="sxs-lookup"><span data-stu-id="0206e-273">Other Resources</span></span>

<span data-ttu-id="0206e-274">Pour plus d’informations sur ASP.NET le suivi et l’accord des performances, consultez les sujets suivants :</span><span class="sxs-lookup"><span data-stu-id="0206e-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="0206e-275">[Vue d’ensemble des performances ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="0206e-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="0206e-276">ASP.NET Utilisation du fil sur IIS 7.5, IIS 7.0, et IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="0206e-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="0206e-277">&lt;applicationPool&gt; Element (Paramètres Web)</span><span class="sxs-lookup"><span data-stu-id="0206e-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
