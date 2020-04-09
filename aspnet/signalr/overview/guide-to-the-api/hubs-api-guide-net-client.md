---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: ASP.NET SignalR Hubs API Guide - .NET Client (C) Microsoft Docs
author: bradygaster
description: Ce document fournit une introduction à l’utilisation de l’API Hubs pour la version SignalR 2 dans les clients .NET, tels que Windows Store (WinRT), WPF, Silverlight, et contre ...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: d3536f1c15cd7dad7cd660becf0577e5c131f707
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676334"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-c"></a>ASP.NET SignalR Hubs API Guide - .NET Client (C)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ce document fournit une introduction à l’utilisation de l’API Hubs pour la version SignalR 2 dans les clients .NET, tels que Windows Store (WinRT), WPF, Silverlight, et les applications console.
>
> L’API SignalR Hubs vous permet de passer des appels de procédure à distance (CRP) d’un serveur à des clients connectés et des clients au serveur. Dans le code serveur, vous définissez les méthodes qui peuvent être appelées par les clients, et vous appelez les méthodes qui s’exécutent sur le client. Dans le code client, vous définissez les méthodes qui peuvent être appelées à partir du serveur, et vous appelez les méthodes qui s’exécutent sur le serveur. SignalR s’occupe de toute la plomberie client-serveur pour vous.
>
> SignalR offre également une API de niveau inférieur appelée Connexions Persistantes. Pour une introduction à SignalR, Hubs, et persistent Connexions, ou pour un tutoriel qui montre comment construire une application SignalR complète, voir [SignalR - Getting Started](../getting-started/index.md).
>
> ## <a name="software-versions-used-in-this-topic"></a>Versions logicielles utilisées dans ce sujet
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
> - .NET 4.5
> - Version SignalR 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versions précédentes de ce sujet
>
> Pour plus d’informations sur les versions antérieures de SignalR, voir [SignalR Older Versions](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Questions et commentaires
>
> S’il vous plaît laisser des commentaires sur la façon dont vous avez aimé ce tutoriel et ce que nous pourrions améliorer dans les commentaires au bas de la page. Si vous avez des questions qui ne sont pas directement liées au tutoriel, vous pouvez les poster sur le [ASP.NET Forum SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Vue d'ensemble

Ce document contient les sections suivantes :

- [Configuration du client](#clientsetup)
- [Comment établir une connexion](#establishconnection)

    - [Connexions cross-domain des clients Silverlight](#slcrossdomain)
- [Comment configurer la connexion](#configureconnection)

    - [Comment définir le nombre maximum de connexions simultanées dans les clients WPF](#maxconnections)
    - [Comment spécifier les paramètres de chaîne de requête](#querystring)
    - [Comment spécifier le mode de transport](#transport)
    - [Comment spécifier les en-têtes HTTP](#httpheaders)
    - [Comment spécifier les certificats clients](#clientcertificate)
- [Comment créer le proxy Hub](#proxy)
- [Comment définir les méthodes sur le client que le serveur peut appeler](#callclient)

    - [Méthodes sans paramètres](#clientmethodswithoutparms)
    - [Méthodes avec paramètres, spécifiant les types de paramètres](#clientmethodswithparmtypes)
    - [Méthodes avec paramètres, spécifiant des objets dynamiques pour les paramètres](#clientmethodswithdynamparms)
    - [Comment enlever un gestionnaire](#removehandler)
- [Comment appeler les méthodes du serveur du client](#callserver)
- [Comment gérer les événements de connexion à vie](#connectionlifetime)
- [Comment gérer les erreurs](#handleerrors)
- [Comment permettre l’enregistrement côté client](#logging)
- [Échantillons de code d’application WPF, Silverlight et console pour les méthodes client que le serveur peut appeler](#wpfsl)

Pour un exemple de projets clients .NET, voir les ressources suivantes :

- [gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) sur GitHub.com (WinRT, Silverlight, exemples d’applications console).
- [DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) sur GitHub.com (exemple WPF).
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).

Pour obtenir de la documentation sur la façon de programmer le serveur ou les clients JavaScript, consultez les ressources suivantes :

- [Guide d’API SignalR Hubs - Serveur](hubs-api-guide-server.md)
- [Guide d’API SignalR Hubs - Client JavaScript](hubs-api-guide-javascript-client.md)

Les liens vers les sujets de référence API sont à la version .NET 4.5 de l’API. Si vous utilisez .NET 4, voir [la version .NET 4 des sujets API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Configuration cliente

Installez le package [NuGet Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) (et non le package [Microsoft.AspNet.SignalR).](http://nuget.org/packages/microsoft.aspnet.signalr) Ce forfait prend en charge les clients WinRT, Silverlight, WPF, console et Windows Phone, pour les clients .NET 4 et .NET 4.5.

Si la version de SignalR que vous avez sur le client est différente de la version que vous avez sur le serveur, SignalR est souvent capable de s’adapter à la différence. Par exemple, un serveur exécutant la version SignalR 2 prendra en charge les clients qui ont installé 1.1.x ainsi que les clients qui ont la version 2 installée. Si la différence entre la version sur le serveur et la version sur le client est trop grande, ou si le client est plus récent que le serveur, SignalR lance une `InvalidOperationException` exception lorsque le client tente d’établir une connexion. Le message d’erreur est "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`" .

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Comment établir une connexion

Avant de pouvoir établir une connexion, `HubConnection` vous devez créer un objet et créer un proxy. Pour établir la connexion, appelez la `Start` méthode sur l’objet. `HubConnection`

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,5)]

> [!NOTE]
> Pour les clients JavaScript, vous devez enregistrer `Start` au moins un gestionnaire d’événements avant d’appeler la méthode pour établir la connexion. Ce n’est pas nécessaire pour les clients .NET. Pour les clients JavaScript, le code proxy généré crée automatiquement des proxys pour tous les Hubs qui existent sur le serveur, et l’enregistrement d’un gestionnaire est la façon dont vous indiquez quels Hubs votre client a l’intention d’utiliser. Mais pour un client .NET vous créez des proxy Hub manuellement, donc SignalR suppose que vous allez utiliser n’importe quel Hub que vous créez un proxy pour.

Le code de l’échantillon utilise l’URL par défaut "/signaleur" pour se connecter à votre service SignalR. Pour plus d’informations sur la façon de spécifier une URL de base différente, voir [ASP.NET Guide d’API De SignalR Hubs - Serveur - L’URL /signalur](hubs-api-guide-server.md#signalrurl).

La `Start` méthode s’exécute asynchronement. Pour vous assurer que les lignes de code ultérieures ne `await` s’exécutent qu’après la mise en place `.Wait()` de la connexion, utilisez-ASP.NET méthode asynchrone 4.5 ou dans une méthode synchrone. N’utilisez `.Wait()` pas chez un client WinRT.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Connexions cross-domain des clients Silverlight

Pour plus d’informations sur la façon d’activer les connexions inter-domaines des clients Silverlight, voir [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Comment configurer la connexion

Avant d’établir une connexion, vous pouvez spécifier l’une des options suivantes :

- Limite de connexions simultanées.
- Paramètres de chaîne de requête.
- Le mode de transport.
- EN-têtes HTTP.
- Certificats de client.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Comment définir le nombre maximum de connexions simultanées dans les clients WPF

Dans les clients WPF, vous devrez peut-être augmenter le nombre maximum de connexions simultanées à partir de sa valeur par défaut de 2. La valeur recommandée est de 10.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=5)]

Pour plus d’informations, voir [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Comment spécifier les paramètres de chaîne de requête

Si vous souhaitez envoyer des données au serveur lorsque le client se connecte, vous pouvez ajouter des paramètres de chaîne de requête à l’objet de connexion. L’exemple suivant montre comment définir un paramètre de chaîne de requête dans le code client.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

L’exemple suivant montre comment lire un paramètre de chaîne de requête dans le code serveur.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Comment spécifier le mode de transport

Dans le cadre du processus de connexion, un client SignalR négocie normalement avec le serveur pour déterminer le meilleur transport qui est pris en charge par le serveur et le client. Si vous savez déjà quel transport vous souhaitez utiliser, vous pouvez contourner ce processus de négociation. Pour spécifier le mode de transport, passez dans un objet de transport à la méthode Démarrer. L’exemple suivant montre comment spécifier la méthode de transport dans le code client.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=5)]

L’espace de nom [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) comprend les classes suivantes que vous pouvez utiliser pour spécifier le transport.

- [LongPollingTransport (LongPollingTransport)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServeursSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponible uniquement lorsque le serveur et le client utilisent .NET 4.5.)
- [AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (choisit automatiquement le meilleur transport qui est pris en charge par le client et le serveur. C’est le transport par défaut. Transmettre cela à `Start` la méthode a le même effet que de ne pas passer dans quoi que ce soit.)

Le transport ForeverFrame n’est pas inclus dans cette liste car il n’est utilisé que par les navigateurs.

Pour plus d’informations sur la façon de vérifier la méthode de transport dans le code serveur, voir [ASP.NET SignalR Hubs API Guide - Server - Comment obtenir des informations sur le client à partir de la propriété Context](hubs-api-guide-server.md#contextproperty). Pour plus d’informations sur les transports et les repli, voir [Introduction à SignalR - Transports et Fallbacks](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Comment spécifier les en-têtes HTTP

Pour définir les en-têtes HTTP, utilisez la `Headers` propriété sur l’objet de connexion. L’exemple suivant montre comment ajouter un en-tête HTTP.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Comment spécifier les certificats clients

Pour ajouter des certificats `AddClientCertificate` clients, utilisez la méthode sur l’objet de connexion.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Comment créer le proxy Hub

Afin de définir les méthodes sur le client qu’un Hub peut appeler à partir du serveur, et `CreateHubProxy` d’invoquer des méthodes sur un Hub sur le serveur, créez un proxy pour le Hub en faisant appel à l’objet de connexion. La chaîne à `CreateHubProxy` laquelle vous passez est le nom de votre `HubName` classe Hub, ou le nom spécifié par l’attribut si l’on a été utilisé sur le serveur. La correspondance de noms ne respecte pas la casse.

**Classe Hub sur le serveur**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Créer un proxy client pour la classe Hub**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=3)]

Si vous décorez votre `HubName` classe Hub avec un attribut, utilisez ce nom.

**Classe Hub sur le serveur**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

**Créer un proxy client pour la classe Hub**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=3)]

Si vous `HubConnection.CreateHubProxy` appelez plusieurs `hubName`fois avec le même, vous obtenez le même `IHubProxy` objet mis en cache.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Comment définir les méthodes sur le client que le serveur peut appeler

Pour définir une méthode que le serveur peut `On` appeler, utilisez la méthode du proxy pour enregistrer un gestionnaire d’événements.

L’appariement du nom de la méthode est insensible aux cas. Par `Clients.All.UpdateStockPrice` exemple, sur le `updateStockPrice` `updatestockprice`serveur `UpdateStockPrice` s’exécute, , ou sur le client.

Différentes plates-formes client ont des exigences différentes pour la façon dont vous écrivez le code de méthode pour mettre à jour l’interface utilisateur. Les exemples présentés sont pour les clients WinRT (Windows Store .NET). WPF, Silverlight, et les exemples d’application de console sont fournis dans [une section distincte plus tard dans ce sujet](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Méthodes sans paramètres

Si la méthode que vous manipulez n’a pas de `On` paramètres, utilisez la surcharge non générique de la méthode :

**Code serveur appelant la méthode client sans paramètres**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**WinRT Code client pour la méthode appelée à partir du serveur sans paramètres[(voir les exemples WPF et Silverlight plus tard dans ce sujet](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Méthodes avec paramètres, spécifiant les types de paramètres

Si la méthode que vous manipulez a des paramètres, spécifiez les types de paramètres comme les types génériques de la `On` méthode. Il existe des surcharges `On` génériques de la méthode pour vous permettre de spécifier jusqu’à 8 paramètres (4 sur Windows Phone 7). Dans l’exemple suivant, un `UpdateStockPrice` paramètre est envoyé à la méthode.

**Code serveur appelant la méthode client avec un paramètre**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**La classe Stock utilisée pour le paramètre**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

**WinRT Code client pour une méthode appelée à partir d’un serveur avec un paramètre[(voir WPF et Silverlight exemples plus tard dans ce sujet](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Méthodes avec paramètres, spécifiant des objets dynamiques pour les paramètres

Comme alternative à la spécifier `On` les paramètres comme types génériques de la méthode, vous pouvez spécifier les paramètres en tant qu’objets dynamiques :

**Code serveur appelant la méthode client avec un paramètre**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**La classe Stock utilisée pour le paramètre**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

**WinRT Code client pour une méthode appelée à partir du serveur avec un paramètre, en utilisant un objet dynamique pour le paramètre[(voir WPF et Silverlight exemples plus tard dans ce sujet](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Comment enlever un gestionnaire

Pour enlever un gestionnaire, appelez sa `Dispose` méthode.

**Code client pour une méthode appelée à partir du serveur**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Code client pour supprimer le gestionnaire**

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Comment appeler les méthodes du serveur du client

Pour appeler une méthode sur `Invoke` le serveur, utilisez la méthode sur le proxy Hub.

Si la méthode du serveur n’a pas de `Invoke` valeur de retour, utilisez la surcharge non générique de la méthode.

**Code serveur pour une méthode qui n’a pas de valeur de retour**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Code client appelant une méthode qui n’a pas de valeur de retour**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Si la méthode du serveur a une valeur de retour, spécifiez le type de retour comme type générique de la `Invoke` méthode.

**Code serveur pour une méthode qui a une valeur de retour et prend un paramètre de type complexe**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**La classe stock utilisée pour le paramètre et la valeur de retour**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

**Code client appelant une méthode qui a une valeur de retour et prend un paramètre de type complexe, dans une méthode ASP.NET 4.5 async**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Code client appelant une méthode qui a une valeur de retour et prend un paramètre de type complexe, dans une méthode synchrone**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

La `Invoke` méthode s’exécute asynchronement et renvoie un `Task` objet. Si vous ne `await` spécifiez pas ou `.Wait()`, la prochaine ligne de code s’exécutera avant que la méthode que vous invoquez a fini d’exécuter.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Comment gérer les événements de connexion à vie

SignalR fournit les événements de connexion suivants à vie que vous pouvez gérer :

- `Received`: Augmenté lorsque des données sont reçues sur la connexion. Fournit les données reçues.
- `ConnectionSlow`: Élevé lorsque le client détecte une connexion lente ou fréquemment en baisse.
- `Reconnecting`: Augmenté lorsque le transport sous-jacent commence à se reconnecter.
- `Reconnected`: Augmenté lorsque le transport sous-jacent s’est reconnecté.
- `StateChanged`: Augmenté lorsque l’état de connexion change. Fournit l’ancien état et le nouvel État. Pour plus d’informations sur les valeurs de l’État de connexion voir [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: Augmenté lorsque la connexion s’est déconnectée.

Par exemple, si vous souhaitez afficher des messages d’avertissement pour des erreurs qui ne sont pas `ConnectionSlow` mortelles mais qui causent des problèmes de connexion intermittents, comme la lenteur ou la baisse fréquente de la connexion, gérez l’événement.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

Pour plus d’informations, voir [Comprendre et gérer les événements à vie de connexion dans SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Comment gérer les erreurs

Si vous n’activez pas explicitement les messages d’erreur détaillés sur le serveur, l’objet d’exception que SignalR renvoie après une erreur contient un minimum d’informations sur l’erreur. Par exemple, si `newContosoChatMessage` un appel échoue, le message`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`d’erreur de l’objet d’erreur contient « L’envoi de messages d’erreur détaillés aux clients en production n’est pas recommandé pour des raisons de sécurité, mais si vous souhaitez activer des messages d’erreur détaillés à des fins de dépannage, utilisez le code suivant sur le serveur.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Pour gérer les erreurs que SignalR soulève, `Error` vous pouvez ajouter un gestionnaire pour l’événement sur l’objet de connexion.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

Pour gérer les erreurs des invocations de la méthode, enveloppez le code dans un bloc d’essai-catch.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Comment permettre l’enregistrement côté client

Pour activer l’enregistrement côté `TraceLevel` `TraceWriter` client, définissez les propriétés et les propriétés sur l’objet de connexion.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=3-4)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>Échantillons de code d’application WPF, Silverlight et console pour les méthodes client que le serveur peut appeler

Les échantillons de code montrés plus tôt pour définir les méthodes client que le serveur peut appeler s’appliquent aux clients WinRT. Les échantillons suivants affichent le code équivalent pour les clients de l’application WPF, Silverlight et console.

### <a name="methods-without-parameters"></a>Méthodes sans paramètres

**Code client WPF pour la méthode appelée à partir du serveur sans paramètres**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Code client Silverlight pour la méthode appelée à partir du serveur sans paramètres**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Console application code client pour la méthode appelée à partir du serveur sans paramètres**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Méthodes avec paramètres, spécifiant les types de paramètres

**Code client WPF pour une méthode appelée à partir du serveur avec un paramètre**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Code client Silverlight pour une méthode appelée à partir du serveur avec un paramètre**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Console application code client pour une méthode appelée à partir du serveur avec un paramètre**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Méthodes avec paramètres, spécifiant des objets dynamiques pour les paramètres

**Code client WPF pour une méthode appelée à partir d’un serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Code client Silverlight pour une méthode appelée à partir du serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Console d’application code client pour une méthode appelée à partir du serveur avec un paramètre, en utilisant un objet dynamique pour le paramètre**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
