---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Comprendre le processus d’exécution ASP.NET MVC (fr) Microsoft Docs
author: rick-anderson
description: Découvrez comment le cadre MVC ASP.NET traite une demande de navigateur étape par étape.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 48afbbe7349b80e0ed0b9bab987ae3ccda493aca
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541024"
---
# <a name="understanding-the-aspnet-mvc-execution-process"></a><span data-ttu-id="1f1ad-103">Présentation du processus d’exécution d’ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="1f1ad-103">Understanding the ASP.NET MVC Execution Process</span></span>

<span data-ttu-id="1f1ad-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1f1ad-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1f1ad-105">Découvrez comment le cadre MVC ASP.NET traite une demande de navigateur étape par étape.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-105">Learn how the ASP.NET MVC framework processes a browser request step-by-step.</span></span>

<span data-ttu-id="1f1ad-106">Les demandes d’une application Web ASP.NET basée sur MVC passent d’abord par l’objet **UrlRoutingModule,** qui est un module HTTP.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-106">Requests to an ASP.NET MVC-based Web application first pass through the **UrlRoutingModule** object, which is an HTTP module.</span></span> <span data-ttu-id="1f1ad-107">Ce module analyse la demande et exécute la sélection d'itinéraire.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-107">This module parses the request and performs route selection.</span></span> <span data-ttu-id="1f1ad-108">**L’objet UrlRoutingModule** sélectionne le premier objet d’itinéraire qui correspond à la demande en cours.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-108">The **UrlRoutingModule** object selects the first route object that matches the current request.</span></span> <span data-ttu-id="1f1ad-109">(Un objet d’itinéraire est une classe qui met en œuvre **RouteBase**, et est généralement une instance de la classe **Route.)** Si aucun itinéraire ne correspond, l’objet **UrlRoutingModule** ne fait rien et laisse la demande revenir au traitement régulier de la demande de ASP.NET ou de l’IIS.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-109">(A route object is a class that implements **RouteBase**, and is typically an instance of the **Route** class.) If no routes match, the **UrlRoutingModule** object does nothing and lets the request fall back to the regular ASP.NET or IIS request processing.</span></span>

<span data-ttu-id="1f1ad-110">De l’objet **Itinéraire** sélectionné, l’objet **UrlRoutingModule** obtient l’objet **IRouteHandler** associé à l’objet **Route.**</span><span class="sxs-lookup"><span data-stu-id="1f1ad-110">From the selected **Route** object, the **UrlRoutingModule** object obtains the **IRouteHandler** object that is associated with the **Route** object.</span></span> <span data-ttu-id="1f1ad-111">Typiquement, dans une application MVC, ce sera un cas de **MvcRouteHandler**.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-111">Typically, in an MVC application, this will be an instance of **MvcRouteHandler**.</span></span> <span data-ttu-id="1f1ad-112">**L’instance IRouteHandler** crée un objet **IHttpHandler** et lui passe l’objet **IHttpContext.**</span><span class="sxs-lookup"><span data-stu-id="1f1ad-112">The **IRouteHandler** instance creates an **IHttpHandler** object and passes it the **IHttpContext** object.</span></span> <span data-ttu-id="1f1ad-113">Par défaut, l’instance **IHttpHandler** pour MVC est l’objet **MvcHandler.**</span><span class="sxs-lookup"><span data-stu-id="1f1ad-113">By default, the **IHttpHandler** instance for MVC is the **MvcHandler** object.</span></span> <span data-ttu-id="1f1ad-114">**L’objet MvcHandler** sélectionne ensuite le contrôleur qui traitera finalement la demande.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-114">The **MvcHandler** object then selects the controller that will ultimately handle the request.</span></span>

> [!NOTE]
> <span data-ttu-id="1f1ad-115">Lorsqu'une application Web ASP.NET MVC s'exécute dans IIS 7.0, aucune extension de nom de fichier n'est requise pour les projets MVC.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-115">When an ASP.NET MVC Web application runs in IIS 7.0, no file name extension is required for MVC projects.</span></span> <span data-ttu-id="1f1ad-116">Toutefois, dans IIS 6.0, le gestionnaire requiert que vous mappiez l'extension de nom de fichier .mvc à la DLL ISAPI ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-116">However, in IIS 6.0, the handler requires that you map the .mvc file name extension to the ASP.NET ISAPI DLL.</span></span>

<span data-ttu-id="1f1ad-117">Le module et le gestionnaire sont les points d’entrée du cadre ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-117">The module and handler are the entry points to the ASP.NET MVC framework.</span></span> <span data-ttu-id="1f1ad-118">Elles exécutent les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="1f1ad-118">They perform the following actions:</span></span>

- <span data-ttu-id="1f1ad-119">Sélection du contrôleur approprié dans une application Web MVC.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-119">Select the appropriate controller in an MVC Web application.</span></span>
- <span data-ttu-id="1f1ad-120">Obtention d'une instance de contrôleur spécifique.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-120">Obtain a specific controller instance.</span></span>
- <span data-ttu-id="1f1ad-121">Appelez la méthode **Execute** du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-121">Call the controller's **Execute** method.</span></span>

