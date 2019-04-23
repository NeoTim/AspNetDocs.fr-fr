---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Création de cases à cocher mutuellement exclusives (c#) | Microsoft Docs
author: wenz
description: 'Lorsque seul un ensemble d’options peuvent être sélectionnés, les cases d’option sont généralement utilisées. Il y a cependant un inconvénient : Une fois une case d’option dans un groupe est sélectionnée...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 01d6d2988278d3d371d93b23bbdf089d83900405
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59397849"
---
# <a name="creating-mutually-exclusive-checkboxes-c"></a>Création de cases à cocher mutuellement exclusives (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)

> Lorsque seul un ensemble d’options peuvent être sélectionnés, les cases d’option sont généralement utilisées. Il y a cependant un inconvénient : Une fois qu’une case d’option dans un groupe est sélectionnée, il n’est pas possible de décocher toutes les cases d’option. Cases à cocher peut être désactivées à tout moment, toutefois, ne sont pas mutuellement exclusives. Ce didacticiel offre le meilleur des deux approches : cases à cocher qui s’excluent mutuellement.


## <a name="overview"></a>Vue d'ensemble

Lorsque seul un ensemble d’options peuvent être sélectionnés, les cases d’option sont généralement utilisées. Il y a cependant un inconvénient : Une fois qu’une case d’option dans un groupe est sélectionnée, il n’est pas possible de décocher toutes les cases d’option. Cases à cocher peut être désactivées à tout moment, toutefois, ne sont pas mutuellement exclusives. Ce didacticiel offre le meilleur des deux approches : cases à cocher qui s’excluent mutuellement.

## <a name="steps"></a>Étapes

ASP.NET AJAX Control Toolkit contient les extendeurs MutuallyExclusiveCheckBox. Cela permet aux programmeurs d’affecter n’importe quel case à cocher à un nom de groupe (`Key` attribut). À partir de toutes les cases à cocher dans le même groupe, seul l’un peut être sélectionné à la fois.

Commençons par placer les deux cases à cocher sur une page ASP.NET. Il peut y avoir plus, mais deux d'entre eux suffit pour illustrer le principe :

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

Pour les deux cases à cocher, vous devez placer un contrôle MutuallyExclusiveCheckBoxExtender sur la page. Les deux attributs de clé doivent ont la même valeur, tout comme la valeur d’attributs des éléments de bouton de case d’option HTML doivent être identiques à indiquer le groupe qu'auquel ils appartiennent. La propriété TargetControlID de l’extendeur pointe vers l’ID de la case à cocher.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

Enfin, inclure ASP.NET AJAX `ScriptManager` qui est requis par tous les éléments d’ASP.NET AJAX Control Toolkit :

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

Enregistrez et exécutez la page : Vous pouvez vérifier et décochez les deux cases à cocher, toutefois à aucun moment peut les deux cases à cocher être vérifiées.


[![Case à cocher qu’une seule peut être vérifiée à la fois](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)

Case à cocher qu’une seule peut être vérifiée à la fois ([cliquez pour afficher l’image en taille réelle](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](creating-mutually-exclusive-checkboxes-vb.md)
