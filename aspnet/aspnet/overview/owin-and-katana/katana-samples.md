---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Échantillons de Katana Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 15cc1084b16db2619f2295ee21dec4f49eb2e354
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540439"
---
# <a name="katana-samples"></a>Exemples Katana

par [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Exemples Katana

**ASP.NET Itinéraires Code** | [source d’échantillons](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)  
Dans certaines applications, vous voudrez brancher les composants OWIN dans la table d’itinéraire Asp.Net côte à côte avec des composants non-OWIN. Cet exemple montre comment utiliser les méthodes d’extension RouteCollection MapOwinPath et MapOwinRoute fournies par Microsoft.Owin.Host.SystemWeb.

**Branchage pipelines Code source** | [d’échantillons](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)  
Les pipelines de traitement des demandes OWIN n’ont pas besoin d’être linéaires, ils peuvent être ramifiés pour traiter les demandes de différentes façons. Cet exemple montre comment construire un pipeline de branchement en fonction des trajectoires de demande ou d’autres données de demande telles que les en-têtes. Ces composants sont disponibles dans le paquet de nuget Microsoft.Owin.Mapping.

**Code source d’échantillon** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) de serveur personnalisé   
Montre comment utiliser un serveur OWIN personnalisé lors de l’auto-hébergement OWIN.

**Code source d’échantillons** | [intégrés](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)  
Certains serveurs OWIN peuvent être exécutés&quot;à l’intérieur de votre propre processus (auto-hébergé ).&quot; Cet exemple montre comment démarrer une application OWIN à l’aide des outils fournis par le paquet de nuget Microsoft.Owin.Hosting.

**Code source d’échantillons** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld) HelloWorld  
OWIN est un serveur HTTP API abstraction qui permet la portabilité d’application sur différents serveurs. Cet exemple montre comment écrire une application Hello World en utilisant quelques **emballages simples** autour de l’abstraction OWIN brute et l’exécuter sur un serveur web comme ASP.NET.

**Bonjour World Raw OWIN Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)  
Cet exemple montre comment écrire une application Hello World en utilisant l’abstraction **OWIN brute** et l’exécuter sur un serveur web comme Asp.Net.

**Code source d’échantillon** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR) SignalR  
Montre comment s’auto-héberger SignalR en utilisant OWIN / Katana. Pour plus d’informations sur l’auto-hébergement SignalR, voir [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Code source d’échantillons** | de fichiers[statiques](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample)   
Montre comment prendre en charge les demandes HTTP de fichiers statiques à l’aide d’OWIN / Katana.

**Web API** | [Code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) Web API   
Cet exemple montre comment héberger OWIN dans l’IIS et ajouter l’API Web au pipeline OWIN.

**Code source d’échantillon** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) Web Socket   
Montre comment prendre en charge les prises Web dans OWIN en utilisant la classe [System.Net.WebSockets.WebSocket.](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx)
