---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: Utilisation des commandes et des extensions de contrôle de la boîte à outils de contrôle AJAX (VB) Microsoft Docs
author: rick-anderson
description: Découvrez comment ajouter des commandes et des extensions AJAX Control Toolkit à vos pages ASP.NET.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: 129d6841bc4db62ecbe5f26c4830d1ce2f199bff
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540010"
---
# <a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a>Utilisation de contrôles AJAX Control Toolkit et extendeurs de contrôle (VB)

par [Microsoft](https://github.com/microsoft)

> Découvrez comment ajouter des commandes et des extensions AJAX Control Toolkit à vos pages ASP.NET.

La boîte à outils de contrôle AJAX contient un ensemble de commandes et d’extenseurs de contrôle. Dans ce bref tutoriel, vous apprenez à ajouter à la fois des contrôles et des extenseurs de contrôle à une page ASP.NET.

> [!NOTE] 
> 
> Pour obtenir des instructions sur l’installation de la boîte à outils de contrôle AJAX et l’ajout de la boîte à outils AJAX Control Toolkit à la boîte à outils Visual Studio/Visual Web Developer, consultez le tutoriel [Get Started avec la trousse de contrôle AJAX](get-started-with-the-ajax-control-toolkit-vb.md).

## <a name="using-ajax-control-toolkit-controls"></a>Utilisation des commandes de boîte à outils de contrôle AJAX

Un contrôle DE la boîte à outils AJAX fonctionne comme un contrôle ASP.NET normal. Vous pouvez faire glisser le contrôle de la boîte à outils sur une page ASP.NET. Vous pouvez ajouter le contrôle à la page dans la vue de conception ou la vue source.

Il y a une exigence spéciale lors de l’utilisation des commandes de la boîte à outils de contrôle AJAX. La page doit contenir un contrôle ScriptManager. Le contrôle ScriptManager est responsable d’inclure tous les JavaScript nécessaires requis par les commandes ajax Control Toolkit.

Par exemple, l’onglet AJAX Control Toolkit comprend un contrôle nommé le contrôle de l’éditeur. Ce contrôle affiche un riche éditeur HTML. Suivez ces étapes pour ajouter le contrôle de l’éditeur à une page :

1. Créer une nouvelle page ASP.NET nommée ShowEditor.aspx
2. Sélectionnez le contrôle ScriptManager sous l’onglet EXTENSIONs AJAX dans la boîte à outils et faites glisser le contrôle sur la page.
3. Sélectionnez le contrôle de l’éditeur sous l’onglet AJAX Control Toolkit dans la boîte à outils et faites glisser le contrôle sur la page (voir la figure 1). Le concepteur devrait ressembler à la figure 2.
4. Exécutez le site web en sélectionnant l’option de menu **Debug, Démarrer Debugging** ou frapper la touche F5.
5. Vous devriez voir la page de la figure 3.

[![Sélection du contrôle de l’éditeur HTML](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)

**Figure 01**: Sélection du contrôle de l’éditeur HTML[(Cliquez pour voir l’image grandeur nature](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))

[![Visual Studio Designer avec ScriptManager et Edit control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)

**Figure 02**: Visual Studio Designer avec ScriptManager et Edit control ([Cliquez pour voir l’image grandeur nature](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))

[![La page DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)

**Figure 03**: La page DisplayEditor.aspx ([Cliquez pour voir l’image grandeur nature](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))

## <a name="using-ajax-control-toolkit-control-extenders"></a>Utilisation des extensions de contrôle ajax

La boîte à outils de contrôle AJAX contient également des extenseurs de contrôle. Comme son nom l’indique, un prolongateur de contrôle étend la fonctionnalité d’un contrôle existant. Par exemple, l’extenseur de commande ConfirmButton étend le contrôle standard ASP.NET Button. L’extenseur modifie le comportement du contrôle du bouton de sorte que le bouton affiche un dialogue de confirmation lorsque vous cliquez dessus.

Un extenseur de contrôle, tout comme un contrôle AJAX Control Toolkit, nécessite un contrôle ScriptManager. Vous devez ajouter un contrôle ScriptManager à une page avant de commencer à utiliser des extenseurs de contrôle dans la page.

Suivez ces étapes pour utiliser l’extension de contrôle ConfirmButton :

1. Créer une nouvelle page ASP.NET nommée ShowConfirmButton.aspx
2. Ajoutez un contrôle ScriptManager à la page en faisant glisser le contrôle sur la page sous l’onglet EXTENSIONs AJAX.
3. Ajoutez un contrôle bouton standard à la page en faisant glisser le bouton sous l’onglet Standard dans la boîte à outils sur la surface du Concepteur.
4. Cliquez sur l’option De tâche **Add Extender** (voir la figure 4).
5. Dans le dialogue Choose Extender, sélectionnez ConfirmButtonExtender (voir figure 5) et cliquez sur le bouton OK.
6. Sélectionnez le contrôle du bouton dans le\_concepteur et élargissez le nœud Extenders, Button1 ConfirmButtonExtender dans la fenêtre Propriétés (voir la figure 6). Attribuer la valeur *vraiment?* à la propriété ConfirmText.
7. Exécutez la page en sélectionnant l’option de menu **Debug, Démarrer Debugging** ou appuyez sur la touche F5.

[![L’option de tâche Add Extender](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)

**Figure 04**: L’option de tâche Add Extender[(Cliquez pour voir l’image grandeur nature](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))

[![Sélection de l’extenseur de contrôle ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)

**Figure 05**: Sélection de l’extenseur de commande ConfirmButton[(Cliquez pour voir l’image grandeur nature](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))

[![Établissement d’une propriété ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)

**Figure 06**: Définir une propriété ConfirmButton[(Cliquez pour voir l’image grandeur nature)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png)

Lorsque la page s’ouvre, vous devriez voir un bouton. Lorsque vous cliquez sur le bouton, vous obtenez le dialogue de confirmation dans la figure 7.

[![Affichage du dialogue de confirmation](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)

**Figure 07**: Affichage du dialogue de confirmation[(Cliquez pour voir l’image grandeur nature](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))

Notez que vous ne faites normalement pas glisser un prolongateur de contrôle sur une page. Au lieu de cela, vous utilisez l’option **Add Extender** tâche pour ajouter un extenseur à un contrôle que vous avez déjà ajouté à une page. Notez, en outre, que vous définissez les propriétés d’extension de contrôle en ouvrant la feuille de propriété pour le contrôle étant étendu.

Un seul contrôle ASP.NET peut être prolongé par plusieurs extenseurs de contrôle. La feuille de propriété pour le contrôle étant étendu énumérera tous les extenseurs de contrôle associés au contrôle.

> [!div class="step-by-step"]
> [Suivant précédent](get-started-with-the-ajax-control-toolkit-vb.md)
> [Next](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)
