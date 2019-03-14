---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Cycle de vie d’une Application ASP.NET MVC 5 | Microsoft Docs
author: cephalin
description: Télécharger un document PDF que les graphiques du cycle de vie d’une application ASP.NET MVC 5. Ce document de cycle de vie fournit une vue d’ensemble du cycle de vie MVC un...
ms.author: riande
ms.date: 02/28/2014
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 5523217ae5674ac4e5898a150f9e5f0ae3a70d26
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57046616"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Cycle de vie d’une application ASP.NET MVC 5
====================
par [Cephas Lin](https://github.com/cephalin)

[Téléchargez le Document PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Ici, vous pouvez télécharger un document PDF qui soutiennent les graphiques du cycle de vie de chaque application ASP.NET MVC 5, à partir de réception HTTP demande à l’envoi de la réponse HTTP au client. Il est conçu comme un outil de formation pour ceux qui débutent avec ASP.NET MVC et également comme une référence pour ceux qui ont besoin pour Explorer les aspects spécifiques de l’application. Le document PDF présente les caractéristiques suivantes :

- Pertinentes [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) étapes pour vous aider à comprendre où MVC intègre le [le cycle de vie ASP.NET application](https://msdn.microsoft.com/library/bb470252.aspx).
- Une vue d’ensemble du cycle de vie d’application MVC, où vous pouvez comprendre les principales phases que chaque application MVC traverse dans le pipeline de traitement de la requête.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Une vue de détail qui affiche des exercices vers le bas dans les détails du pipeline de traitement de la demande. Vous pouvez comparer la vue d’ensemble et la vue de détail pour voir comment les détails de cycles de vie sont rassemblés dans les différentes étapes. [Télécharger le PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) pour obtenir une vue agrandie.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Positionnement et l’objectif de toutes les méthodes substituables sur la [contrôleur](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) objet dans le pipeline de traitement de demande. Vous pouvez ou peut-être pas la nécessité de remplacer n’importe quelle un méthode, mais il est important de comprendre leur rôle dans le cycle de vie d’application afin que vous pouvez écrire du code à l’étape de cycle de vie approprié à l’effet que vous avez l’intention.
- Diagrammes de haut soufflé montrant comment chacun des types de filtre (authentification, autorisation, action et résultats) est appelée.
- Lier à un article utile ou un blog à partir de chaque point d’intérêt dans la vue détail.


## <a name="next-steps"></a>Étapes suivantes

Ce document répond à vos besoins ? Nous aimerions connaître votre opinion. Si vous avez des questions sur le cycle de vie ASP.NET MVC dans votre application, [Stackoverflow](http://stackoverflow.com/help) et [forums d’ASP.NET MVC](https://forums.asp.net/1146.aspx) formidable pour poser. Suivez [me](https://twitter.com/Cephas_MSFT) sur twitter afin d’obtenir les mises à jour sur mon derniers didacticiels.
