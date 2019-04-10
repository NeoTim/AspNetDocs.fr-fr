---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
title: Maître/détail filtrage avec deux DropDownList (c#) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel développe la relation maître/détail pour ajouter une troisième couche, à l’aide de deux contrôles DropDownList pour sélectionner le dit parent et le grand-parent souhaité...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: ac4b0d77-4816-4ded-afd0-88dab667aedd
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
msc.type: authoredcontent
ms.openlocfilehash: 03d0cd7e835b5526af60a21679260f849714c37e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59421288"
---
# <a name="masterdetail-filtering-with-two-dropdownlists-c"></a>Filtrage maître/détail avec deux DropDownList (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_8_CS.exe) ou [télécharger le PDF](master-detail-filtering-with-two-dropdownlists-cs/_static/datatutorial08cs1.pdf)

> Ce didacticiel développe la relation maître/détail pour ajouter une troisième couche, à l’aide de deux contrôles DropDownList pour sélectionner les enregistrements parents et le grand-parent souhaités.


## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](master-detail-filtering-with-a-dropdownlist-cs.md) nous avons examiné comment afficher un rapport maître/détail simple à l’aide d’un seul DropDownList rempli avec les catégories et un GridView affichant les produits qui appartiennent à la catégorie sélectionnée. Ce modèle de rapport fonctionne bien lors de l’affichage des enregistrements qui ont une relation un-à-plusieurs et peuvent facilement être étendus pour fonctionner pour les scénarios incluant plusieurs relations un-à-plusieurs. Par exemple, un système de saisie de commandes aurait des tables qui correspondent aux clients, commandes et éléments de ligne de commande. Un client donné peut avoir plusieurs commandes avec chaque commande composée de plusieurs éléments. Ces données peuvent être présentées à l’utilisateur avec deux DropDownList et un GridView. La première DropDownList aurait un élément de liste pour chaque client dans la base de données et de la seconde de contenu en cours de commandes passées par le client sélectionné. Un GridView répertorie les éléments de ligne de la commande sélectionnée.

Si la base de données Northwind inclut les informations de détails de commande/customer/order canonique dans son `Customers`, `Orders`, et `Order Details` tables, ces tables ne sont pas capturés dans notre architecture. Néanmoins, nous pouvons toujours illustrent à l’aide de deux DropDownList dépendants. La première DropDownList répertorie les catégories et le second les produits appartenant à la catégorie sélectionnée. Un contrôle DetailsView répertorie ensuite les détails du produit sélectionné.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Étape 1 : Création et remplissage de l’objet DropDownList de catégories

Notre objectif premier consiste à ajouter de la liste DropDownList qui répertorie les catégories. Ces étapes ont été examinés en détail dans le didacticiel précédent, mais sont résumées ici par souci d’exhaustivité.

Ouvrir le `MasterDetailsDetails.aspx` page dans le `Filtering` dossier, ajoutez un contrôle DropDownList à la page, définissez son `ID` propriété `Categories`, puis cliquez sur le lien configurer la Source de données dans sa balise active. À partir de l’Assistant de Configuration de Source de données choisir d’ajouter une nouvelle source de données.


[![Aune nouvelle Source de données pour l’objet DropDownList de jj](master-detail-filtering-with-two-dropdownlists-cs/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image1.png)

**Figure 1**: Ajouter une nouvelle Source de données pour l’objet DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image3.png))


La nouvelle source de données devrait être naturellement, un ObjectDataSource. Nommez cette nouvelle ObjectDataSource `CategoriesDataSource` et appeler le `CategoriesBLL` l’objet `GetCategories()` (méthode).


[![Choisissez d’utiliser la classe CategoriesBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image4.png)

**Figure 2**: Choisissez d’utiliser le `CategoriesBLL` classe ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image6.png))


[![Cconfiguration de l’ObjectDataSource d’utiliser la méthode GetCategories()](master-detail-filtering-with-two-dropdownlists-cs/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image7.png)

**Figure 3**: Configurer l’ObjectDataSource à utiliser le `GetCategories()` (méthode) ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image9.png))


Après avoir configuré l’ObjectDataSource nous devons toujours spécifier quel champ de source de données doit être affiché dans le `Categories` DropDownList et celle qui doit être configuré en tant que la valeur de l’élément de liste. Définir le `CategoryName` champ en tant que l’affichage et `CategoryID` comme valeur pour chaque élément de liste.


[![HEnregistrer l’affichage DropDownList CategoryName Field et CategoryID utilisez comme valeur](master-detail-filtering-with-two-dropdownlists-cs/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image10.png)

**Figure 4**: Affiche la liste DropDownList le `CategoryName` champ et utilisez `CategoryID` comme valeur ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image12.png))


