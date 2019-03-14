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
ms.openlocfilehash: dd06a54d396bd85eeef7c004ee115828d541a2c1
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57031106"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="5ca23-104">Rendu des Sites ASP.NET Web Pages (Razor) pour les appareils mobiles</span><span class="sxs-lookup"><span data-stu-id="5ca23-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="5ca23-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5ca23-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5ca23-106">Cet article décrit comment créer des pages dans un site ASP.NET Web Pages (Razor) qui doit être restituée correctement sur les appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="5ca23-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="5ca23-107">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="5ca23-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="5ca23-108">Comment utiliser une convention d’affectation de noms pour spécifier qu’une page est conçue spécifiquement pour les appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="5ca23-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5ca23-109">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="5ca23-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="5ca23-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="5ca23-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="5ca23-111">Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="5ca23-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="5ca23-112">Les Pages Web ASP.NET vous permet de créer des affichages personnalisés pour restituer le contenu sur mobile ou d’autres périphériques.</span><span class="sxs-lookup"><span data-stu-id="5ca23-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="5ca23-113">Pour créer la page spécifique à l’appareil dans un site ASP.NET Web Pages, le plus simple consiste à l’aide d’un modèle d’affectation de noms de fichier comme suit : <em>FileName.</em><em>Mobile</em><em>.cshtml</em>.</span><span class="sxs-lookup"><span data-stu-id="5ca23-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: <em>FileName.</em><em>Mobile</em><em>.cshtml</em>.</span></span> <span data-ttu-id="5ca23-114">Vous pouvez créer deux versions d’une page (par exemple, un nommé <em>MyFile.cshtml</em> et l’autre nommé <em>MyFile.Mobile.cshtml</em>).</span><span class="sxs-lookup"><span data-stu-id="5ca23-114">You can create two versions of a page (for example, one named <em>MyFile.cshtml</em> and one named <em>MyFile.Mobile.cshtml</em>).</span></span> <span data-ttu-id="5ca23-115">À l’exécution, quand un appareil mobile demande <em>MyFile.cshtml</em>, ASP.NET restitue le contenu à partir de <em>MyFile.Mobile.cshtml</em>.</span><span class="sxs-lookup"><span data-stu-id="5ca23-115">At run time, when a mobile device requests <em>MyFile.cshtml</em>, ASP.NET renders the content from <em>MyFile.Mobile.cshtml</em>.</span></span> <span data-ttu-id="5ca23-116">Sinon, <em>MyFile.cshtml</em> est rendu.</span><span class="sxs-lookup"><span data-stu-id="5ca23-116">Otherwise, <em>MyFile.cshtml</em> is rendered.</span></span>

<span data-ttu-id="5ca23-117">L’exemple suivant montre comment activer le rendu mobile en ajoutant une page de contenu pour les appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="5ca23-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="5ca23-118">*Page1.cshtml* contient une barre latérale de navigation, ainsi que contenu.</span><span class="sxs-lookup"><span data-stu-id="5ca23-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="5ca23-119">*Page1.Mobile.cshtml* contient le même contenu, mais omet la barre latérale.</span><span class="sxs-lookup"><span data-stu-id="5ca23-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="5ca23-120">Dans un site ASP.NET Web Pages, créez un fichier nommé *Page1.cshtml* et remplacez le contenu actuel par le balisage suivant.</span><span class="sxs-lookup"><span data-stu-id="5ca23-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="5ca23-121">Créez un fichier nommé *Page1.Mobile.cshtml* et remplacez le contenu existant par le balisage suivant.</span><span class="sxs-lookup"><span data-stu-id="5ca23-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="5ca23-122">Notez que la version mobile de la page omet la section de navigation pour optimiser le rendu sur un écran plus petit.</span><span class="sxs-lookup"><span data-stu-id="5ca23-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="5ca23-123">Exécutez un navigateur de bureau et accédez à *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5ca23-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="5ca23-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5ca23-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="5ca23-125">Exécutez un navigateur mobile (ou un émulateur d’appareil mobile) et accédez à *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5ca23-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="5ca23-126">(Notez que vous n’incluez pas *.mobile.*</span><span class="sxs-lookup"><span data-stu-id="5ca23-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="5ca23-127">dans le cadre de l’URL.) Même si la demande est de *Page1.cshtml*, ASP.NET restitue *Page1.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5ca23-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="5ca23-129">Pour tester les pages mobiles, vous pouvez utiliser un simulateur d’appareil mobile qui s’exécute sur un ordinateur de bureau.</span><span class="sxs-lookup"><span data-stu-id="5ca23-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="5ca23-130">Cet outil vous permet de tester des pages web tel qu’il apparaîtra sur les appareils mobiles (autrement dit, généralement avec un beaucoup plus petite afficher la zone).</span><span class="sxs-lookup"><span data-stu-id="5ca23-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="5ca23-131">Par exemple, un simulateur est le [module complémentaire de sélecteur d’Agent utilisateur](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) pour Mozilla Firefox, qui vous permet d’émuler différents navigateurs mobiles à partir d’une version de bureau de Firefox.</span><span class="sxs-lookup"><span data-stu-id="5ca23-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="5ca23-132">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="5ca23-132">Additional Resources</span></span>


<span data-ttu-id="5ca23-133">[Émulateur de Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="5ca23-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>