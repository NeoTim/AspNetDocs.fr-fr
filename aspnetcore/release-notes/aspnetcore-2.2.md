---
title: Nouveautés d’ASP.NET Core 2.2
author: tdykstra
description: Découvrez les nouvelles fonctionnalités d’ASP.NET Core 2.2.
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/18/2018
uid: aspnetcore-2.2
ms.openlocfilehash: 6dcdf71ec5271690718dd1fe750a9a74d498a0f8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57045106"
---
# <a name="whats-new-in-aspnet-core-22"></a>Nouveautés d’ASP.NET Core 2.2

Cet article met en évidence les modifications les plus importantes dans ASP.NET Core 2.2 et fournit des liens vers la documentation appropriée.

## <a name="open-api-analyzers--conventions"></a>Analyseurs et conventions d’Open API

Open API (également appelé Swagger) est une spécification indépendante du langage pour décrire des API REST. L’écosystème Open API a des outils qui permettent de découvrir, de tester et de produire du code client avec la spécification. La prise en charge de la génération et de la visualisation de documents Open API dans ASP.NET Core MVC est fournie via des projets portés par la communauté, comme [NSwag](https://github.com/RSuter/NSwag) et [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore). ASP.NET Core 2.2 fournit des outils et des expériences de runtime améliorés pour la création de documents Open API.

Pour plus d'informations, reportez-vous aux ressources suivantes :

* <xref:web-api/advanced/analyzers>
* <xref:web-api/advanced/conventions>
* [ASP.NET Core 2.2.0-preview1: Analyseurs et conventions d’Open API](https://blogs.msdn.microsoft.com/webdev/2018/08/23/asp-net-core-2-20-preview1-open-api-analyzers-conventions/)

## <a name="problem-details-support"></a>Prise en charge des détails des problèmes

ASP.NET Core 2.1 a introduit `ProblemDetails`, basé sur la spécification [RFC 7807](https://tools.ietf.org/html/rfc7807) pour convoyer les détails d’une erreur avec une réponse HTTP. Dans la version 2.2, `ProblemDetails` est la réponse standard pour les codes d’erreur des clients dans les contrôleurs avec l’attribut `ApiControllerAttribute`. Un `IActionResult` retournant un code d’état d’erreur du client (4xx) retourne maintenant un corps `ProblemDetails`. Le résultat inclut également un ID de corrélation, qui peut être utilisé pour mettre en corrélation l’erreur en utilisant les journaux des requêtes. Pour les erreurs des clients, `ProducesResponseType` utilise par défaut `ProblemDetails` comme type de réponse. Cela est documenté dans la sortie Open API / Swagger générée avec NSwag ou Swashbuckle.AspNetCore.

## <a name="endpoint-routing"></a>Routage de point de terminaison

ASP.NET Core 2.2 utilise un nouveau système de *routage de point de terminaison* pour améliorer la distribution des requêtes. Les changements sont notamment de nouveaux membres d’API de génération de liens et les transformateurs de paramètres de route.

Pour plus d'informations, reportez-vous aux ressources suivantes :

* [Routage de point de terminaison dans la version 2.2](https://blogs.msdn.microsoft.com/webdev/2018/08/27/asp-net-core-2-2-0-preview1-endpoint-routing/)
* [Transformateurs de paramètres de route](https://www.hanselman.com/blog/ASPNETCore22ParameterTransformersForCleanURLGenerationAndSlugsInRazorPagesOrMVC.aspx) (consultez la section **Routage**)
* [Différences entre le routage IRouter et le routage de point de terminaison](xref:fundamentals/routing?view=aspnetcore-2.2#differences-from-earlier-versions-of-routing)

## <a name="health-checks"></a>Contrôles d’intégrité

Un nouveau service de contrôles d’intégrité facilite l’utilisation d’ASP.NET Core dans les environnements qui nécessitent des contrôles d’intégrité, comme Kubernetes. Les contrôles d’intégrité incluent un middleware (intergiciel), et un ensemble de bibliothèques qui définissent une abstraction et un service `IHealthCheck`.

Les contrôles d’intégrité sont utilisés par un orchestrateur de conteneurs ou un équilibreur de charge pour déterminer rapidement si un système répond normalement aux requêtes. Un orchestrateur de conteneurs peut répondre à un contrôle d’intégrité en échec en arrêtant un déploiement en cours ou en redémarrant un conteneur. Un équilibreur de charge peut répondre à un contrôle d’intégrité en routant le trafic en dehors d’une instance du service en échec.

Les contrôles d’intégrité sont exposés par une application comme point de terminaison HTTP utilisé par les systèmes de surveillance. Les contrôles d’intégrité peuvent être configurés pour une grande variété de scénarios de surveillance en temps réel et de systèmes de surveillance. Le service de contrôles d’intégrité s’intègre au [projet BeatPulse](https://github.com/Xabaril/BeatPulse), ce qui facilite l’ajout de contrôles pour des dizaines de systèmes et de dépendances les plus courants.

Pour plus d’informations, consultez [Contrôles d’intégrité dans ASP.NET Core](xref:host-and-deploy/health-checks).

## <a name="http2-in-kestrel"></a>HTTP/2 dans Kestrel

ASP.NET Core 2.2 ajoute la prise en charge de HTTP/2. 

HTTP/2 est une révision majeure du protocole HTTP. Certaines des principales fonctionnalités de HTTP/2 sont la prise en charge de la compression des en-têtes et le flux entièrement multiplexés sur une même connexion. Si HTTP/2 préserve la sémantique de HTTP (en-têtes HTTP, méthodes, etc.), il s’agit d’un changement majeur par rapport à HTTP/1.x quant à la façon dont les données sont structurées en trames et envoyées sur le réseau.

En raison de ce changement de tramage, les serveurs et les clients doivent négocier la version du protocole utilisée. APLN (Application-Layer Protocol Negotiation) est une extension de TLS qui permet au serveur et au client de négocier la version du protocole utilisé dans le cadre de leur négociation TLS. Bien que le serveur et le client puissent partager une connaissance préalable quant au protocole, tous les principaux navigateurs prennent en charge ALPN comme unique moyen d’établir une connexion HTTP/2.

Pour plus d’informations, consultez [Prise en charge de HTTP/2](xref:fundamentals/servers/index?view=aspnetcore-2.2#http2-support).

## <a name="kestrel-configuration"></a>Configuration de Kestrel

Dans les versions antérieures d’ASP.NET Core, les options de Kestrel sont configurées en appelant `UseKestrel`. Dans la version 2.2, les options de Kestrel sont configurées en appelant `ConfigureKestrel` sur le générateur de l’hôte. Cette modification résout un problème quant à l’ordre des inscriptions `IServer` pour l’hébergement in-process. Pour plus d'informations, reportez-vous aux ressources suivantes :

* [Réduire les conflits liés à UseIIS](https://github.com/aspnet/KestrelHttpServer/issues/2760)
* [Configurer les options du serveur Kestrel avec ConfigureKestrel](xref:fundamentals/servers/kestrel?view=aspnetcore-2.2#how-to-use-kestrel-in-aspnet-core-apps)

## <a name="iis-in-process-hosting"></a>Hébergement in-process d’IIS

Dans les versions antérieures d’ASP.NET Core, IIS sert de proxy inverse. Dans la version 2.2, le module ASP.NET Core peut démarrer CoreCLR et héberger une application au sein du processus worker IIS (*w3wp.exe*). L’hébergement in-process offre des gains en matière de performances et de diagnostic lors de l’exécution avec IIS.

Pour plus d’informations, consultez [Hébergement in-process pour IIS](xref:host-and-deploy/aspnet-core-module?view=aspnetcore-2.2#in-process-hosting-model).

## <a name="signalr-java-client"></a>Client Java SignalR

ASP.NET Core 2.2 introduit un client Java pour SignalR. Ce client prend en charge la connexion à un serveur ASP.NET Core SignalR à partir de code Java, notamment dans des applications Android.

Pour plus d’informations, consultez [Client Java ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/signalr/java-client?view=aspnetcore-2.2).

## <a name="cors-improvements"></a>Améliorations apportées à CORS

Dans les versions antérieures d’ASP.NET Core, le middleware (intergiciel) CORS permet l’envoi des en-têtes `Accept`, `Accept-Language`, `Content-Language` et `Origin` quelles que soient les valeurs configurées dans `CorsPolicy.Headers`. Dans la version 2.2, une correspondance de la stratégie de middleware CORS est possible seulement quand les en-têtes envoyés dans `Access-Control-Request-Headers` correspondent exactement aux en-têtes indiqués dans `WithHeaders`.

Pour plus d’informations, consultez [Middleware CORS](xref:security/cors?view=aspnetcore-2.2#set-the-allowed-request-headers).

## <a name="response-compression"></a>Compression des réponses

ASP.NET Core 2.2 peut compresser les réponses avec le [format de compression Brotli](https://tools.ietf.org/html/rfc7932).

Pour plus d’informations, consultez [Prise en charge de la compression Brotli par le middleware de compression des réponses](xref:performance/response-compression?view=aspnetcore-2.2#brotli-compression-provider).

## <a name="project-templates"></a>Modèles de projet

Les modèles de projet web ASP.NET Core ont été mis à jour vers [Bootstrap 4](https://getbootstrap.com/docs/4.1/migration/) et [Angular 6](https://blog.angular.io/version-6-of-angular-now-available-cc56b0efa7a4). La nouvelle apparence est visuellement plus simple et facilite la visualisation des structures importantes de l’application.

![Page d’accueil ou page d’index](~/tutorials/razor-pages/razor-pages-start/_static/home2.2.png)

## <a name="validation-performance"></a>Performances de la validation

Le système de validation de MVC est conçu pour être extensible et flexible, vous permettant de déterminer pour chaque requête les validateurs à appliquer à un modèle donné. Ceci convient bien à la création de fournisseurs de validation complexe. Cependant, dans la plupart des cas, une application utilise seulement les validateurs intégrés et ne nécessite pas cette flexibilité supplémentaire. Les validateurs intégrés incluent des DataAnnotations comme [Required] et [StringLength], et `IValidatableObject`.

Dans ASP.NET Core 2.2, MVC peut court-circuiter la validation s’il détermine que le graphe d’un modèle donné ne nécessite pas de validation. Ignorer les résultats de la validation permet des améliorations significatives lors de la validation de modèles qui ne peuvent pas avoir ou qui n’ont pas de validateurs. Ceci inclut des objets comme les collections de primitifs (`byte[]`, `string[]`, `Dictionary<string, string>`, etc.) ou des graphes d’objets complexes sans beaucoup de validateurs.

## <a name="http-client-performance"></a>Performances du client HTTP

Dans ASP.NET Core 2.2, les performances de `SocketsHttpHandler` ont été améliorées en réduisant la contention du verrouillage des pools de connexions. Pour les applications qui font de nombreuses requêtes HTTP sortantes, comme certaines architectures de microservices, le débit est amélioré. Sous une charge intensive, le débit de `HttpClient` peut être amélioré d’un facteur allant jusqu’à 60 % sur Linux et jusqu’à 20 % sur Windows.

Pour plus d’informations, consultez [la demande de tirage à l’origine de cette amélioration](https://github.com/dotnet/corefx/pull/32568).

## <a name="additional-information"></a>Informations supplémentaires

Pour obtenir la liste complète des modifications, consultez les [Notes de publication d’ASP.NET Core 2.2 ](https://github.com/aspnet/Home/releases/tag/2.2.0).
