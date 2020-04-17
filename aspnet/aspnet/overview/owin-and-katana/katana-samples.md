---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Échantillons de Katana Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 15cc1084b16db2619f2295ee21dec4f49eb2e354
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540439"
---
# <a name="katana-samples"></a><span data-ttu-id="cb5ed-102">Exemples Katana</span><span class="sxs-lookup"><span data-stu-id="cb5ed-102">Katana Samples</span></span>

<span data-ttu-id="cb5ed-103">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="cb5ed-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="cb5ed-104">Exemples Katana</span><span class="sxs-lookup"><span data-stu-id="cb5ed-104">Katana Samples</span></span>

<span data-ttu-id="cb5ed-105">**ASP.NET Itinéraires Code** | [source d’échantillons](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span><span class="sxs-lookup"><span data-stu-id="cb5ed-105">**ASP.NET Routes Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span></span>  
<span data-ttu-id="cb5ed-106">Dans certaines applications, vous voudrez brancher les composants OWIN dans la table d’itinéraire Asp.Net côte à côte avec des composants non-OWIN.</span><span class="sxs-lookup"><span data-stu-id="cb5ed-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="cb5ed-107">Cet exemple montre comment utiliser les méthodes d’extension RouteCollection MapOwinPath et MapOwinRoute fournies par Microsoft.Owin.Host.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="cb5ed-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="cb5ed-108">**Branchage pipelines Code source** | [d’échantillons](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span><span class="sxs-lookup"><span data-stu-id="cb5ed-108">**Branching Pipelines Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span></span>  
<span data-ttu-id="cb5ed-109">Les pipelines de traitement des demandes OWIN n’ont pas besoin d’être linéaires, ils peuvent être ramifiés pour traiter les demandes de différentes façons.</span><span class="sxs-lookup"><span data-stu-id="cb5ed-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="cb5ed-110">Cet exemple montre comment construire un pipeline de branchement en fonction des trajectoires de demande ou d’autres données de demande telles que les en-têtes.</span><span class="sxs-lookup"><span data-stu-id="cb5ed-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="cb5ed-111">Ces composants sont disponibles dans le paquet de nuget Microsoft.Owin.Mapping.</span><span class="sxs-lookup"><span data-stu-id="cb5ed-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="cb5ed-112">**Code source d’échantillon** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) de serveur personnalisé </span><span class="sxs-lookup"><span data-stu-id="cb5ed-112">**Custom Server Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span></span>  
<span data-ttu-id="cb5ed-113">Montre comment utiliser un serveur OWIN personnalisé lors de l’auto-hébergement OWIN.</span><span class="sxs-lookup"><span data-stu-id="cb5ed-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="cb5ed-114">**Code source d’échantillons** | [intégrés](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span><span class="sxs-lookup"><span data-stu-id="cb5ed-114">**Embedded Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span></span>  
<span data-ttu-id="cb5ed-115">Certains serveurs OWIN peuvent être exécutés&quot;à l’intérieur de votre propre processus (auto-hébergé ).&quot;</span><span class="sxs-lookup"><span data-stu-id="cb5ed-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="cb5ed-116">Cet exemple montre comment démarrer une application OWIN à l’aide des outils fournis par le paquet de nuget Microsoft.Owin.Hosting.</span><span class="sxs-lookup"><span data-stu-id="cb5ed-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="cb5ed-117">**Code source d’échantillons** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld) HelloWorld</span><span class="sxs-lookup"><span data-stu-id="cb5ed-117">**HelloWorld Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span></span>  
<span data-ttu-id="cb5ed-118">OWIN est un serveur HTTP API abstraction qui permet la portabilité d’application sur différents serveurs.</span><span class="sxs-lookup"><span data-stu-id="cb5ed-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="cb5ed-119">Cet exemple montre comment écrire une application Hello World en utilisant quelques **emballages simples** autour de l’abstraction OWIN brute et l’exécuter sur un serveur web comme ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cb5ed-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="cb5ed-120">**Bonjour World Raw OWIN Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span><span class="sxs-lookup"><span data-stu-id="cb5ed-120">**Hello World Raw OWIN Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span></span>  
<span data-ttu-id="cb5ed-121">Cet exemple montre comment écrire une application Hello World en utilisant l’abstraction **OWIN brute** et l’exécuter sur un serveur web comme Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="cb5ed-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="cb5ed-122">**Code source d’échantillon** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR) SignalR</span><span class="sxs-lookup"><span data-stu-id="cb5ed-122">**SignalR Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span></span>  
<span data-ttu-id="cb5ed-123">Montre comment s’auto-héberger SignalR en utilisant OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="cb5ed-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="cb5ed-124">Pour plus d’informations sur l’auto-hébergement SignalR, voir [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="cb5ed-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="cb5ed-125">**Code source d’échantillons** | de fichiers[statiques](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span><span class="sxs-lookup"><span data-stu-id="cb5ed-125">**Static Files Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span></span>  
<span data-ttu-id="cb5ed-126">Montre comment prendre en charge les demandes HTTP de fichiers statiques à l’aide d’OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="cb5ed-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="cb5ed-127">**Web API** | [Code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) Web API </span><span class="sxs-lookup"><span data-stu-id="cb5ed-127">**Web API** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span></span>  
<span data-ttu-id="cb5ed-128">Cet exemple montre comment héberger OWIN dans l’IIS et ajouter l’API Web au pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="cb5ed-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="cb5ed-129">**Code source d’échantillon** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) Web Socket </span><span class="sxs-lookup"><span data-stu-id="cb5ed-129">**Web Socket Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span></span>  
<span data-ttu-id="cb5ed-130">Montre comment prendre en charge les prises Web dans OWIN en utilisant la classe [System.Net.WebSockets.WebSocket.](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="cb5ed-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
