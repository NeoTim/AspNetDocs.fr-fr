---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Lancement d’une fenêtre contextuelle modale à partir du code serveur (VB) | Microsoft Docs
author: wenz
description: Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client. Toutefois, certains scénarios requièrent que t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 1368a78d35ac6461bbc2e852e468f42eef2c0d2c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613273"
---
# <a name="launching-a-modal-popup-window-from-server-code-vb"></a>Lancement d’une fenêtre contextuelle modale à partir de code serveur (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)

> Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client. Toutefois, certains scénarios requièrent que l’ouverture de la fenêtre contextuelle modale soit déclenchée côté serveur.

## <a name="overview"></a>Présentation

Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client. Toutefois, certains scénarios requièrent que l’ouverture de la fenêtre contextuelle modale soit déclenchée côté serveur.

## <a name="steps"></a>Étapes

Tout d’abord, un contrôle Web ASP.NET Button est requis pour illustrer le fonctionnement du contrôle ModalPopup. Ajoutez un bouton de ce type dans le &lt;formulaire&gt; élément sur une nouvelle page :

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

Ensuite, vous avez besoin du balisage pour la fenêtre contextuelle que vous souhaitez créer. Définissez-le en tant que contrôle de `<asp:Panel>` et assurez-vous qu’il comprend un contrôle bouton. Le contrôle ModalPopup offre la fonctionnalité permettant de fermer un bouton de ce type dans la fenêtre contextuelle. dans le cas contraire, il n’existe aucun moyen simple de la laisser disparaître.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

Ajoutez ensuite le contrôle ModalPopup de ASP.NET AJAX Toolkit à la page. Définissez les propriétés du bouton qui charge le contrôle, le bouton qui le fait disparaître et l’ID de la fenêtre contextuelle réelle.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

Comme avec toutes les pages Web basées sur ASP.NET AJAX ; le gestionnaire de scripts est nécessaire pour charger les bibliothèques JavaScript nécessaires pour les différents navigateurs cibles :

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

Exécutez l’exemple dans le navigateur. Lorsque vous cliquez sur le bouton, la fenêtre contextuelle modale s’affiche. Pour obtenir le même effet à l’aide du code côté serveur, un nouveau bouton est requis :

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

Comme vous pouvez le voir, un clic sur le bouton génère une publication et exécute la méthode `ServerButton_Click()` sur le serveur. Dans cette méthode, une fonction JavaScript appelée `launchModal()` est exécutée comme exacte, la fonction JavaScript est exécutée une fois que la page a été chargée :

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

La tâche de `launchModal()` consiste à afficher ModalPopup. La fonction `launchModal()` est exécutée une fois que la page HTML complète a été chargée. Toutefois, à ce stade, l’infrastructure AJAX ASP.NET n’a pas encore été entièrement chargée. Par conséquent, la fonction `launchModal()` définit simplement une variable sur laquelle le contrôle ModalPopup doit être affiché plus tard :

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

La fonction JavaScript `pageLoad()` est une fonction spéciale qui est exécutée une fois ASP.NET AJAX entièrement chargé. Par conséquent, nous ajoutons du code à cette fonction pour afficher le contrôle ModalPopup, mais uniquement si `launchModal()` a été appelée avant :

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

La fonction `$find()` recherche un élément nommé sur la page et attend l’ID côté serveur en tant que paramètre. Par conséquent, `$find("mpe")` retourne la représentation cliente du contrôle ModalPopup ; sa méthode `show()` permet d’afficher la fenêtre contextuelle.

[![la fenêtre contextuelle modale s’affiche lorsque vous cliquez sur l’un des boutons](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)

La fenêtre contextuelle modale s’affiche lorsque l’utilisateur clique sur l’un des boutons ([cliquez pour afficher l’image en taille réelle](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](positioning-a-modalpopup-cs.md)
> [Suivant](using-modalpopup-with-a-repeater-control-vb.md)
