---
title: SignalR HubContext
author: bradygaster
description: Découvrez comment utiliser le service ASP.NET Core SignalR HubContext pour envoyer des notifications aux clients à partir en dehors d’un concentrateur.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/01/2018
uid: signalr/hubcontext
ms.openlocfilehash: 73cf2c9d30ed5e409a75827fdab1f22b20427884
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57025146"
---
# <a name="send-messages-from-outside-a-hub"></a>Envoyer des messages depuis en dehors d’un concentrateur

Par [Mikael Mengistu](https://twitter.com/MikaelM_12)

Le hub SignalR est l’abstraction pour envoyer des messages aux clients connectés au serveur SignalR. Il est également possible d’envoyer des messages à partir d’autres emplacements dans votre application en utilisant le service `IHubContext`. Cet article explique comment accéder à un `IHubContext` SignalR pour envoyer des notifications aux clients en dehors d’un hub.

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(comment télécharger)](xref:index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Obtenir une instance de IHubContext

ASP.NET Core SignalR, vous pouvez accéder à une instance de `IHubContext` via l’injection de dépendance. Vous pouvez injecter une instance de `IHubContext` dans un contrôleur, intergiciel (middleware) ou un autre service de l’injection de dépendances. Utilisez l’instance pour envoyer des messages aux clients.

> [!NOTE]
> Cela diffère d’ASP.NET 4.x SignalR qui utilisait GlobalHost pour accéder au `IHubContext`. ASP.NET Core offre une infrastructure d’injection de dépendance qui supprime le besoin de ce singleton global.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Injecter une instance de IHubContext dans un contrôleur

Vous pouvez injecter une instance de `IHubContext` dans un contrôleur en l’ajoutant à votre constructeur :

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Maintenant, avec accès à une instance de `IHubContext`, vous pouvez appeler des méthodes de concentrateur comme si vous étiez dans le hub lui-même.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Obtenir une instance de IHubContext dans l’intergiciel (middleware)

Accéder au `IHubContext` dans le pipeline d’intergiciel (middleware) comme suit :

```csharp
app.Use(async (context, next) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> Lorsque les méthodes du hub sont appelées d’en dehors de la classe `Hub`, il n'y a aucun appelant associé à l’appel. Par conséquent, il n'y a aucun accès aux propriétés `ConnectionId`, `Caller` et `Others`.

### <a name="inject-a-strongly-typed-hubcontext"></a>Injecter un HubContext fortement typé

Pour injecter un HubContext fortement typée, vérifiez votre Hub hérite `Hub<T>`. Elle est injectée à l’aide de la `IHubContext<THub, T>` interface plutôt que `IHubContext<THub>`.

```csharp
public class ChatController : Controller
{
    public IHubContext<ChatHub, IChatClient> _strongChatHubContext { get; }

    public ChatController(IHubContext<ChatHub, IChatClient> chatHubContext)
    {
        _strongChatHubContext = chatHubContext;
    }

    public async Task SendMessage(string message)
    {
        await _strongChatHubContext.Clients.All.ReceiveMessage(message);
    }
}
```

## <a name="related-resources"></a>Ressources connexes

* [Bien démarrer](xref:tutorials/signalr)
* [Hubs](xref:signalr/hubs)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
