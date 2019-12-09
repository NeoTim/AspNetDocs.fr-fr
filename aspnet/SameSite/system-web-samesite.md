---
title: Utiliser des cookies SameSite dans ASP.NET
author: rick-anderson
description: Découvrez comment utiliser pour SameSite des cookies dans ASP.NET
ms.author: riande
ms.date: 12/03/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: 40e5c13b6834912c13b41cbfad7da8cd84ca6c8b
ms.sourcegitcommit: 969e7db924ebad3cc0f0cb0d65d148e8b9221b9a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74902023"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a><span data-ttu-id="9c15c-103">Utiliser des cookies SameSite dans ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9c15c-103">Work with SameSite cookies in ASP.NET</span></span>

<span data-ttu-id="9c15c-104">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9c15c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9c15c-105">SameSite est un projet [IETF](https://ietf.org/about/) conçu pour offrir une protection contre les attaques de falsification de requête intersites (CSRF).</span><span class="sxs-lookup"><span data-stu-id="9c15c-105">SameSite is an [IETF](https://ietf.org/about/) draft designed to provide some protection against cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="9c15c-106">Le [Brouillon SameSite 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00):</span><span class="sxs-lookup"><span data-stu-id="9c15c-106">The [SameSite 2019 draft](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00):</span></span>

* <span data-ttu-id="9c15c-107">Traite les cookies comme des `SameSite=Lax` par défaut.</span><span class="sxs-lookup"><span data-stu-id="9c15c-107">Treats cookies as `SameSite=Lax` by default.</span></span>
* <span data-ttu-id="9c15c-108">États les cookies qui déclarent explicitement `SameSite=None` afin d’activer la remise entre sites doivent être marqués comme `Secure`.</span><span class="sxs-lookup"><span data-stu-id="9c15c-108">States cookies that explicitly assert `SameSite=None` in order to enable cross-site delivery should be marked as `Secure`.</span></span>

