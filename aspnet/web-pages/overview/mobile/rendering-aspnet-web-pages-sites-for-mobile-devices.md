---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Rendu ASP.NET Web Pages (Razor) Sites pour les appareils mobiles | Microsoft Docs
author: Rick-Anderson
description: 'Cet article décrit comment créer des pages dans un site ASP.NET Web Pages (Razor) qui doit être restituée correctement sur les appareils mobiles. Ce que vous allez apprendre : Comment vous...'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: dbcd25331387f8606343e551302bc3ed1f9b2c25
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59379506"
---
# <a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Rendu des Sites ASP.NET Web Pages (Razor) pour les appareils mobiles

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article décrit comment créer des pages dans un site ASP.NET Web Pages (Razor) qui doit être restituée correctement sur les appareils mobiles.
> 
> Ce que vous allez apprendre :
> 
> - Comment utiliser une convention d’affectation de noms pour spécifier qu’une page est conçue spécifiquement pour les appareils mobiles.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.


Les Pages Web ASP.NET vous permet de créer des affichages personnalisés pour restituer le contenu sur mobile ou d’autres périphériques.

Pour créer la page spécifique à l’appareil dans un site ASP.NET Web Pages, le plus simple consiste à l’aide d’un modèle d’affectation de noms de fichier comme suit : *FileName.Mobile.cshtml*. Vous pouvez créer deux versions d’une page (par exemple, un nommé *MyFile.cshtml* et l’autre nommé *MyFile.Mobile.cshtml*). À l’exécution, quand un appareil mobile demande *MyFile.cshtml*, ASP.NET restitue le contenu à partir de *MyFile.Mobile.cshtml*. Sinon, *MyFile.cshtml* est rendu.

L’exemple suivant montre comment activer le rendu mobile en ajoutant une page de contenu pour les appareils mobiles. *Page1.cshtml* contient une barre latérale de navigation, ainsi que contenu. *Page1.Mobile.cshtml* contient le même contenu, mais omet la barre latérale.

1. Dans un site ASP.NET Web Pages, créez un fichier nommé *Page1.cshtml* et remplacez le contenu actuel par le balisage suivant.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Créez un fichier nommé *Page1.Mobile.cshtml* et remplacez le contenu existant par le balisage suivant. Notez que la version mobile de la page omet la section de navigation pour optimiser le rendu sur un écran plus petit.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Exécutez un navigateur de bureau et accédez à *Page1.cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Exécutez un navigateur mobile (ou un émulateur d’appareil mobile) et accédez à *Page1.cshtml*. (Notez que vous n’incluez pas *.mobile.* dans le cadre de l’URL.) Même si la demande est de *Page1.cshtml*, ASP.NET restitue *Page1.Mobile.cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Pour tester les pages mobiles, vous pouvez utiliser un simulateur d’appareil mobile qui s’exécute sur un ordinateur de bureau. Cet outil vous permet de tester des pages web tel qu’il apparaîtra sur les appareils mobiles (autrement dit, généralement avec un beaucoup plus petite afficher la zone). Par exemple, un simulateur est le [module complémentaire de sélecteur d’Agent utilisateur](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) pour Mozilla Firefox, qui vous permet d’émuler différents navigateurs mobiles à partir d’une version de bureau de Firefox.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires


[Émulateur de Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
