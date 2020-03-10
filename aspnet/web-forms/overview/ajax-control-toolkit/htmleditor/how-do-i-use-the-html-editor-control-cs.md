---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
title: Comment faire utiliser le contrôle de l’éditeur HTML ? (C#) | Microsoft Docs
author: microsoft
description: HTMLEditor est un contrôle ASP.NET AJAX qui vous permet de créer et de modifier facilement du contenu HTML à l’aide de boutons dans une barre d’outils.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: f47e6224-c2e5-4472-b069-b6c7b6115200
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
msc.type: authoredcontent
ms.openlocfilehash: cb7d75b59b1361abeb6d3c38ad6e42e34d6e3f7b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78577860"
---
# <a name="how-do-i-use-the-html-editor-control-c"></a>Comment faire utiliser le contrôle de l’éditeur HTML ? (C#)

par [Microsoft](https://github.com/microsoft)

> HTMLEditor est un contrôle ASP.NET AJAX qui vous permet de créer et de modifier facilement du contenu HTML à l’aide de boutons dans une barre d’outils.

L’objectif de ce didacticiel est de vous fournir une vue d’ensemble du contrôle de l’éditeur HTML inclus dans le kit d’outils de contrôle AJAX. L’éditeur HTML comprend des options pour modifier la taille de police, sélectionner une police, modifier la couleur d’arrière-plan, modifier la couleur de premier plan, ajouter des liens, ajouter des images, modifier l’alignement du texte et effectuer des opérations couper, copier et coller (voir figure 1).

[![l’éditeur HTML](how-do-i-use-the-html-editor-control-cs/_static/image1.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image1.png)

**Figure 01**: éditeur HTML ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-html-editor-control-cs/_static/image2.png))

L’éditeur HTML vous permet d’entrer du contenu à l’aide d’un mode création ou vous pouvez entrer du code HTML directement. Vous disposez également de l’option permettant d’afficher un aperçu de votre contenu HTML (voir figure 2).

[![les boutons conception, HTML et aperçu](how-do-i-use-the-html-editor-control-cs/_static/image2.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image3.png)

**Figure 02**: boutons de conception, html et aperçu ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-html-editor-control-cs/_static/image4.png))

Dans ce didacticiel, vous allez apprendre à afficher l’éditeur HTML, à personnaliser les boutons de la barre d’outils qui s’affichent dans l’éditeur HTML et à éviter les attaques de script entre sites.

## <a name="displaying-the-html-editor"></a>Affichage de l’éditeur HTML

Avant de pouvoir utiliser l’éditeur HTML dans une page ASP.NET, vous devez d’abord ajouter un contrôle ScriptManager à la page. Le contrôle ScriptManager se trouve sous l’onglet Extensions AJAX dans la boîte à outils Visual Studio/Visual Web Developer Express.

Vous devez placer le contrôle ScriptManager en haut de la page avant tous les autres contrôles de la page. Par exemple, vous pouvez le placer immédiatement en dessous de la balise de&gt; &lt;de l’ouverture côté serveur.

Le contrôle de l’éditeur HTML se trouve dans la boîte à outils avec le reste des contrôles du kit d’outils de contrôle AJAX. Elle est nommée contrôle de l’éditeur (voir figure 3).

[![le contrôle de l’éditeur HTML](how-do-i-use-the-html-editor-control-cs/_static/image3.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image5.png)

**Figure 03**: contrôle de l’éditeur HTML ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-html-editor-control-cs/_static/image6.png))

Après avoir fait glisser l’éditeur HTML sur une page, vous pouvez définir ses propriétés dans la feuille de propriétés. Par exemple, vous souhaitez normalement définir les propriétés Width et Height. La liste 1 contient la source d’une page ASP.NET qui contient un éditeur HTML.

**Liste 1-SimpleEditor. aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample1.aspx)]

La page de la liste 1 contient un contrôle d’éditeur HTML, un contrôle bouton et un contrôle Literal. Lorsque vous cliquez sur le bouton, le contenu de l’éditeur HTML s’affiche dans le contrôle Literal (voir figure 4).

[![soumission d’un formulaire à l’aide d’un éditeur HTML](how-do-i-use-the-html-editor-control-cs/_static/image4.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image7.png)

**Figure 04**: envoi d’un formulaire à l’aide d’un éditeur HTML ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-html-editor-control-cs/_static/image8.png))

La propriété contenu de l’éditeur HTML est utilisée pour récupérer le contenu HTML entré dans l’éditeur HTML. N’oubliez pas que ce contenu HTML peut contenir du code JavaScript. Dans la section suivante, nous expliquons comment vous pouvez empêcher les attaques par injection de code JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Personnalisation de la barre d’outils de l’éditeur HTML

