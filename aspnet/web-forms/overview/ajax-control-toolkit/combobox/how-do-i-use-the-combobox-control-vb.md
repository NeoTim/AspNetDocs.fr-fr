---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: Comment puis-je utiliser le contrôle ComboBox ? (VB) - France Microsoft Docs
author: rick-anderson
description: ComboBox est un ASP.NET contrôle AJAX qui combine la flexibilité d’une TextBox avec une liste d’options à partir desquelles les utilisateurs peuvent choisir.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 237e3ef864238c11fc1fb49239c3f6fa3f75537d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543065"
---
# <a name="how-do-i-use-the-combobox-control-vb"></a>Comment puis-je utiliser le contrôle ComboBox ? (VB)

par [Microsoft](https://github.com/microsoft)

> ComboBox est un ASP.NET contrôle AJAX qui combine la flexibilité d’une TextBox avec une liste d’options à partir desquelles les utilisateurs peuvent choisir.

Le but de ce tutoriel est d’expliquer le contrôle AJAX Toolkit ComboBox contrôle. Le ComboBox fonctionne comme une combinaison entre un contrôle DropDownList standard ASP.NET et un contrôle TextBox. Vous pouvez soit sélectionner parmi une liste préexistante d’éléments, soit entrer un nouvel élément.

Le ComboBox est similaire à l’extenseur de contrôle AutoComplete, mais les commandes sont utilisées dans différents scénarios. L’extension AutoComplete interroge un service web pour obtenir des entrées correspondantes. Le contrôle ComboBox, en revanche, est parasécé avec un ensemble d’éléments. L’utilisation de l’extenseur AutoComplete est logique lorsque vous travaillez avec un grand ensemble de données (millions de pièces de voiture) tout en utilisant le contrôle ComboBox est logique lorsque vous travaillez avec un petit ensemble de données (des dizaines de pièces de voiture).

## <a name="selecting-from-a-static-list-of-items"></a>Sélection d’une liste statique d’éléments

Laissez-nous commencer par un simple échantillon d’utilisation du contrôle ComboBox. Imaginez que vous souhaitez afficher une liste statique d’éléments dans une liste de dropdown. Cependant, vous voulez laisser ouverte la possibilité que la liste n’est pas complète. Vous souhaitez permettre à un utilisateur d’entrer une valeur personnalisée dans la liste.

Nous allons créer une nouvelle page ASP.NET Web Forms et utiliser le contrôle ComboBox dans la page. Ajoutez la nouvelle page ASP.NET à votre projet et passez à la vue Design.

Si vous souhaitez utiliser le contrôle ComboBox dans la page, vous devez ajouter un contrôle ScriptManager à la page. Faites glisser le contrôle ScriptManager sous l’onglet EXTENSIONs AJAX sur la surface du Designer. Vous devez ajouter le contrôle ScriptManager en haut de la page; vous pouvez l’ajouter immédiatement en &lt;&gt; dessous de l’étiquette de formulaire côté serveur d’ouverture.

Ensuite, faites glisser le contrôle ComboBox sur la page. Vous pouvez trouver le contrôle ComboBox dans la boîte à outils avec les autres commandes et extenseurs de commande AJAX Control Toolbox (voir figure1).

[![Formulaire simple pour créer une carte de visite](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)

**Figure 01**: Sélection du contrôle ComboBox depuis la boîte à outils ([Cliquez pour voir l’image grandeur nature](how-do-i-use-the-combobox-control-vb/_static/image2.png))

Nous utiliserons le contrôle ComboBox pour afficher une liste statique de choix. L’utilisateur peut choisir un niveau particulier de piquant pour ses aliments à partir d’une liste de trois choix : doux, moyen et chaud (voir la figure 2).

[![Sélection d’une liste statique d’éléments](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)

**Figure 02**: Sélection d’une liste statique d’éléments[(Cliquez pour voir l’image grandeur nature](how-do-i-use-the-combobox-control-vb/_static/image4.png))

Il y a deux façons d’ajouter ces choix au contrôle De ComboBox. Tout d’abord, vous sélectionnez l’option de tâche Edit Options lorsque vous planez votre souris sur le contrôle dans la vue de conception et ouvrez l’éditeur d’éléments (voir la figure 3).

[![Modifier les articles ComboBox](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)

**Figure 03**: Modifier les éléments De ComboBox[(Cliquez pour voir l’image grandeur nature](how-do-i-use-the-combobox-control-vb/_static/image6.png))

La deuxième option est d’ajouter la liste &lt;des éléments entre&gt; l’ouverture et la fermeture asp:ComboBox tags dans Source view. La page de la liste 1 contient la ComboBox mise à jour qui a la liste des éléments.

**Liste 1 - Static.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

Lorsque vous ouvrez la page dans la liste 1, vous pouvez sélectionner l’une des options préexistantes de la ComboBox. En d’autres termes, le ComboBox fonctionne comme un contrôle DropDownList.

Cependant, vous avez également la possibilité d’entrer un nouveau choix (par exemple, Super Spicy) qui n’est pas dans la liste existante. Ainsi, la ComboBox fonctionne également comme un contrôle TextBox.

Que vous choisissiez un article préexistant ou que vous saisissiez un article personnalisé, lorsque vous soumettez le formulaire, votre choix apparaît dans le contrôle de l’étiquette. Lorsque vous soumettez le formulaire,\_le gestionnaire de clic btnSubmit exécute et met à jour l’étiquette (voir la figure 4).

[![Affichage de l’élément sélectionné](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)

**Figure 04**: Affichage de l’élément sélectionné ([Cliquez pour voir l’image grandeur nature](how-do-i-use-the-combobox-control-vb/_static/image8.png))

La ComboBox prend en charge les mêmes propriétés que le contrôle DropDownList pour la récupération de l’élément sélectionné après la présentation d’un formulaire :

- SelectedItem.Text - Affiche la valeur de la propriété Text de l’élément sélectionné.
- SelectedItem.Value - Affiche la valeur de la propriété Value de l’élément sélectionné ou affiche le texte tapé dans la ComboBox.
- SelectedValue - Same as SelectedItem.Value except that this property allows to specify the default (initial) selected item.

Si vous tapez un choix personnalisé dans la ComboBox, le choix personnalisé est attribué à la fois aux propriétés SelectedItem.Text et SelectedItem.Value.

## <a name="selecting-the-list-of-items-from-the-database"></a>Sélection de la liste des éléments de la base de données

Vous pouvez récupérer la liste des éléments que la ComboBox affiche dans une base de données. Par exemple, vous pouvez lier la ComboBox à un contrôle SqlDataSource, un contrôle ObjectDataSource, un LinqDataSource ou un EntityDataSource.

Imaginez que vous souhaitez afficher une liste de films dans une ComboBox. Vous souhaitez récupérer la liste des films dans la table de base de données Movies. Procédez comme suit :

1. Créer une page nommée Movies.aspx
2. Ajoutez un contrôle ScriptManager à la page en faisant glisser le ScriptManager sous l’onglet EXTENSIONs AJAX dans la boîte à outils sur la page.
3. Ajoutez un contrôle ComboBox à la page en faisant glisser la ComboBox sur la page.
4. Dans la vue design, planez votre souris sur le contrôle ComboBox et sélectionnez l’option De tâche **Choose Data Source** (voir la figure 5). Le Data Source Configuration Wizard est lancé.
5. Dans l’étape Choisir une &lt;source de&gt; **données,** sélectionnez la nouvelle option source de données.
6. Dans l’étape **Choisissez un type de source de données,** sélectionnez la base de données.
7. Dans l’étape **Choisissez votre connexion de données,** sélectionnez votre base de données (par exemple, MoviesDB.mdf).
8. Dans la **chaîne Enregistrer la chaîne de connexion à l’étape de fichier de configuration d’application,** sélectionnez l’option pour enregistrer votre chaîne de connexion.
9. Dans **l’étape Configurer l’étape d’énoncé de sélection,** sélectionnez la table de base de données Films et sélectionnez toutes les colonnes.
10. Dans l’étape **Test Query,** cliquez sur le bouton Finition.
11. De retour dans l’étape Choisir la **source de données,** sélectionnez la colonne Titre pour le champ à afficher et la colonne Id pour le champ de données (voir Figure).
12. Cliquez sur le bouton OK pour fermer l’assistant.

[![Choisir une source de données](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)

**Figure 05**: Choisir une source de données[(Cliquez pour voir l’image grandeur nature](how-do-i-use-the-combobox-control-vb/_static/image10.png))

[![Choix du texte de données et des champs de valeur](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)

**Figure 06**: Choisir le texte de données et les champs de valeur[(Cliquez pour voir l’image grandeur nature](how-do-i-use-the-combobox-control-vb/_static/image12.png))

Après avoir terminé les étapes ci-dessus, le ComboBox est lié à un contrôle SqlDataSource qui représente les films de la table de base de données Movies. La source de la page ressemble à la liste 2 (j’ai nettoyé le formatage un peu).

**Liste 2 - Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

Notez que le contrôle ComboBox a une propriété DataSourceID qui pointe vers le contrôle SqlDataSource. Lorsque vous ouvrez la page dans un navigateur, la liste des films de la base de données s’affiche (voir la figure 7). Vous pouvez choisir un film de la liste ou entrer un nouveau film en tapant le film dans la ComboBox.

[![Affichage d’une liste de films](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)

**Figure 07**: Affichage d’une liste de films[(Cliquez pour voir l’image grandeur nature](how-do-i-use-the-combobox-control-vb/_static/image14.png))

## <a name="setting-the-dropdownstyle"></a>Réglage du DropDownStyle

Vous pouvez utiliser la propriété Combox DropDownStyle pour modifier le comportement de la ComboBox. Cette propriété y accepte les valeurs possibles :

- DropDown - (valeur par défaut) Le ComboBox affiche une liste de dropdown lorsque vous cliquez sur la flèche et que vous pouvez entrer une valeur personnalisée.
- Simple - Le ComboBox affiche automatiquement une liste de dropdown et vous pouvez entrer une valeur personnalisée.
- DropDownList - The ComboBox fonctionne comme un contrôle DropDownList.

La différence entre DropDown et Simple est lorsque la liste des éléments est affichée. Dans le cas de Simple, la liste s’affiche immédiatement lorsque vous déplacez la focus vers la ComboBox. Dans le cas de DropDown, vous devez cliquer sur la flèche pour voir la liste des éléments.

La valeur DropDownList fait fonctionner le contrôle De ComboBox tout comme un contrôle DropDownList standard. Cependant, il y a une différence importante ici. Les anciennes versions d’Internet Explorer affichent un contrôle DropDownList avec un z-index infini de sorte que le contrôle apparaîtra en face de tout contrôle placé en face de lui. Parce que la ComboBox &lt;rend&gt; une balise &lt;&gt; de div HTML au lieu d’une balise HTML select, la ComboBox respecte correctement la commande z.

## <a name="setting-the-autocompletemode"></a>Réglage de l’AutoCompleteMode

Vous utilisez la propriété ComboBox AutoCompleteMode pour spécifier ce qui se passe lorsque quelqu’un tape du texte dans la ComboBox. Cette propriété accepte les valeurs possibles suivantes :

- Aucun - (valeur par défaut) La ComboBox ne fournit aucun comportement auto-complet.
- Suggérez - Le ComboBox affiche la liste et il met en évidence l’élément correspondant dans la liste (voir la figure 8).
- Annexe - La ComboBox n’affiche pas la liste et annexe l’élément correspondant de la liste sur ce que vous avez tapé (voir la figure 9).
- SuggestAppend - La ComboBox affiche la liste et joint l’élément correspondant de la liste sur ce que vous avez tapé (voir la figure 10).

[![La ComboBox fait une suggestion](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)

**Figure 08**: La ComboBox fait une suggestion ([Cliquez pour voir l’image grandeur nature](how-do-i-use-the-combobox-control-vb/_static/image16.png))

[![Addbox ComboBox appendices texte correspondant](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)

**Figure 09**: ComboBox annexe le texte correspondant ([Cliquez pour voir l’image grandeur nature](how-do-i-use-the-combobox-control-vb/_static/image18.png))

[![La ComboBox suggère et joint des annexes](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)

**Figure 10**: The ComboBox suggests and appends ([Cliquez pour voir l’image grandeur nature](how-do-i-use-the-combobox-control-vb/_static/image20.png))

## <a name="summary"></a>Récapitulatif

Dans ce tutoriel, vous avez appris à utiliser le contrôle ComboBox pour afficher un ensemble fixe d’éléments. Nous avons lié le contrôle ComboBox à la fois à un ensemble statique d’éléments et à une table de base de données. Enfin, vous avez appris à modifier le comportement de la ComboBox en définissant ses propriétés DropDownStyle et AutoCompleteMode.

> [!div class="step-by-step"]
> [Précédent](how-do-i-use-the-combobox-control-cs.md)
