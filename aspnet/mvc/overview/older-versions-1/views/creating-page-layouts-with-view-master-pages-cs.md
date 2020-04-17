---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
title: Création de mises en page avec Afficher les pages Maîtresses (C) Microsoft Docs
author: rick-anderson
description: Dans ce tutoriel, vous apprenez à créer une mise en page commune pour plusieurs pages de votre application en profitant des pages maîtresses de vue. Vous pouvez utiliser un...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: dff54fcb-68b1-4488-89a2-ca97532d6a4c
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: d313e017d7061ae03e8dea79e611d0cf3838297d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542519"
---
# <a name="creating-page-layouts-with-view-master-pages-c"></a>Création de dispositions de page avec des pages maîtres de vue (C#)

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_CS.pdf)

> Dans ce tutoriel, vous apprenez à créer une mise en page commune pour plusieurs pages de votre application en profitant des pages maîtresses de vue. Vous pouvez utiliser une page principale de vue, par exemple, pour définir une mise en page à deux colonnes et utiliser la mise en page en deux colonnes pour toutes les pages de votre application Web.

## <a name="creating-page-layouts-with-view-master-pages"></a>Création de mises en page avec Afficher les pages Maîtresses

Dans ce tutoriel, vous apprenez à créer une mise en page commune pour plusieurs pages de votre application en profitant des pages maîtresses de vue. Vous pouvez utiliser une page principale de vue, par exemple, pour définir une mise en page à deux colonnes et utiliser la mise en page en deux colonnes pour toutes les pages de votre application Web.

Vous pouvez également profiter des pages-maîtres de vue pour partager du contenu commun sur plusieurs pages de votre application. Par exemple, vous pouvez placer votre logo de site Web, vos liens de navigation et vos bannières publicitaires dans une page principale. De cette façon, chaque page de votre application afficherait automatiquement ce contenu.

Dans ce tutoriel, vous apprenez à créer une nouvelle page maître de vue et de créer une nouvelle page de contenu de vue basée sur la page principale.

### <a name="creating-a-view-master-page"></a>Création d’une page Master View

Commençons par créer une page-maître de vue qui définit une mise en page à deux colonnes. Vous ajoutez une nouvelle page maître d’affichage à un projet MVC en cliquant à droite sur le dossier Views-Shared, en sélectionnant l’option de menu **Ajouter, Nouvel élément**, et en sélectionnant le modèle **de page master de vision MVC** (voir la figure 1).

[![Ajout d’une page maîtresse de vue](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)

**Figure 01**: Ajout d’une page maîtresse de vue[(Cliquez pour voir l’image grandeur nature](creating-page-layouts-with-view-master-pages-cs/_static/image3.png))

Vous pouvez créer plus d’une page principale de vue dans une application. Chaque page principale de vue peut définir une mise en page différente. Par exemple, vous voudrez peut-être que certaines pages disposent d’une mise en page à deux colonnes et d’autres pages pour avoir une mise en page de trois colonnes.

Une page principale de vue ressemble beaucoup à une vue standard ASP.NET MVC. Cependant, contrairement à une vue normale, une `<asp:ContentPlaceHolder>` page principale de vue contient une ou plusieurs balises. Les `<contentplaceholder>` balises sont utilisées pour marquer les zones de la page principale qui peuvent être remplacées dans une page de contenu individuelle.

Par exemple, la page principale de la liste 1 définit une mise en page à deux colonnes. Il contient `<contentplaceholder>` deux balises. Un `<ContentPlaceHolder>` pour chaque colonne.

**Liste 1`Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample1.aspx)]

Le corps de la page principale de `<div>` la page principale de la liste 1 contient deux balises qui correspondent aux deux colonnes. La classe de colonne De feuille `<div>` de style en cascade est appliquée aux deux balises. Cette classe est définie dans la feuille de style déclarée en haut de la page principale. Vous pouvez prévisualiser comment la page principale de vue sera rendue en passant à la vue de conception. Cliquez sur l’onglet Design en bas à gauche de l’éditeur de code source (voir la figure 2).

[![Aperçu d’une page maîtresse dans le concepteur](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)

**Figure 02**: Aperçu d’une page maîtresse dans le concepteur ([Cliquez pour voir l’image grandeur nature](creating-page-layouts-with-view-master-pages-cs/_static/image6.png))

### <a name="creating-a-view-content-page"></a>Création d’une page de contenu d’aperçu

Après avoir créé une page master de vue, vous pouvez créer une ou plusieurs pages de contenu de vue basées sur la page principale de vue. Par exemple, vous pouvez créer une page de contenu d’affichage d’index pour le contrôleur d’accueil en cliquant à droite sur le dossier Views-Home, en sélectionnant **Add, New Item,** en sélectionnant le modèle **de page de contenu MVC View,** en entrant le nom Index.aspx et en cliquant sur le bouton **Ajouter** (voir la figure 3).

[![Ajout d’une page de contenu de vue](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)

**Figure 03**: Ajout d’une page de contenu de vue[(Cliquez pour voir l’image grandeur nature](creating-page-layouts-with-view-master-pages-cs/_static/image9.png))

Après avoir cliqué sur le bouton Ajouter, un nouveau dialogue apparaît qui vous permet de sélectionner une page maître de vue pour vous associer à la page de contenu de vue (voir la figure 4). Vous pouvez naviguer vers la page principale de vue Site.master que nous avons créée dans la section précédente.

[![Sélection d’une page maîtresse](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)

**Figure 04**: Sélection d’une page maîtresse ([Cliquez pour voir l’image grandeur nature](creating-page-layouts-with-view-master-pages-cs/_static/image12.png))

Après avoir créé une nouvelle page de contenu de vue basée sur la page master Site.master, vous obtenez le fichier dans la liste 2.

**Liste 2`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample2.aspx)]