À ce stade, nous avons un contrôle DropDownList (`Categories`) qui est rempli avec les enregistrements à partir de la `Categories` table. Lorsque l’utilisateur choisit une nouvelle catégorie dans la liste DropDownList, nous devrons attraper une publication (postback) se produise pour actualiser le produit DropDownList que nous allons créer à l’étape 2. Par conséquent, vérifiez l’option Activer AutoPostBack à partir de la `categories` balise active de DropDownList.


[![EActiver AutoPostBack pour l’objet DropDownList catégories](master-detail-filtering-with-two-dropdownlists-cs/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image13.png)

**Figure 5**: Activer AutoPostBack pour le `Categories` DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Étape 2 : Affichage des produits de la catégorie sélectionnée dans une deuxième DropDownList

Avec le `Categories` DropDownList terminée, l’étape suivante consiste à afficher un contrôle DropDownList de produits appartenant à la catégorie sélectionnée. Pour ce faire, ajoutez un autre DropDownList vers la page nommée `ProductsByCategory`. Comme avec la `Categories` DropDownList, créer un nouveau ObjectDataSource pour le `ProductsByCategory` DropDownList nommé `ProductsByCategoryDataSource`.


[![Ajj une nouvelle Source de données pour le ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image16.png)

**Figure 6**: Ajouter une nouvelle Source de données pour le `ProductsByCategory` DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image18.png))


[![Ccréer un nouveau ProductsByCategoryDataSource de nommé ObjectDataSource](master-detail-filtering-with-two-dropdownlists-cs/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image19.png)

**Figure 7**: Créer une nouvelle nommée de ObjectDataSource `ProductsByCategoryDataSource` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image21.png))


Dans la mesure où le `ProductsByCategory` besoins DropDownList pour afficher uniquement les produits appartenant à la catégorie sélectionnée, que ObjectDataSource appelle le `GetProductsByCategoryID(categoryID)` méthode à partir de la `ProductsBLL` objet.


[![Choisissez d’utiliser la classe ProductsBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image22.png)

**Figure 8**: Choisissez d’utiliser le `ProductsBLL` classe ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image24.png))


[![Cconfiguration de l’ObjectDataSource d’utiliser la méthode GetProductsByCategoryID(categoryID)](master-detail-filtering-with-two-dropdownlists-cs/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image25.png)

**Figure 9**: Configurer l’ObjectDataSource à utiliser le `GetProductsByCategoryID(categoryID)` (méthode) ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image27.png))


Dans l’étape finale de l’Assistant, nous avons besoin spécifier la valeur de la *`categoryID`* paramètre. Attribuer ce paramètre à l’élément sélectionné à partir de la `Categories` DropDownList.


[![Pplein la valeur du paramètre dans la liste DropDownList catégories categoryID](master-detail-filtering-with-two-dropdownlists-cs/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image28.png)

**Figure 10**: Extraire le *`categoryID`* valeur du paramètre à partir de la `Categories` DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image30.png))


Avec ObjectDataSource configuré, tous ne reste qu’à spécifier les champs de source de données sont utilisées pour l’affichage et la valeur des éléments de la liste DropDownList. Afficher le `ProductName` champ et utilisez le `ProductID` champ comme valeur.


[![Spécifier les champs de Source de données utilisée pour la DropDownList texte et les propriétés de la valeur des ListItems](master-detail-filtering-with-two-dropdownlists-cs/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image31.png)

**Figure 11**: Spécifiez les champs de Source de données utilisée pour la DropDownList `ListItem` s' `Text` et `Value` propriétés ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image33.png))


Avec ObjectDataSource et `ProductsByCategory` DropDownList configuré notre page affichera deux DropDownList : la première liste toutes les catégories tandis que la deuxième répertorie les produits appartenant à la catégorie sélectionnée. Lorsque l’utilisateur sélectionne une nouvelle catégorie dans la liste DropDownList première, résulte d’une publication (postback) et la deuxième DropDownList est petit, montrant les produits qui appartiennent à la catégorie qui vient d’être sélectionnée. Figures 12 et 13 show `MasterDetailsDetails.aspx` en action lorsqu’ils sont affichés via un navigateur.


[![Wpoule consulter la Page, la catégorie des boissons est sélectionnée](master-detail-filtering-with-two-dropdownlists-cs/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image34.png)

**Figure 12**: Lors de la première visite la Page, la catégorie des boissons est sélectionnée ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image36.png))


[![Choosing une autre catégorie affiche produits de la nouvelle catégorie](master-detail-filtering-with-two-dropdownlists-cs/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image37.png)

**Figure 13**: Choix d’un autre catégorie s’affiche produits de la nouvelle catégorie ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image39.png))