<span data-ttu-id="1f1ad-122">Les étapes suivantes énumèrent les étapes d’exécution d’un projet Web de MVC :</span><span class="sxs-lookup"><span data-stu-id="1f1ad-122">The following lists the stages of execution for an MVC Web project:</span></span>

- <span data-ttu-id="1f1ad-123">Réception de la première demande concernant l'application</span><span class="sxs-lookup"><span data-stu-id="1f1ad-123">Receive first request for the application</span></span> 

    - <span data-ttu-id="1f1ad-124">Dans le fichier Global.asax, des objets **Route** sont ajoutés à l’objet **RouteTable.**</span><span class="sxs-lookup"><span data-stu-id="1f1ad-124">In the Global.asax file, **Route** objects are added to the **RouteTable** object.</span></span>
- <span data-ttu-id="1f1ad-125">Effectuer le routage</span><span class="sxs-lookup"><span data-stu-id="1f1ad-125">Perform routing</span></span> 

    - <span data-ttu-id="1f1ad-126">Le module **UrlRoutingModule** utilise le premier objet **Itinéraire** correspondant de la collection **RouteTable** pour créer l’objet **RouteData,** qu’il utilise ensuite pour créer un objet **RequestContext** (**IHttpContext).**</span><span class="sxs-lookup"><span data-stu-id="1f1ad-126">The **UrlRoutingModule** module uses the first matching **Route** object in the **RouteTable** collection to create the **RouteData** object, which it then uses to create a **RequestContext** (**IHttpContext**) object.</span></span>
- <span data-ttu-id="1f1ad-127">Création d'un gestionnaire de demandes MVC</span><span class="sxs-lookup"><span data-stu-id="1f1ad-127">Create MVC request handler</span></span> 

    - <span data-ttu-id="1f1ad-128">**L’objet MvcRouteHandler** crée une instance de la classe **MvcHandler** et lui passe l’instance **RequestContext.**</span><span class="sxs-lookup"><span data-stu-id="1f1ad-128">The **MvcRouteHandler** object creates an instance of the **MvcHandler** class and passes it the **RequestContext** instance.</span></span>
- <span data-ttu-id="1f1ad-129">Création du contrôleur</span><span class="sxs-lookup"><span data-stu-id="1f1ad-129">Create controller</span></span> 

    - <span data-ttu-id="1f1ad-130">**L’objet MvcHandler** utilise **l’instance RequestContext** pour identifier **l’objet IControllerFactory** (généralement une instance de la classe **DefaultControllerFactory)** pour créer l’instance du contrôleur avec.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-130">The **MvcHandler** object uses the **RequestContext** instance to identify the **IControllerFactory** object (typically an instance of the **DefaultControllerFactory** class) to create the controller instance with.</span></span>
- <span data-ttu-id="1f1ad-131">Exécuter le contrôleur - L’instance **MvcHandler** appelle la méthode **d’exécution** du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-131">Execute controller - The **MvcHandler** instance calls the controller s **Execute** method.</span></span> |
- <span data-ttu-id="1f1ad-132">Appel de l'action</span><span class="sxs-lookup"><span data-stu-id="1f1ad-132">Invoke action</span></span> 

    - <span data-ttu-id="1f1ad-133">La plupart des contrôleurs héritent de la classe de base **Controller.**</span><span class="sxs-lookup"><span data-stu-id="1f1ad-133">Most controllers inherit from the **Controller** base class.</span></span> <span data-ttu-id="1f1ad-134">Pour les contrôleurs qui le font, l’objet **ControllerActionInvoker** qui est associé au contrôleur détermine la méthode d’action de la classe de contrôleur à appeler, puis appelle cette méthode.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-134">For controllers that do so, the **ControllerActionInvoker** object that is associated with the controller determines which action method of the controller class to call, and then calls that method.</span></span>
- <span data-ttu-id="1f1ad-135">Exécution du résultat</span><span class="sxs-lookup"><span data-stu-id="1f1ad-135">Execute result</span></span> 

    - <span data-ttu-id="1f1ad-136">Une méthode d’action typique peut recevoir l’entrée de l’utilisateur, préparer les données de réponse appropriées, puis exécuter le résultat en retournant un type de résultat.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-136">A typical action method might receive user input, prepare the appropriate response data, and then execute the result by returning a result type.</span></span> <span data-ttu-id="1f1ad-137">Les types de résultats intégrés qui peuvent être exécutés comprennent ce qui suit: **ViewResult** (qui rend une vue et est le type de résultat le plus souvent utilisé), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, et **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="1f1ad-137">The built-in result types that can be executed include the following: **ViewResult** (which renders a view and is the most-often used result type), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, and **EmptyResult**.</span></span>
