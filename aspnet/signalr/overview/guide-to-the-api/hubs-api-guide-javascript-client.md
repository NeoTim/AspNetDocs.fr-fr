---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: ASP.NET SignalR Hubs API Guide - JavaScript Client (fr) Microsoft Docs
author: bradygaster
description: Ce document fournit une introduction à l’utilisation de l’API Hubs pour la version SignalR 2 dans les clients JavaScript, tels que les navigateurs et Windows Store (WinJS) applicat ...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 8befe133c3627dac1f7d011959c68e2054d345da
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676082"
---
# <a name="aspnet-signalr-hubs-api-guide---javascript-client"></a>ASP.NET SignalR Hubs API Guide - JavaScript Client

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ce document fournit une introduction à l’utilisation de l’API Hubs pour la version SignalR 2 dans les clients JavaScript, tels que les navigateurs et les applications Windows Store (WinJS).
>
> L’API SignalR Hubs vous permet de passer des appels de procédure à distance (CRP) d’un serveur à des clients connectés et des clients au serveur. Dans le code serveur, vous définissez les méthodes qui peuvent être appelées par les clients, et vous appelez les méthodes qui s’exécutent sur le client. Dans le code client, vous définissez les méthodes qui peuvent être appelées à partir du serveur, et vous appelez les méthodes qui s’exécutent sur le serveur. SignalR s’occupe de toute la plomberie client-serveur pour vous.
>
> SignalR offre également une API de niveau inférieur appelée Connexions Persistantes. Pour une introduction à SignalR, Hubs, et persistent Connexions, voir [Introduction à SignalR](../getting-started/introduction-to-signalr.md).
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

