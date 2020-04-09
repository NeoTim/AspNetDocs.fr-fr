---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Introduction au SignalR (fr) Microsoft Docs
author: bradygaster
description: Cet article décrit ce qu’est SignalR, et certaines des solutions qu’il a été conçu pour créer.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8dbc31a5c8d59fa55dc5b513c1a51d24d18a685f
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676348"
---
# <a name="introduction-to-signalr"></a>Introduction à SignalR

par [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cet article décrit ce qu’est SignalR, et certaines des solutions qu’il a été conçu pour créer. 
> 
> ## <a name="questions-and-comments"></a>Questions et commentaires
> 
> S’il vous plaît laisser des commentaires sur la façon dont vous avez aimé ce tutoriel et ce que nous pourrions améliorer dans les commentaires au bas de la page. Si vous avez des questions qui ne sont pas directement liées au tutoriel, vous pouvez les poster sur le [ASP.NET Forum SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).

## <a name="what-is-signalr"></a>Qu’est-ce que SignalR?

ASP.NET SignalR est une bibliothèque pour les développeurs ASP.NET qui simplifie le processus d’ajout de fonctionnalités Web en temps réel aux applications. La fonctionnalité Web en temps réel est la possibilité d’avoir le contenu de pression de code serveur aux clients connectés instantanément comme il devient disponible, plutôt que d’avoir le serveur attendre pour un client de demander de nouvelles données.

SignalR peut être utilisé pour ajouter n’importe quelle sorte de fonctionnalité Web « en temps réel » à votre application ASP.NET. Alors que le chat est souvent utilisé comme un exemple, vous pouvez faire beaucoup plus. Chaque fois qu’un utilisateur actualise une page Web pour voir de nouvelles données, ou que la page implémente [un long sondage](http://en.wikipedia.org/wiki/Push_technology#Long_polling) pour récupérer de nouvelles données, c’est un candidat à l’utilisation de SignalR. Les exemples incluent les tableaux de bord et les applications de surveillance, les applications collaboratives (telles que l’édition simultanée de documents), les mises à jour des progrès de l’emploi et les formulaires en temps réel.

SignalR permet également de nouveaux types d’applications Web qui nécessitent des mises à jour à haute fréquence à partir du serveur, par exemple, les jeux en temps réel.

SignalR fournit une API simple pour créer des appels de procédure à distance serveur à client (RPC) qui appellent les fonctions JavaScript dans les navigateurs clients (et d’autres plates-formes client) à partir du code .NET côté serveur. SignalR comprend également l’API pour la gestion des connexions (par exemple, les événements de connexion et de déconnexion) et le regroupement des connexions.

![Invoquer des méthodes avec SignalR](introduction-to-signalr/_static/image1.png)

SignalR traite automatiquement la gestion des connexions et vous permet de diffuser des messages à tous les clients connectés simultanément, comme une salle de conversation. Vous pouvez également envoyer des messages à des clients spécifiques. La connexion entre le client et le serveur est permanente, contrairement à une connexion HTTP classique qui est rétablie pour chaque communication.

SignalR prend en charge la fonctionnalité « push serveur », dans laquelle le code serveur peut appeler au code client dans le navigateur à l’aide d’appels de procédure à distance (RPC), plutôt que le modèle de réponse aux demandes en commun sur le web aujourd’hui.

Les applications SignalR peuvent s’étendre à des milliers de clients à l’aide de fournisseurs intégrés et tiers.

Les fournisseurs intégrés comprennent :
* [Service Bus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3)
* [SQL Server](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
* [Redis](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Redis)

Les fournisseurs tiers comprennent :
* [NCache](https://www.alachisoft.com/ncache/asp-net-core-signalr.html).

SignalR est open-source, accessible via [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>SignalR et WebSocket

SignalR utilise le nouveau transport WebSocket lorsque disponible et retombe vers les transports plus anciens si nécessaire. Bien que vous puissiez certainement écrire votre application en utilisant WebSocket directement, en utilisant SignalR signifie qu’une grande partie des fonctionnalités supplémentaires que vous auriez besoin de mettre en œuvre est déjà fait pour vous. Plus important encore, cela signifie que vous pouvez coder votre application pour profiter de WebSocket sans avoir à vous soucier de créer un chemin de code distinct pour les clients plus âgés. SignalR vous empêche également d’avoir à vous soucier des mises à jour de WebSocket, puisque SignalR est mis à jour pour prendre en charge les changements dans le transport sous-jacent, fournissant à votre application une interface cohérente entre les versions de WebSocket.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Transports et repli

SignalR est une abstraction sur certains des transports qui sont nécessaires pour effectuer un travail en temps réel entre le client et le serveur. Une connexion SignalR commence sous le titre HTTP, puis est promue à une connexion WebSocket si elle est disponible. WebSocket est le transport idéal pour SignalR, car il fait l’utilisation la plus efficace de la mémoire du serveur, a la latence la plus basse, et a les fonctionnalités les plus sous-jacentes (comme la communication en duplex complet entre le client et le serveur), mais il a également les exigences les plus strictes: WebSocket exige que le serveur utilise Windows Server 2012 ou Windows 8, et .NET Framework 4.5. Si ces exigences ne sont pas remplies, SignalR tentera d’utiliser d’autres transports pour établir ses connexions.

### <a name="html-5-transports"></a>HTML 5 transports

Ces transports dépendent de la prise en charge de [HTML 5](http://en.wikipedia.org/wiki/HTML5). Si le navigateur client ne prend pas en charge la norme HTML 5, les transports plus anciens seront utilisés.

- **WebSocket** (si le serveur et le navigateur indiquent qu’ils peuvent prendre en charge Websocket). WebSocket est le seul moyen de transport qui établit une véritable connexion persistante et bidirectionnel entre le client et le serveur. Cependant, WebSocket a également les exigences les plus strictes; il n’est entièrement pris en charge que dans les dernières versions de Microsoft Internet Explorer, Google Chrome et Mozilla Firefox, et n’a qu’une implémentation partielle dans d’autres navigateurs tels que Opera et Safari.
- **Server Sent Events**, également connu sous le nom EventSource (si le navigateur prend en charge Server Sent Events, qui est essentiellement tous les navigateurs sauf Internet Explorer.)

### <a name="comet-transports"></a>Transport de comètes

Les transports suivants sont basés sur le modèle d’application Web [Comet,](http://en.wikipedia.org/wiki/Comet_(programming)) dans lequel un navigateur ou un autre client maintient une demande HTTP de longue date, que le serveur peut utiliser pour pousser des données au client sans que le client en fait expressément la demande.

- **Forever Frame** (pour Internet Explorer uniquement). Forever Frame crée un IFrame caché qui fait une demande à un point de terminaison sur le serveur qui ne se termine pas. Le serveur envoie ensuite continuellement du script au client qui est immédiatement exécuté, fournissant une connexion aller simple en temps réel d’un serveur à l’autre. La connexion du client au serveur utilise une connexion distincte du serveur à la connexion client, et comme une demande HTTP standard, une nouvelle connexion est créée pour chaque élément de données qui doit être envoyé.
- **Ajax long sondage**. Le long sondage ne crée pas une connexion persistante, mais sonde plutôt le serveur avec une demande qui reste ouverte jusqu’à ce que le serveur répond, à quel point la connexion se ferme, et une nouvelle connexion est demandée immédiatement. Cela peut introduire une certaine latence pendant que la connexion se réinitialise.

Pour plus d’informations sur les transports pris en charge dans quelles configurations, voir [Plateformes supportées](supported-platforms.md).

### <a name="transport-selection-process"></a>Processus de sélection des transports

La liste suivante montre les étapes que SignalR utilise pour décider quel transport utiliser.

1. Si le navigateur est Internet Explorer 8 ou plus tôt, Long Polling est utilisé.
2. Si JSONP est configuré `jsonp` (c’est-à-dire que le paramètre est défini au `true` moment où la connexion est commencée), Long Polling est utilisé.
3. Si une connexion cross-domain est effectuée (c’est-à-dire que si le point de terminaison SignalR n’est pas dans le même domaine que la page d’hébergement), puis WebSocket sera utilisé si les critères suivants sont remplis :

   - Le client prend en charge CORS (Cross-Origin Resource Sharing). Pour plus de détails sur les clients qui prennent en charge CORS, voir [CORS à caniuse.com](http://www.caniuse.com/CORS).
   - Le client prend en charge WebSocket
   - Le serveur prend en charge WebSocket

     Si l’un de ces critères n’est pas satisfait, Long Polling sera utilisé. Pour plus d’informations sur les connexions inter-domaines, voir [Comment établir une connexion inter-domaines](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. Si JSONP n’est pas configuré et que la connexion n’est pas transversale, WebSocket sera utilisé si le client et le serveur le prennent en charge.
5. Si le client ou le serveur ne prennent pas en charge WebSocket, Server Sent Events est utilisé s’il est disponible.
6. Si Server Sent Events n’est pas disponible, Forever Frame est tenté.
7. Si Forever Frame échoue, Long Polling est utilisé.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Surveillance des transports

Vous pouvez déterminer quel transport votre application utilise en permettant la connexion sur votre hub, et en ouvrant la fenêtre de la console dans votre navigateur.

Pour activer la connexion pour les événements de votre hub dans un navigateur, ajoutez la commande suivante à votre application client :

`$.connection.hub.logging = true;`

- Dans Internet Explorer, ouvrez les outils du développeur en appuyant sur F12 et cliquez sur l’onglet Console.

    ![Console dans Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- Dans Chrome, ouvrez la console en appuyant sur Ctrl-Shift-J.

    ![Console dans Google Chrome](introduction-to-signalr/_static/image3.png)

Avec la console ouverte et l’enregistrement activé, vous serez en mesure de voir quel transport est utilisé par SignalR.

![Console dans Internet Explorer montrant le transport WebSocket](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Spécifier un transport

La négociation d’un transport prend un certain temps et les ressources client/serveur. Si les capacités du client sont connues, un transport peut être spécifié lorsque la connexion client est commencée. L’extrait de code suivant démontre le démarrage d’une connexion à l’aide du transport Ajax Long Polling, comme il serait utilisé s’il était connu que le client n’a pris en charge aucun autre protocole:

`connection.start({ transport: 'longPolling' });`

Vous pouvez spécifier une commande de repli si vous souhaitez qu’un client essaie des transports spécifiques dans l’ordre. L’extrait de code suivant démontre essayer WebSocket, et à défaut, aller directement à Long Polling.

`connection.start({ transport: ['webSockets','longPolling'] });`

Les constantes de chaîne pour spécifier les transports sont définies comme suit :

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Connexions et hubs

L’API SignalR contient deux modèles de communication entre les clients et les serveurs : les connexions persistantes et les hubs.

Une connexion représente un critère d’évaluation simple pour l’envoi de messages à un seul destinataire, groupé ou diffusé. L’API De connexion persistante (représentée dans le code .NET par la classe PersistentConnection) donne au développeur un accès direct au protocole de communication de bas niveau que SignalR expose. L’utilisation du modèle de communication Connections sera familière aux développeurs qui ont utilisé des API basées sur la connexion telles que Windows Communication Foundation.

Un Hub est un pipeline de plus haut niveau construit sur l’API Connection qui permet à votre client et serveur d’appeler des méthodes les uns sur les autres directement. SignalR gère l’expédition au-delà des limites de la machine comme par magie, permettant aux clients d’appeler des méthodes sur le serveur aussi facilement que les méthodes locales, et vice versa. L’utilisation du modèle de communication Hubs sera familière aux développeurs qui ont utilisé des API d’invocation à distance telles que la remotage .NET. L’utilisation d’un Hub vous permet également de passer des paramètres fortement tapés aux méthodes, permettant la liaison du modèle.

### <a name="architecture-diagram"></a>Diagramme de l'architecture

Le diagramme suivant montre la relation entre Les hubs, les connexions persistantes et les technologies sous-jacentes utilisées pour les transports.

![Diagramme d’architecture SignalR montrant les API, les transports et les clients](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Fonctionnement des hubs

Lorsque le code côté serveur appelle une méthode sur le client, un paquet est envoyé à travers le transport actif qui contient le nom et les paramètres de la méthode à appeler (lorsqu’un objet est envoyé comme paramètre de méthode, il est sérialisé à l’aide de JSON). Le client associe ensuite le nom de la méthode à des méthodes définies dans le code côté client. S’il y a une correspondance, la méthode client sera exécutée à l’aide des données de paramètres déséialisées.

L’appel de méthode peut être surveillé à l’aide d’outils comme [Fiddler.](http://fiddler2.com/) L’image suivante montre un appel de méthode envoyé à partir d’un serveur SignalR à un client de navigateur Web dans la vitre Logs de Fiddler. L’appel de méthode est `MoveShapeHub`envoyé à partir d’un `updateShape`hub appelé , et la méthode invoquée est appelée .

![Vue du journal Fiddler montrant le trafic SignalR](introduction-to-signalr/_static/image6.png)

Dans cet exemple, le nom `H` du hub est identifié avec le paramètre; le nom de la `M` méthode est identifié avec le paramètre, `A` et les données envoyées à la méthode sont identifiées avec le paramètre. L’application qui a généré ce message est créée dans le tutoriel [High-Frequency Realtime.](tutorial-high-frequency-realtime-with-signalr.md)

### <a name="choosing-a-communication-model"></a>Choisir un modèle de communication

La plupart des applications doivent utiliser l’API Hubs. L’API Connexions pourrait être utilisée dans les circonstances suivantes :

- Le format du message réel envoyé doit être spécifié.
- Le développeur préfère travailler avec un modèle de messagerie et d’expédition plutôt qu’un modèle d’invocation à distance.
- Une application existante qui utilise un modèle de messagerie est portée pour utiliser SignalR.
