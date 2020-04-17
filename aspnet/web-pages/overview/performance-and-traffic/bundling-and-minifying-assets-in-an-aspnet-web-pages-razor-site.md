---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Regroupement et minification des actifs dans un site de pages Web ASP.NET (Razor) Microsoft Docs
author: rick-anderson
description: Le regroupement et la minification sont des moyens de rendre votre site plus rapide. Le regroupement vous permet de combiner plusieurs fichiers JavaScript ( .js) ou plusieurs feuilles de style en cascade (...
ms.author: riande
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 2a877c1e1a06ea2357f96b37ec4ae72f9f9c9ff3
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81539913"
---
# <a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="3a4dd-104">Bundles et minimisation des ressources dans un site ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="3a4dd-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="3a4dd-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="3a4dd-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="3a4dd-106">Le regroupement et la minification sont des moyens de rendre votre site plus rapide.</span><span class="sxs-lookup"><span data-stu-id="3a4dd-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="3a4dd-107">Le regroupement vous permet de combiner plusieurs fichiers JavaScript (*.js)* ou plusieurs fichiers de style en cascade (*.css)* afin qu’ils puissent être téléchargés comme une unité, plutôt qu’une à la fois.</span><span class="sxs-lookup"><span data-stu-id="3a4dd-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="3a4dd-108">Minification presse l’espace blanc et effectue d’autres types de compression pour rendre les fichiers téléchargés aussi petits que possible.</span><span class="sxs-lookup"><span data-stu-id="3a4dd-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="3a4dd-109">La version RC de ASP.NET Pages Web 2 ne prend pas en charge le regroupement et la minification parce que le paquet qui contient les éléments requis n’est pas encore disponible dans Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="3a4dd-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="3a4dd-110">Veuillez nous excuser pour ce désagrément.</span><span class="sxs-lookup"><span data-stu-id="3a4dd-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="3a4dd-111">Le package devrait être disponible dans la version finale de ASP.NET Pages Web 2 et WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="3a4dd-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