<span data-ttu-id="9c15c-109">`Lax` fonctionne pour la plupart des cookies d’application.</span><span class="sxs-lookup"><span data-stu-id="9c15c-109">`Lax` works for most app cookies.</span></span> <span data-ttu-id="9c15c-110">Certaines formes d’authentification comme [OpenID Connect](https://openid.net/connect/) (OIDC) et [WS-Federation](https://auth0.com/docs/protocols/ws-fed) default pour la publication de redirections basées sur.</span><span class="sxs-lookup"><span data-stu-id="9c15c-110">Some forms of authentication like [OpenID Connect](https://openid.net/connect/) (OIDC) and [WS-Federation](https://auth0.com/docs/protocols/ws-fed) default to POST based redirects.</span></span> <span data-ttu-id="9c15c-111">Les redirections basées sur la publication déclenchent les protections du navigateur SameSite, donc SameSite est désactivé pour ces composants.</span><span class="sxs-lookup"><span data-stu-id="9c15c-111">The POST based redirects trigger the SameSite browser protections, so SameSite is disabled for these components.</span></span> <span data-ttu-id="9c15c-112">La plupart des connexions [OAuth](https://oauth.net/) ne sont pas affectées en raison de différences de flux de demande.</span><span class="sxs-lookup"><span data-stu-id="9c15c-112">Most [OAuth](https://oauth.net/) logins are not affected due to differences in how the request flows.</span></span>

<span data-ttu-id="9c15c-113">Le paramètre `None` provoque des problèmes de compatibilité avec les clients qui ont implémenté la [norme préliminaire 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) précédente (par exemple, IOS 12).</span><span class="sxs-lookup"><span data-stu-id="9c15c-113">The `None` parameter causes compatibility problems with clients that implemented the prior [2016 draft standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07) (for example, iOS 12).</span></span> <span data-ttu-id="9c15c-114">Consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="9c15c-114">See [Supporting older browsers](#sob) in this document.</span></span>

<span data-ttu-id="9c15c-115">Chaque composant ASP.NET Core qui émet des cookies doit décider si SameSite est approprié.</span><span class="sxs-lookup"><span data-stu-id="9c15c-115">Each ASP.NET Core component that emits cookies needs to decide if SameSite is appropriate.</span></span>

## <a name="api-usage-with-samesite"></a><span data-ttu-id="9c15c-116">Utilisation des API avec SameSite</span><span class="sxs-lookup"><span data-stu-id="9c15c-116">API usage with SameSite</span></span>

<span data-ttu-id="9c15c-117">Consultez [HttpCookie. SameSite, propriété](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite)</span><span class="sxs-lookup"><span data-stu-id="9c15c-117">See [HttpCookie.SameSite Property](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite)</span></span>

## <a name="history-and-changes"></a><span data-ttu-id="9c15c-118">Historique et modifications</span><span class="sxs-lookup"><span data-stu-id="9c15c-118">History and changes</span></span>

<span data-ttu-id="9c15c-119">La prise en charge de SameSite a été implémentée pour la première fois dans .NET 4.7.2 à l’aide de la [norme draft 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).</span><span class="sxs-lookup"><span data-stu-id="9c15c-119">SameSite support was first implemented in .NET 4.7.2 using the [2016 draft standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).</span></span>

<span data-ttu-id="9c15c-120">Le 19 novembre 2019 mises à jour pour Windows a mis à jour .NET 4.7.2 + de la norme 2016 à la norme 2019.</span><span class="sxs-lookup"><span data-stu-id="9c15c-120">The November 19, 2019 updates for Windows updated .NET 4.7.2+ from the 2016 standard to the 2019 standard.</span></span> <span data-ttu-id="9c15c-121">Des mises à jour supplémentaires sont à venir pour d’autres versions de Windows.</span><span class="sxs-lookup"><span data-stu-id="9c15c-121">Additional updates are forthcoming for other versions of Windows.</span></span> <span data-ttu-id="9c15c-122">Pour plus d’informations, consultez les Articles de la base de connaissances suivants :</span><span class="sxs-lookup"><span data-stu-id="9c15c-122">See the following KB's for more information:</span></span>

* [<span data-ttu-id="9c15c-123">Article 4531182 de la base de connaissances</span><span class="sxs-lookup"><span data-stu-id="9c15c-123">KB article 4531182</span></span>](https://support.microsoft.com/help/4531182/kb4531182)
* [<span data-ttu-id="9c15c-124">Article 4524421 de la base de connaissances</span><span class="sxs-lookup"><span data-stu-id="9c15c-124">KB article 4524421</span></span>](https://support.microsoft.com/help/4524421/kb4524421)

 <span data-ttu-id="9c15c-125">Le brouillon 2019 de la spécification SameSite :</span><span class="sxs-lookup"><span data-stu-id="9c15c-125">The 2019 draft of the SameSite specification:</span></span>

* <span data-ttu-id="9c15c-126">N’est **pas** à compatibilité descendante avec le brouillon 2016.</span><span class="sxs-lookup"><span data-stu-id="9c15c-126">Is **not** backwards compatible with the 2016 draft.</span></span> <span data-ttu-id="9c15c-127">Pour plus d’informations, consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="9c15c-127">For more information, see [Supporting older browsers](#sob) in this document.</span></span>
* <span data-ttu-id="9c15c-128">Spécifie que les cookies sont traités comme `SameSite=Lax` par défaut.</span><span class="sxs-lookup"><span data-stu-id="9c15c-128">Specifies cookies are treated as `SameSite=Lax` by default.</span></span>
* <span data-ttu-id="9c15c-129">Spécifie les cookies qui déclarent explicitement `SameSite=None` afin d’activer la remise entre sites qui doit être marquée comme `Secure`.</span><span class="sxs-lookup"><span data-stu-id="9c15c-129">Specifies cookies that explicitly assert `SameSite=None` in order to enable cross-site delivery should be marked as `Secure`.</span></span> <span data-ttu-id="9c15c-130">`None` est une nouvelle entrée à refuser.</span><span class="sxs-lookup"><span data-stu-id="9c15c-130">`None` is a new entry to opt out.</span></span>
* <span data-ttu-id="9c15c-131">Est pris en charge par les correctifs émis comme indiqué dans les Articles de la base de connaissances répertoriés ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="9c15c-131">Is supported by patches issued as described in the KB's listed above.</span></span>
* <span data-ttu-id="9c15c-132">Est planifié pour être activé par [chrome](https://chromestatus.com/feature/5088147346030592) par défaut au [2020 février](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html).</span><span class="sxs-lookup"><span data-stu-id="9c15c-132">Is scheduled to be enabled by [Chrome](https://chromestatus.com/feature/5088147346030592) by default in [Feb 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html).</span></span> <span data-ttu-id="9c15c-133">Les navigateurs ont commencé à passer à cette norme dans 2019.</span><span class="sxs-lookup"><span data-stu-id="9c15c-133">Browsers started moving to this standard in 2019.</span></span>

<a name="sob"></a>

## <a name="supporting-older-browsers"></a><span data-ttu-id="9c15c-134">Prise en charge des navigateurs plus anciens</span><span class="sxs-lookup"><span data-stu-id="9c15c-134">Supporting older browsers</span></span>

<span data-ttu-id="9c15c-135">La norme 2016 SameSite stipule que les valeurs inconnues doivent être traitées comme des valeurs `SameSite=Strict`.</span><span class="sxs-lookup"><span data-stu-id="9c15c-135">The 2016 SameSite standard mandated that unknown values must be treated as `SameSite=Strict` values.</span></span> <span data-ttu-id="9c15c-136">Les applications accessibles depuis des navigateurs plus anciens qui prennent en charge la norme 2016 SameSite peuvent s’arrêter lorsqu’ils obtiennent une propriété SameSite avec la valeur `None`.</span><span class="sxs-lookup"><span data-stu-id="9c15c-136">Apps accessed from older browsers which support the 2016 SameSite standard may break when they get a SameSite property with a value of `None`.</span></span> <span data-ttu-id="9c15c-137">Les applications Web doivent implémenter la détection du navigateur si elles envisagent de prendre en charge des navigateurs plus anciens.</span><span class="sxs-lookup"><span data-stu-id="9c15c-137">Web apps must implement browser detection if they intend to support older browsers.</span></span> <span data-ttu-id="9c15c-138">ASP.NET n’implémente pas la détection du navigateur, car les valeurs des agents utilisateur sont très volatiles et changent fréquemment.</span><span class="sxs-lookup"><span data-stu-id="9c15c-138">ASP.NET doesn't implement browser detection because User-Agents values are highly volatile and change frequently.</span></span> <span data-ttu-id="9c15c-139">Le code suivant peut être appelé au <xref:HTTP.HttpCookie> site d’appel :</span><span class="sxs-lookup"><span data-stu-id="9c15c-139">The following code can be called at the <xref:HTTP.HttpCookie> call site:</span></span>

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet)]

<span data-ttu-id="9c15c-140">Dans l’exemple précédent, `MyUserAgentDetectionLib.DisallowsSameSiteNone` est une bibliothèque fournie par l’utilisateur, qui détecte si l’agent utilisateur ne prend pas en charge SameSite `None`.</span><span class="sxs-lookup"><span data-stu-id="9c15c-140">In the preceding sample, `MyUserAgentDetectionLib.DisallowsSameSiteNone` is a user supplied library that detects if the user agent doesn't support SameSite `None`.</span></span> <span data-ttu-id="9c15c-141">Le code suivant illustre un exemple de méthode `DisallowsSameSiteNone` :</span><span class="sxs-lookup"><span data-stu-id="9c15c-141">The following code shows a sample `DisallowsSameSiteNone` method:</span></span>

> [!WARNING]
> <span data-ttu-id="9c15c-142">Le code suivant est uniquement à des fins de démonstration :</span><span class="sxs-lookup"><span data-stu-id="9c15c-142">The following code is for demonstration only:</span></span>
> * <span data-ttu-id="9c15c-143">Elle ne doit pas être considérée comme terminée.</span><span class="sxs-lookup"><span data-stu-id="9c15c-143">It should not be considered complete.</span></span>
> * <span data-ttu-id="9c15c-144">Elle n’est pas gérée ni prise en charge.</span><span class="sxs-lookup"><span data-stu-id="9c15c-144">It is not maintained or supported.</span></span>

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet2)]

## <a name="test-apps-for-samesite-problems"></a><span data-ttu-id="9c15c-145">Tester des applications pour les problèmes SameSite</span><span class="sxs-lookup"><span data-stu-id="9c15c-145">Test apps for SameSite problems</span></span>

<span data-ttu-id="9c15c-146">Les applications qui interagissent avec des sites distants, par exemple via une connexion tierce, doivent :</span><span class="sxs-lookup"><span data-stu-id="9c15c-146">Apps that interact with remote sites such as through third-party login need to:</span></span>

* <span data-ttu-id="9c15c-147">Testez l’interaction sur plusieurs navigateurs.</span><span class="sxs-lookup"><span data-stu-id="9c15c-147">Test the interaction on multiple browsers.</span></span>
* <span data-ttu-id="9c15c-148">Appliquez la [détection et l’atténuation du navigateur](#sob) abordées dans ce document.</span><span class="sxs-lookup"><span data-stu-id="9c15c-148">Apply the [browser detection and mitigation](#sob) discussed in this document.</span></span>

<span data-ttu-id="9c15c-149">Testez les applications Web à l’aide d’une version du client qui peut s’abonner au nouveau comportement SameSite.</span><span class="sxs-lookup"><span data-stu-id="9c15c-149">Test web apps using a client version that can opt-in to the new SameSite behavior.</span></span> <span data-ttu-id="9c15c-150">Chrome, Firefox et chrome Edge ont tous des indicateurs de fonctionnalités d’abonnement qui peuvent être utilisés à des fins de test.</span><span class="sxs-lookup"><span data-stu-id="9c15c-150">Chrome, Firefox, and Chromium Edge all have new opt-in feature flags that can be used for testing.</span></span> <span data-ttu-id="9c15c-151">Une fois que votre application applique les correctifs SameSite, testez-la avec des versions client plus anciennes, en particulier Safari.</span><span class="sxs-lookup"><span data-stu-id="9c15c-151">After your app applies the SameSite patches, test it with older client versions, especially Safari.</span></span> <span data-ttu-id="9c15c-152">Pour plus d’informations, consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="9c15c-152">For more information, see [Supporting older browsers](#sob) in this document.</span></span>

### <a name="test-with-chrome"></a><span data-ttu-id="9c15c-153">Tester avec chrome</span><span class="sxs-lookup"><span data-stu-id="9c15c-153">Test with Chrome</span></span>

<span data-ttu-id="9c15c-154">Chrome 78 + donne des résultats trompeurs, car une atténuation temporaire est en place.</span><span class="sxs-lookup"><span data-stu-id="9c15c-154">Chrome 78+ gives misleading results because it has a temporary mitigation in place.</span></span> <span data-ttu-id="9c15c-155">Le chrome 78 + atténuation temporaire autorise les cookies datant de moins de deux minutes.</span><span class="sxs-lookup"><span data-stu-id="9c15c-155">The Chrome 78+ temporary mitigation allows cookies less than two minutes old.</span></span> <span data-ttu-id="9c15c-156">Le chrome 76 ou 77 avec les indicateurs de test appropriés activés fournit des résultats plus précis.</span><span class="sxs-lookup"><span data-stu-id="9c15c-156">Chrome 76 or 77 with the appropriate test flags enabled provides more accurate results.</span></span> <span data-ttu-id="9c15c-157">Pour tester le nouveau comportement SameSite, basculez `chrome://flags/#same-site-by-default-cookies` sur **activé**.</span><span class="sxs-lookup"><span data-stu-id="9c15c-157">To test the new SameSite behavior toggle `chrome://flags/#same-site-by-default-cookies` to **Enabled**.</span></span> <span data-ttu-id="9c15c-158">Les anciennes versions de chrome (75 et versions antérieures) sont signalées pour échouer avec le nouveau paramètre de `None`.</span><span class="sxs-lookup"><span data-stu-id="9c15c-158">Older versions of Chrome (75 and below) are reported to fail with the new `None` setting.</span></span> <span data-ttu-id="9c15c-159">Consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="9c15c-159">See [Supporting older browsers](#sob) in this document.</span></span>

<span data-ttu-id="9c15c-160">Google ne rend pas les versions de chrome plus anciennes disponibles.</span><span class="sxs-lookup"><span data-stu-id="9c15c-160">Google does not make older chrome versions available.</span></span> <span data-ttu-id="9c15c-161">Suivez les instructions de [Télécharger chrome](https://www.chromium.org/getting-involved/download-chromium) pour tester les anciennes versions de chrome.</span><span class="sxs-lookup"><span data-stu-id="9c15c-161">Follow the instructions at [Download Chromium](https://www.chromium.org/getting-involved/download-chromium) to test older versions of Chrome.</span></span> <span data-ttu-id="9c15c-162">Ne téléchargez **pas** chrome à partir des liens fournis en recherchant les anciennes versions de chrome.</span><span class="sxs-lookup"><span data-stu-id="9c15c-162">Do **not** download Chrome from links provided by searching for older versions of chrome.</span></span>

* [<span data-ttu-id="9c15c-163">Chrome 76 Win64</span><span class="sxs-lookup"><span data-stu-id="9c15c-163">Chromium 76 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [<span data-ttu-id="9c15c-164">Chrome 74 Win64</span><span class="sxs-lookup"><span data-stu-id="9c15c-164">Chromium 74 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

### <a name="test-with-safari"></a><span data-ttu-id="9c15c-165">Tester avec Safari</span><span class="sxs-lookup"><span data-stu-id="9c15c-165">Test with Safari</span></span>

<span data-ttu-id="9c15c-166">Safari 12 implémentait strictement le brouillon précédent et échoue lorsque la nouvelle valeur de `None` se trouve dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="9c15c-166">Safari 12 strictly implemented the prior draft and fails when the new `None` value is in a cookie.</span></span> <span data-ttu-id="9c15c-167">`None` est évité par le biais du code de détection du navigateur qui [prend en charge les navigateurs plus anciens](#sob) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="9c15c-167">`None` is avoided via the browser detection code [Supporting older browsers](#sob) in this document.</span></span> <span data-ttu-id="9c15c-168">Testez les connexions de style du système d’exploitation Safari 12, Safari 13 et WebKit à l’aide de MSAL, ADAL ou toute bibliothèque que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="9c15c-168">Test Safari 12, Safari 13, and WebKit based OS style logins using MSAL, ADAL or whatever library you are using.</span></span> <span data-ttu-id="9c15c-169">Le problème dépend de la version du système d’exploitation sous-jacent.</span><span class="sxs-lookup"><span data-stu-id="9c15c-169">The problem is dependent on the underlying OS version.</span></span> <span data-ttu-id="9c15c-170">OSX Mojave (10,14) et iOS 12 sont connus pour avoir des problèmes de compatibilité avec le nouveau comportement de SameSite.</span><span class="sxs-lookup"><span data-stu-id="9c15c-170">OSX Mojave (10.14) and iOS 12 are known to have compatibility problems with the new SameSite behavior.</span></span> <span data-ttu-id="9c15c-171">La mise à niveau du système d’exploitation vers OSX Catalina (10,15) ou iOS 13 résout le problème.</span><span class="sxs-lookup"><span data-stu-id="9c15c-171">Upgrading the OS to OSX Catalina (10.15) or iOS 13 fixes the problem.</span></span> <span data-ttu-id="9c15c-172">Safari ne dispose pas actuellement d’un indicateur d’abonnement pour tester le nouveau comportement des spécifications.</span><span class="sxs-lookup"><span data-stu-id="9c15c-172">Safari does not currently have an opt-in flag for testing the new spec behavior.</span></span>

### <a name="test-with-firefox"></a><span data-ttu-id="9c15c-173">Test avec Firefox</span><span class="sxs-lookup"><span data-stu-id="9c15c-173">Test with Firefox</span></span>

<span data-ttu-id="9c15c-174">La prise en charge de Firefox pour la nouvelle norme peut être testée sur la version 68 + en choisissant dans la page `about:config` avec l’indicateur de fonctionnalité `network.cookie.sameSite.laxByDefault`.</span><span class="sxs-lookup"><span data-stu-id="9c15c-174">Firefox support for the new standard can be tested on version 68+ by opting in on the `about:config` page with the feature flag `network.cookie.sameSite.laxByDefault`.</span></span> <span data-ttu-id="9c15c-175">Il n’y a eu aucun rapport sur les problèmes de compatibilité avec les versions antérieures de Firefox.</span><span class="sxs-lookup"><span data-stu-id="9c15c-175">There haven't been reports of compatibility issues with older versions of Firefox.</span></span>

### <a name="test-with-edge-browser"></a><span data-ttu-id="9c15c-176">Tester avec le navigateur Edge</span><span class="sxs-lookup"><span data-stu-id="9c15c-176">Test with Edge browser</span></span>

<span data-ttu-id="9c15c-177">Edge prend en charge l’ancien standard SameSite.</span><span class="sxs-lookup"><span data-stu-id="9c15c-177">Edge supports the old SameSite standard.</span></span> <span data-ttu-id="9c15c-178">Edge version 44 ne présente aucun problème de compatibilité connu avec la nouvelle norme.</span><span class="sxs-lookup"><span data-stu-id="9c15c-178">Edge version 44 doesn't have any known compatibility problems with the new standard.</span></span>

### <a name="test-with-edge-chromium"></a><span data-ttu-id="9c15c-179">Test avec Edge (chrome)</span><span class="sxs-lookup"><span data-stu-id="9c15c-179">Test with Edge (Chromium)</span></span>

<span data-ttu-id="9c15c-180">Les indicateurs SameSite sont définis sur la page `edge://flags/#same-site-by-default-cookies`.</span><span class="sxs-lookup"><span data-stu-id="9c15c-180">SameSite flags are set on the `edge://flags/#same-site-by-default-cookies` page.</span></span> <span data-ttu-id="9c15c-181">Aucun problème de compatibilité n’a été découvert avec le chrome Edge.</span><span class="sxs-lookup"><span data-stu-id="9c15c-181">No compatibility issues were discovered with Edge Chromium.</span></span>

### <a name="test-with-electron"></a><span data-ttu-id="9c15c-182">Test avec électron</span><span class="sxs-lookup"><span data-stu-id="9c15c-182">Test with Electron</span></span>

<span data-ttu-id="9c15c-183">Les versions d’électrons incluent des versions plus anciennes de chrome.</span><span class="sxs-lookup"><span data-stu-id="9c15c-183">Versions of Electron include older versions of Chromium.</span></span> <span data-ttu-id="9c15c-184">Par exemple, la version de l’électron utilisée par les équipes est le chrome 66, qui présente le comportement plus ancien.</span><span class="sxs-lookup"><span data-stu-id="9c15c-184">For example, the version of Electron used by Teams is Chromium 66, which exhibits the older behavior.</span></span> <span data-ttu-id="9c15c-185">Vous devez effectuer vos propres tests de compatibilité avec la version d’électron que votre produit utilise.</span><span class="sxs-lookup"><span data-stu-id="9c15c-185">You must perform your own compatibility testing with the version of Electron your product uses.</span></span> <span data-ttu-id="9c15c-186">Consultez [prise en charge des navigateurs plus anciens](#sob) dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="9c15c-186">See [Supporting older browsers](#sob) in the following section.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c15c-187">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9c15c-187">Additional resources</span></span>

* [<span data-ttu-id="9c15c-188">Blog du chrome : développeurs : Préparez-vous à la nouvelle SameSite = None ; Sécuriser les paramètres de cookie</span><span class="sxs-lookup"><span data-stu-id="9c15c-188">Chromium Blog:Developers: Get Ready for New SameSite=None; Secure Cookie Settings</span></span>](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [<span data-ttu-id="9c15c-189">Explication des cookies SameSite</span><span class="sxs-lookup"><span data-stu-id="9c15c-189">SameSite cookies explained</span></span>](https://web.dev/samesite-cookies-explained/)