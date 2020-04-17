---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Utilisez AJAX pour fournir des mises à jour dynamiques ( Microsoft Docs
author: rick-anderson
description: Étape 10 met en œuvre le soutien pour les utilisateurs connectés à RSVP leur intérêt à assister à un dîner, en utilisant une approche basée à Ajax intégré dans le détail du dîner ...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 13680b91edc665852fd4af56e4fc79551e6de15e
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81541245"
---
# <a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="425cb-103">Utiliser AJAX pour fournir des mises à jour dynamiques</span><span class="sxs-lookup"><span data-stu-id="425cb-103">Use AJAX to Deliver Dynamic Updates</span></span>

<span data-ttu-id="425cb-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="425cb-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="425cb-105">Télécharger le PDF</span><span class="sxs-lookup"><span data-stu-id="425cb-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="425cb-106">Il s’agit de l’étape 10 d’un tutoriel gratuit ["NerdDinner" application](introducing-the-nerddinner-tutorial.md) qui marche à travers la façon de construire une petite, mais complète, application web en utilisant ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="425cb-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="425cb-107">L’étape 10 met en œuvre le soutien aux utilisateurs connectés à RSVP leur intérêt à assister à un dîner, en utilisant une approche basée sur Ajax intégrée dans la page des détails du dîner.</span><span class="sxs-lookup"><span data-stu-id="425cb-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="425cb-108">Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les tutoriels [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="425cb-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="425cb-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepte</span><span class="sxs-lookup"><span data-stu-id="425cb-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="425cb-110">Mettons maintenant en œuvre le soutien aux utilisateurs connectés à RSVP leur intérêt à assister à un dîner.</span><span class="sxs-lookup"><span data-stu-id="425cb-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="425cb-111">Nous allons activer cela en utilisant une approche basée sur AJAX intégrée dans la page de détails du dîner.</span><span class="sxs-lookup"><span data-stu-id="425cb-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="425cb-112">Indiquer si l’utilisateur est RSVP’d</span><span class="sxs-lookup"><span data-stu-id="425cb-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="425cb-113">Les utilisateurs peuvent visiter l’URL */Dinners/Details/[id]* pour voir les détails d’un dîner particulier :</span><span class="sxs-lookup"><span data-stu-id="425cb-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="425cb-114">La méthode d’action Détails() est mise en œuvre comme telle:</span><span class="sxs-lookup"><span data-stu-id="425cb-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="425cb-115">Notre première étape pour mettre en œuvre le support RSVP sera d’ajouter une méthode d’aide "IsUserRegistered (username)" à notre objet Dinner (dans la classe partielle Dinner.cs que nous avons construite plus tôt).</span><span class="sxs-lookup"><span data-stu-id="425cb-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="425cb-116">Cette méthode d’aide revient vrai ou faux selon que l’utilisateur est actuellement RSVP’d pour le dîner:</span><span class="sxs-lookup"><span data-stu-id="425cb-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="425cb-117">Nous pouvons ensuite ajouter le code suivant à notre modèle de vue Details.aspx pour afficher un message approprié indiquant si l’utilisateur est enregistré ou non pour l’événement :</span><span class="sxs-lookup"><span data-stu-id="425cb-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="425cb-118">Et maintenant, quand un utilisateur visite un dîner, ils sont inscrits pour qu’ils voient ce message:</span><span class="sxs-lookup"><span data-stu-id="425cb-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="425cb-119">Et quand ils visitent un dîner, ils ne sont pas inscrits pour ils verront le message ci-dessous:</span><span class="sxs-lookup"><span data-stu-id="425cb-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="425cb-120">Mise en œuvre de la méthode d’action du registre</span><span class="sxs-lookup"><span data-stu-id="425cb-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="425cb-121">Ajoutons maintenant les fonctionnalités nécessaires pour permettre aux utilisateurs de RSVP pour un dîner à partir de la page détails.</span><span class="sxs-lookup"><span data-stu-id="425cb-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="425cb-122">Pour mettre en œuvre cela, nous allons créer une nouvelle classe "RSVPController" en&gt;cliquant à droite sur le répertoire des contrôleurs et en choisissant la commande de menu Add-Controller.</span><span class="sxs-lookup"><span data-stu-id="425cb-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="425cb-123">Nous metndons en œuvre une méthode d’action «Register» au sein de la nouvelle classe RSVPController qui prend un id pour un dîner comme argument, récupère l’objet de dîner approprié, vérifie si l’utilisateur connecté est actuellement dans la liste des utilisateurs qui se sont inscrits pour elle, et si ce n’est ajoute un objet RSVP pour eux:</span><span class="sxs-lookup"><span data-stu-id="425cb-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="425cb-124">Remarquez ci-dessus comment nous retournons une chaîne simple comme la sortie de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="425cb-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="425cb-125">Nous aurions pu intégrer ce message dans un modèle de vue , mais comme il est si petit, nous allons simplement utiliser la méthode d’aide content() sur la classe de base du contrôleur et retourner un message de chaîne comme ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="425cb-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="425cb-126">Appel à la méthode d’action RSVPForEvent en utilisant AJAX</span><span class="sxs-lookup"><span data-stu-id="425cb-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="425cb-127">Nous utiliserons AJAX pour invoquer la méthode d’action Du Registre de notre point de vue Détails.</span><span class="sxs-lookup"><span data-stu-id="425cb-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="425cb-128">Mettre en œuvre cela est assez facile.</span><span class="sxs-lookup"><span data-stu-id="425cb-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="425cb-129">Tout d’abord, nous allons ajouter deux références de bibliothèque de script:</span><span class="sxs-lookup"><span data-stu-id="425cb-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="425cb-130">La première bibliothèque fait référence au noyau ASP.NET bibliothèque de scripts côté client AJAX.</span><span class="sxs-lookup"><span data-stu-id="425cb-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="425cb-131">Ce fichier est d’environ 24k de taille (compressé) et contient la fonctionnalité DE base DU client AJAX.</span><span class="sxs-lookup"><span data-stu-id="425cb-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="425cb-132">La deuxième bibliothèque contient des fonctions utilitaires qui s’intègrent aux méthodes d’aide AJAX intégrées de ASP.NET MVC (que nous utiliserons sous peu).</span><span class="sxs-lookup"><span data-stu-id="425cb-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="425cb-133">Nous pouvons ensuite mettre à jour le code de modèle de vue que nous avons ajouté plus tôt de sorte qu’au lieu de sortir un message "Vous n’êtes pas inscrit pour cet événement", nous rendons plutôt un lien qui, lorsqu’il est poussé effectue un appel AJAX qui invoque notre méthode d’action RSVPForEvent sur notre contrôleur RSVP et RSVPs l’utilisateur:</span><span class="sxs-lookup"><span data-stu-id="425cb-133">We can then update the view template code we added earlier so that instead of outputting a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="425cb-134">La méthode d’aide Ajax.ActionLink () utilisée ci-dessus est intégrée ASP.NET MVC et est similaire à la méthode d’aide Html.ActionLink(), sauf qu’au lieu d’effectuer une navigation standard, il fait un appel AJAX à la méthode d’action lorsque le lien est cliqué.</span><span class="sxs-lookup"><span data-stu-id="425cb-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="425cb-135">Ci-dessus, nous appelons la méthode d’action "Register" sur le contrôleur "RSVP" et de passer le DinnerID comme le paramètre "id" à elle.</span><span class="sxs-lookup"><span data-stu-id="425cb-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="425cb-136">Le paramètre Final AjaxOptions que nous passons indique que nous voulons &lt;prendre&gt; le contenu retourné de la méthode d’action et mettre à jour l’élément div HTML sur la page dont l’id est "rsvpmsg".</span><span class="sxs-lookup"><span data-stu-id="425cb-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="425cb-137">Et maintenant, quand un utilisateur navigue à un dîner, ils ne sont pas inscrits pour encore ils vont voir un lien vers RSVP pour elle:</span><span class="sxs-lookup"><span data-stu-id="425cb-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="425cb-138">S’ils cliquent sur le lien "RSVP pour cet événement", ils feront un appel AJAX à la méthode d’action Du Registre sur le contrôleur RSVP, et quand il se termine, ils verront un message mis à jour comme ci-dessous:</span><span class="sxs-lookup"><span data-stu-id="425cb-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="425cb-139">La bande passante réseau et le trafic impliqué lors de la prise de cet appel AJAX est vraiment léger.</span><span class="sxs-lookup"><span data-stu-id="425cb-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="425cb-140">Lorsque l’utilisateur clique sur le lien "RSVP pour cet événement", une petite demande de réseau HTTP POST est faite à l’URL */Dîners/Register/1* qui ressemble ci-dessous sur le fil:</span><span class="sxs-lookup"><span data-stu-id="425cb-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="425cb-141">Et la réponse de notre méthode d’action Registre est simplement:</span><span class="sxs-lookup"><span data-stu-id="425cb-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="425cb-142">Cet appel léger est rapide et fonctionnera même sur un réseau lent.</span><span class="sxs-lookup"><span data-stu-id="425cb-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="425cb-143">Ajout d’une animation jQuery</span><span class="sxs-lookup"><span data-stu-id="425cb-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="425cb-144">La fonctionnalité AJAX que nous avons implémentée fonctionne bien et rapidement.</span><span class="sxs-lookup"><span data-stu-id="425cb-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="425cb-145">Parfois, il peut arriver si vite, cependant, qu’un utilisateur pourrait ne pas remarquer que le lien RSVP a été remplacé par un nouveau texte.</span><span class="sxs-lookup"><span data-stu-id="425cb-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="425cb-146">Pour rendre le résultat un peu plus évident, nous pouvons ajouter une animation simple pour attirer l’attention sur le message de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="425cb-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="425cb-147">Le modèle de projet MVC par défaut ASP.NET comprend jQuery , une excellente (et très populaire) bibliothèque Open Source JavaScript qui est également pris en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="425cb-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="425cb-148">jQuery fournit un certain nombre de fonctionnalités, y compris une belle sélection HTML DOM et la bibliothèque d’effets.</span><span class="sxs-lookup"><span data-stu-id="425cb-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="425cb-149">Pour utiliser jQuery nous allons d’abord ajouter une référence script à elle.</span><span class="sxs-lookup"><span data-stu-id="425cb-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="425cb-150">Parce que nous allons utiliser jQuery dans une variété d’endroits dans notre site, nous allons ajouter la référence de script dans notre fichier de page principale Site.master afin que toutes les pages peuvent l’utiliser.</span><span class="sxs-lookup"><span data-stu-id="425cb-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="425cb-151">*Astuce: assurez-vous d’avoir installé le hotfix JavaScript intellisense pour VS 2008 SP1 qui permet un support intellisense plus riche pour les fichiers JavaScript (y compris jQuery). Vous pouvez le télécharger à partir de:http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="425cb-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="425cb-152">Code écrit à l’aide de JQuery utilise souvent une méthode JavaScript globale qui récupère un ou plusieurs éléments HTML à l’aide d’un sélecteur CSS.</span><span class="sxs-lookup"><span data-stu-id="425cb-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="425cb-153">Par exemple, *$(« #rsvpmsg »)* sélectionne n’importe quel élément HTML avec l’id de rsvpmsg, tandis que *$(".quelque chose")* sélectionnerait tous les éléments avec le nom de classe CSS "quelque chose".</span><span class="sxs-lookup"><span data-stu-id="425cb-153">For example, *$("#rsvpmsg")* selects any HTML element with the id of rsvpmsg, while *$(".something")* would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="425cb-154">Vous pouvez également écrire des requêtes plus avancées comme « retourner tous les boutons radio vérifiés » à l’aide d’une requête de sélecteur comme : *$ («@typeentrée[radio][]«@checked)*.</span><span class="sxs-lookup"><span data-stu-id="425cb-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: *$("input[@type=radio][@checked]")*.</span></span>

<span data-ttu-id="425cb-155">Une fois que vous avez sélectionné des éléments, vous pouvez appeler des méthodes sur eux pour agir, comme les cacher: *$("#rsvpmsg").hide();*</span><span class="sxs-lookup"><span data-stu-id="425cb-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="425cb-156">Pour notre scénario RSVP, nous définirons une fonction JavaScript simple nommée "AnimateRSVPMessage" qui &lt;sélectionne&gt; le div "rsvpmsg" et anime la taille de son contenu texte.</span><span class="sxs-lookup"><span data-stu-id="425cb-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="425cb-157">Le code ci-dessous démarre le texte petit et le fait ensuite augmenter sur un délai de 400 millisecondes:</span><span class="sxs-lookup"><span data-stu-id="425cb-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="425cb-158">Nous pouvons ensuite mettre en écoute cette fonction JavaScript à appeler après notre appel AJAX avec succès en passant son nom à notre Ajax.ActionLink () méthode d’aide (via l’AjaxOptions "OnSuccess" propriété événement):</span><span class="sxs-lookup"><span data-stu-id="425cb-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="425cb-159">Et maintenant, lorsque le lien "RSVP pour cet événement" est cliqué et que notre appel AJAX se termine avec succès, le message de contenu renvoyé animera et grandira:</span><span class="sxs-lookup"><span data-stu-id="425cb-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="425cb-160">En plus de fournir un événement "OnSuccess", l’objet AjaxOptions expose OnBegin, OnFailure, et OnComplete événements que vous pouvez gérer (avec une variété d’autres propriétés et des options utiles).</span><span class="sxs-lookup"><span data-stu-id="425cb-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="425cb-161">Nettoyage - Refactor out a RSVP Partial View</span><span class="sxs-lookup"><span data-stu-id="425cb-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="425cb-162">Notre modèle de vue des détails commence à être un peu long, ce qui rendra les heures supplémentaires un peu plus difficiles à comprendre.</span><span class="sxs-lookup"><span data-stu-id="425cb-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="425cb-163">Pour aider à améliorer la lisibilité du code, finissons-en en créant une vue partielle - RSVPStatus.ascx - qui résume tout le code de vue RSVP pour notre page Détails.</span><span class="sxs-lookup"><span data-stu-id="425cb-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="425cb-164">Nous pouvons le faire en cliquant à droite sur le dossier «Vues-Dîners», puis en choisissant la commande de menu Add-View.&gt;</span><span class="sxs-lookup"><span data-stu-id="425cb-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="425cb-165">Nous allons le faire prendre un objet dîner comme son ViewModel fortement typé.</span><span class="sxs-lookup"><span data-stu-id="425cb-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="425cb-166">Nous pouvons ensuite copier /coller le contenu RSVP de notre vue Details.aspx en elle.</span><span class="sxs-lookup"><span data-stu-id="425cb-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="425cb-167">Une fois que nous avons fait cela, créons également une autre vue partielle - EditAndDeleteLinks.ascx - qui résume notre code de vue de lien Edit and Delete.</span><span class="sxs-lookup"><span data-stu-id="425cb-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="425cb-168">Nous allons également l’avoir prendre un objet de dîner comme son ViewModel fortement typé, et copier / coller la logique d’édition et de suppression de notre vue Details.aspx en elle.</span><span class="sxs-lookup"><span data-stu-id="425cb-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="425cb-169">Notre modèle de vue des détails peut alors simplement inclure deux appels de méthode Html.RenderPartial() en bas :</span><span class="sxs-lookup"><span data-stu-id="425cb-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="425cb-170">Cela rend le code plus propre à lire et à entretenir.</span><span class="sxs-lookup"><span data-stu-id="425cb-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="425cb-171">étape suivante</span><span class="sxs-lookup"><span data-stu-id="425cb-171">Next Step</span></span>

<span data-ttu-id="425cb-172">Examinons maintenant comment nous pouvons utiliser AJAX encore plus loin et ajouter un support de cartographie interactif à notre application.</span><span class="sxs-lookup"><span data-stu-id="425cb-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="425cb-173">[Suivant précédent](secure-applications-using-authentication-and-authorization.md)
> [Next](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="425cb-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
