---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Gestion des publications (postback) à partir d’un contrôle Popup sans un UpdatePanel (c#) | Microsoft Docs
author: wenz
description: L’extendeur PopupControl dans les outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé. Lorsqu’une publication (postback) se produit en unités de recherche...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 3ad041b3df270e65c1dd39cf0cb6adb28b57880e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57036846"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>Gestion des publications (postback) à partir d’un contrôle de fenêtre contextuelle sans un UpdatePanel (C#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> L’extendeur PopupControl dans les outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé. Lorsqu’une publication (postback) se produit dans un tel panneau et il existe plusieurs panneaux sur la page, il est difficile de déterminer le panneau qui a été cliqué.


## <a name="overview"></a>Vue d'ensemble

L’extendeur PopupControl dans les outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé. Lorsqu’une publication (postback) se produit dans un tel panneau et il existe plusieurs panneaux sur la page, il est difficile de déterminer le panneau qui a été cliqué.

## <a name="steps"></a>Étapes

Lorsque vous utilisez un `PopupControl` avec une publication (postback), mais sans avoir un `UpdatePanel` dans la page, les outils de contrôle n’offre pas un moyen de déterminer quel élément client a déclenché la fenêtre contextuelle qui à son tour a provoqué la publication (postback). Toutefois, une petite astuce fournit une solution de contournement pour ce scénario.

Tout d’abord, voici la configuration de base : deux zones de texte qui tous deux de déclenchent la fenêtre contextuelle même, un calendrier. Deux `PopupControlExtenders` rassembler les zones de texte et la fenêtre contextuelle.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

L’idée de base consiste à ajouter un champ de formulaire masqué dans le &lt; `form` &gt; élément qui contient la zone de texte qui a lancé la fenêtre contextuelle :

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

Lorsque la page est chargée, le code JavaScript ajoute un gestionnaire d’événements pour les deux zones de texte : Chaque fois que l’utilisateur clique sur une zone de texte, son nom est écrit dans le champ de formulaire masqué :

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

Dans le code côté serveur, la valeur du champ masqué doit être lues. Dans la mesure où les champs de formulaire masqués sont faciles à manipuler, une approche de la liste verte pour valider la valeur masquée est requise. Une fois que la zone de texte correct a été identifiée, la date dans le calendrier est écrit dedans.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


[![Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))


[![En cliquant sur une date de la place dans la zone de texte](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

En cliquant sur une date de la place dans la zone de texte ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Précédent](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [Suivant](using-multiple-popup-controls-vb.md)
