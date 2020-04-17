---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
title: Comment puis-je utiliser le contrôle d’éditeur HTML ? (VB) - France Microsoft Docs
author: rick-anderson
description: HTMLEditor est un ASP.NET CONTRÔLE AJAX qui vous permet de créer et d’éditer facilement du contenu HTML via des boutons dans une barre d’outils.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 32ec9321-7c8c-4b0f-8234-99acb56df6b5
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
msc.type: authoredcontent
ms.openlocfilehash: bba42b4b6ff130b209b92c1608bc37f2f574c19c
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542831"
---
# <a name="how-do-i-use-the-html-editor-control-vb"></a>Comment puis-je utiliser le contrôle d’éditeur HTML ? (VB)

par [Microsoft](https://github.com/microsoft)

> HTMLEditor est un ASP.NET CONTRÔLE AJAX qui vous permet de créer et d’éditer facilement du contenu HTML via des boutons dans une barre d’outils.

Le but de ce tutoriel est de vous fournir un aperçu du contrôle de l’éditeur HTML inclus avec la boîte à outils de contrôle AJAX. L’éditeur HTML comprend des options pour changer la taille de la police, sélectionner une police, changer la couleur de fond, modifier la couleur du premier plan, ajouter des liens, ajouter des images, modifier l’alignement du texte et effectuer des opérations de coupe, de copie et de pâte (voir la figure 1).

[![L’éditeur HTML](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)

**Figure 01**: L’éditeur HTML[(Cliquez pour voir l’image grandeur nature](how-do-i-use-the-html-editor-control-vb/_static/image2.png))

L’éditeur HTML vous permet d’entrer du contenu en utilisant un mode de conception ou vous pouvez entrer HTML directement. Vous avez également la possibilité de prévisualiser votre contenu HTML (voir la figure 2).

[![Boutons de conception, DE HTML et de prévisualisation](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)

**Figure 02**: Boutons design, HTML et Aperçu[(Cliquez pour voir l’image grandeur nature](how-do-i-use-the-html-editor-control-vb/_static/image4.png))

Dans ce tutoriel, vous apprenez à afficher l’éditeur HTML, comment personnaliser les boutons de la barre d’outils qui apparaissent dans l’éditeur HTML, et comment éviter les attaques de script cross-Site.

## <a name="displaying-the-html-editor"></a>Affichage de l’éditeur HTML

Avant de pouvoir utiliser l’éditeur HTML dans une page ASP.NET, vous devez d’abord ajouter un contrôle ScriptManager à la page. Le contrôle ScriptManager est situé sous l’onglet EXTENSIONs AJAX dans la boîte à outils Visual Studio/Visual Web Developer Express.

Vous devez placer le contrôle ScriptManager en haut de la page avant tout autre contrôle sur la page. Par exemple, vous pouvez le placer immédiatement &lt;&gt; en dessous de l’étiquette de formulaire côté serveur d’ouverture.

Le contrôle d’éditeur HTML est situé dans la boîte à outils avec le reste des commandes ajax Control Toolkit. Il est nommé le contrôle de l’éditeur (voir la figure 3).

[![Le contrôle de l’éditeur HTML](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)

**Figure 03**: Le contrôle de l’éditeur HTML[(Cliquez pour voir l’image grandeur nature](how-do-i-use-the-html-editor-control-vb/_static/image6.png))

Après avoir fait glisser l’éditeur HTML sur une page, vous pouvez définir ses propriétés dans la feuille de propriété. Par exemple, vous souhaitez normalement définir les propriétés Largeur et Hauteur. La liste 1 contient la source d’une page ASP.NET qui contient un éditeur HTML.

**Liste 1 - SimpleEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

La page de Liste 1 contient un contrôle d’éditeur HTML, un contrôle de bouton et un contrôle littéral. Lorsque vous cliquez sur le bouton, le contenu de l’éditeur HTML apparaît dans le contrôle littéral (voir la figure 4).

[![Soumettre un formulaire auprès d’un éditeur HTML](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)

**Figure 04**: Soumettre un formulaire avec un éditeur HTML[(cliquez pour voir l’image grandeur nature)](how-do-i-use-the-html-editor-control-vb/_static/image8.png)

La propriété HTML Editor Content est utilisée pour récupérer le contenu HTML entré dans l’éditeur HTML. Sachez que ce contenu HTML peut contenir JavaScript. Dans la section suivante, nous discutons de la façon dont vous pouvez prévenir les attaques d’injection JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Personnaliser la barre d’outils de l’éditeur HTML

Vous pouvez personnaliser exactement les boutons qui apparaissent dans l’éditeur. Par exemple, vous pouvez supprimer l’onglet HTML pour empêcher les utilisateurs de passer l’éditeur HTML en mode HTML. Ou, vous pouvez supprimer la liste de délabrement de la taille de la police pour empêcher les utilisateurs de créer un texte trop grand dans un message de forum (voir la figure 5).

[![Un éditeur HTML personnalisé](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)

**Figure 05**: Un éditeur HTML personnalisé[(Cliquez pour voir l’image grandeur nature](how-do-i-use-the-html-editor-control-vb/_static/image10.png))

Vous personnalisez les boutons de la barre d’outils en dérivant un nouvel éditeur HTML de la classe d’éditeur de base. Par exemple, l’éditeur personnalisé dans Listing 2 ne contient que des boutons de barre d’outils pour gras et italiques. Tous les autres boutons de barre d’outils ont été supprimés. En outre, l’onglet HTML a été supprimé du bas de l’éditeur (mais les onglets Design et Preview sont toujours là).

**Liste 2\_- App Code-CustomEditor.vb**

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

Vous devez ajouter la classe dans\_la liste 2 à votre dossier App Code afin que la classe soit compilée automatiquement. Si le\_dossier App Code n’existe pas dans votre site Web, vous pouvez simplement ajouter le dossier.

Après avoir créé un éditeur personnalisé, vous pouvez l’ajouter à une page ASP.NET de la même manière que vous ajoutez l’éditeur HTML normal (voir Liste 3).

**Liste 3 - ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Éviter les attaques de scripts croisés (XSS)

Chaque fois que vous acceptez les commentaires d’un utilisateur et que vous redisjouez cette entrée sur votre site Web, vous pouvez ouvrir votre site Web aux attaques de script cross-Site (XSS). En théorie, un pirate malveillant pourrait soumettre le code JavaScript qui est exécuté lorsque l’entrée est redisplayed. Le JavaScript peut être utilisé pour voler des mots de passe utilisateur ou d’autres informations sensibles.

Normalement, vous pouvez vaincre les attaques XSS en codant HTML quelle que soit l’entrée que vous récupérez auprès d’un utilisateur avant de l’afficher dans une page Web. Cependant, le code HTML de la sortie de &lt;&gt; l’éditeur HTML ne serait pas seulement coder les balises de script, il serait également coder toutes les balises HTML. En d’autres termes, vous perdriez tout le formatage tel que le type de police, la taille de police, et la couleur de fond.

Si vous collectez des informations sensibles auprès de vos utilisateurs - tels que les mots de passe, les numéros de carte de crédit et les numéros de sécurité sociale - alors vous ne devez pas afficher de contenu non codé que vous récupérez auprès d’un utilisateur avec l’éditeur HTML. Vous ne devez utiliser l’éditeur HTML que dans les situations où vous ne réjouez pas le contenu HTML, ou le contenu HTML est soumis à votre site Web par une partie de confiance.

Imaginez, par exemple, que vous créez une application de blog. Dans cette situation, il est logique d’utiliser l’éditeur HTML lors de la composition des billets de blog. Vous êtes le seul qui soumet un billet de blog et, sans doute, vous pouvez vous faire confiance pour ne pas soumettre JavaScript malveillant. Cependant, il n’est pas logique d’utiliser l’éditeur HTML lorsque vous permettez aux utilisateurs anonymes de poster des commentaires. Vous devez être particulièrement prudent dans les situations où les utilisateurs soumettent des informations sensibles telles que les mots de passe. Potentiellement, un utilisateur malveillant pourrait poster un commentaire qui contient le bon JavaScript pour voler un mot de passe.

## <a name="summary"></a>Récapitulatif

Dans ce tutoriel, vous avez reçu un bref aperçu du contrôle de l’éditeur HTML inclus dans la boîte à outils de contrôle AJAX. Vous avez appris à utiliser l’éditeur HTML pour accepter le contenu riche d’un utilisateur et soumettre le contenu au serveur. Nous avons également discuté de la façon dont vous pouvez personnaliser les boutons de la barre d’outils qui sont affichés par l’éditeur HTML. Enfin, vous avez appris à éviter les attaques de script cross-Site lors de l’utilisation de l’éditeur HTML pour accepter des entrées potentiellement malveillantes.

> [!div class="step-by-step"]
> [Précédent](how-do-i-use-the-html-editor-control-cs.md)
