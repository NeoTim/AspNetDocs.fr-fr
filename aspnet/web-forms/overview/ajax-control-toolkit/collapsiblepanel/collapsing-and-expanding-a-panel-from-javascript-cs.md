---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Réduction et développement d’un panneau à partir deC#JavaScript () | Microsoft Docs
author: wenz
description: Le contrôle CollapsiblePanel dans la boîte à outils de contrôle ASP.NET AJAX étend un panneau et lui offre la possibilité de réduire son contenu et de le développer...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: bed14d82394d28336493bec10e31ddb4d411a192
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614155"
---
# <a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>Réduction et développement d’un panneau à partir de JavaScript (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> Le contrôle CollapsiblePanel dans la boîte à outils de contrôle ASP.NET AJAX étend un panneau et lui offre la possibilité de réduire son contenu et de le développer à nouveau. Ces deux actions peuvent également être déclenchées à partir de code JavaScript personnalisé.

## <a name="overview"></a>Présentation

Le contrôle CollapsiblePanel dans la boîte à outils de contrôle ASP.NET AJAX étend un panneau et lui offre la possibilité de réduire son contenu et de le développer à nouveau. Ces deux actions peuvent également être déclenchées à partir de code JavaScript personnalisé.

## <a name="steps"></a>Étapes

Tout d’abord, créez une nouvelle page ASP.NET et incluez la `ScriptManager` dans l’élément `<form>`. Cela charge la bibliothèque AJAX ASP.NET requise par Control Toolkit :

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

Ensuite, créez un panneau avec du texte afin que l’effet de réduction/développement puisse être affiché :

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

Comme vous pouvez le voir, le panneau fait référence à une classe CSS qui est présentée ici (et définit fondamentalement une couleur d’arrière-plan et la largeur du panneau) :

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

Le contrôle `CollapsiblePanelExtender` nécessite l’attribut `TargetControlID` afin que la boîte à outils sache quel volet réduire ou développer à la demande :

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

Malheureusement, l’extendeur n’expose pas actuellement une API spécifique pour réduire ou développer le panneau, mais certaines méthodes non documentées en font. Tout d’abord, ajoutez trois boutons HTML à la page, ce qui déclenchera le JavaScript côté client pour réduire ou développer le contenu du panneau :

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

Dans le code JavaScript côté client (démarré avec `<script type="text/javascript">`), la méthode `$find()` doit être utilisée pour accéder à la `CollapsiblePanelExtender`. `$find("cpe")` retourne une référence à celui-ci. À partir de là, des méthodes spécifiques résolvent la tâche en cours.

La méthode permettant d’ouvrir (développer) le panneau est appelée `_doOpen()`; le code suivant implémente la fonction `doOpen()` appelée suite à un clic sur le premier bouton :

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

Pour fermer ou réduire le panneau, la méthode `_doClose()` doit être exécutée. Ainsi, quand l’utilisateur clique sur le deuxième bouton, le code JavaScript suivant est appelé :

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

Le troisième bouton bascule l’état du panneau : de réduit à développé et vice versa. L' `CollapsiblePanelExtender` expose la méthode `toggle()` qui effectue exactement ce qui suit : inverse l’état du panneau. Toutefois, il existe également une autre approche (qui est utilisée en interne par la méthode `toggle()`) : la méthode `get_Collapsed()` de la `CollapsiblePanelExtender()` indique si le panneau est réduit ou non. Selon la valeur de retour de cette fonction, le panneau est ensuite développé (méthode`_doOpen()`) ou méthode réduite (`_doClose()`) :

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]

[![le troisième bouton modifie l’état du panneau : de réduit à développé et de retour](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

Le troisième bouton modifie l’état du panneau : de réduit à développé et de retour ([cliquez pour afficher l’image en taille réelle](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](collapsing-and-expanding-a-panel-from-javascript-vb.md)
