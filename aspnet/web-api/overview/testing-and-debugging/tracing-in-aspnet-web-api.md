---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Traçage dans API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Montre comment activer le traçage dans API Web ASP.NET.
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a01acb649556d06ab9828ceab0fcbdf363bbc0d1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598552"
---
# <a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="ce1c8-103">Traçage dans API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="ce1c8-103">Tracing in ASP.NET Web API 2</span></span>

<span data-ttu-id="ce1c8-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ce1c8-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="ce1c8-105">Lorsque vous essayez de déboguer une application Web, il n’y a pas de substitut pour un bon ensemble de journaux de suivi.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="ce1c8-106">Ce didacticiel montre comment activer le suivi dans API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="ce1c8-107">Vous pouvez utiliser cette fonctionnalité pour suivre ce que fait l’infrastructure de l’API Web avant et après l’appel de votre contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="ce1c8-108">Vous pouvez également l’utiliser pour effectuer le suivi de votre propre code.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-108">You can also use it to trace your own code.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ce1c8-109">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="ce1c8-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="ce1c8-110">[Visual studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (fonctionne également avec visual studio 2015)</span><span class="sxs-lookup"><span data-stu-id="ce1c8-110">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="ce1c8-111">API Web 2</span><span class="sxs-lookup"><span data-stu-id="ce1c8-111">Web API 2</span></span>
> - [<span data-ttu-id="ce1c8-112">Microsoft. AspNet. WebApi. Tracing</span><span class="sxs-lookup"><span data-stu-id="ce1c8-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)

## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="ce1c8-113">Activer le suivi System. Diagnostics dans l’API Web</span><span class="sxs-lookup"><span data-stu-id="ce1c8-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="ce1c8-114">Tout d’abord, nous allons créer un projet d’application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="ce1c8-115">Dans Visual Studio, dans le menu **fichier** , sélectionnez **nouveau** > **projet**.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-115">In Visual Studio, from the **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="ce1c8-116">Sous **modèles**, **Web**, sélectionnez **application Web ASP.net**.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="ce1c8-117">Choisissez le modèle de projet d’API Web.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="ce1c8-118">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis **console de gestion de package**.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-118">From the **Tools** menu, select **NuGet Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="ce1c8-119">Dans la fenêtre console du gestionnaire de package, tapez les commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="ce1c8-120">La première commande installe le dernier package de suivi d’API Web.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="ce1c8-121">Il met également à jour les packages d’API Web principaux.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="ce1c8-122">La deuxième commande met à jour le package WebApi. WebHost vers la dernière version.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="ce1c8-123">Si vous souhaitez cibler une version spécifique de l’API Web, utilisez l’indicateur-version lorsque vous installez le package de suivi.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>

<span data-ttu-id="ce1c8-124">Ouvrez le fichier WebApiConfig.cs dans le dossier de démarrage de l’application\_.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="ce1c8-125">Ajoutez le code suivant à la méthode **Register** .</span><span class="sxs-lookup"><span data-stu-id="ce1c8-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="ce1c8-126">Ce code ajoute la classe [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) au pipeline de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="ce1c8-127">La classe **SystemDiagnosticsTraceWriter** écrit les traces dans [System. Diagnostics. trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span><span class="sxs-lookup"><span data-stu-id="ce1c8-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="ce1c8-128">Pour afficher les traces, exécutez l’application dans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="ce1c8-129">Dans un navigateur, accédez à `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="ce1c8-130">Les instructions de suivi sont écrites dans la fenêtre sortie de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="ce1c8-131">(Dans le menu **affichage** , sélectionnez **sortie**).</span><span class="sxs-lookup"><span data-stu-id="ce1c8-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="ce1c8-132">Étant donné que **SystemDiagnosticsTraceWriter** écrit des traces dans **System. Diagnostics. trace**, vous pouvez inscrire des écouteurs de suivi supplémentaires. par exemple, pour écrire des traces dans un fichier journal.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="ce1c8-133">Pour plus d’informations sur les enregistreurs de trace, consultez la rubrique relative aux [écouteurs de suivi](https://msdn.microsoft.com/library/4y5y10s7.aspx) sur MSDN.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="ce1c8-134">Configuration de SystemDiagnosticsTraceWriter</span><span class="sxs-lookup"><span data-stu-id="ce1c8-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="ce1c8-135">Le code suivant montre comment configurer l’enregistreur de trace.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="ce1c8-136">Vous pouvez contrôler deux paramètres :</span><span class="sxs-lookup"><span data-stu-id="ce1c8-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="ce1c8-137">IsVerbose : si la valeur est false, chaque trace contient des informations minimales.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="ce1c8-138">Si la valeur est true, les traces contiennent davantage d’informations.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-138">If true, traces include more information.</span></span>
- <span data-ttu-id="ce1c8-139">MinimumLevel : définit le niveau de trace minimal.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="ce1c8-140">Les niveaux de suivi, dans l’ordre, sont Debug, info, Warn, Error et fatal.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="ce1c8-141">Ajout de traces à votre application API Web</span><span class="sxs-lookup"><span data-stu-id="ce1c8-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="ce1c8-142">L’ajout d’un enregistreur de trace vous donne un accès immédiat aux traces créées par le pipeline de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="ce1c8-143">Vous pouvez également utiliser le générateur de trace pour tracer votre propre code :</span><span class="sxs-lookup"><span data-stu-id="ce1c8-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="ce1c8-144">Pour accéder au writer de trace, appelez **HttpConfiguration. services. GetTraceWriter**.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="ce1c8-145">À partir d’un contrôleur, cette méthode est accessible via la propriété **ApiController. Configuration** .</span><span class="sxs-lookup"><span data-stu-id="ce1c8-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="ce1c8-146">Pour écrire une trace, vous pouvez appeler directement la méthode **ITraceWriter. trace** , mais la classe [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) définit des méthodes d’extension plus conviviales.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="ce1c8-147">Par exemple, la méthode **info** présentée ci-dessus crée une trace avec les **informations**de niveau de suivi.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="ce1c8-148">Infrastructure de suivi des API Web</span><span class="sxs-lookup"><span data-stu-id="ce1c8-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="ce1c8-149">Cette section décrit comment écrire un enregistreur de trace personnalisé pour l’API Web.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="ce1c8-150">Le package Microsoft. AspNet. WebApi. Tracing repose sur une infrastructure de suivi plus générale dans l’API Web.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="ce1c8-151">Au lieu d’utiliser Microsoft. AspNet. WebApi. Tracing, vous pouvez également brancher une autre bibliothèque de suivi/journalisation, telle que [nlog](http://nlog-project.org/) ou [log4net](http://logging.apache.org/log4net/).</span><span class="sxs-lookup"><span data-stu-id="ce1c8-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/logging library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="ce1c8-152">Pour collecter des traces, implémentez l’interface **ITraceWriter** .</span><span class="sxs-lookup"><span data-stu-id="ce1c8-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="ce1c8-153">Voici un exemple simple :</span><span class="sxs-lookup"><span data-stu-id="ce1c8-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="ce1c8-154">La méthode **ITraceWriter. trace** crée une trace.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="ce1c8-155">L’appelant spécifie une catégorie et un niveau de suivi.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="ce1c8-156">La catégorie peut être n’importe quelle chaîne définie par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-156">The category can be any user-defined string.</span></span> <span data-ttu-id="ce1c8-157">Votre implémentation de **trace** doit effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="ce1c8-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="ce1c8-158">Créez un nouveau **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="ce1c8-159">Initialisez-le avec la requête, la catégorie et le niveau de suivi, comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="ce1c8-160">Ces valeurs sont fournies par l’appelant.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="ce1c8-161">Appelez le délégué *traceAction* .</span><span class="sxs-lookup"><span data-stu-id="ce1c8-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="ce1c8-162">À l’intérieur de ce délégué, l’appelant est censé remplir le reste du **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="ce1c8-163">Écrivez **TraceRecord**en utilisant une technique de journalisation de votre choix.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="ce1c8-164">L’exemple illustré ici appelle simplement **System. Diagnostics. trace**.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="ce1c8-165">Définition de l’enregistreur de trace</span><span class="sxs-lookup"><span data-stu-id="ce1c8-165">Setting the Trace Writer</span></span>

<span data-ttu-id="ce1c8-166">Pour activer le suivi, vous devez configurer l’API Web pour utiliser votre implémentation de **ITraceWriter** .</span><span class="sxs-lookup"><span data-stu-id="ce1c8-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="ce1c8-167">Pour ce faire, utilisez l’objet **HttpConfiguration** , comme indiqué dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ce1c8-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="ce1c8-168">Un seul enregistreur de suivi peut être actif.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-168">Only one trace writer can be active.</span></span> <span data-ttu-id="ce1c8-169">Par défaut, l’API Web définit un &quot;de non-op&quot; un traceur qui ne fait rien.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="ce1c8-170">(Le &quot;non-op&quot; audit existe afin que le code de traçage n’ait pas à vérifier si le writer de trace est **null** avant d’écrire une trace.)</span><span class="sxs-lookup"><span data-stu-id="ce1c8-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="ce1c8-171">Fonctionnement du suivi d’API Web</span><span class="sxs-lookup"><span data-stu-id="ce1c8-171">How Web API Tracing Works</span></span>

<span data-ttu-id="ce1c8-172">Le suivi dans l’API Web utilise un modèle de *façade* : lorsque le suivi est activé, l’API Web encapsule diverses parties du pipeline de requête avec des classes qui effectuent des appels de trace.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-172">Tracing in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="ce1c8-173">Par exemple, lors de la sélection d’un contrôleur, le pipeline utilise l’interface **IHttpControllerSelector** .</span><span class="sxs-lookup"><span data-stu-id="ce1c8-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="ce1c8-174">Lorsque le suivi est activé, le pipeline insère une classe qui implémente **IHttpControllerSelector** mais appelle l’implémentation réelle :</span><span class="sxs-lookup"><span data-stu-id="ce1c8-174">With tracing enabled, the pipeline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![Le suivi de l’API Web utilise le modèle de façade.](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="ce1c8-176">Les avantages de cette conception sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="ce1c8-176">The benefits of this design include:</span></span>

- <span data-ttu-id="ce1c8-177">Si vous n’ajoutez pas d’enregistreur de trace, les composants de suivi ne sont pas instanciés et n’ont pas d’impact sur les performances.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="ce1c8-178">Si vous remplacez des services par défaut tels que **IHttpControllerSelector** par votre propre implémentation personnalisée, le suivi n’est pas affecté, car le suivi est effectué par l’objet de wrapper.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="ce1c8-179">Vous pouvez également remplacer l’ensemble de l’infrastructure de trace de l’API Web par votre propre infrastructure personnalisée, en remplaçant le service **ITraceManager** par défaut :</span><span class="sxs-lookup"><span data-stu-id="ce1c8-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="ce1c8-180">Implémentez **ITraceManager. Initialize** pour initialiser votre système de suivi.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="ce1c8-181">N’oubliez pas que cela remplace l' *intégralité* de l’infrastructure de trace, y compris l’ensemble du code de traçage intégré à l’API Web.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
