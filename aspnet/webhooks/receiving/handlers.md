---
uid: webhooks/receiving/handlers
title: ASP.NET les gestionnaires de WebHooks (fr) Microsoft Docs
author: rick-anderson
description: Comment traiter les demandes dans ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: ff12dd8df167eca17ecbd9f03a71807d44af2b30
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675217"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="87ffd-103">ASP.NET les gestionnaires de WebHooks</span><span class="sxs-lookup"><span data-stu-id="87ffd-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="87ffd-104">Une fois que les demandes de WebHooks ont été validées par un récepteur WebHook, elle est prête à être traitée par code utilisateur.</span><span class="sxs-lookup"><span data-stu-id="87ffd-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="87ffd-105">C’est là que *les gestionnaires* entrent en question.</span><span class="sxs-lookup"><span data-stu-id="87ffd-105">This is where *handlers* come in.</span></span> <span data-ttu-id="87ffd-106">Les manutentionnaires dérivent de l’interface [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) mais utilisent généralement la classe [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) au lieu de dériver directement de l’interface.</span><span class="sxs-lookup"><span data-stu-id="87ffd-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="87ffd-107">Une demande WebHook peut être traitée par un ou plusieurs gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="87ffd-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="87ffd-108">Les manutentionnaires sont appelés dans l’ordre en fonction de leur propriété *de commande* respective allant du plus bas au plus haut où l’ordre est un simple intégrier (suggéré d’être entre 1 et 100):</span><span class="sxs-lookup"><span data-stu-id="87ffd-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Diagramme de propriété de commande de gestionnaire de WebHook](_static/Handlers.png)

<span data-ttu-id="87ffd-110">Un gestionnaire peut régler la propriété *Response* sur le WebHookHandlerContext qui conduira le traitement à s’arrêter et la réponse à renvoyer comme réponse HTTP au WebHook.</span><span class="sxs-lookup"><span data-stu-id="87ffd-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="87ffd-111">Dans le cas ci-dessus, Handler C ne sera pas appelé parce qu’il a un ordre plus élevé que B et B définit la réponse.</span><span class="sxs-lookup"><span data-stu-id="87ffd-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="87ffd-112">La configuration de la réponse n’est généralement pertinente que pour Les WebHooks où la réponse peut rapporter des informations à l’API d’origine.</span><span class="sxs-lookup"><span data-stu-id="87ffd-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="87ffd-113">C’est par exemple le cas avec Slack WebHooks où la réponse est affichée sur la chaîne d’où vient le WebHook.</span><span class="sxs-lookup"><span data-stu-id="87ffd-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="87ffd-114">Les manutentionnaires peuvent définir la propriété du récepteur s’ils ne veulent recevoir des WebHooks de ce récepteur particulier.</span><span class="sxs-lookup"><span data-stu-id="87ffd-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="87ffd-115">S’ils ne réglent pas le récepteur, ils sont appelés pour chacun d’eux.</span><span class="sxs-lookup"><span data-stu-id="87ffd-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="87ffd-116">Une autre utilisation courante d’une réponse est d’utiliser une réponse *410 Gone* pour indiquer que le WebHook n’est plus actif et qu’aucune autre demande ne doit être présentée.</span><span class="sxs-lookup"><span data-stu-id="87ffd-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="87ffd-117">Par défaut, un gestionnaire sera appelé par tous les récepteurs WebHook.</span><span class="sxs-lookup"><span data-stu-id="87ffd-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="87ffd-118">Toutefois, si la propriété *du récepteur* est réglée au nom d’un gestionnaire, ce gestionnaire ne recevra que les demandes de WebHook de ce récepteur.</span><span class="sxs-lookup"><span data-stu-id="87ffd-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="87ffd-119">Traitement d’un WebHook</span><span class="sxs-lookup"><span data-stu-id="87ffd-119">Processing a WebHook</span></span>

<span data-ttu-id="87ffd-120">Lorsqu’un gestionnaire est appelé, il reçoit un [texte WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) contenant des informations sur la demande WebHook.</span><span class="sxs-lookup"><span data-stu-id="87ffd-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="87ffd-121">Les données, généralement l’organisme de demande HTTP, sont disponibles à partir de la propriété *Data.*</span><span class="sxs-lookup"><span data-stu-id="87ffd-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="87ffd-122">Le type de données est généralement JSON ou données de formulaire HTML, mais il est possible de jeter à un type plus spécifique si désiré.</span><span class="sxs-lookup"><span data-stu-id="87ffd-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="87ffd-123">Par exemple, les WebHook personnalisés générés par ASP.NET WebHooks peuvent être jetés sur le type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) comme suit :</span><span class="sxs-lookup"><span data-stu-id="87ffd-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a><span data-ttu-id="87ffd-124">Traitement en file d’attente</span><span class="sxs-lookup"><span data-stu-id="87ffd-124">Queued Processing</span></span>

<span data-ttu-id="87ffd-125">La plupart des expéditeurs WebHook réinduquent un WebHook si une réponse n’est pas générée en quelques secondes.</span><span class="sxs-lookup"><span data-stu-id="87ffd-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="87ffd-126">Cela signifie que votre gestionnaire doit terminer le traitement dans ce délai afin de ne pas être appelé à nouveau.</span><span class="sxs-lookup"><span data-stu-id="87ffd-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="87ffd-127">Si le traitement prend plus de temps, ou est mieux géré séparément, puis le [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) peut être utilisé pour soumettre la demande WebHook à une file d’attente, par exemple [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="87ffd-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="87ffd-128">Voici les grandes lignes d’une implémentation [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) :</span><span class="sxs-lookup"><span data-stu-id="87ffd-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
