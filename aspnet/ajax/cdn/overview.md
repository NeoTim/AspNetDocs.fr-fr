---
uid: ajax/cdn/overview
title: Réseau de distribution de contenu Microsoft Ajax | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2017
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 9eebe0e52af2a0fca967a51afb58c7db174d9fdb
ms.sourcegitcommit: feb88edfb01b32f6fc9488f0f0ddb3c5b34e6ff0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88702918"
---
# <a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="88637-102">Réseau de distribution de contenu Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="88637-102">Microsoft Ajax Content Delivery Network</span></span>

> [!WARNING]
> <span data-ttu-id="88637-103">Les applications de production ne doivent pas prendre de dépendance matérielle sur les ressources CDN.</span><span class="sxs-lookup"><span data-stu-id="88637-103">Production applications should not take a hard dependency on CDN assets.</span></span> <span data-ttu-id="88637-104">Les applications doivent tester la ressource CDN référencée et utiliser une ressource de secours lorsque le CDN n’est pas disponible.</span><span class="sxs-lookup"><span data-stu-id="88637-104">Applications should test for the CDN asset referenced, and use a fallback asset when the CDN is not available.</span></span>
>
> <span data-ttu-id="88637-105">Le CDN Microsoft Ajax n’a pas de contrat SLA au-dessus et au-delà de l’utilisation d’un Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="88637-105">The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>
>
> <span data-ttu-id="88637-106">Utilisez [ce problème GitHub](https://github.com/dotnet/AspNetDocs/issues/116) pour signaler des problèmes avec le CDN Microsoft Ajax.</span><span class="sxs-lookup"><span data-stu-id="88637-106">Use [this GitHub issue](https://github.com/dotnet/AspNetDocs/issues/116) to report problems with the Microsoft Ajax CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="88637-107">Sommaire</span><span class="sxs-lookup"><span data-stu-id="88637-107">Table of Contents</span></span>

<span data-ttu-id="88637-108">**[ajax.microsoft.com renommée en ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="88637-108">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="88637-109">**[Prise en charge de Visual Studio. vsdoc](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="88637-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="88637-110">**[Utilisation d’ASP.NET AJAX à partir du CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="88637-110">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="88637-111">**[Utilisation de jQuery à partir du CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="88637-111">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="88637-112">**[Utilisation de jQuery UI à partir du CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="88637-112">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="88637-113">**[Fichiers tiers sur le CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="88637-113">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="88637-114">Versions jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-114">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="88637-115">Versions de jQuery Migrate sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-115">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="88637-116">Versions de l’interface utilisateur jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-116">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="88637-117">Versions de validation jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-117">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="88637-118">Versions de jQuery mobile sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-118">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="88637-119">Versions de modèles jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-119">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="88637-120">Versions de cycle jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-120">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="88637-121">les versions jQuery DataTables sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-121">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="88637-122">Versions de Modernizr sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-122">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="88637-123">Versions de JSHint sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-123">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="88637-124">Versions de Knockout sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-124">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="88637-125">Globaliser les mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-125">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="88637-126">Répondre aux mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-126">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="88637-127">Versions d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-127">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="88637-128">Release TouchCarousel releases sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-128">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="88637-129"> Versions deHammer.js sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-129">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="88637-130">ASP.NET Web Forms et les versions Ajax sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-130">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="88637-131">Versions de ASP.NET MVC sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-131">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="88637-132">Mises en production de Signalr ASP.NET sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-132">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="88637-133">Le réseau de distribution de contenu (CDN) Microsoft Ajax héberge des bibliothèques JavaScript tierces populaires, telles que jQuery, et vous permet de les ajouter facilement à vos applications Web.</span><span class="sxs-lookup"><span data-stu-id="88637-133">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="88637-134">Par exemple, vous pouvez commencer à utiliser jQuery qui est hébergé sur ce CDN simplement en ajoutant &lt; une &gt; balise de script à votre page qui pointe vers Ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="88637-134">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="88637-135">En tirant parti du CDN, vous pouvez améliorer considérablement les performances de vos applications Ajax.</span><span class="sxs-lookup"><span data-stu-id="88637-135">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="88637-136">Le contenu du CDN est mis en cache sur les serveurs situés dans le monde entier.</span><span class="sxs-lookup"><span data-stu-id="88637-136">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="88637-137">En outre, le CDN permet aux navigateurs de réutiliser les fichiers JavaScript tiers mis en cache pour les sites Web qui se trouvent dans des domaines différents.</span><span class="sxs-lookup"><span data-stu-id="88637-137">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="88637-138">Le CDN prend en charge le protocole SSL (HTTPs) au cas où vous devriez traiter une page Web à l’aide de l’protocole SSL.</span><span class="sxs-lookup"><span data-stu-id="88637-138">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="88637-139">Le CDN héberge les bibliothèques de scripts tierces suivantes qui ont été téléchargées et qui vous sont concédées sous licence par les propriétaires de ces bibliothèques :</span><span class="sxs-lookup"><span data-stu-id="88637-139">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="88637-140">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="88637-140">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="88637-141">interface utilisateur jQuery (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="88637-141">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="88637-142">jQuery mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="88637-142">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="88637-143">jQuery validation (https://jqueryvalidation.org/)</span><span class="sxs-lookup"><span data-stu-id="88637-143">jQuery Validation (https://jqueryvalidation.org/)</span></span>
- <span data-ttu-id="88637-144">Cycle jQuery (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="88637-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="88637-145">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="88637-145">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="88637-146">Le CDN Microsoft Ajax comprend également les bibliothèques suivantes, qui ont été chargées par Microsoft :</span><span class="sxs-lookup"><span data-stu-id="88637-146">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="88637-147">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="88637-147">ASP.NET Ajax</span></span>
- <span data-ttu-id="88637-148">ASP.NET les fichiers JavaScript MVC</span><span class="sxs-lookup"><span data-stu-id="88637-148">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="88637-149">Fichiers JavaScript Signalr ASP.NET</span><span class="sxs-lookup"><span data-stu-id="88637-149">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="88637-150">Microsoft ne réclame pas la propriété de toutes les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="88637-150">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="88637-151">Les propriétaires des droits d’auteur des bibliothèques sont les licences de ces bibliothèques.</span><span class="sxs-lookup"><span data-stu-id="88637-151">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="88637-152">Les droits dont vous pouvez avoir besoin pour télécharger et utiliser ces bibliothèques sont octroyés uniquement par les propriétaires des droits d’auteur.</span><span class="sxs-lookup"><span data-stu-id="88637-152">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="88637-153">Étant donné qu’il ne s’agit pas de bibliothèques Microsoft, Microsoft ne fournit aucune garantie ni aucune licence de droits de propriété intellectuelle (y compris aucun droit de brevet implicite) pour les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="88637-153">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="88637-154">Si vous souhaitez envoyer votre bibliothèque JavaScript et que votre bibliothèque est l’une des principales bibliothèques JavaScript (répertoriées sur les http://trends.builtwith.com) extensions ou plug-ins de ces bibliothèques (a) populaires ; ou (b) utiles pour une utilisation sur ASP.net, contactez AjaxCDNSubmission@Microsoft.com .</span><span class="sxs-lookup"><span data-stu-id="88637-154">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="88637-155">ajax.microsoft.com renommée en ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="88637-155">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="88637-156">Le CDN utilisé pour utiliser le nom de domaine microsoft.com et a été modifié pour utiliser le nom de domaine aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="88637-156">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="88637-157">Cette modification a été apportée pour améliorer les performances, car lorsqu’un navigateur référençait le domaine microsoft.com, il enverra tous les cookies de ce domaine sur le réseau avec chaque demande.</span><span class="sxs-lookup"><span data-stu-id="88637-157">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="88637-158">L’attribution d’un nouveau nom à un nom de domaine autre que microsoft.com les performances peut être augmentée jusqu’à 25%.</span><span class="sxs-lookup"><span data-stu-id="88637-158">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="88637-159">Remarque ajax.microsoft.com continuera à fonctionner, mais ajax.aspnetcdn.com est recommandé.</span><span class="sxs-lookup"><span data-stu-id="88637-159">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="88637-160">Ancien format : https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="88637-160">Old Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="88637-161">Nouveau format : https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="88637-161">New Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="88637-162">Prise en charge de Visual Studio. vsdoc</span><span class="sxs-lookup"><span data-stu-id="88637-162">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="88637-163">Pour utiliser correctement les fichiers. vsdoc avec Visual Studio 2008, vous devez vous assurer que VS 2008 SP1 est installé et que le correctif logiciel pour les fichiers vsdoc est installé.</span><span class="sxs-lookup"><span data-stu-id="88637-163">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="88637-164">Vous pouvez les récupérer ici :</span><span class="sxs-lookup"><span data-stu-id="88637-164">You can get these from here:</span></span>

- [<span data-ttu-id="88637-165">Télécharger Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="88637-165">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Télécharger Visual Studio 2008 SP1")
- [<span data-ttu-id="88637-166">Télécharger. vsdoc Hotfix pour Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="88637-166">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "Télécharger. vsdoc Hotfix pour Visual Studio 2008 SP1")

<span data-ttu-id="88637-167">Visual Studio 2010 prend en charge les fichiers. vsdoc sans correctifs supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="88637-167">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="88637-168">Utilisation d’ASP.NET AJAX à partir du CDN</span><span class="sxs-lookup"><span data-stu-id="88637-168">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="88637-169">Lorsque vous utilisez ASP.NET 4, vous pouvez rediriger toutes les demandes de scripts ASP.NET Framework vers le CDN.</span><span class="sxs-lookup"><span data-stu-id="88637-169">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="88637-170">La récupération de scripts du CDN au lieu de votre serveur Web local peut améliorer considérablement les performances des sites Web ASP.NET publics.</span><span class="sxs-lookup"><span data-stu-id="88637-170">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="88637-171">Utilisez la propriété ScriptManager EnableCDN pour rediriger toutes les demandes de script ASP.NET Framework vers le CDN Microsoft Ajax :</span><span class="sxs-lookup"><span data-stu-id="88637-171">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="88637-172">Utilisation de jQuery à partir du CDN</span><span class="sxs-lookup"><span data-stu-id="88637-172">Using jQuery from the CDN</span></span>

<span data-ttu-id="88637-173">Vous pouvez utiliser des scripts jQuery hébergés sur CDN dans votre application Web en ajoutant l’élément de script suivant à une page :</span><span class="sxs-lookup"><span data-stu-id="88637-173">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="88637-174">Le CDN comprend également la version minimisés du script jQuery, que vous pouvez utiliser à l’aide de l’élément suivant :</span><span class="sxs-lookup"><span data-stu-id="88637-174">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="88637-175">Pour permettre à votre page de revenir au chargement de jQuery à partir d’un chemin d’accès local sur votre propre site Web si le CDN n’est pas disponible, ajoutez l’élément suivant immédiatement après l’élément qui référence le CDN :</span><span class="sxs-lookup"><span data-stu-id="88637-175">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="88637-176">L’exemple de page suivant utilise la version CDN de la bibliothèque jQuery (avec recours à une copie locale) pour afficher le contenu d’un élément div lorsque l’utilisateur clique sur un bouton.</span><span class="sxs-lookup"><span data-stu-id="88637-176">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="88637-177">Vous pouvez en savoir plus sur jQuery et télécharger une copie locale de jQuery en visitant le site Web [jQuery](http://jquery.com/) .</span><span class="sxs-lookup"><span data-stu-id="88637-177">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="88637-178">Utilisation de jQuery UI à partir du CDN</span><span class="sxs-lookup"><span data-stu-id="88637-178">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="88637-179">Le CDN héberge également la bibliothèque de l’interface utilisateur jQuery.</span><span class="sxs-lookup"><span data-stu-id="88637-179">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="88637-180">La bibliothèque jQuery UI comprend un ensemble complet de widgets et d’effets que vous pouvez utiliser dans vos applications ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="88637-180">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="88637-181">Par exemple, la page suivante montre comment vous pouvez utiliser le DatePicker de l’interface utilisateur jQuery dans le contexte d’une application Web Forms ASP.NET pour afficher un calendrier contextuel :</span><span class="sxs-lookup"><span data-stu-id="88637-181">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="88637-182">Lorsque vous déplacez le focus sur la zone de texte à l’aide de votre clavier, un calendrier s’affiche :</span><span class="sxs-lookup"><span data-stu-id="88637-182">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendrier contextuel créé avec DatePicker](overview/_static/image1.png)

<span data-ttu-id="88637-184">Notez que vous devez inclure trois fichiers du CDN dans le code ci-dessus :</span><span class="sxs-lookup"><span data-stu-id="88637-184">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="88637-185">La bibliothèque jQuery de la bibliothèque de l' &mdash; interface utilisateur jQuery dépend de la bibliothèque jQuery.</span><span class="sxs-lookup"><span data-stu-id="88637-185">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="88637-186">Vous devez ajouter la bibliothèque jQuery à votre page avant d’ajouter la bibliothèque jQuery UI.</span><span class="sxs-lookup"><span data-stu-id="88637-186">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="88637-187">Bibliothèque de l’interface utilisateur jQuery &mdash; la bibliothèque jQuery UI contient tous les effets et widgets de l’interface utilisateur jQuery, tels que le widget DatePicker utilisé dans la page ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="88637-187">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="88637-188">Thème de l’interface utilisateur jQuery &mdash; l’interface utilisateur jQuery prend en charge différents thèmes.</span><span class="sxs-lookup"><span data-stu-id="88637-188">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="88637-189">La page ci-dessus contient un lien vers un fichier CSS pour importer le thème Redmond.</span><span class="sxs-lookup"><span data-stu-id="88637-189">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="88637-190">Tous les thèmes standard de l’interface utilisateur jQuery sont hébergés sur le CDN.</span><span class="sxs-lookup"><span data-stu-id="88637-190">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="88637-191">[Visitez cette page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 sur le CDN Microsoft Ajax") pour afficher les miniatures de chaque thème.</span><span class="sxs-lookup"><span data-stu-id="88637-191">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="88637-192">Pour en savoir plus sur la bibliothèque de l’interface utilisateur jQuery, visitez le site officiel de l' [interface utilisateur jQuery](http://jQueryUI.com "site Web jQuery UI").</span><span class="sxs-lookup"><span data-stu-id="88637-192">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="88637-193">Fichiers tiers sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-193">Third-Party Files on the CDN</span></span>

<span data-ttu-id="88637-194">Le CDN héberge certaines des bibliothèques JavaScript tierces les plus populaires.</span><span class="sxs-lookup"><span data-stu-id="88637-194">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="88637-195">Microsoft ne réclame pas la propriété de toutes les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="88637-195">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="88637-196">Les propriétaires des droits d’auteur des bibliothèques sont les licences de ces bibliothèques.</span><span class="sxs-lookup"><span data-stu-id="88637-196">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="88637-197">Les droits dont vous pouvez avoir besoin pour télécharger et utiliser ces bibliothèques sont octroyés uniquement par les propriétaires des droits d’auteur.</span><span class="sxs-lookup"><span data-stu-id="88637-197">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="88637-198">Étant donné qu’il ne s’agit pas de bibliothèques Microsoft, Microsoft ne fournit aucune garantie ni aucune licence de droits de propriété intellectuelle (y compris aucun droit de brevet implicite) pour les bibliothèques tierces hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="88637-198">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="88637-199">Versions jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-199">jQuery Releases on the CDN</span></span>

<span data-ttu-id="88637-200">Les versions suivantes de jQuery sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="88637-200">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-351"></a><span data-ttu-id="88637-201">jQuery version 3.5.1</span><span class="sxs-lookup"><span data-stu-id="88637-201">jQuery version 3.5.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.slim.min.map

#### <a name="jquery-version-350"></a><span data-ttu-id="88637-202">jQuery version 3.5.0</span><span class="sxs-lookup"><span data-stu-id="88637-202">jQuery version 3.5.0</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.slim.min.map

#### <a name="jquery-version-341"></a><span data-ttu-id="88637-203">jQuery version 3.4.1</span><span class="sxs-lookup"><span data-stu-id="88637-203">jQuery version 3.4.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.min.map

#### <a name="jquery-version-340"></a><span data-ttu-id="88637-204">jQuery version 3.4.0</span><span class="sxs-lookup"><span data-stu-id="88637-204">jQuery version 3.4.0</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.min.map

#### <a name="jquery-version-331"></a><span data-ttu-id="88637-205">jQuery version 3.3.1</span><span class="sxs-lookup"><span data-stu-id="88637-205">jQuery version 3.3.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="88637-206">jQuery version 3.2.1</span><span class="sxs-lookup"><span data-stu-id="88637-206">jQuery version 3.2.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="88637-207">jQuery version 3.2.0</span><span class="sxs-lookup"><span data-stu-id="88637-207">jQuery version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="88637-208">jQuery version 3.1.1</span><span class="sxs-lookup"><span data-stu-id="88637-208">jQuery version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="88637-209">jQuery version 3.1.0</span><span class="sxs-lookup"><span data-stu-id="88637-209">jQuery version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="88637-210">jQuery version 3.0.0</span><span class="sxs-lookup"><span data-stu-id="88637-210">jQuery version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="88637-211">jQuery version 2.2.4</span><span class="sxs-lookup"><span data-stu-id="88637-211">jQuery version 2.2.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="88637-212">jQuery version 2.2.3</span><span class="sxs-lookup"><span data-stu-id="88637-212">jQuery version 2.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="88637-213">jQuery version 2.2.2</span><span class="sxs-lookup"><span data-stu-id="88637-213">jQuery version 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="88637-214">jQuery version 2.2.1</span><span class="sxs-lookup"><span data-stu-id="88637-214">jQuery version 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="88637-215">jQuery version 2.2.0</span><span class="sxs-lookup"><span data-stu-id="88637-215">jQuery version 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="88637-216">jQuery version 2.1.4</span><span class="sxs-lookup"><span data-stu-id="88637-216">jQuery version 2.1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="88637-217">jQuery version 2.1.3</span><span class="sxs-lookup"><span data-stu-id="88637-217">jQuery version 2.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="88637-218">jQuery version 2.1.2</span><span class="sxs-lookup"><span data-stu-id="88637-218">jQuery version 2.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="88637-219">jQuery version 2.1.1</span><span class="sxs-lookup"><span data-stu-id="88637-219">jQuery version 2.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="88637-220">jQuery version 2.1.0</span><span class="sxs-lookup"><span data-stu-id="88637-220">jQuery version 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="88637-221">jQuery version 2.0.3</span><span class="sxs-lookup"><span data-stu-id="88637-221">jQuery version 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="88637-222">jQuery version 2.0.2</span><span class="sxs-lookup"><span data-stu-id="88637-222">jQuery version 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="88637-223">jQuery version 2.0.1</span><span class="sxs-lookup"><span data-stu-id="88637-223">jQuery version 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="88637-224">jQuery version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="88637-224">jQuery version 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="88637-225">jQuery version 1.12.4</span><span class="sxs-lookup"><span data-stu-id="88637-225">jQuery version 1.12.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="88637-226">jQuery version 1.12.3</span><span class="sxs-lookup"><span data-stu-id="88637-226">jQuery version 1.12.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="88637-227">jQuery version 1.12.2</span><span class="sxs-lookup"><span data-stu-id="88637-227">jQuery version 1.12.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="88637-228">jQuery version 1.12.1</span><span class="sxs-lookup"><span data-stu-id="88637-228">jQuery version 1.12.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="88637-229">jQuery version 1.12.0</span><span class="sxs-lookup"><span data-stu-id="88637-229">jQuery version 1.12.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="88637-230">jQuery version 1.11.3</span><span class="sxs-lookup"><span data-stu-id="88637-230">jQuery version 1.11.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="88637-231">jQuery version 1.11.2</span><span class="sxs-lookup"><span data-stu-id="88637-231">jQuery version 1.11.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="88637-232">jQuery version 1.11.1</span><span class="sxs-lookup"><span data-stu-id="88637-232">jQuery version 1.11.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="88637-233">jQuery version 1.11.0</span><span class="sxs-lookup"><span data-stu-id="88637-233">jQuery version 1.11.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="88637-234">jQuery version 1.10.2</span><span class="sxs-lookup"><span data-stu-id="88637-234">jQuery version 1.10.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="88637-235">jQuery version 1.10.1</span><span class="sxs-lookup"><span data-stu-id="88637-235">jQuery version 1.10.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="88637-236">jQuery version 1.10.0</span><span class="sxs-lookup"><span data-stu-id="88637-236">jQuery version 1.10.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="88637-237">jQuery version 1.9.1</span><span class="sxs-lookup"><span data-stu-id="88637-237">jQuery version 1.9.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="88637-238">jQuery version 1.9.0</span><span class="sxs-lookup"><span data-stu-id="88637-238">jQuery version 1.9.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="88637-239">jQuery version 1.8.3 administrer</span><span class="sxs-lookup"><span data-stu-id="88637-239">jQuery version 1.8.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="88637-240">jQuery version 1.8.2</span><span class="sxs-lookup"><span data-stu-id="88637-240">jQuery version 1.8.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="88637-241">jQuery version 1.8.1</span><span class="sxs-lookup"><span data-stu-id="88637-241">jQuery version 1.8.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="88637-242">jQuery version 1.8.0</span><span class="sxs-lookup"><span data-stu-id="88637-242">jQuery version 1.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="88637-243">jQuery version 1.7.2</span><span class="sxs-lookup"><span data-stu-id="88637-243">jQuery version 1.7.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="88637-244">jQuery version 1.7.1</span><span class="sxs-lookup"><span data-stu-id="88637-244">jQuery version 1.7.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="88637-245">jQuery version 1,7</span><span class="sxs-lookup"><span data-stu-id="88637-245">jQuery version 1.7</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="88637-246">jQuery version 1.6.4</span><span class="sxs-lookup"><span data-stu-id="88637-246">jQuery version 1.6.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="88637-247">jQuery version 1.6.3</span><span class="sxs-lookup"><span data-stu-id="88637-247">jQuery version 1.6.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="88637-248">jQuery version 1.6.2</span><span class="sxs-lookup"><span data-stu-id="88637-248">jQuery version 1.6.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="88637-249">jQuery version 1.6.1</span><span class="sxs-lookup"><span data-stu-id="88637-249">jQuery version 1.6.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="88637-250">jQuery version 1,6</span><span class="sxs-lookup"><span data-stu-id="88637-250">jQuery version 1.6</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="88637-251">jQuery version 1.5.2</span><span class="sxs-lookup"><span data-stu-id="88637-251">jQuery version 1.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="88637-252">jQuery version 1.5.1</span><span class="sxs-lookup"><span data-stu-id="88637-252">jQuery version 1.5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="88637-253">jQuery version 1,5</span><span class="sxs-lookup"><span data-stu-id="88637-253">jQuery version 1.5</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="88637-254">jQuery version 1.4.4</span><span class="sxs-lookup"><span data-stu-id="88637-254">jQuery version 1.4.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="88637-255">jQuery version 1.4.3</span><span class="sxs-lookup"><span data-stu-id="88637-255">jQuery version 1.4.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="88637-256">jQuery version 1.4.2</span><span class="sxs-lookup"><span data-stu-id="88637-256">jQuery version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="88637-257">jQuery version 1.4.1</span><span class="sxs-lookup"><span data-stu-id="88637-257">jQuery version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="88637-258">jQuery version 1,4</span><span class="sxs-lookup"><span data-stu-id="88637-258">jQuery version 1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="88637-259">jQuery version 1.3.2</span><span class="sxs-lookup"><span data-stu-id="88637-259">jQuery version 1.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="88637-260">Versions de jQuery Migrate sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-260">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="88637-261">Les versions suivantes de jQuery Migrate sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="88637-261">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="88637-262">jQuery Migrate version 3.0.0</span><span class="sxs-lookup"><span data-stu-id="88637-262">jQuery Migrate version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="88637-263">jQuery Migrate version 1.2.1</span><span class="sxs-lookup"><span data-stu-id="88637-263">jQuery Migrate version 1.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="88637-264">jQuery Migrate version 1.2.0</span><span class="sxs-lookup"><span data-stu-id="88637-264">jQuery Migrate version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="88637-265">jQuery Migrate version 1.1.1</span><span class="sxs-lookup"><span data-stu-id="88637-265">jQuery Migrate version 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="88637-266">jQuery migrer la version 1.1.0</span><span class="sxs-lookup"><span data-stu-id="88637-266">jQuery Migrate version 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="88637-267">jQuery Migrate version 1.0.0</span><span class="sxs-lookup"><span data-stu-id="88637-267">jQuery Migrate version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="88637-268">Versions de l’interface utilisateur jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-268">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="88637-269">Les versions suivantes de la bibliothèque de l’interface utilisateur jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="88637-269">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="88637-270">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="88637-270">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="88637-271">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="88637-271">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-272">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="88637-272">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-273">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="88637-273">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-274">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="88637-274">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-275">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="88637-275">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-276">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="88637-276">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-277">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="88637-277">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-278">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="88637-278">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-279">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="88637-279">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-280">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="88637-280">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-281">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="88637-281">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-282">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="88637-282">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-283">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="88637-283">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-284">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="88637-284">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-285">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="88637-285">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-286">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="88637-286">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-287">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="88637-287">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-288">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="88637-288">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-289">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="88637-289">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-290">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="88637-290">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-291">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="88637-291">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-292">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="88637-292">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-293">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="88637-293">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-294">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="88637-294">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-295">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="88637-295">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-296">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="88637-296">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-297">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="88637-297">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-298">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="88637-298">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-299">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="88637-299">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-300">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="88637-300">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-301">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="88637-301">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-302">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="88637-302">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-303">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="88637-303">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-304">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="88637-304">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-305">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="88637-305">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="88637-306">Versions de validation jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-306">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="88637-307">Les versions suivantes du plug-in de [validation jQuery](https://jqueryvalidation.org/ "Plug-in de validation jQuery") sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="88637-307">The following releases of the [jQuery Validation](https://jqueryvalidation.org/ "jQuery Validation Plugin") plugin are hosted on this CDN.</span></span> <span data-ttu-id="88637-308">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="88637-308">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="88637-309">jQuery Validate 1.19.2</span><span class="sxs-lookup"><span data-stu-id="88637-309">jQuery Validate 1.19.2</span></span>](jquery-validate/cdnjqueryvalidate1192.md "1.19.2 de validation jQuery")
- [<span data-ttu-id="88637-310">jQuery Validate 1.19.1</span><span class="sxs-lookup"><span data-stu-id="88637-310">jQuery Validate 1.19.1</span></span>](jquery-validate/cdnjqueryvalidate1191.md "1.19.1 de validation jQuery")
- [<span data-ttu-id="88637-311">jQuery Validate 1.19.0</span><span class="sxs-lookup"><span data-stu-id="88637-311">jQuery Validate 1.19.0</span></span>](jquery-validate/cdnjqueryvalidate1190.md "1.19.0 de validation jQuery")
- [<span data-ttu-id="88637-312">jQuery Validate 1.17.0</span><span class="sxs-lookup"><span data-stu-id="88637-312">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "1.17.0 de validation jQuery")
- [<span data-ttu-id="88637-313">jQuery Validate 1.16.0</span><span class="sxs-lookup"><span data-stu-id="88637-313">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery Validation 1.16.0")
- [<span data-ttu-id="88637-314">jQuery Validate 1.15.1</span><span class="sxs-lookup"><span data-stu-id="88637-314">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery Validation 1.15.1")
- [<span data-ttu-id="88637-315">jQuery Validate 1.15.0</span><span class="sxs-lookup"><span data-stu-id="88637-315">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery Validation 1.15.0")
- [<span data-ttu-id="88637-316">jQuery Validate 1.14.0</span><span class="sxs-lookup"><span data-stu-id="88637-316">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery Validation 1.14.0")
- [<span data-ttu-id="88637-317">jQuery Validate 1.13.1</span><span class="sxs-lookup"><span data-stu-id="88637-317">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery Validation 1.13.1")
- [<span data-ttu-id="88637-318">jQuery Validate 1.13.0</span><span class="sxs-lookup"><span data-stu-id="88637-318">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery Validation 1.13.0")
- [<span data-ttu-id="88637-319">jQuery Validate 1.12.0</span><span class="sxs-lookup"><span data-stu-id="88637-319">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery Validation 1.12.0")
- [<span data-ttu-id="88637-320">jQuery Validate 1.11.1</span><span class="sxs-lookup"><span data-stu-id="88637-320">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery Validation 1.11.1")
- [<span data-ttu-id="88637-321">jQuery Validate 1.11.0</span><span class="sxs-lookup"><span data-stu-id="88637-321">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery Validation 1.11.0")
- [<span data-ttu-id="88637-322">jQuery Validate 1.10.0</span><span class="sxs-lookup"><span data-stu-id="88637-322">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery Validation 1.10.0")
- [<span data-ttu-id="88637-323">jQuery valider 1,9</span><span class="sxs-lookup"><span data-stu-id="88637-323">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate version 1.9")
- [<span data-ttu-id="88637-324">jQuery Validate 1.8.1</span><span class="sxs-lookup"><span data-stu-id="88637-324">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate version 1.8.1")
- [<span data-ttu-id="88637-325">jQuery valider 1,8</span><span class="sxs-lookup"><span data-stu-id="88637-325">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate version 1.8")
- [<span data-ttu-id="88637-326">jQuery valider 1,7</span><span class="sxs-lookup"><span data-stu-id="88637-326">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate version 1.7")
- [<span data-ttu-id="88637-327">jQuery Validate 1.6</span><span class="sxs-lookup"><span data-stu-id="88637-327">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery Validate 1.6")
- [<span data-ttu-id="88637-328">jQuery Validate 1.5.5</span><span class="sxs-lookup"><span data-stu-id="88637-328">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery Validate 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="88637-329">Versions de jQuery mobile sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-329">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="88637-330">Les versions suivantes de la bibliothèque jQuery mobile sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="88637-330">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="88637-331">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="88637-331">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="88637-332">1.4.5 jQuery mobile</span><span class="sxs-lookup"><span data-stu-id="88637-332">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-333">jQuery mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="88637-333">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-334">jQuery mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="88637-334">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-335">1.4.0 jQuery mobile</span><span class="sxs-lookup"><span data-stu-id="88637-335">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-336">jQuery mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="88637-336">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-337">jQuery mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="88637-337">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-338">1.3.0 jQuery mobile</span><span class="sxs-lookup"><span data-stu-id="88637-338">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-339">1.2.0 jQuery mobile</span><span class="sxs-lookup"><span data-stu-id="88637-339">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-340">jQuery mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="88637-340">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-341">jQuery mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="88637-341">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-342">jQuery mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="88637-342">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-343">jQuery mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="88637-343">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-344">jQuery mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="88637-344">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-345">jQuery mobile 1,0</span><span class="sxs-lookup"><span data-stu-id="88637-345">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-346">jQuery mobile 1,0 RC 2</span><span class="sxs-lookup"><span data-stu-id="88637-346">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-347">jQuery mobile 1,0 RC 1</span><span class="sxs-lookup"><span data-stu-id="88637-347">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 sur le CDN Microsoft Ajax")
- [<span data-ttu-id="88637-348">jQuery mobile 1,0 bêta 3</span><span class="sxs-lookup"><span data-stu-id="88637-348">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Bêta 3 sur le CDN Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="88637-349">Versions de modèles jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-349">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="88637-350">Les versions suivantes du plug-in modèles jQuery sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="88637-350">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="88637-351">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="88637-351">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="88637-352">jQuery Templates Bêta 1</span><span class="sxs-lookup"><span data-stu-id="88637-352">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery Templates Bêta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="88637-353">Versions de cycle jQuery sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-353">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="88637-354">Les versions suivantes du plug-in jQuery cycle sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="88637-354">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="88637-355">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="88637-355">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="88637-356">jQuery cycle 2,99</span><span class="sxs-lookup"><span data-stu-id="88637-356">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery Cycle 2.99")
- [<span data-ttu-id="88637-357">jQuery Cycle 2.94</span><span class="sxs-lookup"><span data-stu-id="88637-357">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Cycle 2.94")
- [<span data-ttu-id="88637-358">jQuery Cycle 2.88</span><span class="sxs-lookup"><span data-stu-id="88637-358">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery Cycle 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="88637-359">les versions jQuery DataTables sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-359">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="88637-360">Les versions suivantes du plug-in jQuery DataTables sont hébergées sur ce CDN.</span><span class="sxs-lookup"><span data-stu-id="88637-360">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="88637-361">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="88637-361">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="88637-362">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="88637-362">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="88637-363">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="88637-363">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="88637-364">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="88637-364">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="88637-365">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="88637-365">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="88637-366">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="88637-366">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="88637-367">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="88637-367">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="88637-368">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="88637-368">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="88637-369">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="88637-369">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="88637-370">Versions de Modernizr sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-370">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="88637-371">Les versions suivantes de [Modernizr](http://www.modernizr.com "Modernizr") sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="88637-371">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-3.5.0.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="88637-372">Versions de JSHint sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-372">JSHint Releases on the CDN</span></span>

<span data-ttu-id="88637-373">Les versions suivantes de [JSHint](http://www.jshint.com "JSHint") sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="88637-373">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="88637-374">Versions de Knockout sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-374">Knockout Releases on the CDN</span></span>

<span data-ttu-id="88637-375">Les versions suivantes de [Knockout](http://www.knockoutjs.com "Knockout") sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="88637-375">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="88637-376">Globaliser les mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-376">Globalize Releases on the CDN</span></span>

<span data-ttu-id="88637-377">Les versions suivantes de [globalisation](https://github.com/jquery/globalize "Globaliser") sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="88637-377">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="88637-378">Globaliser la version 1.0.0</span><span class="sxs-lookup"><span data-stu-id="88637-378">Globalize version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="88637-379">Globaliser la version 0.1.1</span><span class="sxs-lookup"><span data-stu-id="88637-379">Globalize version 0.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="88637-380">toutes les cultures</span><span class="sxs-lookup"><span data-stu-id="88637-380">all cultures</span></span>
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="88637-381">Remplacez « {culture-code} » par le code de culture souhaité, par exemple globalize.culture.en-GB.js= = fichiers Microsoft sur le CDN = = ces bibliothèques ont été chargées par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="88637-381">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="88637-382">Répondre aux mises en production sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-382">Respond Releases on the CDN</span></span>

<span data-ttu-id="88637-383">Les versions de [réponse](https://github.com/scottjehl/Respond "Réponse") suivantes sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="88637-383">The following releases of [Respond](https://github.com/scottjehl/Respond "Respond") are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="88637-384">Répondre à la version 1.4.2</span><span class="sxs-lookup"><span data-stu-id="88637-384">Respond version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="88637-385">Répondre à la version 1.4.1</span><span class="sxs-lookup"><span data-stu-id="88637-385">Respond version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="88637-386">Répondre à la version 1.4.0</span><span class="sxs-lookup"><span data-stu-id="88637-386">Respond version 1.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="88637-387">Répondre à la version 1.3.0</span><span class="sxs-lookup"><span data-stu-id="88637-387">Respond version 1.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="88637-388">Répondre à la version 1.2.0</span><span class="sxs-lookup"><span data-stu-id="88637-388">Respond version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="88637-389">Versions d’amorçage sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-389">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="88637-390">Les versions suivantes de bootstrap [GetBootstrap.com](http://getbootstrap.com "getbootstrap.com") sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="88637-390">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-452"></a><span data-ttu-id="88637-391">Version d’amorçage 4.5.2</span><span class="sxs-lookup"><span data-stu-id="88637-391">Bootstrap version 4.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-450"></a><span data-ttu-id="88637-392">Bootstrap version 4.5.0</span><span class="sxs-lookup"><span data-stu-id="88637-392">Bootstrap version 4.5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-441"></a><span data-ttu-id="88637-393">Bootstrap version 4.4.1</span><span class="sxs-lookup"><span data-stu-id="88637-393">Bootstrap version 4.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-431"></a><span data-ttu-id="88637-394">Version de démarrage 4.3.1</span><span class="sxs-lookup"><span data-stu-id="88637-394">Bootstrap version 4.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-421"></a><span data-ttu-id="88637-395">Version d’amorçage 4.2.1</span><span class="sxs-lookup"><span data-stu-id="88637-395">Bootstrap version 4.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-411"></a><span data-ttu-id="88637-396">Version d’amorçage 4.1.1</span><span class="sxs-lookup"><span data-stu-id="88637-396">Bootstrap version 4.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-400"></a><span data-ttu-id="88637-397">Version d’amorçage 4.0.0</span><span class="sxs-lookup"><span data-stu-id="88637-397">Bootstrap version 4.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-341"></a><span data-ttu-id="88637-398">Version d’amorçage 3.4.1</span><span class="sxs-lookup"><span data-stu-id="88637-398">Bootstrap version 3.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-340"></a><span data-ttu-id="88637-399">Version de démarrage 3.4.0</span><span class="sxs-lookup"><span data-stu-id="88637-399">Bootstrap version 3.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="88637-400">Version de démarrage 3.3.7</span><span class="sxs-lookup"><span data-stu-id="88637-400">Bootstrap version 3.3.7</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="88637-401">Version de démarrage 3.3.6</span><span class="sxs-lookup"><span data-stu-id="88637-401">Bootstrap version 3.3.6</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="88637-402">Version de démarrage 3.3.5</span><span class="sxs-lookup"><span data-stu-id="88637-402">Bootstrap version 3.3.5</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="88637-403">Version de démarrage 3.3.4</span><span class="sxs-lookup"><span data-stu-id="88637-403">Bootstrap version 3.3.4</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="88637-404">Bootstrap version 3.3.2</span><span class="sxs-lookup"><span data-stu-id="88637-404">Bootstrap version 3.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="88637-405">Version d’amorçage 3.3.1</span><span class="sxs-lookup"><span data-stu-id="88637-405">Bootstrap version 3.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="88637-406">Version de démarrage 3.3.0</span><span class="sxs-lookup"><span data-stu-id="88637-406">Bootstrap version 3.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="88637-407">Version de démarrage 3.2.0</span><span class="sxs-lookup"><span data-stu-id="88637-407">Bootstrap version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="88637-408">Version d’amorçage 3.1.1</span><span class="sxs-lookup"><span data-stu-id="88637-408">Bootstrap version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="88637-409">Version d’amorçage 3.1.0</span><span class="sxs-lookup"><span data-stu-id="88637-409">Bootstrap version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="88637-410">Version de démarrage 3.0.3</span><span class="sxs-lookup"><span data-stu-id="88637-410">Bootstrap version 3.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="88637-411">Version d’amorçage 3.0.2</span><span class="sxs-lookup"><span data-stu-id="88637-411">Bootstrap version 3.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="88637-412">Bootstrap version 3.0.1</span><span class="sxs-lookup"><span data-stu-id="88637-412">Bootstrap version 3.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="88637-413">Version de démarrage 3.0.0</span><span class="sxs-lookup"><span data-stu-id="88637-413">Bootstrap version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="88637-414">Version 2.3.2 de bootstrap</span><span class="sxs-lookup"><span data-stu-id="88637-414">Bootstrap version 2.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="88637-415">Bootstrap version 2.3.1</span><span class="sxs-lookup"><span data-stu-id="88637-415">Bootstrap version 2.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="88637-416">Release TouchCarousel releases sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-416">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="88637-417">Les versions suivantes des [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") versions de bootstrap TouchCarousel sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="88637-417">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="88637-418">Bootstrap TouchCarousel version 0.8.0</span><span class="sxs-lookup"><span data-stu-id="88637-418">Bootstrap TouchCarousel version 0.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="88637-419">Versions de Hammer.js sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-419">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="88637-420">Les versions de [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js suivantes sont hébergées sur le CDN :</span><span class="sxs-lookup"><span data-stu-id="88637-420">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="88637-421">Version de Hammer.js 2.0.4</span><span class="sxs-lookup"><span data-stu-id="88637-421">Hammer.js version 2.0.4</span></span>

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="88637-422">ASP.NET Web Forms et les versions Ajax sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-422">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="88637-423">Les versions suivantes de la bibliothèque ASP.NET AJAX sont hébergées sur le CDN.</span><span class="sxs-lookup"><span data-stu-id="88637-423">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="88637-424">Cliquez sur chaque lien pour afficher la liste réelle des fichiers.</span><span class="sxs-lookup"><span data-stu-id="88637-424">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="88637-425">ASP.NET Web Forms et Ajax version 4.5.2</span><span class="sxs-lookup"><span data-stu-id="88637-425">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "ASP.NET Web Forms et Ajax 4.5.2")
- [<span data-ttu-id="88637-426">ASP.NET Web Forms et Ajax version 4</span><span class="sxs-lookup"><span data-stu-id="88637-426">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "ASP.NET Web Forms et Ajax 4")
- [<span data-ttu-id="88637-427">ASP.NET AJAX version 3,5</span><span class="sxs-lookup"><span data-stu-id="88637-427">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="88637-428">Versions de ASP.NET MVC sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-428">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="88637-429">Les fichiers JavaScript ASP.NET MVC suivants sont hébergés sur ce CDN :</span><span class="sxs-lookup"><span data-stu-id="88637-429">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="88637-430">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="88637-430">ASP.NET MVC 5.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="88637-431">ASP.NET MVC 5,1</span><span class="sxs-lookup"><span data-stu-id="88637-431">ASP.NET MVC 5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="88637-432">ASP.NET MVC 5,0</span><span class="sxs-lookup"><span data-stu-id="88637-432">ASP.NET MVC 5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="88637-433">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="88637-433">ASP.NET MVC 4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="88637-434">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="88637-434">ASP.NET MVC 3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.min.js 
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.js 
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="88637-435">ASP.NET MVC 2,0</span><span class="sxs-lookup"><span data-stu-id="88637-435">ASP.NET MVC 2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="88637-436">ASP.NET MVC 1,0</span><span class="sxs-lookup"><span data-stu-id="88637-436">ASP.NET MVC 1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="88637-437">Mises en production de Signalr ASP.NET sur le CDN</span><span class="sxs-lookup"><span data-stu-id="88637-437">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="88637-438">Les fichiers JavaScript Signalr ASP.NET suivants sont hébergés sur ce CDN :</span><span class="sxs-lookup"><span data-stu-id="88637-438">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="88637-439">ASP.NET Signalr 2.2.2</span><span class="sxs-lookup"><span data-stu-id="88637-439">ASP.NET SignalR 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="88637-440">ASP.NET Signalr 2.2.1</span><span class="sxs-lookup"><span data-stu-id="88637-440">ASP.NET SignalR 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="88637-441">ASP.NET Signalr 2.2.0</span><span class="sxs-lookup"><span data-stu-id="88637-441">ASP.NET SignalR 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="88637-442">ASP.NET Signalr 2.1.0</span><span class="sxs-lookup"><span data-stu-id="88637-442">ASP.NET SignalR 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="88637-443">ASP.NET Signalr 2.0.3</span><span class="sxs-lookup"><span data-stu-id="88637-443">ASP.NET SignalR 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="88637-444">Signaleur ASP.NET 2.0.2</span><span class="sxs-lookup"><span data-stu-id="88637-444">ASP.NET SignalR 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="88637-445">Signalr ASP.NET 2.0.1</span><span class="sxs-lookup"><span data-stu-id="88637-445">ASP.NET SignalR 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="88637-446">ASP.NET Signalr 2.0.0</span><span class="sxs-lookup"><span data-stu-id="88637-446">ASP.NET SignalR 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="88637-447">Signaleur ASP.NET 1.1.3</span><span class="sxs-lookup"><span data-stu-id="88637-447">ASP.NET SignalR 1.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="88637-448">ASP.NET Signalr 1.1.2</span><span class="sxs-lookup"><span data-stu-id="88637-448">ASP.NET SignalR 1.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="88637-449">ASP.NET Signalr 1.1.1</span><span class="sxs-lookup"><span data-stu-id="88637-449">ASP.NET SignalR 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="88637-450">Signalr ASP.NET 1.1.0</span><span class="sxs-lookup"><span data-stu-id="88637-450">ASP.NET SignalR 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="88637-451">ASP.NET Signalr-1.0.1</span><span class="sxs-lookup"><span data-stu-id="88637-451">ASP.NET SignalR 1.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="88637-452">Pour plus d’informations sur les conditions d’utilisation du CDN, consultez [conditions d’utilisation du CDN Microsoft Ajax](https://www.asp.net/terms-of-use "Conditions d’utilisation du CDN Microsoft Ajax").</span><span class="sxs-lookup"><span data-stu-id="88637-452">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
