---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: '[Comment faire]  Cache une Page ASP.NET en fonction des informations dans l’en-tête HTTP | Microsoft Docs'
author: rick-anderson
description: Dans cette vidéo Chris Pels montre comment conserver une page dans le cache de sortie ASP.NET en fonction des informations dans l’en-tête HTTP de la page. Tout d’abord, le potentiel Affich HTTP...
ms.author: riande
ms.date: 02/26/2009
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: 79c27f39793a4a3a94ea412838fb3844579e874d
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59404141"
---
# <a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="1f431-104">[Comment faire]  Cache une Page ASP.NET en fonction des informations dans l’en-tête HTTP</span><span class="sxs-lookup"><span data-stu-id="1f431-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>

<span data-ttu-id="1f431-105">par [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="1f431-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="1f431-106">Dans cette vidéo Chris Pels montre comment conserver une page dans le cache de sortie ASP.NET en fonction des informations dans l’en-tête HTTP de la page.</span><span class="sxs-lookup"><span data-stu-id="1f431-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="1f431-107">Tout d’abord, les valeurs d’en-tête HTTP potentiels sont examinés.</span><span class="sxs-lookup"><span data-stu-id="1f431-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="1f431-108">Ensuite, un exemple de page est créé et ensuite la directive OutputCache est utilisée avec l’attribut VaryByHeader qui contient une valeur « accept--language », un en-tête HTTP, pour contrôler la mise en cache en fonction de la langue du navigateur de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="1f431-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="1f431-109">L’exemple de page est affiché dans Internet Explorer qui est définie sur anglais, puis dans FireFox, qui est configuré pour utiliser le Français.</span><span class="sxs-lookup"><span data-stu-id="1f431-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="1f431-110">Enfin, l’option pour déplacer la définition du cache pour un CacheProfile dans le fichier web.config est abordée.</span><span class="sxs-lookup"><span data-stu-id="1f431-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="1f431-111">&#9654;Regardez la vidéo (12 minutes)</span><span class="sxs-lookup"><span data-stu-id="1f431-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
