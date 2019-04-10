---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: Paramètres déclaratifs (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous illustrerons comment utiliser un paramètre défini sur une valeur codée en dur pour sélectionner les données à afficher dans un contrôle DetailsView.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: be792b0511e91b65cf3dd56458630b4e8ec3b5af
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59384226"
---
# <a name="declarative-parameters-vb"></a>Paramètres déclaratifs (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) ou [télécharger le PDF](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> Dans ce didacticiel, nous illustrerons comment utiliser un paramètre défini sur une valeur codée en dur pour sélectionner les données à afficher dans un contrôle DetailsView.


## <a name="introduction"></a>Introduction

Dans le [dernier didacticiel](displaying-data-with-the-objectdatasource-vb.md) nous avons vu l’affichage de données avec les contrôles GridView, DetailsView et FormView liés à un contrôle ObjectDataSource qui a appelé le `GetProducts()` méthode à partir de la `ProductsBLL` classe. Le `GetProducts()` méthode retourne un DataTable fortement typé rempli avec tous les enregistrements de la base de données Northwind `Products` table. Le `ProductsBLL` classe contient des méthodes supplémentaires pour des sous-ensembles de simples le retour des produits - `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`, et `GetProductsBySupplierID(supplierID)`. Ces trois méthodes attendent un paramètre d’entrée indiquant comment filtrer les informations de produit retourné.

ObjectDataSource peut être utilisé pour appeler des méthodes qui attendent des paramètres d’entrée, mais pour cela, nous devons spécifier d'où proviennent les valeurs de ces paramètres. Les valeurs de paramètre peuvent être codées en dur ou peuvent provenir de diverses sources dynamiques, y compris : valeurs de chaîne de requête, variables de Session, la valeur de propriété d’un contrôle Web sur la page, ou d’autres.

Pour ce didacticiel, nous allons commencer par illustrant l’utilisation d’un paramètre défini sur une valeur codée en dur. Plus précisément, nous examinerons l’ajout d’un contrôle DetailsView à la page qui affiche des informations sur un produit spécifique, à savoir du Chef Anton Gumbo Mix, ce qui a un `ProductID` de 5. Ensuite, nous verrons comment définir la valeur du paramètre basée sur un contrôle Web. En particulier, nous allons utiliser une zone de texte pour permettre à l’utilisateur de type dans un pays, après laquelle ils peuvent cliquer sur un bouton pour afficher la liste des fournisseurs qui se trouvent dans ce pays.

## <a name="using-a-hard-coded-parameter-value"></a>À l’aide d’une valeur de paramètre codées en dur

Pour le premier exemple, commencez par ajouter un contrôle DetailsView pour le `DeclarativeParams.aspx` page dans le `BasicReporting` dossier. À partir de la balise active de DetailsView, sélectionnez &lt;nouvelle source de données&gt; à partir de la liste déroulante liste et choisissez d’ajouter un ObjectDataSource.


[![Ajj ObjectDataSource à la Page](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Figure 1**: Ajouter un ObjectDataSource à la Page ([cliquez pour afficher l’image en taille réelle](declarative-parameters-vb/_static/image3.png))


Ceci démarrera automatiquement Assistant de choisir la Source de données du contrôle ObjectDataSource. Sélectionnez le `ProductsBLL` classe dans le premier écran de l’Assistant.


[![Schoisir la classe ProductsBLL](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**Figure 2**: Sélectionnez le `ProductsBLL` classe ([cliquez pour afficher l’image en taille réelle](declarative-parameters-vb/_static/image6.png))


Étant donné que nous souhaitons afficher des informations sur un produit particulier que nous souhaitons utiliser la `GetProductByProductID(productID)` (méthode).


[![Choisissez la méthode GetProductByProductID(productID)](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Figure 3**: Choisissez le `GetProductByProductID(productID)` (méthode) ([cliquez pour afficher l’image en taille réelle](declarative-parameters-vb/_static/image9.png))


Étant donné que la méthode que nous avons sélectionné inclut un paramètre, il est un écran plus pour l’Assistant, où nous avons invités à définir la valeur à utiliser pour le paramètre. La liste sur la gauche affiche tous les paramètres de la méthode sélectionnée. Pour `GetProductByProductID(productID)` seul `productID`. Sur la droite, nous pouvons spécifier la valeur pour le paramètre sélectionné. La liste de liste déroulante de paramètre source énumère les différentes sources possibles pour la valeur du paramètre. Dans la mesure où nous voulons spécifier une valeur codée en dur de 5 pour le `productID` paramètre, laissez le paramètre source en tant que None et entrer 5 dans la zone de texte DefaultValue.


[![A Codé en dur paramètre valeur de 5 sera utilisée pour le paramètre productID](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Figure 4**: Un Hard-Coded paramètre valeur de 5 sera utilisée pour le `productID` paramètre ([cliquez pour afficher l’image en taille réelle](declarative-parameters-vb/_static/image12.png))


À l’issue de l’Assistant Configurer la Source de données, le balisage déclaratif du contrôle ObjectDataSource inclut un `Parameter` de l’objet dans le `SelectParameters` collection pour chacun des paramètres d’entrée attendus par la méthode définie dans le `SelectMethod` propriété. Étant donné que la méthode que nous utilisons dans cet exemple attend simplement un paramètre d’entrée unique, `parameterID`, il existe une seule entrée ici. Le `SelectParameters` collection peut contenir n’importe quelle classe qui dérive de la `Parameter` classe dans le `System.Web.UI.WebControls` espace de noms. Pour les valeurs de paramètre codées en dur, la base de `Parameter` classe est utilisée, mais pour l’autre paramètre source options une dérivée `Parameter` classe est utilisée ; vous pouvez également créer vos propres [types de paramètres personnalisés](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), si nécessaire.

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Si vous programmez sur votre ordinateur le balisage déclaratif vous voyez à ce stade mai incluent des valeurs pour le `InsertMethod`, `UpdateMethod`, et `DeleteMethod` propriétés, ainsi que `DeleteParameters`. Assistant de choisir la Source de données de l’ObjectDataSource spécifie automatiquement les méthodes à partir de la `ProductBLL` à utiliser pour l’insertion, de la mise à jour et de suppression, sauf si vous avez désactivé explicitement ces difficultés, ils amèneront inclus dans le balisage ci-dessus.


Lorsque vous visitez cette page, les données de contrôle Web appellera l’ObjectDataSource `Select` (méthode), qui appellera le `ProductsBLL` la classe `GetProductByProductID(productID)` méthode à l’aide de la valeur codée en dur de 5 pour le `productID` paramètre d’entrée. La méthode retourne un fortement typée `ProductDataTable` objet qui contient une seule ligne avec les informations à propos Gumbo Mix du Chef Anton (le produit avec `ProductID` 5).


[![IPour plus d’informations sur Chef Anton Gumbo Mix sont affichées](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Figure 5**: Affiche des Gumbo Mix d’informations sur Chef Anton ([cliquez pour afficher l’image en taille réelle](declarative-parameters-vb/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Définition de la valeur de paramètre à la valeur de propriété d’un contrôle Web

Paramètre de l’ObjectDataSource valeurs peuvent également être définies selon la valeur d’un contrôle Web dans la page. Pour illustrer cela, nous allons avoir un GridView qui répertorie tous les fournisseurs qui sont trouvent dans un pays spécifié par l’utilisateur. Pour accomplir ce guide de démarrage en ajoutant une zone de texte à la page dans laquelle l’utilisateur peut entrer un nom de pays. Définir ce contrôle de zone de texte `ID` propriété `CountryName`. Ajoutez également un contrôle bouton Web.


[![Ajj une zone de texte à la Page avec l’ID CountryName](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Figure 6**: Ajouter une zone de texte à la Page avec `ID` `CountryName` ([cliquez pour afficher l’image en taille réelle](declarative-parameters-vb/_static/image18.png))


Ensuite, ajoutez un GridView à la page et, à partir de la balise active, choisissez Ajouter un nouveau ObjectDataSource. Étant donné que nous souhaitons afficher sélectionner des informations de fournisseur la `SuppliersBLL` classe à partir du premier écran de l’Assistant. Dans le deuxième écran, choisir le `GetSuppliersByCountry(country)` (méthode).


[![Choisissez la méthode GetSuppliersByCountry(country)](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Figure 7**: Choisissez le `GetSuppliersByCountry(country)` (méthode) ([cliquez pour afficher l’image en taille réelle](declarative-parameters-vb/_static/image21.png))


Dans la mesure où le `GetSuppliersByCountry(country)` méthode a un paramètre d’entrée, l’Assistant inclut une fois encore un écran final pour le choix de la valeur du paramètre. Cette fois, définissez la source de paramètre au contrôle. Cela remplira la liste déroulante ControlID avec les noms des contrôles sur la page. Sélectionnez le `CountryName` contrôle dans la liste. Lorsque la page est visitée en premier le `CountryName` zone de texte sera vide, aucun résultat n’est renvoyés et rien ne s’affiche. Si vous souhaitez afficher des résultats par défaut, définissez la zone de texte DefaultValue en conséquence.


[![Set la valeur du paramètre à la valeur de contrôle de CountryName](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Figure 8**: Définissez la valeur de paramètre sur le `CountryName` valeur de contrôle ([cliquez pour afficher l’image en taille réelle](declarative-parameters-vb/_static/image24.png))


Balisage déclaratif de l’ObjectDataSource diffère légèrement de notre premier exemple, à l’aide un [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) au lieu de la norme `Parameter` objet. Un `ControlParameter` possède des propriétés supplémentaires pour spécifier le `ID` du contrôle Web et la valeur de propriété à utiliser pour le paramètre (`PropertyName`). L’Assistant Configurer la Source de données est suffisamment malin pour déterminer que, pour une zone de texte, nous allons probablement souhaitez utiliser le `Text` propriété pour la valeur du paramètre. Si, toutefois, vous souhaitez utiliser une valeur de propriété différente à partir du contrôle Web, vous pouvez modifier le `PropertyName` valeur ici ou en cliquant sur le lien « Afficher les propriétés avancées » dans l’Assistant.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

Lorsque vous visitez la page pour la première fois le `CountryName` zone de texte est vide. L’ObjectDataSource `Select` méthode est toujours appelée par le contrôle GridView, mais la valeur de `Nothing` est passé dans le `GetSuppliersByCountry(country)` (méthode). Le TableAdapter convertit le `Nothing` dans une base de données `NULL` valeur (`DBNull.Value`), mais la requête utilisée par le `GetSuppliersByCountry(country)` méthode est écrite telles qu’elle ne retourne pas les valeurs lorsque un `NULL` valeur est spécifiée pour le `@CategoryID`paramètre. En bref, aucun fournisseur n’est retournés.

Une fois que le visiteur entre dans un pays, toutefois et qu’il clique sur le bouton Afficher les fournisseurs pour provoquer une publication (postback), ObjectDataSource `Select` méthode est à nouveau interrogée, en passant le contrôle de zone de texte `Text` valeur en tant que le `country` paramètre.


[![Ttuyau fournisseurs du Canada est affichés](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Figure 9**: Ces fournisseurs du Canada sont affichés ([cliquez pour afficher l’image en taille réelle](declarative-parameters-vb/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>Affichage de tous les fournisseurs par défaut

Plutôt qu’afficher aucun des fournisseurs lors de l’affichage de tout d’abord la page Nous souhaiterons peut-être afficher *tous les* fournisseurs dans un premier temps, permettant à l’utilisateur alléger la liste en entrant un nom de pays dans la zone de texte. Lorsque la zone de texte est vide, le `SuppliersBLL` la classe `GetSuppliersByCountry(country)` est passé dans la méthode `Nothing` pour son *`country`* paramètre d’entrée. Cela `Nothing` valeur est alors transmise vers le bas dans la couche Data Access `GetSupplierByCountry(country)` méthode, où elle est convertie en une base de données `NULL` valeur pour le `@Country` paramètre dans la requête suivante :

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

L’expression `Country = NULL` retourne toujours False, même pour les enregistrements dont la propriété `Country` colonne a un `NULL` valeur ; par conséquent, aucun enregistrements ne sont renvoyés.

Pour retourner *tous les* fournisseurs lorsque le pays de zone de texte est vide, nous pouvons augmenter la `GetSuppliersByCountry(country)` méthode dans la couche BLL à appeler le `GetSuppliers()` méthode lors de son paramètre de pays est `Nothing` et d’appeler la couche Data Access `GetSuppliersByCountry(country)` méthode sinon. Cela aura l’effet de retourner tous les fournisseurs lorsque aucun pays n’est spécifié et le sous-ensemble approprié de fournisseurs lorsque le paramètre de pays est inclus.

Modifier le `GetSuppliersByCountry(country)` méthode dans la `SuppliersBLL` classe à ce qui suit :

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Avec cette modification le `DeclarativeParams.aspx` page affiche tous les fournisseurs lors de la première visite (ou chaque fois que le `CountryName` zone de texte est vide).


[![All fournisseurs sont maintenant affichés par défaut](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Figure 10**: Tous les fournisseurs sont maintenant affichés par défaut ([cliquez pour afficher l’image en taille réelle](declarative-parameters-vb/_static/image30.png))


## <a name="summary"></a>Récapitulatif

Pour utiliser des méthodes avec des paramètres d’entrée, nous devons spécifier les valeurs pour les paramètres de l’ObjectDataSource `SelectParameters` collection. Autorisent les différents types de paramètres pour la valeur du paramètre doit être obtenu à partir de différentes sources. Le type de paramètre par défaut utilise une valeur codée en dur, mais comme facilement (et sans une ligne de code) des valeurs de paramètre peuvent être obtenues à partir de la chaîne de requête, Session variables, les cookies et même entré par l’utilisateur les valeurs à partir de contrôles Web sur la page.

Les exemples que nous avons vu dans ce didacticiel illustre comment utiliser les valeurs de paramètre déclarative. Toutefois, il peut arriver lorsque nous devons utiliser une source de paramètre qui n’est pas disponible, comme la date et heure actuelles, ou, si notre site a été à l’aide de l’appartenance, l’ID d’utilisateur du visiteur. Pour de tels scénarios, nous pouvons définir les valeurs de paramètre par programme avant l’ObjectDataSource appel de méthode de son de l’objet sous-jacent. Nous verrons comment procéder dans le [didacticiel suivant](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md).

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Hilton Giesenow. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](displaying-data-with-the-objectdatasource-vb.md)
> [Suivant](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
