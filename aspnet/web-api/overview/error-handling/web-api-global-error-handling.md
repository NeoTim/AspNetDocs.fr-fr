---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Gestion globale des erreurs dans ASP.NET’API Web 2 - ASP.NET 4.x
author: davidmatson
description: Un aperçu de la gestion globale des erreurs dans ASP.NET Web API 2 pour ASP.NET 4.x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 5ff54d2e4ed881ce927d0a401fb79d9b8bc5b8a1
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675427"
---
# <a name="global-error-handling-in-aspnet-web-api-2"></a>Gestion globale des erreurs dans ASP.NET’API Web 2

par [David Matson](https://github.com/davidmatson), [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce sujet donne un aperçu de la gestion globale des erreurs dans ASP.NET Web API 2 pour ASP.NET 4.x. Aujourd’hui, il n’y a pas de moyen facile dans l’API Web de se connecter ou de gérer des erreurs à l’échelle mondiale. Certaines exceptions non gérées peuvent être traitées par l’intermédiaire de [filtres d’exception,](exception-handling.md)mais il existe un certain nombre de cas que les filtres d’exception ne peuvent pas gérer. Par exemple :

1. Les exceptions lancées à partir des constructeurs de contrôleur.
2. Les exceptions lancées à partir des gestionnaires de messages.
3. Les exceptions lancées pendant le routage.
4. Les exceptions lancées pendant la sérialisation du contenu de réponse.

Nous voulons fournir un moyen simple et cohérent de connecter et de gérer (si possible) ces exceptions. 

Il y a deux cas importants pour le traitement des exceptions, le cas où nous sommes en mesure d’envoyer une réponse d’erreur et le cas où tout ce que nous pouvons faire est de consigner l’exception. Un exemple pour ce dernier cas est lorsqu’une exception est projetée au milieu du contenu de réponse en continu; dans ce cas, il est trop tard pour envoyer un nouveau message de réponse puisque le code d’état, les en-têtes et le contenu partiel ont déjà traversé le fil, donc nous avons simplement interrompre la connexion. Même si l’exception ne peut pas être traitée pour produire un nouveau message de réponse, nous soutenons toujours l’enregistrement de l’exception. Dans les cas où nous pouvons détecter une erreur, nous pouvons retourner une réponse d’erreur appropriée comme indiqué dans ce qui suit :

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Options existantes

En plus des [filtres d’exception,](exception-handling.md) [les gestionnaires de messages](../advanced/http-message-handlers.md) peuvent être utilisés aujourd’hui pour observer toutes les réponses de 500 niveaux, mais il est difficile d’agir sur ces réponses, car ils manquent de contexte au sujet de l’erreur initiale. Les gestionnaires de messages ont également certaines des mêmes limitations que les filtres d’exception concernant les cas qu’ils peuvent traiter. Bien que l’API Web dispose d’une infrastructure de traçage qui capture les conditions d’erreur, l’infrastructure de traçage est à des fins diagnostiques et n’est pas conçue ou adaptée pour fonctionner dans des environnements de production. La manipulation et l’enregistrement des exceptions mondiales devraient être des services qui peuvent fonctionner pendant la production et être branchés aux solutions de surveillance existantes (par exemple, [ELMAH](https://code.google.com/p/elmah/)).

### <a name="solution-overview"></a>Vue d'ensemble de la solution

 Nous fournissons deux nouveaux services remplaçables par les utilisateurs, [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) et IExceptionHandler, pour vous connecter et gérer les exceptions non gérées. Les services sont très similaires, avec deux différences principales:

1. Nous soutenons l’enregistrement de plusieurs enregistreurs d’exception, mais seulement un seul gestionnaire d’exception.
2. Les bûcherons d’exception sont toujours appelés, même si nous sommes sur le point d’interrompre la connexion. Les gestionnaires d’exception ne sont appelés que lorsque nous sommes toujours en mesure de choisir le message de réponse à envoyer.

Les deux services donnent accès à un contexte d’exception contenant des informations pertinentes à partir du point où l’exception a été détectée, en particulier le [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), le [httpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), l’exception jetée et la source d’exception (détails ci-dessous).

### <a name="design-principles"></a>Principes de conception

1. **Pas de changements de rupture** Parce que cette fonctionnalité est ajoutée dans une version mineure, une contrainte importante ayant un impact sur la solution est qu’il n’y a pas de changements de rupture, que ce soit pour taper des contrats ou à un comportement. Cette contrainte exclut un certain nettoyage que nous aimerions avoir fait en termes de blocs de capture existants transformant les exceptions en 500 réponses. Ce nettoyage supplémentaire est quelque chose que nous pourrions envisager pour une version majeure ultérieure. Si cela est important pour vous s’il vous plaît voter sur elle à [ASP.NET voix d’utilisateur Web API](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Maintenir la cohérence avec les constructions d’API Web** Le pipeline de filtres Web API est un excellent moyen de répondre aux préoccupations transversales avec la flexibilité d’appliquer la logique à une portée spécifique à l’action, spécifique au contrôleur ou globale. Les filtres, y compris les filtres d’exception, ont toujours des contextes d’action et de contrôleur, même lorsqu’ils sont enregistrés à portée globale. Ce contrat est logique pour les filtres, mais cela signifie que les filtres d’exception, même à portée globale, ne conviennent pas bien à certains cas de traitement des exceptions, comme les exceptions des gestionnaires de messages, où il n’existe pas de contexte d’action ou de contrôleur. Si nous voulons utiliser la portée flexible offerte par les filtres pour la manipulation des exceptions, nous avons encore besoin de filtres d’exception. Mais si nous devons gérer l’exception en dehors d’un contexte de contrôleur, nous avons également besoin d’une construction distincte pour la gestion complète des erreurs globales (quelque chose sans le contexte du contrôleur et les contraintes de contexte d’action).

### <a name="when-to-use"></a>Quand utiliser

- Les bûcherons d’exception sont la solution pour voir toutes les exceptions non gérées prises par l’API Web.
- Les gestionnaires d’exception sont la solution pour personnaliser toutes les réponses possibles aux exceptions non manipulées prises par l’API Web.
- Les filtres d’exception sont la solution la plus simple pour traiter les exceptions non gérées liées à une action ou à un contrôleur spécifique.

### <a name="service-details"></a>Détails sur le service

 Les interfaces de service d’exception logger et de gestionnaire sont des méthodes async simples prenant les contextes respectifs : 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Nous fournissons également des classes de base pour ces deux interfaces. L’extension des méthodes de base (sync ou async) est tout ce qui est nécessaire pour se connecter ou manipuler aux heures recommandées. Pour l’enregistrement, la `ExceptionLogger` classe de base s’assurera que la méthode d’enregistrement de base n’est appelée qu’une seule fois pour chaque exception (même si elle propage plus tard plus haut la pile d’appel et est capturée à nouveau). La `ExceptionHandler` classe de base appellera la méthode de manutention de base seulement pour des exceptions au sommet de la pile d’appel, ignorant les blocs de capture imbriqués hérités. (Les versions simplifiées de ces classes de base sont dans l’annexe ci-dessous.) Les `IExceptionLogger` `IExceptionHandler` deux et de recevoir `ExceptionContext`des informations sur l’exception via un .

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Lorsque le cadre appelle un enregistreur d’exception ou `Exception` un `Request`gestionnaire d’exception, il fournira toujours un et un . Sauf pour les tests unitaires, il fournira également toujours un `RequestContext`. Il fournira rarement `ControllerContext` `ActionContext` un et (seulement lors de l’appel à partir du bloc de capture pour les filtres d’exception). Il fournira très `Response`rarement un (seulement dans certains cas IIS quand au milieu d’essayer d’écrire la réponse). Notez que parce que `null` certaines de ces propriétés `null` peuvent être, il appartient au consommateur de vérifier avant d’accéder aux membres de la classe d’exception.`CatchBlock` est une chaîne indiquant quel bloc de capture a vu l’exception. Les chaînes de blocs de capture sont les suivantes:

- HttpServer (méthode SendAsync)
- HttpControllerDispatcher (méthode SendAsync)
- HttpBatchHandler (méthode SendAsync)
- IExceptionFilter (ApiController’s processing of the exception filter pipeline in ExecuteAsync)
- OWIN hôte:

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (pour la sortie tampon)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (pour la sortie en streaming)
- Hébergeur Web :

    - HttpControllerHandler.WriteBufferedResponseContentAsync (pour la sortie tampon)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (pour la sortie en streaming)
    - HttpControllerHandler.WriteErrorResponseContentAsync (pour les échecs de récupération d’erreurs en mode de sortie tampon)

La liste des chaînes de blocs de capture est également disponible via des propriétés lisseuses statiques. (La chaîne de bloc de capture de base se trouvent sur les blocs d’exception statiques; le reste apparaît sur une classe statique chacun pour OWIN et l’hébergeur).`IsTopLevelCatchBlock` est utile pour suivre le modèle recommandé de manipulation des exceptions seulement en haut de la pile d’appels. Plutôt que de transformer les exceptions en 500 réponses partout où un bloc de capture imbriqué se produit, un gestionnaire d’exception peut laisser les exceptions se propager jusqu’à ce qu’ils soient sur le point d’être vus par l’hôte.

En plus `ExceptionContext`de la , un bûcheron `ExceptionLoggerContext`obtient un élément de plus de l’information via le plein:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

La deuxième `CanBeHandled`propriété, permet à un bûcheron d’identifier une exception qui ne peut pas être manipulée. Lorsque la connexion est sur le point d’être avortée et qu’aucun nouveau message de réponse ne peut être envoyé, les bûcherons seront appelés, mais le gestionnaire ne sera ***pas*** appelé, et les bûcherons peuvent identifier ce scénario à partir de cette propriété.

En plus `ExceptionContext`de la , un gestionnaire obtient une `ExceptionHandlerContext` propriété de plus, il peut mettre sur le plein pour gérer l’exception:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Un gestionnaire d’exception indique qu’il `Result` a géré une exception en fixant la propriété à un résultat d’action (par exemple, une [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), ou un résultat personnalisé). Si `Result` la propriété est nulle, l’exception n’est pas gérée et l’exception initiale sera recondûment jetée.

Pour les exceptions en haut de la pile d’appels, nous avons pris une mesure supplémentaire pour nous assurer que la réponse est appropriée pour les appelants API. Si l’exception se propage à l’hôte, l’appelant verrait l’écran jaune de la mort ou une autre réponse fournie par l’hôte qui est généralement HTML et généralement pas une réponse appropriée d’erreur d’API. Dans ces cas, le résultat commence non-null, et ce n’est que si un gestionnaire d’exception personnalisé le remet explicitement à `null` (non manipulé) l’exception se propagera à l’hôte. Le `Result` `null` réglage de ces cas peut être utile pour deux scénarios :

1. OWIN a hébergé l’API Web avec une exception personnalisée en manipulant middleware enregistré avant/extérieur API Web.
2. Débogage local via un navigateur, où l’écran jaune de la mort est en fait une réponse utile pour une exception non manipulée.

Pour les enregistreurs d’exception et les gestionnaires d’exception, nous ne faisons rien pour récupérer si le bûcheron ou le gestionnaire lui-même jette une exception. (Autre que de laisser l’exception se propager, laissez la rétroaction au bas de cette page si vous avez une meilleure approche.) Le contrat pour les bûcherons et les manutentionnaires d’exception est qu’ils ne devraient pas laisser les exceptions se propager à leurs appelants; sinon, l’exception se propagera, souvent tout le chemin à l’hôte résultant en une erreur HTML (comme ASP. L’écran jaune de NET) renvoyé au client (qui n’est généralement pas l’option préférée pour les appelants API qui attendent JSON ou XML).

## <a name="examples"></a>Exemples

### <a name="tracing-exception-logger"></a>Traçage De l’exception Logger

Le bûcheron d’exception ci-dessous envoie des données d’exception aux sources Trace configurées (y compris la fenêtre de sortie Debug dans Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Gestionnaire d’exception de message d’erreur personnalisé

Le gestionnaire d’exception ci-dessous produit une réponse personnalisée d’erreur aux clients, y compris une adresse e-mail pour contacter le support.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Enregistrement des filtres d’exception

Si vous utilisez le modèle de projet « ASP.NET MVC 4 Web Application » pour `WebApiConfig` créer votre projet, placez votre code de configuration Web API à l’intérieur de la classe, dans le dossier *App_Start* :

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Annexe: Détails de la classe de base

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