Notez que cette `<asp:Content>` vue contient une balise qui correspond à chacune des `<asp:ContentPlaceHolder>` balises dans la page principale de vue. Chaque `<asp:Content>` balise comprend un attribut ContentPlaceHolderID qui indique en particulier `<asp:ContentPlaceHolder>` qu’il remplace.

Notez, en outre, que la page d’affichage du contenu dans la liste 2 ne contient aucune des balises HTML d’ouverture et de fermeture normales. Par exemple, il ne contient `<html>` pas `<head>` l’ouverture et la fermeture ou les balises. Toutes les balises normales d’ouverture et de fermeture sont contenues dans la page principale de vue.

Tout contenu que vous souhaitez afficher dans une page `<asp:Content>` de contenu de vue doit être placé dans une balise. Si vous placez un contenu HTML ou autre en dehors de ces balises, alors vous obtiendrez une erreur lorsque vous essayez de voir la page.

Vous n’avez pas besoin `<asp:ContentPlaceHolder>` de remplacer chaque balise à partir d’une page principale dans une page de vision de contenu. Vous n’avez qu’à remplacer une `<asp:ContentPlaceHolder>` balise lorsque vous souhaitez remplacer l’étiquette par un contenu particulier.

Par exemple, la vue index modifiée dans `<asp:Content>` la liste 3 ne contient que deux balises. Chacune `<asp:Content>` des balises comprend un texte.

**Liste 3`Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample3.aspx)]

Lorsque la vue dans la liste 3 est demandée, elle rend la page de la figure 5. Notez que la vue rend une page avec deux colonnes. Notez, en outre, que le contenu de la page de contenu de vue est fusionné avec le contenu de la page principale de vue

[![La page de contenu de l’index](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)

**Figure 05**: La page de contenu de l’index[(Cliquez pour voir l’image grandeur nature](creating-page-layouts-with-view-master-pages-cs/_static/image15.png))

### <a name="modifying-view-master-page-content"></a>Modification du contenu de la page Master View

Un problème que vous rencontrez presque immédiatement lorsque vous travaillez avec les pages maîtresses de vue est le problème de modifier le contenu de la page principale lorsque différentes pages de contenu de vue sont demandées. Par exemple, vous voulez que chaque page de votre application Web ait un titre unique. Cependant, le titre est déclaré dans la page principale de vue et non dans la page de contenu de vue. Alors, comment personnalisez-vous le titre de la page pour chaque page de contenu de vue ?

Il y a deux façons de modifier le titre affiché par une page de contenu de vue. Tout d’abord, vous pouvez attribuer un `<%@ page %>` titre de page à l’attribut de titre de la directive déclarée en haut d’une page de contenu de vue. Par exemple, si vous souhaitez attribuer le titre de page "Super Great Website" à la vue index, vous pouvez inclure la directive suivante en haut de la vue de l’index:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample4.aspx)]

Lorsque la vue de l’index est rendue au navigateur, le titre souhaité apparaît dans la barre de titre du navigateur :

[![Barre de titre de navigateur](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)

Il y a une exigence importante qu’une page de vue principale doit satisfaire pour que l’attribut de titre fonctionne. La page principale de `<head runat="server">` vue doit `<head>` contenir une balise au lieu d’une balise normale pour son en-tête. Si `<head>` l’étiquette n’inclut pas l’attribut runat "serveur", le titre n’apparaîtra pas. La page principale de `<head runat="server">` vue par défaut comprend l’étiquette requise.

Une autre approche pour modifier le contenu de la page principale à partir d’une page de contenu de vue individuelle est d’envelopper la région que vous souhaitez modifier dans une `<asp:ContentPlaceHolder>` balise. Par exemple, imaginez que vous voulez changer non seulement le titre, mais aussi les balises méta, rendues par une page de vue maître. La page de vue principale `<asp:ContentPlaceHolder>` dans La `<head>` liste 4 contient une balise dans son étiquette.

**Liste 4`Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample5.aspx)]

Notez `<asp:ContentPlaceHolder>` que l’étiquette dans la liste 4 inclut le contenu par défaut : un titre par défaut et des balises méta par défaut. Si vous ne remplacez `<asp:ContentPlaceHolder>` pas cette balise dans une page de contenu de vue individuelle, le contenu par défaut sera affiché.

La page d’affichage du contenu `<asp:ContentPlaceHolder>` dans La liste 5 l’emporte sur l’étiquette afin d’afficher un titre personnalisé et des balises méta personnalisées.

**Liste 5`Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample6.aspx)]

### <a name="summary"></a>Récapitulatif

Ce tutoriel vous a fourni une introduction de base pour afficher les pages maîtresses et afficher les pages de contenu. Vous avez appris à créer de nouvelles pages de master de vue et à créer des pages de contenu de vue basées sur elles. Nous avons également examiné comment vous pouvez modifier le contenu d’une page principale de vue à partir d’une page de contenu de vue particulière.

> [!div class="step-by-step"]
> [Suivant précédent](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
> [Next](passing-data-to-view-master-pages-cs.md)
