---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Gestion des publications (postback) à partir d’un ModalPopup (VB) | Microsoft Docs
author: wenz
description: Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client. Une attention particulière doit entreprendre lorsqu’un pos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 3c1951e1ae4f97982d1263dfa9dc29454f7ce55a
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65132677"
---
# <a name="handling-postbacks-from-a-modalpopup-vb"></a>Gestion des publications (postback) à partir d’un ModalPopup (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)

> Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client. Une attention particulière doit prendre lorsqu’une publication (postback) est créé à partir de la fenêtre contextuelle.

## <a name="overview"></a>Vue d'ensemble

Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client. Une attention particulière doit prendre lorsqu’une publication (postback) est créé à partir de la fenêtre contextuelle.

## <a name="steps"></a>Étapes

Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

Ensuite, ajoutez un panneau qui sert de la fenêtre contextuelle modale. Là, l’utilisateur peut entrer un nom et une adresse de messagerie. Un bouton est utilisé pour fermer la fenêtre contextuelle et enregistrer les informations. Notez que le `OnClick` attribut a la valeur afin qu’une publication (postback) se produit lorsqu’un clic sur ce bouton :

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

La page elle-même se compose de deux étiquettes exactement les mêmes informations : nom et adresse de messagerie. Un bouton est utilisé pour déclencher la fenêtre contextuelle modale :

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

Pour que la fenêtre contextuelle s’affiche, ajoutez le `ModalPopupExtender` contrôle. Définir le `PopupControlID` ID du panneau par d’attribut et `TargetControlID` à l’ID du bouton :

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

Maintenant chaque fois que le `Save` dans la fenêtre contextuelle modale avec le bouton est cliqué, côté serveur `SaveData()` méthode est exécutée. Là, vous pouvez enregistrer les données entrées dans un magasin de données. Par souci de simplicité, les nouvelles données sont tout simplement sorties dans l’étiquette :

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

En outre, les contrôles de zone de texte dans la fenêtre contextuelle modale doivent être remplis avec le nom et le courrier électronique. Toutefois cela est nécessaire uniquement lorsque aucune publication (postback) se produit. En l’absence d’une publication (postback), la fonctionnalité de viewstate ASP.NET remplira automatiquement les zones de texte avec les valeurs appropriées.

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]

[![La fenêtre contextuelle modale entraîne une publication](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)

La fenêtre contextuelle modale entraîne une publication ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](using-modalpopup-with-a-repeater-control-vb.md)
> [Suivant](positioning-a-modalpopup-vb.md)