Actuellement le `productsByCategory` DropDownList, en cas de modification, est *pas* provoquer une publication (postback). Toutefois, nous allons une publication (postback) se produit une fois que nous avons ajouté un contrôle DetailsView pour afficher les détails du produit sélectionné (étape 3). Par conséquent, cochez la case à cocher Activer AutoPostBack à partir de la `productsByCategory` balise active de DropDownList.


[![EActiver la fonctionnalité AutoPostBack pour l’objet DropDownList productsByCategory](master-detail-filtering-with-two-dropdownlists-cs/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image40.png)

**Figure 14**: Activer la fonctionnalité AutoPostBack pour le `productsByCategory` DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Étape 3 : À l’aide d’un contrôle DetailsView pour afficher les détails pour le produit sélectionné

L’étape finale consiste à afficher les détails pour le produit sélectionné dans un contrôle DetailsView. Pour ce faire, ajoutez un contrôle DetailsView à la page, définissez son `ID` propriété `ProductDetails`et créer un nouveau ObjectDataSource pour celui-ci. Configurer cette ObjectDataSource afin d’extraire ses données à partir de la `ProductsBLL` la classe `GetProductByProductID(productID)` méthode à l’aide de la valeur sélectionnée de la `ProductsByCategory` DropDownList pour la valeur de la *`productID`* paramètre.


[![Choisissez d’utiliser la classe ProductsBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image43.png)

**Figure 15**: Choisissez d’utiliser le `ProductsBLL` classe ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image45.png))


[![Cconfiguration de l’ObjectDataSource d’utiliser la méthode GetProductByProductID(productID)](master-detail-filtering-with-two-dropdownlists-cs/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image46.png)

**Figure 16**: Configurer l’ObjectDataSource à utiliser le `GetProductByProductID(productID)` (méthode) ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image48.png))


[![Pplein la valeur du paramètre à partir de ProductsByCategory DropDownList productID](master-detail-filtering-with-two-dropdownlists-cs/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image49.png)

**Figure 17**: Extraire le *`productID`* valeur du paramètre à partir de la `ProductsByCategory` DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image51.png))


Vous pouvez choisir d’afficher un des champs disponibles dans le contrôle DetailsView. J’ai choisi de supprimer le `ProductID`, `SupplierID`, et `CategoryID` champs et réorganisés et mis en forme les champs restants. En outre, j’ai effacés de DetailsView `Height` et `Width` propriétés, ce qui permet le contrôle DetailsView d’étendre à la largeur nécessaire pour obtenir un affichage optimal ses données au lieu il limité à une taille spécifiée. Le balisage complet est affichée ci-dessous :


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample1.aspx)]

Prenez un moment pour essayer la `MasterDetailsDetails.aspx` page dans un navigateur. À première vue, il peut sembler que tout fonctionne comme vous le souhaitez, mais il existe un problème subtil. Lorsque vous choisissez une nouvelle catégorie la `ProductsByCategory` DropDownList est mis à jour pour inclure ces produits pour la catégorie sélectionnée, mais la `ProductDetails` DetailsView a continué à afficher les informations de produit précédentes. Le contrôle DetailsView est mis à jour lors du choix d’un autre produit de la catégorie sélectionnée. En outre, si vous testez minutieusement suffisamment, vous découvrirez que si vous choisissez en permanence de nouvelles catégories (tel que le choix des boissons à partir de la `Categories` DropDownList, puis les Condiments, confiseries puis) chaque autre sélection de catégorie provoque le `ProductDetails`Contrôle DetailsView afin d’être actualisé.

Pour aider à concrétiser la ce problème, nous allons étudier un exemple spécifique. Lorsque vous visitez tout d’abord la page de la catégorie boissons est sélectionnée et les produits connexes sont chargés dans le `ProductsByCategory` DropDownList. Tran est le produit sélectionné et ses détails sont affichent dans le `ProductDetails` DetailsView, comme illustré dans la Figure 18.


[![TDétails du produit de sélectionné de he sont affichés dans un contrôle DetailsView](master-detail-filtering-with-two-dropdownlists-cs/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image52.png)

**Figure 18**: Détails du produit sélectionné sont affichés dans un contrôle DetailsView ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image54.png))


Si vous modifiez la sélection de catégories à partir de boissons pour les Condiments, une publication (postback) et le `ProductsByCategory` DropDownList est mis à jour en conséquence, mais le contrôle DetailsView affiche toujours les détails de Tran.


[![TIl a détails du produit sélectionné précédemment sont toujours affichées](master-detail-filtering-with-two-dropdownlists-cs/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image55.png)

**Figure 19**: Le détails du produit sélectionné précédemment sont toujours affichées ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image57.png))