Vous pouvez personnaliser exactement les boutons qui apparaissent dans l’éditeur. Par exemple, vous souhaiterez peut-être supprimer l’onglet HTML pour empêcher les utilisateurs de basculer l’éditeur HTML en mode HTML. Vous pouvez également supprimer la liste déroulante taille de police pour empêcher les utilisateurs de créer du texte trop grand dans une publication de message de Forum (voir figure 5).

[![un éditeur HTML personnalisé](how-do-i-use-the-html-editor-control-cs/_static/image5.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image9.png)

**Figure 05**: éditeur HTML personnalisé ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-html-editor-control-cs/_static/image10.png))

Vous personnalisez les boutons de la barre d’outils en dérivant un nouvel éditeur HTML de la classe de l’éditeur de base. Par exemple, l’éditeur personnalisé dans la liste 2 contient uniquement des boutons de barre d’outils pour le gras et l’italique. Tous les autres boutons de la barre d’outils ont été supprimés. En outre, l’onglet HTML a été supprimé au bas de l’éditeur (mais les onglets conception et aperçu sont toujours présents).

**Liste 2-application\_Code\CustomEditor.cs**

[!code-csharp[Main](how-do-i-use-the-html-editor-control-cs/samples/sample2.cs)]

Vous devez ajouter la classe dans la liste 2 à votre application\_dossier de code afin que la classe soit compilée automatiquement. Si l’application\_dossier de code n’existe pas dans votre site Web, vous pouvez simplement ajouter le dossier.

Après avoir créé un éditeur personnalisé, vous pouvez l’ajouter à une page ASP.NET de la même façon que vous ajoutez l’éditeur HTML normal (voir le Listing 3).

**Liste 3-ShowCustomEditor. aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Éviter les attaques de script entre sites (XSS)

Chaque fois que vous acceptez l’entrée d’un utilisateur et que vous réaffichez cette entrée sur votre site Web, vous pouvez ouvrir votre site Web pour les attaques de script entre sites (XSS). En théorie, un pirate malveillant pourrait envoyer du code JavaScript qui est exécuté lorsque l’entrée est réaffichée. Le JavaScript peut être utilisé pour voler les mots de passe des utilisateurs ou d’autres informations sensibles.

Normalement, vous pouvez faire face aux attaques XSS en codant le code HTML, quelle que soit l’entrée que vous récupérez d’un utilisateur avant de l’afficher dans une page Web. Toutefois, l’encodage HTML de la sortie de l’éditeur HTML n’encoderait pas uniquement les balises de&gt; de script &lt;. En d’autres termes, vous perdez toute la mise en forme, comme le type de police, la taille de police et la couleur d’arrière-plan.

Si vous collectez des informations sensibles auprès de vos utilisateurs, telles que des mots de passe, des numéros de carte de crédit et des numéros de sécurité sociale, vous ne devez pas afficher le contenu non encodé que vous récupérez à partir d’un utilisateur à l’aide de l’éditeur HTML. Vous devez utiliser l’éditeur HTML uniquement dans les situations où vous ne réaffichez pas le contenu HTML, ou si le contenu HTML est soumis à un tiers de confiance sur votre site Web.

Imaginez, par exemple, que vous créez une application de blog. Dans ce cas, il est judicieux d’utiliser l’éditeur HTML pour composer des billets de blog. Vous êtes le seul responsable de l’envoi d’un billet de blog et, vraisemblablement, vous pouvez vous faire confiance à ne pas envoyer de code JavaScript malveillant. Toutefois, il n’est pas judicieux d’utiliser l’éditeur HTML lorsque vous autorisez les utilisateurs anonymes à poster des commentaires. Vous devez être particulièrement vigilant dans les situations dans lesquelles les utilisateurs envoient des informations sensibles telles que des mots de passe. Potentiellement, un utilisateur malveillant pourrait poster un commentaire qui contient le JavaScript approprié pour voler un mot de passe.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez été fourni avec une brève vue d’ensemble du contrôle de l’éditeur HTML inclus dans le kit d’outils de contrôle AJAX. Vous avez appris à utiliser l’éditeur HTML pour accepter du contenu riche d’un utilisateur et l’envoyer au serveur. Nous avons également abordé la manière dont vous pouvez personnaliser les boutons de la barre d’outils qui sont affichés par l’éditeur HTML. Enfin, vous avez appris à éviter les attaques de script entre sites lors de l’utilisation de l’éditeur HTML pour accepter les entrées potentiellement malveillantes.

> [!div class="step-by-step"]
> [Next](how-do-i-use-the-html-editor-control-vb.md)
