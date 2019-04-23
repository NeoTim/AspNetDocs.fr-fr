---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Réduction et développement d’un panneau à partir de JavaScript (c#) | Microsoft Docs
author: wenz
description: Le contrôle CollapsiblePanel dans ASP.NET AJAX Control Toolkit étend un panneau et lui fournit la fonctionnalité pour réduire son contenu et le développer un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 157a486af3d11dfbd7431680b6c9fe4f0e262892
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59422393"
---
# <a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>Réduction et développement d’un panneau à partir de JavaScript (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> Le contrôle CollapsiblePanel dans ASP.NET AJAX Control Toolkit étend un panneau et lui fournit la fonctionnalité pour réduire son contenu et le développer à nouveau. Ces deux actions peuvent également être déclenchées à partir de code JavaScript personnalisé.


## <a name="overview"></a>Vue d'ensemble

Le contrôle CollapsiblePanel dans ASP.NET AJAX Control Toolkit étend un panneau et lui fournit la fonctionnalité pour réduire son contenu et le développer à nouveau. Ces deux actions peuvent également être déclenchées à partir de code JavaScript personnalisé.

## <a name="steps"></a>Étapes

Tout d’abord, créez une page ASP.NET et inclure le `ScriptManager` dans celui `<form>` élément. Cela charge la bibliothèque ASP.NET AJAX, ce qui est requis par les outils de contrôle :

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

Ensuite, créez un panneau avec du texte, afin de pouvoir l’effet de réduction/développement peut être :

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

Comme vous pouvez le voir, le panneau de configuration fait référence à une classe CSS qui est indiquée ici (et fondamentalement définit une couleur d’arrière-plan et la largeur du panneau) :

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

Le `CollapsiblePanelExtender` contrôle requiert le `TargetControlID` attribut afin que le Kit de ressources sache quel panneau pour réduire ou développer à la demande :

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

Malheureusement, l’extendeur actuellement n’expose pas une API spécifique pour réduire ou de développer le panneau de configuration, mais certaines méthodes non documentées effectuera. Tout d’abord, ajoutez trois boutons HTML à la page qui déclenchera le JavaScript côté client pour réduire ou développer le contenu du panneau :

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

Dans le code JavaScript côté client (Démarrer avec `<script type="text/javascript">`), la `$find()` méthode doit être utilisée pour accéder à la `CollapsiblePanelExtender`. `$find("cpe")` Retourne une référence à celui-ci. Depuis lors, des méthodes spécifiques permettent de résoudre la tâche en cours.

La méthode d’ouverture (développement) le panneau de configuration est appelé `_doOpen()`; le code suivant implémente la `doOpen()` fonction appelée lorsque l’utilisateur clique sur le premier bouton :

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

Pour une fermeture ou réduire le panneau de configuration, le `_doClose()` méthode doit être exécutée. Par conséquent, lorsque l’utilisateur clique sur le deuxième bouton, le code JavaScript suivant est appelé :

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

Le troisième bouton bascule l’état du panneau : à partir de réduits à développée et vice versa. Le `CollapsiblePanelExtender` expose le `toggle()` méthode qui est exactement ce que : inverse l’état du panneau. Toutefois, il est également une autre approche (qui est utilisé en interne par le `toggle()` (méthode)) : Le `get_Collapsed()` méthode de la `CollapsiblePanelExtender()` nous indique si le panneau est réduit ou non. Selon la valeur de retour de cette fonction, le panneau est ensuite soit développé (`_doOpen()` méthode) ou réduit (`_doClose()`) méthode :

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


[![Le troisième bouton modifie l’état du panneau : à partir de réduits à développée et précédent](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

Le troisième bouton modifie l’état du panneau : à partir de réduits à développée et précédent ([cliquez pour afficher l’image en taille réelle](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](collapsing-and-expanding-a-panel-from-javascript-vb.md)
