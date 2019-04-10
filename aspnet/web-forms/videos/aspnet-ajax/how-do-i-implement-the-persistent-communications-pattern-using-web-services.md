---
uid: web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
title: '[Comment faire] Implémenter le modèle de communication persistant à l’aide des Services Web ? | Microsoft Docs'
author: JoeStagner
description: Dans un site Web traditionnel le navigateur et le serveur ne conservent pas une communication en cours, mais communiquent uniquement en réponse à l’utilisateur qui effectue un acte...
ms.author: riande
ms.date: 08/22/2007
ms.assetid: 424c06cd-6d61-43cd-a1f2-d1a6b62e47b1
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
msc.type: video
ms.openlocfilehash: de2eb281cd4bab46635af480ac2e8f07f60f1591
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59408106"
---
# <a name="how-do-i-implement-the-persistent-communications-pattern-using-web-services"></a><span data-ttu-id="39ce4-104">[Comment faire] Implémenter le modèle de communication persistant à l’aide des Services Web ?</span><span class="sxs-lookup"><span data-stu-id="39ce4-104">[How Do I:] Implement the Persistent Communications Pattern using Web Services?</span></span>

<span data-ttu-id="39ce4-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="39ce4-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="39ce4-106">Dans un site Web traditionnel le navigateur et le serveur ne conservent pas une communication en cours, mais communiquent uniquement en réponse à l’utilisateur qui effectue une action.</span><span class="sxs-lookup"><span data-stu-id="39ce4-106">In a traditional Web site the browser and the server do not maintain an ongoing communication, but communicate only in response to the user performing an action.</span></span> <span data-ttu-id="39ce4-107">Dans un site Web moderne, où la page devient un conteneur d’applications, il peut être avantageux pour le navigateur et le serveur pour maintenir une communication en cours afin que les mises à jour de la page peuvent se produire sans que l’utilisateur effectue une action.</span><span class="sxs-lookup"><span data-stu-id="39ce4-107">In a modern Web site where the page becomes an application container, it can be advantageous for the browser and the server to maintain an ongoing communication so that page updates can occur without the user performing an action.</span></span> <span data-ttu-id="39ce4-108">Il s’agit en tant que le modèle de communication persistant pour AJAX.</span><span class="sxs-lookup"><span data-stu-id="39ce4-108">This is known as the Persistent Communications Pattern for AJAX.</span></span> <span data-ttu-id="39ce4-109">ASP.NET AJAX fournit deux méthodes principales pour les développeurs Web implémenter le modèle de communication persistant.</span><span class="sxs-lookup"><span data-stu-id="39ce4-109">ASP.NET AJAX provides two main ways for Web developers to implement the Persistent Communications Pattern.</span></span> <span data-ttu-id="39ce4-110">Dans une vidéo précédente, nous avons vu comment utiliser le contrôle UpdatePanel d’ASP.NET AJAX en tant que la base de l’implémentation.</span><span class="sxs-lookup"><span data-stu-id="39ce4-110">In an earlier video we saw how to use the ASP.NET AJAX UpdatePanel as the basis of the implementation.</span></span> <span data-ttu-id="39ce4-111">Dans cette vidéo, nous Apprenez à implémenter le même modèle à l’aide d’un appel de JavaScrpt à un service Web, ce qui supprime la nécessité d’un contrôle UpdatePanel d’ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="39ce4-111">In this video we learn how to implement the same pattern using a JavaScrpt call to a Web service, which removes the need for an ASP.NET AJAX UpdatePanel.</span></span>

[<span data-ttu-id="39ce4-112">&#9654;Regardez la vidéo (16 minutes)</span><span class="sxs-lookup"><span data-stu-id="39ce4-112">&#9654; Watch video (16 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-implement-the-persistent-communications-pattern-using-web-services)

> [!div class="step-by-step"]
> <span data-ttu-id="39ce4-113">[Précédent](how-do-i-localize-an-aspnet-ajax-application.md)
> [Suivant](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span><span class="sxs-lookup"><span data-stu-id="39ce4-113">[Previous](how-do-i-localize-an-aspnet-ajax-application.md)
[Next](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span></span>
