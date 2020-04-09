---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Création d’aides HTML personnalisées (C) Microsoft Docs
author: microsoft
description: Le but de ce tutoriel est de démontrer comment vous pouvez créer des aides HTML personnalisées que vous pouvez utiliser dans vos vues MVC. En profitant de HTML Helper...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 7a2e5a5b42aa5bf267a42fef2fcad7022001ce6f
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675327"
---
# <a name="creating-custom-html-helpers-c"></a>Création de helpers HTML personnalisés (C#)

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> Le but de ce tutoriel est de démontrer comment vous pouvez créer des aides HTML personnalisées que vous pouvez utiliser dans vos vues MVC. En profitant de HTML Helpers, vous pouvez réduire la quantité de dactylographie fastidieuse des balises HTML que vous devez effectuer pour créer une page HTML standard.

Le but de ce tutoriel est de démontrer comment vous pouvez créer des aides HTML personnalisées que vous pouvez utiliser dans vos vues MVC. En profitant de HTML Helpers, vous pouvez réduire la quantité de dactylographie fastidieuse des balises HTML que vous devez effectuer pour créer une page HTML standard.

Dans la première partie de ce tutoriel, je décris certains des aides HTML existants inclus avec le cadre ASP.NET MVC. Ensuite, je décris deux méthodes de création d’aides HTML personnalisés: j’explique comment créer des aides HTML personnalisées en créant une méthode statique et en créant une méthode d’extension.

## <a name="understanding-html-helpers"></a>Comprendre les aides HTML

Un assistant HTML est juste une méthode qui renvoie une chaîne. La chaîne peut représenter n’importe quel type de contenu que vous voulez. Par exemple, vous pouvez utiliser DES aides HTML `<input>` `<img>` pour rendre les balises HTML standard comme HTML et les balises. Vous pouvez également utiliser des aides HTML pour rendre un contenu plus complexe, comme une bande d’onglet ou une table HTML de données de base de données.

Le cadre ASP.NET MVC comprend l’ensemble suivant d’aides HTML standard (ce n’est pas une liste complète):

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox ()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden ()
- Html.ListBox ()
- Html.Password()
- Html.RadioButton ()
- Html.TextArea ()
- Html.TextBox ()

Par exemple, considérez le formulaire dans la liste 1. Ce formulaire est rendu avec l’aide de deux des aides HTML standard (voir la figure 1). Ce formulaire `Html.BeginForm()` utilise `Html.TextBox()` les méthodes et helper pour rendre un formulaire HTML simple.

[![Page rendue avec les aides HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Figure 01**: Page rendue avec des aides HTML ([Cliquez pour voir l’image grandeur nature](creating-custom-html-helpers-cs/_static/image3.png))

**Liste 1`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

La méthode Html.BeginForm() Helper est utilisée pour `<form>` créer les balises HTML d’ouverture et de fermeture. Notez `Html.BeginForm()` que la méthode est appelée dans une instruction à l’aide. L’instruction d’utilisation `<form>` garantit que l’étiquette est fermée à la fin du bloc d’utilisation.

Si vous préférez, au lieu de créer un bloc d’utilisation, vous pouvez `<form>` appeler la méthode Html.EndForm() Helper pour fermer l’étiquette. Utilisez n’importe quelle approche `<form>` pour créer une balise d’ouverture et de fermeture qui vous semble la plus intuitive.

Les `Html.TextBox()` méthodes Helper sont utilisées dans `<input>` la liste 1 pour rendre les balises HTML. Si vous sélectionnez la source d’afficher dans votre navigateur, vous voyez la source HTML dans la liste 2. Notez que la source contient des balises HTML standard.

> [!IMPORTANT]
> remarquez `Html.TextBox()`que l’aide -HTML `<%= %>` est `<% %>` rendu avec des balises au lieu de balises. Si vous n’incluez pas le signe égal, alors rien n’est rendu au navigateur.

Le cadre MVC ASP.NET contient un petit ensemble d’aides. Très probablement, vous devrez étendre le cadre MVC avec des aides HTML personnalisées. Dans le reste de ce tutoriel, vous apprenez deux méthodes de création d’aides HTML personnalisées.

**Liste 2`Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Création d’aides HTML avec des méthodes statiques

La façon la plus simple de créer un nouvel assistant HTML est de créer une méthode statique qui renvoie une chaîne. Imaginez, par exemple, que vous décidiez de créer `<label>` un nouvel assistant HTML qui rend une balise HTML. Vous pouvez utiliser la classe dans `<label>` la liste 2 pour rendre un .

**Liste 2`Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

Il n’y a rien de spécial dans la classe dans la liste 2. La `Label()` méthode renvoie simplement une chaîne.

La vue index modifiée dans `LabelHelper` la liste `<label>` 3 utilise les balises HTML pour rendre. Notez que le `<%@ imports %>` point de `Application1.Helpers` vue comprend une directive qui importe l’espace nom.

**Liste 2`Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Création d’aides HTML avec des méthodes d’extension

Si vous souhaitez créer des aides HTML qui fonctionnent comme les aides HTML standard incluses dans le cadre MVC ASP.NET, alors vous devez créer des méthodes d’extension. Les méthodes d’extension vous permettent d’ajouter de nouvelles méthodes à une classe existante. Lors de la création d’une méthode d’assistance HTML, vous ajoutez de nouvelles méthodes à la classe HtmlHelper représentée par la propriété Html d’une vue.

La classe dans La liste 3 `HtmlHelper` ajoute `Label()`une méthode d’extension à la classe nommée . Il ya un couple de choses que vous devriez remarquer à propos de cette classe. Tout d’abord, notez que la classe est une classe statique. Vous devez définir une méthode d’extension avec une classe statique.

Deuxièmement, notez que `Label()` le premier paramètre `this`de la méthode est précédé par le mot clé . Le premier paramètre d’une méthode d’extension indique la classe que la méthode d’extension s’étend.

**Liste 3`Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Après avoir créé une méthode d’extension, et de construire votre application avec succès, la méthode d’extension apparaît dans Visual Studio Intellisense comme toutes les autres méthodes d’une classe (voir la figure 2). La seule différence est que les méthodes d’extension apparaissent avec un symbole spécial à côté d’eux (une icône d’une flèche vers le bas).

[![Utilisation de la méthode d’extension Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Figure 02**: Utilisation de la méthode d’extension Html.Label()[(Cliquez pour voir l’image grandeur nature](creating-custom-html-helpers-cs/_static/image6.png))

La vue index modifiée dans La liste 4 utilise la méthode `<label>` d’extension Html.Label() pour rendre toutes ses balises.

**Liste 4`Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Récapitulatif

Dans ce tutoriel, vous avez appris deux méthodes de création d’aides HTML personnalisées. Tout d’abord, vous `Label()` avez appris à créer un assistant HTML personnalisé en créant une méthode statique qui renvoie une chaîne. Ensuite, vous avez appris `Label()` à créer une méthode d’assistance `HtmlHelper` HTML personnalisée en créant une méthode d’extension sur la classe.

Dans ce tutoriel, je me suis concentré sur la construction d’une méthode d’aide HTML extrêmement simple. Réalisez qu’un assistant HTML peut être aussi compliqué que vous le souhaitez. Vous pouvez créer des aides HTML qui rendent un contenu riche comme des vues d’arbres, des menus ou des tableaux de données de base de données.

> [!div class="step-by-step"]
> [Suivant précédent](asp-net-mvc-views-overview-cs.md)
> [Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
