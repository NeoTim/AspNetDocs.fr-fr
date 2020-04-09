---
uid: signalr/overview/guide-to-the-api/handling-connection-lifetime-events
title: Comprendre et gérer les événements à vie de connexion dans SignalR (fr) Microsoft Docs
author: bradygaster
description: Cet article décrit comment utiliser les événements exposés par l’API Hubs.
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 03960de2-8d95-4444-9169-4426dcc64913
msc.legacyurl: /signalr/overview/guide-to-the-api/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 5bdf20549fccab5d644e35fdf4ce351540c8620d
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676215"
---
# <a name="understanding-and-handling-connection-lifetime-events-in-signalr"></a>Présentation et gestion des événements de durée de vie des connexions dans SignalR

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cet article fournit un aperçu de la connexion SignalR, de la reconnexion et des événements de déconnexion que vous pouvez gérer, ainsi que des délais d’attente et des paramètres keepalives que vous pouvez configurer.
>
> L’article suppose que vous avez déjà une certaine connaissance de SignalR et de connexion événements à vie. Pour une introduction à SignalR, voir [Introduction à SignalR](../getting-started/introduction-to-signalr.md). Pour les listes d’événements à vie de connexion, voir les ressources suivantes :
>
> - [Comment gérer les événements de connexion à vie dans la classe Hub](hubs-api-guide-server.md#connectionlifetime)
> - [Comment gérer les événements de connexion à vie dans les clients JavaScript](hubs-api-guide-javascript-client.md#connectionlifetime)
> - [Comment gérer les événements de connexion vie dans les clients .NET](hubs-api-guide-net-client.md#connectionlifetime)
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

Cet article contient les sections suivantes :

- [Connexion de la terminologie et des scénarios à vie](#terminology)

    - [Connexions SignalR, connexions de transport et connexions physiques](#signalrvstransport)
    - [Scénarios de déconnexion des transports](#transportdisconnect)
    - [Scénarios de déconnexion des clients](#clientdisconnect)
    - [Scénarios de déconnexion du serveur](#serverdisconnect)
- [Temps d’arrêt et paramètres keepalive](#timeoutkeepalive)

    - [ConnexionTimeout](#connectiontimeout)
    - [DisconnectTimeout (en)](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [Comment modifier le délai d’attente et les paramètres keepalive](#changetimeout)
- [Comment informer l’utilisateur des déconnexions](#notifydisconnect)
- [Comment se reconnecter en permanence](#continuousreconnect)
- [Comment déconnecter un client dans le code serveur](#disconnectclientfromserver)
- [Détecter la raison d’une déconnexion](#detectingreasonfordisconnection)

Les liens vers les sujets de référence API sont à la version .NET 4.5 de l’API. Si vous utilisez .NET 4, voir [la version .NET 4 des sujets API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Connexion de la terminologie et des scénarios à vie

Le `OnReconnected` gestionnaire d’événements dans un `OnConnected` hub SignalR peut exécuter directement après mais pas après `OnDisconnected` pour un client donné. La raison pour laquelle vous pouvez avoir une reconnexion sans déconnexion est qu’il existe plusieurs façons dont le mot "connexion" est utilisé dans SignalR.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Connexions SignalR, connexions de transport et connexions physiques

Cet article fera la distinction entre *les connexions SignalR,* *les connexions de transport*et les *connexions physiques*:

- **La connexion SignalR** fait référence à une relation logique entre un client et une URL serveur, maintenue par l’API SignalR et identifiée uniquement par un identifiant de connexion. Les données sur cette relation sont conservées par SignalR et sont utilisées pour établir une connexion de transport. La relation se termine et SignalR dispose des `Stop` données lorsque le client appelle la méthode ou une limite de délai d’attente est atteinte pendant que SignalR tente de rétablir une connexion de transport perdue.
- **La connexion transport** se réfère à une relation logique entre un client et un serveur, maintenu par l’une des quatre API de transport: WebSockets, événements envoyés par serveur, cadre pour toujours, ou de longs sondages. SignalR utilise l’API de transport pour créer une connexion de transport, et l’API de transport dépend de l’existence d’une connexion réseau physique pour créer la connexion de transport. La connexion de transport prend fin lorsque SignalR y met fin ou lorsque l’API de transport détecte que la connexion physique est rompue.
- **La connexion physique** se réfère aux liens de réseau physique -- fils, signaux sans fil, routeurs, etc. -- qui facilitent la communication entre un ordinateur client et un ordinateur serveur. La connexion physique doit être présente afin d’établir une connexion de transport, et une connexion de transport doit être établie afin d’établir une connexion SignalR. Cependant, la rupture de la connexion physique ne met pas toujours fin immédiatement à la connexion de transport ou à la connexion SignalR, comme cela sera expliqué plus tard dans ce sujet.

Dans le diagramme suivant, la connexion SignalR est représentée par la couche aPI Hubs et PersistentConnection, la connexion de transport est représentée par la couche Transports, et la connexion physique est représentée par les lignes entre le serveur et les clients.

![Diagramme d’architecture SignalR](handling-connection-lifetime-events/_static/image1.png)

Lorsque vous `Start` appelez la méthode d’un client SignalR, vous fournissez au code client SignalR toutes les informations dont il a besoin afin d’établir une connexion physique à un serveur. Le code client SignalR utilise ces informations pour faire une demande HTTP et établir une connexion physique qui utilise l’une des quatre méthodes de transport. Si la connexion de transport échoue ou si le serveur échoue, la connexion SignalR ne disparaît pas immédiatement parce que le client a toujours les informations dont il a besoin pour rétablir automatiquement une nouvelle connexion de transport à la même URL SignalR. Dans ce scénario, aucune intervention de l’application utilisateur n’est impliquée, et lorsque le code client SignalR établit une nouvelle connexion de transport, il ne démarre pas une nouvelle connexion SignalR. La continuité de la connexion SignalR se reflète dans le fait que `Start` l’ID de connexion, qui est créé lorsque vous appelez la méthode, ne change pas.

Le `OnReconnected` gestionnaire d’événements sur le Hub s’exécute lorsqu’une connexion de transport est automatiquement rétablie après avoir été perdue. Le `OnDisconnected` gestionnaire d’événements exécute à la fin d’une connexion SignalR. Une connexion SignalR peut se terminer de toutes les façons suivantes :

- Si le client `Stop` appelle la méthode, un message d’arrêt est envoyé au serveur, et le client et le serveur terminent immédiatement la connexion SignalR.
- Une fois la connectivité entre le client et le serveur perdue, le client tente de se reconnecter et le serveur attend que le client se reconnecte. Si les tentatives de reconnexion sont infructueuses et que la période de décalage se termine, le client et le serveur mettent fin à la connexion SignalR. Le client cesse d’essayer de se reconnecter, et le serveur dispose de sa représentation de la connexion SignalR.
- Si le client cesse de fonctionner `Stop` sans avoir la possibilité d’appeler la méthode, le serveur attend que le client se reconnecte, puis met fin à la connexion SignalR après la période de décalage.
- Si le serveur cesse de fonctionner, le client tente de se reconnecter (recréer la connexion de transport), puis met fin à la connexion SignalR après la période de décalage.

Lorsqu’il n’y a pas de problèmes de connexion, et que l’application utilisateur met fin à la connexion SignalR en appelant la `Stop` méthode, la connexion SignalR et la connexion de transport commencent et se terminent à peu près au même moment. Les sections suivantes décrivent plus en détail les autres scénarios.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Scénarios de déconnexion des transports

Les connexions physiques peuvent être lentes ou il peut y avoir des interruptions de connectivité. Selon des facteurs tels que la durée de l’interruption, la connexion de transport peut être supprimée. SignalR tente alors de rétablir la connexion de transport. Parfois, la connexion de transport API détecte l’interruption et laisse tomber la connexion de transport, et SignalR découvre immédiatement que la connexion est perdue. Dans d’autres scénarios, ni l’API de la connexion de transport ni SignalR ne prennent conscience immédiatement que la connectivité a été perdue. Pour tous les transports, sauf les longs sondages, le client SignalR utilise une fonction appelée *keepalive* pour vérifier la perte de connectivité que l’API de transport est incapable de détecter. Pour plus d’informations sur les longs connexions de sondage, consultez [les paramètres Timeout et keepalive](#timeoutkeepalive) plus tard dans ce sujet.

Lorsqu’une connexion est inactive, le serveur envoie périodiquement un paquet keepalive au client. À la date de l’écriture de cet article, la fréquence par défaut est toutes les 10 secondes. En écoutant ces paquets, les clients peuvent dire s’il y a un problème de connexion. Si un paquet de keepalive n’est pas reçu lorsqu’il est prévu, après un court laps de temps, le client suppose qu’il existe des problèmes de connexion tels que la lenteur ou les interruptions. Si le keepalive n’est toujours pas reçu après une plus longue période, le client suppose que la connexion a été supprimée, et il commence à essayer de se reconnecter.

Le diagramme suivant illustre les événements clients et serveur qui sont soulevés dans un scénario typique lorsqu’il y a des problèmes avec la connexion physique qui ne sont pas immédiatement reconnus par l’API de transport. Le diagramme s’applique aux circonstances suivantes :

- Le transport est WebSockets, image pour toujours, ou des événements envoyés par le serveur.
- Il y a différentes périodes d’interruption dans la connexion réseau physique.
- L’API de transport ne prend pas conscience des interruptions, donc SignalR s’appuie sur la fonctionnalité de maintien pour les détecter.

![Déconnexions de transport](handling-connection-lifetime-events/_static/image2.png)

Si le client passe en mode de reconnexion mais ne peut pas établir une connexion de transport dans la limite de délai de débranchement, le serveur met fin à la connexion SignalR. Lorsque cela se produit, le serveur `OnDisconnected` exécute la méthode du Hub et fait la queue d’un message de déconnexion à envoyer au client au cas où le client parvient à se connecter plus tard. Si le client se reconnecte alors, il `Stop` reçoit la commande de déconnexion et appelle la méthode. Dans ce `OnReconnected` scénario, n’est pas exécuté `OnDisconnected` lorsque le client se `Stop`reconnecte, et n’est pas exécuté lorsque le client appelle . Le diagramme suivant illustre ce scénario.

![Perturbations de transport - délai d’attente du serveur](handling-connection-lifetime-events/_static/image3.png)

Les événements à vie de connexion SignalR qui peuvent être soulevés sur le client sont les suivants :

- `ConnectionSlow`événement client.

    Augmenté quand une proportion prédéfinie de la période de temps d’arrêt de keepalive s’est passée depuis que le dernier message ou le ping keepalive a été reçu. La période d’avertissement de délai d’arrêt par défaut est de 2/3 du délai d’attente. Le délai d’attente est de 20 secondes, de sorte que l’avertissement se produit à environ 13 secondes.

    Par défaut, le serveur envoie des pings keepalive toutes les 10 secondes, et le client vérifie les pings keepalive environ toutes les 2 secondes (un tiers de la différence entre la valeur de temps d’arrêt keepalive et la valeur d’avertissement de temps d’arrêt keepalive).

    Si l’API de transport prend connaissance d’une déconnexion, SignalR peut être informé de la déconnexion avant que la période d’avertissement de délai d’attente ne passe. Dans ce cas, l’événement `ConnectionSlow` ne serait pas soulevé, `Reconnecting` et SignalR irait directement à l’événement.
- `Reconnecting`événement client.

    Augmenté lorsque (a) l’API de transport détecte que la connexion est perdue, ou (b) la période de délai d’attente a passé depuis le dernier message ou ping keepalive a été reçu. Le code client SignalR commence à essayer de se reconnecter. Vous pouvez gérer cet événement si vous voulez que votre application prenne des mesures lorsqu’une connexion de transport est perdue. La période de délai d’arrêt par défaut est actuellement de 20 secondes.

    Si votre code client essaie d’appeler une méthode Hub pendant que SignalR est en mode de reconnexion, SignalR va essayer d’envoyer la commande. La plupart du temps, de telles tentatives échoueront, mais dans certaines circonstances, elles pourraient réussir. Pour les événements envoyés par le serveur, pour toujours le cadre, et les longs transports de vote, SignalR utilise deux canaux de communication, l’un que le client utilise pour envoyer des messages et celui qu’il utilise pour recevoir des messages. Le canal utilisé pour recevoir est le canal ouvert en permanence, et c’est celui qui est fermé lorsque la connexion physique est interrompue. Le canal utilisé pour l’envoi reste disponible, donc si la connectivité physique est restaurée, un appel de méthode du client au serveur pourrait être réussi avant que le canal de réception est rétabli. La valeur de retour ne serait pas reçue jusqu’à ce que SignalR rouvre le canal utilisé pour la réception.
- `Reconnected`événement client.

    Augmenté lorsque la connexion de transport est rétablie. Le `OnReconnected` gestionnaire d’événements dans le Hub s’exécute.
- `Closed`événement client`disconnected` (événement à JavaScript).

    Augmenté lorsque la période de délai de déconnexion expire pendant que le code client SignalR tente de se reconnecter après avoir perdu la connexion de transport. Le délai de déconnectage par défaut est de 30 secondes. (Cet événement est également soulevé lorsque `Stop` la connexion se termine parce que la méthode est appelée.)

Interruptions de connexion de transport qui ne sont pas détectées par l’API de transport et ne retardent pas la réception des pings keepalive du serveur pendant plus longtemps que la période d’avertissement de délai d’attente pourrait ne pas provoquer des événements à vie de connexion à augmenter.

Certains environnements réseau ferment délibérément les connexions inactives, et une autre fonction des paquets keepalive est d’aider à prévenir cela en faisant savoir à ces réseaux qu’une connexion SignalR est utilisée. Dans les cas extrêmes, la fréquence par défaut des pings keepalives peut ne pas être suffisante pour empêcher les connexions fermées. Dans ce cas, vous pouvez configurer des pings keepalives à envoyer plus souvent. Pour plus d’informations, consultez [Timeout et keepalive paramètres](#timeoutkeepalive) plus tard dans ce sujet.

> [!NOTE]
>
> **Important**: La séquence des événements décrits ici n’est pas garantie. SignalR fait toutes les tentatives pour augmenter les événements de connexion à vie d’une manière prévisible selon ce régime, mais il existe de nombreuses variations d’événements réseau et de nombreuses façons dont les cadres de communication sous-jacents tels que les API de transport les gérer. Par exemple, `Reconnected` l’événement peut ne pas être `OnConnected` soulevé lorsque le client se reconnecte, ou le gestionnaire sur le serveur peut s’exécuter lorsque la tentative d’établir une connexion est infructueuse. Ce sujet ne décrit que les effets qui seraient normalement produits par certaines circonstances typiques.

<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Scénarios de déconnexion des clients

Dans un client navigateur, le code client SignalR qui maintient une connexion SignalR s’exécute dans le contexte JavaScript d’une page Web. C’est pourquoi la connexion SignalR doit prendre fin lorsque vous naviguez d’une page à l’autre, et c’est pourquoi vous avez plusieurs connexions avec plusieurs connexions ID si vous vous connectez à partir de plusieurs fenêtres ou onglets de navigateur. Lorsque l’utilisateur ferme une fenêtre ou un onglet de navigateur, ou navigue vers une nouvelle page ou actualise la page, `Stop` la connexion SignalR se termine immédiatement parce que le code client SignalR gère cet événement de navigateur pour vous et appelle la méthode. Dans ces scénarios, ou dans n’importe `Stop` quelle plate-forme client lorsque votre application appelle la méthode, le `OnDisconnected` gestionnaire d’événements s’exécute immédiatement sur le serveur et le client soulève l’événement `Closed` (l’événement est nommé `disconnected` dans JavaScript).

Si une application client ou l’ordinateur qu’il est en cours d’exécution sur les accidents ou va dormir (par exemple, lorsque l’utilisateur ferme l’ordinateur portable), le serveur n’est pas informé de ce qui s’est passé. Pour autant que le serveur le sache, la perte du client peut être due à une interruption de connectivité et le client pourrait essayer de se reconnecter. Par conséquent, dans ces scénarios, le serveur attend pour `OnDisconnected` donner au client une chance de se reconnecter, et ne s’exécute pas jusqu’à ce que la période de délai de déconnectement expire (environ 30 secondes par défaut). Le diagramme suivant illustre ce scénario.

![Défaillance de l’ordinateur client](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Scénarios de déconnexion du serveur

Lorsqu’un serveur est hors connexion -- il redémarre, échoue, le domaine de l’application recycle, etc. -- le résultat peut être similaire à une connexion perdue, `ConnectionSlow` ou l’API de transport et SignalR peuvent savoir immédiatement que le serveur est parti, et SignalR peut commencer à essayer de se reconnecter sans soulever l’événement. Si le client passe en mode de reconnexion, et si le serveur récupère ou redémarre ou si un nouveau serveur est mis en ligne avant l’expiration de la période de délai de déconnexion, le client se reconnectera au serveur restauré ou nouveau. Dans ce cas, la connexion SignalR `Reconnected` se poursuit sur le client et l’événement est soulevé. Sur le premier `OnDisconnected` serveur, n’est jamais exécuté, et sur le nouveau serveur, `OnReconnected` est exécuté, mais n’a `OnConnected` jamais été exécuté pour ce client sur ce serveur avant. (L’effet est le même si le client se reconnecte au même serveur après un redémarrage ou un recyclage de domaine d’application, parce que lorsque le serveur redémarre, il n’a aucun souvenir de l’activité de connexion antérieure.) Le diagramme suivant suppose que l’API de transport prend `ConnectionSlow` connaissance immédiatement de la connexion perdue, de sorte que l’événement n’est pas soulevé.

![Défaillance du serveur et reconnexion](handling-connection-lifetime-events/_static/image5.png)

Si un serveur n’est pas disponible dans la période de décalage, la connexion SignalR se termine. Dans ce scénario,`disconnected` l’événement (dans les `Closed` clients `OnDisconnected` JavaScript) est soulevé sur le client, mais n’est jamais appelé sur le serveur. Le diagramme suivant suppose que l’API de transport ne prend pas conscience de la connexion perdue, de sorte qu’il est détecté par La fonctionnalité de maintien signalR et l’événement `ConnectionSlow` est soulevé.

![Défaillance du serveur et délai d’attente](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Temps d’arrêt et paramètres keepalive

Le `ConnectionTimeout`défaut `DisconnectTimeout`, `KeepAlive` , et les valeurs sont appropriées pour la plupart des scénarios, mais peuvent être modifiés si votre environnement a des besoins spéciaux. Par exemple, si votre environnement réseau ferme les connexions qui sont inactives pendant 5 secondes, vous devrez peut-être diminuer la valeur continue.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

Ce paramètre représente le temps de laisser une connexion de transport ouverte et d’attendre une réponse avant de la fermer et d’ouvrir une nouvelle connexion. La valeur par défaut est de 110 secondes.

Ce paramètre ne s’applique que lorsque la fonctionnalité de maintien est désactivée, qui ne s’applique normalement qu’au long transport de vote. Le diagramme suivant illustre l’effet de ce paramètre sur une longue connexion de transport de scrutin.

![Longue connexion de transport de sondage](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout (en)

Ce paramètre représente le temps d’attente après la `Disconnected` perte d’une connexion de transport avant de soulever l’événement. La valeur par défaut est de 30 secondes. Lorsque vous `DisconnectTimeout` `KeepAlive` définissez, est automatiquement réglé à `DisconnectTimeout` 1/3 de la valeur.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

Ce paramètre représente le temps d’attente avant d’envoyer un paquet de keepalive sur une connexion au ralenti. La valeur par défaut est 10 secondes. Cette valeur ne doit pas dépasser 1/3 de la `DisconnectTimeout` valeur.

Si vous voulez `DisconnectTimeout` définir `KeepAlive`les `KeepAlive` `DisconnectTimeout`deux et , ensemble après . Sinon, `KeepAlive` votre paramètre `DisconnectTimeout` sera `KeepAlive` écrasé lorsque vous définissez automatiquement à 1/3 de la valeur de délai d’attente.

Si vous souhaitez désactiver la fonctionnalité de `KeepAlive` keepalive, définissez-vous à nul. La fonctionnalité keepalive est automatiquement désactivée pour le long transport de bureaux de vote.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Comment modifier le délai d’attente et les paramètres keepalive

Pour modifier les valeurs par défaut `Application_Start` de ces paramètres, définissez-les dans votre fichier *Global.asax,* comme indiqué dans l’exemple suivant. Les valeurs indiquées dans le code de l’échantillon sont les mêmes que les valeurs par défaut.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Comment informer l’utilisateur des déconnexions

Dans certaines applications, vous pouvez afficher un message à l’utilisateur en cas de problèmes de connectivité. Vous avez plusieurs options pour savoir comment et quand le faire. Les échantillons de code suivants sont pour un client JavaScript en utilisant le proxy généré.

- Gérez `connectionSlow` l’événement pour afficher un message dès que SignalR est conscient des problèmes de connexion, avant qu’il ne passe en mode de reconnexion.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Gérez `reconnecting` l’événement pour afficher un message lorsque SignalR est au courant d’une déconnexion et passe en mode de reconnexion.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Gérez `disconnected` l’événement pour afficher un message lorsqu’une tentative de reconnexion s’est évanouie. Dans ce scénario, la seule façon de rétablir une connexion avec le serveur `Start` est de redémarrer la connexion SignalR en appelant la méthode, qui créera un nouvel ID de connexion. L’exemple de code suivant utilise un drapeau pour s’assurer que vous n’émetssez la notification qu’après un délai de reconnexion, et non après une fin normale de la connexion SignalR causée par l’appel de la `Stop` méthode.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Comment se reconnecter en permanence

Dans certaines applications, vous pouvez automatiquement rétablir une connexion après qu’elle a été perdue et que la tentative de reconnexion a été chronométrée. Pour ce faire, vous `Start` pouvez `Closed` appeler la`disconnected` méthode de votre gestionnaire d’événements (gestionnaire d’événements sur les clients JavaScript). Vous pouvez attendre un certain temps `Start` avant d’appeler afin d’éviter de le faire trop fréquemment lorsque le serveur ou la connexion physique ne sont pas disponibles. L’échantillon de code suivant s’adresse à un client JavaScript utilisant le proxy généré.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Un problème potentiel à connaître chez les clients mobiles est que les tentatives de reconnexion continue lorsque le serveur ou la connexion physique n’est pas disponible pourrait causer une vidange inutile de la batterie.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Comment déconnecter un client dans le code serveur

SignalR version 2 ne dispose pas d’une API serveur intégré pour déconnecter les clients. Il est [prévu d’ajouter cette fonctionnalité à l’avenir](https://github.com/SignalR/SignalR/issues/2101). Dans la version SignalR actuelle, la façon la plus simple de déconnecter un client du serveur est d’implémenter une méthode de déconnexion sur le client et d’appeler cette méthode à partir du serveur. L’échantillon de code suivant montre une méthode de déconnexion pour un client JavaScript utilisant le proxy généré.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Sécurité - Ni cette méthode de déconnectement des clients, ni l’API intégrée proposée ne traitera du scénario des clients `stopClient` piratés qui exécutent du code malveillant, puisque les clients pourraient se reconnecter ou le code piraté pourrait supprimer la méthode ou changer ce qu’il fait. L’endroit approprié pour mettre en œuvre la protection par déni de service (DOS) état n’est pas dans le cadre ou la couche du serveur, mais plutôt dans l’infrastructure frontale.

<a id="detectingreasonfordisconnection"></a>
## <a name="detecting-the-reason-for-a-disconnection"></a>Détecter la raison d’une déconnexion

SignalR 2.1 ajoute une surcharge `OnDisconnect` à l’événement serveur qui indique si le client s’est délibérément déconnecté plutôt que de le chronométrer. Le `StopCalled` paramètre est vrai si le client a explicitement fermé la connexion. Dans JavaScript, si une erreur de serveur a conduit le client `$.connection.hub.lastError`à se déconnecter, les informations d’erreur seront transmises au client comme .

**Code serveur C `stopCalled` : paramètre**

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample7.cs?highlight=1,3)]

**Code client JavaScript `lastError` : `disconnect` accès à l’événement.**

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample8.js?highlight=2-3)]
