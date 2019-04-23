---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Helper Twitter avec ASP.NET Web Pages | Microsoft Docs
author: Rick-Anderson
description: Cette rubrique et l’application montrent comment ajouter une application auxiliaire Twitter à votre projet WebMatrix 3. Il contient le code de l’application auxiliaire Twitter et montre comment appeler l’assistance...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 5dda267b146f11355dd94181ef2926e4a304a3ad
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59399006"
---
# <a name="twitter-helper-with-aspnet-web-pages"></a>Helper Twitter avec ASP.NET Web Pages

par [Tom FitzMacken](https://github.com/tfitzmac)

> [!IMPORTANT]
> Programmes d’assistance Twitter sont obsolètes. Pour les outils de Twitter dernière engagement pour les sites Web, consultez [Twitter pour une vue d’ensemble de sites Web](https://developer.twitter.com/en/docs/twitter-for-websites/overview).

> Cette rubrique et l’application montrent comment ajouter une application auxiliaire Twitter à votre projet WebMatrix 3. Il contient le code de l’application auxiliaire Twitter et montre comment appeler les méthodes d’assistance.
> 
> Ce code pour le fichier Twitter.cshtml a été développé par **Tian panoramique** de Microsoft.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.


## <a name="introduction"></a>Introduction

Cette rubrique montre comment ajouter une application auxiliaire Twitter à votre application et utiliser la syntaxe Razor pour appeler les méthodes d’assistance. L’application auxiliaire Twitter vous permet de facilement incorporer des widgets dans votre application et les boutons de Twitter. Pour utiliser un widget de Twitter, telles que la chronologie d’un utilisateur ou les résultats de recherche pour un mot-dièse, vous devez d’abord créer le [widget sur Twitter](https://twitter.com/settings/widgets). Après avoir créé votre widget, vous recevrez un id de widget. Vous passez cet id de widget en tant que paramètre lorsque vous appelez les méthodes d’assistance qui montrent le widget.

Cette rubrique a été écrit pour la version 1.1 de l’API Twitter. En ajoutant directement le code de l’application auxiliaire Twitter à votre projet, vous pouvez mettre à jour le code d’assistance si l’API Twitter change.

Pour plus d’informations sur l’installation de WebMatrix, consultez [Introducing ASP.NET Web Pages 2 - mise en route](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Ajouter l’application auxiliaire Twitter à votre projet

Pour ajouter l’application auxiliaire Twitter, tout d’abord, ajoutez un dossier nommé **application\_Code** à votre projet. Ensuite, créez un fichier nommé **Twitter.cshtml**.

![Dossier App_Code](twitter-helper/_static/image1.png)

Remplacez le code par défaut dans Twitter.cshtml par le code suivant.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Appelez les méthodes de Twitter à partir de vos pages web

L’exemple suivant montre comment utiliser les méthodes d’assistance de Twitter à partir d’une page dans votre projet. Dans votre projet, vous devez remplacer les valeurs des paramètres avec des valeurs qui correspondent à vos besoins. Vous pouvez utiliser les ID de widget fourni pour Explorer le fonctionnement des méthodes, mais vous pouvez générer vos propres widgets pour votre projet.

Tous les paramètres indiqués ci-dessous sont requis. Les paramètres facultatifs sont utilisés pour personnaliser la façon dont le bouton ou un widget s’affiche. Par exemple, le bouton suivre requiert uniquement le nom d’utilisateur à suivre, mais l’exemple montre comment inclure le nombre d’abonnés et comment spécifier la taille du bouton et la langue.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Afficher les résultats

Le code ci-dessus génère les boutons et les widgets suivants. Ces boutons et les widgets sont entièrement fonctionnelles, pas des captures d’écran. Le bouton suivre est affiché en espagnol, car le paramètre de langue a été défini sur **es**.

### <a name="follow-button"></a>Suivez le bouton

[Suivez @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="tweet-button"></a>Bouton de tweet

[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="user-timeline-profile"></a>Chronologie de l’utilisateur (profil)

[Tweets par @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="favorites"></a>Favoris

[Favoris Tweets par @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="list"></a>Liste

[À partir des tweets @Microsoft/MS \_consommateur\_bandes](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="search"></a>Rechercher

[Un Tweet sur &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`
