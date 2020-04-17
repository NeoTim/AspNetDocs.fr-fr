---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: Utilisation de l’extension de contrôle ColorPicker (VB) Microsoft Docs
author: rick-anderson
description: ColorPicker est un extenseur AJAX ASP.NET qui fournit la fonctionnalité de color-picking côté client avec interface utilisateur dans un contrôle de popup. Il peut être attaché à n’importe quel ASP.NET...
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: c373102665ac17020ca8d5f14d3488a874bb68de
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542844"
---
# <a name="using-the-colorpicker-control-extender-vb"></a>Utilisation de l’extension de contrôle ColorPicker (VB)

par [Microsoft](https://github.com/microsoft)

> ColorPicker est un extenseur AJAX ASP.NET qui fournit la fonctionnalité de color-picking côté client avec interface utilisateur dans un contrôle de popup. Il peut être attaché à n’importe quel contrôle textbox ASP.NET. Il.

Le but de ce tutoriel est d’expliquer comment vous pouvez utiliser l’extension de contrôle AJAX Toolkit ColorPicker. L’extenseur de contrôle ColorPicker affiche un dialogue popup qui vous permet de sélectionner une couleur. Le ColorPicker est utile chaque fois que vous voulez fournir une interface utilisateur intuitive pour un utilisateur de choisir une couleur.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Étendre un contrôle TextBox avec l’extension de contrôle ColorPicker

Imaginez, par exemple, que vous souhaitez créer un site Web qui permet aux visiteurs de créer des cartes de visite personnalisées. Les visiteurs peuvent entrer le texte pour une carte de visite et choisir la couleur. La page ASP.NET de La liste 1 contient deux commandes TextBox nommées txtCardText et txtCardColor. Lorsque vous soumettez le formulaire, les valeurs sélectionnées sont affichées (voir la figure 1).

[![Formulaire simple pour créer une carte de visite](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**Figure 01**: Formulaire simple pour créer une carte de visite ([Cliquez pour voir l’image grandeur nature](using-the-colorpicker-control-extender-vb/_static/image2.png))

**Liste 1 - CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

Le formulaire dans La liste 1 fonctionne, mais il ne fournit pas une grande expérience utilisateur. L’utilisateur doit taper une couleur dans la boîte à texte. Si l’utilisateur veut une couleur spécialisée - par exemple, juste la bonne nuance de vert pois - alors l’utilisateur doit comprendre le code couleur HTML sans aucune aide.

Vous pouvez utiliser l’extenseur de contrôle ColorPicker pour créer une meilleure expérience utilisateur. Le ColorPicker affiche un dialogue de couleur lorsque vous déplacez la mise au point vers un contrôle TextBox (voir la figure 2).

[![L’extension de contrôle ColorPicker](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**Figure 02**: The ColorPicker Control Extender ([Cliquez pour voir l’image grandeur nature](using-the-colorpicker-control-extender-vb/_static/image4.png))

Vous devez effectuer deux étapes pour utiliser l’extenseur de contrôle ColorPicker avec le formulaire dans la liste 1 :

1. Ajouter un contrôle ScriptManager à la page
2. Ajouter l’extenseur de contrôle ColorPicker à la page

Avant de pouvoir utiliser le ColorPicker, vous devez ajouter un ScriptManager à votre page. Un bon endroit pour ajouter le ScriptManager est &lt;&gt; juste en dessous de l’étiquette de formulaire serveur d’ouverture. Vous pouvez faire glisser le ScriptManager sur la page à partir de la boîte à outils (le ScriptManager est situé sous l’onglet EXTENSIONs AJAX). Alternativement, vous pouvez taper l’étiquette suivante dans Source View sous l’étiquette de formulaire côté serveur d’ouverture :

&lt;asp:ScriptManager ID"ScriptManager1" runat"server" /&gt;

La façon la plus simple d’ajouter l’extension de contrôle ColorPicker à la page est dans Design View. Si vous planez votre souris au-dessus de la TextBox txtCardColor, une option de tâche intelligente apparaît qui vous permet d’ajouter un extenseur (voir la figure 3). Si vous choisissez cette option, l’Extender Wizard apparaît (voir la figure 4).

[![Ajout d’un extenseur](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**Figure 03**: Ajout d’un extenseur[(Cliquez pour voir l’image grandeur nature](using-the-colorpicker-control-extender-vb/_static/image6.png))

[![Sélection d’un extenseur de contrôle avec le Assistant Extender](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**Figure 04**: Sélection d’un extenseur de contrôle avec le Assistant Extender ([Cliquez pour voir l’image grandeur nature](using-the-colorpicker-control-extender-vb/_static/image8.png))

Vous pouvez choisir l’extenseur ColorPicker pour étendre la textBox txtCardColor avec l’extenseur ColorPicker. Cliquez sur OK pour fermer la boîte de dialogue.

Après avoir apporté ces modifications, la source de la page ressemble à la liste 2.

**Liste 2 - CreateCard.aspx (avec ColorPicker)**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

Notez que la page contient maintenant un contrôle ColorPickerExtender qui apparaît directement en dessous du contrôle txtCardColor TextBox. Le contrôle ColorPickerExtender étend le contrôle txtCardColor de sorte qu’il affiche un dialogue de cueilleur de couleur.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Utilisation d’un bouton pour lancer le dialogue Color Picker

L’extenseur ColorPicker prend en charge les propriétés suivantes :

- PopupButtonId - L’ID d’un bouton sur la page qui provoque le dialogue cueilleur de couleur à apparaître.
- PopupPosition - La position, par rapport au contrôle de la cible, du dialogue de cueilleur de couleurs. Les valeurs possibles sont Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right et Left (la valeur par défaut est BottomLeft).
- SampleControlId - L’ID d’un contrôle qui affiche la couleur sélectionnée.
- SelectedColor - La couleur initiale sélectionnée par le ColorPicker.

Vous pouvez utiliser ces propriétés pour personnaliser la façon dont le dialogue de cueilleur de couleurs est affiché et comment la couleur sélectionnée est affichée. La page de La liste 3 illustre comment vous pouvez utiliser plusieurs de ces propriétés.

**Liste 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

La page de liste 3 comprend un bouton Pick Color (voir la figure 5). Lorsque vous cliquez sur ce bouton, le dialogue de cueilleur de couleurs apparaît au-dessus de la TextBox. Si vous sélectionnez une couleur du dialogue, la couleur sélectionnée apparaît comme la couleur de fond du contrôle de l’étiquette lblSample.

La propriété ColorPicker PopupButtonID est utilisée pour associer le bouton Pick Color à l’extenseur ColorPicker. Lorsque vous fournissez une valeur pour la propriété PopupButtonID, le dialogue de cueilleur de couleurs n’apparaît plus lorsque le contrôle cible a la concentration. Vous devez cliquer sur le bouton pour afficher le dialogue.

La propriété SampleControlID est utilisée pour associer un contrôle qui affiche la couleur sélectionnée avec le ColorPicker. Le ColorPicker change la couleur de fond de ce contrôle à la couleur actuellement sélectionnée.

[![Affichage du dialogue de cueilleur de couleur avec un bouton](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**Figure 05**: Affichage du dialogue de cueilleur de couleurs avec un bouton ([Cliquez pour voir l’image grandeur nature](using-the-colorpicker-control-extender-vb/_static/image10.png))

## <a name="summary"></a>Récapitulatif

Dans ce tutoriel, vous avez appris à utiliser l’extension de contrôle ColorPicker pour afficher un dialogue popup color picker. Tout d’abord, nous avons examiné comment vous pouvez afficher le dialogue lorsque l’accent est déplacé vers un contrôle TextBox. Ensuite, vous avez appris à créer un bouton qui affiche le dialogue de cueilleur de couleurs lorsque le bouton est cliqué.

> [!div class="step-by-step"]
> [Précédent](using-the-colorpicker-control-extender-cs.md)
