---
title: Client JavaScript ASP.NET Core SignalR
author: bradygaster
description: Vue d’ensemble du client JavaScript ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/javascript-client
ms.openlocfilehash: db9a8bbc8f111728f0827e3639e40785149bf79e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57054596"
---
# <a name="aspnet-core-signalr-javascript-client"></a>Client JavaScript ASP.NET Core SignalR

Par [Rachel Appel](http://twitter.com/rachelappel)

La bibliothèque cliente JavaScript ASP.NET Core SignalR permet aux développeurs d’appeler un hub à partir de code côté serveur.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Installer le package du client SignalR

La bibliothèque de client SignalR JavaScript est remise en tant qu’un [npm](https://www.npmjs.com/) package. Si vous utilisez Visual Studio, exécutez `npm install` à partir de la **Console du Gestionnaire de Package** lorsque vous êtes dans le dossier racine. Pour Visual Studio Code, exécutez la commande depuis le **Terminal intégré**.

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

npm installe le contenu du package dans le dossier *node_modules\\@aspnet\signalr\dist\browser*. Créez un dossier nommé *signalr* sous le dossier *wwwroot\\lib*. Copiez le fichier *signalr.js* dans le dossier *wwwroot\lib\signalr*.

## <a name="use-the-signalr-javascript-client"></a>Utiliser le client JavaScript SignalR

Référencez le client JavaScript SignalR dans l'élément `<script>`.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Se connecter à un hub

Le code suivant crée et démarre une connexion. Nom de concentrateur respecte la casse.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>Connexions cross-origin

En règle générale, les navigateurs chargent des connexions à partir du même domaine que la page demandée. Toutefois, il existe des cas où une connexion à un autre domaine est nécessaire.

Pour empêcher la lecture des données sensibles à partir d’un autre site, un site malveillant [cross-origin connexions](xref:security/cors) sont désactivés par défaut. Pour autoriser une demande de cross-origin, activez-la dans le `Startup` classe.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Appeler des méthodes de hub à partir du client

Les clients JavaScript appellent les méthodes publiques sur les hubs via la méthode [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) de la [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection). La méthode `invoke` accepte deux arguments :

* Le nom de la méthode de hub. Dans l’exemple suivant, le nom de méthode sur le hub est `SendMessage`.
* Tous les arguments définis dans la méthode de hub. Dans l’exemple suivant, le nom de l’argument est `message`. L’exemple de code utilise la syntaxe de fonction arrow est prise en charge dans les versions actuelles de tous les principaux navigateurs à l’exception d’Internet Explorer.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>Appeler des méthodes de client à partir de hub

Pour recevoir des messages à partir du hub, définissez une méthode à l’aide de la méthode [on](/javascript/api/%40aspnet/signalr/hubconnection#on) de la `HubConnection`.

* Le nom de la méthode du client JavaScript. Dans l’exemple suivant, le nom de la méthode est `ReceiveMessage`.
* Les arguments que le hub passe à la méthode. Dans l’exemple suivant, la valeur d’argument est `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Le code précédent dans `connection.on` s’exécute lorsque le code côté serveur appelle à l’aide de la [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) (méthode).

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR détermine la méthode de client à appeler en faisant correspondre le nom de méthode et les arguments définis dans `SendAsync` et `connection.on`.

> [!NOTE]
> Comme meilleure pratique, appelez la méthode [start](/javascript/api/%40aspnet/signalr/hubconnection#start) sur le `HubConnection` après `on`. En procédant comme ceci, vos gestionnaires sont enregistrés avant que tous les messages soient reçus.

## <a name="error-handling-and-logging"></a>Journalisation et gestion des erreurs

Chaînez une méthode `catch` à la fin de la méthode `start` pour gérer les erreurs côté client. Utilisez `console.error` pour envoyer les erreurs vers la console du navigateur.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

Configurer le suivi du journal côté client en passant un enregistreur d’événements et le type d’événement pour vous connecter lorsque la connexion est établie. Les messages sont enregistrés avec le niveau de journalisation spécifié et versions ultérieures. Niveaux de consignation disponibles sont les suivantes :

* `signalR.LogLevel.Error` &ndash; Messages d’erreur. Journaux `Error` messages uniquement.
* `signalR.LogLevel.Warning` &ndash; Messages d’avertissement concernant les erreurs potentielles. Journaux `Warning`, et `Error` messages.
* `signalR.LogLevel.Information` &ndash; Messages d’état sans erreurs. Journaux `Information`, `Warning`, et `Error` messages.
* `signalR.LogLevel.Trace` &ndash; Messages de trace. Consigne tout, y compris les données transportées entre le hub et le client.

Utilisez le [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) méthode sur [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) pour configurer le niveau de journalisation. Les messages sont enregistrés dans la console du navigateur.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>Reconnecter les clients

Le client JavaScript pour SignalR ne se reconnecter automatiquement. Vous devez écrire du code qui se reconnectera votre client manuellement. Le code suivant illustre une approche classique de reconnexion :

1. Une fonction (dans ce cas, le `start` (fonction)) est créé pour démarrer la connexion.
1. Appelez le `start` (fonction) dans la connexion `onclose` Gestionnaire d’événements.

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

Une implémentation réelle serait utiliser une temporisation exponentielle ou réessayer un nombre spécifié de fois avant d’abandonner. 

## <a name="additional-resources"></a>Ressources supplémentaires

* [Référence API JavaScript](/javascript/api/?view=signalr-js-latest)
* [Didacticiel de JavaScript](xref:tutorials/signalr)
* [Didacticiel WebPack et TypeScript](xref:tutorials/signalr-typescript-webpack)
* [Hubs](xref:signalr/hubs)
* [Client .NET](xref:signalr/dotnet-client)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
* [Demandes de cross-Origin (CORS)](xref:security/cors)
