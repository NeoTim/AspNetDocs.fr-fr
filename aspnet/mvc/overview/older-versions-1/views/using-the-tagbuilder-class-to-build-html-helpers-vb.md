---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: À l’aide de la classe TagBuilder pour générer des Helpers HTML (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther vous présente une classe d’utilitaire utile dans l’infrastructure ASP.NET MVC nommé la classe TagBuilder. Vous pouvez utiliser la classe TagBuilder pour facilement...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 4fe34858aadb705ffb59e06ba805493d89aa4028
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59403205"
---
# <a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a>À l’aide de la classe TagBuilder pour générer des Helpers HTML (VB)

par [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther vous présente une classe d’utilitaire utile dans l’infrastructure ASP.NET MVC nommé la classe TagBuilder. Vous pouvez utiliser la classe TagBuilder pour créer facilement des balises HTML.


L’infrastructure ASP.NET MVC inclut une classe d’utilitaire utile nommée la classe TagBuilder que vous pouvez utiliser lors de la création de programmes d’assistance HTML. La classe TagBuilder, comme le suggère le nom de la classe, vous permet de créer facilement des balises HTML. Dans ce bref didacticiel, vous sont fournis avec une vue d’ensemble de la classe TagBuilder et vous allez apprendre à utiliser cette classe lors de la création d’un programme d’assistance HTML simple qui effectue le rendu HTML &lt;img&gt; balises.

## <a name="overview-of-the-tagbuilder-class"></a>Vue d’ensemble de la classe TagBuilder

La classe TagBuilder est contenue dans l’espace de noms System.Web.Mvc. Il a cinq méthodes :

- AddCssClass() – vous permet d’ajouter une nouvelle *classe = « »* à une balise d’attribut.
- GenerateId() – permet d’ajouter un attribut id à une balise. Cette méthode remplace automatiquement les points dans l’id (par défaut, les points sont remplacés par des traits de soulignement)
- MergeAttribute() – permet d’ajouter des attributs à une balise. Il existe plusieurs surcharges de cette méthode.
- SetInnerText() – vous permet de définir le texte interne de la balise. Le texte interne est automatiquement encoder en HTML.
- ToString() – permet d’afficher la balise. Vous pouvez spécifier si vous souhaitez créer une balise normale, une balise de début, une balise de fin ou une balise de fermeture automatique.
  

La classe TagBuilder a quatre propriétés importantes :

- Attributs : représente tous les attributs de la balise.
- IdAttributeDotReplacement – représente le caractère utilisé par la méthode GenerateId() pour remplacer les points (la valeur par défaut est un trait de soulignement).
- InnerHTML – représente le contenu interne de la balise. Assignez une chaîne à cette propriété *pas* encoder en HTML la chaîne.
- TagName – représente le nom de la balise.

Ces méthodes et propriétés vous donnent toutes les méthodes de base et les propriétés dont vous avez besoin pour créer une balise HTML. Vous n’avez pas vraiment besoin d’utiliser la classe TagBuilder. Vous pouvez utiliser une classe StringBuilder à la place. Toutefois, la classe TagBuilder simplifie la vie un peu.

## <a name="creating-an-image-html-helper"></a>Création d’une Image HTML Helper

Lorsque vous créez une instance de la classe TagBuilder, vous passez le nom de la balise que vous souhaitez générer au constructeur TagBuilder. Ensuite, vous pouvez appeler des méthodes, telles que les méthodes AddCssClass et MergeAttribute() pour modifier les attributs de la balise. Enfin, vous appelez la méthode ToString() pour afficher la balise.

Par exemple, le Listing 1 contient une application d’assistance HTML de l’Image. L’application d’assistance de l’Image est implémentée en interne avec un TagBuilder qui représente un élément HTML &lt;img&gt; balise.

**Liste 1 – Helpers\ImageHelper.vb**

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

Le module dans le Listing 1 contient deux méthodes surchargées nommés Image(). Lorsque vous appelez la méthode Image(), vous pouvez passer un objet qui représente un ensemble d’attributs HTML ou non.

Notez comment la méthode TagBuilder.MergeAttribute() est utilisée pour ajouter des attributs individuels tels que l’attribut src la TagBuilder. Notez que, en outre, comment la méthode TagBuilder.MergeAttributes() est utilisée pour ajouter une collection d’attributs à la TagBuilder. La méthode MergeAttributes() accepte un dictionnaire&lt;chaîne, objet&gt; paramètre. La classe RouteValueDictionary est utilisée pour convertir l’objet qui représente la collection d’attributs dans un dictionnaire&lt;chaîne, objet&gt;.

Après avoir créé l’application d’assistance d’Image, vous pouvez utiliser l’application d’assistance dans vos vues ASP.NET MVC comme une les autres assistances HTML standards. La vue dans la liste 2 utilise l’application d’assistance de l’Image pour afficher la même image d’une console Xbox deux fois (voir Figure 1). Le programme d’assistance Image() est appelée avec et sans une collection d’attributs HTML.

**Listing 2 – Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]


[![Tboîte de dialogue Nouveau projet he](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)

**Figure 01**: À l’aide du programme d’assistance d’Image ([cliquez pour afficher l’image en taille réelle](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))


Notez que vous devez importer l’espace de noms associé à l’application d’assistance de l’Image en haut de la vue Index.aspx. Le programme d’assistance est importé avec la directive suivante :

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

Dans une application Visual Basic, l’espace de noms par défaut est le même que le nom de l’application.

> [!div class="step-by-step"]
> [Précédent](creating-custom-html-helpers-vb.md)
> [Suivant](creating-page-layouts-with-view-master-pages-vb.md)
