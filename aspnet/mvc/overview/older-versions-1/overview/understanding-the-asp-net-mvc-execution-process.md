---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Comprendre le processus d’exécution ASP.NET MVC (fr) Microsoft Docs
author: rick-anderson
description: Découvrez comment le cadre MVC ASP.NET traite une demande de navigateur étape par étape.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 48afbbe7349b80e0ed0b9bab987ae3ccda493aca
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541024"
---
# <a name="understanding-the-aspnet-mvc-execution-process"></a>Présentation du processus d’exécution d’ASP.NET MVC

par [Microsoft](https://github.com/microsoft)

> Découvrez comment le cadre MVC ASP.NET traite une demande de navigateur étape par étape.

Les demandes d’une application Web ASP.NET basée sur MVC passent d’abord par l’objet **UrlRoutingModule,** qui est un module HTTP. Ce module analyse la demande et exécute la sélection d'itinéraire. **L’objet UrlRoutingModule** sélectionne le premier objet d’itinéraire qui correspond à la demande en cours. (Un objet d’itinéraire est une classe qui met en œuvre **RouteBase**, et est généralement une instance de la classe **Route.)** Si aucun itinéraire ne correspond, l’objet **UrlRoutingModule** ne fait rien et laisse la demande revenir au traitement régulier de la demande de ASP.NET ou de l’IIS.

De l’objet **Itinéraire** sélectionné, l’objet **UrlRoutingModule** obtient l’objet **IRouteHandler** associé à l’objet **Route.** Typiquement, dans une application MVC, ce sera un cas de **MvcRouteHandler**. **L’instance IRouteHandler** crée un objet **IHttpHandler** et lui passe l’objet **IHttpContext.** Par défaut, l’instance **IHttpHandler** pour MVC est l’objet **MvcHandler.** **L’objet MvcHandler** sélectionne ensuite le contrôleur qui traitera finalement la demande.

> [!NOTE]
> Lorsqu'une application Web ASP.NET MVC s'exécute dans IIS 7.0, aucune extension de nom de fichier n'est requise pour les projets MVC. Toutefois, dans IIS 6.0, le gestionnaire requiert que vous mappiez l'extension de nom de fichier .mvc à la DLL ISAPI ASP.NET.

Le module et le gestionnaire sont les points d’entrée du cadre ASP.NET MVC. Elles exécutent les actions suivantes :

- Sélection du contrôleur approprié dans une application Web MVC.
- Obtention d'une instance de contrôleur spécifique.
- Appelez la méthode **Execute** du contrôleur.

Les étapes suivantes énumèrent les étapes d’exécution d’un projet Web de MVC :

- Réception de la première demande concernant l'application 

    - Dans le fichier Global.asax, des objets **Route** sont ajoutés à l’objet **RouteTable.**
- Effectuer le routage 

    - Le module **UrlRoutingModule** utilise le premier objet **Itinéraire** correspondant de la collection **RouteTable** pour créer l’objet **RouteData,** qu’il utilise ensuite pour créer un objet **RequestContext** (**IHttpContext).**
- Création d'un gestionnaire de demandes MVC 

    - **L’objet MvcRouteHandler** crée une instance de la classe **MvcHandler** et lui passe l’instance **RequestContext.**
- Création du contrôleur 

    - **L’objet MvcHandler** utilise **l’instance RequestContext** pour identifier **l’objet IControllerFactory** (généralement une instance de la classe **DefaultControllerFactory)** pour créer l’instance du contrôleur avec.
- Exécuter le contrôleur - L’instance **MvcHandler** appelle la méthode **d’exécution** du contrôleur. |
- Appel de l'action 

    - La plupart des contrôleurs héritent de la classe de base **Controller.** Pour les contrôleurs qui le font, l’objet **ControllerActionInvoker** qui est associé au contrôleur détermine la méthode d’action de la classe de contrôleur à appeler, puis appelle cette méthode.
- Exécution du résultat 

    - Une méthode d’action typique peut recevoir l’entrée de l’utilisateur, préparer les données de réponse appropriées, puis exécuter le résultat en retournant un type de résultat. Les types de résultats intégrés qui peuvent être exécutés comprennent ce qui suit: **ViewResult** (qui rend une vue et est le type de résultat le plus souvent utilisé), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, et **EmptyResult**.
