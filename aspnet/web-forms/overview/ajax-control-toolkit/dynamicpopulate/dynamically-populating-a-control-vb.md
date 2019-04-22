---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Remplissage dynamique d’un contrôle (VB) | Microsoft Docs
author: wenz
description: Le contrôle de DynamicPopulate dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible sur t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c9fdbe5f0e24aa3f09f11a67c6d13a32897e8b85
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59388372"
---
# <a name="dynamically-populating-a-control-vb"></a>Remplissage dynamique d’un contrôle (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)

> Le contrôle DynamicPopulate dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible dans la page, sans une actualisation de la page.


## <a name="overview"></a>Vue d'ensemble

Le `DynamicPopulate` contrôle dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible dans la page, sans une actualisation de la page. Ce didacticiel montre comment configurer ce paramètre.

## <a name="steps"></a>Étapes

Tout d’abord, vous avez besoin d’un Service Web de ASP.NET qui implémente la méthode doit être appelée par `DynamicPopulate`. La classe de service web requiert le `ScriptService` attribut qui est défini dans `Microsoft.Web.Script.Services`; sinon ASP.NET AJAX ne peut pas créer le proxy JavaScript côté client pour le service web, ce qui à son tour, est requis par `DynamicPopulate`.

La méthode web doit attendre un argument de type chaîne, appelée `contextKey`, dans la mesure où le `DynamicPopulate` contrôle envoie une information de contexte à chaque appel de service web. Le service web suivant retourne la date actuelle dans un format représenté par le `contextKey` argument :

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

Le service web est ensuite enregistré en tant que `DynamicPopulate.vb.asmx`. Vous pouvez également implémenter le `getDate()` méthode comme une méthode de page au sein de la page ASP.NET avec le `DynamicPopulate` contrôle.

Dans l’étape suivante, créez un nouveau fichier ASP.NET. Comme toujours, la première étape consiste à inclure le `ScriptManager` dans la page actuelle pour charger la bibliothèque AJAX ASP.NET et pour rendre le travail de la boîte à outils de contrôle :

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

Ensuite, ajoutez un contrôle d’étiquette (par exemple en utilisant le contrôle HTML du même nom, ou le &lt; `asp:Label`  / &gt; contrôle web) qui affiche plus tard le résultat de l’appel de service web.

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

Un bouton HTML (comme un contrôle HTML, étant donné que nous ne nécessitent pas une publication (postback) sur le serveur) sera ensuite servir à déclencher le remplissage dynamique :

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

Enfin, nous devons le `DynamicPopulateExtender` contrôle pour associer les choses. Les attributs suivants seront définis (en dehors de celles évident, `ID` et `runat` = `"server"`) :

- `TargetControlID` où placer le résultat de l’appel de service web
- `ServicePath` chemin d’accès au service web (omettre si vous souhaitez utiliser une méthode de page)
- `ServiceMethod` nom de la méthode web ou d’une méthode de page
- `ContextKey` informations de contexte à envoyer au service web
- `PopulateTriggerControlID` élément qui déclenche l’appel de service web
- `ClearContentsDuringUpdate` s’il faut vide de l’élément cible lors de l’appel de service web

Comme vous pouvez le voir, le contrôle nécessite des informations mais tout ce que la mise en place est très simple. Voici le balisage pour le `DynamicPopulateExtender` contrôle dans le scénario en cours :

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

Exécuter la page ASP.NET dans le navigateur, puis cliquez sur le bouton ; Vous recevrez la date actuelle au format de mois-jour-année.


[![Un clic sur le bouton récupère la date à partir du serveur](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)

Un clic sur le bouton récupère la date à partir du serveur ([cliquez pour afficher l’image en taille réelle](dynamically-populating-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [Suivant](dynamically-populating-a-control-using-javascript-code-vb.md)