- [Le proxy généré et ce qu’il fait pour vous](#genproxy)

    - [Quand utiliser le proxy généré](#cantusegenproxy)
- [Configuration du client](#clientsetup)

    - [Comment faire référence au proxy généré dynamiquement](#dynamicproxy)
    - [Comment créer un fichier physique pour le proxy généré par SignalR](#manualproxy)
- [Comment établir une connexion](#establishconnection)

    - [$.connection.hub est le même objet que $.hubConnection() crée](#connequivalence)
    - [Exécution asynchrone de la méthode de départ](#asyncstart)
- [Comment établir une connexion inter-domaines](#crossdomain)
- [Comment configurer la connexion](#configureconnection)

    - [Comment spécifier les paramètres de chaîne de requête](#querystring)
    - [Comment spécifier le mode de transport](#transport)
- [Comment obtenir un proxy pour une classe Hub](#getproxy)
- [Comment définir les méthodes sur le client que le serveur peut appeler](#callclient)
- [Comment appeler les méthodes du serveur du client](#callserver)
- [Comment gérer les événements de connexion à vie](#connectionlifetime)
- [Comment gérer les erreurs](#handleerrors)
- [Comment permettre l’enregistrement côté client](#logging)

Pour obtenir de la documentation sur la façon de programmer le serveur ou les clients .NET, consultez les ressources suivantes :

- [Guide d’API SignalR Hubs - Serveur](hubs-api-guide-server.md)
- [Guide d’API SignalR Hubs - .NET Client](hubs-api-guide-net-client.md)

Le composant serveur SignalR 2 n’est disponible que sur .NET 4.5 (bien qu’il existe un client .NET pour SignalR 2 sur .NET 4.0).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>Le proxy généré et ce qu’il fait pour vous

Vous pouvez programmer un client JavaScript pour communiquer avec un service SignalR avec ou sans proxy que SignalR génère pour vous. Ce que le proxy fait pour vous est de simplifier la syntaxe du code que vous utilisez pour connecter, écrire des méthodes que le serveur appelle, et les méthodes d’appel sur le serveur.

Lorsque vous écrivez du code pour appeler les méthodes du serveur, le proxy généré vous permet `serverMethod(arg1, arg2)` d’utiliser la syntaxe qui semble exécuter une fonction locale: vous pouvez écrire au lieu de `invoke('serverMethod', arg1, arg2)`. La syntaxe proxy générée permet également une erreur immédiate et intelligible côté client si vous maltypé un nom de méthode serveur. Et si vous créez manuellement le fichier qui définit les procurations, vous pouvez également obtenir le support IntelliSense pour écrire du code qui appelle les méthodes du serveur.

Supposons, par exemple, que vous ayez la classe Hub suivante sur le serveur :

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Les exemples de code suivants montrent à quoi `NewContosoChatMessage` ressemble le code JavaScript pour `addContosoChatMessageToPage` invoquer la méthode sur le serveur et recevoir des invocations de la méthode à partir du serveur.

**Avec le proxy généré**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Sans le proxy généré**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Quand utiliser le proxy généré

Si vous souhaitez enregistrer plusieurs gestionnaires d’événements pour une méthode client que le serveur appelle, vous ne pouvez pas utiliser le proxy généré. Sinon, vous pouvez choisir d’utiliser le proxy généré ou non en fonction de votre préférence de codage. Si vous choisissez de ne pas l’utiliser, vous n’avez pas à `script` référencer l’URL "signaleur/hubs" dans un élément de votre code client.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Configuration cliente

Un client JavaScript nécessite des références à jQuery et au fichier JavaScript de base SignalR. La version jQuery doit être 1.6.4 ou les versions plus importantes ultérieures, telles que 1.7.2, 1.8.2, ou 1.9.1. Si vous décidez d’utiliser le proxy généré, vous avez également besoin d’une référence au fichier JavaScript proxy généré par SignalR. L’exemple suivant montre à quoi pourraient ressembler les références dans une page HTML qui utilise le proxy généré.

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

Ces références doivent être incluses dans cet ordre : jQuery d’abord, noyau SignalR après cela, et les procurations SignalR durent.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Comment faire référence au proxy généré dynamiquement

Dans l’exemple précédent, la référence au proxy généré par SignalR est de générer dynamiquement le code JavaScript, et non vers un fichier physique. SignalR crée le code JavaScript pour le proxy à la volée et le sert au client en réponse à l’URL "/signaleur/hubs". Si vous avez spécifié une URL de base `MapSignalR` différente pour les connexions SignalR sur le serveur dans votre méthode, l’URL du fichier proxy généré dynamiquement est votre URL personnalisée avec des "/hubs" qui lui sont annexés.

> [!NOTE]
> Pour les clients JavaScript de Windows 8 (Windows Store), utilisez le fichier proxy physique au lieu de celui généré dynamiquement. Pour plus d’informations, voir [Comment créer un fichier physique pour le proxy généré par SignalR](#manualproxy) plus tard dans ce sujet.

Dans une ASP.NET vue MVC 4 ou 5 Razor, utilisez le tilde pour désigner la racine d’application dans votre référence de fichier proxy :

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

Pour plus d’informations sur l’utilisation de SignalR dans MVC 5, voir [Getting Started with SignalR et MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

Dans une ASP.NET vue de rasoir MVC `Url.Content` 3, utilisez pour votre référence de fichier proxy :

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

Dans une application ASP.NET Web Forms, utilisez `ResolveClientUrl` votre référence de fichier de procurations ou enregistrez-la via le ScriptManager à l’aide d’un chemin relatif à la racine de l’application (à commencer par un tilde) :

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

En règle générale, utilisez la même méthode pour spécifier l’URL "/signaleur/hubs" que vous utilisez pour les fichiers CSS ou JavaScript. Si vous spécifiez une URL sans utiliser de tilde, dans certains scénarios, votre application fonctionnera correctement lorsque vous testez dans Visual Studio à l’aide d’IIS Express, mais échouera avec une erreur 404 lorsque vous déployez à l’IIS complète. Pour plus d’informations, voir **Résoudre les références aux ressources de niveau racine** dans les serveurs Web dans Visual Studio pour ASP.NET [projets Web](https://msdn.microsoft.com/library/58wxa9w5.aspx) sur le site MSDN.

Lorsque vous exécutez un projet web dans Visual Studio 2017 en mode débogé, et si vous utilisez Internet Explorer comme navigateur, vous pouvez voir le fichier proxy dans **Solution Explorer** sous **Scripts**.

Pour voir le contenu du fichier, double clic **hubs**. Si vous n’utilisez pas Visual Studio 2012 ou 2013 et Internet Explorer, ou si vous n’êtes pas en mode débogé, vous pouvez également obtenir le contenu du fichier en naviguant vers l’URL "/signalR/hubs". Par exemple, si votre `http://localhost:56699`site est `http://localhost:56699/SignalR/hubs` en cours d’exécution à , aller à dans votre navigateur.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Comment créer un fichier physique pour le proxy généré par SignalR

Comme alternative au proxy généré dynamiquement, vous pouvez créer un fichier physique qui a le code proxy et la référence de ce fichier. Vous voudrez peut-être le faire pour le contrôle sur la mise en cache ou le comportement de regroupement, ou pour obtenir IntelliSense lorsque vous codez des appels vers des méthodes de serveur.

Pour créer un fichier proxy, effectuez les étapes suivantes :

1. Installez le package [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet.
2. Ouvrez une invite de commande et naviguez vers le dossier *d’outils* qui contient le fichier SignalR.exe. Le dossier d’outils se trouve à l’endroit suivant :

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. Entrez la commande suivante :

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    Le chemin vers votre *.dll* est généralement le dossier *bin* dans votre dossier de projet.

    Cette commande crée un fichier nommé *server.js* dans le même dossier que *signalr.exe*.
4. Mettez le fichier *server.js* dans un dossier approprié dans votre projet, renommez-le le comme approprié pour votre application, et ajoutez une référence à celui-ci à la place de la référence "signaleur / hubs".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Comment établir une connexion

Avant de pouvoir établir une connexion, vous devez créer un objet de connexion, créer un proxy et enregistrer les gestionnaires d’événements pour les méthodes qui peuvent être appelées à partir du serveur. Lorsque les gestionnaires de procuration et d’événement sont `start` configurés, établissez la connexion en appelant la méthode.

Si vous utilisez le proxy généré, vous n’avez pas à créer l’objet de connexion dans votre propre code parce que le code proxy généré le fait pour vous.

<a id="nogenconnection"></a>

**Établir une connexion (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Établir une connexion (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

Le code de l’échantillon utilise l’URL par défaut "/signaleur" pour se connecter à votre service SignalR. Pour plus d’informations sur la façon de spécifier une URL de base différente, voir [ASP.NET Guide d’API De SignalR Hubs - Serveur - L’URL /signalur](hubs-api-guide-server.md#signalrurl).

Par défaut, l’emplacement du hub est le serveur actuel; si vous vous connectez à un serveur différent, spécifiez l’URL avant d’appeler la `start` méthode, comme indiqué dans l’exemple suivant :

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> Normalement, vous enregistrez les `start` gestionnaires d’événements avant d’appeler la méthode pour établir la connexion. Si vous souhaitez enregistrer certains gestionnaires d’événements après avoir établi la connexion, vous pouvez le faire, `start` mais vous devez vous inscrire au moins un de vos gestionnaires d’événements avant d’appeler la méthode. Une raison pour cela est qu’il peut y avoir de nombreux Hubs dans une application, mais vous ne voudriez pas déclencher l’événement `OnConnected` sur chaque Hub si vous allez seulement utiliser à l’un d’eux. Lorsque la connexion est établie, la présence d’une méthode client sur le `OnConnected` proxy d’un Hub est ce qui indique à SignalR de déclencher l’événement. Si vous n’enregistrez pas les `start` gestionnaires d’événements avant d’appeler la méthode, vous `OnConnected` serez en mesure d’invoquer des méthodes sur le Hub, mais la méthode du Hub ne sera pas appelée et aucune méthode client ne sera invoquée à partir du serveur.

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$.connection.hub est le même objet que $.hubConnection() crée

Comme vous pouvez le voir dans les exemples, lorsque vous utilisez le proxy généré, `$.connection.hub` se réfère à l’objet de connexion. C’est le même objet `$.hubConnection()` que vous obtenez en appelant lorsque vous n’utilisez pas le proxy généré. Le code proxy généré crée la connexion pour vous en exécutant la déclaration suivante :

![Création d’une connexion dans le fichier proxy généré](hubs-api-guide-javascript-client/_static/image3.png)

Lorsque vous utilisez le proxy généré, vous `$.connection.hub` pouvez faire n’importe quoi avec ce que vous pouvez faire avec un objet de connexion lorsque vous n’utilisez pas le proxy généré.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Exécution asynchrone de la méthode de départ

La `start` méthode s’exécute asynchronement. Il renvoie un [objet différé jQuery](http://api.jquery.com/category/deferred-object/), ce qui signifie que `pipe`vous `done`pouvez `fail`ajouter des fonctions de rappel en appelant des méthodes telles que , , et . Si vous avez du code que vous souhaitez exécuter après la mise en place de la connexion, comme un appel à une méthode serveur, mettez ce code dans une fonction de rappel ou appelez-le à partir d’une fonction de rappel. La `.done` méthode de rappel est exécutée après la connexion a été `OnConnected` établie, et après tout code que vous avez dans votre méthode de gestionnaire d’événement sur le serveur termine l’exécution.

Si vous mettez l’instruction "Maintenant connectée" de l’exemple `start` précédent comme la `.done` prochaine ligne `console.log` de code après l’appel de méthode (pas dans un rappel), la ligne s’exécutera avant que la connexion soit établie, comme indiqué dans l’exemple suivant:

![Mauvaise façon d’écrire du code qui s’exécute après la connexion est établie](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Comment établir une connexion inter-domaines

Typiquement, si le navigateur `http://contoso.com`charge une page à partir de `http://contoso.com/signalr`, la connexion SignalR est dans le même domaine, à . Si la `http://contoso.com` page de `http://fabrikam.com/signalr`fait une connexion à , c’est une connexion cross-domain. Pour des raisons de sécurité, les connexions inter-domaines sont désactivées par défaut.

Dans SignalR 1.x, les demandes de domaine transversal ont été contrôlées par un seul drapeau EnableCrossDomain. Ce drapeau contrôlait à la fois les demandes de JSONP et de CORS. Pour une plus grande flexibilité, tout le support CORS a été supprimé de la composante serveur de SignalR (les clients JavaScript utilisent toujours CORS normalement s’il est détecté que le navigateur le prend en charge), et de nouveaux middleware OWIN a été mis à disposition pour prendre en charge ces scénarios.

Si JSONP est nécessaire sur le client (pour prendre en charge les demandes de `EnableJSONP` cross-domain dans les anciens navigateurs), il devra être activé explicitement en définissant sur l’objet `HubConfiguration` à `true`, comme indiqué ci-dessous. JSONP est désactivé par défaut, car il est moins sûr que CORS.

**Ajout de Microsoft.Owin.Cors à votre projet :** Pour installer cette bibliothèque, exécutez la commande suivante dans la console Package Manager :

`Install-Package Microsoft.Owin.Cors`

Cette commande ajoutera la version 2.1.0 du paquet à votre projet.

### <a name="calling-usecors"></a>Appel UseCors

 L’extrait de code suivant montre comment implémenter des connexions inter-domaines dans SignalR 2.

**Mise en œuvre des demandes inter-domaines dans SignalR 2**

Le code suivant montre comment activer CORS ou JSONP dans un projet SignalR 2. Cet exemple `Map` de `RunSignalR` code `MapSignalR`utilise et au lieu de , de sorte que le middleware CORS fonctionne uniquement `MapSignalR`pour les demandes SignalR qui nécessitent une prise en charge CORS (plutôt que pour tout le trafic sur le chemin spécifié dans .) La carte peut également être utilisée pour tout autre middleware qui doit s’exécuter pour un préfixe URL spécifique, plutôt que pour l’ensemble de l’application.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - Ne définissez `jQuery.support.cors` pas pour vrai dans votre code.
>
>     ![Ne définissez pas jQuery.support.cors à vrai](hubs-api-guide-javascript-client/_static/image7.png)
>
>     SignalR gère l’utilisation de CORS. Réglage `jQuery.support.cors` à vrai désactive JSONP parce qu’il provoque SignalR à assumer le navigateur prend en charge CORS.
> - Lorsque vous vous connectez à une URL locale, Internet Explorer 10 ne le considérera pas comme une connexion cross-domain, de sorte que l’application fonctionnera localement avec IE 10, même si vous n’avez pas activé les connexions cross-domain sur le serveur.
> - Pour plus d’informations sur l’utilisation de connexions cross-domain avec Internet Explorer 9, voir [ce fil StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Pour plus d’informations sur l’utilisation de connexions cross-domain avec Chrome, consultez [ce thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - Le code de l’échantillon utilise l’URL par défaut "/signaleur" pour se connecter à votre service SignalR. Pour plus d’informations sur la façon de spécifier une URL de base différente, voir [ASP.NET Guide d’API De SignalR Hubs - Serveur - L’URL /signalur](hubs-api-guide-server.md#signalrurl).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Comment configurer la connexion

Avant d’établir une connexion, vous pouvez spécifier les paramètres de chaîne de requête ou spécifier le mode de transport.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Comment spécifier les paramètres de chaîne de requête

Si vous souhaitez envoyer des données au serveur lorsque le client se connecte, vous pouvez ajouter des paramètres de chaîne de requête à l’objet de connexion. Les exemples suivants montrent comment définir un paramètre de chaîne de requête dans le code client.

**Définissez une valeur de chaîne de requête avant d’appeler la méthode de démarrage (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

**Définissez une valeur de chaîne de requête avant d’appeler la méthode de démarrage (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

L’exemple suivant montre comment lire un paramètre de chaîne de requête dans le code serveur.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Comment spécifier le mode de transport

Dans le cadre du processus de connexion, un client SignalR négocie normalement avec le serveur pour déterminer le meilleur transport qui est pris en charge par le serveur et le client. Si vous savez déjà quel transport vous souhaitez utiliser, vous pouvez contourner ce `start` processus de négociation en spécifiant le mode de transport lorsque vous appelez la méthode.

**Code client qui spécifie la méthode de transport (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

**Code client qui spécifie la méthode de transport (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

Comme alternative, vous pouvez spécifier plusieurs méthodes de transport dans l’ordre dans lequel vous souhaitez SignalR pour les essayer:

**Code client qui spécifie un système de repli sur mesure des transports (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Code client qui spécifie un système de repli sur mesure des transports (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Vous pouvez utiliser les valeurs suivantes pour spécifier le mode de transport :

- "webSockets"
- "ForeverFrame"
- "serverSentEvents"
- "longPolling"

Les exemples suivants montrent comment savoir quelle méthode de transport est utilisée par une connexion.

**Code client qui affiche la méthode de transport utilisée par une connexion (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

**Code client qui affiche la méthode de transport utilisée par une connexion (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

Pour plus d’informations sur la façon de vérifier la méthode de transport dans le code serveur, voir [ASP.NET SignalR Hubs API Guide - Server - Comment obtenir des informations sur le client à partir de la propriété Context](hubs-api-guide-server.md#contextproperty). Pour plus d’informations sur les transports et les repli, voir [Introduction à SignalR - Transports et Fallbacks](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Comment obtenir un proxy pour une classe Hub

Chaque objet de connexion que vous créez encapsule des informations sur une connexion à un service SignalR qui contient une ou plusieurs classes Hub. Pour communiquer avec une classe Hub, vous utilisez un objet proxy que vous créez vous-même (si vous n’utilisez pas le proxy généré) ou qui est généré pour vous.

Sur le client, le nom proxy est une version à dos de chameau du nom de la classe Hub. SignalR effectue automatiquement cette modification de sorte que le code JavaScript peut se conformer aux conventions JavaScript.

**Classe Hub sur le serveur**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

**Obtenez une référence au proxy client généré pour le Hub**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

**Créez un proxy client pour la classe Hub (sans proxy généré)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

Si vous décorez votre `HubName` classe Hub avec un attribut, utilisez le nom exact sans changer de boîtier.

**Classe Hub sur serveur avec l’attribut HubName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

**Obtenez une référence au proxy client généré pour le Hub**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

**Créez un proxy client pour la classe Hub (sans proxy généré)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Comment définir les méthodes sur le client que le serveur peut appeler

Pour définir une méthode que le serveur peut appeler à partir d’un Hub, ajoutez un gestionnaire d’événements au proxy Hub en utilisant la `client` propriété du proxy généré, ou appelez la `on` méthode si vous n’utilisez pas le proxy généré. Les paramètres peuvent être des objets complexes.

Ajoutez le gestionnaire d’événements avant d’appeler la `start` méthode pour établir la connexion. (Si vous souhaitez ajouter des gestionnaires `start` d’événements après avoir appelé la méthode, voir la note dans [Comment établir une connexion](#establishconnection) plus tôt dans ce document, et utiliser la syntaxe indiquée pour définir une méthode sans utiliser le proxy généré.)

L’appariement du nom de la méthode est insensible aux cas. Par `Clients.All.addContosoChatMessageToPage` exemple, sur le `AddContosoChatMessageToPage` `addContosoChatMessageToPage`serveur `addcontosochatmessagetopage` s’exécute, , ou sur le client.

**Définir la méthode sur le client (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

**Autre moyen de définir la méthode sur le client (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

**Définir la méthode sur le client (sans le proxy généré, ou lors de l’ajout après avoir appelé la méthode de démarrage)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

**Code serveur qui appelle la méthode client**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

Les exemples suivants incluent un objet complexe comme paramètre de méthode.

**Définir la méthode sur le client qui prend un objet complexe (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

**Définir la méthode sur le client qui prend un objet complexe (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

**Code serveur qui définit l’objet complexe**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

**Code serveur qui appelle la méthode client à l’aide d’un objet complexe**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Comment appeler les méthodes du serveur du client

Pour appeler une méthode de serveur `server` du client, utilisez `invoke` la propriété du proxy généré ou de la méthode sur le proxy Hub si vous n’utilisez pas le proxy généré. La valeur de retour ou les paramètres peuvent être des objets complexes.

Passez dans une version camel-case du nom de la méthode sur le Hub. SignalR effectue automatiquement cette modification de sorte que le code JavaScript peut se conformer aux conventions JavaScript.

Les exemples suivants montrent comment appeler une méthode serveur qui n’a pas de valeur de retour et comment appeler une méthode serveur qui a une valeur de retour.

**Méthode de serveur sans attribut HubMethodName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

**Code serveur qui définit l’objet complexe passé dans un paramètre**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

**Code client qui invoque la méthode du serveur (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

**Code client qui invoque la méthode du serveur (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

Si vous avez décoré la `HubMethodName` méthode Hub avec un attribut, utilisez ce nom sans changer de boîtier.

**Méthode de serveur** avec un attribut HubMethodName

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

**Code client qui invoque la méthode du serveur (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

**Code client qui invoque la méthode du serveur (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

Les exemples précédents montrent comment appeler une méthode serveur qui n’a aucune valeur de retour. Les exemples suivants montrent comment appeler une méthode serveur qui a une valeur de retour.

**Code serveur pour une méthode qui a une valeur de retour**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

**La classe stock utilisée pour la** valeur de retour

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

**Code client qui invoque la méthode du serveur (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

**Code client qui invoque la méthode du serveur (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Comment gérer les événements de connexion à vie

SignalR fournit les événements de connexion suivants à vie que vous pouvez gérer :

- `starting`: Augmenté avant que des données soient envoyées sur la connexion.
- `received`: Augmenté lorsque des données sont reçues sur la connexion. Fournit les données reçues.
- `connectionSlow`: Élevé lorsque le client détecte une connexion lente ou fréquemment en baisse.
- `reconnecting`: Augmenté lorsque le transport sous-jacent commence à se reconnecter.
- `reconnected`: Augmenté lorsque le transport sous-jacent s’est reconnecté.
- `stateChanged`: Augmenté lorsque l’état de connexion change. Fournit l’ancien état et le nouvel état (Connecting, Connected, Reconnecting, ou Déconnecté).
- `disconnected`: Augmenté lorsque la connexion s’est déconnectée.

Par exemple, si vous souhaitez afficher des messages d’avertissement lorsqu’il y a des problèmes de connexion qui peuvent causer des retards notables, gérez l’événement. `connectionSlow`

**Gérer l’événement connectionSlow (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

**Gérer l’événement connectionSlow (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

Pour plus d’informations, voir [Comprendre et gérer les événements à vie de connexion dans SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Comment gérer les erreurs

Le client SignalR JavaScript fournit un `error` événement pour lequel vous pouvez ajouter un gestionnaire. Vous pouvez également utiliser la méthode d’échec pour ajouter un gestionnaire pour les erreurs qui résultent d’une invocation de méthode de serveur.

Si vous n’activez pas explicitement les messages d’erreur détaillés sur le serveur, l’objet d’exception que SignalR renvoie après une erreur contient un minimum d’informations sur l’erreur. Par exemple, si `newContosoChatMessage` un appel échoue, le message`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`d’erreur de l’objet d’erreur contient « L’envoi de messages d’erreur détaillés aux clients en production n’est pas recommandé pour des raisons de sécurité, mais si vous souhaitez activer des messages d’erreur détaillés à des fins de dépannage, utilisez le code suivant sur le serveur.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

L’exemple suivant montre comment ajouter un gestionnaire pour l’événement d’erreur.

**Ajouter un gestionnaire d’erreur (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

**Ajouter un gestionnaire d’erreur (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

L’exemple suivant montre comment gérer une erreur à partir d’une invocation de la méthode.

**Gérer une erreur à partir d’une invocation de la méthode (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

**Gérer une erreur à partir d’une invocation de la méthode (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Si une invocation de `error` méthode échoue, l’événement `error` est également soulevé, ainsi votre code dans le gestionnaire de méthode et dans le `.fail` rappel de méthode exécuterait.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Comment permettre l’enregistrement côté client

Pour activer la connexion côté client, `logging` définissez la propriété sur `start` l’objet de connexion avant d’appeler la méthode pour établir la connexion.

**Activez l’enregistrement (avec le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

**Activez l’enregistrement (sans le proxy généré)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Pour voir les journaux, ouvrez les outils de développeur de votre navigateur et allez à l’onglet Console. Pour un tutoriel qui montre des instructions étape par étape et des captures d’écran qui montrent comment le faire, voir [Server Broadcast avec ASP.NET Signalr - Activez l’enregistrement](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).
