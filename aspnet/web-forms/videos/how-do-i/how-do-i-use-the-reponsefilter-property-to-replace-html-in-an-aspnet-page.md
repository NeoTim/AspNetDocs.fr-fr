---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Comment faire] Utilisez la propriété Reponse.Filter pour remplacer le code HTML dans une Page ASP.NET | Microsoft Docs'
author: rick-anderson
description: Dans cette vidéo Chris Pels montre comment utiliser la propriété Reponse.Filter pour intercepter et de modifier le code HTML envoyé à une page. Tout d’abord, un exemple de page est créée w...
ms.author: riande
ms.date: 01/29/2009
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 2ebd9162f81f5270c92c6b8d55e2d2dad4660701
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59403426"
---
# <a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="7476d-104">[Comment faire] Utilisez la propriété Reponse.Filter pour remplacer le code HTML dans une Page ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7476d-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>

<span data-ttu-id="7476d-105">par [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="7476d-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="7476d-106">Dans cette vidéo Chris Pels montre comment utiliser la propriété Reponse.Filter pour intercepter et de modifier le code HTML envoyé à une page.</span><span class="sxs-lookup"><span data-stu-id="7476d-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="7476d-107">Tout d’abord, un exemple de page est créé avec du texte simple.</span><span class="sxs-lookup"><span data-stu-id="7476d-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="7476d-108">Ensuite, une classe Stream personnalisée est créée qui sert le flux de remplacement pour le flux actuel qui est envoyé au navigateur de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7476d-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="7476d-109">Dans cette classe de flux personnalisée, le contenu de la page est récupéré à partir du flux, modifié et puis écrites dans le flux de réponse.</span><span class="sxs-lookup"><span data-stu-id="7476d-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="7476d-110">Dans cette classe Stream personnalisée, la méthode Write est personnalisée pour remplacer le code HTML dans le flux de réponse de base, altérant ainsi ce qui est envoyé au navigateur de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7476d-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="7476d-111">Enfin, la nouvelle classe de flux de données est affectée à la propriété Response.Filter dans la Page\_charge l’événement, ce qui, en fournissant le mécanisme permettant de modifier le contenu de la page.</span><span class="sxs-lookup"><span data-stu-id="7476d-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="7476d-112">&#9654;Regardez la vidéo (13 minutes)</span><span class="sxs-lookup"><span data-stu-id="7476d-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
