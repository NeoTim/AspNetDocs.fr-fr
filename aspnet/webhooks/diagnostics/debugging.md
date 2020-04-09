---
uid: webhooks/diagnostics/debugging
title: ASP.NET WebHooks débogage (fr) Microsoft Docs
author: rick-anderson
description: Comment déboiffer ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 2f1f8196478e7025a0467acb945d9ed36c8fd0ca
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675371"
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="50fdd-103">ASP.NET WebHooks débogage</span><span class="sxs-lookup"><span data-stu-id="50fdd-103">ASP.NET WebHooks debugging</span></span>

## <a name="debugging-in-azure"></a><span data-ttu-id="50fdd-104">Débogage à Azure</span><span class="sxs-lookup"><span data-stu-id="50fdd-104">Debugging in Azure</span></span>

<span data-ttu-id="50fdd-105">Pour déboguer votre application Web en cours d’exécution à Azure, veuillez consulter le tutoriel [Troubleshoot une application web dans Azure App Service à l’aide de Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="50fdd-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="50fdd-106">Débogage avec Source et Symboles</span><span class="sxs-lookup"><span data-stu-id="50fdd-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="50fdd-107">En plus de débogage de votre propre code, il est possible de déboguer directement dans Microsoft ASP.NET WebHooks, et en fait tout de .NET.</span><span class="sxs-lookup"><span data-stu-id="50fdd-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="50fdd-108">Cela fonctionne indépendamment du fait que vous déboillez localement ou à distance.</span><span class="sxs-lookup"><span data-stu-id="50fdd-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="50fdd-109">Tout d’abord, configurer Visual Studio pour trouver la source et les symboles en allant à **Debug,** puis **Options et Paramètres**.</span><span class="sxs-lookup"><span data-stu-id="50fdd-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="50fdd-110">Définissez les options comme celle-ci :</span><span class="sxs-lookup"><span data-stu-id="50fdd-110">Set the options like this:</span></span>

![Options et Paramètres](_static/SourceSymbols.png)

<span data-ttu-id="50fdd-112">Ensuite, ajoutez un lien vers [symbolsource.org](http://symbolsource.org) pour télécharger la source et les symboles.</span><span class="sxs-lookup"><span data-stu-id="50fdd-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="50fdd-113">Rendez-vous sur l’onglet **Symboles** du menu ci-dessus et ajoutez ce qui suit comme emplacement de symbole :</span><span class="sxs-lookup"><span data-stu-id="50fdd-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="50fdd-114">En outre, assurez-vous que le répertoire cache a un nom court; sinon les noms de fichiers peuvent devenir trop longs, ce qui fera que les symboles ne se chargent pas.</span><span class="sxs-lookup"><span data-stu-id="50fdd-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="50fdd-115">Un échantillon de chemin est :</span><span class="sxs-lookup"><span data-stu-id="50fdd-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="50fdd-116">Les paramètres doivent ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="50fdd-116">The settings should look similar to this:</span></span>

![Exemple de localisation de fichier de symbole d’options](_static/SymSource.png)
