---
uid: signalr/overview/performance/signalr-performance
title: Performance Signalr Microsoft Docs
author: bradygaster
description: Performances de SignalR
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: b8a44f4c924c94cdfff1ce7630539b45fe269bbf
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676089"
---
# <a name="signalr-performance"></a>Performances de SignalR

par [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ce sujet décrit comment concevoir, mesurer et améliorer les performances dans une application SignalR.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versions logicielles utilisées dans ce sujet
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
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

Pour une présentation récente sur la performance SignalR et la mise à l’échelle, voir [L’échelle du Web en temps réel avec ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).

Cette rubrique contient les sections suivantes :

- [Remarques relatives à la conception](#design)
- [Réglage de votre serveur SignalR pour les performances](#tuning)
- [Résolution des problèmes de performances](#troubleshooting)
- [Utilisation de compteurs de performance SignalR](#perfcounters)
- [Utilisation d’autres compteurs de performance](#othercounters)
- [Autres ressources](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Remarques relatives à la conception

Cette section décrit les modèles qui peuvent être mis en œuvre lors de la conception d’une application SignalR, afin de s’assurer que les performances ne sont pas entravées par la génération de trafic réseau inutile.

### <a name="throttling-message-frequency"></a>Fréquence de message de limitation

Même dans une application qui envoie des messages à haute fréquence (comme une application de jeu en temps réel), la plupart des applications n’ont pas besoin d’envoyer plus de quelques messages par seconde. Pour réduire la quantité de trafic que chaque client génère, une boucle de message peut être implémentée que les files d’attente et envoie des messages pas plus fréquemment qu’un taux fixe (c’est-à-dire que jusqu’à un certain nombre de messages seront envoyés chaque seconde, s’il y a des messages dans cet intervalle de temps à envoyer). Pour une application d’échantillon qui limite les messages à un certain taux (à partir du client et du serveur), voir [High-Frequency Realtime avec SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Réduire la taille du message

Vous pouvez réduire la taille d’un message SignalR en réduisant la taille de vos objets sérialisés. Dans le code serveur, si vous envoyez un objet qui contient des propriétés qui n’ont `JsonIgnore` pas besoin d’être transmises, empêcher ces propriétés d’être sérialisées en utilisant l’attribut. Les noms des propriétés sont également stockés dans le message; les noms des propriétés peuvent `JsonProperty` être raccourcis à l’aide de l’attribut. L’échantillon de code suivant montre comment exclure une propriété d’être envoyée au client, et comment raccourcir les noms de propriété :

**.NET code serveur qui démontre l’attribut JsonIgnore pour exclure les données d’être envoyé au client, et l’attribut JsonProperty pour réduire la taille du message**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Afin de conserver la lisibilité/la maintenance du code client, les noms de propriétés abrégées peuvent être rembpés sur des noms adaptés à l’homme après la réception du message. L’échantillon de code suivant démontre un moyen possible de réapprevrer les noms raccourcis `reMap` à des noms plus longs, en définissant un contrat de message (cartographie), et en utilisant la fonction pour appliquer le contrat à la classe de message optimisée :

**Code JavaScript côté client qui remaps raccourci les noms de propriété à des noms lisibles par l’homme**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Les noms peuvent également être raccourcis dans les messages du client au serveur, en utilisant la même méthode.

La réduction de l’empreinte mémoire (c’est-à-dire la quantité de mémoire utilisée pour le message) de l’objet du message peut également améliorer les performances. Par exemple, si la `int` gamme complète d’un n’est pas nécessaire, a `short` ou `byte` peut être utilisé à la place.

Étant donné que les messages sont stockés dans le bus de message dans la mémoire du serveur, la réduction de la taille des messages peut également résoudre les problèmes de mémoire du serveur.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Réglage de votre serveur SignalR pour les performances

Les paramètres de configuration suivants peuvent être utilisés pour régler votre serveur pour de meilleures performances dans une application SignalR. Pour plus d’informations générales sur la façon d’améliorer les performances dans une application ASP.NET, voir [Améliorer ASP.NET performance](https://msdn.microsoft.com/library/ff647787.aspx).

**Paramètres de configuration SignalR**

- **DefaultMessageBufferSize**: Par défaut, SignalR conserve 1000 messages dans la mémoire par hub par connexion. Si de gros messages sont utilisés, cela peut créer des problèmes de mémoire qui peuvent être atténués en réduisant cette valeur. Ce paramètre peut `Application_Start` être défini dans le gestionnaire d’événements `Configuration` dans une application ASP.NET, ou dans la méthode d’une classe de démarrage OWIN dans une application auto-hébergée. L’échantillon suivant montre comment réduire cette valeur afin de réduire l’empreinte mémoire de votre application afin de réduire la quantité de mémoire du serveur utilisée :

    **.NET code serveur en Startup.cs pour la diminution de la taille de mémoire tampon message par défaut**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**Paramètres de configuration IIS**

- **Demandes simultanées maximales par application**: L’augmentation du nombre de demandes d’IIS simultanées augmentera les ressources du serveur disponibles pour les demandes de service. La valeur par défaut est de 5000; pour augmenter ce paramètre, exécutez les commandes suivantes dans une invite de commande élevée :

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- **ApplicationPool QueueLength**: Il s’agit du nombre maximum de demandes que Http.sys files d’attente pour le pool d’applications. Lorsque la file d’attente est complète, les nouvelles demandes reçoivent une réponse 503 « Service Indisponible ». La valeur par défaut est 1000.

    Le raccourcissement de la longueur de file d’attente pour le processus de travail dans le pool d’applications hébergeant votre application permettra de conserver les ressources de mémoire. Pour plus d’informations, voir [Gérer, Tuning et Configurer les pools d’applications](https://technet.microsoft.com/library/cc745955.aspx).

**ASP.NET paramètres de configuration**

Cette section comprend les paramètres de `aspnet.config` configuration qui peuvent être définis dans le fichier. Ce fichier se trouve dans l’un des deux emplacements, selon la plate-forme:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

ASP.NET paramètres qui peuvent améliorer les performances de SignalR comprennent les éléments suivants :

- **Demandes simultanées maximales par processeur**: L’augmentation de ce paramètre peut atténuer les goulots d’étranglement des performances. Pour augmenter ce paramètre, ajoutez `aspnet.config` le paramètre de configuration suivant au fichier :

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Limite de file d’attente**de demande `maxConcurrentRequestsPerCPU` : Lorsque le nombre total de connexions dépasse le paramètre, ASP.NET commencera à étrangler les demandes à l’aide d’une file d’attente. Pour augmenter la taille de la `requestQueueLimit` file d’attente, vous pouvez augmenter le réglage. Pour ce faire, ajoutez le `processModel` réglage de `config/machine.config` configuration `aspnet.config`suivant au nœud (plutôt que ):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Résolution des problèmes de performances

Cette section décrit les moyens de trouver des goulots d’étranglement de performance dans votre application.

### <a name="verifying-that-websocket-is-being-used"></a>Vérifier que WebSocket est utilisé

Bien que SignalR puisse utiliser une variété de transports pour la communication entre le client et le serveur, WebSocket offre un avantage de performance significatif, et doit être utilisé si le client et le serveur le prennent en charge. Pour déterminer si votre client et votre serveur répondent aux exigences de WebSocket, consultez [Transports et Fallbacks](../getting-started/introduction-to-signalr.md#transports). Pour déterminer quel transport est utilisé dans votre application, vous pouvez utiliser les outils de développeur de navigateur, et examiner les journaux pour voir quel transport est utilisé pour la connexion. Pour plus d’informations sur l’utilisation des outils de développement du navigateur dans Internet Explorer et Chrome, voir [Transports et Fallbacks](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>Utilisation de compteurs de performance SignalR

Cette section décrit comment activer et utiliser les compteurs de performance SignalR, que l’on trouve dans l’emballage. `Microsoft.AspNet.SignalR.Utils`

### <a name="installing-signalrexe"></a>Installation signalur.exe

Les compteurs de performance peuvent être ajoutés au serveur à l’aide d’un utilitaire appelé SignalR.exe. Pour installer cet utilitaire, suivez ces étapes :

1. Dans Visual Studio, sélectionnez **Tools** > **NuGet Package Manager** > **Gérer les paquets NuGet pour la solution**
2. Recherchez **signalr.utils**, et sélectionnez Installer.

    ![](signalr-performance/_static/image1.png)
3. Accepter l’accord de licence pour installer le paquet.
4. SignalR.exe sera installé `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`pour .

### <a name="installing-performance-counters-with-signalrexe"></a>Installation de compteurs de performance avec SignalR.exe

Pour installer des compteurs de performance SignalR, exécutez SignalR.exe dans une invite de commande élevée avec le paramètre suivant :

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Pour supprimer les compteurs de performance SignalR, exécutez SignalR.exe dans une invite de commande élevée avec le paramètre suivant :

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Compteurs SignalR Performance

Le paquet utilitaires installe les compteurs de performance suivants. Les compteurs "Total" mesurent le nombre d’événements depuis le dernier pool d’applications ou le redémarrage du serveur.

**Métriques de connexion**

Les mesures suivantes mesurent les événements de la durée de vie des connexions qui se produisent. Pour plus d’informations, voir [Comprendre et gérer les événements à vie de connexion](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Connexions connectées**
- **Connexions reconnectées**
- **Connexions déconnectées**
- **Connexions actuelles**

**Métriques de message**

Les mesures suivantes mesurent le trafic de messages généré par SignalR.

- **Messages de connexion reçus Total**
- **Messages de connexion envoyés Total**
- **Messages de connexion reçus/Sec**
- **Messages de connexion Envoyés/Sec**

**Mesures d’autobus de message**

Les mesures suivantes mesurent le trafic à travers le bus à messages SignalR interne, la file d’attente dans laquelle tous les messages SignalR entrants et sortants sont placés. Un message est **publié** lorsqu’il est envoyé ou diffusé. Un **abonné** dans ce contexte est un abonnement sur le bus message; cela devrait égaler le nombre de clients plus le serveur lui-même. Un **travailleur alloué** est un composant qui envoie des données à des connexions actives; un **travailleur occupé** est un travailleur qui envoie activement un message.

- **Messages d’autobus reçus Total**
- **Messages d’autobus reçus/Sec**
- **Messages d’autobus publiés Total**
- **Messages d’autobus publiés/Sec**
- **Abonnés Message Bus En cours**
- **Abonnés Message Bus Total**
- **Message Bus Abonnés/Sec**
- **Message Bus Allocated Workers**
- **Message Bus Travailleurs occupés**
- **Sujets d’autobus de message actuels**

**Mesures d’erreur**

Les mesures suivantes mesurent les erreurs générées par le trafic de messages SignalR. **Les** erreurs de résolution du Hub se produisent lorsqu’une méthode de moyeu ou de moyeu ne peut pas être résolue. Les erreurs **d’invocation du Hub** sont des exceptions lancées lors de l’invocation d’une méthode de moyeu. **Les** erreurs de transport sont des erreurs de connexion lancées lors d’une demande ou d’une réponse HTTP.

- **Erreurs: Tous les total**
- **Erreurs: All/Sec**
- **Erreurs: Résolution du hub Total**
- **Erreurs: Résolution du Hub/Sec**
- **Erreurs: Hub Invocation Total**
- **Erreurs: Hub Invocation/Sec**
- **Erreurs: Transport Total**
- **Erreurs: Transport/Sec**

<a id="scaleout_metrics"></a>

**Mesures d’échelle**

Les mesures suivantes mesurent le trafic et les erreurs générées par le fournisseur de scaleout. Un **flux** dans ce contexte est une unité d’échelle utilisée par le fournisseur d’échelle; il s’agit d’une table si SQL Server est utilisé, un sujet si Service Bus est utilisé, et un abonnement si Redis est utilisé. Chaque flux assure la lecture et l’écriture des opérations commandées; un seul flux est un goulot d’étranglement à l’échelle potentielle, de sorte que le nombre de flux peut être augmenté pour aider à réduire ce goulot d’étranglement. Si plusieurs flux sont utilisés, SignalR distribuera automatiquement des messages (shard) sur ces flux d’une manière qui garantit que les messages envoyés à partir d’une connexion donnée sont en ordre.

Le paramètre [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) contrôle la longueur de la file d’attente d’envoi d’échelle entretenue par SignalR. Le définir à une valeur supérieure à 0 placera tous les messages dans une file d’attente d’envoi pour être envoyé un à la fois à l’avion de messagerie configuré. Si la taille de la file d’attente dépasse la longueur configurée, les appels ultérieurs à envoyer échoueront immédiatement avec une [analyse d’opération invalideEnlaçation](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) jusqu’à ce que le nombre de messages dans la file d’attente soit à nouveau inférieur au réglage. La file d’attente est désactivée par défaut parce que les backplanes mis en œuvre ont généralement leur propre file d’attente ou de contrôle du débit en place. Dans le cas de SQL Server, la mise en commun des connexions limite effectivement le nombre d’envois en cours à tout moment.

Par défaut, un seul flux est utilisé pour SQL Server et Redis, cinq flux sont utilisés pour Service Bus, et la file d’attente est désactivée, mais ces paramètres peuvent être modifiés grâce à la configuration sur SQL Server et Service Bus:

**.NET Server Code pour configurer le nombre de tables et la longueur de file d’attente pour l’avion de fond SQL Server**

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

**.NET Code serveur pour configurer le nombre de sujets et la longueur de file d’attente pour l’avion de soutien de bus de service**

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

Un flux **tamponné** est celui qui est entré dans un état défectueux; lorsque le flux est dans l’état défectueux, tous les messages envoyés à l’avion de fond échoueront immédiatement jusqu’à ce que le flux ne soit plus défectueux. La **durée de file d’attente d’envoi** est le nombre de messages qui ont été postés mais pas encore envoyés.

- **Messages d’autobus à l’échelle reçus/Sec**
- **Scaleout Streams Total**
- **Streams Scaleout Ouvert**
- **Scaleout Streams Buffering**
- **Erreurs d’échelle Total**
- **Erreurs d’échelle/Sec**
- **Scaleout Envoyer la longueur de file d’attente**

Pour plus d’informations sur ce que ces compteurs mesurent, voir [SignalR Scaleout avec Azure Service Bus](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Utilisation d’autres compteurs de performance

Les compteurs de performances suivants peuvent également être utiles pour surveiller les performances de votre application.

**Mémoire**

- .NET CLR\\Memory ' octets dans tous les tas (pour w3wp)

**ASP.NET**

- ASP.NET-Demandes actuelles
- ASP.NET-File
- ASP.NET-Rejeté

**UC**

- Informations processeur-Processeur Temps

**TCP/IP**

- TCPv6/Connexions établies
- TCPv4/Connexions établies

**Web Service**

- Service Web-Connexions actuelles
- Service Web-Connexions maximales

**Thread**

- .NET CLR Locks\\And Threads - de threads logiques actuels
- .NET CLR Locks\\And Threads - de threads physiques actuels

<a id="otherresources"></a>

## <a name="other-resources"></a>Autres ressources

Pour plus d’informations sur ASP.NET le suivi et l’accord des performances, consultez les sujets suivants :

- [Vue d’ensemble des performances ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [ASP.NET Utilisation du fil sur IIS 7.5, IIS 7.0, et IIS 6.0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;applicationPool&gt; Element (Paramètres Web)](https://msdn.microsoft.com/library/dd560842.aspx)
