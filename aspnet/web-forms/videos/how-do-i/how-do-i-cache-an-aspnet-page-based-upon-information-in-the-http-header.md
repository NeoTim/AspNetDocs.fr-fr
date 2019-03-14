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
ms.openlocfilehash: c90a3db1357df062909ad0e3b73fdeeb3dc16329
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037236"
---
<a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a>[Comment faire]  Cache une Page ASP.NET en fonction des informations dans l’en-tête HTTP
====================
par [Chris Pels](https://twitter.com/chrispels)

Dans cette vidéo Chris Pels montre comment conserver une page dans le cache de sortie ASP.NET en fonction des informations dans l’en-tête HTTP de la page. Tout d’abord, les valeurs d’en-tête HTTP potentiels sont examinés. Ensuite, un exemple de page est créé et ensuite la directive OutputCache est utilisée avec l’attribut VaryByHeader qui contient une valeur « accept--language », un en-tête HTTP, pour contrôler la mise en cache en fonction de la langue du navigateur de l’utilisateur. L’exemple de page est affiché dans Internet Explorer qui est définie sur anglais, puis dans FireFox, qui est configuré pour utiliser le Français. Enfin, l’option pour déplacer la définition du cache pour un CacheProfile dans le fichier web.config est abordée.

[&#9654;Regardez la vidéo (12 minutes)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
