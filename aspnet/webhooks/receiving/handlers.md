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
# <a name="aspnet-webhooks-handlers"></a>ASP.NET les gestionnaires de WebHooks

Une fois que les demandes de WebHooks ont été validées par un récepteur WebHook, elle est prête à être traitée par code utilisateur. C’est là que *les gestionnaires* entrent en question. Les manutentionnaires dérivent de l’interface [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) mais utilisent généralement la classe [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) au lieu de dériver directement de l’interface.

Une demande WebHook peut être traitée par un ou plusieurs gestionnaires. Les manutentionnaires sont appelés dans l’ordre en fonction de leur propriété *de commande* respective allant du plus bas au plus haut où l’ordre est un simple intégrier (suggéré d’être entre 1 et 100):

![Diagramme de propriété de commande de gestionnaire de WebHook](_static/Handlers.png)

Un gestionnaire peut régler la propriété *Response* sur le WebHookHandlerContext qui conduira le traitement à s’arrêter et la réponse à renvoyer comme réponse HTTP au WebHook. Dans le cas ci-dessus, Handler C ne sera pas appelé parce qu’il a un ordre plus élevé que B et B définit la réponse.

La configuration de la réponse n’est généralement pertinente que pour Les WebHooks où la réponse peut rapporter des informations à l’API d’origine. C’est par exemple le cas avec Slack WebHooks où la réponse est affichée sur la chaîne d’où vient le WebHook. Les manutentionnaires peuvent définir la propriété du récepteur s’ils ne veulent recevoir des WebHooks de ce récepteur particulier. S’ils ne réglent pas le récepteur, ils sont appelés pour chacun d’eux.

Une autre utilisation courante d’une réponse est d’utiliser une réponse *410 Gone* pour indiquer que le WebHook n’est plus actif et qu’aucune autre demande ne doit être présentée.

Par défaut, un gestionnaire sera appelé par tous les récepteurs WebHook. Toutefois, si la propriété *du récepteur* est réglée au nom d’un gestionnaire, ce gestionnaire ne recevra que les demandes de WebHook de ce récepteur.

## <a name="processing-a-webhook"></a>Traitement d’un WebHook

Lorsqu’un gestionnaire est appelé, il reçoit un [texte WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) contenant des informations sur la demande WebHook. Les données, généralement l’organisme de demande HTTP, sont disponibles à partir de la propriété *Data.*

Le type de données est généralement JSON ou données de formulaire HTML, mais il est possible de jeter à un type plus spécifique si désiré. Par exemple, les WebHook personnalisés générés par ASP.NET WebHooks peuvent être jetés sur le type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) comme suit :

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

  ## <a name="queued-processing"></a>Traitement en file d’attente

La plupart des expéditeurs WebHook réinduquent un WebHook si une réponse n’est pas générée en quelques secondes. Cela signifie que votre gestionnaire doit terminer le traitement dans ce délai afin de ne pas être appelé à nouveau.

Si le traitement prend plus de temps, ou est mieux géré séparément, puis le [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) peut être utilisé pour soumettre la demande WebHook à une file d’attente, par exemple [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Voici les grandes lignes d’une implémentation [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) :

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
