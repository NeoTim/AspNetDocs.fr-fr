---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Utilisation d’un site CAPTCHA pour empêcher les bots d’utiliser votre site de ASP.NET De rasoir Web) Microsoft Docs
author: rick-anderson
description: Cet article explique comment utiliser ReCaptcha (une mesure de sécurité) pour empêcher les programmes automatisés (bots) d’effectuer des tâches dans une ASP.NET Pages Web (Razor) nous ...
ms.author: riande
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 65f414ae3fed5e2fa28b1e57f5327c6411a43d55
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543754"
---
# <a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="60f35-103">Utilisation d’un site CAPTCHA pour empêcher les bots d’utiliser votre site ASP.NET Web Razor)</span><span class="sxs-lookup"><span data-stu-id="60f35-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>

<span data-ttu-id="60f35-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="60f35-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="60f35-105">Cet article explique comment utiliser ReCaptcha (une mesure de sécurité) pour empêcher les programmes automatisés (bots) d’effectuer des tâches dans un site Web ASP.NET Pages Web (Razor).</span><span class="sxs-lookup"><span data-stu-id="60f35-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="60f35-106">**Ce que vous allez apprendre :**</span><span class="sxs-lookup"><span data-stu-id="60f35-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="60f35-107">Comment ajouter un test CAPTCHA à votre site.</span><span class="sxs-lookup"><span data-stu-id="60f35-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="60f35-108">Voici les caractéristiques ASP.NET introduites dans l’article :</span><span class="sxs-lookup"><span data-stu-id="60f35-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="60f35-109">L’aide. `ReCaptcha`</span><span class="sxs-lookup"><span data-stu-id="60f35-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="60f35-110">Les informations contenues dans cet article s’appliquent aux pages Web ASP.NET 1.0 et Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="60f35-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>

## <a name="about-captchas"></a><span data-ttu-id="60f35-111">À propos de CAPTCHAs</span><span class="sxs-lookup"><span data-stu-id="60f35-111">About CAPTCHAs</span></span>

