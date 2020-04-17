---
uid: signalr/overview/older-versions/troubleshooting
title: Dépannage SignalR (SignalR 1.x) Microsoft Docs
author: bradygaster
description: Cet article décrit les problèmes communs avec le développement d’applications SignalR.
ms.author: bradyg
ms.date: 06/05/2013
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: e65ce086d28cff2a36c609f47a05af632081be63
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543091"
---
# <a name="signalr-troubleshooting-signalr-1x"></a>Résolution des problèmes de SignalR (SignalR 1.x)

par [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ce document décrit les problèmes courants de dépannage avec SignalR.

Ce document contient les sections suivantes.

- [Les méthodes d’appel entre le client et le serveur échouent silencieusement](#connection)
- [Autres problèmes de connexion](#other)
- [Compilation et erreurs côté serveur](#server)
- [Problèmes de studio visuel](#vs)
- [Questions relatives aux services d’information sur Internet](#iis)
- [Questions Azure](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>Les méthodes d’appel entre le client et le serveur échouent silencieusement

Cette section décrit les causes possibles d’un appel de méthode entre le client et le serveur pour échouer sans un message d’erreur significatif. Dans une application SignalR, le serveur n’a aucune information sur les méthodes que le client implémente; lorsque le serveur invoque une méthode client, le nom de la méthode et les données de paramètres sont envoyés au client, et la méthode n’est exécutée que si elle existe dans le format spécifié par le serveur. Si aucune méthode de correspondance n’est trouvée sur le client, rien ne se passe, et aucun message d’erreur n’est soulevé sur le serveur.

Pour enquêter davantage sur les méthodes des clients ne se faisant pas appeler, vous pouvez activer la connexion avant d’appeler la méthode de démarrage sur le hub pour voir quels appels proviennent du serveur. Pour activer la connexion à une application JavaScript, découvrez [comment activer la connexion client (version client JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Pour activer la connexion dans une application client .NET, voir [Comment activer l’enregistrement côté client (.NET Client version)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Méthode mal orthographiée, signature incorrecte de la méthode ou nom incorrect du moyeu

Si le nom ou la signature d’une méthode appelée ne correspond pas exactement à une méthode appropriée sur le client, l’appel échouera. Vérifiez que le nom de méthode appelé par le serveur correspond au nom de la méthode sur le client. En outre, SignalR crée le proxy hub en utilisant des méthodes à `SendMessage` dos de chameau, `sendMessage` comme il est approprié dans JavaScript, de sorte qu’une méthode appelée sur le serveur serait appelé dans le proxy client. Si vous `HubName` utilisez l’attribut dans votre code côté serveur, vérifiez que le nom utilisé correspond au nom utilisé pour créer le moyeu sur le client. Si vous n’utilisez pas l’attribut, `HubName` vérifiez que le nom du hub d’un client JavaScript est camouflé, comme chatHub au lieu de ChatHub.

### <a name="duplicate-method-name-on-client"></a>Nom de méthode dupliqué sur le client

Vérifiez que vous n’avez pas de méthode en double sur le client qui ne diffère que par cas. Si votre application client `sendMessage`a une méthode appelée, vérifiez `SendMessage` qu’il n’y a pas aussi une méthode appelée ainsi.

### <a name="missing-json-parser-on-the-client"></a>Analyseur JSON manquant sur le client

SignalR exige qu’un analyseur JSON soit présent pour sérialiser les appels entre le serveur et le client. Si votre client n’a pas de analyse JSON intégré (comme Internet Explorer 7), vous devrez en inclure un dans votre application. Vous pouvez télécharger le parser JSON [ici](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Mixage Hub et Syntaxe PersistentConnection

SignalR utilise deux modèles de communication : Hubs et PersistentConnections. La syntaxe pour appeler ces deux modèles de communication est différente dans le code client. Si vous avez ajouté un hub dans votre code serveur, vérifiez que tout votre code client utilise la syntaxe appropriée.

**Code client JavaScript qui crée une connexion persistante dans un client JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Code client JavaScript qui crée un proxy Hub dans un client Javascript**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**Code serveur CMD qui cartographie un itinéraire vers une connexion persistante**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**Code serveur CMD qui cartographie un itinéraire vers un Hub, ou vers plusieurs hubs si vous avez plusieurs applications**

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a>Connexion commencée avant l’ajout d’abonnements

Si la connexion du Hub est commencée avant que les méthodes qui peuvent être appelées à partir du serveur soient ajoutées au proxy, les messages ne seront pas reçus. Le code JavaScript suivant ne démarre pas correctement le moyeu :

**Code client JavaScript incorrect qui ne permettra pas de recevoir les messages Hubs**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

Au lieu de cela, ajoutez les abonnements de méthode avant d’appeler Démarrer :

**Code client JavaScript qui ajoute correctement des abonnements à un hub**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Nom de méthode manquant sur le proxy hub

Vérifiez que la méthode définie sur le serveur est abonnée sur le client. Même si le serveur définit la méthode, elle doit tout de même être ajoutée au proxy du client. Des méthodes peuvent être ajoutées au proxy du client de `client` la manière suivante (Notez que la méthode est ajoutée au membre du hub, et non directement au moyeu) :

**Code client JavaScript qui ajoute des méthodes à un proxy hub**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Méthodes de hub ou de hub non déclarées publiques

Pour être visible sur le client, la mise `public`en œuvre et les méthodes du hub doivent être déclarées comme .

### <a name="accessing-hub-from-a-different-application"></a>Accès au hub à partir d’une application différente

Les hubs SignalR ne peuvent être consultés que par le biais d’applications qui implémentent les clients SignalR. SignalR ne peut pas interopérer avec d’autres bibliothèques de communication (comme les services Web SOAP ou WCF).) S’il n’y a pas de client SignalR disponible pour votre plate-forme cible, vous ne pouvez pas accéder directement au point de terminaison du serveur.

### <a name="manually-serializing-data"></a>Données sérialisant manuellement

SignalR utilisera automatiquement JSON pour sérialiser vos paramètres de méthode, il n’est pas nécessaire de le faire vous-même.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Méthode Remote Hub non exécutée sur le client dans la fonction OnDisconnected

Ce comportement est normal. Lorsqu’il `OnDisconnected` est appelé, le `Disconnected` hub est déjà entré dans l’état, ce qui ne permet pas d’appeler d’autres méthodes de moyeu.

**Code serveur CMD qui exécute correctement le code dans l’événement OnDisconnected**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a>Limite de connexion atteinte

Lors de l’utilisation de la version complète de l’IIS sur un système d’exploitation client comme Windows 7, une limite de 10 connexions est imposée. Lorsque vous utilisez un système d’exploitation client, utilisez IIS Express plutôt pour éviter cette limite.

### <a name="cross-domain-connection-not-set-up-properly"></a>Connexion cross-domain non configuré correctement

Si une connexion cross-domain (une connexion pour laquelle l’URL SignalR n’est pas dans le même domaine que la page d’hébergement) n’est pas configurée correctement, la connexion peut échouer sans un message d’erreur. Pour plus d’informations sur la façon d’activer la communication inter-domaines, voir [Comment établir une connexion inter-domaines](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>Connexion à l’aide de NTLM (Active Directory) ne fonctionne pas dans le client .NET

Une connexion dans une application client .NET qui utilise la sécurité de Domaine peut échouer si la connexion n’est pas configurée correctement. Pour utiliser SignalR dans un environnement de domaine, définissez la propriété de connexion requise comme suit :

**Code client CMD qui implémente les informations d’identification de connexion**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a>Autres problèmes de connexion

Cette section décrit les causes et les solutions pour des symptômes ou des messages d’erreur spécifiques qui se produisent lors d’une connexion.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Erreur de « Démarrer avant que les données puissent être envoyées »

Cette erreur est généralement visible si le code fait référence aux objets SignalR avant le début de la connexion. L’écoute électronique pour les gestionnaires et autres que les méthodes d’appel définies sur le serveur doivent être ajoutées après la fin de la connexion. Notez que `Start` l’appel à est asynchrone, de sorte que le code après l’appel peut être exécuté avant qu’il ne se termine. La meilleure façon d’ajouter des gestionnaires après une connexion commence complètement est de les mettre dans une fonction de rappel qui est passé comme un paramètre à la méthode de départ:

**Code client JavaScript qui ajoute correctement les gestionnaires d’événements qui font référence aux objets SignalR**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Cette erreur sera également visible si une connexion s’arrête pendant que les objets SignalR sont toujours référencés.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>Erreur "301 déplacée de façon permanente" ou "302 déplacé temporairement"

Cette erreur peut être vue si le projet contient un dossier appelé SignalR, ce qui interfère avec le proxy créé automatiquement. Pour éviter cette erreur, n’utilisez `SignalR` pas de dossier appelé dans votre application, ou éteignez la génération de proxy automatique. Voir [The Generated Proxy et ce qu’il fait pour vous](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) pour plus de détails.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>Erreur "403 Interdit" dans .NET ou Silverlight client

Cette erreur peut se produire dans des environnements trans-domaines où la communication inter-domaines n’est pas correctement activée. Pour plus d’informations sur la façon d’activer la communication inter-domaines, voir [Comment établir une connexion inter-domaines](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Pour établir une connexion cross-domain dans un client Silverlight, voir [les connexions Cross-domain des clients Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>Erreur "404 Non trouvée"

Il y a plusieurs causes à ce problème. Vérifier tous les éléments suivants :

- **Référence d’adresse de proxy Hub non formatée correctement :** Cette erreur est généralement visible si la référence à l’adresse proxy hub générée n’est pas formatée correctement. Vérifiez que la référence à l’adresse du hub est faite correctement. Voir [comment faire référence au proxy généré dynamiquement](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) pour plus de détails.
- **Ajout d’itinéraires à l’application avant d’ajouter l’itinéraire hub :** Si votre application utilise d’autres itinéraires, vérifiez `MapHubs`que le premier itinéraire ajouté est l’appel à .

### <a name="500-internal-server-error"></a>"500 Erreur de serveur interne"

Il s’agit d’une erreur très générique qui pourrait avoir une grande variété de causes. Les détails de l’erreur doivent apparaître dans le journal de l’événement du serveur, ou peuvent être trouvés en débogage du serveur. Des informations d’erreur plus détaillées peuvent être obtenues en allumant des erreurs détaillées sur le serveur. Pour plus d’informations, voir [Comment gérer les erreurs dans la classe Hub](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>Erreur "TypeError: &lt;&gt; hubType n’est pas définie"

Cette erreur se traduira `MapHubs` si l’appel à n’est pas fait correctement. Découvrez [comment enregistrer l’itinéraire SignalR et configurer les options SignalR](../guide-to-the-api/hubs-api-guide-server.md#route) pour plus d’informations.

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException n’a pas été géré par le code utilisateur

Vérifiez que les paramètres que vous envoyez à vos méthodes n’incluent pas les types non sérialisables (comme les poignées de fichiers ou les connexions de base de données). Si vous devez utiliser des membres sur un objet côté serveur que vous ne souhaitez pas être envoyé au `JSONIgnore` client (pour la sécurité ou pour des raisons de sérialisation), utilisez l’attribut.

### <a name="protocol-error-unknown-transport-error"></a>Erreur de protocole : erreur de transport inconnu

Cette erreur peut se produire si le client ne prend pas en charge les transports que SignalR utilise. Consultez [Transports et Fallbacks](../getting-started/introduction-to-signalr.md#transports) pour plus d’informations sur les navigateurs qui peuvent être utilisés avec SignalR.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"JavaScript Hub proxy generation has been disabled."

Cette erreur se `DisableJavaScriptProxies` produira si elle est définie tout en `signalr/hubs`incluant une référence au proxy généré dynamiquement à . Pour plus d’informations sur la création de la procuration manuellement, voir [Le proxy généré et ce qu’il fait pour vous](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"L’ID de connexion est dans le format incorrect" ou "L’identité de l’utilisateur ne peut pas changer lors d’une connexion SignalR active" erreur

Cette erreur peut être vue si l’authentification est utilisée, et le client est déconnecté avant que la connexion soit arrêtée. La solution est d’arrêter la connexion SignalR avant d’enregistrer le client.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"Erreur non-tendue: SignalR: jQuery pas trouvé. S’il vous plaît assurez-vous jQuery est référencé avant le fichier SignalR.js" erreur

Le client SignalR JavaScript nécessite jQuery pour fonctionner. Vérifiez que votre référence à jQuery est correcte, que le chemin utilisé est valide, et que la référence à jQuery est avant la référence à SignalR.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"Uncaught TypeError: Impossible&lt;de&gt;lire la propriété ' propriété ' de non défini" erreur

Cette erreur résulte de ne pas avoir jQuery ou les hubs proxy référencé correctement. Vérifiez que votre référence à jQuery et le proxy des hubs est correcte, que le chemin utilisé est valide, et que la référence à jQuery est avant la référence au proxy des hubs. La référence par défaut au proxy des hubs doit ressembler à ce qui suit :

**Code HTML côté client qui fait correctement référence au proxy Hubs**

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>"RuntimeBinderException n’a pas été géré par le code utilisateur" erreur

Cette erreur peut se produire `Hub.On` lorsque la surcharge incorrecte est utilisée. Si la méthode a une valeur de retour, le type de rendement doit être spécifié comme paramètre de type générique :

**Méthode définie sur le client (sans proxy généré)**

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>L’id de connexion est incohérent ou les ruptures de connexion entre les charges de page

Ce comportement est normal. Étant donné que l’objet hub est hébergé dans l’objet de la page, le moyeu est détruit lorsque la page se actualise. Une application de plusieurs pages doit maintenir l’association entre les utilisateurs et les ID de connexion afin qu’ils soient cohérents entre les charges de page. Les pièces d’ID de connexion peuvent `ConcurrentDictionary` être stockées sur le serveur dans un objet ou une base de données.

### <a name="value-cannot-be-null-error"></a>Erreur de « valeur ne peut pas être nulle »

Les méthodes côté serveur avec paramètres facultatifs ne sont pas actuellement prises en charge; si le paramètre facultatif est omis, la méthode échouera. Pour plus d’informations, voir [Paramètres facultatifs](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox ne peut pas établir une &lt;&gt;connexion au serveur à l’adresse" erreur dans Firebug

Ce message d’erreur peut être vu dans Firebug si la négociation du transport WebSocket échoue et un autre transport est utilisé à la place. Ce comportement est normal.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>"Le certificat à distance est invalide selon la procédure de validation" erreur dans l’application client .NET

Si votre serveur nécessite des certificats clients personnalisés, vous pouvez ajouter un x509certificate à la connexion avant que la demande ne soit faite. Ajouter le certificat à `Connection.AddClientCertificate`la connexion à l’aide de .

### <a name="connection-drops-after-authentication-times-out"></a>La connexion diminue après les temps d’authentification

Ce comportement est normal. Les informations d’authentification ne peuvent pas être modifiées pendant qu’une connexion est active; pour actualiser les informations d’identification, la connexion doit être arrêtée et redémarrée.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected est appelé deux fois lors de l’utilisation de jQuery Mobile

La fonction de `initializePage` jQuery Mobile force les scripts de chaque page à être réécrits, créant ainsi une deuxième connexion. Les solutions à ce problème comprennent :

- Inclure la référence à jQuery Mobile avant votre fichier JavaScript.
- Désactiver la `initializePage` fonction `$.mobile.autoInitializePage = false`en définissant .
- Attendez que la page termine l’initialisation avant de commencer la connexion.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>Les messages sont retardés dans les applications Silverlight à l’aide d’événements Envoyés par serveur

Les messages sont retardés lors de l’utilisation du serveur envoyé des événements sur Silverlight. Pour forcer l’utilisation de longs sondages, utilisez ce qui suit au démarrage de la connexion :

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>"Permission refusée" en utilisant le protocole Forever Frame

Il s’agit d’un problème connu, décrit [ici](https://github.com/SignalR/SignalR/issues/1963). Ce symptôme peut être vu à l’aide de la dernière bibliothèque JQuery; la solution de contournement est de rétrograder votre demande à JQuery 1.8.2.

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Compilation et erreurs côté serveur

 La section suivante contient des solutions possibles pour compiler et côté serveur des erreurs d’exécution. 

### <a name="reference-to-hub-instance-is-null"></a>La référence à l’instance Hub est nulle

Étant donné qu’une instance de hub est créée pour chaque connexion, vous ne pouvez pas créer une instance d’un hub dans votre code vous-même. Pour appeler les méthodes sur un hub de l’extérieur du hub lui-même, voir [Comment appeler les méthodes des clients et gérer les groupes de l’extérieur de la classe Hub](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) pour obtenir une référence au contexte du hub.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session est nul

Ce comportement est normal. SignalR ne prend pas en charge l’état de session ASP.NET, car permettre à l’état de session briserait la messagerie duplex.

### <a name="no-suitable-method-to-override"></a>Aucune méthode appropriée pour remplacer

Vous pouvez voir cette erreur si vous utilisez le code de la documentation plus ancienne ou des blogs. Vérifiez que vous ne faites pas référence aux noms des méthodes `OnConnectedAsync`qui ont été modifiées ou dépréciées (comme).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl est nul

Ce comportement est normal. Ce membre est déprécié et ne doit pas être utilisé.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>"Un itinéraire nommé 'signalr.hubs' est déjà dans la collection d’itinéraires" erreur

Cette erreur sera `MapHubs` vue si elle est appelée deux fois par votre application. Certains exemples `MapHubs` d’applications sont directement dans le fichier de demande global; d’autres font l’appel dans une classe d’emballage. Assurez-vous que votre demande ne fait pas les deux.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Problèmes de studio visuel

Cette section décrit les problèmes rencontrés dans Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>Script Documents nœud n’apparaît pas dans Solution Explorer

Certains de nos tutoriels vous dirigent vers le nœud "Script Documents" dans Solution Explorer tout en débogage. Ce nœud est produit par le debugger JavaScript, et n’apparaîtra que lorsque vous débogage des clients navigateur dans Internet Explorer; le nœud n’apparaîtra pas si Chrome ou Firefox sont utilisés. Le debugger JavaScript ne fonctionnera pas non plus si un autre debugger client est en cours d’exécution, comme le débbugger Silverlight.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR ne fonctionne pas sur Visual Studio 2008 ou plus tôt

Ce comportement est normal. SignalR nécessite .NET Framework 4 ou plus tard; cela exige que les applications SignalR soient développées dans Visual Studio 2010 ou plus tard.

<a id="iis"></a>

## <a name="iis-issues"></a>Questions DE LAS

Cette section contient des problèmes avec les services d’information Sur Internet.

### <a name="web-site-crashes-after-maphubs-call"></a>Le site Web s’écrase après l’appel de MapHubs

Ce problème a été résolu dans la dernière version de SignalR. Vérifiez que vous utilisez la dernière version de SignalR en mettant à jour votre installation à l’aide de NuGet.

<a id="azure"></a>

## <a name="azure-issues"></a>Questions Azure

Cette section contient des problèmes avec Microsoft Azure.

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>Les messages ne sont pas reçus par l’avion de fond Azure après avoir modifié les noms de sujet

Les sujets utilisés par l’avion de fond Azure ne sont pas destinés à être configurables par l’utilisateur.
