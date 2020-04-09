---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: ASP.NET SignalR Hubs API Guide - Server (C) Microsoft Docs
author: bradygaster
description: Ce document fournit une introduction à la programmation du côté serveur de l’API ASP.NET SignalR Hubs pour la version SignalR 2, avec des échantillons de code démontrant...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c681b104b15bfc4a04587c7abf685dcf20def2ca
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676187"
---
# <a name="aspnet-signalr-hubs-api-guide---server-c"></a>ASP.NET SignalR Hubs API Guide - Serveur (C)

par [Patrick Fletcher](https://github.com/pfletcher), Tom [Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ce document fournit une introduction à la programmation du côté serveur de l’API ASP.NET SignalR Hubs pour la version SignalR 2, avec des échantillons de code démontrant des options communes.
> 
> L’API SignalR Hubs vous permet de passer des appels de procédure à distance (CRP) d’un serveur à des clients connectés et des clients au serveur. Dans le code serveur, vous définissez les méthodes qui peuvent être appelées par les clients, et vous appelez les méthodes qui s’exécutent sur le client. Dans le code client, vous définissez les méthodes qui peuvent être appelées à partir du serveur, et vous appelez les méthodes qui s’exécutent sur le serveur. SignalR s’occupe de toute la plomberie client-serveur pour vous.
> 
> SignalR offre également une API de niveau inférieur appelée Connexions Persistantes. Pour une introduction à SignalR, Hubs, et persistent Connexions, voir [Introduction à SignalR 2](../getting-started/introduction-to-signalr.md).
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versions logicielles utilisées dans ce sujet
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Version SignalR 2
>   
> 
> 
> ## <a name="topic-versions"></a>Versions thématiques
> 
> Pour plus d’informations sur les versions antérieures de SignalR, voir [SignalR Older Versions](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Questions et commentaires
> 
> S’il vous plaît laisser des commentaires sur la façon dont vous avez aimé ce tutoriel et ce que nous pourrions améliorer dans les commentaires au bas de la page. Si vous avez des questions qui ne sont pas directement liées au tutoriel, vous pouvez les poster sur le [ASP.NET Forum SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Vue d'ensemble

Ce document contient les sections suivantes :

- [Comment enregistrer SignalR middleware](#route)

    - [L’URL /signaleur](#signalrurl)
    - [Configuration des options SignalR](#options)
- [Comment créer et utiliser les classes Hub](#hubclass)

    - [Durée de vie de l’objet Hub](#transience)
    - [Camel-casing des noms Hub dans les clients JavaScript](#hubnames)
    - [Centres multiples](#multiplehubs)
    - [Hubs fortement typés](#stronglytypedhubs)
- [Comment définir les méthodes de la classe Hub que les clients peuvent appeler](#hubmethods)

    - [Camélion-casage de noms de méthode dans les clients JavaScript](#methodnames)
    - [Quand exécuter asynchroneously](#asyncmethods)
    - [Définir les surcharges](#overloads)
    - [Signaler les progrès réalisés par rapport aux invocations de la méthode du hub](#progress)
- [Comment appeler les méthodes client de la classe Hub](#callfromhub)

    - [Sélection des clients qui recevront le RPC](#selectingclients)
    - [Aucune validation de temps de compilation pour les noms de méthode](#dynamicmethodnames)
    - [Nom de méthode insensible au cas correspondant](#caseinsensitive)
    - [Exécution asynchrone](#asyncclient)
- [Comment gérer l’adhésion du groupe de la classe Hub](#groupsfromhub)

    - [Exécution asynchrone des méthodes Add and Remove](#asyncgroupmethods)
    - [Persistance de l’adhésion au groupe](#grouppersistence)
    - [Groupes d’utilisateurs uniques](#singleusergroups)
- [Comment gérer les événements de connexion à vie dans la classe Hub](#connectionlifetime)

    - [Lorsque OnConnected, OnDisconnected et OnReconnected sont appelés](#onreconnected)
    - [État appelant non peuplé](#nocallerstate)
- [Comment obtenir des informations sur le client à partir de la propriété Context](#contextproperty)
- [Comment passer l’état entre les clients et la classe Hub](#passstate)
- [Comment gérer les erreurs dans la classe Hub](#handleErrors)
- [Comment appeler les méthodes des clients et gérer les groupes de l’extérieur de la classe Hub](#callfromoutsidehub)

    - [Appeler les méthodes clientes](#callingclientsoutsidehub)
    - [Gestion des membres du groupe](#managinggroupsoutsidehub)
- [Comment activer le traçage](#tracing)
- [Comment personnaliser le pipeline Hubs](#hubpipeline)

Pour obtenir de la documentation sur la façon de programmer les clients, consultez les ressources suivantes :

- [Guide d’API SignalR Hubs - Client JavaScript](hubs-api-guide-javascript-client.md)
- [Guide d’API SignalR Hubs - .NET Client](hubs-api-guide-net-client.md)

Les composants du serveur pour SignalR 2 ne sont disponibles que dans .NET 4.5. Les serveurs fonctionnant en cours d’exécution .NET 4.0 doivent utiliser SignalR v1.x.

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a>Comment enregistrer SignalR middleware

Pour définir l’itinéraire que les clients utiliseront `MapSignalR` pour vous connecter à votre Hub, appelez la méthode lorsque l’application démarre. `MapSignalR`est une méthode `OwinExtensions` [d’extension](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) pour la classe. L’exemple suivant montre comment définir l’itinéraire SignalR Hubs à l’aide d’une classe de démarrage OWIN.

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

Si vous ajoutez la fonctionnalité SignalR à une application MVC ASP.NET, assurez-vous que l’itinéraire SignalR est ajouté avant les autres itinéraires. Pour plus d’informations, voir [Tutorial: Getting Started with SignalR 2 et MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>L’URL /signaleur

Par défaut, l’URL d’itinéraire que les clients utiliseront pour se connecter à votre Hub est "/signalur". (Ne confondez pas cette URL avec l’URL "/signaleur/hubs", qui est pour le fichier JavaScript généré automatiquement. Pour plus d’informations sur le proxy généré, voir [SignalR Hubs API Guide - JavaScript Client - Le proxy généré et ce qu’il fait pour vous](hubs-api-guide-javascript-client.md#genproxy).)

Il pourrait y avoir des circonstances extraordinaires qui rendent cette URL de base non utilisable pour SignalR; par exemple, vous avez un dossier dans votre projet nommé *signaleur* et vous ne voulez pas changer le nom. Dans ce cas, vous pouvez modifier l’URL de base, comme indiqué dans les exemples suivants (remplacer "/signaleur" dans le code de l’échantillon avec votre URL désirée).

**Code serveur qui spécifie l’URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

**Code client JavaScript qui spécifie l’URL (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

**Code client JavaScript qui spécifie l’URL (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

**.NET code client qui spécifie l’URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Configurer les options SignalR

Les surcharges `MapSignalR` de la méthode vous permettent de spécifier une URL personnalisée, un résolveur de dépendance personnalisé et les options suivantes :

- Activez les appels inter-domaines à l’aide de CORS ou de JSONP auprès des clients du navigateur.

    Typiquement, si le navigateur `http://contoso.com`charge une page à partir de `http://contoso.com/signalr`, la connexion SignalR est dans le même domaine, à . Si la `http://contoso.com` page de `http://fabrikam.com/signalr`fait une connexion à , c’est une connexion cross-domain. Pour des raisons de sécurité, les connexions inter-domaines sont désactivées par défaut. Pour plus d’informations, voir [ASP.NET Guide API SignalR Hubs - JavaScript Client - Comment établir une connexion cross-domain](hubs-api-guide-javascript-client.md#crossdomain).
- Activez des messages d’erreur détaillés.

    Lorsque des erreurs se produisent, le comportement par défaut de SignalR est d’envoyer aux clients un message de notification sans plus de détails sur ce qui s’est passé. L’envoi d’informations détaillées sur les erreurs aux clients n’est pas recommandé dans la production, car les utilisateurs malveillants peuvent être en mesure d’utiliser les informations lors d’attaques contre votre application. Pour le dépannage, vous pouvez utiliser cette option pour activer temporairement des rapports d’erreurs plus informatifs.
- Désactiver les fichiers proxy JavaScript générés automatiquement.

    Par défaut, un fichier JavaScript avec des procurations pour vos classes Hub est généré en réponse à l’URL "/signalur/hubs". Si vous ne souhaitez pas utiliser les proxys JavaScript, ou si vous souhaitez générer ce fichier manuellement et vous référer à un fichier physique dans vos clients, vous pouvez utiliser cette option pour désactiver la génération proxy. Pour plus d’informations, voir [SignalR Hubs API Guide - JavaScript Client - Comment créer un fichier physique pour le proxy généré SignalR](hubs-api-guide-javascript-client.md#manualproxy).

L’exemple suivant montre comment spécifier l’URL de `MapSignalR` connexion SignalR et ces options dans un appel à la méthode. Pour spécifier une URL personnalisée, remplacez "/signaleur" dans l’exemple avec l’URL que vous souhaitez utiliser.

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Comment créer et utiliser les classes Hub

Pour créer un Hub, créez une classe dérivée de [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). L’exemple suivant montre une classe Hub simple pour une application de chat.

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

Dans cet exemple, un client `NewContosoChatMessage` connecté peut appeler la méthode, et quand il le fait, les données reçues sont diffusées à tous les clients connectés.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Durée de vie de l’objet Hub

Vous n’avez pas instantané la classe Hub ou n’appelez pas ses méthodes à partir de votre propre code sur le serveur ; tout ce qui est fait pour vous par le pipeline SignalR Hubs. SignalR crée une nouvelle instance de votre classe Hub chaque fois qu’il a besoin de gérer une opération Hub telle que lorsqu’un client se connecte, se déconnecte ou fait un appel de méthode au serveur.

Étant donné que les instances de la classe Hub sont transitoires, vous ne pouvez pas les utiliser pour maintenir l’état d’un appel de méthode à l’autre. Chaque fois que le serveur reçoit un appel de méthode d’un client, une nouvelle instance de votre classe Hub traite le message. Pour maintenir l’état par le biais de connexions multiples et d’appels de méthode, utilisez une autre `Hub`méthode telle qu’une base de données, ou une variable statique sur la classe Hub, ou une classe différente qui ne dérive pas de . Si vous persistez les données dans la mémoire, en utilisant une méthode telle qu’une variable statique sur la classe Hub, les données seront perdues lorsque le domaine de l’application recycle.

Si vous souhaitez envoyer des messages aux clients à partir de votre propre code qui s’exécute en dehors de la classe Hub, vous ne pouvez pas le faire en instantanéisant une instance de classe Hub, mais vous pouvez le faire en obtenant une référence à l’objet de contexte SignalR pour votre classe Hub. Pour plus d’informations, voir [Comment appeler les méthodes des clients et gérer les groupes de l’extérieur de la classe Hub](#callfromoutsidehub) plus tard dans ce sujet.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Camel-casing des noms Hub dans les clients JavaScript

Par défaut, les clients JavaScript se réfèrent à Hubs en utilisant une version à dos de chameau du nom de la classe. SignalR effectue automatiquement cette modification de sorte que le code JavaScript peut se conformer aux conventions JavaScript. L’exemple précédent serait appelé `contosoChatHub` dans le code JavaScript.

**Serveur**

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

**JavaScript client à l’aide de proxy généré**

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

Si vous souhaitez spécifier un nom `HubName` différent pour les clients à utiliser, ajoutez l’attribut. Lorsque vous `HubName` utilisez un attribut, il n’y a pas de changement de nom à étui à chameaux sur les clients JavaScript.

**Serveur**

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

**JavaScript client à l’aide de proxy généré**

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Centres multiples

Vous pouvez définir plusieurs classes Hub dans une application. Lorsque vous faites cela, la connexion est partagée, mais les groupes sont séparés:

- Tous les clients utiliseront la même URL pour établir une connexion SignalR avec votre service ("/signaleur" ou votre URL personnalisée si vous en avez spécifié un), et cette connexion est utilisée pour tous les Hubs définis par le service.

    Il n’y a pas de différence de performances pour plusieurs Hubs par rapport à la définition de toutes les fonctionnalités Hub en une seule classe.
- Tous les Hubs obtiennent les mêmes informations de demande HTTP.

    Étant donné que tous les Hubs partagent la même connexion, les seules informations de demande HTTP que le serveur obtient est ce qui vient dans la demande https://version originale qui établit la connexion SignalR. Si vous utilisez la demande de connexion pour transmettre des informations du client au serveur en spécifiant une chaîne de requête, vous ne pouvez pas fournir différentes chaînes de requête à différents Hubs. Tous les Hubs recevront les mêmes informations.
- Le fichier javaScript des mandataires généré contiendra des procurations pour tous les Hubs dans un seul fichier.

    Pour plus d’informations sur les proxys JavaScript, voir [SignalR Hubs API Guide - JavaScript Client - Le proxy généré et ce qu’il fait pour vous](hubs-api-guide-javascript-client.md#genproxy).
- Les groupes sont définis au sein des Hubs.

    Dans SignalR, vous pouvez définir les groupes nommés à diffuser à des sous-ensembles de clients connectés. Les groupes sont maintenus séparément pour chaque Hub. Par exemple, un groupe nommé « Administrateurs » comprendrait un ensemble de clients pour votre `ContosoChatHub` classe, `StockTickerHub` et le même nom de groupe ferait référence à un ensemble différent de clients pour votre classe.

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a>Hubs fortement typés

Pour définir une interface pour vos méthodes de moyeu que votre client peut référencer (et activer Intellisense sur vos méthodes de moyeu), dérivez votre hub (introduit `Hub<T>` dans SignalR 2.1) plutôt que `Hub`:

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Comment définir les méthodes de la classe Hub que les clients peuvent appeler

Pour exposer une méthode sur le Hub que vous souhaitez être callable du client, déclarez une méthode publique, comme le montrent les exemples suivants.

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

Vous pouvez spécifier un type de retour et des paramètres, y compris des types et des tableaux complexes, comme vous le feriez dans n’importe quelle méthode C. Toutes les données que vous recevez dans les paramètres ou le retour à l’appelant sont communiquées entre le client et le serveur en utilisant JSON, et SignalR gère automatiquement la liaison d’objets complexes et de tableaux d’objets.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Camélion-casage de noms de méthode dans les clients JavaScript

Par défaut, les clients JavaScript se réfèrent aux méthodes Hub en utilisant une version à dos de chameau du nom de la méthode. SignalR effectue automatiquement cette modification de sorte que le code JavaScript peut se conformer aux conventions JavaScript.

**Serveur**

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**JavaScript client à l’aide de proxy généré**

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

Si vous souhaitez spécifier un nom `HubMethodName` différent pour les clients à utiliser, ajoutez l’attribut.

**Serveur**

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**JavaScript client à l’aide de proxy généré**

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Quand exécuter asynchroneously

Si la méthode sera longue ou doit faire un travail qui impliquerait l’attente, comme une recherche de base de données ou un `void` appel de service Web, rendre la méthode Hub asynchrone en retournant une [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (en lieu de retour) ou [l’objet&lt;de la tâche T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) (au lieu du type de `T` retour). Lorsque vous `Task` retournez un objet de la `Task` méthode, SignalR attend que le devait se terminer, puis il renvoie le résultat non emballé au client, de sorte qu’il n’y a aucune différence dans la façon dont vous codez l’appel de méthode dans le client.

Rendre une méthode Hub asynchrone évite de bloquer la connexion lorsqu’elle utilise le transport WebSocket. Lorsqu’une méthode Hub s’exécute de manière synchrone et que le transport est WebSocket, les invocations ultérieures de méthodes sur le Hub à partir du même client sont bloquées jusqu’à ce que la méthode Hub se termine.

L’exemple suivant montre la même méthode codée pour s’exécuter de façon synchrone ou asynchrone, suivie par le code client JavaScript qui fonctionne pour appeler l’une ou l’autre version.

**Synchrone**

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

**Asynchrone**

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**JavaScript client à l’aide de proxy généré**

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

Pour plus d’informations sur la façon d’utiliser des méthodes asynchrones dans ASP.NET 4.5, voir [Using Asynchroneous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Définir les surcharges

Si vous souhaitez définir les surcharges d’une méthode, le nombre de paramètres dans chaque surcharge doit être différent. Si vous différenciez une surcharge simplement en spécifiant différents types de paramètres, votre classe Hub se compilera, mais le service SignalR fera une exception au moment de l’exécution lorsque les clients essaient d’appeler l’une des surcharges.

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a>Signaler les progrès réalisés par rapport aux invocations de la méthode du hub

SignalR 2.1 ajoute un soutien pour le [modèle de rapports d’étape](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduit dans .NET 4.5. Pour mettre en œuvre des rapports d’étape, définissez un `IProgress<T>` paramètre pour votre méthode de hub à laquelle votre client peut accéder :

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

Lors de l’écriture d’une méthode serveur de longue durée, il est important d’utiliser un modèle de programmation asynchrone comme Async / Attendez plutôt que de bloquer le fil hub.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Comment appeler les méthodes client de la classe Hub

Pour appeler les méthodes client `Clients` du serveur, utilisez la propriété dans une méthode de votre classe Hub. L’exemple suivant montre `addNewMessageToPage` le code serveur qui appelle tous les clients connectés, et le code client qui définit la méthode dans un client JavaScript.

**Serveur**

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

Invoquer une méthode client est une opération asynchrone et renvoie un `Task`. Utilisation `await`:

* Pour s’assurer que le message est envoyé sans erreur. 
* Pour permettre des erreurs de capture et de manipulation dans un bloc d’essai-catch.

**JavaScript client à l’aide de proxy généré**

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

Vous ne pouvez pas obtenir une valeur de retour d’une méthode client; syntaxe `int x = Clients.All.add(1,1)` telle que ne fonctionne pas.

Vous pouvez spécifier des types et des tableaux complexes pour les paramètres. L’exemple suivant transmet un type complexe au client dans un paramètre de méthode.

**Code serveur qui appelle une méthode client à l’aide d’un objet complexe**

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

**Code serveur qui définit l’objet complexe**

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

**JavaScript client à l’aide de proxy généré**

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Sélection des clients qui recevront le RPC

La propriété Clients renvoie un objet [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) qui offre plusieurs options pour spécifier quels clients recevront le RPC :

- Tous les clients connectés.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- Seulement le client appelant.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- Tous les clients sauf le client appelant.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- Un client spécifique identifié par id de connexion.

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    Cet exemple `addContosoChatMessageToPage` appelle le client appelant et `Clients.Caller`a le même effet que l’utilisation .
- Tous les clients connectés, à l’exception des clients spécifiés, identifiés par id de connexion.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- Tous les clients connectés dans un groupe spécifié.

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- Tous les clients connectés dans un groupe spécifié, à l’exception des clients spécifiés, identifiés par id de connexion.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- Tous les clients connectés dans un groupe spécifié, à l’exception du client appelant.

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- Un utilisateur spécifique, identifié par userId.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    Par défaut, `IPrincipal.Identity.Name`c’est , mais cela peut être changé [en enregistrant une mise en œuvre de IUserIdProvider avec l’hôte mondial](mapping-users-to-connections.md#IUserIdProvider).
- Tous les clients et groupes dans une liste d’ID de connexion.

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- Une liste de groupes.

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- Un utilisateur par son nom.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- Une liste de noms d’utilisateur (introduit dans SignalR 2.1).

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Aucune validation de temps de compilation pour les noms de méthode

Le nom de méthode que vous spécifiez est interprété comme un objet dynamique, ce qui signifie qu’il n’y a pas d’IntelliSense ou de validation de temps de compilation pour elle. L’expression est évaluée au moment de l’exécution. Lorsque l’appel de méthode s’exécute, SignalR envoie le nom de la méthode et les valeurs de paramètre au client, et si le client a une méthode qui correspond au nom, cette méthode est appelée et les valeurs de paramètres sont transmises à elle. Si aucune méthode de correspondance n’est trouvée sur le client, aucune erreur n’est soulevée. Pour plus d’informations sur le format des données que SignalR transmet au client dans les coulisses lorsque vous appelez une méthode client, voir [Introduction à SignalR](../getting-started/introduction-to-signalr.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Nom de méthode insensible au cas correspondant

L’appariement du nom de la méthode est insensible aux cas. Par `Clients.All.addContosoChatMessageToPage` exemple, sur le `AddContosoChatMessageToPage` `addcontosochatmessagetopage`serveur `addContosoChatMessageToPage` s’exécute, , ou sur le client.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Exécution asynchrone

La méthode que vous appelez exécute asynchronement. Tout code qui vient après un appel de méthode à un client s’exécute immédiatement sans attendre que SignalR finisse la transmission de données aux clients, sauf si vous spécifiez que les lignes de code ultérieures doivent attendre l’achèvement de la méthode. L’exemple de code suivant montre comment exécuter deux méthodes clientes de façon séquentielle.

**Utilisation d’attente (.NET 4.5)**

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

Si vous `await` attendez qu’une méthode client se termine avant l’exécution de la prochaine ligne de code, cela ne signifie pas que les clients recevront effectivement le message avant l’exécution de la prochaine ligne de code. « L’achèvement » d’un appel de méthode client signifie seulement que SignalR a fait tout le nécessaire pour envoyer le message. Si vous avez besoin de vérification que les clients ont reçu le message, vous devez programmer ce mécanisme vous-même. Par exemple, vous `MessageReceived` pouvez coder une méthode `addContosoChatMessageToPage` sur le Hub, `MessageReceived` et dans la méthode sur le client, vous pouvez appeler après avoir fait tout le travail que vous devez faire sur le client. Dans `MessageReceived` le Hub, vous pouvez faire tout le travail dépend de la réception réelle des clients et le traitement de l’appel de méthode d’origine.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Comment utiliser une variable de chaîne comme nom de méthode

Si vous souhaitez invoquer une méthode client en utilisant `Clients.All` une `Clients.Others` `Clients.Caller`variable de chaîne `IClientProxy` comme nom de méthode, moulé (ou , , etc) et ensuite appeler [Invoke (méthodeName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Comment gérer l’adhésion du groupe de la classe Hub

Les groupes de SignalR fournissent une méthode de diffusion de messages à des sous-ensembles spécifiés de clients connectés. Un groupe peut avoir n’importe quel nombre de clients, et un client peut être membre d’un certain nombre de groupes.

Pour gérer l’adhésion au groupe, utilisez `Groups` les méthodes [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) fournies par la propriété de la classe Hub. L’exemple suivant `Groups.Add` `Groups.Remove` montre les méthodes et les méthodes utilisées dans les méthodes Hub qui sont appelées par le code client, suivie par le code client JavaScript qui les appelle.

**Serveur**

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

**JavaScript client à l’aide de proxy généré**

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

Vous n’avez pas à créer explicitement des groupes. En effet, un groupe est automatiquement créé la première `Groups.Add`fois que vous spécifiez son nom dans un appel à , et il est supprimé lorsque vous supprimez la dernière connexion de l’adhésion à celui-ci.

Il n’y a pas d’API pour obtenir une liste d’adhésion de groupe ou une liste de groupes. SignalR envoie des messages aux clients et aux groupes en fonction d’un [pub/sous-modèle,](http://en.wikipedia.org/wiki/Publish/subscribe)et le serveur ne tient pas de listes de groupes ou d’adhésions de groupe. Cela permet de maximiser l’évolutivité, parce que chaque fois que vous ajoutez un nœud à une ferme web, tout état que SignalR maintient doit être propagé au nouveau nœud.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Exécution asynchrone des méthodes Add and Remove

Les `Groups.Add` `Groups.Remove` méthodes et les méthodes s’exécutent asynchronement. Si vous souhaitez ajouter un client à un groupe et envoyer immédiatement un message au `Groups.Add` client en utilisant le groupe, vous devez vous assurer que la méthode se termine en premier. L’exemple de code suivant montre comment le faire.

**Ajout d’un client à un groupe, puis messagerie de ce client**

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Persistance de l’adhésion au groupe

SignalR suit les connexions, pas les utilisateurs, donc si vous voulez qu’un utilisateur soit `Groups.Add` dans le même groupe chaque fois que l’utilisateur établit une connexion, vous devez appeler chaque fois que l’utilisateur établit une nouvelle connexion.

Après une perte temporaire de connectivité, signalR peut parfois restaurer la connexion automatiquement. Dans ce cas, SignalR rétablit la même connexion, n’établit pas de nouvelle connexion, et donc l’adhésion du groupe du client est automatiquement restaurée. Cela est possible même lorsque la pause temporaire est le résultat d’un redémarrage ou d’une défaillance du serveur, car l’état de connexion pour chaque client, y compris les adhésions de groupe, est trébuché vers le client. Si un serveur tombe en panne et est remplacé par un nouveau serveur avant la sortie des temps de connexion, un client peut se reconnecter automatiquement au nouveau serveur et ré-inscrire dans les groupes dont il est membre.

Lorsqu’une connexion ne peut pas être restaurée automatiquement après une perte de connectivité, ou lorsque la connexion se déconnecte ou lorsque le client se déconnecte (par exemple, lorsqu’un navigateur navigue vers une nouvelle page), les abonnements du groupe sont perdus. La prochaine fois que l’utilisateur se connecte sera une nouvelle connexion. Pour maintenir les adhésions de groupe lorsque le même utilisateur établit une nouvelle connexion, votre application doit suivre les associations entre les utilisateurs et les groupes, et restaurer les adhésions de groupe chaque fois qu’un utilisateur établit une nouvelle connexion.

Pour plus d’informations sur les connexions et les reconnexions, voir [Comment gérer les événements de connexion à vie dans la classe Hub](#connectionlifetime) plus tard dans ce sujet.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Groupes d’utilisateurs uniques

Les applications qui utilisent SignalR doivent généralement suivre les associations entre les utilisateurs et les connexions afin de savoir quel utilisateur a envoyé un message et quel utilisateur(s) doit recevoir un message. Les groupes sont utilisés dans l’un des deux modèles couramment utilisés pour ce faire.

- Groupes d’utilisateurs uniques.

    Vous pouvez spécifier le nom d’utilisateur comme nom de groupe, et ajouter l’ID de connexion en cours au groupe chaque fois que l’utilisateur se connecte ou se reconnecte. Pour envoyer des messages à l’utilisateur que vous envoyez au groupe. Un inconvénient de cette méthode est que le groupe ne vous fournit pas un moyen de savoir si l’utilisateur est en ligne ou hors ligne.
- Suivre les associations entre les noms d’utilisateur et les identifiants de connexion.

    Vous pouvez stocker une association entre chaque nom d’utilisateur et une ou plusieurs identifiants de connexion dans un dictionnaire ou une base de données, et mettre à jour les données stockées chaque fois que l’utilisateur se connecte ou se déconnecte. Pour envoyer des messages à l’utilisateur, vous spécifiez les identifiants de connexion. Un inconvénient de cette méthode est qu’il faut plus de mémoire.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Comment gérer les événements de connexion à vie dans la classe Hub

Les raisons typiques de la gestion des événements de connexion à vie sont de garder une trace de savoir si un utilisateur est connecté ou non, et de garder une trace de l’association entre les noms d’utilisateur et les identifiants de connexion. Pour exécuter votre propre code lorsque les `OnConnected`clients `OnDisconnected`se `OnReconnected` connectent ou se déconnectent, remplacez les méthodes, et virtuelles de la classe Hub, comme indiqué dans l’exemple suivant.

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Lorsque OnConnected, OnDisconnected et OnReconnected sont appelés

Chaque fois qu’un navigateur navigue vers une nouvelle page, une nouvelle `OnDisconnected` connexion doit `OnConnected` être établie, ce qui signifie que SignalR exécutera la méthode suivie par la méthode. SignalR crée toujours une nouvelle connexion ID lorsqu’une nouvelle connexion est établie.

La `OnReconnected` méthode est appelée lorsqu’il y a eu une rupture temporaire de connectivité dont SignalR peut automatiquement récupérer, par exemple lorsqu’un câble est temporairement déconnecté et reconnecté avant que la connexion ne soit désactivée. La `OnDisconnected` méthode est appelée lorsque le client est déconnecté et SignalR ne peut pas automatiquement se reconnecter, par exemple quand un navigateur navigue vers une nouvelle page. Par conséquent, une séquence possible d’événements pour un client donné est `OnConnected`, `OnReconnected`, `OnDisconnected`; ou `OnConnected` `OnDisconnected`, . Vous ne verrez pas `OnConnected` `OnDisconnected`la `OnReconnected` séquence , , pour une connexion donnée.

La `OnDisconnected` méthode n’est pas appelée dans certains scénarios, comme lorsqu’un serveur tombe en panne ou que le domaine de l’application est recyclé. Lorsqu’un autre serveur est en ligne ou que le domaine d’application `OnReconnected` complète son recyclage, certains clients peuvent être en mesure de se reconnecter et de tirer l’événement.

Pour plus d’informations, voir [Comprendre et gérer les événements à vie de connexion dans SignalR](handling-connection-lifetime-events.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>État appelant non peuplé

Les méthodes de gestionnaire d’événements à vie de connexion sont `state` appelées à partir du serveur, `Caller` ce qui signifie que tout état que vous mettez dans l’objet sur le client ne sera pas peuplé dans la propriété sur le serveur. Pour plus `state` d’informations `Caller` sur l’objet et la propriété, voir [Comment passer l’état entre les clients et la classe Hub](#passstate) plus tard dans ce sujet.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Comment obtenir des informations sur le client à partir de la propriété Context

Pour obtenir des informations sur `Context` le client, utilisez la propriété de la classe Hub. La `Context` propriété renvoie un objet [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) qui donne accès aux informations suivantes :

- ID de connexion du client appelant.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    L’ID de connexion est un GUID qui est attribué par SignalR (vous ne pouvez pas spécifier la valeur de votre propre code). Il y a un ID de connexion pour chaque connexion, et le même IDENTIFIANT de connexion est utilisé par tous les Hubs si vous avez plusieurs Hubs dans votre application.
- DONNÉES d’en-tête HTTP.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    Vous pouvez également obtenir `Context.Headers`des en-têtes HTTP de . La raison de multiples références à `Context.Headers` la même `Context.Request` chose est qui `Context.Headers` a été créé en premier, la propriété a été ajoutée plus tard, et a été conservé pour la compatibilité arrière.
- Requête des données de chaîne.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    Vous pouvez également obtenir des `Context.QueryString`données de chaîne de requête à partir de .

    La chaîne de requête que vous obtenez dans cette propriété est celle qui a été utilisée avec la demande HTTP qui a établi la connexion SignalR. Vous pouvez ajouter des paramètres de chaîne de requête dans le client en configurant la connexion, ce qui est un moyen pratique de transmettre des données sur le client du client au serveur. L’exemple suivant montre une façon d’ajouter une chaîne de requête dans un client JavaScript lorsque vous utilisez le proxy généré.

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    Pour plus d’informations sur le réglage des paramètres de chaîne de requête, consultez les guides API pour les clients [JavaScript](hubs-api-guide-javascript-client.md) et [.NET.](hubs-api-guide-net-client.md)

    Vous pouvez trouver la méthode de transport utilisée pour la connexion dans les données de chaîne de requête, ainsi que d’autres valeurs utilisées en interne par SignalR :

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    La valeur `transportMethod` de sera "webSockets", "serverSentEvents", "foreverFrame" ou "longPolling". Notez que si vous `OnConnected` vérifiez cette valeur dans la méthode de gestionnaire d’événements, dans certains scénarios, vous pourriez d’abord obtenir une valeur de transport qui n’est pas la méthode de transport négociée finale pour la connexion. Dans ce cas, la méthode jettera une exception et sera appelée à nouveau plus tard lorsque le mode de transport final sera établi.
- Cookies.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    Vous pouvez également `Context.RequestCookies`obtenir des cookies de .
- informations utilisateur.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- L’objet HttpContext pour la demande :

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    Utilisez cette méthode `HttpContext.Current` au lieu `HttpContext` d’obtenir l’objet pour la connexion SignalR.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Comment passer l’état entre les clients et la classe Hub

Le proxy client `state` fournit un objet dans lequel vous pouvez stocker les données que vous souhaitez être transmises au serveur à chaque appel de méthode. Sur le serveur, vous pouvez `Clients.Caller` accéder à ces données dans la propriété dans les méthodes Hub qui sont appelés par les clients. La `Clients.Caller` propriété n’est pas peuplée pour `OnConnected` `OnDisconnected`les `OnReconnected`méthodes de gestionnaire d’événements à vie de connexion , , et .

La création ou `state` la mise `Clients.Caller` à jour de données dans l’objet et la propriété fonctionne dans les deux directions. Vous pouvez mettre à jour les valeurs dans le serveur et elles sont transmises au client.

L’exemple suivant montre le code client JavaScript qui stocke l’état pour la transmission au serveur avec chaque appel de méthode.

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

L’exemple suivant affiche le code équivalent dans un client .NET.

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

Dans votre classe Hub, vous pouvez `Clients.Caller` accéder à ces données dans la propriété. L’exemple suivant montre le code qui récupère l’état mentionné dans l’exemple précédent.

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> Ce mécanisme pour l’état persistant n’est pas destiné à `state` `Clients.Caller` de grandes quantités de données, puisque tout ce que vous mettez dans la propriété ou est trébuché avec chaque invocation de méthode. Il est utile pour les petits éléments tels que les noms d’utilisateur ou les compteurs.

Dans VB.NET ou dans un moyeu fortement typé, l’objet d’état de l’appelant ne peut pas être consulté par ; `Clients.Caller` au lieu `Clients.CallerState` de cela, l’utilisation (introduite dans SignalR 2.1) :

**Utilisation de CallerState en C #**

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

**Utilisation de CallerState dans Visual Basic**

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Comment gérer les erreurs dans la classe Hub

Pour gérer les erreurs qui se produisent dans vos méthodes de classe Hub, assurez-vous d’abord `await`d'"observer" toutes les exceptions des opérations async (comme l’invocation des méthodes clientes) en utilisant . Ensuite, utilisez une ou plusieurs des méthodes suivantes :

- Enveloppez votre code de méthode dans des blocs d’essai et connectez l’objet d’exception. Pour des raisons de débogage, vous pouvez envoyer l’exception au client, mais pour des raisons de sécurité l’envoi d’informations détaillées aux clients en production n’est pas recommandé.
- Créez un module de pipeline Hubs qui gère la méthode [OnIncomingError.](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) L’exemple suivant montre un module de pipeline qui enregistre les erreurs, suivi d’un code en Startup.cs qui injecte le module dans le pipeline Hubs.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- Utilisez `HubException` la classe (introduite dans SignalR 2). Cette erreur peut être jetée de n’importe quelle invocation de hub. Le `HubError` constructeur prend un message de chaîne, et un objet pour stocker des données d’erreur supplémentaires. SignalR va auto-sérialiser l’exception et l’envoyer au client, où il sera utilisé pour rejeter ou échouer l’invocation de la méthode de moyeu.

    Les échantillons de code suivants `HubException` montrent comment lancer une invocation au cours d’un Hub, et comment gérer l’exception sur les clients JavaScript et .NET.

    **Code serveur démontrant la classe HubException**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    **Code client JavaScript démontrant la réponse à la création d’un HubException dans un hub**

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    **.NET code client démontrant la réponse à jeter un HubException dans un hub**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

Pour plus d’informations sur les modules de pipeline Hub, voir [comment personnaliser le pipeline Hubs](#hubpipeline) plus tard dans ce sujet.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Comment activer le traçage

Pour activer le traçage côté serveur, ajoutez un élément system.diagnostics à votre fichier Web.config, tel qu’indiqué dans cet exemple :

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

Lorsque vous exécutez l’application dans Visual Studio, vous pouvez afficher les journaux dans la fenêtre **Output.**

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Comment appeler les méthodes des clients et gérer les groupes de l’extérieur de la classe Hub

Pour appeler les méthodes client d’une classe différente de votre classe Hub, obtenez une référence à l’objet de contexte SignalR pour le Hub et utilisez-le pour appeler des méthodes sur le client ou gérer des groupes.

La classe `StockTicker` d’échantillon suivante obtient l’objet de contexte, le stocke dans un cas de la classe, stocke l’instance de classe dans une propriété statique, et utilise le contexte de l’instance de classe singleton pour appeler la méthode sur les `updateStockPrice` clients qui sont connectés à un Hub nommé `StockTickerHub`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

Si vous avez besoin d’utiliser le contexte plusieurs fois dans un objet à longue durée de vie, obtenir la référence une fois et l’enregistrer plutôt que de l’obtenir à nouveau à chaque fois. L’obtention du contexte une fois garantit que SignalR envoie des messages aux clients dans la même séquence dans laquelle vos méthodes Hub font des invocations de la méthode client. Pour un tutoriel qui montre comment utiliser le contexte SignalR pour un hub, voir [Server Broadcast avec ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Appeler les méthodes clientes

Vous pouvez spécifier quels clients recevront le RPC, mais vous avez moins d’options que lorsque vous appelez à partir d’une classe Hub. La raison en est que le contexte n’est pas associé à un appel particulier d’un `Clients.Others`client, de sorte que toutes les méthodes qui nécessitent une connaissance de l’ID de connexion actuelle, tels que , ou `Clients.Caller`, ou `Clients.OthersInGroup`, ne sont pas disponibles. Les options suivantes sont disponibles :

- Tous les clients connectés.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- Un client spécifique identifié par id de connexion.

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- Tous les clients connectés, à l’exception des clients spécifiés, identifiés par id de connexion.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- Tous les clients connectés dans un groupe spécifié.

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- Tous les clients connectés dans un groupe spécifié, à l’exception des clients spécifiés, identifiés par id de connexion.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

Si vous appelez dans votre classe non-Hub à partir de méthodes dans votre classe `Clients.Client`Hub, vous pouvez passer dans l’ID de connexion actuelle et l’utiliser avec `Clients.AllExcept`, , ou `Clients.Group` pour simuler `Clients.Caller`, `Clients.Others`, ou `Clients.OthersInGroup`. Dans l’exemple `MoveShapeHub` suivant, la classe `Broadcaster` transmet l’ID de connexion à la classe afin que la `Broadcaster` classe puisse simuler `Clients.Others`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Gestion des membres du groupe

Pour gérer des groupes, vous avez les mêmes options que vous dans une classe Hub.

- Ajouter un client à un groupe

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- Retirez un client d’un groupe

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Comment personnaliser le pipeline Hubs

SignalR vous permet d’injecter votre propre code dans le pipeline Hub. L’exemple suivant montre un module de pipeline Hub personnalisé qui enregistre chaque appel de méthode entrant reçu du client et l’appel de méthode sortant invoqué sur le client :

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

Le code suivant dans le fichier *Startup.cs* enregistre le module pour fonctionner dans le pipeline Hub :

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

Il existe de nombreuses méthodes différentes que vous pouvez remplacer. Pour une liste complète, voir [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).