<span data-ttu-id="60f35-112">Chaque fois que vous laissez les gens s’inscrire sur votre site, ou même tout simplement entrer un nom et URL (comme pour un commentaire de blog), vous pourriez obtenir un flot de faux noms.</span><span class="sxs-lookup"><span data-stu-id="60f35-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="60f35-113">Ceux-ci sont souvent laissés par des programmes automatisés (bots) qui essaient de laisser des URL dans chaque site Web qu’ils peuvent trouver.</span><span class="sxs-lookup"><span data-stu-id="60f35-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="60f35-114">(Une motivation commune est d’afficher les URL des produits à vendre.)</span><span class="sxs-lookup"><span data-stu-id="60f35-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="60f35-115">Vous pouvez vous assurer qu’un utilisateur est une personne réelle et non un programme informatique en utilisant un *CAPTCHA* pour valider les utilisateurs lorsqu’ils s’inscrivent ou entrent leur nom et leur site.</span><span class="sxs-lookup"><span data-stu-id="60f35-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="60f35-116">CAPTCHA signifie tout à fait automatisé test de turing public pour dire ordinateurs et humains à part.</span><span class="sxs-lookup"><span data-stu-id="60f35-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="60f35-117">Un CAPTCHA est un test *de réponse* aux défis dans lequel l’utilisateur est invité à faire quelque chose qui est facile à faire pour une personne, mais difficile pour un programme automatisé à faire.</span><span class="sxs-lookup"><span data-stu-id="60f35-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="60f35-118">Le type le plus commun de CAPTCHA est celui où vous voyez quelques lettres déformées et on vous demande de les taper.</span><span class="sxs-lookup"><span data-stu-id="60f35-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="60f35-119">(La distorsion est censée rendre difficile pour les bots de déchiffrer les lettres.)</span><span class="sxs-lookup"><span data-stu-id="60f35-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="60f35-120">Ajout d’un test ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="60f35-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="60f35-121">Dans ASP.NET pages, vous pouvez `ReCaptcha` utiliser l’aide pour rendre un test CAPTCHA basé[http://recaptcha.net](http://recaptcha.net)sur le service ReCaptcha ( ).</span><span class="sxs-lookup"><span data-stu-id="60f35-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="60f35-122">L’aide `ReCaptcha` affiche une image de deux mots déformés que les utilisateurs doivent entrer correctement avant que la page ne soit validée.</span><span class="sxs-lookup"><span data-stu-id="60f35-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="60f35-123">La réponse de l’utilisateur est validée par le service ReCaptcha.Net.</span><span class="sxs-lookup"><span data-stu-id="60f35-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="60f35-124">Inscrivez votre site Web à[http://recaptcha.net](http://recaptcha.net)ReCaptcha.Net ( ).</span><span class="sxs-lookup"><span data-stu-id="60f35-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="60f35-125">Lorsque vous aurez terminé l’inscription, vous obtiendrez une clé publique et une clé privée.</span><span class="sxs-lookup"><span data-stu-id="60f35-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="60f35-126">Ajoutez la ASP.NET Bibliothèque d’aide Web à votre site Web tel que décrit dans [Installing Helpers dans un site de pages Web ASP.NET,](https://go.microsoft.com/fwlink/?LinkId=252372)si vous ne l’avez pas déjà fait.</span><span class="sxs-lookup"><span data-stu-id="60f35-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="60f35-127">Si vous n’avez pas déjà un \* \_fichier AppStart.cshtml,\* dans le dossier racine d’un site Web créer un fichier nommé \* \_AppStart.cshtml\*.</span><span class="sxs-lookup"><span data-stu-id="60f35-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="60f35-128">Ajoutez les `Recaptcha` paramètres d’assistance suivants dans le \* \_fichier AppStart.cshtml\* :</span><span class="sxs-lookup"><span data-stu-id="60f35-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="60f35-129">Réglez `PrivateKey` les propriétés et les propriétés à l’aide `PublicKey` de vos propres clés publiques et privées.</span><span class="sxs-lookup"><span data-stu-id="60f35-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="60f35-130">Enregistrez le fichier \* \_AppStart.cshtml\* et fermez-le.</span><span class="sxs-lookup"><span data-stu-id="60f35-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="60f35-131">Dans le dossier racine d’un site Web, créer une nouvelle page nommée *Recaptcha.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="60f35-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="60f35-132">Remplacer le contenu existant par les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="60f35-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="60f35-133">Exécutez la page *Recaptcha.cshtml* dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="60f35-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="60f35-134">Si `PrivateKey` la valeur est valide, la page affiche le contrôle ReCaptcha et un bouton.</span><span class="sxs-lookup"><span data-stu-id="60f35-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="60f35-135">Si vous n’aviez pas défini les clés globalement dans \* \_AppStart.html\*, la page afficherait une erreur.</span><span class="sxs-lookup"><span data-stu-id="60f35-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="60f35-136">Entrez les mots pour le test.</span><span class="sxs-lookup"><span data-stu-id="60f35-136">Enter the words for the test.</span></span> <span data-ttu-id="60f35-137">Si vous réussissez le test ReCaptcha, vous voyez un message à cet effet.</span><span class="sxs-lookup"><span data-stu-id="60f35-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="60f35-138">Sinon, vous voyez un message d’erreur et le contrôle ReCaptcha est redisjoué.</span><span class="sxs-lookup"><span data-stu-id="60f35-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="60f35-139">Si votre ordinateur se trouve sur un domaine qui `defaultproxy` utilise un serveur proxy, vous devrez peut-être configurer l’élément du fichier *Web.config.*</span><span class="sxs-lookup"><span data-stu-id="60f35-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="60f35-140">L’exemple suivant montre un fichier `defaultproxy` *Web.config* avec l’élément configuré pour permettre au service ReCaptcha de fonctionner.</span><span class="sxs-lookup"><span data-stu-id="60f35-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="60f35-141">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="60f35-141">Additional Resources</span></span>

- [<span data-ttu-id="60f35-142">Personnaliser le comportement à l’échelle du site pour ASP.NET sites de pages Web</span><span class="sxs-lookup"><span data-stu-id="60f35-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="60f35-143">Site de ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="60f35-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
