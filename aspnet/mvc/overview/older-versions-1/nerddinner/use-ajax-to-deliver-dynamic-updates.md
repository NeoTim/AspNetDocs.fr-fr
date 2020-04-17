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
# <a name="use-ajax-to-deliver-dynamic-updates"></a>Utiliser AJAX pour fournir des mises à jour dynamiques

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 10 d’un tutoriel gratuit ["NerdDinner" application](introducing-the-nerddinner-tutorial.md) qui marche à travers la façon de construire une petite, mais complète, application web en utilisant ASP.NET MVC 1.
> 
> L’étape 10 met en œuvre le soutien aux utilisateurs connectés à RSVP leur intérêt à assister à un dîner, en utilisant une approche basée sur Ajax intégrée dans la page des détails du dîner.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les tutoriels [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner Step 10: AJAX Enabling RSVPs Accepte

Mettons maintenant en œuvre le soutien aux utilisateurs connectés à RSVP leur intérêt à assister à un dîner. Nous allons activer cela en utilisant une approche basée sur AJAX intégrée dans la page de détails du dîner.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Indiquer si l’utilisateur est RSVP’d

Les utilisateurs peuvent visiter l’URL */Dinners/Details/[id]* pour voir les détails d’un dîner particulier :

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

La méthode d’action Détails() est mise en œuvre comme telle:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Notre première étape pour mettre en œuvre le support RSVP sera d’ajouter une méthode d’aide "IsUserRegistered (username)" à notre objet Dinner (dans la classe partielle Dinner.cs que nous avons construite plus tôt). Cette méthode d’aide revient vrai ou faux selon que l’utilisateur est actuellement RSVP’d pour le dîner:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

Nous pouvons ensuite ajouter le code suivant à notre modèle de vue Details.aspx pour afficher un message approprié indiquant si l’utilisateur est enregistré ou non pour l’événement :

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

Et maintenant, quand un utilisateur visite un dîner, ils sont inscrits pour qu’ils voient ce message:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

Et quand ils visitent un dîner, ils ne sont pas inscrits pour ils verront le message ci-dessous:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Mise en œuvre de la méthode d’action du registre

Ajoutons maintenant les fonctionnalités nécessaires pour permettre aux utilisateurs de RSVP pour un dîner à partir de la page détails.

Pour mettre en œuvre cela, nous allons créer une nouvelle classe "RSVPController" en&gt;cliquant à droite sur le répertoire des contrôleurs et en choisissant la commande de menu Add-Controller.

Nous metndons en œuvre une méthode d’action «Register» au sein de la nouvelle classe RSVPController qui prend un id pour un dîner comme argument, récupère l’objet de dîner approprié, vérifie si l’utilisateur connecté est actuellement dans la liste des utilisateurs qui se sont inscrits pour elle, et si ce n’est ajoute un objet RSVP pour eux:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Remarquez ci-dessus comment nous retournons une chaîne simple comme la sortie de la méthode d’action. Nous aurions pu intégrer ce message dans un modèle de vue , mais comme il est si petit, nous allons simplement utiliser la méthode d’aide content() sur la classe de base du contrôleur et retourner un message de chaîne comme ci-dessus.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Appel à la méthode d’action RSVPForEvent en utilisant AJAX

Nous utiliserons AJAX pour invoquer la méthode d’action Du Registre de notre point de vue Détails. Mettre en œuvre cela est assez facile. Tout d’abord, nous allons ajouter deux références de bibliothèque de script:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

La première bibliothèque fait référence au noyau ASP.NET bibliothèque de scripts côté client AJAX. Ce fichier est d’environ 24k de taille (compressé) et contient la fonctionnalité DE base DU client AJAX. La deuxième bibliothèque contient des fonctions utilitaires qui s’intègrent aux méthodes d’aide AJAX intégrées de ASP.NET MVC (que nous utiliserons sous peu).

Nous pouvons ensuite mettre à jour le code de modèle de vue que nous avons ajouté plus tôt de sorte qu’au lieu de sortir un message "Vous n’êtes pas inscrit pour cet événement", nous rendons plutôt un lien qui, lorsqu’il est poussé effectue un appel AJAX qui invoque notre méthode d’action RSVPForEvent sur notre contrôleur RSVP et RSVPs l’utilisateur:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

La méthode d’aide Ajax.ActionLink () utilisée ci-dessus est intégrée ASP.NET MVC et est similaire à la méthode d’aide Html.ActionLink(), sauf qu’au lieu d’effectuer une navigation standard, il fait un appel AJAX à la méthode d’action lorsque le lien est cliqué. Ci-dessus, nous appelons la méthode d’action "Register" sur le contrôleur "RSVP" et de passer le DinnerID comme le paramètre "id" à elle. Le paramètre Final AjaxOptions que nous passons indique que nous voulons &lt;prendre&gt; le contenu retourné de la méthode d’action et mettre à jour l’élément div HTML sur la page dont l’id est "rsvpmsg".

Et maintenant, quand un utilisateur navigue à un dîner, ils ne sont pas inscrits pour encore ils vont voir un lien vers RSVP pour elle:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

S’ils cliquent sur le lien "RSVP pour cet événement", ils feront un appel AJAX à la méthode d’action Du Registre sur le contrôleur RSVP, et quand il se termine, ils verront un message mis à jour comme ci-dessous:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

La bande passante réseau et le trafic impliqué lors de la prise de cet appel AJAX est vraiment léger. Lorsque l’utilisateur clique sur le lien "RSVP pour cet événement", une petite demande de réseau HTTP POST est faite à l’URL */Dîners/Register/1* qui ressemble ci-dessous sur le fil:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

Et la réponse de notre méthode d’action Registre est simplement:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Cet appel léger est rapide et fonctionnera même sur un réseau lent.

### <a name="adding-a-jquery-animation"></a>Ajout d’une animation jQuery

La fonctionnalité AJAX que nous avons implémentée fonctionne bien et rapidement. Parfois, il peut arriver si vite, cependant, qu’un utilisateur pourrait ne pas remarquer que le lien RSVP a été remplacé par un nouveau texte. Pour rendre le résultat un peu plus évident, nous pouvons ajouter une animation simple pour attirer l’attention sur le message de mise à jour.

Le modèle de projet MVC par défaut ASP.NET comprend jQuery , une excellente (et très populaire) bibliothèque Open Source JavaScript qui est également pris en charge par Microsoft. jQuery fournit un certain nombre de fonctionnalités, y compris une belle sélection HTML DOM et la bibliothèque d’effets.

Pour utiliser jQuery nous allons d’abord ajouter une référence script à elle. Parce que nous allons utiliser jQuery dans une variété d’endroits dans notre site, nous allons ajouter la référence de script dans notre fichier de page principale Site.master afin que toutes les pages peuvent l’utiliser.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Astuce: assurez-vous d’avoir installé le hotfix JavaScript intellisense pour VS 2008 SP1 qui permet un support intellisense plus riche pour les fichiers JavaScript (y compris jQuery). Vous pouvez le télécharger à partir de:http://tinyurl.com/vs2008javascripthotfix*

Code écrit à l’aide de JQuery utilise souvent une méthode JavaScript globale qui récupère un ou plusieurs éléments HTML à l’aide d’un sélecteur CSS. Par exemple, *$(« #rsvpmsg »)* sélectionne n’importe quel élément HTML avec l’id de rsvpmsg, tandis que *$(".quelque chose")* sélectionnerait tous les éléments avec le nom de classe CSS "quelque chose". Vous pouvez également écrire des requêtes plus avancées comme « retourner tous les boutons radio vérifiés » à l’aide d’une requête de sélecteur comme : *$ («@typeentrée[radio][]«@checked)*.

Une fois que vous avez sélectionné des éléments, vous pouvez appeler des méthodes sur eux pour agir, comme les cacher: *$("#rsvpmsg").hide();*

Pour notre scénario RSVP, nous définirons une fonction JavaScript simple nommée "AnimateRSVPMessage" qui &lt;sélectionne&gt; le div "rsvpmsg" et anime la taille de son contenu texte. Le code ci-dessous démarre le texte petit et le fait ensuite augmenter sur un délai de 400 millisecondes:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Nous pouvons ensuite mettre en écoute cette fonction JavaScript à appeler après notre appel AJAX avec succès en passant son nom à notre Ajax.ActionLink () méthode d’aide (via l’AjaxOptions "OnSuccess" propriété événement):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

Et maintenant, lorsque le lien "RSVP pour cet événement" est cliqué et que notre appel AJAX se termine avec succès, le message de contenu renvoyé animera et grandira:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

En plus de fournir un événement "OnSuccess", l’objet AjaxOptions expose OnBegin, OnFailure, et OnComplete événements que vous pouvez gérer (avec une variété d’autres propriétés et des options utiles).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Nettoyage - Refactor out a RSVP Partial View

Notre modèle de vue des détails commence à être un peu long, ce qui rendra les heures supplémentaires un peu plus difficiles à comprendre. Pour aider à améliorer la lisibilité du code, finissons-en en créant une vue partielle - RSVPStatus.ascx - qui résume tout le code de vue RSVP pour notre page Détails.

Nous pouvons le faire en cliquant à droite sur le dossier «Vues-Dîners», puis en choisissant la commande de menu Add-View.&gt; Nous allons le faire prendre un objet dîner comme son ViewModel fortement typé. Nous pouvons ensuite copier /coller le contenu RSVP de notre vue Details.aspx en elle.

Une fois que nous avons fait cela, créons également une autre vue partielle - EditAndDeleteLinks.ascx - qui résume notre code de vue de lien Edit and Delete. Nous allons également l’avoir prendre un objet de dîner comme son ViewModel fortement typé, et copier / coller la logique d’édition et de suppression de notre vue Details.aspx en elle.

Notre modèle de vue des détails peut alors simplement inclure deux appels de méthode Html.RenderPartial() en bas :

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Cela rend le code plus propre à lire et à entretenir.

### <a name="next-step"></a>étape suivante

Examinons maintenant comment nous pouvons utiliser AJAX encore plus loin et ajouter un support de cartographie interactif à notre application.

> [!div class="step-by-step"]
> [Suivant précédent](secure-applications-using-authentication-and-authorization.md)
> [Next](use-ajax-to-implement-mapping-scenarios.md)