Sélection d’un nouveau produit dans la liste d’actualise le contrôle DetailsView comme prévu. Si vous sélectionnez une nouvelle catégorie après avoir modifié le produit, le contrôle DetailsView n’actualisez à nouveau. Toutefois, si au lieu de choisir un nouveau produit, vous avez sélectionné une nouvelle catégorie, le contrôle DetailsView voulez-vous actualiser. Dans le monde est passe-t-il ici ?

Le problème est un problème de synchronisation dans le cycle de vie de la page. Chaque fois qu’une page est demandée qu'elle se poursuit dans un nombre d’étapes en tant que son rendu. Dans une de ces étapes ObjectDataSource contrôle vérifie si un de leurs `SelectParameters` valeurs ont changé. Si, par conséquent, le contrôle Web de données lié à ObjectDataSource sait qu’il doit actualiser son affichage. Par exemple, quand une nouvelle catégorie est sélectionnée, le `ProductsByCategoryDataSource` ObjectDataSource détecte que ses valeurs de paramètre ont été modifiés et le `ProductsByCategory` DropDownList lui-même, relie les produits pour la catégorie sélectionnée.

Le problème qui survient dans cette situation est que le point dans le cycle de vie de page les ObjectDataSources vérifier les paramètres modifiés se produit *avant* le rétablissement de la liaison des données associées des contrôles Web. Par conséquent, lorsque vous sélectionnez une nouvelle catégorie la `ProductsByCategoryDataSource` ObjectDataSource détecte une modification de sa valeur de paramètre. ObjectDataSource utilisé par le `ProductDetails` DetailsView, toutefois, ne notez les modifications requises, car le `ProductsByCategory` DropDownList doit être reliée. Plus loin dans le cycle de vie du `ProductsByCategory` DropDownList relie à son ObjectDataSource, en saisissant les produits pour la catégorie qui vient d’être sélectionnée. Bien que le `ProductsByCategory` les valeur de DropDownList a changé, le `ProductDetails` ObjectDataSource de DetailsView a déjà effectué son contrôle de valeur de paramètre ; par conséquent, le contrôle DetailsView affiche ses résultats précédents. Cette interaction est représentée dans la Figure 20.


[![TObjectDataSource vérifie he ProductsByCategory DropDownList valeur modifications après la ProductDetails de DetailsView les modifications](master-detail-filtering-with-two-dropdownlists-cs/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image58.png)

**Figure 20**: Le `ProductsByCategory` DropDownList des modifications de valeur après la `ProductDetails` ObjectDataSource vérifie de DetailsView les modifications ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image60.png))


Pour remédier à cela nous devons relier explicitement les `ProductDetails` DetailsView après le `ProductsByCategory` DropDownList a été liée. Nous pouvons effectuer cette opération en appelant le `ProductDetails` de DetailsView `DataBind()` méthode lorsque le `ProductsByCategory` de DropDownList `DataBound` se déclenche des événements. Ajouter le code de gestionnaire d’événements suivant à la `MasterDetailsDetails.aspx` classe code-behind de la page (reportez-vous à la «[définition par programmation les valeurs des paramètres de l’ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)» pour une discussion sur l’ajout d’un gestionnaire d’événements) :


[!code-csharp[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample2.cs)]

Après cet appel explicit à la `ProductDetails` de DetailsView `DataBind()` méthode a été ajoutée, le didacticiel fonctionne comme prévu. Points importants figure 21 comment cela modifié remédié à notre problème antérieures.


[![TIl ProductDetails DetailsView est DataBound événement déclenche d’explicitement actualisée lorsque le ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image61.png)

**Figure 21**: Le `ProductDetails` DetailsView est explicitement actualisée lorsque le `ProductsByCategory` de DropDownList `DataBound` se déclenche des événements ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-cs/_static/image63.png))


## <a name="summary"></a>Récapitulatif

Le DropDownList constitue un élément d’interface utilisateur idéal pour les rapports maître/détail s’il existe une relation un-à-plusieurs entre les enregistrements maître et détail. Dans le didacticiel précédent, nous avons vu comment utiliser un DropDownList unique pour filtrer les produits affichés par la catégorie sélectionnée. Dans ce didacticiel, nous remplacé le GridView de produits avec une DropDownList et un contrôle DetailsView permet d’afficher les détails du produit sélectionné. Les concepts abordés dans ce didacticiel peuvent facilement être étendus aux modèles de données impliquant plusieurs relations un-à-plusieurs, telles que les clients, commandes et éléments de commande. En règle générale, vous pouvez toujours ajouter un contrôle DropDownList pour chacune des entités « un » dans les relations un-à-plusieurs.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Hilton Giesenow. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](master-detail-filtering-with-a-dropdownlist-cs.md)
> [Suivant](master-detail-filtering-across-two-pages-cs.md)
